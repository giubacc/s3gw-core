# Running s3gw/Longhorn

Refer to the appropriate section to proceed with the setup.

* [s3gw/Longhorn on bare metal](./README.bm.md)
* [s3gw/Longhorn on virtual machines](./README.vm.md)

## Ingresses

Services are exposed with an Kubernetes ingress; each service category is
allocated on a separate virtual host:

* **Longhorn dashboard**, on: `longhorn.local`
* **s3gw**, on: `s3gw.local` and `no-tls-s3gw.local`
* **s3gw UI**, on: `ui-s3gw.local` and `no-tls-ui-s3gw.local`

Host names are exposed with a node port service listening on ports
30443 (https) and 30080 (http).  
You are required to resolve these names with the external ip of one
of the nodes of the cluster.  

You can patch host's `/etc/hosts` file as follow:  

```text
YOUR-HOST-IP   longhorn.local s3gw.local no-tls-s3gw.local ui-s3gw.local no-tls-ui-s3gw.local
```

Services can now be accessed at:

```text
https://longhorn.local:30443
https://s3gw.local:30443
http://no-tls-s3gw.local:30080
https://ui-s3gw.local:30443
http://no-tls-ui-s3gw.local:30080
```
