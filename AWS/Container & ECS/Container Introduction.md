#### Image Anatomy
![[Container Introduction.png]]
#### Container Anatomy
![[Container Introduction-1.png]]
A Docker *container* has an additional **read-write file system layer**. File system layers, so the layers that make up a Docker image, by default, **they’re read only**. They never change after they’re created and so this special read-write layer is added, which allows containers to run.

Anything which happens in the container, if log files are generated or if an application generates or reads data, that’s all stored in the read-write layer of the container.
#### Container Registry
![[Container Introduction-2.png]]The reuse architecture that’s offered by the way that containers do their disk images, scales really well.

The function of a container registry is almost revealed in the name. It’s a registry or a hub of container images. You can make or use a Dockerfile to create a container image and then you upload that image to a private repository or a public one, such as Docker hub.

From there, these container images can be deployed to Docker hosts which are just servers running a container engine

Docker hosts can run many containers based on one or more images and a single image can be used to generate containers on many different Docker hosts.

## Exam Powerup
- **Dockerfiles** are used to **build images**
- Portable - self-contained, always run as expected
- Lightweight - Parent OS used, **fs layers are shared**
- Container only runs the application & environment it needs
- Provides much of the isolation VM’s do
- Ports are **`exposed`** to the host and beyond…
- Application stacks can be multi-container ..

[[ECS Concept]]
