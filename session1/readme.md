🚀 PHASE 1: AWS account setup (pehla step)
✅ Step 1: AWS account banao

Go to:
👉 Amazon Web Services

Sign up karo
Email verify karo
Card add hota hai (free tier available hota hai)
🚀 PHASE 2: EC2 machine create karna
✅ Step 2: EC2 open karo

AWS dashboard → search:
👉 EC2

✅ Step 3: Instance create karo

Click:
👉 “Launch Instance”

Settings:

Name: mern-server
OS: Ubuntu 22.04
Instance type: t2.micro (free tier)
Key pair:
“Create new key pair”
Download .pem file 🔥 (VERY IMPORTANT)
Security Group:
✔ SSH (22)
✔ HTTP (80)
✔ HTTPS (443)

👉 Launch instance

✅ Step 4: IP copy karo

Instance open karo → copy:

👉 Public IPv4 address

Example:

3.110.45.22
🚀 PHASE 3: Ab CMD / Git Bash open karo (Windows)
⚡ Step 5: Git Bash open karo

Agar Git installed hai:
👉 Right click → “Git Bash Here”

⚡ Step 6: key file wali folder me jao

Agar download folder me hai:

cd Downloads
⚡ Step 7: server connect karo (SSH)
ssh -i your-key.pem ubuntu@YOUR-IP

Example:

ssh -i mykey.pem ubuntu@3.110.45.22
🎉 Agar successful hua to yeh aayega:
Welcome to Ubuntu
ubuntu@ip-172-31-xx:~$

👉 Matlab: tum AWS server ke andar ho 🔥

🚀 PHASE 4: Server setup (inside EC2)

Ab yeh commands run karo:

✅ Update system
sudo apt update && sudo apt upgrade -y
✅ Node install
sudo apt install nodejs npm -y

Check:

node -v
npm -v
✅ Nginx install
sudo apt install nginx -y
✅ PM2 install
sudo npm install -g pm2
🚀 PHASE 5: Apna project upload karo
Option A (best): GitHub
git clone your-repo
cd your-project
npm install
Run app:
pm2 start server.js --name app
🚀 PHASE 6: Browser test

Browser me open karo:

http://YOUR-IP:3000
🚀 PHASE 7: Nginx connect (final step)

Edit file:

sudo nano /etc/nginx/sites-available/default

Paste:

server {
    listen 80;

    server_name YOUR-IP;

    location / {
        proxy_pass http://localhost:3000;
    }
}

Restart:

sudo systemctl restart nginx
🎉 FINAL RESULT

Ab tum open karo:

http://YOUR-IP

👉 App live 🔥