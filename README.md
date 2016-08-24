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

# Install [cmake](https://cmake.org)

### Debians

```
todo
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
./monitor --splitter_addr 127.0.0.1 --splitter_port 8002 > /dev/null &
vlc http://localhost:9999 &
./peer --splitter_addr 127.0.0.1 --splitter_port 8001 --player_port 10000 &
vlc http://localhost:10000 &
```

# Run a source and a local team

```
cd bin
wget https://upload.wikimedia.org/wikipedia/commons/7/79/Big_Buck_Bunny_small.ogv
cvlc Big_Buck_Bunny_small.ogv --sout "#http{mux=ogg,dst=:8080/BBB-143.ogv}" :sout-keep &
./splitter --source_addr 127.0.0.1 --source_port 8080 --splitter_port 8001 --channel BBB-143.ogv --header_size 30000 > /dev/null &
./monitor --splitter_addr 127.0.0.1 --splitter_port 8001 > /dev/null &
vlc http://localhost:9999 &
./peer --splitter_addr 127.0.0.1 --splitter_port 8001 --player_port 10000 &
vlc http://localhost:10000 &
```


