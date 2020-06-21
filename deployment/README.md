# Deploy your Application on DigitalOcean

This guide is meant for students who are interested in deploying their applications to the cloud.

**DISLAIMER:** This guide assumes that you are familiar with basic unix commands, Vim and SSH/SCP. Otherwise, here are some useful tutorials to get started:

- [Beginner's Guide to the Bash terminal](https://www.youtube.com/watch?v=oxuRxtrO2Ag)
- [Vim Text Editor for Beginners](https://www.youtube.com/watch?v=ohk3cpBqY60&list=PLT98CRl2KxKHy4A5N70jMRYAROzzC2a6x)
- [SSH Crash Course | With Some DevOps](https://www.youtube.com/watch?v=hQWRp-FdTpc)
> ^ This tutorial also goes through the setup for DigitalOcean droplets.

## Lesson Plan

- [Generate SSH key pair](#generate-ssh-key-pair)
- [Create a DigitalOcean droplet](#create-a-digitalocean-droplet)
- [Access the droplet and create a new user](#access-the-droplet-and-create-a-new-user)
- [Install React and Rails](#install-react-and-rails)
- [Web Servers with Nginx](#web-servers-with-nginx)
- [DNS with Namecheap](#dns-with-namecheap)
- [HTTPS with Certbot](#https-with-certbot)
- [Remote Storage with AWS S3 Buckets](#remote-storage-with-aws-s3-buckets)
- [Backups with Cron jobs](#backups-with-cron-jobs)
- [Systemd services](#systemd-services)
- [Docker](#containerization-with-docker)
- [Docker-compose](#container-orchestration-with-docker-compose)

## Generate SSH key pair

> If you already have an existing SSH key pair before, feel free to skip this section.

**Secure Shell (SSH)** is by far the most common way for developers to access remote servers. As these servers may be publicly accessible on the internet (e.g. DigitalOcean droplets), there has to be some form of authentication mechanism to prevent malicious parties from accessing these servers. 

Sure, the solution is easy - we can use passwords, right? The thing is, passwords are cumbersome and nobody likes them. Imagine having to type in your password everytime you access a website. Fortunately, Computer Scientists invented something called **Public Key Authentication**, which allows a user to access remote servers without having to key in a password every time. In this section, we will be setting up an SSH key pair using **RSA cryptography**.

> We won't be going into the specifics of how Public Key Authentication works. If you are interested in how it works, you can watch [this video](https://www.youtube.com/watch?v=AQDCe585Lnc).

1. Open a new terminal
2. Run the following command. This will generate a new SSH key-pair of length 4096 bits using [RSA cryptography](https://en.wikipedia.org/wiki/RSA_(cryptosystem)). 
```bash
ssh-keygen -t rsa -b 4096
```

3. You will be prompted for a few things
- **File to save the key:**
By default this will be in `/home/<username>/.ssh/id_rsa`. Press Enter to continue.
- **Passphrase:** This is the "password" needed to access the file. You can choose not to set one but it is not recommended.

4. Your public key pair is created! Check that the following files exist by running the commands:

  **Private key:**
  > Keep this file safe and  don't share it with anyone!
  ```bash
  cat ~/.ssh/id_rsa
  ```
  The output should look something like this:
  ```
  -----BEGIN RSA PRIVATE KEY-----
  MIIEowIBAAKCAQEAl3GRfqmw36t9ltk5muqFdGm+OQdLwcp+qtx1dfUDMQ0B1oBW
  YgNUvvAnVkfei3nJnKN7GGdUx1TXHWPmAJsU915e4MiK3o1topEqLu/wfl0kg0bY
  BLHNkGTZylHCGXZt2NqPkHfArYvjcdu7q6flDto1QJLxiKQ0Gh3LDFGnLLbraONH
  Sy34zmOmmdyimlmRowi/aAT3NEnB+IeZgZVy4tf8z8Ra1qZkQu9gStxBSqz6/9ct
  uJySvyhjCpfBVrofqk5XFj3dmbQU7UbLs3TghhN9qfHyMwYwpuflOReXHiZq8t9z
  VGDAxYF2Ho7p1HJysTo80wAsN6jdKSFNNwlDlwIDAQABAoIBAA4k/ErRPITXdoZl
  SX0PlGFYEv0ukkPKTuRUbOAUfzTQmzBWkjrbRsoCkhn74mwydsMbfb68v+1SHjlP
  gEbkNSQZe1ERSe8ZVHkPh9oUbOjQeD2Om1Rs6t8mnDTKSA+qwP21BB2hIazT2O1k
  cXXJ25n0hW6/irGRbJBX4gQHiE6jrjbemL0KGbthbctZGfLBvrTWRpnlIdYJ/45W
  ejeaUTwtOjiiLVnnMCsNQ7v/B9dgyV/7zy6ko4UoLtUDD9UpYtuE4aXUXwV7FUrS
  6hNXiD3ZDx4NV2eZsCxrL7OExkw01rgNP9W+FfKxjVcZ+cjL3noIMkCQPFnyU4zp
  +FdpnZkCgYEAxobCsVPXA7llAj3TxxMVjR2GXJn8F7by0W6E2+wpzgDfi5Z/nFo6
  AFhHWg4YCrvz8xbh7zHPXP738CNkxlocmKWwkxOdmq3eFHtIqUvlNyNxcO0Qpq2q
  IEdLknvgHx+rKxMAxf2oJeO1msEGb4KRK3xy7i5ONBbTQBz2gIgvT3MCgYEAw0ll
  kZcHCu9497j4XeW3Izy81iVqH44+PS/hV6g39RQ5AC5rbmQks8n5a9tetwV5eExy
  n5RRaAfi/h/fwjkqjfbzdFld2jO5x9INUgfNaQDhKZdYbicreY3vi+HKxx/Zxewq
  zNT9H9joq/PKcBUqyr6O1X3SLTxzhGdR73bEqk0CgYAJH8Bq7fN/1FF0HOtSxunC
  poy6TMltPZdDUNUCVoRFV3zuqWgMA4mO4n/E/8jTFXhMv8x6dcuV9pHmk3naM+IE
  kfjfiZNAvKTsRA4+2aIbOqHIEt0lC+45tY0fmlnelFIFlMYAU3wa4bBDAIQPM+0A
  FqQhljc55aKn26zok1m5SQKBgQC2YNR3bHmKT+1ERL3HS2KGiRG+WMDMaZZcpFup
  9pMT0egN8Ewqk2Hnemfyv7Or73Pq0lJ2EBkas8rdE71v8N16Kbhh35gT0RzerZ/9
  DQZb2xNtOUe/z9r9MX4WwC8VWfySqCWsl/kxhex9sjdMB6ioIeDZJyFjV8J2U9uk
  bOHsPQKBgFDQuTQZWyBgL7+uhr9EEYKomoTWq/wmHrc9e8xCEinTMNfgfe4WxYHe
  fEFw4A19w0xoLlWj0MTn3h1hqGk8DSHDprX9VJlQWLuzXkk0S0gD2Fj/R1Qm1ucP
  iTOe6wpCn9mEKbW2Vacd3PFLt6fOZ5mkFKSsoUqGyResnWIVSED0
  -----END RSA PRIVATE KEY-----

  ```

  **Public key:**
  > This file is safe to share
  ```bash
  cat ~/.ssh/id_rsa.pub
  ```
  The output should look something like
  ```
  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCXcZF+qbDfq32W2Tma6oV0ab45B0vByn6q3HV19QMxDQHWgFZiA1S+8CdWR96Lecmco3sYZ1THVNcdY+YAmxT3Xl7gyIrejW2ikSou7/B+XSSDRtgEsc2QZNnKUcIZdm3Y2o+Qd8Cti+Nx27urp+UO2jVAkvGIpDQaHcsMUacstuto40dLLfjOY6aZ3KKaWZGjCL9oBPc0ScH4h5mBlXLi1/zPxFrWpmRC72BK3EFKrPr/1y24nJK/KGMKl8FWuh+qTlcWPd2ZtBTtRsuzdOCGE32p8fIzBjCm5+U5F5ceJmry33NUYMDFgXYejunUcnKxOjzTACw3qN0pIU03CUOX weehan@cvwo-2020
  ```
5. Congratulations, you have setup your SSH key pair!
## Create a DigitalOcean droplet

[DigitalOcean](https://www.digitalocean.com/about/)  is a cloud platform which provides **Infrastructure as a Service (IaaS)**. This just means you can spin up **Virtual Machines (VMs)** in the cloud that you can access using SSH. These VMs are known as **Droplets** in DigitalOcean terminology.

You can think of a cloud VM as just another computer running somewhere in the world (in this case, in one of DigitalOcean's data centers). However, unlike regular computers that we use, these computers are meant to run 24/7, and are permanently conencted to the internet. Hence, this makes it very suitable for serving web applications.

There are other companies that provide IaaS services such as Amazon ([AWS EC2](https://aws.amazon.com/ec2/)), Google ([GCP Compute Engine](https://cloud.google.com/compute)) and MicroSoft ([Azure VMs](https://azure.microsoft.com/en-us/services/virtual-machines/)), but DigitalOcean has a clean and simple UI and is very beginner friendly.

> Aside from IaaS, other commonly used cloud services include Platform as a Service (PaaS) and Software as a Service (SaaS). 
Examples of PaaS applications include Heroku and Netlify, and examples of SaaS are Google Docs and Google Sheets. Read more about the differences [here](https://www.bmc.com/blogs/saas-vs-paas-vs-iaas-whats-the-difference-and-how-to-choose/).

In this section, we will create a DigitalOcean account and spin up a droplet.


1.  Create a new [DigitalOcean](https://www.digitalocean.com/) account.

> **TIP**: If you are a student, you are eligible for $50 DigitalOcean credits under the [GitHub Student Developer pack](https://education.github.com/pack). If you are feeling generous, you can also sign up with my [referral link](https://m.do.co/c/4ebee4a0ce0f), which will give you $100 (but only for 60 days).


2. Create a new project
3. Create a new droplet with the following configurations. For those not specified, just leave it as the default.
- **Distro:** The latest Ubuntu version 
- **Plan:** Starter plan that costs $5/month
- **Data Center region:** Singapore
- **Authentication:**: Click on `New SSH key` and enter the contents in the file `~/.ssh/id_rsa.pub`. You can get its contents by running:
  ```bash
  cat ~/.ssh/id_rsa.pub
  ```
  > Make sure *not* to accidentally use `~/.ssh/id_rsa`
- **Hostname:** This can be whatever you want to name your droplet. I named mine `cvwo`.

4. Click on create droplet. It should take up to a few minutes to create a droplet. Afterwards, go to the droplets page, and you will see something like this:

![Alt text](./DigitalOcean-Droplet-Page.png?raw=true "DigitalOcean page")

5. Congrats on creating your first DigitalOcean droplet!

## Access the droplet and create a new user

In this section, we will be accessing the droplet we created, and we will also create a new unix user.

1. Copy the IP address of the droplet from the 'droplets' page. In my case, the IP address is 161.35.59.43.

2. SSH into the droplet as the `root` user with the droplet's IP address.

  ```bash
  ssh root@161.35.59.43
  ```
  > If you are unable to SSH into the system, you probably made a mistake in the previous steps. Do review them again if you are having trouble.

  - The [root user](https://en.wikipedia.org/wiki/Superuser) is a special account used for system administration. It has the privilege to do almost anything in the system. It is generally not recommended to run commands as the root user unless you know what you are doing.

3. Create a new user with your desired username. I will use `weehan` for subsequent examples, but this can be any username of your choice.
```bash
adduser weehan
```
- You will be prompted for a password and other details.

4. Make yourself a `sudo` user.
```bash
usermod -aG sudo weehan
```
  - `usermod` stands for 'modify user'
  - `aG` stands for 'add group'
  - Hence, what the command is doing is adding the user `weehan` to the group `sudo`.
  - users in the `sudo` group, also known as 'sudo users' or 'sudoers', are able to run privileged commands as the `root` user. Read more about it [here](https://www.linux.com/training-tutorials/linux-101-introduction-sudo/).

5. Switch to the newly created user.
```bash
su weehan
```
  - `su` is short for 'switch user'

6. Create a `.ssh` directory
```bash
mkdir ~/.ssh
```
  - `~` is a shorthand notation for your home directory, which is `/home/<username>`. In my case, it is `/home/weehan`. The command above hence creates the directory `/home/weehan/.ssh`

7. Create a file called `~/.ssh/authorized_keys`
```bash
touch ~/.ssh/authorized_keys
```

8. Copy and paste the contents of your public key and paste it into `~/.ssh/authorized_keys`

9. Exit the server
```bash
exit
```

10. SSH into the server as your username
```bash
ssh weehan@161.35.59.43
```

11. Congrats on creating your first user!

## Install React, Rails and PostgreSQL

## Web Servers with Nginx

When developing Rails, you have probably noticed that your application runs on `localhost:3000` and is being served by a **puma** server. Puma is what is known as an **Application server**, as it serves the content of your application. While it can be done, developers usually do not serve content directly to the internet using Application servers. The is because there might be some stuff you want to add before serving your content to the internet, most commonly HTTPS.

This is where **Web servers** come in. Web servers can add functionality between your application and the internet, such as serving static assets like images directly, HTTPS and Load balancing. The most commonly used web servers include [Nginx](https://en.wikipedia.org/wiki/Nginx) and [Apache server](https://en.wikipedia.org/wiki/Apache_HTTP_Server). In this section, we will install nginx and use it to serve our rails application, albeit without any of the fancy stuff.

1. Install nginx

```bash
sudo apt-get update
sudo apt-get install -y nginx 
```

2. Become the `root` user.

```bash
sudo su
```
3. Create a new file at `/etc/nginx/sites-available/app.conf` with the following contents:

```nginx
upstream app {
    server localhost:8080;
}

server {
    listen 80;
    listen [::]:80;
    server_name 161.35.59.43

    # Pass request on to application
    location / {
        proxy_pass http://app;
    }
}
```
- There are 2 blocks of code. The first is an **upstream** block and the second is a **server** block.
- The **upstream** block is a way to identify where your Application Server is running. In this case, our application is being served on `localhost:8080`. We have also given the name `app` to our upstream block.
- The **server** block creates a server that listens on **port 80** of the IP address `161.35.59.43`, i.e. the droplet. Replace this with the IP address of your own droplet

  - Port 80 is the default port for serving HTTP content. For example, when you connect to google.com, you are actually connecting to port 80 of google.com, i.e. google.com:80. 

- Within our server block, we have defined a **location** block with the path `/`.
  - This means that for all queries that start with `/` (i.e. *all* queries in this case), it will be passed (known as **proxy passed**) to our upstream block.
  - There can be multiple location blocks within a server block. If so, any incoming request will be passed to the **longest match path** as opposed to sequentially. Read more about [location matching here](https://www.keycdn.com/support/nginx-location-directive#:~:text=In%20case%20the%20longest%20matching,stored%20and%20the%20process%20continues.).

4. Create a [soft link](https://www.geeksforgeeks.org/soft-hard-links-unixlinux/) from `/etc/nginx/sites-enabled/app.conf` to `/etc/nginx/sites-available/app.conf`
```bash
ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/app.conf
```

5. Restart nginx
```bash
sudo systemctl restart nginx
```

6. Run your application on `localhost:8080` on your droplet.

7. Open your browser and key in your IP address (``161.35.59.43``) in the address bar. You should be able to see your application now. Congratulations!

## DNS with Namecheap

It's a lot easier for humans to remember words as compared to numbers. Hence, the [Domain Name System (DNS)](https://www.cloudflare.com/learning/dns/what-is-dns/) was created to map domain names to IP addresses on the internet. In this section, you will set up your own domain and link it to your DigitalOcean droplet, and in the process learn about DNS records.

1. Sign up for a domain with [Namecheap](https://www.namecheap.com/). If you own domains under other domain providers (e.g Godaddy), the steps should be similar.
  > **TIP**: If you are a student, you are eligible for a free domain ending in `.me` for a year from Namecheap under the [GitHub Student Developer pack](https://education.github.com/pack).

2. Go to the Namecheap dashboard and click on 'Manage' next to the domain you purchased. For this tutorial, I  will be using a domain I own, `hello.com.de`.

![Alt text](./Namecheap-Dashboard.png?raw=true "DigitalOcean page")

3. Go to the 'Advanced DNS' tab and add the following DNS Records.
- Add an `A Record` from the `@` host to your DigitalOcean IP address. This will redirect all requests from hello.com.de to 161.35.59.43.
  
  > A Records are the most important type of DNS records. They connect domain names to IP addresses.
  
- Add a `CNAME Record` from the `www` to your domain. This will redirect all requests from `www.hello.com.de` to `hello.com.de`
  > CNAME Records are used for aliasing, i.e. redirecting from one domain to another domain, or from a subdomain (like www) to a domain.

- If you did the steps above correctly, they shoud look something like this. You may refer to [this guide](https://ap.www.namecheap.com/Domains/DomainControlPanel/hello.com.de/advancedns) for further clarification.

![Alt text](./DNS-records.png?raw=true "DigitalOcean page")

- `A Records` and `CNAME Records` are some just some examples of DNS records. There are [many other kinds of DNS records](https://ns1.com/resources/dns-types-records-servers-and-queries).

- Due to caching of DNS, it may take between a few hours and 2 days for this DNS record change to be reflected on the internet. This is known as [DNS Propagation](https://www.siteground.com/kb/what_is_dns_propagation_and_why_it_takes_so_long/).

## HTTPS with Certbot

**HTTPS** is a protocol for sending HTTP Traffic over **SSL (Secure Sockets Layer)**. Without technical jargon, this just means that the data sent between you and the server is encrypted, so no one who gets a hold of your data (e.g. your ISP) is able to read its contents.

The HTTPS protocol is based on **Public Key Infrastructure (PKI)**, an extension of Public Key Authentication. In addition to the 2 parties (client and server), there is an additional body called the **Certificate Authority (CA)** which is trusted by both the client and server. The CA issues a **certificate** to the server, who in turn presents it to the client as proof of authenticity. These certificates form the basis of how HTTPS works. Do checkout this [video](https://www.youtube.com/watch?v=T4Df5_cojAs&t=376s) for a better understanding of how PKI works.

In this section, we will be obtain a certificate for our domain and serve it using our Nginx web server, so our website will be secured with HTTPS.

1. [Install Certbot](https://certbot.eff.org/all-instructions). 
  - `Certbot` is a free command line tool for generating SSL certificates.
  - The certificate we will be generating is provided by [`LetsEncrypt`](https://letsencrypt.org/), a non-profit CA.
2. Update nginx configuration in `/etc/nginx/sites-available/app.conf` to add an additional location block to the server.

```nginx
upstream app {
    server localhost:8080;
}

server {
    listen 80;
    listen [::]:80;
    server_name hello.com.de

    # Path for certbot renewal
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
  
    # Redirect to HTTPS
    location / {
        return 301 https://$host$request_uri;
    }
}
```
  - This additional block is used by Certbot for generating the certificate using the **Automatic Certificate Management Environment (ACME)** protocol, more specifically the **HTTP-01** Challenge. Read more about the ACME protocol [here](https://letsencrypt.org/docs/challenge-types/).

3. Restart nginx
```bash
sudo systemctl restart nginx
```

4. Generate certificate for `hello.com.de`
```bash
sudo certbot certonly -d hello.com.de
```
  - The newly generated certificate will be stored in the directory `/etc/letsencrypt/live/hello.com.de/`. It should contain the following 4 files:
    - `cert.pem`: The certificate itself
    - `chain.pem`: Additional intermediate certificate(s) that web browers need to validate the certificate. See [chain of trust](https://www.youtube.com/watch?v=LPxeYtMDxl0).
    - `fullchain.pem`: All certificates, i.e server certificate + additional intermediate certificates.
    - `privkey.pem`: Private key for the certificate.
  - For purposes of authentication, having both `fullchain.pem` and `privkey.pem` is sufficient. We will be serving these files using Nginx in the next step.

5. Update nginx configuration again in `/etc/nginx/sites-available/app.conf`
```nginx
upstream app {
    server localhost:8080;
}

server {
    listen 80;
    listen [::]:80;
    server_name hello.com.de

    # Path for certbot renewal
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
  
    # Redirect to HTTPS
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen 443;
    listen [::]:443;
    server_name hello.com.de

    # Path for certbot renewal
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }

    	# set max client body to 20M for file attachments
	client_max_body_size 20M;

	# certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
	ssl_certificate /etc/letsencrypt/live/hello.com.de/fullchain.pem;
	ssl_certificate_key /etc/letsencrypt/live/hello.com.de/privkey.pem;
	ssl_session_timeout 1d;
	ssl_session_cache shared:MozSSL:10m;  # about 40000 sessions
	ssl_session_tickets off;

	# intermediate configuration
	ssl_protocols TLSv1.2 TLSv1.3;
	ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
	ssl_prefer_server_ciphers off;

	# HSTS (ngx_http_headers_module is required) (63072000 seconds)
	add_header Strict-Transport-Security "max-age=63072000" always;

	# OCSP stapling
	ssl_stapling on;
	ssl_stapling_verify on;

	# Google DNS resolver
	resolver 8.8.8.8;

	proxy_set_header Host $http_host;
	proxy_set_header X-Real-IP $remote_addr;
	proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_set_header X-Forwarded-SSL on;
	proxy_set_header X-Forwarded-Proto $scheme;
	proxy_pass_request_headers on;

  
    # Pass request on to application
    location / {
        proxy_pass http://app;
    }
}
```
  - A new server block listening at **port 443** is added.
    - Port 443 is the default port for serving HTTPS content. For example, when you connect to https://www.google.com, you are actually connecting to port 443 of https://www.google.com, i.e. https://www.google.com:443. 

  - The old server block listening at post 80 will now redirect to the https block with a status code of 301.

  - Some additional headers related to SSL are added to the request before it is proxy passed to the upstream (i.e. the Application server).

6. Restart nginx
```bash
sudo systemctl restart nginx
```

7. If you open your webpage now, your website should now be served securely over HTTPS. You should see a green lock when accessing your website. Good work!

## Remote Storage with AWS S3 Buckets

## Backups with Cron jobs

## Systemd services

## Containerization with Docker

## Container Orchestration with Docker-compose

© tiuweehan 2020
