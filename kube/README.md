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
