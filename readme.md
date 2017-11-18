# Creating new images

# Docker file basics
1. Combine multiple RUN directives together: I'll reduce the amount of images layers created.   
2. `COPY` is preferred way to copy file to the container.   
3. `ADD` directive can download files from provided links and also unpack supported compression formats.   
4. Using `COPY` directive over the `ADD` directive is recommended.   
5. Aggressive Docker cache: place `RUN apt-get update && apt-get install ...` because in case update command is in a separate `RUN` directive, then this layer can be cached and you'll get outdated packages.   
6. When docker build container from Dockerfile it creates a new layer for every directive in intermediate container and create a new layer on top of the base image.    
7. Tagging and pushing modified images to the hub could save some time!   
