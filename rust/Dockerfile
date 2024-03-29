FROM messense/cargo-zigbuild:0.18.1

LABEL org.opencontainers.image.source https://github.com/gngpp/profiles

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

WORKDIR /build

RUN git config --global user.name "gngpp" && git config --global user.email "gngpp@example.com"
ENV GO111MODULE=on GOPROXY=https://goproxy.cn,direct

RUN apt-get update -y
RUN apt install -y git zsh htop wget neofetch vim net-tools autoconf mingw-w64 sudo rename
RUN rustup update

# Uses "Spaceship" theme with some customization. Uses some bundled plugins and installs some more from github
RUN sh -c "$(wget -O- https://github.com/deluan/zsh-in-docker/releases/download/v1.1.5/zsh-in-docker.sh)" -- \
    -t https://github.com/denysdovhan/spaceship-prompt \
    -a 'SPACESHIP_PROMPT_ADD_NEWLINE="false"' \
    -a 'SPACESHIP_PROMPT_SEPARATE_LINE="false"' \
    -p git \
    -p https://github.com/zsh-users/zsh-autosuggestions \
    -p https://github.com/zsh-users/zsh-completions

RUN echo "neofetch" >> ~/.zshrc
RUN cargo install cargo-deb
RUN git clone https://github.com/NixOS/patchelf
RUN cd patchelf && \
    ./bootstrap.sh && \
    ./configure && \
    make && make check && make install

# install upx
RUN wget https://github.com/upx/upx/releases/download/v4.2.1/upx-4.2.1-amd64_linux.tar.xz
RUN tar -xvf upx-4.2.1-amd64_linux.tar.xz
RUN mv upx-4.2.1-amd64_linux/upx /usr/bin/upx
RUN rm -rf upx-4.2.1-amd64_linux.tar.xz upx-4.2.1-amd64_linux

ENTRYPOINT ["/usr/bin/zsh"]