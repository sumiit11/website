ansible installation: https://docs.ansible.com/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu
Jenkins installation: https://www.jenkins.io/doc/book/installing/linux/#debianubuntu

----------------------------------------------------------------------------------------------
(install.yaml)
---
 - hosts: localhost
   become: true
   name: install jenkins, java and docker
   tasks:
    - name: master task
      script: jenkins.sh
 - hosts: test
   become: true
   name: install java and docker
   tasks:
    - name: test task
      script: docker.sh
 - hosts: prod
   become: true
   name: install java and docker
   tasks:
    - name: prod task
      script: docker.sh
----------------------------------------------------------------------------
(jenkins.sh)

curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install openjdk-11-jdk -y
sudo apt-get install docker.io -y
sudo apt-get install Jenkins -y 
----------------------------------------------------------------------------------
(docker.sh)

sudo apt-get update
sudo apt-get install openjdk-11-jdk -y
sudo apt-get install docker.io -y
-------------------------------------------------------------------------
(Dockerfile)
FROM ubuntu
RUN apt update
RUN apt install apache2 -y
ADD . /var/www/html
ENTRYPOINT apachectl -D FOREGROUND
