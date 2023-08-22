# nodejs_ec2_server_aws

Ok, you have clear ec2 machine on AWS (if you don't know how to setup machine look throught this [link][ec2]).

What should we do now? In this topic I will describe step by step what you need for ready to go Node server on linux machine.

Configuration
On AWS console:

Step 1 -- Update dependencies
From scratch ec2 linux machine use yum package manager. Let update it to the latest version:

```sudo yum update```\
Step 2 -- Install git for project version control
```sudo yum install git```\
Step 3 -- Install Node.js and npm
There are 2 ways to do it:

a) Install via nvm (node version manager) - will give you a possibility to switch between node versions in a second

-- Download and install nvm:

```sudo curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh | bash```\
-- Node\npm installation:

nvm install node
-- Specific version installation:

```nvm install x.x.x```\
-- To check installed versions:

nvm ls
-- To use specific version:

```nvm use x.x.x```\
b) Install via yum

```sudo curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash
sudo yum install nodejs
node --version
npm --version
```
Step 4 -- Install specific dependencies
There are some dependencies require to start Node server (ex. Python)

Easiest way - install group of Development Tools - will install (automake, gcc, perl, python, flex, make, gdb, bison)

```yum groupinstall 'Development Tools'```\
You can check it using command:

```yum group info 'Development Tools'```\
Minimum what you will require is g++:

```yum install gcc-g++```\
for Fedora 20:

```apt-get install build-essential```\
Step 5 -- Install pm2
```npm install pm2```\
Step 6 -- Install and setup nginx
```sudo yum install nginx```\
In nginx.conf file set redirect port:
```
location / {
               proxy_pass http://127.0.0.1:3000;
           }
```
main commands for nginx manage:
```
sudo service nginx start
sudo service nginx stop
sudo service nginx restart
```
In case you need ssl connection. In nginx.conf file add following code:
```
server {
        listen       443 ssl http2 default_server;
        listen       [::]:443 ssl http2 default_server;
        server_name www.wixanswersjiraintegration.info wixanswersjiraintegration.info;
        root         /usr/share/nginx/html;
        add_header Strict-Transport-Security "max-age=31536000";
        ssl_certificate /etc/letsencrypt/live/wixanswersjiraintegration.info/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/wixanswersjiraintegration.info/privkey.pem;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_prefer_server_ciphers on;
        ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH:AES256+ECDHE';
}
```
Step 7 -- Create sertificate
In continuation of previous step if you need https on your server you can do next steps (ex. letsencrypt):

a) Clone letsencrypt repo:

```sudo git clone https://github.com/letsencrypt/letsencrypt /opt/letsencrypt```\
b) Move to folder:

```cd /opt/letsencrypt```\
c) Create certificate:

```sudo -H ./letsencrypt-auto certonly --standalone -d example.com -d www.yourboughtdomain.com```\
d) In case installation was fine you can find it in:

```sudo ls /etc/letsencrypt/live```\
e) For renew SSL certificates:
```
cd /opt/letsencrypt
sudo -H ./letsencrypt-auto certonly --standalone --renew-by-default -d example.com -d www.yourboughtdomain.com
```
f) For automatically renew SSL certificates (optional). Create a crontab and add the following line to the end of the crontab file:
```
./letsencrypt-auto renew sudo crontab -e 0 0 1 * * /opt/letsencrypt/letsencrypt-auto renew
```
Update Let's Encrypt (optional):
```
cd /opt/letsencrypt
sudo git pull
```
Useful links
Here links which you may also need:\
1)https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html\
2)https://sumantmishra.medium.com/how-to-deploy-node-js-app-on-aws-with-github-db99758294f1\


[ec2]: https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/get-set-up-for-amazon-ec2.html
