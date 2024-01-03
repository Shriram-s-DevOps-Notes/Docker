# DOCKER NETWORKING
----
- **Overview of Docker Networking**
 ----   
    
    The Docker networking subsystem is pluggable, using drivers.
    
    There are several drivers available by default and provides core networking functionality.
    
    - bridge
    - host
    - overlay =⇒ For docker enterprises
    - macvlan
    - none
    
    In VM also we can observe the different type of networks
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/54f593d6-d22c-4824-9f62-a23f66e62204)

    
    When you run ifconfig on docker-machine other than system network settings you can also see networking details of **docker0**
    
    ```bash
    [root@ip-172-31-89-143 ~]# ifconfig
    **docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
            inet6 fe80::42:65ff:feae:ff82  prefixlen 64  scopeid 0x20<link>
            ether 02:42:65:ae:ff:82  txqueuelen 0  (Ethernet)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 5  bytes 526 (526.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0**
    
    enX0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
            inet 172.31.89.143  netmask 255.255.240.0  broadcast 172.31.95.255
            inet6 fe80::1010:39ff:fe02:9b91  prefixlen 64  scopeid 0x20<link>
            ether 12:10:39:02:9b:91  txqueuelen 1000  (Ethernet)
            RX packets 91086  bytes 105415312 (100.5 MiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 20550  bytes 32333207 (30.8 MiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            inet6 ::1  prefixlen 128  scopeid 0x10<host>
            loop  txqueuelen 1000  (Local Loopback)
            RX packets 24  bytes 2040 (1.9 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 24  bytes 2040 (1.9 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    veth063be55: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet6 fe80::a8f7:b4ff:fe5b:da29  prefixlen 64  scopeid 0x20<link>
            ether aa:f7:b4:5b:da:29  txqueuelen 0  (Ethernet)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 21  bytes 1742 (1.7 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    [root@ip-172-31-89-143 ~]#
    ```
    
    When you want to know about drivers in docker run the below command
    
    Since we are using the community edition we only have 3 drivers.
    
    ```bash
    [root@ip-172-31-89-143 ~]# **docker network ls**
    NETWORK ID     NAME      DRIVER    SCOPE
    df670a89e6ff   bridge    bridge    local
    6501562a2282   host      host      local
    e52a934414b1   none      null      local
    [root@ip-172-31-89-143 ~]#
    ```
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/1379bcf2-cfa6-4f29-9d2e-f6d4bb9cc6b8)

    
    To know about particular networking details
    
    Currently, we have only one container so it is showing details of one container
    
    ```bash
    [root@ip-172-31-89-143 ~]# **docker inspect bridge**
    [
        {
            "Name": "bridge",
            "Id": "df670a89e6ff2e0057ff20cd7071ef09952b6d2d562183e04c135ca73f5131b4",
            "Created": "2023-08-28T05:28:07.252503307Z",
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
                "6309d9a703709b2047f52323ff66be98532ed3cd4c1498e1d036e45bd07b4ca3": {
                    "Name": "myubuntu",
                    "EndpointID": "0ab4b68d1db443954a5da453a7a0f2d513bae098f9e0420d7574ab8f76d43e7d",
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
    [root@ip-172-31-89-143 ~]#
    ```
    
    Suppose you have multiple containers that can be identified by the container IP
    
   ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/ae1a4d69-179d-4885-b2bc-182224bb732c)

    
    We have container IP "IPv4Address": "172.17.0.2/16", and Gateway IP  **"Gateway": "172.17.0.1". This means if the container wants to communicate with the internet it should contact the gateway.** 
    
    Suppose you want to run the container specifically on one driver you can define it during the installation
    
    ```bash
    docker container run -dt --name shriram --network host nginx
    ```
    ----
