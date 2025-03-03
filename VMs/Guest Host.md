Hardware: https://aoostar.com/products/aoostar-r7-2-bay-40t-nas-storage-amd-ryzen-7-5825u-mini-pc8c-16t-up-to-4-5ghz-with-w11-pro-ddr4-ram-2-m-2-nvme-%E5%A4%8D%E5%88%B6?variant=49012947910954

Software
- Download debian netinst media (https://www.debian.org/CD/netinst/)
- Install with expert graphical.  Most of the defaults are fine, except partitioning.  I used the partial disk LVM layout to partition 200 GB and separate /home, /var, /tmp, and /

 - Saved the preseed file https://wiki.debian.org/DebianInstaller/Preseed https://d-i.debian.org/doc/installation-guide/en.amd64/apbs03.html
 - Install KVM https://wiki.debian.org/KVM `$ sudo apt install --no-install-recommends qemu-system libvirt-clients libvirt-daemon-system virtinst libguestfs-tools` 
 - `adduser <youruser> libvirt`
 - `adduser sweeneyb kvm`
 - Added my ssh key


- copy the install media over to ~
- `virt-install --virt-type kvm --name bookworm-amd64 \
--location /home/sweeneyb/debian-12.5.0-amd64-netinst.iso \
--os-variant debian11 \
--memory 1024 \
--disk path=/home/sweeneyb/images/bookworm-amd64-001,size=10,bus=virtio,format=qcow2 \
--graphics none \
--console pty,target_type=serial \
--extra-args "console=ttyS0"`

- grab latest generic from https://cloud.debian.org/images/cloud/bookworm/latest/

- install `apt install genisoimage cloud-init`

user-data:
```
#cloud-config

users:
  - name: sweeneyb
    groups: sudo
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIArG/xbYCIDNJJH/RU6krtD7YijTu+ZnBezHUwu7np5/
    lock_passwd: false
    passwd: "<output of mkpasswd -m sha512"
    sudo: ALL=(ALL) NOPASSWD:ALL
```



cloud-init wasn't setting the password.  Used guestfish to set the password:
https://www.cyberciti.biz/faq/how-to-reset-forgotten-root-password-for-linux-kvm-qcow2-image-vm/

echo "allow br0" > /etc/qemu/bridge.conf


chmod u+s /usr/lib/qemu/qemu-bridge-helper

Here is my /etc/network/interfaces
```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
allow-hotplug eno1
iface eno1 inet dhcp

auto br0
iface br0 inet dhcp
  bridge_ports enp3s0
  bridge_stp off
  bridge_waitport 0
  bridge_fd 0
```



virt-install --name=test002 --ram=2048 --vcpus=1 --import --disk path=ubuntu-test.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu20.04 --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial

I've added a usb_device.xml as per 
https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/6/html/virtualization_administration_guide/sect-managing_guest_virtual_machines_with_virsh-attaching_and_updating_a_device_with_virsh#sect-Managing_guest_virtual_machines_with_virsh-Attaching_and_updating_a_device_with_virsh
https://askubuntu.com/questions/595896/how-to-allow-software-access-to-any-usb-devices
says to:
```
```
echo 'SUBSYSTEM\=\="usb", MODE="0660", GROUP="plugdev"' > /etc/udev/rules.d/00-usb-permissions.rules
udevadm control --reload-rules


zfs create -p -o refquota=10GB  -o mountpoint=/data/vms/docker tank/data/vms/docker
zfs create -p -o refquota=10GB  -o mountpoint=/data/vms/homeassistant tank/data/vms/homeassistant


-o refquota=10GB
zfs create -o mountpoint=/data tank/data


zfs set mountpoint=/data/vms/docker tank/data/vms/docker
zfs set mountpoint=/data/vms/homeassistant tank/data/vms/homeassistant
zfs set mountpoint=/data/vms tank/data/vms


zfs destroy tank/data/vms/docker
zfs destroy tank/data/vms/homeassistant
zfs destroy tank/data/vms

tank/data/vms/homeassistant

### expand zfs refquota
sudo zfs set refquota=30G tank/data/vms/pulsar-010
sudo zfs set refquota=30G tank/data/vms/pulsar-020
sudo zfs set refquota=30G tank/data/vms/pulsar-030


```
export VM_NAME=homedir
sudo zfs create -p -o refquota=10GB  -o mountpoint=/data/vms/$VM_NAME tank/data/vms/$VM_NAME
sudo chown sweeneyb:sweeneyb /data/vms/$VM_NAME
cd /data/vms/$VM_NAME
cp ~/focal-server-cloudimg-amd64.img /data/vms/$VM_NAME
cat << EOF > /data/vms/$VM_NAME/meta-data
instance-id: $VM_NAME
local-hostname: $VM_NAME
EOF 

cat << EOF > /data/vms/$VM_NAME/user-data
#cloud-config

hostname: $VM_NAME
users:
  - name: sweeneyb
    groups: sudo
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIArG/xbYCIDNJJH/RU6krtD7YijTu+ZnBezHUwu7np5/
    lock_passwd: false
    passwd: "$1$sC8RMknt$ryy.SalurqDuCLPu1H1Gx."
    sudo: ALL=(ALL) NOPASSWD:ALL
EOF
genisoimage -output cidata.iso -V cidata -r -J user-data meta-data
qemu-img create -b focal-server-cloudimg-amd64.img -f qcow2 -F qcow2 ubuntu-test.img 10G
virt-install --name=$VM_NAME --ram=2048 --vcpus=1 --autostart --import --disk path=ubuntu-test.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu20.04 --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial
```

for ubuntu 22:
```

qemu-img create -b jammy-server-cloudimg-amd64.img -f qcow2 -F qcow2 ubuntu-test.img 10G
virt-install --name=$VM_NAME --ram=2048 --vcpus=1 --autostart --import --disk path=ubuntu-test.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu22.04 --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial
```

for ubuntu 24:
```

qemu-img create -b noble-server-cloudimg-amd64.img -f qcow2 -F qcow2 ubuntu-test.img 10G
virt-install --name=$VM_NAME --ram=2048 --vcpus=1 --autostart --import --disk path=ubuntu-test.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu-lts-latest --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial
```


```
export VM_NAME=sftp-go-01
export VM_IMAGE=noble-server-cloudimg-amd64.img

sudo zfs create -p -o refquota=10GB  -o mountpoint=/data/vms/$VM_NAME tank/data/vms/$VM_NAME
sudo chown sweeneyb:sweeneyb /data/vms/$VM_NAME
cd /data/vms/$VM_NAME
cp ~/$VM_IMAGE /data/vms/$VM_NAME
cat << EOF > /data/vms/$VM_NAME/meta-data
instance-id: $VM_NAME
local-hostname: $VM_NAME
EOF 

cat << EOF > /data/vms/$VM_NAME/user-data
#cloud-config

hostname: $VM_NAME
users:
  - name: sweeneyb
    groups: sudo
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIArG/xbYCIDNJJH/RU6krtD7YijTu+ZnBezHUwu7np5/
    lock_passwd: false
    passwd: "$1$sC8RMknt$ryy.SalurqDuCLPu1H1Gx."
    sudo: ALL=(ALL) NOPASSWD:ALL
EOF

genisoimage -output cidata.iso -V cidata -r -J user-data meta-data
qemu-img create -b $VM_IMAGE -f qcow2 -F qcow2 root.img 10G
virt-install --name=$VM_NAME --ram=2048 --vcpus=1 --autostart --import --disk path=root.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu-lts-latest --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial
```

virsh undefine
virsh destroy

to see what's attached
```
virsh domblklist docker001
```

To create an additional disk:
qemu-img create -f qcow2 data-disk 5G
attach it:
virsh attach-disk docker001 `pwd`/data-disk vdb --persistent --live
virsh attach-disk docker001 `pwd`/data-disk vdb --driver qemu --subdriver qcow2 --targetbus virtio --persistent --live
detach:
virsh detach-disk docker001 `pwd`/data-disk --persistent --live


18d1:9302

1a6e:089a