# Kubernetes commandes

## Affiche la version de kubectl et du cluster kubernetes

```bash
kubectl version
```

## Fournit des informations sur le cluster

```bash
kubectl cluster-info
```

## Liste tous les noeuds du cluster

```bash
kubectl get nodes
```

## Créer une ressource (comme un pod, un service, ou un déploiement)

```bash
kubectl create -f <fichier.yaml>
```

## Liste tous les pods

```bash
kubectl get pods
```

## Affiche les infos détaillées sur une ressource (un pod, un noeud, ou un service)

```bash
kubectl describe <ressource> <nom>
```

## Récupère les journaux d'un conteneur dans un pod

```bash
kubectl logs <nom-du-pod>
```

## Exécute un terminal interactif dans un conteneur d'un pod

```bash
kubectl exec -it <nom-du-pod> -- /bin/bash
```

## Applique une configuration à une ressource

```bash
kubectl apply -f <fichier.yaml>
```

## Supprime une ressource par son nom

```bash
kubectl delete <ressource> <nom>
```

## Met à jour l'image d'un conteneur dans un déploiement

```bash
kubectl set image deployment/<nom-deployment> <conteneur>=<nouvelle-image>
```

## Modifie le nombre de réplicas d'un déploiement

```bash
kubectl scale deployment/<nom-deployment> --replicas=<nombre>
```

## Liste tous les services

```bash
kubectl get services
```

## Expose un déploiement comme un service

```bash
kubectl expose deployment <nom-deployment> --type=<TypeDeService> --port=<port>
```

## Affiche l'état de déploiement d'un déploiement

```bash
kubectl rollout status deployment/<nom-deployment>
```

## Annule le dernier déploiement

```bash
kubectl rollout undo deployment/<nom-deployment>
```

```bash
kubectl delete -f go-demo-2.yml --cascade=orphan

kubectl get rs

kubectl get pods

kubectl create -f go-demo-2.yml --save-config

kubectl get pods

kubectl apply -f go-demo-2-scaled.yml

kubectl get pods

POD_NAME=$(kubectl get pods -o name \
   | tail -1)

kubectl delete $POD_NAME

kubectl get pods

POD_NAME=$(kubectl get pods -o name \
    | tail -1)

kubectl label $POD_NAME service-

kubectl describe $POD_NAME

kubectl get pods --show-labels

kubectl label $POD_NAME service=go-demo-2

kubectl get pods --show-labels

k3d cluster delete mycluster --all
```

## crée un service nommé go-demo-2-svc

```bash
kubectl expose rs go-demo-2 \
    --name=go-demo-2-svc \
    --target-port=28017 \
    --type=NodePort
```

## permet d'accéder à l'interface utilisateur de MongoDB

```bash
kubectl port-forward service/go-demo-2 3000:28017 --address 0.0.0.0
#Now click on the link beside Run button
# Exit the port-forward process by clicking Ctrl + C for Windows
# Or Control + C for Mac before running the next command
```

## Mise à jour de l'image db

```bash
kubectl set image -f go-demo-2-db.yml db=mongo:3.4

kubectl describe -f go-demo-2-db.yml

kubectl get all

kubectl create -f go-demo-2-db-svc.yml
```

## Création d'un déploiement sans temps d'arrêt

```bash
kubectl create -f go-demo-2-api.yml --record

kubectl get -f go-demo-2-api.yml

kubectl set image -f go-demo-2-api.yml api=vfarcic/go-demo-2:2.0 --record

kubectl rollout status -w -f go-demo-2-api.yml

kubectl describe -f go-demo-2-api.yml

kubectl rollout history -f go-demo-2-api.yml

kubectl get rs
```

## Revenir en arrière ou avancer

```bash
kubectl rollout undo -f go-demo-2-api.yml

kubectl describe -f go-demo-2-api.yml

kubectl rollout history -f go-demo-2-api.yml
```

## Revenir à un moment précis dans le temps

