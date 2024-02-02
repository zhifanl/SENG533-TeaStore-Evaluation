run container locally:
docker-compose -f ./examples/docker/docker-compose_default.yaml up -d

In one process:
cd ./TeaStore/examples/httploadgenerator
java -jar httploadgenerator.jar loadgenerator

In the other:
cd ./TeaStore/examples/httploadgenerator
java -jar httploadgenerator.jar director -s 127.0.0.1  -a ./increasingLowIntensity.csv -l ./teastore_browse.lua -o loadGeneratingResult.csv -t 8