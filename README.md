Sample k8s Zimbra POC from Docker containers

* Using minikube, you can replicate the zimbra right away after downloading the YAML's

```
$ minikube start
$ kubectl get node
NAME       STATUS    AGE
minikube   Ready     6d
```

* Deploy the cluster using the YAML.

```
$ kubectl create -f myzimbra.yaml
$ kubectl get deployment
NAME      DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
zimbra    1         1         1            1           2h
```

* Check if the POD is created.

```
$ kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
zimbra-842147904-0f4gl   1/1       Running   0          2h
$ kubectl logs zimbra-842147904-0f4gl
hostname - zimbra-842147904-0f4gl
domain - company1.default.svc.cluster.local
fqdn - zimbra-842147904-0f4gl.company1.default.svc.cluster.local
server IP - 172.17.0.4
arp - 0.17.172
```
* Create the service to expose the Zimbra webmail and admin console.

```
$ kubectl create -f svc_zimbra.yaml
$ kubectl get svc
NAME         CLUSTER-IP   EXTERNAL-IP    PORT(S)            AGE
kubernetes   10.0.0.1     <none>         443/TCP            12d
zimbra       10.0.0.17    192.168.64.2   7071/TCP,443/TCP   44m
```
EXTERNAL-IP can be replaced on whatever is your node IP has. on my side, I used 192.168.64.2

```
$ kubectl describe node|grep Addr
Addresses:		192.168.64.2,192.168.64.2,minikube
```

* After the instance is created. You can check the log if the output as shown.

```
Host zimbra-842147904-0f4gl.company1.default.svc.cluster.local
	Starting ldap...Done.
	Starting zmconfigd...Done.
	Starting logger...Done.
	Starting mailbox...Done.
	Starting memcached...Done.
	Starting proxy...Done.
	Starting antispam...Done.
	Starting antivirus...Done.
	Starting snmp...Done.
	Starting spell...Done.
	Starting mta...Done.
	Starting stats...Done.
Restart CROND
Stopping crond: [  OK  ]
Starting crond: [  OK  ]
Server is ready...
Login to https://172.17.0.4 as normal user
Login as admin user at https://172.17.0.4:7071
```

* Login using the EXTERNAL-IP.

```
Webmail login
https://EXTERNAL-IP

Webmail Admin console
https://EXTERNAL-IP:7071
```
