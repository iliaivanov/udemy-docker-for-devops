# Docker file basics
1. Combine multiple RUN instructions together: I'll reduce the amount of images layers created.   
2. `COPY` is preferred way to copy file to the container.   
3. `ADD` instruction can download files from provided links and also unpack supported compression formats.   
4. Using `COPY` instruction over the `ADD` instruction is recommended.   
5. Aggressive Docker cache: place `RUN apt-get update && apt-get install ...` because in case update command is in a separate `RUN` instruction, then this layer can be cached and you'll get outdated packages.   
6. When docker build container from Dockerfile it creates a new layer for every instruction in intermediate container and create a new layer on top of the base image.    
7. Tagging and pushing modified images to the hub could save some time!   
8. `USER` instruction should be added to every docker file. Container should run with non-privileged user (nobody).   

# Docker containers links
`docker run -d -p 5000:5000 --link %link_container_name% %container_name%:%tag%` - run container from builded image and link some other container to it by passing it name to the `--link` option.   

*Starting from docker compose version 2 it supports docker network feature which allows containers being discovered by it's name automatically. Linking is not required anymore.*   

# Docker compose
`docker-compose logs -f` show and follow all containers logs.   
`docker-compose logs -f %container_name%` show and follow specific container logs.   
`docker-compose ps` - check the status of the containers managed by docker compose.   

# Docker networking
`docker network ls` - shows list of docker networks.   
`docker network inspect %network_id%` - inspect network.   
`docker network connect %network_id% %container_id%` - connect container to the network.      
Network type:   
- _none_ - doesn't have an access to the outside world, isolated.   
- _bridge_ - containers can access each other within bridge network. Containers can access outside worlds through the bridge.   
- _host_ - containers are located in the host network.   
- _overlay_ - support multi-host! _none_, _bridge_ and _host_ networkds could be created only on the sigle host. Running docker engine in the swarm mode or using key-value store like consul.   

# Commands
`docker inspect %container/image id%` - inspect container or image.   
`docker commit %container_id% %repository%:%tag%` - by committing make a new image with changes done on top of the base image.   
`docker push %repositorys%:%tag%` - push container to the cloud service (docker hub).   
`docker build -t %container_name%:%tag% %context|path%` - build container from in the context (context means path to the directory where things should happen and where Dockerfile is).   
`docker logs %container_id%` - show running in background container logs.   
`docker exec -it %container_id% %command%` - execute commnd inside a container.    

# Continuos Integration
- With Circke CI you need to create `.circleci` directory and place a .yml configuration file there.   
- Every config file should have `build` job.   

# Docker-machine
Docker machine helps to provision VMs where containers could be run. Running containers inside VMs gives better kernel and resouces separation. AWS doing this  stuff.   
`docker-machine create --driver digitalocean --digitalocean-access-token f00bdaf3160983727dd444a34a38818ecbb8c7f381878632f60833bf794a1540 docker-app-machine` - provosion VM. VMs can be provisioned to the different platforms including AWS, Google Cloud, Azure.   
`eval $(docker-machine env docker-app-machine)` - setup current docker environment to the create VM in digital ocean.   
`docker-compose -f docker-compose.prod.yml up -d` - gereric command, but in case of remote VM deployed to the cloud current command will bring containers up there.   
`docker info` - current environment information.   
https://docs.docker.com/machine/drivers/digital-ocean/ - digital ocean possible configs.   

# Docker swarm
- Created VMs in digital ocean has a two IPs: one is publically accesable and another is private and can be accessed by hosts in the same datacenter.   

`docker swarm init` - init swarm mode. The docker engine targeted by this command becomes a manager in the newly created single-node swarm. Swarm managers can be a workers at the same time too but it's also possible to set them do manager job only.          
`docker swarm join --token SWMTKN-1-21z6ye90esgx10o0kohnbm3379iyx7w75jiqs4oue09902xzw4-0o9dltev9jfib8kvtldozr3bm 104.236.10.31:2377` - join worker node to the swarm. This command should be run under worker node, so you can ssh into node (`docker-machne ssh swarm-node`) and tun command inside of it.    
`swarm leave` - leave the swarm.   

## Docker stack
- `docker stack` is a `docker-compose` in the swarm mode.   

`docker stack deploy --compose-file docker-compose.prod.yml dockerapp_stack` - create a services stack from the docker-composer file. This command will update all services in the cluster automatically.   
`docker stack ls` - list all created docker stacks.  
`docker stack services dockerapp_stack` - give an overview of the services in stack.    
`docker stack rm dockerapp_stack` - remvoe stack.   