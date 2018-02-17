#nginx, php ir mysql server documentation file

before launching the server make sure you have the latest versions of Docker-CE and Docker-composer
You can find the instructions on how to install Docker-CE at https://docs.docker.com/install/

1. Download all required files from https://github.com/adeoweb/devops-workshop
2. Extract the files wherever you like
3. Download the configuration files from (mano github)
4. Put the docker-compose.yml and nginx.conf files into the devops-workshop-master folder
5. Open up a terminal windows and while you're in the devops-workshop-master folder type "sudo docker-compose up"
6. In a new terminal window type "sudo docker exec -it devopsworkshopmaster_php_1 bash -c "cd /main;composer update;composer install"" and wait until the installation has finished
7. Type "docker-compose down" to stop the server
8. Go to devops-workshop-master/config/packages folder
9. Edit the doctrine.yaml file. Change the "driver" and "server_version" lines accordingly:
    driver: 'pdo_mysql'
    server_version: '5.6.39"
10. Edit the docker-compose.yml file in the devops-workshop-master folder. Find these lines:
       - MYSQL_DATABASE=(database_name)
       - MYSQL_USER=(your_username)    
       - MYSQL_PASSWORD=(your_password)
    and edit each one of them. Enter the database name, your preferred username and password.
11. Edit the .env file in the devops-workshop-master folder. Comment(add a # before) the "DATABASE_URL=sqlite:///%kernel.project_dir%/var/data/blog.sqlite" line. Add a new line:
    DATABASE_URL=mysql://(your_username):(your_password)@mysql:3306/(database_name)
    replace "(your_username)", "(your_password)" and "(database_name)' with the info that you have entered in the docker-compose.yml file.
12. Change the permissions of ever file in var folder located in the devops-workshop-master catalog with "chmod -R 777 var/" command
13. Launch the server from the devops-workshop-master folder(docker-compose up)
14. In another terminal window type the following command:
    sudo docker exec -it devopsworkshopmaster_php_1 bash -c "cd /main;bin/console doctrine:database:create;bin/console doctrine:schema:update --force;bin/console doctrine:fixtures:load"
    Once the "Careful, database will be purged. Do you want to continue y/N ?" prompt appears type "y" and press enter.
15. Go to your browser and type in localhost:8080 to access the website.
