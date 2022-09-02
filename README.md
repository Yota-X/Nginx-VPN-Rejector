# NGINX VPN Rejector
## VPN Connection's block with NGINX


NGINX VPN Rejector is an NGINX assembly designed for filtering PROXY/VPN traffic based on the IP2Proxy library.

The IP2Proxy database can be downloaded from https://lite.ip2location.com (Free) or https://www.ip2location.com (Commercial).

## Installation

1. ```git clone https://github.com/Yota-X/Nginx-VPN-Rejector.git```
2. ```cd nginx-vpn-rejector/```
3. You need to download the IP2Proxy database and name the file PROXY.BIN and place it in the ```nginx/ip2proxy``` folder in the project folder, after that you need to delete the REMOVEME file from the ```nginx/ip2proxy``` folder.
4. ```docker-compose up -d --build```
5. Enjoy ;)

## Folder/file hierarchy
```
├───logs              # log folder
│   └───nginx         # NGINX log folder
├───nginx             # config/IP2Proxy database folder
│   ├───config        # config's folder
│   │   ├───conf.d    # NGINX default config
│   │   └───security  # IP2Proxy config
│   └───ip2proxy      # IP2Proxy database
└───sites             # Your site
```

## Example

```
http {
	ip2proxy_database           /usr/share/ip2location/PROXY.BIN;
	ip2proxy_proxy_recursive    on;
	ip2proxy_proxy              192.168.1.0/24;
}
```

Block Proxy IP:

```
if ( $ip2proxy_is_proxy = '1' ) {
    return 444;
}
```

Block Spammers

```
if ( $ip2proxy_threat = 'SPAM' ) {
    return 444;
}
```

*You can see how the configuration is implemented in the project directory ```nginx/config/```*

## Source

You can learn more about the functions of the Nginx IP2Proxy module [here](https://github.com/ip2location/ip2proxy-nginx.git) and [here](https://github.com/ip2location/ip2proxy-c.git)

## License

MIT

**Yota-X Free Software, Hell Yeah!**