### Bridged networks
----    
    **WHENEVER YOU CREATE THE NETWORK WITHOUT DEFINING THE NETWORK TYPE AUTOMATICALLY THE DRIVER WILL BE CREATED IN BRIDGED NETWORK**
    
    A bridge network uses a software bridge that allows containers connected to the same bridge network to communicate while providing isolation from containers that are not connected to that bridge network.
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/203794ea-f0ec-4ff5-9031-c05a861cbf8d)

    To check the details about the container run the below command
    
     
    
    ```bash
    docker container inspect shriram
    ```
    
    Under the network section, you can see it's a Bridged type connection.
    
    ```bash
    [root@ip-172-31-89-143 ~]# docker container inspect shriram
    [
        {
            "Id": "34ecb7c30a33d6ad96b84208e8bcb134e45f5f90938eb0fa015bdd8ad0e38d3e",
            "Created": "2023-08-28T08:45:59.133510189Z",
            "Path": "/docker-entrypoint.sh",
            "Args": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "State": {
                "Status": "running",
                "Running": true,
                "Paused": false,
                "Restarting": false,
                "OOMKilled": false,
                "Dead": false,
                "Pid": 9411,
                "ExitCode": 0,
                "Error": "",
                "StartedAt": "2023-08-28T08:45:59.488895982Z",
                "FinishedAt": "0001-01-01T00:00:00Z"
        
            },
            "GraphDriver": {
                "Data": {
                    "LowerDir": "/var/lib/docker/overlay2/6dc3a936a2fae901749580327f1f4de7b6e8aab3b1464d2c20d62d5304d77813-init/diff:/var/lib/docker/overlay2/2b5b2f858417d919b0d552851c9e3ff51cfdda45bf82866aca2fc4d00677ab68/diff:/var/lib/docker/overlay2/205337524b3b5b5e45cf2927b6ed327e4d4787a797566443f961125891d349fd/diff:/var/lib/docker/overlay2/2b9903a5d307ad3020a9b05323beb7f5013e3403f43bc8012b13c43e2c0b4642/diff:/var/lib/docker/overlay2/fe3ce274aa94bc452d442ef24ef619102f513aec586d698141e2c431afc6d791/diff:/var/lib/docker/overlay2/f23a07c87bf0275bdce8d2e823b197d2849c3f354cfde44ff5a2ac05460184c6/diff:/var/lib/docker/overlay2/2577f6eda0735d37d8b00e1616116dc979c0b45515d524f2543712c5445a476f/diff:/var/lib/docker/overlay2/c6966111ebd65518e2835df84858a7b905e4c8165760e83d33de7ba830063e6c/diff",
                    "MergedDir": "/var/lib/docker/overlay2/6dc3a936a2fae901749580327f1f4de7b6e8aab3b1464d2c20d62d5304d77813/merged",
                    "UpperDir": "/var/lib/docker/overlay2/6dc3a936a2fae901749580327f1f4de7b6e8aab3b1464d2c20d62d5304d77813/diff",
                    "WorkDir": "/var/lib/docker/overlay2/6dc3a936a2fae901749580327f1f4de7b6e8aab3b1464d2c20d62d5304d77813/work"
                },
                "Name": "overlay2"
            },
            "Mounts": [],
            "Config": {
                "Hostname": "ip-172-31-89-143.ec2.internal",
                "Domainname": "",
                "User": "",
                "AttachStdin": false,
                "AttachStdout": false,
                "AttachStderr": false,
                "ExposedPorts": {
                    "80/tcp": {}
                },
                "Tty": true,
                "OpenStdin": false,
                "StdinOnce": false,
                "Env": [
                    "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                    "NGINX_VERSION=1.25.2",
                    "NJS_VERSION=0.8.0",
                    "PKG_RELEASE=1~bookworm"
                ],
                "Cmd": [
                    "nginx",
                    "-g",
                    "daemon off;"
                ],
                "Image": "nginx",
                "Volumes": null,
                "WorkingDir": "",
                "Entrypoint": [
                    "/docker-entrypoint.sh"
                ],
                "OnBuild": null,
                "Labels": {
                    "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
                },
                "StopSignal": "SIGQUIT"
            },
            "NetworkSettings": {
                "Bridge": "",
                "SandboxID": "ea8db84eddd0c0ce69cd6b10b1dff018fd1711e91dff1767d095102ee815c3e4",
                "HairpinMode": false,
                "LinkLocalIPv6Address": "",
                "LinkLocalIPv6PrefixLen": 0,
                "Ports": {},
                "SandboxKey": "/var/run/docker/netns/default",
                "SecondaryIPAddresses": null,
                "SecondaryIPv6Addresses": null,
                "EndpointID": "",
                "Gateway": "",
                "GlobalIPv6Address": "",
                "GlobalIPv6PrefixLen": 0,
                "IPAddress": "",
                "IPPrefixLen": 0,
                "IPv6Gateway": "",
                "MacAddress": "",
                "Networks": {
                    "host": {
                        "IPAMConfig": null,
                        "Links": null,
                        "Aliases": null,
                        "NetworkID": "6501562a2282f00dfefff0a81caa1f266d50f66196f0506dcff9ebed988cb8f3",
                        "EndpointID": "f852cfec66d021517f6233f4fd2f67d8257c10d319ad77eb4b2b61a3b48f9fcd",
                        "Gateway": "",
                        "IPAddress": "",
                        "IPPrefixLen": 0,
                        "IPv6Gateway": "",
                        "GlobalIPv6Address": "",
                        "GlobalIPv6PrefixLen": 0,
                        "MacAddress": "",
                        "DriverOpts": null
                    }
                }
            }
        }
    ]
    ```
    
