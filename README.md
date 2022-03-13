# Nginx-Config-on-mac
Config Nginx on Mac


## Homebrew 
[Homebrew](http://brew.sh/) is the missing package manager for OSX. 

Download and install using the following command:

ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
    
Check for any problems or conflicts:

brew doctor
    
Update and Upgrade `brew` formulas:

brew update && brew upgrade


## NGINX

brew install nginx
   
#### Setup auto start

Since we want to use port 80 have to start the Nginx process as root:
```
sudo cp /usr/local/opt/nginx/*.plist /Library/LaunchDaemons/
sudo chown root:wheel /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
``` 
#### Test web server

Start Nginx for the first time:
```
sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
```
The default configuration is set that it will listen on port 8080 instead of the HTTP standard 80. Ignore that for now:
```
curl -IL http://localhost:8080
```

sudo launchctl unload /Library/LaunchDaemons/homebrew.mxcl.nginx.plist

Stop Nginx again:
```
sudo launchctl unload /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
```
## Configurations

#### nginx.conf

Create some folders which we are going to use in the configuration files:

```
mkdir -p /usr/local/etc/nginx/logs
mkdir -p /usr/local/etc/nginx/sites-available
mkdir -p /usr/local/etc/nginx/sites-enabled
mkdir -p /usr/local/etc/nginx/conf.d
mkdir -p /usr/local/etc/nginx/ssl
sudo mkdir -p /var/www
 
sudo chown :staff /var/www
sudo chmod 775 /var/www
```
Remove the current default nginx.conf (also available as `/usr/local/etc/nginx/nginx.conf.default` in case you want to take a look) and download this custom one via curl from GitHub:

```
rm /usr/local/etc/nginx/nginx.conf
    
curl -L https://github.com/soumyabmr/Nginx-Config-on-mac/blob/main/nginx.conf -o /usr/local/etc/nginx/nginx.conf

```
Download the following PHP-FPM configuration from GitHub:
```
curl -L https://github.com/soumyabmr/Nginx-Config-on-mac/blob/main/php-fpm -o /usr/local/etc/nginx/conf.d/php-fpm

```
#### Create default virtual hosts
```
curl -L https://github.com/soumyabmr/Nginx-Config-on-mac/blob/main/sites-available_default -o /usr/local/etc/nginx/sites-available/default
```
#### Enable virtual hosts

Now we need to symlink the virtual hosts that we want to enable into the sites-enabled folder:

```
ln -sfv /usr/local/etc/nginx/sites-available/default /usr/local/etc/nginx/sites-enabled/default

```
#### restart Nginx
```
sudo brew services restart nginx
```
#### Final tests

Thats it, everything should be up and running. Click on the links below to ensure that:

- [http://localhost](http://localhost) → “Nginx works” page
- [http://localhost/info](http://localhost/info) → phpinfo() 

## Control your services like a boss





