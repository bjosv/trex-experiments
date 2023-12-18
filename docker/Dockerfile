# Run:
# docker build -t trex-dev .
# docker run -it --privileged --cap-add=ALL trex-dev
# or
# docker run -it --privileged --cap-add=ALL -v /mnt/huge:/mnt/huge -v /sys/bus/pci/devices:/sys/bus/pci/devices -v /sys/devices/system/node:/sys/devices/system/node -v /dev:/dev trex-dev
#
FROM golang:1.21-bookworm

RUN apt-get update
RUN apt-get install -y git gcc g++ zlib1g-dev \
    python-is-python3 python3.11-distutils python3-yaml \
    pciutils sudo ethtool iproute2 net-tools

RUN mkdir /scratch
WORKDIR /scratch

# trex-core
RUN git clone https://github.com/cisco-system-traffic-generator/trex-core.git
RUN cd trex-core/linux && sed -i "s/'-Wno-error=uninitialized'/'-Wno-error=uninitialized','-Wno-error=maybe-uninitialized','-Wno-error=dangling-pointer'/g" ws_main.py
RUN cd trex-core/linux && ./b configure && ./b build
RUN cd trex-core/linux_dpdk && sed -i "s/'-Werror',//g" ws_main.py
RUN cd trex-core/linux_dpdk && ./b configure && ./b build

# trex-emu
RUN git clone https://github.com/cisco-system-traffic-generator/trex-emu.git
RUN cd trex-emu/scripts && ./utility_script build -v
RUN cd trex-core/scripts/trex_emu/ && ln -s ../../../trex-emu/bin/trex-emu

# Configs
COPY trex_cfg.yaml /etc/trex_cfg.yaml

ENV LD_LIBRARY_PATH=/scratch/trex-core/scripts/so/x86_64/

WORKDIR /scratch/trex-core/scripts

CMD ["/bin/bash"]

