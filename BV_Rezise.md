# Boot Volume Resize 하는 방법


OCI에서 Compute Instance를 생성하는 경우, Boot Volume을 기본 사이즈 이상으로 설정하는 경우가 있습니다.
하지만 리눅스의 경우 Provisioning 이후 접속을 해서 보면, Boot 영역의 사이즈가 기본 사이즈로 설정되어 있습니다.

Oracle Linux 7.5의 경우, Boot Volume size를 100GB로 설정한 뒤 Provisioning 했는데 접속해서 보면 root영역 ( / )이 38.4GB 로 설정되어 있습니다.

Boot의 File System 이 xfs 인지 ext4 인지 다음과 같이 확인합니다.

	$ lsblk		                ==> root (/) 파티션 확인 (sda3)
	$ mount | grep sda3		==> sda3 파일시스템 확인


## Boot의 Filesystem이 xfs 인 경우
  
그리고 다음 명령어를 통해 root (/)인 /dev/sda3를 resize 할 수 있습니다.

	$ sudo growpart /dev/sda 3
	$ sudo xfs_growfs -d /dev/sda3
	$ df -h
	
아래의 예제를 통해서도 사이즈가 커짐을 확인하실 수 있습니다.

![](https://github.com/jesamkim/oci-tech/blob/master/img/bv_resize01.png)
![](https://github.com/jesamkim/oci-tech/blob/master/img/bv_resize02.png)


## Boot의 Filesystem이 ext4 인 경우

CentOS 6 등 boot 파티션의 파일시스템이 ext4 인 경우, 다음과 같이 rezise를 할 수 있습니다.

    $ sudo yum -y install cloud-utils-growpart
    $ sudo yum -y install gdisk
    $ sudo yum -y install libicu
    $ sudo reboot


**[Go back to HOME](https://jesamkim.github.io/oci-tech/)**
