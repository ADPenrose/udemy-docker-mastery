# Creating Networks

docker network create --driver overlay frontend
docker network create --driver overlay backend

# Vote service

docker service create --replicas 2 --name vote -p 80:80 --network frontend bretfisher/examplevotingapp_vote

# Redis service

docker service create --name redis --network frontend redis:3.2

# Worker service

docker service create --name worker --network backend --network frontend bretfisher/examplevotingapp_worker

# Postgres service

docker service create --name db --network backend --mount type=volume,source=db-data,target=/var/lib/postgresql/data -e POSTGRES_HOST_AUTH_METHOD=trust postgres:9.4

# Result service

docker service create --name result -p 5001:80 --network backend bretfisher/examplevotingapp_result
