## DOCKER NETWORKING

### Basic commands

- To check the list of network drives
```
docker network ls
```
- To Inspect network

```
docker network inspect <network-name>
```
Example:
```
root@ip-172-31-16-115:~# docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "5e11d554bda9a2c05b5de7c053da9f9e87d6c8150c925f6adb6feae4e466f42f",
        "Created": "2024-01-02T06:38:01.420096705Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "9e9649d2e38b7c6f9fe4e0bf6c8083ac314e07ba5b69edd2e4a14507e7b956dd": {
                "Name": "alph",
                "EndpointID": "18c5ff04d363c027db40f7468860d967ef8a1f8f6e8b87d63b7742179765a843",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

- To create the new network drive
```
docker network create <Network-name>
```
Example:
```
root@ip-172-31-16-115:~# docker network create my_new_network
c047b45414b8b44e5e62727632de956d7896dff81238ee562f2bb119f95fb76b
```
- Now if yo chaeck the network drive's list you will find new network. But the new network is created under bridged network.
```
root@ip-172-31-16-115:~# docker network ls
NETWORK ID     NAME             DRIVER    SCOPE
5e11d554bda9   bridge           bridge    local
b85cffed47a8   host             host      local
c047b45414b8   my_new_network   bridge    local
379d0c0fd632   newbridge        bridge    local
1a26a3bf1e7c   none             null      local
```

- To create new container using new drive
```
docker container run --name <container-name> --network <network-name> nginx
```
- **We can not relay on IP address because it may change when we restart the server on this point DNS is very usefull**
- **Main advantage of coustom made network is that when 2 or more containers are connected to same network you can ping the other containers using DNS name**

- **We can only ping using DNS name to another container when we create an coustom made network it is not possible in default bridge network**

- Login to your container

- Install IP utils package (IP UTILS is used to install ping ackages)
```
apt-get install -y iputils-ping
```
- Now ping the another container that is in same network using container name

```
ping <container name>
```
Example:

- Below i have 2 containers and they are in same network.
```
root@ip-172-31-16-115:~# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
76f0ba0c5388   httpd     "httpd-foreground"       12 minutes ago   Up 12 minutes   80/tcp    second_nginx
e2326abd9a90   nginx     "/docker-entrypoint.…"   7 hours ago      Up 7 hours      80/tcp    mynginx
root@ip-172-31-16-115:~# 
```

- Now i will ping 2nd container mynginx from 1st container httpd-foreground
```
root@ip-172-31-16-115:~# docker container exec -it mynginx bash
root@e2326abd9a90:/# ping second_nginx
PING second_nginx (172.19.0.3) 56(84) bytes of data.
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=1 ttl=64 time=0.063 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=2 ttl=64 time=0.088 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=3 ttl=64 time=0.077 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=4 ttl=64 time=0.064 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=5 ttl=64 time=0.063 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=6 ttl=64 time=0.063 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=7 ttl=64 time=0.063 ms
64 bytes from second_nginx.my_new_network (172.19.0.3): icmp_seq=8 ttl=64 time=0.065 ms
^C
--- second_nginx ping statistics ---
8 packets transmitted, 8 received, 0% packet loss, time 7160ms
rtt min/avg/max/mdev = 0.063/0.068/0.088/0.008 ms
root@e2326abd9a90:/# 
```

- **What is Round robin DNS**

- Round Robin DNS is a technique used in network management where multiple IP addresses are associated with a single domain name in the DNS records. When a user tries to connect to the domain, the DNS server responds with one of these IP addresses, rotating through them in a circular order. This method is often used for load balancing and fault tolerance.

Here's how it works:

1. **Multiple IP Addresses**: The domain name is configured with multiple IP addresses. Each of these IPs typically corresponds to a different server hosting the same service or website.

2. **DNS Query**: When a client makes a DNS query for the domain name, the DNS server responds with one of the IP addresses from the list.

3. **Rotation**: On the next query for the same domain, the DNS server rotates to the next IP address in the list. This rotation continues in a round-robin fashion.

4. **Load Balancing**: By distributing the requests across multiple servers, round robin DNS can help in balancing the load. This can prevent any single server from becoming overloaded with requests.

5. **Fault Tolerance**: If one server becomes unavailable, the DNS will continue to rotate through the remaining servers. This helps in maintaining the availability of the service even if one or more servers fail.

However, Round Robin DNS has some limitations:

- **Lack of Sophistication**: It does not take into account the load or health of the servers; it simply rotates through the list regardless of each server's current status.
- **Caching Issues**: DNS caching by ISPs and client machines can lead to uneven distribution of requests, as some clients may continue to use a cached IP address instead of requesting a new one.
- **No Geographic Awareness**: Round Robin DNS does not consider the geographic location of the client, which can result in suboptimal routing for some users.

Despite these limitations, Round Robin DNS is a simple and cost-effective way to achieve rudimentary load balancing and increase the availability of web services.






























