This is for ubuntu 24.
```
export VM_NAME=pulsar-010
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
packages:
 - openjdk-21-jdk
 - podman
EOF

# no autostart
genisoimage -output cidata.iso -V cidata -r -J user-data meta-data
qemu-img create -b $VM_IMAGE -f qcow2 -F qcow2 root.img 10G
virt-install --name=$VM_NAME --ram=2048 --vcpus=1 --import --disk path=root.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu-lts-latest --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial
```