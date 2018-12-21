## Documentation on all installations
#### Date: 16112018
#### Author: Jenny Lee

**ESSENTIALS**
```shell
##first get g++ and gcc (only if you are using an empty cloud instance)
sudo apt-get install build-essential ##for root
sudo apt install liblapack-dev libopenblas-dev
sudo apt install gfortran

##then get conda
wget https://repo.anaconda.com/miniconda/Miniconda2-latest-Linux-x86_64.sh
bash Miniconda2-latest-Linux-x86_64.sh
##follow the prompts
##logout and login again after installation is done

##get cpanminus
curl -L https://cpanmin.us | perl - --sudo App::cpanminus

##R
##for newest R version 3.5, add this line to /etc/apt/sources.list
##deb https://cloud.r-project.org/bin/linux/ubuntu cosmic-cran35/
##BUT WITHOUT DOING THIS, APT GET AUTOMATICALLY DOWNLOAD THE MOST STABLE CRAN VERSION FOR YOUR UBUNTU VERSION. FOR EXAMPLE I GET UBUNTU 16 AND THE R I GET IS 3.2. 3.5 can't work.
##then run these
apt install r-base-core ##for root
sudo apt-get install r-base
sudo apt-get install r-base-devel

##to completely remove R
#You can list all packages whose name starts with "r-" like this:
dpkg -l | grep ^ii | awk '$2 ~ /^r-/ { print $2 }'
#To uninstall all of them, pipe the output to xargs apt-get remove:
dpkg -l | grep ^ii | awk '$2 ~ /^r-/ { print $2 }' | xargs apt-get remove --purge -y
```

**GREAT R PACKAGES TO HAVE**
```shell
##for fast read.table using fread
install.packages("data.table")
##ggplot2
install.packages("ggplot2")
```

**INSTALLATION FOR BISULPHITE SEQUENCES ANALYSIS**
```shell
## get samtools and bowtie2
conda install -c bioconda samtools
conda install -c bioconda bowtie2

##then get bismark
wget https://github.com/FelixKrueger/Bismark/archive/0.20.0.tar.gz
tar -xzf 0.20.0.tar.gz

##then get R

##R packages
```

**INSTALLATION FOR REPEATMODELER**

This is a toughie, the conda package is incomplete. We have to install each dependency by source. Here we go:

```shell
##Repbase library [registration is needed]
##I have them in lummerland
/homes/jlee/installer/RepBaseRepeatMaskerEdition-20170127.tar.gz

##RMBlast, comes with a patch
mkdir rmblast
wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.6.0/ncbi-blast-2.6.0+-src.tar.gz
wget http://www.repeatmasker.org/isb-2.6.0+-changes-vers2.patch.gz
tar -xzf ncbi-blast-2.6.0+-src.tar.gz
gunzip isb-2.6.0+-changes-vers2.patch.gz
cd ncbi-blast-2.6.0+-src/
patch -p1 < ../isb-2.6.0+-changes-vers2.patch
cd c++
./configure --with-mt --prefix=/root/jlee/programs/rmblast --without-debug
make
make install
##[somehow get error "recipe for target install-toolkit failed" but executables are working, other people also get the same error]

##TRF
##I have the binary in lummerland
/homes/jlee/programs/bin/trf

##RepeatMasker
wget http://www.repeatmasker.org/RepeatMasker-open-4-0-7.tar.gz
tar -xzf RepeatMasker-open-4-0-7.tar.gz
##unzip repbase library into repeatmasker folder
cp RepBaseRepeatMaskerEdition-20170127.tar.gz RepeatMasker
cd RepeatMasker
tar -xzf RepBaseRepeatMaskerEdition-20170127.tar.gz
rm RepBaseRepeatMaskerEdition-20170127.tar.gz
perl ./configure
##when prompted for perl path, do "env"
cpan Text::Soundex

##RECON
wget http://www.repeatmasker.org/RepeatModeler/RECON-1.08.tar.gz
tar -xzf RECON-1.08.tar.gz
cd RECON-1.08/src
make
make install
sed -i 's|$path = "";|$path = "/root/jlee/programs/RECON-1.08/bin";|' scripts/recon.pl

##RepeatScout
wget http://www.repeatmasker.org/RepeatScout-1.0.5.tar.gz
tar -xzf RepeatScout-1.0.5.tar.gz
cd RepeatScout-1.0.5
make

##NSEG
mkdir nseg
cd nseg
wget ftp://ftp.ncbi.nih.gov/pub/seg/nseg/*
make

##Repeatmodeler
wget http://www.repeatmasker.org/RepeatModeler/RepeatModeler-open-1.0.11.tar.gz
cpan JSON
cpan URI
cpan LWP::UserAgent
perl ./configure
```
**OTHER PROGRAMS**
```shell
##EVM
wget https://github.com/EVidenceModeler/EVidenceModeler/archive/v1.1.1.tar.gz
tar -xzf v1.1.1.tar.gz

##minimap2 (precompiled)
wget  https://github.com/lh3/minimap2/releases/download/v2.14/minimap2-2.14_x64-linux.tar.bz2
tar -jxvf minimap2-2.14_x64-linux.tar.bz2

##gsrc
##using R 3.1.2
##dependencies:
##ERROR: dependencies illuminaio, dbscan, Ckmeans.1d.dp, DNAcopy, openxlsx are not available for package gsrc
source("http://bioconductor.org/biocLite.R")
biocLite("illuminaio")
biocLite("DNAcopy")
wget https://cran.r-project.org/src/contrib/Archive/Ckmeans.1d.dp/Ckmeans.1d.dp_3.02.tar.gz
install.packages("/home/s400/home00/pz1/lee-j/programs/Ckmeans.1d.dp_3.02.tar.gz", repos= NULL, type="source")
install.packages("dbscan")
wget https://cran.r-project.org/src/contrib/Archive/openxlsx/openxlsx_3.0.0.tar.gz
install.packages("/home/s400/home00/pz1/lee-j/programs/openxlsx_3.0.0.tar.gz", repos= NULL, type="source")
wget https://cran.r-project.org/src/contrib/Archive/gsrc/gsrc_1.1.tar.gz
install.packages("/home/s400/home00/pz1/lee-j/programs/gsrc_1.1.tar.gz", repos= NULL, type="source")

##check linux distribution
[14:28:46] lee-j@fb09-pg-s400: ~/programs $   cat /proc/version
Linux version 2.6.32-696.10.3.el6.x86_64 (mockbuild@sl6.fnal.gov) (gcc version 4.4.7 20120313 (Red Hat 4.4.7-18) (GCC) ) #1 SMP Tue Sep 26 15:28:41 CDT 2017

#Install DMRCaller R version 3.2.3
source("https://bioconductor.org/biocLite.R")
biocLite("DMRcaller")

```

** partitioning drives **
```shell
##install
wget ttps://github.com/trapexit/mergerfs/releases/download/2.24.0/mergerfs-2.24.0-1.el6.x86_64.rpm
rpm -i mergerfs-2.24.0-1.el6.x86_64.rpm

##check original mounts
[root@fb09-pg-s400 s400]# mount
/dev/mapper/vg_s400-lv00_root on / type ext4 (rw)
proc on /proc type proc (rw)
sysfs on /sys type sysfs (rw)
devpts on /dev/pts type devpts (rw,gid=5,mode=620)
tmpfs on /dev/shm type tmpfs (rw,rootcontext="system_u:object_r:tmpfs_t:s0")
/dev/sda1 on /boot type ext4 (rw)
/dev/mapper/vg_s400-lv01_tmp on /tmp type ext4 (rw)
/dev/mapper/vg_s400-lv_vbox on /home/vbox type ext4 (rw)
/dev/mapper/vg_s400-lv_home00 on /home/s400/home00 type ext4 (rw)
/dev/mapper/vg_md1-lv_transfer on /home/s400/transfer type xfs (rw)
/dev/mapper/vg_md1-lv_clc01 on /home/s400/clc01 type xfs (rw)
/dev/mapper/vg_md1-lv_clc02 on /home/s400/clc02 type xfs (rw)
/dev/mapper/vg_md1-lv_sw400 on /home/s400/sw400 type xfs (rw)
/dev/mapper/vg_md1-lv_data01 on /home/s400/data01 type xfs (rw)
/dev/mapper/vg_md1-lv_clc03 on /home/s400/clc03 type xfs (rw)
/dev/mapper/vg_md1-lv_data02 on /home/s400/data02 type xfs (rw)
/dev/mapper/vg_md1-lv_blast on /opt/blast type xfs (rw)
/dev/mapper/vg_md1-lv_b on /home/s400/samans_home type xfs (rw)
none on /proc/sys/fs/binfmt_misc type binfmt_misc (rw)
sunrpc on /var/lib/nfs/rpc_pipefs type rpc_pipefs (rw)
nfsd on /proc/fs/nfsd type nfsd (rw)

##mount
mount -t xfs /dev/mapper/vg_md1-lv_b /home/s400/mnt1
mount -t xfs /dev/mapper/vg_md1-lv_b /home/s400/mnt2

##merge points
mergerfs -o defaults,allow_other,use_ino  /home/s400/mnt1:/home/s400/mnt2 /home/s400/data03

##to check
[root@fb09-pg-s400 s400]# mergerfs.ctl -m  /home/s400/data03 info
- mount: /home/s400/data03
  version: 2.24.0
  pid: 13143
  srcmounts:
    - /home/s400/mnt1
    - /home/s400/mnt2
```