```bash
kubectl set image -f go-demo-2-api.yml api=vfarcic/go-demo-2:3.0 --record

kubectl rollout status -f go-demo-2-api.yml

kubectl set image -f go-demo-2-api.yml api=vfarcic/go-demo-2:4.0 --record

kubectl rollout status -f go-demo-2-api.yml

kubectl rollout history -f go-demo-2-api.yml

kubectl rollout undo -f go-demo-2-api.yml --to-revision=2

kubectl rollout history -f go-demo-2-api.yml
```

## Annulation des déploiements ayant échoué

```bash
kubectl set image -f go-demo-2-api.yml api=vfarcic/go-demo-2:does-not-exist --record

kubectl get rs -l type=api

kubectl rollout status -f go-demo-2-api.yml

echo $?

kubectl rollout undo -f go-demo-2-api.yml

kubectl rollout status -f go-demo-2-api.yml

kubectl delete -f go-demo-2-db.yml

kubectl delete -f go-demo-2-db-svc.yml

kubectl delete -f go-demo-2-api.yml
```

## Mise à jour de plusieurs déploiements

```bash
kubectl create -f different-app-db.yml

kubectl get deployments --show-labels

kubectl get deployments -l type=db,vendor=MongoLabs

kubectl set image deployments -l type=db,vendor=MongoLabs db=mongo:3.4 --record

kubectl describe -f go-demo-2.yml
```

## Mise à l'échelle des déploiements

```bash
#wait a few seconds before executing the first command
kubectl apply -f go-demo-2-scaled.yml

kubectl get -f go-demo-2-scaled.yml

kubectl scale deployment go-demo-2-api --replicas 8 --record

kubectl get -f go-demo-2.yml

k3d cluster delete mycluster --all
```

## Service et accès externe

```bash
kubectl create -f go-demo-2-deploy.yml

kubectl get -f go-demo-2-deploy.yml

kubectl get pods

kubectl port-forward service/go-demo-2-api 3000:8080 --address 0.0.0.0
#Now open a new terminal and run the following command
curl -i "0.0.0.0:3000/demo/hello"
#close the new terminal after running and observing the output of curl

kubectl create -f devops-toolkit-dep.yml --record --save-config

kubectl get -f devops-toolkit-dep.yml

kubectl port-forward service/devops-toolkit 3000:80 --address 0.0.0.0
#Now open a new terminal and run the following commands

curl -i "0.0.0.0:3000"

curl -i "0.0.0.0:80/demo/hello"

curl -i \
    -H "Host: devopstoolkitseries.com" \
    "http://0.0.0.0:80"
```

## Dépannage avec minikube

```bash
IP=$(minikube ip)

PORT=$(kubectl get svc go-demo-2-api \
    -o jsonpath="{.spec.ports[0].nodePort}")

curl -i "http://$IP:$PORT/demo/hello"
```

## Activation du contrôleur Ingress

```bash
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml

kubectl get pods --namespace=ingress-nginx

kubectl get pods -n ingress-nginx \
    | grep ingress

nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0 > /dev/null 2>&1 &

# Please wait for a few seconds before running the next commands
curl -i "0.0.0.0:3000/healthz"

curl -i "0.0.0.0:3000/something"
```

## Creation de ressources d'entrée basées sur des chemins

```bash
kubectl create -f go-demo-2-ingress.yml

kubectl get -f go-demo-2-ingress.yml

nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0 > /dev/null 2>&1 &

curl -i "http://0.0.0.0:3000/demo/hello"

kubectl delete -f go-demo-2-ingress.yml

kubectl delete -f go-demo-2-deploy.yml

kubectl create -f go-demo-2.yml \
    --record --save-config

nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0 > /dev/null 2>&1 &

curl -i "http://0.0.0.0:3000/demo/hello"
```

## Creation de ressources d'entrée en fonction des domaines

```bash
kubectl apply \
  -f devops-toolkit-dom.yml \
  --record

nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0 > /dev/null 2>&1 &

curl -i "http://0.0.0.0:3000"

curl -I -H "Host: devopstoolkitseries.com" "http://0.0.0.0:3000"

curl -H "Host: acme.com" "http://0.0.0.0:3000/demo/hello"
```

## Creation d'une ressource Ingress avec le Backend par défaut

