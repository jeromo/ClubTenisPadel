FROM  ubuntu:latest

ENV DEBIAN_FRONTEND noninteractive
ENV QT_PATH /opt/Qt-5.7.1/
ENV QT_ANDROID ${QT_PATH}/android_armv7
ENV ANDROID_HOME /opt/android-sdk-linux
ENV ANDROID_SDK_ROOT ${ANDROID_HOME}
ENV ANDROID_NDK_ROOT /opt/android-ndk
ENV ANDROID_NDK_TOOLCHAIN_PREFIX arm-linux-androideabi
ENV ANDROID_NDK_TOOLCHAIN_VERSION 4.9
ENV ANDROID_NDK_HOST linux-x86_64
ENV ANDROID_NDK_PLATFORM android-21
ENV ANDROID_NDK_TOOLS_PREFIX ${ANDROID_NDK_TOOLCHAIN_PREFIX}
ENV QMAKESPEC android-g++
ENV PATH ${PATH}:${QT_ANDROID}/bin:${PATH}:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

RUN   apt update  -yqq                &&  \
      apt install -y build-essential      \
            libfontconfig1                \
            libsdl2-dev                   \
            wget                          \
            curl                          \
            openssh-server                \
            git                           \
            ccache                        \
            perl                          \
            python                    &&  \
     apt -y build-dep qtbase5-dev     &&  \
     apt -y install --no-install-recommends \
         "^libxcb.*"                      \
         libx11-xcb-dev                   \
         libglu1-mesa-dev                 \
         libxrender-dev                   \
         libxi-dev                        \
         "libatspi2*"                     \
         libdbus-1-dev                    \
         flex                                       \
         bison                                      \
         gperf                                      \
         libicu-dev                                 \
         libxslt-dev                                \
         ruby                                       \
         libssl-dev                                 \
         libxcursor-dev                             \
         libxcomposite-dev                          \
         libxdamage-dev                             \
         libxrandr-dev                              \
         libfontconfig1-dev                         \
         libcap-dev                                 \
         libbz2-dev                                 \
         libgcrypt11-dev                            \
         libpci-dev                                 \
         libnss3-dev                                \
         libxcursor-dev                             \
         libxcomposite-dev                          \
         libxdamage-dev                             \
         libxrandr-dev                              \
         libdrm-dev                                 \
         libfontconfig1-dev                         \
         libxtst-dev                                \
         libasound2-dev                             \
         libcups2-dev                               \
         libpulse-dev                               \
         libudev-dev                                \
         libssl-dev                                 \
         libasound2-dev                             \
         libgstreamer0.10-dev                       \
         cmake                                      \
         libboost-all-dev                           \
         libfreetype6-dev                           \
         libx11-dev                                 \
         libsm-dev                                  \
         libice-dev                                 \
         libxfixes-dev                              \
         libjpeg-dev                                \
         libgstreamer-plugins-base0.10-dev       && \
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd                       && \
    useradd -m jenkins                           && \
    echo "jenkins:jenkins" | chpasswd            && \
    dpkg --add-architecture i386 && apt install -qq -y --no-install-recommends \
         libc6:i386 \
         libncurses5:i386 \
         libstdc++6:i386 \
         libz1:i386 \
        && apt-get -qq clean

# Download & unpack android SDK
RUN mkdir /tmp/android && curl -Lo /tmp/android/sdk.tgz 'http://dl.google.com/android/android-sdk_r24.4.1-linux.tgz' \
    && tar --no-same-owner -xf /tmp/android/sdk.tgz -C /opt \
    && rm -rf /tmp/android && echo "y" | android update sdk -u -a -t tools,platform-tools,build-tools-21.1.2,$ANDROID_NDK_PLATFORM

# Download & unpack android NDK
RUN mkdir /tmp/android && cd /tmp/android && curl -Lo ndk.bin 'http://dl.google.com/android/ndk/android-ndk-r10e-linux-x86_64.bin' \
    && chmod +x ndk.bin && ./ndk.bin > /dev/null && mv android-ndk-r10e $ANDROID_NDK_ROOT && chmod -R +rX $ANDROID_NDK_ROOT \
    && rm -rf /tmp/android

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
