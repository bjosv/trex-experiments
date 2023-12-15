# TRex and TRex EMU

https://trex-tgn.cisco.com/trex/doc/trex_emu.html
https://trex-tgn.cisco.com/trex/doc/trex_stateless.html


## Build container
cd docker
docker build -t trex-dev .

## Run
docker run -it --rm --privileged --cap-add=ALL trex-dev

