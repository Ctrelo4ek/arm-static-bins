Source: [iperf2](https://sourceforge.net/projects/iperf2/)

results:

	file ./iperf
	./iperf: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), statically linked, stripped


```
iperf -v
iperf version 2.2.1 (4 Nov 2024) pthreads
```

Compile note:

```bash
add #define _GNU_SOURCE to include/headers.h

LDFLAGS="-static" ./configure
```

