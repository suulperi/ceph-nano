# HOWTO deploy ceph-nano on OpenShift 4.x
If you need S3 endpoints for Quay, Kafka etc in a small-scale OpenShift lab environment you can easily deploy ceph-nano to satisfy that need. Working example can be found from [here](https://github.com/suulperi/ceph-nano/blob/main/deploy_ceph-nano.yml).

Just follow the next steps and you are good to go. Instructions are tested with OCP 4.6 and 4.7.

Original instructions how to run ceph-nano on Kubernetes can be found from [the project website](https://github.com/ceph/cn).

> It's good to be aware `cn kube` generates a broken yaml and it will take while to fix everything (indendations, doubles and so on)

#### 1) Set Anyuid SCC 
Follow next instructions: https://examples.openshift.pub/deploy/scc-anyuid/ 

#### 2) Don't use default container image - Use: 
> registry.access.redhat.com/rhceph/rhceph-3-rhel7

#### 3) Change Kubernetes Service type from NodePort to ClusterIP

#### 4) Add one more environment variable
> RGW_NAME - value must match with route host name

#### 5) Login to OpenShift

```oc login```

#### 6) Apply

```oc apply -f deploy_ceph-nano.yml -n your-ns```

#### 7) Setup your S3-Client - For example s3cmd configuration should look like this:
```s3cmd --configure``` and change following values:
```

New settings:
  Access Key: userkey
  Secret Key: secretkey
  Default Region: us-east-1
  S3 Endpoint: your.ceph.nano.host.name
  DNS-style bucket+hostname:port template for accessing a bucket: your.ceph.nano.host.name
  Use HTTPS protocol: True
```

#### 8) Test it (example using s3cmd)

```
s3cmd mb s3://foobucket
date > test.txt
s3cmd put ./test.txt s3://foobucket
s3cmd ls s3://foobucket
s3cmd get s3://foobucket/test.txt froms3.txt
```
