# How to install 

## shadowsocksR

shadowsocks-python is the initial version written by
[@clowwindy](https://github.com/clowwindy). It aims to provide a
simple-to-use and easy-to-deploy implementation with basic features of
shadowsocks.

```
wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh
chmod +x shadowsocksR.sh
./shadowsocksR.sh 2>&1 | tee shadowsocksR.log
```

## shadowsocks-libev

shadowsocks-libev is a lightweight and full featured port for embedded
devices and low end boxes. It's a pure C implementation and has a very
small footprint (several megabytes) for thousands of connections. This
port is maintained by [@madeye](https://github.com/madeye).

```
wget --no-check-certificate https://raw.githubusercontent.com/tennfy/shadowsocks-libev/master/debian_shadowsocks_tennfy.sh
chmod a+x debian_shadowsocks_tennfy.sh
bash debian_shadowsocks_tennfy.sh
```

## shadowsocks-go

shadowsocks-go is a state-of-the-art port written in Go language,
designed for large-scale system. It implements the
multi-ports-multi-password feature, which is suitable for paid service
providers with user management and traffic statistics support. This
port is maintained by [@cyfdecyf](https://github.com/cyfdecyf).

```
wget --no-check-certificate https://raw.githubusercontent.com/tennfy/shadowsocks-go/master/debian_shadowsocksgo_tennfy.sh
chmod a+x debian_shadowsocksgo_tennfy.sh
bash debian_shadowsocksgo_tennfy.sh
```

## Reference

- https://shadowsocks.org/en/download/servers.html

