# Les pods
```
kubectl version
```
```
minikube start
```
```
minikube status
```
Assurer :  </br>
minikube  </br>
type: Control Plane  </br>
host: Running  </br>
kubelet: Running  </br>
apiserver: Running  </br>
kubeconfig: Configured  </br>
</br>
Pour avoir l'info sur le cluster :
```
kubectl config get-contexts
```
```
kubectl config current-context
```
Changer de contexte :
```
kubectl config use-context <new_context>
```
new_context : docker-desktop </br>
Pour que le terminal connaît le contexte, utiliser un outil comme starship </br>
Le config est dans : cat ~/.config/starship.taml </br>
=> Interessant pour eviter de deployer sur le mauvais cluster </br>

```
kubectl get namespace
```
NAME              STATUS   AGE  </br>
default           Active   2d11h </br>
ingress-nginx     Active   2d11h </br>
kube-node-lease   Active   2d11h </br>
kube-public       Active   2d11h </br>
kube-system       Active   2d11h </br>
```
kubectl get pod -n kube-system
```
# Manipulation des pods
Un pod est l'unité de base déployable dans kubernetes. Il contient: 
* un ou plusieurs conteneurs (généralement seul)
* les ressources partagées ; IP,espace de stockage, volumes, etc.
* les définitions de configuraiton comme les variables d'environnement
  => Un pod est un conteneur </br>
  </br>
  * Déclaratif : fichier yaml
  * Impératif : ligne de commande
Fichier de refenrences
```
apiVersion: v1
kind: pod
metadata:
  name: myapp
  labels:
    name: myapp
spec:
  containers:
  - name: myapp
    image: <Image>
    ressources: 
      limits: "128Mi"
      cpu: "500m"
    ports:
      - containerPort <Port>    
```

```
apiVersion: 
```
 
  
```

```



# Ressources
The formation is will be at [kubernetes_04_les_pods](https://www.youtube.com/watch?v=QiuV-49frh8)