- As the above image shows the containers can communicate with each other
    
    For the demo, we will create containers of Ubuntu
    
    ```bash
    [root@ip-172-31-89-143 ~]# docker container run -dt --name bridge1 ubuntu
    df86319bd079e75c55d71c9fece154d3f584a4c4bacd455fa7057e58560587ef
    [root@ip-172-31-89-143 ~]# docker container run -dt --name bridge2 ubuntu
    7b2b76e44a355f9fb919d2793d9d6f645ab441fccc0fef15402464d77b15a5d6
    
    root@ip-172-31-89-143 ~]# docker ps
    CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
    7b2b76e44a35   ubuntu    "/bin/bash"              3 seconds ago    Up 2 seconds              bridge2
    df86319bd079   ubuntu    "/bin/bash"              10 seconds ago   Up 9 seconds              bridge1
    34ecb7c30a33   nginx     "/docker-entrypoint.…"   22 minutes ago   Up 22 minutes             shriram
    6309d9a70370   ubuntu    "/bin/bash"              2 hours ago      Up 2 hours                myubuntu
    [root@ip-172-31-89-143 ~]#
    ```
    
    Now let's log in to the container.
    
    After login install net-tools 
    
    Run ifconfig
    
    ```bash
    [root@ip-172-31-89-143 ~]# docker container exec -it bridge1 bash
    root@df86319bd079:/# apt-get update && apt install net-tools
    Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [110 kB]
    Get:2 http://archive.ubuntu.com/ubuntu jammy InRelease [270 kB]
    Get:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease [119 kB]
    Get:4 http://archive.ubuntu.com/ubuntu jammy-backports InRelease [109 kB]
    Get:5 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [44.0 kB]
    Get:6 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [983 kB]
    Get:7 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [897 kB]
    Get:8 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [860 kB]
    Get:9 http://archive.ubuntu.com/ubuntu jammy/universe amd64 Packages [17.5 MB]
    Get:10 http://archive.ubuntu.com/ubuntu jammy/restricted amd64 Packages [164 kB]
    Get:11 http://archive.ubuntu.com/ubuntu jammy/main amd64 Packages [1792 kB]
    Get:12 http://archive.ubuntu.com/ubuntu jammy/multiverse amd64 Packages [266 kB]
    Get:13 http://archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [49.8 kB]
    Get:14 http://archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1247 kB]
    Get:15 http://archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [923 kB]
    Get:16 http://archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [1136 kB]
    Get:17 http://archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [49.2 kB]
    Get:18 http://archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [25.6 kB]
    Fetched 26.5 MB in 2s (12.0 MB/s)
    Reading package lists... Done
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    The following NEW packages will be installed:
      net-tools
    0 upgraded, 1 newly installed, 0 to remove and 2 not upgraded.
    Need to get 204 kB of archives.
    After this operation, 819 kB of additional disk space will be used.
    Get:1 http://archive.ubuntu.com/ubuntu jammy/main amd64 net-tools amd64 1.60+git20181103.0eebece-1ubuntu5 [204 kB]
    Fetched 204 kB in 0s (2370 kB/s)
    debconf: delaying package configuration, since apt-utils is not installed
    Selecting previously unselected package net-tools.
    (Reading database ... 4395 files and directories currently installed.)
    Preparing to unpack .../net-tools_1.60+git20181103.0eebece-1ubuntu5_amd64.deb ...
    Unpacking net-tools (1.60+git20181103.0eebece-1ubuntu5) ...
    Setting up net-tools (1.60+git20181103.0eebece-1ubuntu5) ...
    root@df86319bd079:/# ifconfig
    eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet **172.17.0.3**  netmask 255.255.0.0  broadcast 172.17.255.255
            ether 02:42:ac:11:00:03  txqueuelen 0  (Ethernet)
            RX packets 1138  bytes 26801309 (26.8 MB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 819  bytes 58638 (58.6 KB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            loop  txqueuelen 1000  (Local Loopback)
            RX packets 0  bytes 0 (0.0 B)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 0  bytes 0 (0.0 B)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    root@df86319bd079:/#
    ```
    
    Now exit from 1st container and check the IP of the 2nd container
    
    IP of 2nd container is 172.17.0.4
    
    Again log to container 1
    
    Install iputils-ping
    
    ```bash
    apt install iputils-ping
    ```
    
    Now ping the 2nd container from the 1st container
    
    ```bash
    root@df86319bd079:/# ping 172.17.0.4
    PING 172.17.0.4 (172.17.0.4) 56(84) bytes of data.
    64 bytes from 172.17.0.4: icmp_seq=1 ttl=127 time=0.079 ms
    64 bytes from 172.17.0.4: icmp_seq=2 ttl=127 time=0.059 ms
    64 bytes from 172.17.0.4: icmp_seq=3 ttl=127 time=0.059 ms
    64 bytes from 172.17.0.4: icmp_seq=4 ttl=127 time=0.058 ms
    64 bytes from 172.17.0.4: icmp_seq=5 ttl=127 time=0.057 ms
    64 bytes from 172.17.0.4: icmp_seq=6 ttl=127 time=0.059 ms
    64 bytes from 172.17.0.4: icmp_seq=7 ttl=127 time=0.058 ms
    ^C
    --- 172.17.0.4 ping statistics ---
    7 packets transmitted, 7 received, 0% packet loss, time 6269ms
    rtt min/avg/max/mdev = 0.057/0.061/0.079/0.007 ms
    root@df86319bd079:/#
    ```
    
    Every time to ping outside or inside the containers must pass through the gateway
    
    to check that run the below command
    
    ```bash
    route -n
    ```
    
    ```bash
    root@df86319bd079:/# route -n
    Kernel IP routing table
    Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
    0.0.0.0         172.17.0.1      0.0.0.0         UG    0      0        0 eth0
    172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 eth0
    root@df86319bd079:/#
    ```
    
    Bridge is the default network driver for Docker.
    
    If we do not specify a driver,  this is the type of network you are creating.
    
    When you start Docker, a default bridge network (also called a bridge) is created automatically, and newly-started containers connect to it unless otherwise specified.
    
    We also can create a User-Defined Bridge Network which is superior to the default bridge.
    
