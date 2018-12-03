# install ffmpeg with GPU

## update nvidia driver

### uninstall old driver
```
sudo chmod +x *.run
sudo ./NVIDIA-Linux-xxxxxxx.run --uninstall
```
### add nouveau to blacklist
```
gedit /etc/modprobe.d/blacklist.conf
blacklist nouveau //add to the end

sudo update-initramfs -u
rboot
lsmod | grep nouveau //should get nothing 
```
### install new driver
```
./NVIDIA-Linux-x86_64-375.20.run –no-opengl-files –no-x-check –no-nouveau-check
```
### if you get error opening terminal
```
export TERM=vt100
export TERMINFO=/usr/share/terminfo
```
## install CUDA

### install
```
wget --no-check-certificate https://developer.nvidia.com/compute/cuda/8.0/prod/local_installers/cuda-repo-ubuntu1604-8-0-local_8.0.44-1_amd64-deb
dpkg -i cuda-repo-ubuntu1604-8-0-local_8.0.44-1_amd64-deb
deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64 /
apt-get update
apt-get install cuda
```
### set env
```
export PATH=/usr/local/cuda-8.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
source ~/.bashrc
```

## install ffmpeg

### prepare
```
apt-get update
apt-get -y install autoconf automake build-essential libass-dev libfreetype6-dev \
  libsdl2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev \
  libxcb-xfixes0-dev pkg-config texinfo zlib1g-dev
  apt-get install yasm #版本为1.3

apt-get install libx264-dev #版本为148

apt-get install libx265-dev

apt-get install libvpx-dev #版本为1.5

apt-get install libfdk-aac-dev # 无版本要求

apt-get install libmp3lame-dev

apt-get install libopus-dev # 1.1.2

apt-get -y install glew-utils libglew-dbg libglew-dev libglew1.13 \
  libglewmx-dev libglewmx-dbg freeglut3 freeglut3-dev freeglut3-dbg libghc-glut-dev \
  libghc-glut-doc libghc-glut-prof libalut-dev libxmu-dev libxmu-headers libxmu6 \
  libxmu6-dbg libxmuu-dev libxmuu1 libxmuu1-dbg
```
### get ffmpeg
```
git clone https://github.com/FFmpeg/FFmpeg ffmpeg -b master
```
### set configure
```
./configure --enable-cuda --enable-cuvid --enable-nvenc --enable-nonfree --enable-libnpp 
--extra-cflags=-I/usr/local/cuda/include --extra-ldflags=-L/usr/local/cuda/lib64
```
### make
```
make -j8
```