```bash
nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0 > /dev/null 2>&1 &

kubectl create \
    -f default-backend.yml

nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0 > /dev/null 2>&1 &

curl -I -H "Host: acme.com" \
    "http://0.0.0.0:3000"

k3d cluster delete mycluster --all
```

## Accéder à une ressource host via le volume hostPath

```bash
kubectl run docker \
    --image=docker:17.11  --restart=Never \
    docker image ls

kubectl get pods

kubectl logs -f docker

kubectl delete pod docker
```

## Exécuter le pod après avoir monté hostPath

```bash
kubectl create -f docker.yml

kubectl exec -it docker \
    -- docker image ls \
    --format "{{.Repository}}"

kubectl exec docker -it -- sh

apk add -U git
git clone \
    https://github.com/Faizan-Zia/go-demo-2
cd go-demo-2

docker image build \
    -t vfarcic/go-demo-2:beta .
docker image ls \
    --format "{{.Repository}}"

docker system prune -f

docker image ls \
    --format "{{.Repository}}"

exit

kubectl delete \
    -f docker.yml
```

## Utilisation de hostPath volume type pour injecter des fichiers de configuration

```bash
kubectl create -f prometheus.yml
kubectl rollout status deploy prometheus

kubectl port-forward service/prometheus --address 0.0.0.0  3000:9090
# click the link beside run button

# open the link beside the run button and add /targets at the end

# open the link beside the run button and add /config at the end
```

## Travailler avec la nouvelle configuration de prometheus

```bash
kubectl apply -f volume/prometheus-host-path.yml

kubectl rollout status deploy prometheus

kubectl port-forward service/prometheus --address 0.0.0.0  3000:9090

kubectl delete \
    -f volume/prometheus-host-path.yml
```

## Etat non persistant

```bash
kubectl create \
    -f jenkins.yml \
    --record --save-config

kubectl rollout status deploy jenkins

kubectl port-forward service/jenkins 3000:8080 --address 0.0.0.0
# click the link besides run button

kubectl port-forward service/jenkins 3000:8080 --address 0.0.0.0
#click link besides run button and add /newJob at the end

POD_NAME=$(kubectl get pods \
    -l service=jenkins,type=master \
    -o jsonpath="{.items[*].metadata.name}")

kubectl exec $POD_NAME -it -- kill 1

kubectl get pods

kubectl port-forward service/jenkins 3000:8080 --address 0.0.0.0
#click the link besides run button
```

## Etat persistant à travers le type de volume emptyDir

```bash
kubectl apply \
    -f jenkins-empty-dir.yml

kubectl rollout status deploy jenkins

kubectl port-forward service/jenkins 3000:8080 --address 0.0.0.0
#Open the link beside run button
#add a new job

POD_NAME=$(kubectl get pods \
    -l service=jenkins,type=master \
    -o jsonpath="{.items[*].metadata.name}")

kubectl exec $POD_NAME -- kill 1

kubectl get pods

kubectl port-forward service/jenkins 3000:8080 --address 0.0.0.0
#Open the link beside run button and remove /newJob at the end

k3d cluster delete mycluster --all
```

## Injection d'une configuration à partir d'un seul fichier

```bash
kubectl create cm my-config \
    --from-file=prometheus-conf.yml

kubectl describe cm my-config

kubectl create -f alpine.yml
kubectl get pods

kubectl exec -it alpine -- \
    ls /etc/config

kubectl exec -it alpine --  \
  ls -l /etc/config

kubectl exec -it alpine -- \
    cat /etc/config/prometheus-conf.yml

kubectl delete -f alpine.yml

kubectl delete cm my-config
```

## Injection de configurations à partir de plusieurs fichiers

```bash
kubectl create cm my-config \
    --from-file=cm/prometheus-conf.yml \
    --from-file=cm/prometheus.yml

kubectl create -f cm/alpine.yml

#Run the following command separately
kubectl exec -it alpine -- \
    ls /etc/config

kubectl delete -f cm/alpine.yml
#Run the following command separately to delete the configmap
kubectl delete cm my-config

kubectl create cm my-config \
    --from-file=cm

kubectl describe cm my-config

kubectl create -f cm/alpine.yml
#Run the below command separately after the "alpine" container is created
kubectl exec -it alpine -- \
    ls /etc/config

kubectl delete -f cm/alpine.yml
#Run the following command separately to delete the configmap
kubectl delete cm my-config
```

