## Assignment: CLI App Testing
- Use different Linux distro containers to check curl cli tool version
- Use two different terminal windows to start bash in both centos:7
and ubuntu:14.04, using -it
- Learn the docker container run —rm option so you can save
cleanup
- Ensure curl is installed and on latest version for that distro
- ubuntu: apt-get update && apt-get install curl
- centos: yum update curl
- Check curl --version

## Assignment: DNS Round Robin Test
- Ever since Docker Engine 1.11, we can have multiple containers
on a created network respond to the same DNS address
* Create a new virtual network (default bridge driver)
- Create two containers from elasticsearch:2 image
- Research and use --network-alias search when creating them
to give them an additional DNS name to respond to
- Run alpine nslookup search with --net to see the two
containers list for the same DNS name
- Run centos curl -s search:9200 with --net multiple times
until you see both "name" fields show
