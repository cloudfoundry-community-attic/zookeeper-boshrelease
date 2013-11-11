# BOSH Release for zookeeper

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
git clone git@github.com:cloudfoundry-community/zookeeper-boshrelease.git
cd zookeeper-boshrelease
bosh upload release releases/zookeeper-1.yml
```