## Injection de config à partir de littéraux clé/valeur

```bash
kubectl create cm my-config \
    --from-literal=something=else \
    --from-literal=weather=sunny

kubectl get cm my-config -o yaml

kubectl create -f alpine.yml
#Wait a few seconds before executing the following command
kubectl exec -it alpine -- \
    ls /etc/config

kubectl exec -it alpine -- \
    cat /etc/config/something

kubectl delete -f alpine.yml
#Run the below command separately to the configMap
kubectl delete cm my-config
```

## Injection de config à partir des fichiers d'environnement

```bash
kubectl create cm my-config \
    --from-env-file=my-env-file.yml

kubectl get cm my-config -o yaml
```

## Conversion de la sortie ConfigMap en variables d'environnement

```bash
kubectl create -f alpine-env.yml
#Wait for a few seconds before executing the below command
kubectl exec -it alpine-env -- env

kubectl delete \
    -f alpine-env.yml

kubectl create \
    -f alpine-env-all.yml

kubectl exec -it alpine-env -- env
```

## Création de secrets génériques

```bash
kubectl create secret \
    generic my-creds \
    --from-literal=username=jdoe \
    --from-literal=password=incognito

kubectl get secrets

kubectl get secret my-creds -o json

kubectl get secret my-creds \
    -o jsonpath="{.data.username}" \
    | base64 --decode

kubectl get secret my-creds \
    -o jsonpath="{.data.password}" \
    | base64 --decode
```

## Montage de secrets génériques

```bash
kubectl apply -f jenkins.yml

kubectl rollout status deploy jenkins

POD_NAME=$(kubectl get pods \
    -l service=jenkins,type=master \
    -o jsonpath="{.items[*].metadata.name}")

kubectl exec -it $POD_NAME \
    -- ls /etc/secrets

kubectl exec -it $POD_NAME \
    -- cat /etc/secrets/jenkins-user

kubectl port-forward service/jenkins 3000:8080 --address 0.0.0.0

k3d cluster delete mycluster --all
```

## Déploiement de la première version

```bash
#update image and create go-demo-2.yml
IMG=vfarcic/go-demo-2

TAG=1.0

cat go-demo-2.yml \
    | sed -e \
    "s@image: $IMG@image: $IMG:$TAG@g" \
    | kubectl create -f -

# Rollout status of go-demo-2.yml
kubectl rollout status \
    deploy go-demo-2-api

# Building connection and calling application
nohup kubectl port-forward service/go-demo-2-api 3000:8080 --address 0.0.0.0 > /dev/null 2>&1 &

# Please wait fpr a few second before running the following command:
curl -H "Host: go-demo-2.com"  "http://0.0.0.0:3000/demo/hello"
```

## Explorer les namespaces

```bash
kubectl get ns

kubectl --namespace kube-public get all

kubectl --namespace kube-system get all
```

## Création d'un nouvel espace de noms

```bash
kubectl create ns testing

kubectl get ns

kubectl config set-context testing \
    --namespace testing \
    --cluster k3d-mycluster \
    --user admin@k3d-mycluster

kubectl config view

kubectl config use-context testing

kubectl get all
```

## Déploiement vers un nouvel espace de noms

```bash
IMG=vfarcic/go-demo-2

TAG=2.0

DOM=go-demo-2.com

cat go-demo-2.yml \
    | sed -e \
    "s@image: $IMG@image: $IMG:$TAG@g" \
    | sed -e \
    "s@host: $DOM@host: $TAG\.$DOM@g" \
    | kubectl create -f -

kubectl rollout status \
    deploy go-demo-2-api

nohup kubectl port-forward -n ingress-nginx service/ingress-nginx-controller 3000:80 --address 0.0.0.0  > /dev/null 2>&1 &

curl -H "Host: go-demo-2.com" "http://0.0.0.0:3000/demo/hello"

curl -H "Host: 2.0.go-demo-2.com" "http://0.0.0.0:3000/demo/hello"
```

