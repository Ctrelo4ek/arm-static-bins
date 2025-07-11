Source: [n2n](https://github.com/ntop/n2n)

results:

	file ./edge
	./stunnel: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, stripped


```
./edge -h
Welcome to n2n v.3.1.1-243-g23e5160-dirty
Built on Fri May 2 18:26:25 2025 +0200
Copyright 2007-2022 - ntop.org and contributors

 general usage:  edge <config file> (see edge.conf)

            or   edge  -c <community name> -l <supernode host:port>
                      [-p [<local bind ip address>:]<local port>]
                      [-T <type of service>] [-D]
 options for under-   [-i <registration interval>] [-L <registration ttl>]
 lying connection     [-k <key>] [-A<cipher>] [-H] [-z<compression>]
                      [-e <preferred local IP address>] [-S<level of solitude>]
                      [--select-rtt]

 tap device and       [-a [static:|dhcp:]<tap IP address>[/<cidr suffix>]]
 overlay network      [-m <tap MAC address>] [-d <tap device name>]
 configuration        [-M <tap MTU>] [-r] [-E] [-I <edge description>]
                      [-J <password>] [-P <public key>] [-R <rule string>]

 local options        [-f] [-t <management port>] [--management-password <pw>]
                      [-v] [-V]
                      [-u <numerical user id>] [-g <numerical group id>]

 environment          N2N_KEY         instead of [-k <key>]
 variables            N2N_COMMUNITY   instead of -c <community>
                      N2N_PASSWORD    instead of [-J <password>]

 meaning of the       [-D]  enable PMTU discovery
 flag options         [-H]  enable header encryption
                      [-r]  enable packet forwarding through n2n community
                      [-E]  accept multicast MAC addresses
            [--select-rtt]  select supernode by round trip time
            [--select-mac]  select supernode by MAC address
                      [-f]  do not fork but run in foreground
                      [-v]  make more verbose, repeat as required
                      [-V]  make less verbose, repeat as required

  -h    shows this quick reference including all available options
 --help gives a detailed parameter description
   man  files for n2n, edge, and supernode contain in-depth information
```

Compile note:

```bash
#!/bin/sh

set -x

STANDARD="c++17"

arch="armv5te"
host="arm-linux-musleabi"
base_dir=$(cd $(dirname $0) && pwd)
install_dir="${base_dir}/${arch}_dev"
include_dir="${install_dir}/include"
lib_dir="${install_dir}/lib"
result_dir="${base_dir}/${arch}_result"

custom_flags_set() {
    CXXFLAGS="-std=${STANDARD}"
    CPPFLAGS="--static -static -I${include_dir}"
    LDFLAGS="--static -static -Wl,--no-as-needed -L${lib_dir} -lpthread -pthread"
    LD_LIBRARY_PATH="-L${lib_dir}"
    PKG_CONFIG_PATH="${lib_dir}/pkgconfig"
}

custom_flags_reset() {
    CXXFLAGS="-std=${STANDARD}"
    CPPFLAGS=""
    LDFLAGS=""
}

custom_flags_reset

build_sdns() {
    cd n2n
    custom_flags_set
    make clean
    ./autogen.sh
    CC=${host}-gcc CXX=${host}-g++ AR=${host}-ar CPPFLAGS="${CPPFLAGS}" LDFLAGS="${LDFLAGS}" PKG_CONFIG_PATH="${lib_dir}/pkgconfig" ./configure --host=${host}
    make -j$(nproc)
    [ $? -ne 0 ] && exit 2
    ${host}-strip ./edge
    ${host}-strip ./supernode
    file ./edge
    file ./supernode
    mv ./edge ${result_dir}/edge-${arch}
    mv ./supernode ${result_dir}/supernode-${arch}
}

build_sdns

```

