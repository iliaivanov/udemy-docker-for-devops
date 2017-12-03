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