- **Implementing User-Defined Bridge Networks**
    
    To check what are the bridge-utils are present
    
    ```python
    yum install -y bridge-utils
    ```
    
    Then run the below command
    
    ```python
    brctl show
    ```
    
    ```python
    [root@ip-172-31-44-56 ~]# brctl show
    bridge name     bridge id               STP enabled     interfaces
    docker0         8000.024268cee3e7       no              veth129b8e5
                                                            veth2d30c34
                                                            veth9008a80
                                                            veth928bc32
    ```
    
    Currently, there are 3 bridges. This is the reason for the intercommunication between the bridges. 
    
    3-bridges means 3 containers are running in the same container.
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/d4489371-b871-4277-bf91-ca33b307e8f4)

    
    There are differences between default and user-defined bride. Some of these include:
    
    - User-defined bridges provide better isolation and interoperability between containerized applications.
    - **User-defined bridges provide automatic DNS resolution between containers.**
    
    This means we ping the container using the DNS name or container name. This feature is not available in default container bridges.
    
    EX: U have 2 container container_1 & container container_2 having custom bridge network. Now you login to container 1 and you can ping container 2 using its name.
    
    But this is impossible in the default bridge.
    
    ```python
    ping container_2
    ```
    
    - Containers can be attached and detached from user-defined networks on the fly.
    - Each user-defined network creates a configurable bridge.
    - Linked containers on the default bridge network share environmental variables
    
    To create the new user-defined bridges
    
    1. The Name can be anything and the Driver should be the important point
    
    To create the new driver
    
    ```python
     docker network create --driver <driver> <name>
    ```
    
    The new bridge is created. Even if you did not specify the driver type by default bridge will be selected.
    
    ```python
    [root@ip-172-31-89-143 ~]# docker network ls
    NETWORK ID     NAME      DRIVER    SCOPE
    bea8346de8f9   bridge    bridge    local
    6501562a2282   host      host      local
    e52a934414b1   none      null      local
    
    [root@ip-172-31-89-143 ~]# **docker network create --driver bridge mybridge**
    453f55ef76b91da72022bfcae561f7961481967ee9128e6cc0c1cf7e93c07272
    
    [root@ip-172-31-89-143 ~]# docker network ls
    NETWORK ID     NAME       DRIVER    SCOPE
    bea8346de8f9   bridge     bridge    local
    6501562a2282   host       host      local
    **453f55ef76b9   mybridge   bridge    local**
    e52a934414b1   none       null      local
    
    ```
    
    Now if you do ifconfig you can see the driver. The first one is the new driver.
    
    ```python
    [root@ip-172-31-89-143 ~]# ifconfig
    **br-453f55ef76b9: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500
            inet 172.18.0.1  netmask 255.255.0.0  broadcast 172.18.255.255
            ether 02:42:9e:9a:c5:b3  txqueuelen 0  (Ethernet)
            RX packets 82  bytes 6442 (6.2 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 97  bytes 212441 (207.4 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0**
    
    docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
            inet6 fe80::42:d1ff:fe31:51a1  prefixlen 64  scopeid 0x20<link>
            ether 02:42:d1:31:51:a1  txqueuelen 0  (Ethernet)
            RX packets 1189  bytes 73918 (72.1 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 1859  bytes 45176629 (43.0 MiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    enX0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 9001
            inet 172.31.89.143  netmask 255.255.240.0  broadcast 172.31.95.255
            inet6 fe80::1010:39ff:fe02:9b91  prefixlen 64  scopeid 0x20<link>
            ether 12:10:39:02:9b:91  txqueuelen 1000  (Ethernet)
            RX packets 56459  bytes 48912523 (46.6 MiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 24551  bytes 2374283 (2.2 MiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
            inet 127.0.0.1  netmask 255.0.0.0
            inet6 ::1  prefixlen 128  scopeid 0x10<host>
            loop  txqueuelen 1000  (Local Loopback)
            RX packets 24  bytes 2040 (1.9 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 24  bytes 2040 (1.9 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    
    veth9f11e9a: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
            inet6 fe80::4061:32ff:fefb:10f0  prefixlen 64  scopeid 0x20<link>
            ether 42:61:32:fb:10:f0  txqueuelen 0  (Ethernet)
            RX packets 82  bytes 6442 (6.2 KiB)
            RX errors 0  dropped 0  overruns 0  frame 0
            TX packets 97  bytes 212441 (207.4 KiB)
            TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
    ```
    
    Let’s create a container using the new bridge
    
    ```python
    docker container run --name bridge1 --network **mybridge ubuntu
    ```
    
    Now we launched 2 containers.
    
    ```python
    [root@ip-172-31-89-143 ~]# docker container run -dt --name bridge123 --network mybridge ubuntu
    b8638648b723b9fdf7cc71b4fbad03e8bc088fab2bed9ed54f9dd3b2998decfe
    
    [root@ip-172-31-89-143 ~]# docker ps
    CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
    b8638648b723   ubuntu    "/bin/bash"              4 seconds ago   Up 3 seconds             bridge123
    1561655e68f8   nix       "nginx -g 'daemon of…"   4 hours ago     Up 4 hours
                   ng
    [root@ip-172-31-89-143 ~]# docker container run -dt --name bridge124 --network mybridge ubuntu
    6f9a960677fbbc5cca25891d4af66bf369906ec3ac759da6ef37b10d5fd04303
    
    [root@ip-172-31-89-143 ~]# docker ps
    CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
    6f9a960677fb   ubuntu    "/bin/bash"              3 seconds ago   Up 2 seconds             bridge124
    b8638648b723   ubuntu    "/bin/bash"              2 minutes ago   Up 2 minutes             bridge123
    1561655e68f8   nix       "nginx -g 'daemon of…"   4 hours ago     Up 4 hours               ng
    ```
    
    From the below, you can see that the what are all the networks are connected to the bridge.
    
    ```python
    [root@ip-172-31-89-143 ~]# docker network inspect mybridge
    [
        {
            "Name": "mybridge",
            "Id": "453f55ef76b91da72022bfcae561f7961481967ee9128e6cc0c1cf7e93c07272",
            "Created": "2023-09-07T06:01:12.14560373Z",
            "Scope": "local",
            "Driver": "bridge",
            "EnableIPv6": false,
            "IPAM": {
                "Driver": "default",
                "Options": {},
                "Config": [
                    {
                        "Subnet": "172.18.0.0/16",
                        "Gateway": "172.18.0.1"
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
            **"Containers": {
                "6f9a960677fbbc5cca25891d4af66bf369906ec3ac759da6ef37b10d5fd04303": {
                    "Name": "bridge124",
                    "EndpointID": "d7e4b8df4404321ff21324743fdc08055049ccb6b4c0d2574085cc491eebc11d",
                    "MacAddress": "02:42:ac:12:00:03",
                    "IPv4Address": "172.18.0.3/16",
                    "IPv6Address": ""
                },
                "b8638648b723b9fdf7cc71b4fbad03e8bc088fab2bed9ed54f9dd3b2998decfe": {
                    "Name": "bridge123",
                    "EndpointID": "52e583ea89c27025508fd0fb35c8429f1a16dfb520fad0209739372a5e1848bb",
                    "MacAddress": "02:42:ac:12:00:02",
                    "IPv4Address": "172.18.0.2/16",
                    "IPv6Address": ""
                }**
            },
            "Options": {},
            "Labels": {}
        }
    ```
