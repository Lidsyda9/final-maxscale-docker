# Database Shard Github
______________________________________________________

# Introduction
This app sets up sharding with two MariaDB servers using MaxScale and Docker Compose. The master1 database is populated with shard1.sql, and the master2 database is populated with shard2.sql. Additionally, a Python script is provided which connects, queries, and demonstrates the merged database.

## Building
* Build the containers:

sudo docker-compose build


## Running
Run the 3 Docker containers:

> sudo docker-compose up -d
 
## Configuration
* Three services are launched: one containing the **master1** MariaDB database, another containing the **master2** MariaDB database, and the third containing a MaxScale instance.
The MaxScale database username is **maxscale**, and the password is **shard**.
You can access the sharded database via MaxScale as follows:

> mysql -h localhost -P 4000 -u maxscale -p
 
Use the password ***shard*** when prompted.

## Max scale Docker-Compose Setup
To access the ***master1*** database as ***root***, use password ***root***:

> mysql -u root -h localhost -P 3307 -p

 
To access the #master2# database as #root#, use password #root#:

> mysql -u root -h localhost -P 3308 -p

> To access the sharded database via MaxScale, use password shard and username maxscale:

> mysql -h localhost -P 4000 -u maxscale -p

 
 To access the instances, you can run the following commands:

sudo docker exec -it maxscale-docker_master1_1 bash
sudo docker exec -it maxscale-docker_master2_1 bash
sudo docker exec -it maxscale-docker_maxscale_1 bash
 
## Script

> pip install -r requirements.txt

 
 To run the script, ensure that the Docker Compose is running:

> python3 script.py
 
## To Delete the docker containers, and volumes created


> docker-compose down --volumes --remove-orphans
