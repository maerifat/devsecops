# devsecops
### Step 1
```bash
sudo vi /etc/sysctl.conf
```
### Step 2 - Paste the below lines in /etc/sysctl.conf
```bash
vm.max_map_count=262144
fs.file-max=65536
```
### Step 3 - hit below command
```bash
sudo sysctl -p
```
### Step 4 - change host name
```bash
sudo hostnamectl set-hostname SonarQube
```
### Step 5 - perform system update
```bash
sudo apt-get update
```

### Step 6 - install docker compose
```bash
sudo apt-get install docker-compose -y
```
### Step 7 - Add current user to docker group
```bash
sudo usermod -aG docker $USER
```
### Step 8 - Create docker-compose.yml
```bash
sudo vi docker-compose.yml 
```
### Step 9 - Paste the below lines in docker-compose.yml
```bash
version: "3"
services:
  sonarqube:
    image: sonarqube:lts-community
    container_name: sonarqube
    restart: unless-stopped
    environment:
      - SONARQUBE_JDBC_USERNAME=sonar
      - SONARQUBE_JDBC_PASSWORD=password123
      - SONARQUBE_JDBC_URL=jdbc:postgresql://db:5432/sonarqube
    ports:
      - "9000:9000"
      - "9092:9092"
    volumes:
      - sonarqube_conf:/opt/sonarqube/conf
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_bundled-plugins:/opt/sonarqube/lib/bundled-plugins

  db:
    image: postgres:12
    container_name: db
    restart: unless-stopped
    environment:
      - POSTGRES_USER=sonar
      - POSTGRES_PASSWORD=password123
      - POSTGRES_DB=sonarqube
    volumes:
      - sonarqube_db:/var/lib/postgresql10
      - postgresql_data:/var/lib/postgresql10/data

volumes:
  postgresql_data:
  sonarqube_bundled-plugins:
  sonarqube_conf:
  sonarqube_data:
  sonarqube_db:
  sonarqube_extensions: 
```
### Step 10 - execute docker-compose file
```bash
sudo docker-compose up -d 
```