----    
### Host Network
 ----   
    This driver removes the network isolation between the docker host and the docker containers to use the host’s networking directly.
    
    For instance, if you run a container that binds to port 80 and you use host networking, the container’s application will be available on port 80 on the host’s IP address.
    
    In simple we have to mention -p 80:80 in bridge network to connect to the host from outside and inside the container. But in the host driver, we don’t have to add -p 80:80 because the host will connect the container directly to the interface.
    
    If you install NGINX inside one container it will directly connect to the outside of the port. This is a disadvantage also because we can’t use port 80 on other containers.
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/4d5ababc-955b-4a67-a0a8-c1e35e91805e)

    
    Another great advantage of the host network is in the bridge network every container has different eth0 and IP’s this is not good while installing the intrusion detection system.
----    
### None Network
----   
    If you want to completely disable the networking stack on a container, you can use the none network.
    
    This mode will not configure any IP for the container and doesn’t have any access to the external network as well as for other containers.
    
    The same specifications can be seen in VM where we can see that the containers are isolated because we are doing malware testing so that the virus can not escape the server.
    
    Let’s create the none network for practical
    
    ```python
    [root@ip-172-31-44-56 ~]# docker container run -dt --name nonetest --network none ubuntu
    30f4a81b1a77395b67b1d09d21486169fef0fa8e6303d2581d61f34d8a803c07
    
    [root@ip-172-31-44-56 ~]# docker ps
    CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS                   PORTS     NAMES
    30f4a81b1a77   ubuntu         "/bin/bash"   4 seconds ago   Up 3 seconds                       nonetest
    39c7da5291b3   a8da8769259d   "/bin/bash"   5 hours ago     Up 5 hours                         envcheck
    c9dbeeb059db   da1c           "/bin/bash"   6 hours ago     Up 6 hours                         workdir
    242e867c43dc   0c1d4a942a17   "/bin/bash"   6 hours ago     Up 6 hours (unhealthy)             unhealthy
    cad8cee67e1c   3f045a715e22   "/bin/bash"   7 hours ago     Up 7 hours                         ca
    ```
    
    Let's get inside the container. But we are not able to connect the server because of the no internet connectivity. We can open the container but we can not be able to connect the internet.
    
    ```python
    [root@ip-172-31-44-56 ~]# docker container exec 30f4a81b1a7 apt-get update
    Ign:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
    Ign:2 http://archive.ubuntu.com/ubuntu jammy InRelease
    Ign:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease
    Ign:4 http://archive.ubuntu.com/ubuntu jammy-backports InRelease
    Ign:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
    Ign:2 http://archive.ubuntu.com/ubuntu jammy InRelease
    Ign:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease
    Ign:4 http://archive.ubuntu.com/ubuntu jammy-backports InRelease
    Ign:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
    Ign:2 http://archive.ubuntu.com/ubuntu jammy InRelease
    Ign:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease
    Ign:4 http://archive.ubuntu.com/ubuntu jammy-backports InRelease
    Err:1 http://security.ubuntu.com/ubuntu jammy-security InRelease
      Temporary failure resolving 'security.ubuntu.com'
    Err:2 http://archive.ubuntu.com/ubuntu jammy InRelease
      Temporary failure resolving 'archive.ubuntu.com'
    Err:3 http://archive.ubuntu.com/ubuntu jammy-updates InRelease
      Temporary failure resolving 'archive.ubuntu.com'
    Err:4 http://archive.ubuntu.com/ubuntu jammy-backports InRelease
      Temporary failure resolving 'archive.ubuntu.com'
    Reading package lists...
    W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/jammy/InRelease  Temporary failure resolving 'archive.ubuntu.com'
    W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/jammy-updates/InRelease  Temporary failure resolving 'archive.ubuntu.com'
    W: Failed to fetch http://archive.ubuntu.com/ubuntu/dists/jammy-backports/InRelease  Temporary failure resolving 'archive.ubuntu.com'
    W: Failed to fetch http://security.ubuntu.com/ubuntu/dists/jammy-security/InRelease  Temporary failure resolving 'security.ubuntu.com'
    W: Some index files failed to download. They have been ignored, or old ones used instead.
    [root@ip-172-31-44-56 ~]# docker container exec 30f4a81b1a7 ifconfig
    OCI runtime exec failed: exec failed: unable to start container process: exec: "ifconfig": executable file not found in $PATH: unknown
    [root@ip-172-31-44-56 ~]#
    ```
    
    There will be only one loopback address of 127.0.0.1
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/817d10a5-41d7-4b13-a043-8dba6d70f88e)

