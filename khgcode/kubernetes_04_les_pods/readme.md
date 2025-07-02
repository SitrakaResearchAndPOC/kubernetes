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
kind: Pod
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
Génerer notre premier code décalartif
```
apiVersion: v1
kind: Pod
metadata:
  name: khgcode
  labels:
    name: khgcode
spec:
  containers:
  - name: myapp
    image: nginx:latest
    ports:
      - containerPort: 80   
```
Tapez ctrl+x et yes pour confirmer
```
kubectl apply -f 01-pod.yaml
```
Verifier le pod (surtout running) : 
```
kubectl get pod
```
Décrire un pod par 
```
kubectl describe pod <nom_du_pod>
```
```
kubectl describe pod khgcode
```
Induire une erreur puis refaire l'exercice </br>
eg: image : nginxxxxxxx </br>
Regarder status de </br>
```
kubectl get pod
```
Regarder l'erreur pour le debug
```
kubectl describe pod khgcode
```
Rechanger en veritable image de nginx
```
kubectl apply -f 01-pod.yaml
```
Etat running de : 
```
kubectl get pods
```
Voir l'etat de notre pod :
```
kubectl logs <nom_pod>
```
```
kubectl logs khgcode
```


# Ressources
The formation is will be at [kubernetes_04_les_pods](https://www.youtube.com/watch?v=QiuV-49frh8)
