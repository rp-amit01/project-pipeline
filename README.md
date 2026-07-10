Jenkins Configuration 

# server need with configuration 
- jenkins server 
- sonar server

1. Jenkins Server

   i. install java and maven
   
    apt update && apt install openjdk-21-jdk -y && apt install maven -y 

      or
   
    sudo apt update
    sudo apt install fontconfig openjdk-21-jre
    java -version


   ii. install jenkins

   sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt update
sudo apt install jenkins



2. Sonar Server Setup

   # Install and configure Database
   
apt install openjdk-17-jdk -y
apt install postgresql -y
systemctl start postgresql

sudo -u postgres psql
>> CREATE USER linux PASSWORD 'redhat';
>> CREATE DATABASE sonarqube;
>> GRANT ALL PRIVILEGES ON DATABASE sonarqube TO linux;
>> \c sonarqube;
>> GRANT ALL PRIVILEGES ON SCHEMA public TO linux;
>> \q

# Configure Linux Machine

sysctl -w vm.max_map_count=524288
sysctl -w fs.file-max=131072
ulimit -n 131072
ulimit -u 8192

# Install and Configure Sonarqube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-25.5.0.107428.zip
apt install unzip -y
unzip sonarqube-25.5.0.107428.zip

mv sonarqube-25.5.0.107428 /opt/sonar
cd /opt/sonar

vim conf/sonar.properties
>> sonar.jdbc.username=linux
>> sonar.jdbc.password=redhat
>> sonar.jdbc.url=jdbc:postgresql://localhost/sonarqube

useradd sonar -m
chown sonar:sonar -R /opt/sonar
su sonar

cd /opt/sonar/bin/linux-x86-64
./sonar.sh start
./sonar.sh status





