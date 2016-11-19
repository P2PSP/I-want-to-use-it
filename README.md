# I-want-to-use-it
==================

Minimal, basic and always updated information about how to use P2PSP.

# Install [Boost C++ Libraries](http://www.boost.org)

## Debians

```
sudo apt-get install libboost-all-dev
```

### Mac

```
brew install boost
```

### Arch

```
sudo pacman -S boost
```
# Install [OpenSSL](https://www.openssl.org/)

## Debians

```
sudo apt-get install libssl-dev
```

## Mac

## Arch

# Install [cmake](https://cmake.org)

### Debians

```
sudo apt-get install cmake
```

### Mac

```
brew install cmake
```

### Arch

```
sudo pacman -S cmake
```

# Install [P2PSP core](https://github.com/P2PSP/core.git) and [P2PSP console](https://github.com/P2PSP/p2psp-console.git)

```
git clone https://github.com/P2PSP/core.git
git clone https://github.com/P2PSP/p2psp-console.git
```

# Compile

```
cd core
./make.py
cd ../p2psp-console
./make.py
```

# Run a peer

```
cd bin
./peer --splitter_addr 150.214.150.68 --splitter_port 4552 > /dev/null &
vlc http://localhost:9999 &
```

# Run a local team

```
cd bin
./splitter --source_addr 150.214.150.68 --source_port 4551 --splitter_port 8001 --channel BBB-134.ogv --header_size 30000 > /dev/null &
./monitor --splitter_addr 127.0.0.1 --splitter_port 8001 > /dev/null &
vlc http://localhost:9999 &
./peer --splitter_addr 127.0.0.1 --splitter_port 8001 --player_port 10000 &
vlc http://localhost:10000 &
```

# Run a source and a local team

```
cd bin
wget https://upload.wikimedia.org/wikipedia/commons/7/79/Big_Buck_Bunny_small.ogv
cvlc Big_Buck_Bunny_small.ogv --sout "#http{mux=ogg,dst=:8080/BBB.ogv}" :sout-keep &
./splitter --source_addr 127.0.0.1 --source_port 8080 --splitter_port 8001 --channel BBB.ogv --header_size 30000 > /dev/null &
./monitor --splitter_addr 127.0.0.1 --splitter_port 8001 > /dev/null &
vlc http://localhost:9999 &
./peer --splitter_addr 127.0.0.1 --splitter_port 8001 --player_port 10000 &
vlc http://localhost:10000 &
```

# Run a simple Icecast structure
```
# Kill all VLC instances (sources and listeners)
killall vlc

# Create two sources
cvlc ~/Videos/Big_Buck_Bunny_small.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:8000/BBBs.ogv}" --loop &
cvlc ~/Videos/chi84_14_m4.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:8000/LLL.ogv}" --loop &

# Check the infrastructure
firefox http://localhost:8000 &

# Create two listeners
cvlc http://localhost:8000/BBBs.ogv &
cvlc http://localhost:8000/LLL.ogv &
```

# Run a relayed Icecast structure
```
# Kill all VLC instances
killall vlc

# Run a second Icecast2 server listening at port 9000
/usr/bin/icecast2 -b -c ~/icecast-2.4.3/icecast.xml
sleep 5

# Feed the fist (8000) icecast server
cvlc ~/Videos/Big_Buck_Bunny_small.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:8000/BBBs.ogv}" --loop &
cvlc ~/Videos/chi84_14_m4.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:8000/LLL.ogv}" --loop &

# Feed the second (9000) icecast server
cvlc ~/Videos/hcil2003_01.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:9000/hcil.ogv}" --loop &

# Check the infrastructure
firefox http://localhost:8000 &
sleep 10
firefox http://localhost:9000

# The listeners, one for the 8000 and two for the 9000
cvlc http://localhost:8000/BBBs.ogv &
cvlc http://localhost:9000/LLL.ogv &
cvlc http://localhost:9000/hcil.ogv &
```

# Run a hybrid (Iceast/P2PSP) structure
```
# Kill all VLC instances
killall vlc

# Run a second Icecast2 server listening at port 9000
/usr/bin/icecast2 -b -c ~/icecast-2.4.3/icecast.xml

# Feed all icecast servers (2 movies for 8000 and 1 foe 9000)
cvlc ~/Videos/Big_Buck_Bunny_small.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:8000/BBBs.ogv}" --loop &
cvlc ~/Videos/chi84_14_m4.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:8000/LLL.ogv}" --loop &
cvlc ~/Videos/hcil2003_01.ogv --sout "#std{access=shout,mux=ogg,dst=source:hackme@localhost:9000/hcil.ogv}" --loop &

# Check the infrastructure
firefox http://localhost:8000 &
sleep 10
firefox http://localhost:9000

# Create a P2PSP team
xterm -e "~/P2PSP/p2psp-console/bin/splitter --source_addr 127.0.0.1 --source_port 8000 --splitter_port 8001 --channel BBBs.ogv --header_size 30000" &
sleep 1
xterm -e "~/P2PSP/p2psp-console/bin/monitor --splitter_addr 127.0.0.1 --splitter_port 8001" &
sleep 1
cvlc http://localhost:9999 & # The monitor
xterm -e "~/P2PSP/p2psp-console/bin/peer --splitter_addr 127.0.0.1 --splitter_port 8001 --player_port 10000" &
sleep 1
cvlc http://localhost:10000 & # The first peer
```
