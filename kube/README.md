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
