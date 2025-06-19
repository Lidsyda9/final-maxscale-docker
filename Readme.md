![logo](https://mariadb.com/wp-content/uploads/2019/11/mariadb-logo_blue-transparent.png)





# Database Shard Github
______________________________________________________

# Introduction
This app sets up sharding with two MariaDB servers using MaxScale and Docker Compose. The master1 database is populated with shard1.sql, and the master2 database is populated with shard2.sql. Additionally, a Python script is provided which connects, queries, and demonstrates the merged database.

## Building
* Build the containers:


`sudo docker-compose build`




## Running
Run the 3 Docker containers:
```
git clone https://github.com/Lidsyda9/final-maxscale-docker.git
cd final-maxscale-docker
docker-compose up -d --build
```
to stop the containers:

`docker-compose down`

 
## Configuration
The [default configuration](maxscale/maxscale.cnf) for the container is minimal
and only enables the REST API.

`example.cnf` (MaxScale config)
Located in `maxscale/maxscale.cnf.d/example.cnf`, it defines:

Two monitors for `master1` and `master2`

Two services and listeners (on ports `4006` and `4007`)

Mapped servers with internal Docker service names:

* `master1` → `shard1-node1`

* `master2` → `shard2-node1`

```
### 🔌 Ports

| Container | Internal Port | Host Port |
|-----------|----------------|-----------|
| master1   | 3306           | 3307      |
| master2   | 3306           | 3308      |
| maxscale  | 4006           | 4006      |
| maxscale  | 4007           | 4007      |

```


## Max scale Docker-Compose Setup
To access the ***master1*** database as ***root***, use password ***root***:

`mysql -u root -h localhost -P 3307 -p`

 
To access the **master2** database as **root**, use password **root**:

`mysql -u root -h localhost -P 3308 -p`

To access the sharded database via MaxScale, use password **shard** and username **maxscale**:

`mysql -h localhost -P 4000 -u maxscale -p`

 
 To access the instances, you can run the following commands:

`sudo docker exec -it maxscale-docker_master1_1 bash`
```
sudo docker exec -it maxscale-docker_master2_1 bash
```
`sudo docker exec -it maxscale-docker_maxscale_1 bash`
 
## Script

`pip install -r requirements.txt`

 
 To run the script, ensure that the Docker Compose is running:

`python3 script.py`
 
## To Delete the docker containers, and volumes created


```docker-compose down --volumes --remove-orphans```
