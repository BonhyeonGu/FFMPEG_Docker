FROM nvidia/cuda:12.0.0-devel-ubuntu20.04

ENV TZ=Asia/Seoul
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade && apt -y dist-upgrade && apt-get install -y nvidia-cuda-toolkit
RUN apt-get install -y wget git cmake nano snapd build-essential git yasm unzip wget sysstat nasm libc6 \
libavcodec-dev libavformat-dev libavutil-dev pkgconf g++ freeglut3-dev \
libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev libgstreamer-plugins-bad1.0-dev gstreamer1.0-plugins-good

#!----------------------------------------------------------------------------------------------------------ffmpeg

#WORKDIR /root
#COPY ./Video_Codec_SDK_12.0.16.zip ./
#RUN unzip Video_Codec_SDK_12.0.16.zip
#WORKDIR /root/Video_Codec_SDK_12.0.16/Samples
#RUN mkdir build
#WORKDIR /root/Video_Codec_SDK_12.0.16/Samples/build
#RUN cmake -DCMAKE_BUILD_TYPE=Release ..
#RUN make && make install

WORKDIR /root
RUN git clone https://github.com/FFmpeg/nv-codec-headers
WORKDIR /root/nv-codec-headers
RUN make && make install

WORKDIR /root
RUN git clone https://code.videolan.org/videolan/x264.git
WORKDIR /root/x264
RUN ./configure --disable-cli --enable-static --enable-shared --enable-strip
RUN make && make install
RUN ldconfig

WORKDIR /root
RUN git clone git://source.ffmpeg.org/ffmpeg.git ffmpeg
WORKDIR /root/ffmpeg
RUN ./configure --enable-nonfree --enable-nvenc --enable-libx264 --enable-gpl --enable-cuda --enable-cuvid --enable-cuda-nvcc
RUN make

#!----------------------------------------------------------------------------------------------------------------------

ENTRYPOINT ["/bin/sh", "-c" , "tail -f /dev/null"]
