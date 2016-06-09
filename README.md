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
git clone --recursive https://github.com/P2PSP/p2psp-console.git
```

# Compile

```
cd p2psp-console/lib/p2psp
./make.py
cd ../..
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
./splitter --NTS --source_addr 150.214.150.68 --source_port 4551 --team_port 4552 --channel BBB-134.ogv > /dev/null &
./peer --monitor --splitter_addr 127.0.0.1 --splitter_port 4552 > /dev/null &
vlc http://localhost:9999 &
```
# Run a source and a local team

```
cd bin
wget https://upload.wikimedia.org/wikipedia/commons/7/79/Big_Buck_Bunny_small.ogv
cvlc Big_Buck_Bunny_small.ogv --sout "#http{mux=ogg,dst=:8080/bbb}" :sout-keep &
./splitter --NTS --source_addr 127.0.0.1 --source_port 8080 --team_port 4552 --channel BBB-143.ogv > /dev/null &
/peer --monitor --splitter_addr 127.0.0.1 --splitter_port 4552 > /dev/null &
vlc http://localhost:9999 &
```