## Communication entre les namespaces

```bash
kubectl config use-context k3d-mycluster

kubectl run test \
    --image=alpine \
    sleep 10000

kubectl get pod test

kubectl exec -it test \
    -- apk add -U curl

kubectl exec -it test -- curl \
    "http://go-demo-2-api:8080/demo/hello"

kubectl exec -it test -- curl \
    "http://go-demo-2-api.testing:8080/demo/hello"
```

## Suppression de namespace et de tout ses objets

```bash
kubectl delete ns testing

kubectl -n testing get all

kubectl get all

nohup kubectl port-forward service/go-demo-2-api 3000:8080 --address 0.0.0.0 > /dev/null 2>&1 &

# Please wait fpr a few second before running the following command:
curl -H "Host: go-demo-2.com" "http://0.0.0.0:3000/demo/hello"

kubectl set image \
    deployment/go-demo-2-api \
    api=vfarcic/go-demo-2:2.0 \
    --record

k3d cluster delete mycluster --all
```

## Vérification du port sur lequel notre cluster s'exécute

```bash
kubectl config view \
    -o jsonpath='{.clusters[?(@.name=="k3d-mycluster")].cluster.server}'

ls usercode/certs
```

## Même cmdes de vérification du port pour minikube

```bash
# Get the port for minikube cluster
kubectl config view \
    -o jsonpath='{.clusters[?(@.name=="minikube")].cluster.server}'

# Get the certificates for minikube cluster
kubectl config view \
    -o jsonpath='{.clusters[?(@.name=="minikube")].cluster.certificate-authority}'
```

## Créer des users pour accéder au cluster

```bash
mkdir keys
openssl genrsa \
    -out keys/jdoe.key 2048

openssl req -new \
    -key keys/jdoe.key \
    -out keys/jdoe.csr \
    -subj "/CN=jdoe/O=devs"

ls -1 /usercode/certs/client-ca.*

openssl x509 -req \
    -in keys/jdoe.csr \
    -CA /usercode/certs/client-ca.crt \
    -CAkey /usercode/certs/client-ca.key \
    -CAcreateserial \
    -out keys/jdoe.crt \
    -days 365

cp /usercode/certs/server-ca.crt /usercode/certs/keys

ls -1 keys

SERVER=$(kubectl config view \
    -o jsonpath='{.clusters[?(@.name=="k3d-mycluster")].cluster.server}')

echo $SERVER
```

## Accès au cluster en tant que user

```bash
kubectl config set-cluster jdoe \
    --certificate-authority \
    /usercode/certs/keys/server-ca.crt \
    --server $SERVER

kubectl config set-credentials jdoe \
    --client-certificate keys/jdoe.crt \
    --client-key keys/jdoe.key

kubectl config set-context jdoe \
    --cluster jdoe \
    --user jdoe

kubectl config use-context jdoe

kubectl config view

kubectl get pods

kubectl get all
```

## Explorer les clusterRoles prédéfinis

```bash
kubectl config use-context k3d-mycluster

kubectl get all

kubectl auth can-i get pods --as jdoe

kubectl get roles

kubectl get clusterroles

kubectl get clusterroles | grep -v system

kubectl describe clusterrole view

kubectl describe clusterrole edit

kubectl describe clusterrole admin

kubectl describe clusterrole \
    cluster-admin

kubectl auth can-i "*" "*"
```

## Création de RoleBindings

```bash
kubectl create rolebinding jdoe \
    --clusterrole view \
    --user jdoe \
    --namespace default \
    --save-config

kubectl get rolebindings

kubectl describe rolebinding jdoe

kubectl --namespace kube-system \
    describe rolebinding jdoe

kubectl auth can-i get pods \
    --as jdoe

kubectl auth can-i get pods \
    --as jdoe --all-namespaces

kubectl delete rolebinding jdoe
```

## Création de cluster role bindings