**ERRORS and SOLUTION (if possible)**
```shell
(base) root@bp-small:~/jlee/programs/ncbi-blast-2.6.0+-src/c++# ./configure --with-mt --prefix=/root/jlee/programs/rmblast
configure: loading site script ./src/build-system/config.site

configure: loading cache config.cache
checking TeamCity build number... none
checking Subversion revision... unknown
checking NCBI stable components' version... unknown
checking build system type... x86_64-unknown-linux-gnu
checking host system type... x86_64-unknown-linux-gnu
checking for a BSD-compatible install... /usr/bin/install -c
checking for gcc... gcc
checking for C compiler default output file name... configure: error: C compiler cannot create executables
See `config.log' for more details.

##in config.log
gcc version 5.4.0 20160609 (Ubuntu 5.4.0-6ubuntu1~16.04.10)
configure:5104: $? = 0
configure:5111: gcc -V >&5
gcc: error: unrecognized command line option '-V'
gcc: fatal error: no input files
compilation terminated.
configure:5114: $? = 1
configure:5137: checking for C compiler default output file name

##Reason is that basic C compilier is installed but not the headers for standard library, so let's install libc6-dev
sudo apt-get update
sudo apt install libc6-dev
sudo apt-get install build-essential
```

To get rid of perl LANG warnings like this
```shell
perl: warning: Setting locale failed.
perl: warning: Please check that your locale settings:
	LANGUAGE = (unset),
	LC_ALL = (unset),
	LANG = "en_AU.UTF-8"
    are supported and installed on your system.

##it is complaining that "en_AU.UTF-8" cannot be recognised, so we uncomment this line in this file
sudo vi /etc/locale.gen

##then rerun
sudo locale-gen

##if get errors like these when installing R pacakges or others, it just means that the PKG_CONFIG_PATH is not defined or the openssl is not installed or the headers are not installed
n failed because openssl was not found. Try installing:
 * deb: libssl-dev (Debian, Ubuntu, etc)
 * rpm: openssl-devel (Fedora, CentOS, RHEL)
 * csw: libssl_dev (Solaris)
 * brew: openssl@1.1 (Mac OSX)
If openssl is already installed, check that 'pkg-config' is in your
PATH and PKG_CONFIG_PATH contains a openssl.pc file. If pkg-config
is unavailable you can set INCLUDE_DIR and LIB_DIR manually via:
R CMD INSTALL --configure-vars='INCLUDE_DIR=... LIB_DIR=...'

If openssl is already installed, check that 'pkg-config' is in your
PATH and PKG_CONFIG_PATH contains a openssl.pc file. If pkg-config
is unavailable you can set INCLUDE_DIR and LIB_DIR manually via:
R CMD INSTALL --configure-vars='INCLUDE_DIR=... LIB_DIR=...

##to check if it is installed
[13:25:05] lee-j@fb09-pg-s400: ~/programs $ rpm -qa | grep -i openssl
openssl-1.0.1e-57.el6.x86_64
pyOpenSSL-0.13.1-2.el6.x86_64
openssl098e-0.9.8e-20.el6_7.1.x86_64
##to check if headers are installed
cd /usr
find -name "*openssl.pc*"
##if no pc files found then header is not installed. so to install for redhat distribution
yum install openssl-devel  ##root needed
##to define the path, looks like i have two pkgconfig folders, so two paths
export PKG_CONFIG_PATH=/usr/share/pkgconfig/
export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/lib64/pkgconfig/
```
