## Deploy MERN App To K8s (Minikube)

The [kubernetes](kubernetes) folder consists of all the config files needed to deploy a MERN app on a simple K8s cluster like Minikube (1 node).

- Source code for the frontend app (react app) could be found [here](https://github.com/Ananya2001-an/note).

- Source code for the server (express app) could be found [here](https://github.com/Ananya2001-an/note-server).


### Steps

1. Clone repo on your local machine. Change directory to `kubernetes`.
2. Make sure you have Minikube installed and running properly.
3. You can use a Kubernetes IDE like "Lens" to connect to your Minikube cluster. Makes it easier for monitoring changes.
4. Now first create the secret. (Holds mongodb admin username and password)
    ```bash
    kubectl apply -f secrets/mongodb-secret.yml
    ```
5. Later create the mongodb stateful set. It will create 2 replicas.
    ```bash
    kubectl apply -f stateful-sets/mongodb-stateful-set.yml 
    ```
6. Now create the mongodb internal service so that our server can later access the mongodb pods using it.
    ```bash
    kubectl apply -f services/mongodb-service.yml
    ```
7. Now deploy the server app by running:
    ```bash
    kubectl apply -f deployments/note-server-depl.yml
    ```
    If everything is working properly you would able to see the message **connected to DB** in the server pod logs.

8. Now create the server internal service for communication between the frontend and the backend.
    ```bash
    kubectl apply -f services/note-server-service.yml
    ```        
9. Deploy frontend:
    ```bash
    kubectl apply -f deployments/note-depl.yml
    ```
10. Create external service for the frontend app (Load Balancer) by running:
    ```bash
    kubectl apply -f services/note-service.yml
    ```
11. To get an external IP assigned to the frontend deployment you can run:
    ```bash
    minikube service note-service
    ```
12. Open the IP adddress in browser and you will be able to see the app running!
13. If you want to run a database GUI like `mongo-express`, you can simply run:
    ```bash
    kubectl apply -f deployments/mongo-express-depl.yml
    ```
14. Later create mongo-express external service for acccessing the GUI in browser:
    ```bash
    kubectl apply -f services/mongo-express-service.yml
    minikube service mongo-express-service
    ```

### Resources needed
- Docker
- Minikube
- Kubectl
- Lens: K8s IDE (Recommended)