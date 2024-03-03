## Build

### run container locally:

 ```docker-compose -f ./examples/docker/docker-compose_default.yaml up -d```

### then in tools:

change registry name in the build_docker.sh file


then run ```sh build_docker.sh``` to build & push & check on docker hub https://hub.docker.com/


## Start Kubernetes & Docker on Personal Laptop & RAC


### kubernetes setup:

1. download minikube:

https://devopscube.com/minikube-mac/#:~:text=How%20to%20Setup%20Minikube%20on%20MAC%20M1%2FM2%201,get%20the%20following%20error.%20...%204%20Conclusion%20


2. minikube start / minikube stop

```
kubectl config view
minikube: minikube kubectl config view
```


```
kubectl config current-context (either docker-desktop or minikube is fine here)
minikube: minikube kubectl config current-context
```

```
kubectl create namespace teastore (we use teastore here) / kubectl delete namespace teastore
minikube: minikube kubectl -- create namespace teastore / minikube kubectl -- delete namespace teastore
```

3. for deployment & starting the pods:

```
kubectl apply -f teastore-new.yaml (I modified the file)
minikube: minikube kubectl -- apply -f teastore-new.yaml
```

4. for verification:

```
kubectl get deployments -n teastore / kubectl delete deployment teastore-db -n teastore
minikube: minikube kubectl -- get deployments -n teastore
```

```
kubectl get services -n teastore / kubectl delete service teastore-db -n teastore
minikube: minikube kubectl -- get services -n teastore
```

```
kubectl get pods -n teastore / kubectl delete pods teastore-db -n teastore
minikube: minikube kubectl -- get pods -n teastore
```

5. for scaling:

```
kubectl scale deployment teastore-db --replicas=1 -n teastore
minikube: minikube kubectl  -- scale deployment teastore-db --replicas=1 -n teastore
```

for checking ip address of pods:

```
kubectl get pods --all-namespaces
minikube: minikube kubectl  -- get pods --all-namespaces
```

```
kubectl get pod teastore-webui--${rest of id} -n teastore -o wide
```

```
get ip:
kubectl get svc -n teastore
```


## URL of UI (TO BE TESTED):

```
personal laptop: http://localhost:30090/tools.descartes.teastore.webui/
test on RAC: curl http://10.1.11.211:8080/tools.descartes.teastore.webui/
(replace host url with your own...)
```

### scale all to zero & delete resources:

```
kubectl get deployments --all-namespaces -o name | xargs -n 1 kubectl scale --replicas=0

kubectl delete deployments --all -n teastore

kubectl delete services --all -n teastore

kubectl delete pods --all -n teastore
```

## Generate Load:

In one process:
cd ./TeaStore/examples/httploadgenerator
java -jar httploadgenerator.jar loadgenerator

In the other:
cd ./TeaStore/examples/httploadgenerator
java -jar httploadgenerator.jar director -s 127.0.0.1  -a ./increasingLowIntensity.csv -l ./teastore_browse.lua -o loadGeneratingResult.csv -t 8

or

java -jar httploadgenerator.jar director -s 10.1.11.211 -a ./increasingLowIntensity.csv -l ./teastore_browse.lua -o loadGeneratingResult.csv -t 8

After running the tests, shut down loadgenerator (the first process)

## Locust Testing

**- Short Duration Tests**

- Low Intensity (10 RPS)
    ```
    locust -f locustfile.py --headless --users 10 --spawn-rate 10 --run-time 1m  -csv=low-intensity-short
    ```

- Medium Intensity (50 RPS)
    ```
    locust -f locustfile.py --headless --users 50 --spawn-rate 50 --run-time 1m  -csv=medium-intensity-short
    ```
- High Intensity (100 RPS)
    ```
    locust -f locustfile.py --headless --users 100 --spawn-rate 100 --run-time 1m  -csv=high-intensity-short
    ```
- Extreme Intensity (300 RPS)
    ``` 
    locust -f locustfile.py --headless --users 300 --spawn-rate 300 --run-time 1m  -csv=extreme-intensity-short
    ```


**- Long Duration Tests**

- Low Intensity (10 RPS)
    ```
    locust -f locustfile.py --headless --users 10 --spawn-rate 10 --run-time 5m  -csv=low-intensity-long
    ```

- Medium Intensity (50 RPS)
    ```
    locust -f locustfile.py --headless --users 50 --spawn-rate 50 --run-time 5m  -csv=medium-intensity-long
    ```
- High Intensity (100 RPS)
    ```
    locust -f locustfile.py --headless --users 100 --spawn-rate 100 --run-time 5m  -csv=high-intensity-long
    ```
- Extreme Intensity (300 RPS)
    ```
    locust -f locustfile.py --headless --users 300 --spawn-rate 300 --run-time 5m  -csv=extreme-intensity-long
    ```

**- Spike Test**

- Preparing for the Spike
    ```
    locust -f locustfile.py --headless --users 50 --spawn-rate 50 --run-time 2m -csv=pre-spike
    ```
- Executing the Spike
    ```
    locust -f locustfile.py --headless --users 1000 --spawn-rate 1000 --run-time 2m -csv=spike
    ```
- Post-Spike Observation
    ```
    locust -f locustfile.py --headless --users 50 --spawn-rate 50 --run-time 15m -csv=post-spike
    ```


## Next steps:

data

script organize data

export data as csv

jupyter notebook calc

utilization, throughput