# Guest config
This is for ubuntu 24.
```
export VM_NAME=docker-020
export VM_IMAGE=noble-server-cloudimg-amd64.img

sudo zfs create -p -o refquota=30GB  -o mountpoint=/data/vms/$VM_NAME tank/data/vms/$VM_NAME
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
    groups: sudo, docker
    shell: /bin/bash
    ssh_authorized_keys:
      - ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIArG/xbYCIDNJJH/RU6krtD7YijTu+ZnBezHUwu7np5/
    lock_passwd: false
    passwd: "$1$sC8RMknt$ryy.SalurqDuCLPu1H1Gx."
    sudo: ALL=(ALL) NOPASSWD:ALL
packages:
 - openjdk-21-jdk
 - podman
 - docker.io
 - docker-compose
EOF

# no autostart
genisoimage -output cidata.iso -V cidata -r -J user-data meta-data
qemu-img create -b $VM_IMAGE -f qcow2 -F qcow2 root.img 30G
virt-install --name=$VM_NAME --ram=16384 --autostart --vcpus=1 --import --disk path=root.img,format=qcow2 --disk path=cidata.iso,device=cdrom --os-variant=ubuntu-lts-latest --network bridge=br0,model=virtio --network bridge=br0,model=virtio --graphics none --console pty,target_type=serial
```


After boot, ip addr showed enp2s0 w/o an ip, so I ran `sudo dhcpcd enp2s0` and it got an IP.
put that IP in cloudflare


 sudo docker run --detach \
   --hostname gitlab.home.briansweeney.dev \
   --env GITLAB_OMNIBUS_CONFIG="external_url 'http://gitlab.home.briansweeney.dev'" \
   --publish 443:443 --publish 80:80 --publish 192.168.3.122:22:22 \
   --name gitlab \
   --restart always \
   --volume $GITLAB_HOME/config:/etc/gitlab \
   --volume $GITLAB_HOME/logs:/var/log/gitlab \
   --volume $GITLAB_HOME/data:/var/opt/gitlab \
   --shm-size 256m \
   gitlab/gitlab-ce:17.6.4-ce.0


