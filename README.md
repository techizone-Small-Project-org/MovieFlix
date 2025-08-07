# Install Nginx
### Using Amazon Linux 2 HERE 

```
sudo yum update -y
sudo yum install git nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
```
# Setup Project

## Remove Default Nginx Content
```
cd /usr/share/nginx/html
sudo rm -rf *
```
## Get the Project Code
```
cd /tmp
git clone https://github.com/techizone-Small-Project-org/MovieFlix.git
cd movieflix
git checkout movieflix-mono
sudo mv * /usr/share/nginx/html
```

## Check your App Working or Not

```
http://<Public-IP>/
http://<Public-IP>/songs/index.html
http://<Public-IP>/movies/index.html
http://<Public-IP>/games/index.html
```
# Requirment ==> PATH-Based Routing

When we hit 
```
http://<Public-IP>/  ==> it will show Homepage
http://<Public-IP>/songs  ==> it will show Songs Page
http://<Public-IP>/movies  ==> it will show Movies page
http://<Public-IP>/games  ==> it will show Games page
```
# To achieve these we need to configure the "nginx.conf" file

In nginx.conf file we need to mention the alias of the songs, movies, games

```
        location /movies {
            alias /usr/share/nginx/html/movies/;
            index index.html;
        }

        location /songs {
            alias /usr/share/nginx/html/songs/;
            index index.html;
        }

        location /games {
            alias /usr/share/nginx/html/games/;
            index index.html;
        }
```

We  already have "nginx.conf" file so Copy that file in the Nginx PATH "/etc/nginx/nginx.conf"

```
sudo mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bak
sudo cp /usr/share/nginx/html/nginx.conf /etc/nginx/nginx.conf
sudo systemctl restart nginx
```

## Check your App Working or Not

```
http://<Public-IP>/  ==> it will show Homepage
http://<Public-IP>/songs  ==> it will show Songs Page
http://<Public-IP>/movies  ==> it will show Movies page
http://<Public-IP>/games  ==> it will show Games page)
```
