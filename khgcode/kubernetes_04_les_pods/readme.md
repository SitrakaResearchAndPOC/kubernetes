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
Fichier de references (fiche manifest)
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
spec:
  containers:
  - name: nginx
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
comment acceder au pod ?
```
kubectl exec -it <nom_pod> -- cmd
```
Réellement : 
```
kubectl exec -it <nom_pod> -- cmd
```
```
kubectl exec -it khgcode -- /bin/bash
```
OU
```
kubectl exec -it khgcode -- bash
```
```
ls
```
```
cd /temp
```
```
ls
```
```
cd ..
```
```
cat docker-entrypoint.sh 
```
```
cat docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
```
```
exit
```
Sans se connecter au pod
```
kubectl exec -it khgcode -- ls /
```
Création de pod sans YAML (réelement non, mais pour le certif oui) </br>
MODE IMPERATIF
```
kubectl run khgcode2 --image=nginx:latest --port=80
```
# Configurer un pod avec plusieurs conteneurs
Le tiret avec yaml c'est de liste </br>
```
nano 02-podMulti.yaml
```
```
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
      - containerPort: 80  
  - name: sidecar
    image: busybox:latest
    command: ["sh", "-c", "while true; do echo 'testing sidecar'; sleep 5; done"]     
```
Tapez ctrl+x et yes et entrée
Pour mode watcher (pour voir en temps réels les changements d'etat) lancer dans un autre terminal : </br>
eg : démarrage trop longtemps, pour voir les differents états avant running
```
kubectl get pod -w
```
Revenir dans le terminal principal
```
kubectl apply -f 02-podMulti.yaml
```
```
kubectl get pod
```
Supprimer le pod 
```
kubectl delete pod <nom pod>
```
Comme,
```
kubectl delete pod multi-container-pod
```
```
kubectl apply -f 02-podMulti.yaml
```
regarder dans le terminal watcher pour voir le changement d'etat
```
kubectl describe pod  multi-container-pod
```
verifie si khgcode2 existe comme pod
```
kubectl get pod
```
Pour un pod avec un seul conteneur 
```
kubectl logs khgcode2
```
Pour un pod avec plusieurs conteneurs 
```
kubectl logs multi-container-pod
```
On voit seulement le log de nginx </br>
Alors qu'on a deux pods : 
```
kubectl describe pod multi-container-pod | grep Image:
```
```
kubectl logs <nom_pod> -c <nom_conteneur> -f
```
comme 
```
kubectl logs multi-container-pod  -c sidecar -f
```
-f : follow (dans notre cas tous les 5 seconde il va s'afficher) </br>
=> Il faut préciser le nom de conteneur du pod en utilisant -c

```
kubectl logs multi-container-pod  -c nginx -f
```
Ajoutons un autre conteneur dans le pod : 
```
kubectl delete pod multi-container-pod
```
```
kubectl apply -f 02-podMulti.yaml
```
```
kubectl get pod -w
```
```
kubectl describe pod multi-container-pod
```
```
kubectl logs multi-container-pod  -c sidecar2 -f
```

# Ressources
The formation is will be at [kubernetes_04_les_pods](https://www.youtube.com/watch?v=QiuV-49frh8)
