# Corrections exercices
```
nano 03-pod.yaml
```
copier
```
apiVersion: v1
kind: Pod
metadata:
  name: custom-pod
spec:
  containers:
  - name: httpd-container
    image: httpd:latest
    ports:
      - containerPort: 8080
    command: ["/bin/sh", "-c"]
    args: ["echo 'server apache' ; httpd-foreground"] 
```
Tapez ctrl+x puis yes et entrée
```
kubectl apply -f 03-pod.yaml
```
```
kubectl logs custom-pod
```

# Ressources
[deployments](https://www.youtube.com/watch?v=C6NYX2RrE3g&list=PL1CpaUw4aBIxTPvr5l_pKoHxI0Tz2qoyK&index=9)
