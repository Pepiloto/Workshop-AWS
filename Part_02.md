# Epitech AWS Workshop - Part 2

## The app finally

### Connect to the instance

Select the instance and click on `Connect`.

Select SSH Client.

Run the commands in your terminal.

In the terminal, run `whoami` to check if you are connected as `ec2-user`.

### Configure the instance

#### Install the dependencies

```bash
sudo yum update -y
sudo yum groupinstall -y "Development Tools"
sudo amazon-linux-extras install -y nginx1
sudo yum install -y python3 python3-devel python3-pip
sudo pip3 install pipenv wheel uwsgi
```

#### Get the source code

```bash
aws s3 cp s3://<bucket_name>/ ./app --recursive
```
Check everything is here.

Create the the directories

```bash
sudo mkdir /var/www/SampleApp
sudo cp -r ./app/* /var/www/SampleApp
```

Check everything is here.

#### Configure Nginx

```bash
cd /var/www/SampleApp
sudo usermod -a -G nginx ec2-user
sudo chown ec2-user:nginx -R ./*
sudo chown ec2-user:nginx -R /var/www
sudo chown ec2-user:nginx -R /var/www/SampleApp
```

#### Configure the server

```bash
sudo aws s3 cp s3://<bucket_name>/nginx.conf /etc/nginx/conf.d/nginx.conf
```

Check the file is here.

#### Configure uWSGI

```bash
sudo aws s3 cp s3://<bucket_name>/uwsgi.service /etc/systemd/system/mywebapp.uwsgi.service
sudo mkdir -p /var/log/uwsgi
sudo chown ec2-user:nginx -R /var/log/uwsgi
```

#### Start the services

```bash
# uWSGI
sudo systemctl enable mywebapp.uwsgi.service
sudo systemctl start mywebapp.uwsgi.service

# Nginx
sudo systemctl enable nginx
sudo systemctl restart nginx
```

Check the status of the services.