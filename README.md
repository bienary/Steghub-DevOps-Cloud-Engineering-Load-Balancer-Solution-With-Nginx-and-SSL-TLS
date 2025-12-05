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

