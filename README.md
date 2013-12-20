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

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a 3 VM cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For Openstack (Nova Networks), create a single VM:

```
templates/make_manifest openstack-nova
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

Then you can ping any of the servers to confirm they are OK:

```
$ bosh vms
+-------------+---------+---------------+-------------+
| Job/index   | State   | Resource Pool | IPs         |
+-------------+---------+---------------+-------------+
| zookeeper/0 | running | small_z1      | 10.244.0.6  |
| zookeeper/1 | running | small_z1      | 10.244.0.10 |
| zookeeper/2 | running | small_z1      | 10.244.0.14 |
+-------------+---------+---------------+-------------+

$ echo ruok | nc 10.244.0.6 2181
imok

$ echo ruok | nc 10.244.0.10 2181
imok

$ echo ruok | nc 10.244.0.14 2181
imok
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group with ports 22 and 2181 open. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

``` yaml
---
networks:
  - name: zookeeper1
    type: dynamic
    cloud_properties:
      security_groups:
        - zookeeper
```

Where `- zookeeper` means you wish to use an existing security group called `zookeeper`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```
