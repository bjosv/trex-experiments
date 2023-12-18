# TRex and TRex EMU

https://trex-tgn.cisco.com/trex/doc/trex_emu.html
https://trex-tgn.cisco.com/trex/doc/trex_stateless.html


## Setup
```
# Start containers (trex and test)
docker compose up

# Run tcpdump in test container
docker exec -it test tcpdump

# Start test via console
docker exec -it trex ./trex-console --emu

# Setup 6 clients
emu_load_profile -f emu/simple_emu.py --max 100 -t --ns 1 --clients 5 --4 172.100.0.10 --dg 172.100.0.3

emu_show_all
emu_load_profile -f emu/simple_ipv4.py -t --ns 1 --clients 1
...
```

cd docker
docker build -t trex-dev .

## Run
docker run -it --rm --privileged --cap-add=ALL trex-dev

