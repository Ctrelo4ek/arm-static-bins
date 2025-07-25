Source: [iperf3](https://github.com/esnet/iperf)

results:

	file ./iperf3
	./iperf3: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, stripped


```
iperf3 -v
iperf 3.19 (cJSON 1.7.15)
Optional features available: CPU affinity setting, IPv6 flow label, TCP congestion algorithm setting, sendfile / zerocopy, socket pacing, authentication, bind to device, support IPv4 don't fragment, POSIX threads
```

Compile note:

```bash
add #define TCP_USER_TIMEOUT 18 to src/iperf.h

LDFLAGS="-static" ./configure

make -j8

##error:
cd src
distcc arm-linux-gcc -g -g -O2 -Wall -g -static -s -o iperf3 iperf3-main.o  -L/mmc/lib ./.libs/libiperf.a -lssl -lcrypto -lpthread -pthread
```

