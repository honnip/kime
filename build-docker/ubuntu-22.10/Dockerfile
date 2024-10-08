FROM ubuntu:22.10

ENV DEBIAN_FRONTEND=noninteractive \
  PATH=/root/.cargo/bin:$PATH

WORKDIR /opt/kime

RUN sed -i -re 's/([a-z]{2}.)?archive.ubuntu.com|security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y curl apt-utils \
                       build-essential git gcc libclang-11-dev cmake extra-cmake-modules pkg-config zstd \
                       libpango1.0-dev libcairo2-dev libgtk-4-dev libgtk-3-dev libglib2.0 libxcb1 \
                       qtbase5-dev qtbase5-private-dev libqt5gui5 \
					   qt6-base-dev qt6-base-private-dev \
                       libxcb-shape0-dev libxcb-xfixes0-dev
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --default-toolchain stable --no-modify-path --profile minimal
RUN rustc --version
RUN mkdir -pv /opt/kime-out

COPY src ./src

COPY Cargo.toml .
COPY Cargo.lock .

RUN cargo fetch

COPY res ./res
COPY ci ./ci
COPY docs ./docs
COPY scripts ./scripts
COPY LICENSE .
COPY NOTICE.md .
COPY README.ko.md .
COPY README.md .
COPY VERSION .

ENTRYPOINT [ "ci/build_deb.sh" ]
