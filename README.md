# HOWTO deploy ceph-nano in OpenShift 4.x
If you need S3 endpoints for Quay, Kafka etc in a small-scale OpenShift lab environment you can easily deploy ceph-nano to satisfy that need.

Just follow the next steps and you are good to go. Instructions are tested with OCP 4.6 and 4.7.
Original instructions how to run ceph-nano on Kubernetes can be found from https://github.com/ceph/cn .

It's good to be aware `cn kube` generates a broken yaml and it will take while to fix everything (indendations, doubles and so on)

#### 1) Set Anyuid SCC 
Follow next instructions: https://examples.openshift.pub/deploy/scc-anyuid/ 

#### 2) Don't use default container image - Use: 
> registry.access.redhat.com/rhceph/rhceph-3-rhel7

#### 3) Change Kubernetes Service type from NodePort to ClusterIP

#### 4) Add one more environment variable
> RGW_NAME - value must match with route host name

#### 5) Setup your S3-Client - For example s3cmd configuration should look like this:
```
New settings:
  Access Key: userkey
  Secret Key: secretkey
  Default Region: us-east-1
  S3 Endpoint: your.ceph.nano.host.name
  DNS-style bucket+hostname:port template for accessing a bucket: your.ceph.nano.host.name
```
