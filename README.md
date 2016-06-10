#FFmpeg codecs libs compilation

#Required dependences
yum groupinstall "Development Tools" -y

#Dynamic Linked Libraries Path
grep /usr/local/lib /etc/ld.so.conf || echo "/usr/local/lib" >> /etc/ld.so.conf  

#yasm 
cd /usr/local/src/yasm-1.2.0
./configure  && make && make install

--- Yasm check ---

yasm --version
yasm 1.2.0 (this is how the result should look)


#Codecs and video conversion tools

#LIBOGG
cd /usr/local/src/libogg-1.3.0
make distclean
./configure && make clean && make && make install

--- LIBOGG Check ---

ls /usr/local/lib/libogg*
Results in
/usr/local/lib/libogg.a   /usr/local/lib/libogg.so.0
/usr/local/lib/libogg.la  /usr/local/lib/libogg.so.0.8.0


#LIBVORBIS
cd /usr/local/src/libvorbis-1.3.3
make distclean
./configure && make clean && make && make install 

--- LIBVORBIS Check ---
ls /usr/local/lib/libvorbis*
Resutls in
/usr/local/lib/libvorbis.a            /usr/local/lib/libvorbisfile.so
/usr/local/lib/libvorbisenc.a         /usr/local/lib/libvorbisfile.so.3
/usr/local/lib/libvorbisenc.la        /usr/local/lib/libvorbisfile.so.3.3.5
/usr/local/lib/libvorbisenc.so        /usr/local/lib/libvorbis.la
/usr/local/lib/libvorbisenc.so.2      /usr/local/lib/libvorbis.so
/usr/local/lib/libvorbisenc.so.2.0.9  /usr/local/lib/libvorbis.so.0
/usr/local/lib/libvorbisfile.a        /usr/local/lib/libvorbis.so.0.4.6
/usr/local/lib/libvorbisfile.la


#LibXvid

cd /usr/local/src/xvidcore/build/generic
./configure 
make
make install

--- LibXvid Check ---

ls /usr/local/lib/libxvid*

/usr/local/lib/libxvidcore.a    /usr/local/lib/libxvidcore.so.4@
/usr/local/lib/libxvidcore.so@  /usr/local/lib/libxvidcore.so.4.3


#Theora
cd /usr/local/src/libtheora-1.1.1
./configure && make clean && make && make install 

--- Theora Check ---

ls /usr/local/lib/libtheora*

/usr/local/lib/libtheora.a            /usr/local/lib/libtheoraenc.so
/usr/local/lib/libtheoradec.a         /usr/local/lib/libtheoraenc.so.1
/usr/local/lib/libtheoradec.la        /usr/local/lib/libtheoraenc.so.1.1.2
/usr/local/lib/libtheoradec.so        /usr/local/lib/libtheora.la
/usr/local/lib/libtheoradec.so.1      /usr/local/lib/libtheora.so
/usr/local/lib/libtheoradec.so.1.1.4  /usr/local/lib/libtheora.so.0
/usr/local/lib/libtheoraenc.a         /usr/local/lib/libtheora.so.0.3.10
/usr/local/lib/libtheoraenc.la

#LIBX264

cd /usr/local/src/x264-snapshot-*
make distclean
./configure --enable-shared && make clean && make && make install

--- LIBX264 Check ---

ls /usr/local/lib/libx264*

/usr/local/lib/libx264.so  /usr/local/lib/libx264.so.124 

#libfaac
cd /usr/local/src/faac-1.28
./configure && make clean && make && make install 

ls /usr/local/lib/libfaac*
 /usr/local/lib/libfaac.a   /usr/local/lib/libfaac.so    /usr/local/lib/libfaac.so.0.0.0
 /usr/local/lib/libfaac.la  /usr/local/lib/libfaac.so.0


 - COMMON PROBLEM ALERT -
 In recent Linux distrubutions (Centos 6, Debian 6) the compilation may fail with the following error: 
 				In file included from mp4common.h:29, from 3gp.cpp:28:mpeg4ip.h:126: error: new declaration ‘char* strcasestr(const char*, const char*)’
That is because the C function strcasestr declared in the libfaac sources is already declared in a system-wide library.
To solve it: Edit the file /usr/local/src/faac-1.28/common/mp4v2/mpeg4ip.h and delete the following line (around line 126)

	char *strcasestr(const char *haystack, const char *needle); 

-- Then run --

	make clean && ./configure && make && make install

#libfdk-aac
yum install unzip
cd /usr/local/src/mstorsjo-fdk-aac*
autoreconf -fiv
./configure
make
make install
make distclean

-- libfdk-aac Check --

ls /usr/local/lib/libfdk-aac*
Results in
/usr/local/lib/libfdk-aac.a  /usr/local/lib/libfdk-aac.la*  /usr/local/lib/libfdk-aac.so@  /usr/local/lib/libfdk-aac.so.1@  /usr/local/lib/libfdk-aac.so.1.0.0*

#LAME
cd /usr/local/src/lame-3.99.5
make distclean
./configure && make clean && make && make install

--- LAME Check ---

ls /usr/local/lib/libmp3lame*
Results in

/usr/local/lib/libmp3lame.a   /usr/local/lib/libmp3lame.so.0
/usr/local/lib/libmp3lame.la  /usr/local/lib/libmp3lame.so.0.0.0
/usr/local/lib/libmp3lame.so


#libvpx
cd /usr/local/src/libvpx-1.5.0
./configure --disable-examples --enable-shared && make && make install && ldconfig

ls /usr/local/lib/libvpx*
/usr/local/lib/libvpx.a /usr/local/lib/libvpx.so.1 /usr/local/lib/libvpx.so.1.1.0
/usr/local/lib/libvpx.so /usr/local/lib/libvpx.so.1.1

#libx265
cd /usr/local/src/x265/build/linux
./make-Makefiles.bash
make -j6
make install
ldconfig 


--- libx265 Check ---

ls /usr/local/lib/libx265*
Results in
/usr/local/lib/libx265.a  /usr/local/lib/libx265.so@  /usr/local/lib/libx265.so.41*

#Install Libass-devel (for the subtitles)
yum install /usr/local/src/epel-release-6-8.noarch.rpm
yum update
yum --enablerepo=epel install libass-devel


#FFMPEG
cd /usr/local/src
chmod 777 /usr/local/src/tmp
export TMPDIR=/usr/local/src/tmp
cd ffmpeg
make distclean
PKG_CONFIG_PATH="/usr/local/lib/pkgconfig" ./configure --disable-static --enable-gpl --enable-version3 --enable-nonfree --enable-shared --enable-libmp3lame --enable-libx264 --enable-libx265 --enable-libfaac --enable-libvpx --enable-libvorbis --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopencore-amrnb --enable-libtheora --enable-libxvid --enable-libfdk_aac --enable-libopus --enable-libass
make clean && make && make install
make tools/qt-faststart
cp tools/qt-faststart /usr/local/bin/
ldconfig