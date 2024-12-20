# real-time-data-processing-with-docker

Setup Docker Compose

1. Download Docker Compose binary:

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

2. Make Docker Compose executable:

sudo chmod +x /usr/local/bin/docker-compose

3. Verify Docker Compose installation:

docker-compose --version

4. Complete Docker Compose Setup
Create the project directory real-time-data-pipeline and add the following in this directory:
- docker-compose.yml 
- producer folder
- consumer folder


Install PostgreSQL 13

5. Add PostgreSQL Repository

sudo amazon-linux-extras install -y postgresql13

6. Install PostgreSQL:

sudo yum install -y postgresql13 postgresql13-server

7. Verify PostgreSQL Installation:

psql --version


Create Docker group and add user

8. Create docker group

sudo groupadd docker

9. Add user to the docker group

sudo usermod -aG docker $USER

10. Apply the changes without logging out

newgrp docker

11. Install and start Docker

sudo yum update -y
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker

12. Run the Application:

docker-compose up -d


Access PostgreSQL Container

13. List all the running containers

docker ps

14. Execute the below command to Enter PostgreSQL Container, replace the <container_id> with the container_id of postgresql:

docker exec -it <container_id> /bin/bash

15. Access PostgreSQL CLI:

psql -U user -d my_database

16. Create a Table inside the Database:

CREATE TABLE sensor_data (
    id SERIAL PRIMARY KEY,
    timestamp TIMESTAMPTZ NOT NULL,
    temperature FLOAT,
    humidity FLOAT
);


17. Insert Sample Data to the table:

INSERT INTO sensor_data (timestamp, temperature, humidity) VALUES
(NOW(), 22.5, 60.0),
(NOW() - INTERVAL '1 hour', 21.0, 65.0);


Configure Grafana to connect to this database

18. Navigate to Grafana Dashboard in your browser, replace <EC2_PUBLIC_IP> with Public IPv4 Address:

http://<EC2_PUBLIC_IP>:3000

19. Login to Grafana using default credentials username and password as admin.

20. Add a new PostgreSQL connection

21. Add a new data source in it with following:

HOST URL: db:5432
DATABASE NAME: my_database
USERNAME: user
PASSWORD: password
*Disable TLS/SSL Mode*
VERSION: 13

Create Grafana Dashboard

22. Select New Dashboard and Add visualization

23. Select Data source as PostgreSQL and add the following code in it: 

SELECT
    timestamp AS "time",
    temperature,
    humidity
FROM
    sensor_data
ORDER BY
    timestamp DESC
LIMIT 10;

24. Save the Dashboard. 
