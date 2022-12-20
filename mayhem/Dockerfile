FROM --platform=linux/amd64 ubuntu:22.04 as builder

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y build-essential g++-multilib ninja-build cmake checkinstall libc6-dev \
    libfreetype6-dev \
    zlib1g-dev clang

COPY . /repo
WORKDIR /repo
RUN ./scripts/rebuild_linux.sh

WORKDIR /repo/libmediation/build
RUN cmake ..
RUN make -j8

RUN clang++ /repo/mayhem/fuzz_timeToFloat.cpp -fsanitize=fuzzer,address -I /repo/tsMuxer/ -I /repo/libmediation/ /repo/tsMuxer/vod_common.cpp /repo/libmediation/build/libmediation.a -o /fuzz

FROM ubuntu:22.04 as package
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y libfreetype6-dev
COPY --from=builder /repo/bin/tsMuxeR /
COPY --from=builder /fuzz /fuzz_timeToFloat
