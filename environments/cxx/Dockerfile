FROM  ubuntu:latest

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
    cd /tmp                                      && \
    curl -o install.sh https://raw.githubusercontent.com/SpectoLabs/hoverfly/master/install.sh && \
    bash install.sh                             && \
    git clone git://code.qt.io/qt/qt5.git          \
            --branch 5.7.1 &&                      \
    cd qt5 &&                                      \
    git checkout 5.7.1                          && \
    ./init-repository                           && \
    pwd && cd /tmp/qt5 &&                          \
    ./configure                                    \
       -prefix /opt/Qt-5.7.1                       \
       -confirm-license                            \
       -opensource                                 \
       -nomake tests                               \
       -nomake examples                            \
       -developer-build                         && \
     cd /tmp/qt5 && make -j4                    && \
     cd /tmp/qt5 && make install                && \
     gem install cucumber                       && \
     cd /opt                                    && \
     git clone https://github.com/cucumber/cucumber-cpp.git && \
     cd cucumber-cpp                              && \
     git submodule init                          && \
     git submodule update                        && \
     cmake -E make_directory build               && \
     cmake -E chdir build cmake -DCUKE_ENABLE_EXAMPLES=on .. && \
     cmake --build build                         && \
     rm -rf /tmp/qt5                             && \
     apt-get purge -y                            && \
     rm -rf /var/lib/apt/lists/*

ENV PATH /opt/Qt-5.7.1/bin:$PATH

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
