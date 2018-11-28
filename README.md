# Toolbox


 
## Development Environments

### 1. **ranger-env**
Development environment with all installed pre-reqs for apache Ranger (Incubator)

#### Usage
```
docker build -t rangerdev ./ranger-dev/
docker run -itd --name=rdev rangderdev
docker exec -it rdev bash

```
To start the needed plugins, extract them as explained in the ranger repository
readme and install them following these instructions:
`https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.6.5/bk_command-line-installation/content/ch14s05s03s02.html`


### 2. **ambari-dev**
Dev environment for quick ambari builds

*UPDATE*: the following instructions run an old version of the container. Build it
from source following the `ranger-dev` instructions
```
# Start regular container with local amabri source 
docker run -itd --name=ibmdev -v <your_ambari_source_dir>:/tmp/ambari chuyqa/ambari-dev

# Start container with custom .m2 repo and local ambari source
docker run -itd --name=ambdev -v <your_ambari_source_dir>:/tmp/ambari <your_m2_home>:/root/.m2 chuyqa/ambari-dev


# Build ambari
docker exec -it ambdev bash /tmp/build.sh

```
