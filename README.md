# mosdns_docker
Key
```
head -c 32 /dev/urandom > ./mosdns-x/.mosdns_stateless_reset.key
```
compose.yml
```
services:
  mosdns-x:
    image: bibica/mosdns-x:mod-latest
    container_name: mosdns-x
    restart: always
    environment:
      - TZ=Asia/Ho_Chi_Minh 
    ports:
      - "53:53/tcp"   # TCP
      - "53:53/udp"   # UDP
      - "443:443/tcp" # DOH
      - "443:443/udp" # DOH3
      - "853:853/tcp" # DOT
      - "853:853/udp" # DOQ
    volumes:
      - ./mosdns-x/config:/home/mosdns-x/config
      - ./mosdns-x/log:/home/mosdns-x/log
      - ./mosdns-x/rules:/home/mosdns-x/rules
      - ./mosdns-x/.mosdns_stateless_reset.key:/home/mosdns-x/.mosdns_stateless_reset.key
      - ./lego:/home/lego
    networks:
      reverse_proxy:
        ipv4_address: 172.18.0.6

networks:
  reverse_proxy:
    driver: bridge
    name: reverse_proxy
    ipam:
      driver: default
      config:
        - subnet: 172.18.0.0/16
```