```bash
kubectl create -f crb-view.yml \
    --record --save-config

kubectl describe clusterrolebinding \
    view

kubectl auth can-i get pods \
    --as jdoe --all-namespaces
```

## Combinaison de roleBinding avec les namespaces

```bash
kubectl create -f rb-dev.yml \
    --record --save-config

kubectl --namespace dev auth can-i \
    create deployments --as jdoe

kubectl --namespace dev auth can-i \
    delete deployments --as jdoe

kubectl --namespace dev auth can-i \
    "*" "*" --as jdoe

kubectl create -f rb-jdoe.yml \
    --record --save-config

kubectl --namespace jdoe auth can-i \
    "*" "*" --as jdoe
```

## Accès en tant que release manager

```bash
kubectl describe clusterrole admin

kubectl create \
    -f crb-release-manager.yml \
    --record --save-config

kubectl describe \
    clusterrole release-manager

kubectl --namespace default auth \
    can-i "*" pods --as jdoe

kubectl --namespace default auth \
    can-i create deployments --as jdoe

kubectl --namespace default auth can-i \
    delete deployments --as jdoe

kubectl config use-context jdoe

kubectl --namespace default \
     create deployment db \
     --image mongo:3.3

kubectl --namespace default \
    delete deployment db

kubectl config set-context jdoe \
    --cluster jdoe \
    --user jdoe \
    --namespace jdoe

kubectl config use-context jdoe

kubectl create deployment db --image mongo:3.3

kubectl delete deployment db

kubectl create rolebinding mgandhi \
    --clusterrole=view \
    --user=mgandhi \
    --namespace=jdoe
```

## Remplacement des utilisateurs par des groupes

```bash
openssl req -in /usercode/certs/keys/jdoe.csr \
    -noout -subject

kubectl config use-context k3d-mycluster

kubectl apply -f groups.yml \
    --record

kubectl --namespace dev auth \
    can-i create deployments --as jdoe

kubectl config use-context jdoe

kubectl --namespace dev \
     create deployment new-db \
     --image mongo:3.3

k3d cluster delete mycluster --all
```

## Mesurer la mémoire et la consommation cpu réelles

```bash
kubectl --namespace kube-system \
    get pods

kubectl top pods
```

## Définition des valeurs par défaut et des limites de ressources dans un namespace

```bash
kubectl create ns test

kubectl --namespace test create \
    -f limit-range.yml \
    --save-config --record

kubectl describe namespace test

kubectl --namespace test create \
    -f go-demo-2-no-res.yml \
    --save-config --record

kubectl --namespace test \
    rollout status \
    deployment go-demo-2-api

kubectl --namespace test describe \
    pod go-demo-2-db
```

## Scénario Mismatch

```bash
kubectl create ns test

kubectl --namespace test apply \
    -f go-demo-2.yml \
    --record

kubectl --namespace test \
    get events \
    --watch

kubectl delete namespace test
```

## Explorer les effets en violant les quotas

```bash
kubectl create \
    -f dev.yml \
    --record --save-config

kubectl --namespace dev describe \
    quota dev

kubectl --namespace dev create \
    -f go-demo-2.yml \
    --save-config --record

kubectl --namespace dev \
    rollout status \
    deployment go-demo-2-api

kubectl --namespace dev describe \
    quota dev

kubectl --namespace dev apply \
    -f go-demo-2-scaled.yml \
    --record

kubectl --namespace dev get events

kubectl describe namespace dev

kubectl get pods --namespace dev

kubectl --namespace dev apply \
    -f go-demo-2.yml \
    --record

kubectl --namespace dev \
    rollout status \
    deployment go-demo-2-api

kubectl --namespace dev apply \
    -f go-demo-2-mem.yml \
    --record

kubectl --namespace dev get events \
    | grep mem

kubectl describe namespace dev

kubectl --namespace dev apply \
    -f go-demo-2.yml \
    --record

kubectl --namespace dev \
    rollout status \
    deployment go-demo-2-api

kubectl expose deployment go-demo-2-api \
    --namespace dev \
    --name go-demo-2-api \
    --port 8080 \
    --type NodePort

k3d cluster delete mycluster --all
```
