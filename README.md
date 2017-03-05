Sample k8s Zimbra POC from Docker containers

Using minikube, you can replicate the zimbra right away after downloading the YAML's

```
$ minikube start
$ kubectl get node
NAME       STATUS    AGE
minikube   Ready     6d
$ kubectl create -f myzimbra.yaml
$ kubectl create -f svc_zimbra.yaml
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

Be sure to have the hostname and domain, else the installation will fail.

if the docker image is behind the private repository

Create a POD using the docker container.

1) Login to Docker

```
$ docker login
```

2) Creating a Secret that holds your authorization token

```
$ kubectl create secret docker-registry regsecret --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

3) Understanding your Secret

```
$ kubectl get secret regsecret --output=yaml
```

4) Creating a Pod that uses your Secret

```
Checkout the file myzimbra.yaml
DockerHub information:
Account: roselroa
Container: myzimbra-auto
Tag: latest
```

5) Deploy and verify.

```
$ kubectl create -f myzimbra.yaml
$ kubectl create -f svc_zimbra.yaml
$ kubectl get deployment
$ kubectl get pods
NAME                     READY     STATUS    RESTARTS   AGE
zimbra-842147904-0f4gl   1/1       Running   0          2h
Monitor the logs if the zimbra server is ready.
$ kubectl logs zimbra-842147904-0f4gl
```
