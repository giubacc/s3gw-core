# s3gw/Longhorn on Bare Metal

Follow this guide if you wish to setup a K3s cluster running s3gw/Longhorn directly on your machine.

# Setup

## Note Before

In some host systems, including OpenSUSE Tumbleweed, one will need to disable
firewalld to ensure proper functioning of k3s and its pods:

```
$ sudo systemctl stop firewalld.service
```

This is something we intend figuring out in the near future.

## K3s Installation

Install k3s from the internet, by running

```
$ curl -sfL https://raw.githubusercontent.com/aquarist-labs/s3gw-core/main/k3s/setup.sh | sh -
```

## Longhorn Deploy

Deploy Longhorn from the internet, by running

```
$ kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.2.4/deploy/prerequisite/longhorn-iscsi-installation.yaml
$ kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.2.4/deploy/longhorn.yaml
```

## s3gw Deploy

Follow the instructions at [s3gw-charts](https://github.com/aquarist-labs/s3gw-charts) repository to deploy s3gw with an Helm chart. 
# Access the Longhorn UI

The Longhorn UI can be access via the URL `http://longhorn.local`.

# Access the S3 API

The S3 API can be accessed via `http://s3gw.local`.

We provide a [s3cmd](https://github.com/s3tools/s3cmd) configuration file
to easily communicate with the S3 gateway in the k3s cluster.

```
$ cd ~/git/s3gw-core/k3s
$ s3cmd -c ./s3cmd.cfg mb s3://foo
$ s3cmd -c ./s3cmd.cfg ls s3://
```

Please adapt the `host_base` and `host_bucket` properties in the `s3cmd.cfg`
configuration file if your K3s cluster is not accessible via localhost.

# Configure s3gw as Longhorn backup target

Use the following values in the Longhorn settings page to use the s3gw as
backup target.

Backup Target: `s3://<BUCKET_NAME>@us/`  
Backup Target Credential Secret: `s3gw-secret`
