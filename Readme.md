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

⚙️ MaxScale Configuration – example.cnf
This project uses a custom MaxScale configuration file located at:

```
maxscale/maxscale.cnf.d/example.cnf
```
✅ Important: This file is used instead of the default maxscale.cnf in the root directory.

🧩 Configuration Details
The example.cnf file is configured for a sharding setup, replacing the default master-slave architecture. It defines two separate master shards and connects MaxScale to each using a dedicated monitor, service, and listener:

```
### 🧩 Shards Setup

| Shard   | Server Name | Listener Port |
|---------|-------------|----------------|
| Shard 1 | master1     | 4006           |
| Shard 2 | master2     | 4007           |

```

✅ Key Sections in example.cnf



# Final-docker-compose-YML on Lubuntu
Objective
The purpose of this repo is to test out Docker on Lubuntu. This project demonstrates a docker environment with:
*  **Maxscale** instance-a database load balancer (`maxscale`)
* **Master Databases** (MariaDB/MySQL), each initialized with a specific table.
  This setup is ideal for learning, testing, or developing database routing, high availability, or sharding solutions.

Note, I followed [https://docs.docker.com/compose/intro/features-uses/) in building the following demonstration.

## ⚙️Setup


* I created **two database** (`master1`, `master2`) in my docker-compose.YML folder
* I created **one Maxscale** that routes connections to these databases in my docker-compose.YML folder
* Simple monitoring user and MaxScale configuration
* Ideal for learning replication, sharding, and load balancing with MaxScale

## 📦 Technologies used

- [MariaDB 11.4](https://mariadb.com/docs/maxscale/other-maxscale-versions/mariadb-maxscale-25/maxscale-25-tutorials/mariadb-maxscale-25-simple-sharding-with-two-servers)
- [MariaDB MaxScale](https://mariadb.com/products/technology/maxscale/)
- Docker & Docker Compose

```
  ## 📁 Project Structure

Final-docker-compose-YML/
├── docker-compose.yml # Docker services for MariaDB and MaxScale
├── maxscale.cnf # MaxScale routing and monitoring config
└── README.md # Project overview and setup instructions
```

## 🚀 Setup Instructions

### 1. Clone this Repository

```bash
git clone https://github.com/My_USERNAME/Final-docker-compose-YML.git
cd Final-docker-compose-YML
```
## Start the Containers

```
bash
docker compose up -d
```

This brings up:

`master1` and `master2` MariaDB instances with `root` password `root123`

A MaxScale instance exposing ports:

`4000`: SQL client access

`8989`: MaxScale admin interface
🔐 Step 3: Create MaxScale Monitoring User in Both Masters
On master1:

`bash
docker exec -it master1 mysql -uroot -proot123`

> sql

CREATE USER 'maxscale'@'%' IDENTIFIED BY 'maxscale123';
GRANT ALL PRIVILEGES ON *.* TO 'maxscale'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;

On master2:

`bash
docker exec -it master2 mysql -uroot -proot123`

Repeat the same SQL commands.
⚙️ Step 4: MaxScale Configuration
I Make sure that I have a file named maxscale.cnf in my root project directory. 

✅ Status & Testing
Check all containers are running:

`docker ps`

so no can test MaxScale routing by connecting to port 4000:

bash
Copy

`mysql -h 127.0.0.1 -P 4000 -umaxscale -pmaxscale123`

📚 Resources
MariaDB MaxScale Docs

MariaDB Docker Hub

🧑‍💻 Author
Created by Lidsyda
GitHub: github.com/Lidsyda9



