
Introduction
This app sets up sharding with two MariaDB servers using MaxScale and Docker Compose. The master1 database is populated with shard1.sql, and the master2 database is populated with 



 Build the containers:






Run the 3 Docker containers:





Three services are launched: one containing the 
 MariaDB database, another containing the            
MariaDB database, and the third containing a MaxScale instance.
The MaxScale database username is           , and the password is
You can access the sharded database via MaxScale as follows:


Use the password when prompted.



To access the          database as                    , use the password               


To access the sharded database via MaxScale, use the password          and username :


To access the instances, you can run the following commands:





 
Install the necessary libraries using pip: 
To run the script, ensure that the Docker Compose is running:
  
 
