
Pour ce TP Kubernetes, j'ai repris le code source de l'application et du cache de notre cours. 

- Partie 1 :
  - J'ai activé Kubernetes avec Docker Desktop.
  - INGRESS Nginx : 
    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.7.1/deploy/static/provider/cloud/deploy.yaml
    ```
  - Installer dashboard
    ```
    kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/charts/recommended.yaml
    ```
  - Activer le dashboard
    ```
    kubectl proxy
    ```
  - Création du token pour se connecter dans le dashboard
    ```
    kubectl apply -f dashboard-adminuser.yaml
    kubectl apply -f clusterRoleBinding.yaml
    kubectl -n kubernetes-dashboard create token admin-user
    ```
  - Lancer un docker registry en local 
    ```
    docker run -d -p 5000:5000 --restart always --name registry registry:2
    ```
- Partie 2 :  
    Voir le manifest.yaml 
    <br/>
    Le fichier déploie l'application et le cache, expose deux services ainsi qu'un ingress pour pouvoir accèder à l'application depuis l'externe.
    
    Avant d'appliquer ce fichier, il fallait d'abord build les images de l'application et du cache, j'ai utilisé les scripts fournis par le cours. J'ai ensuite push ces images dans mon registry local pour qu'ils puissent être retrouvés par Kubernetes par la suite :
    ```
    docker push localhost:5000/cache:latest
    docker push localhost:5000/dev:latest
    ```

    On applique ensuite le fichier manifest.yaml :
    ```
    kubectl apply -f manifest.yaml
    ```

    Toutes les ressources ont été bien déployées et j'ai réussi à accéder à mon application depuis localhost:80/myapp ( en utilisant postman ).