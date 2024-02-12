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


1. kubernetes
2. concrete plan
3. 