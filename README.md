Docker task
Write a simple airflow dag to connect with db(postgres) and add entry in db for each execution (Time of dag execution)
Add the given dag into the container and install dependencies.
Use docker compose to launch airflow and postgres
Schedule the dag
Validate entry in postgres



Kubernetes Task
Create deployment and service for above airflow and postgres (you can use postgres helm chart for postgres deployment)
Deploy airflow and postgres
Schedule the dag
Validate entry in postgres


# docker-kubernetes-tasks

# Instructions for kubernetes task

1. Start minikube
```bash
minikube start
```
2. cd into kubernetes files 
`cd docker-kubernetes-tasks/kubernetes-files` and mount the volume of dags folder from host machine to minikube and airflow will take it from minikube.
```bash
minikube mount ./dags/:/mnt/airflow/dags
```
3. 
Script container 2 deployments files and 2 services for airflow and postgres service.
```bash
chmod +x ./script-create.sh
./script-create.sh
```
4. 
Pods will now be created and be accessible by following commands
```bash
kubectl get pods
```
5.
Enter into bash mode of airflow container. Write airflow container name in the place of pod.
```bash
kubectl exec -it <pod> -- bash
``` 
6. 
Fernet key error will be thrown if following command is not used. This is to export Fernet key variable to env variable.
```bash
FERNET_KEY=$(python -c "from cryptography.fernet import Fernet; FERNET_KEY = Fernet.generate_key().decode(); print(FERNET_KEY)")

export FERNET_KEY=$FERNET_KEY
```
7. 
Initiate the database of airflow in postgres.
```bash
airflow initdb
```
8.
Port forward to using kubectl to listen 8080 on minikube on your host machine.
```bash
kubectl port-forward svc/<service>  8080:8080
```

