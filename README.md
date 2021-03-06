# crome os live usb setup

```bash
wget https://dl.bintray.com/chromebrew/chromebrew/parted-3.2-chromeos-x86_64.tar.xz
wget https://dl.bintray.com/chromebrew/chromebrew/ncurses-6.2-chromeos-x86_64.tar.xz
tar xvf ncurses-6.2-chromeos-x86_64.tar.xz
tar xvf parted-3.2-chromeos-x86_64.tar.xz
export LD_LIBRARY_PATH=/usr/local/opt/usr/local/lib64:$LD_LIBRARY_PATH
cd usr/local/sbin

sudo ./parted
print free
resizepart 16
quit

resize2fs /dev/sda16 
```
### docker setup
```bash
sudo start docker
sleep 2
sudo docker run --rm alpine echo "hello"
sudo bash -c "echo 0-3>/sys/fs/cgroup/cpuset/docker/cpuset.cpus"
sudo docker run --rm alpine echo "hello"

# docker-compose setup https://github.com/docker/compose/releases
sudo curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
# sudo TMPDIR=/usr/local/opt/tmp docker-compose up
```
### chroot setup
```bash
mkdir bin  dev  etc  home  lib  lib64  opt  proc  root  run  sbin  sys  tmp  usr  usrlocal  var

mount --rbind /dev ./dev
mount -t proc /proc ./proc -o nosuid,nodev,noexec
mount --rbind /sys ./sys -o nosuid,nodev,noexec,ro
mount --rbind /dev/pts ./dev/pts -o nosuid,noexec
mount --rbind /run ./run

mount --rbind /bin ./bin
mount --rbind /sbin ./sbin
mount --rbind /lib ./lib
mount --rbind /lib64 ./lib64
mount --rbind /usr ./usr
mount --rbind /root ./root
mount --rbind /home ./home
mount --rbind /etc ./etc

mount --rbind ./usrlocal ./usr/local/
mount --rbind /tmp ./tmp


umount ./dev
umount ./sys
umount ./dev/pts
umount ./run
umount ./proc

umount ./bin
umount ./sbin
umount ./lib
umount ./lib64
umount ./usr/local/
umount ./usr
umount ./root
umount ./home
umount ./etc
umount ./tmp
```
