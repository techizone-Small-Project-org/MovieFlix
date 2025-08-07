# Launch Infra for Movieflix Micrroservices

```
Launch 3 "te.micro" Ec2 Instances 
```

# Install Nginx
### Using Amazon Linux 2 HERE 
Login to all 3 Ec2 Instance and Install Nginx
```
sudo yum update -y
sudo yum install git nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```
# Setup Project
## Remove Default Nginx Content
Login to all 3 Ec2 Instance and Install Nginx
```
cd /usr/share/nginx/html
sudo rm -rf *
```
## Get the Project Code

### Steps at "Homepage" Instance
```
cd /tmp
git clone https://github.com/techizone-Small-Project-org/MovieFlix.git
cd MovieFlix
sudo git checkout movieflix-micro
sudo rm -rf games/ movies/ songs/
sudo mv * /usr/share/nginx/html
```
### Steps at "Movies" Instance
```
cd /tmp
git clone https://github.com/techizone-Small-Project-org/MovieFlix.git
cd MovieFlix
git checkout movieflix-micro
cd movies
sudo mv * /usr/share/nginx/html
```
### Steps at "Songs" Instance
```
cd /tmp
git clone https://github.com/techizone-Small-Project-org/MovieFlix.git
cd MovieFlix
git checkout movieflix-micro
cd songs
sudo mv * /usr/share/nginx/html
```

## Check your App Working or Not

```
http://<HomePage-Public-IP>/
http://<Movies-Public-IP>/
http://<songs-Public-IP>/
```
# Requirment ==> Host-Based Routing
When we hit 
```
http://<Public-IP>/  ==> it will show Homepage
http://<Public-IP>/songs  ==> it will show Songs Page
http://<Public-IP>/movies  ==> it will show Movies page
http://<Public-IP>/games  ==> it will show Games page
```
# Here we use "Reverse Proxy" Concept
## To achieve these we need to configure the "nginx.conf" file

In nginx.conf file we need to mention the alias of the songs, movies, games

```
    location /movies/ {
        proxy_pass http://<movies-private-IP>:80/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /songs/ {
        proxy_pass http://<songs-private-IP>:80/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
```

We  already have "nginx.conf" file so Copy that file in the Nginx PATH "/etc/nginx/nginx.conf"

```
sudo mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
sudo vim 
sudo systemctl restart nginx
```

## Check your App Working or Not

```
http://<Public-IP>/  ==> it will show Homepage
http://<Public-IP>/songs  ==> it will show Songs Page
http://<Public-IP>/movies  ==> it will show Movies page
http://<Public-IP>/games  ==> it will show Games page)
```