----   
### Publishing Exposed Ports of Container
----    
    We were discussing an approach to publishing container ports to the host.
    
    ```python
    docker container run -dt --name webserver -p 80:80 nginx
    ```
    
    This is also referred to as a publishlist(-p) as it publishes only a list of the port specified.
    
    There is also a second approach to publish all the exposed ports of the container.
    
    ```python
    docker container run -dt --name webserver -P nginx
    ```
    
    This is also referred to as a publishall (P).
    
    In this approach, all exposed ports are published to random ports of the host.
    
    ![image](https://github.com/Shriram-s-DevOps-Notes/Docker/assets/110009356/43adcd30-27d5-4e8b-a917-05952400709a)

    
### Legacy Approach for Linking Containers
    
    Before the Docker networks feature, you could use the Docker link feature to allow containers to discover each other and securely transfer information about one container to another container.
    
    The --link flag is a legacy feature of Docker. It may eventually be removed. Unless you absolutely need to continue using it, we recommend that you use user-defined networks to facilitate communication between two containers instead of using --link
    
    ```python
    docker container run -dt --link <container name which you wnat to link>:<Alias name> --name <new container name> <ubuntu,busybox..etc> sh
    ```
    
    The main advantage of this is we can connect the 2 containers like custom bridged networks.
    
    This is the oldest approach to the connecting bridged network. Now we have a custom bridged network. So no use of this noe.
----
- **What is Round robin DNS ?**
----
- Round Robin DNS is a technique used in network management where multiple IP addresses are associated with a single domain name in the DNS records. When a user tries to connect to the domain, the DNS server responds with one of these IP addresses, rotating through them in a circular order. This method is often used for load balancing and fault tolerance.

Here's how it works:

1. **Multiple IP Addresses**: The domain name is configured with multiple IP addresses. Each of these IPs typically corresponds to a different server hosting the same service or website.

2. **DNS Query**: When a client makes a DNS query for the domain name, the DNS server responds with one of the IP addresses from the list.

3. **Rotation**: On the next query for the same domain, the DNS server rotates to the next IP address in the list. This rotation continues in a round-robin fashion.

4. **Load Balancing**: By distributing the requests across multiple servers, round-robin DNS can help in balancing the load. This can prevent any single server from becoming overloaded with requests.

5. **Fault Tolerance**: If one server becomes unavailable, the DNS will continue to rotate through the remaining servers. This helps in maintaining the availability of the service even if one or more servers fail.

However, Round Robin DNS has some limitations:

- **Lack of Sophistication**: It does not take into account the load or health of the servers; it simply rotates through the list regardless of each server's current status.
- **Caching Issues**: DNS caching by ISPs and client machines can lead to uneven distribution of requests, as some clients may continue to use a cached IP address instead of requesting a new one.
- **No Geographic Awareness**: Round Robin DNS does not consider the geographic location of the client, which can result in suboptimal routing for some users.

Despite these limitations, Round Robin DNS is a simple and cost-effective way to achieve rudimentary load balancing and increase the availability of web services.






























