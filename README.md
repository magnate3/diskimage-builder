# diskimage-builder
1、the centos8_nlvm.qcow2 should be without llvm patition

image-builder supports centos8 by using aliyun DIB_DISTRIBUTION_MIRROR

env DIB_DEBUG_TRACE=3 DIB_LOCAL_IMAGE=/data1/isoImages/centos8_nlvm.qcow2   DIB_DISTRIBUTION_MIRROR=https://mirrors.aliyun.com/centos/    disk-image-create vm block-device-efi centos8   dhcp-all-interfaces  -o centos8-baremetal.qcow2 -a arm64 --logfile ./centos8-os_v6.log

2、update grub.cfg 

qemu-img convert  -f   qcow2 -O raw  centos8-baremetal.qcow2   centos8-baremetal.qcow2

losetup -f # result: /dev/loop0

losetup /dev/loop0 centos8-baremetal.raw

kpartx -av /dev/loop0 

ls -l /dev/mapper/loop0p*

mount /dev/mapper/loop0p1 /mnt

edit grub.cfg:

 root=LABEL=cloudimg-rootfs 
 not 
 root=uuid=fbfd1e91-049d-449f-9a56-c351d5823103
