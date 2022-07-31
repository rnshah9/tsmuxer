FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential g++-multilib ninja-build cmake checkinstall libc6-dev \
    libfreetype6-dev \
    zlib1g-dev

COPY . /repo
WORKDIR /repo
RUN ./scripts/rebuild_linux.sh

FROM ubuntu:22.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libfreetype6-dev
COPY --from=builder /repo/bin/tsMuxeR /
