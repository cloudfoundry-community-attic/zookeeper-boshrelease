# BOSH Release for Apache ZooKeeper

ZooKeeper is a high-performance coordination service for distributed applications. It exposes common services - such as naming, configuration management, synchronization, and group services - in a simple interface so you don't have to write them from scratch. You can use it off-the-shelf to implement consensus, group management, leader election, and presence protocols.

This project is a BOSH release for Apache ZooKeeper, allowing you to run it on your BOSH server within AWS, OpenStack, vSphere, etc.

[[home](http://zookeeper.apache.org/ "Apache ZooKeeper")]

## Usage

To use this bosh release, first upload it to your bosh:

```
git clone https://github.com/cloudfoundry-community/zookeeper-boshrelease.git
cd zookeeper-boshrelease
bosh upload release releases/zookeeper-1.yml
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest:

```
cp examples/bosh-lite-cluster.yml local-cluster.yml
sed -i '' -e "s/DIRECTOR_UUID/$(bosh status | grep UUID | awk '{print $2}')/" local-cluster.yml
bosh deployment local-cluster.yml
bosh -n deploy
```

Then you can ping any of the servers to confirm they are OK:

```
$ bosh vms
+-------------+---------+---------------+-------------+
| Job/index   | State   | Resource Pool | IPs         |
+-------------+---------+---------------+-------------+
| zookeeper/0 | running | common        | 10.244.0.6  |
| zookeeper/1 | running | common        | 10.244.0.10 |
| zookeeper/2 | running | common        | 10.244.0.14 |
+-------------+---------+---------------+-------------+

$ echo ruok | nc 10.244.0.6 2181
imok

$ echo ruok | nc 10.244.0.10 2181
imok

$ echo ruok | nc 10.244.0.14 2181
imok
```