Sample k8s Zimbra POC from Docker containers

Using minikube

```
$ minikube start
$ kubectl get node
AME       STATUS    AGE
minikube   Ready     6d
```

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

5) Create the POD and verify.

```
$ kubectl create -f myzimbra.yaml
$ kubectl get pod
Monitor the logs if the zimbra server is ready.
$ kubectl logs <pod name>
```

