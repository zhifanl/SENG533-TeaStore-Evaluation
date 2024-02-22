run container locally:
docker-compose -f ./examples/docker/docker-compose_default.yaml up -d

In one process:
cd ./TeaStore/examples/httploadgenerator
java -jar httploadgenerator.jar loadgenerator

In the other:
cd ./TeaStore/examples/httploadgenerator
java -jar httploadgenerator.jar director -s 127.0.0.1  -a ./increasingLowIntensity.csv -l ./teastore_browse.lua -o loadGeneratingResult.csv -t 8

After running the tests, shut down loadgenerator (the first process)


httploadgene:


locust: ui, define load 


data
script organize data

export data as csv

jupyter notebook calc

utilization, throughput, 


1. kubernetes setup:

minikube:
https://devopscube.com/minikube-mac/#:~:text=How%20to%20Setup%20Minikube%20on%20MAC%20M1%2FM2%201,get%20the%20following%20error.%20...%204%20Conclusion%20

change registry name in the build_docker.sh file

sh build_docker.sh

build & push & check on docker hub https://hub.docker.com/

minikube start / minikube stop

kubectl config view

kubectl config current-context (either docker-desktop or minikube is fine here)

kubectl create namespace teastore (we use teastore here) / kubectl delete namespace teastore

for deployment & starting the pods:

kubectl apply -f teastore-new.yaml (I modified the file)

for verification:

kubectl get deployments -n teastore / kubectl delete deployment teastore-db -n teastore

kubectl get services -n teastore / kubectl delete service teastore-db -n teastore

kubectl get pods -n teastore / kubectl delete pods teastore-db -n teastore

for scaling:

kubectl scale deployment teastore-db --replicas=1 -n teastore

for checking ip address of pods:

kubectl get pods --all-namespaces

kubectl get pod teastore-webui-767fdf848-gc9p7 -n teastore -o wide

get ip:
kubectl get svc -n teastore

url of the ui:

http://localhost:30090/tools.descartes.teastore.webui/

scale all to zero & delete resources:

kubectl get deployments --all-namespaces -o name | xargs -n 1 kubectl scale --replicas=0

kubectl delete deployments --all -n teastore

kubectl delete services --all -n teastore

kubectl delete pods --all -n teastore
