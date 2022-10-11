FROM i386/debian:bullseye-slim
LABEL com.iximiuz-project="docker-to-linux"
ENV ELIXIR_VER=1.14.1 \
OTP_MAIN_VER=25 \
DEBIAN_FRONTEND=noninteractive \
LANG=en_US.UTF-8 \
LANGUAGE=en_US.UTF-8 \
LC_ALL=en_US.UTF-8
RUN apt-get -y update
RUN apt-get -y install --no-install-recommends \
    linux-image-686 \
    systemd-sysv unzip libncurses5 libssl-dev \
    wget locales make && \
    locale-gen en_US.UTF-8 && \
    dpkg-reconfigure locales && \
    echo "export LANG=en_US.UTF-8\nexport LANGUAGE=en_US.UTF-8\nexport LC_ALL=en_US.UTF-8" >> /root/.bashrc
COPY erlang-25.1.1.tar.gz /erlang-25.1.1.tar.gz
RUN tar xf erlang-25.1.1.tar.gz && \
    cp -arf erlang-25.1.1/usr/local/* /usr/local/ && \
    rm -f erlang-25.1.1.tar.gz && \
    rm -rf erlang-25.1.1 && \
    mkdir /.elixir && cd /.elixir && \
    wget --no-check-certificate "https://github.com/elixir-lang/elixir/releases/download/v${ELIXIR_VER}/elixir-otp-${OTP_MAIN_VER}.zip" -O "elixir-${ELIXIR_VER}.zip" && \
    unzip "elixir-${ELIXIR_VER}.zip" && \
    rm -f "elixir-${ELIXIR_VER}.zip" && \
    cp -arf bin/* /usr/local/bin && \
    cp -arf lib/* /usr/local/lib
COPY evision.tar.gz /root/evision.tar.gz
RUN cd /root && \
    tar xf evision.tar.gz && \
    rm -f evision.tar.gz
COPY hex.tar.gz /root/hex.tar.gz
COPY mix.tar.gz /root/mix.tar.gz
RUN cd /root && \
    tar xf hex.tar.gz && \
    rm -f hex.tar.gz && \
    tar xf mix.tar.gz && \
    rm -f mix.tar.gz
RUN apt-get clean \
  && rm -rf /.root/cache \
  && rm -rf /var/lib/apt/lists/*
RUN echo "root:root" | chpasswd
