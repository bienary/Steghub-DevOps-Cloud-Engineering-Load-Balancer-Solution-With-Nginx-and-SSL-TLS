# Load Balancer Solution With Nginx and SSL/TLS

This project focuses on deploying a scalable and secure web architecture by implementing Nginx as a reverse proxy and load balancer, combined with end-to-end HTTPS encryption using Let’s Encrypt SSL/TLS certificates. The solution demonstrates essential DevOps capabilities including traffic distribution, high availability design, certificate automation with Certbot, and secure communication enforcement.

By completing this project, engineers gain real-world experience configuring Nginx load-balancing mechanisms, establishing trusted SSL termination, managing DNS and domain validation, and ensuring compliant, encrypted data flow across all layers of a web system.

## Task
This project consists of two parts:

> Configure Nginx as a Load Balancer

> Register a new domain name and configure secured connection using SSL/TLS certificates

<img width="888" height="519" alt="image" src="https://github.com/user-attachments/assets/9f973ff1-4b47-473b-8a9b-178a8d9075e4" />


# Part 1: Configuring Nginx as a Load Balancer

## Create an EC2 Virtual Machine: Nginx LB

### Follow the steps below to launch an AWS EC2 instance that will serve as an Nginx load balancer.

1. Launch an EC2 Instance

> Ubuntu Server 24.04 LTS

> Instance Name: Nginx LB

2. Configure Security Group

### Ensure the instance is accessible for both HTTP and HTTPS traffic by opening the necessary ports:

Port	Protocol	Description
80	TCP	HTTP (web traffic)
443	TCP	HTTPS (secure web traffic)

<img width="1147" height="310" alt="image" src="https://github.com/user-attachments/assets/9396d2a1-d74d-436c-af7a-c47df7e734d6" />


<img width="1160" height="410" alt="image" src="https://github.com/user-attachments/assets/d2ba4806-bc73-4bbd-9ea0-a4d1bf40dd38" />

## Configure Local DNS

- Access the instace via SSH
- Update the /etc/hosts file with web servers' names and their local IP addresses:
```
sudo vi /etc/hosts
```

<img width="1321" height="281" alt="image" src="https://github.com/user-attachments/assets/04cfa069-d472-4a6e-aede-b29c25af523b" />

## Install and Configure Nginx

- Update the instance and Install Nginx Install Nginx

```
sudo apt update && sudo apt upgrade -y
```

```
sudo apt install nginx
```

<img width="1315" height="279" alt="image" src="https://github.com/user-attachments/assets/304db253-8c0c-44d2-9cf1-ed258a914fa2" />

<img width="1323" height="733" alt="image" src="https://github.com/user-attachments/assets/c61af1f2-ba0d-4540-99fb-150b6eb13e44" />

<img width="1324" height="734" alt="image" src="https://github.com/user-attachments/assets/b7afd916-008a-45a8-af93-77454f2a70c3" />


## Configure Nginx as a Load Balancer

- Open the Nginx configuration file:

```
sudo vi /etc/nginx/nginx.conf
```


- Add the following configuration in the http section:

```
upstream myproject {
   server Web1 weight=5;
   server Web2 weight=5;
}

server {
    listen 80;
    server_name www.domain.com;

    location / {
        proxy_pass http://myproject;
    }
}
```

<img width="1324" height="734" alt="image" src="https://github.com/user-attachments/assets/608c9c35-8434-47b8-af30-5c0257cfdf3e" />


- Restart Nginx:

```
sudo systemctl restart nginx
sudo systemctl status nginx
```
<img width="1325" height="426" alt="image" src="https://github.com/user-attachments/assets/83811890-6da0-4e75-bd87-c04d7deaa582" />

## Testing the Connection:

<img width="1325" height="426" alt="image" src="https://github.com/user-attachments/assets/e1a7ac5e-7f19-4c3f-92de-a895b15fe875" />

<img width="1325" height="426" alt="image" src="https://github.com/user-attachments/assets/42d02fe3-3fc6-4e85-b19a-2145811812bf" />


# Part 2: Registering a Domain and Configuring SSL/TLS

- In order to get a valid SSL certificate - you need to register a new domain name, you can do it using any Domain name registrar (e.g., Godaddy, Domain.com, Bluehost) and register a new domain name. For this project.

- Assign an Elastic IP to your Nginx LB server and associate your domain name with this Elastic IP.

<img width="1319" height="635" alt="image" src="https://github.com/user-attachments/assets/3d673304-bb70-4345-9d22-4d98427abb0c" />

<img width="1160" height="358" alt="image" src="https://github.com/user-attachments/assets/e3985994-b9c3-427e-983e-897217f038ec" />

- Update A record in registrar to point to Nginx LB using Elastic IP address.

- Adding records inside the Hosted Zone.

### Configure Nginx to recognize your new domain name.

- Update nginx.conf with your domain name:

```
sudo vi /etc/nginx/nginx.conf
```

> Replace server_name www.domain.com with your actual domain.

> Confirm the configuration insdie the nginx.conf is Syntax ok.

```
sudo nginx -t
```

- Reload Nginx:

```
sudo systemctl reload nginx
```

### Launch the domain name on web-browser;

<img width="1319" height="635" alt="Screenshot From 2025-12-13 01-11-43" src="https://github.com/user-attachments/assets/19668759-4bed-438c-810c-27b8dd4b7c02" />

### Install Certbot and Obtain SSL/TLS Certificate

- Ensure snapd is active

```
sudo systemctl status snapd
```

- Install Certbot:

```
sudo snap install --classic certbot
```

- Create a symlink for Certbot:

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```
- Obtain the SSL/TLS certificate:

> Follow the prompt to configure and request for ssl certificate.

```
sudo certbot --nginx
```

<img width="1323" height="655" alt="image" src="https://github.com/user-attachments/assets/ee95df18-8950-426b-9bf8-d10fd80fd6eb" />

<img width="1323" height="655" alt="image" src="https://github.com/user-attachments/assets/af156a18-0eff-4fef-af7f-9f1a486dcf6a" />

<img width="1317" height="638" alt="image" src="https://github.com/user-attachments/assets/471986bc-7679-46af-b6e4-b019e75d6ffb" />


### Set Up Automatic Certificate Renewal

- Test the renewal process:

```
sudo certbot renew --dry-run
```

- Configure a cronjob for automatic renewal:

```
crontab -e
```

- Add the following line:

```
* */12 * * *   root /usr/bin/certbot renew > /dev/null 2>&1
```

<img width="1240" height="465" alt="image" src="https://github.com/user-attachments/assets/d9138799-af82-43be-a52f-ae3a5ec77a6d" />


<img width="1305" height="638" alt="image" src="https://github.com/user-attachments/assets/39d0c63e-24cf-4380-9149-491a05e876c4" />


