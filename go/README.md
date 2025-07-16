Source: [Go](https://go.dev/dl/)

Usage:

Copy packed gz file to you device, say /mnt/data:

```
rm -rf /mnt/data/go
tar -xzf /mnt/data/go1.24.5.linux-armv7.tar.gz -C /mnt/data
export PATH=/mnt/data/go/bin:$PATH
go version
```