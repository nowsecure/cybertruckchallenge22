FROM ubuntu:22.04

LABEL org.opencontainers.image.authors="enovella@nowsecure.com"
ENV REFRESHED_AT 2022-06-13

ENV VER_APKTOOL  "2.6.1"
ENV VER_JADX     "1.4.1"
ENV VER_FRIDA    "15.1.24"
ENV COMMIT_R2    "e45c08acbff838eef559e2177a37266ad79e5c46"
ENV COMMIT_APKID "d979ffa26dea8d39814a817ae34b608532cb4443"

# Package requirements
RUN mkdir -p /usr/share/man/man1 /usr/share/man/man1

# Avoid prompts
ARG DEBIAN_FRONTEND=noninteractive

RUN dpkg --add-architecture amd64 && \
  apt update && apt upgrade -y && \
  apt install -qq -y --no-install-recommends \
  jq \
  curl \
  wget \
  libffi-dev \
  libarchive13 \
  openssl \
  openjdk-11-jre-headless \
  unzip zip lib32z1 \
  python3-pip \
  python3-dev \
  libssl-dev \
  libuv1 \
  pkg-config \
  libbsd-dev zlib1g-dev \
  python3-setuptools \
  aapt \
  git \
  build-essential \
  gcc \
  file \
  pandoc \
  mitmproxy \
  android-tools-adb \
  android-tools-fastboot && \
  apt remove --purge -y pandoc && \
  apt clean && apt autoclean && apt autoremove -y && \
  rm -rf /var/lib/apt/lists/* /tmp/* /usr/share/doc/* /var/tmp/* > /dev/null 2>&1

ADD . /opt/cbtruck/
WORKDIR /opt/cbtruck

RUN pip3 install wheel && \
    pip3 install -r requirements.txt && \
    rm requirements.txt

# Radare2
RUN git clone https://github.com/radare/radare2 && \
    cd radare2 && \
    git checkout ${COMMIT_R2} && \
    /bin/sh sys/user.sh
ENV PATH $PATH:/root/bin
RUN r2pm init && r2pm update
RUN r2pm -ci r2frida
RUN r2pm -ci r2ghidra r2ghidra-sleigh
RUN cd .. && rm -rf radare2

# Apktool
RUN wget -O /opt/cbtruck/apktool \
    https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool
RUN wget -O /opt/cbtruck/apktool.jar \
    https://github.com/iBotPeaches/Apktool/releases/download/v${VER_APKTOOL}/apktool_${VER_APKTOOL}.jar

# Jadx
ADD https://github.com/skylot/jadx/releases/download/v${VER_JADX}/jadx-${VER_JADX}.zip /tmp/jadx.zip
RUN unzip /tmp/jadx.zip -d /opt/cbtruck/jadx && \
    rm -f /tmp/jadx.zip && \
    rm -f /opt/cbtruck/jadx/bin/*.bat
ENV PATH /opt/cbtruck/jadx/bin:${PATH}

# APKiD https://github.com/rednaga/APKiD/blob/master/Dockerfile
RUN git clone https://github.com/rednaga/APKiD.git && \
    cd APKiD && \
    git checkout ${COMMIT_APKID} && \
    python3 prep-release.py && \
    python3 setup.py install && \
    cd .. && rm -rf APKiD

# Frida binaries
RUN mkdir -p /opt/cbtruck/frida
# ADD https://github.com/frida/frida/releases/download/${VER_FRIDA}/frida-server-${VER_FRIDA}-android-x86.xz \
#     /opt/cbtruck/frida/frida-server-${VER_FRIDA}-android-x86.xz
ADD https://github.com/frida/frida/releases/download/${VER_FRIDA}/frida-server-${VER_FRIDA}-android-x86_64.xz \
    /opt/cbtruck/frida/frida-server-${VER_FRIDA}-android-x86_64.xz
# ADD https://github.com/frida/frida/releases/download/${VER_FRIDA}/frida-server-${VER_FRIDA}-android-arm64.xz \
#     /opt/cbtruck/frida/frida-server-${VER_FRIDA}-android-arm64.xz

# Only unpack the one used in our emulator
RUN unxz /opt/cbtruck/frida/frida-server-${VER_FRIDA}-android-x86_64.xz

# Add our specific binaries to path
RUN chmod 700 /opt/cbtruck/*
ENV PATH /opt/cbtruck:${PATH}

