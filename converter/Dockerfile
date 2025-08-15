FROM ubuntu:22.04

# 基础依赖
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential cmake git libtbb-dev ca-certificates \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /opt

# 拉取并编译 PotreeConverter（v2，使用 TBB、C++17）
RUN git clone --recursive https://github.com/potree/PotreeConverter.git \
 && cd PotreeConverter \
 && mkdir build && cd build \
 && cmake -DCMAKE_BUILD_TYPE=Release .. \
 && make -j"$(nproc)"

# 把可执行文件加入 PATH
ENV PATH="/opt/PotreeConverter/build:${PATH}"

# 默认直接把容器当命令用：docker run ... <输入> -o <输出>
ENTRYPOINT ["PotreeConverter"]
