📌 Session Overview
🔹 1. Understanding Domains & DNS
Before diving into domain management, we covered:

Domain Basics: What is a domain, how it works, and its role in networking.
Types of Domains:
Top-Level Domains (TLDs) (e.g., .com, .org, .net, .io)
Second-Level Domains (SLDs)
Subdomains
DNS Flow: How a domain name is resolved to an IP address (recursive & authoritative name servers).
🔹 2. Configuring DNS for an EC2 Instance with Nginx
🔸 Direct DNS Configuration (Without AWS Route 53)
Purchase a domain from any provider (e.g., Namecheap, GoDaddy).
Update the A record in the domain provider’s DNS settings:
Record Type: A
Value: Public IP of your EC2 instance
Install Nginx on your EC2 instance:
sudo apt update && sudo apt install nginx -y
Configure Nginx to serve the domain:
sudo nano /etc/nginx/sites-available/default
Update the server block:
server {
    listen 80;
    server_name yourdomain.com;
    root /var/www/html;
    index index.html;
}
Restart Nginx:
sudo systemctl restart nginx
🔸 Configuring DNS with AWS Route 53
Create a Hosted Zone in AWS Route 53 for your domain.
Update Name Servers (NS) in your domain provider’s settings with the ones from Route 53.
Add an A record in Route 53 for your EC2 instance:
Record Type: A
Value: Public IP of your EC2 instance
🔹 3. Advanced Route 53 Routing Policies
Simple Routing Policy - Basic DNS resolution.
Latency-Based Routing - Routes users to the closest low-latency server.
Geolocation-Based Routing - Directs users based on their region.
Failover Routing - Ensures high availability by switching traffic to a backup resource.
Weighted Routing - Distributes traffic across multiple resources based on percentage weights.
🔹 4. Linux Essentials - 20 Must-Know Commands
We covered these fundamental Linux commands:

Command	Description
ls	List directory contents
cd	Change directory
mkdir	Create a new directory
rmdir	Remove an empty directory
rm	Delete a file
touch	Create a new empty file
cat	Display file contents
locate	Find a file by name
cp	Copy files and directories
mv	Move or rename files
chmod	Change file permissions
chown	Change file ownership
ps	Display running processes
top	Show system resource usage
df	Show disk space usage
du	Show directory size
grep	Search inside files
find	Locate files based on conditions
tar	Archive files
nano / vim	Text editors
🔹 5. Advanced Git Commands & Concepts
Git Command	Description
git cherry-pick <commit>	Apply specific commits from another branch.
git merge <branch>	Merge one branch into another.
git rebase <branch>	Reapply commits on top of another branch, keeping history clean.
git reset --hard <commit>	Reset branch to a previous commit, discarding all changes.
git reset --soft <commit>	Reset branch but keep changes staged.
git stash	Save uncommitted changes for later use.
git stash pop	Apply and remove the latest stashed changes.
git push --force	Force push changes (use with caution!).
🎯 Key Takeaways
✅ Understanding domains, DNS flow, and how domain resolution works.
✅ Configuring a domain to point to an EC2 instance both manually and via AWS Route 53.
✅ Implementing different Route 53 routing policies.
✅ Mastering essential Linux commands for system administration.
✅ Learning advanced Git techniques for efficient version control.


Happy coding! 🚀

