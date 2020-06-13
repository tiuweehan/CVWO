# Deploy your Application on DigitalOcean

This guide is meant for students who are interested in deploying their applications to the cloud.

**DISLAIMER:** This guide assumes that you are familiar with basic unix commands and SSH/SCP. Otherwise, here are some useful tutorials to get started:

- [Beginner's Guide to the Bash terminal](https://www.youtube.com/watch?v=oxuRxtrO2Ag)
- [SSH Crash Course | With Some DevOps](https://www.youtube.com/watch?v=hQWRp-FdTpc)
> ^ This tutorial also goes through the setup for DigitalOcean droplets.

## Lesson Plan

- [Generate SSH key pair](#generate-ssh-key-pair)
- [Create a DigitalOcean droplet](#create-a-digitalocean-droplet)
- [Access the droplet and create a new user](#access-the-droplet-and-create-a-new-user)
- [Install React and Rails](#install-react-and-rails)
- [Web Servers with Nginx](#web-servers-with-nginx)
- [Remote Storage with AWS S3 Buckets](#remote-storage-with-aws-s3-buckets)
- [Backups with Cron jobs](#backups-with-cron-jobs)
- [Systemd services](#systemd-services)
- [DNS with Namecheap](#dns-with-namecheap)
- [HTTPS with Certbot](#https-with-certbot)
- [Docker](#containerization-with-docker)
- [Docker-compose](#container-orchestration-with-docker-compose)

## Generate SSH key pair

> If you already have an existing SSH key pair before, feel free to skip this section.

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
  ```
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
  ```
  cat ~/.ssh/id_rsa.pub
  ```
  The output should look something like
  ```
  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCXcZF+qbDfq32W2Tma6oV0ab45B0vByn6q3HV19QMxDQHWgFZiA1S+8CdWR96Lecmco3sYZ1THVNcdY+YAmxT3Xl7gyIrejW2ikSou7/B+XSSDRtgEsc2QZNnKUcIZdm3Y2o+Qd8Cti+Nx27urp+UO2jVAkvGIpDQaHcsMUacstuto40dLLfjOY6aZ3KKaWZGjCL9oBPc0ScH4h5mBlXLi1/zPxFrWpmRC72BK3EFKrPr/1y24nJK/KGMKl8FWuh+qTlcWPd2ZtBTtRsuzdOCGE32p8fIzBjCm5+U5F5ceJmry33NUYMDFgXYejunUcnKxOjzTACw3qN0pIU03CUOX weehan@cvwo-2020
  ```
5. Congratulations, you have setup your SSH key pair!
## Create a DigitalOcean droplet

[DigitalOcean](https://www.digitalocean.com/about/)  is a cloud platform which provides Infrastructure as a Service (IaaS). This just means you can spin up Virtual Machines (VMs) in the cloud that you can access using SSH.

There are other companies that provide IaaS services such as Amazon ([AWS EC2](https://aws.amazon.com/ec2/)), Google ([GCP Compute Engine](https://cloud.google.com/compute)) and MicroSoft ([Azure VMs](https://azure.microsoft.com/en-us/services/virtual-machines/)), but DigitalOcean has a clean and simple UI and is very beginner friendly.


1.  Create a new [DigitalOcean](https://www.digitalocean.com/) account.

> **TIP**: If you are a student, you are eligible for $50 DigitalOcean credits under the [GitHub Student Developer pack](https://education.github.com/pack). If you are feeling generous, you can also sign up with my [referral link](https://m.do.co/c/4ebee4a0ce0f), which will give you $100 (but only for 60 days).


2. Create a new project
3. Create a new droplet with the following configurations. For those not specified, just leave it as the default.
- **Distro:** The latest Ubuntu version 
- **Plan:** Starter plan that costs $5/month
- **Data Center region:** Singapore
- **Authentication:**: Click on `New SSH key` and enter the contents in the file `~/.ssh/id_rsa.pub`. You can get its contents by running:
  ```
  cat ~/.ssh/id_rsa.pub
  ```
  > Make sure *not* to accidentally use `~/.ssh/id_rsa`
- **Hostname:** This can be whatever you want to name your droplet. I named mine `cvwo`.

4. Click on create droplet. It should take up to a few minutes to create a droplet. Afterwards, go to the droplets page, and you will see something like this:

![Alt text](./DigitalOcean-Droplet-Page.png?raw=true "DigitalOcean page")

5. Congrats on creating your first DigitalOcean droplet!

## Access the droplet and create a new user

## Install React and Rails

## Web Servers with Nginx
## Remote Storage with AWS S3 Buckets
## Backups with Cron jobs
## Systemd services
## DNS with Namecheap
## HTTPS with Certbot
## Containerization with Docker
## Container Orchestration with Docker-compose

© tiuweehan 2020
