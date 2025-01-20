# Self-hosting project on pi

This project was an experiment to learn to self-host a website on a raspberry pi, and expose it to the internet.
After some reading, learning and testing, the takeaway is that it is very do-able. 
For static sites, the traffic (requests/bandwidth) is easily manageable with home internet. Hence [nahar](https://nahar.cc) was born..
The next phase is to add a service-layer and analyze the resource usage and scalability potential.

# Related tools
- [system-monitor](https://github.com/tmazumdar/system-monitor)
- [Cloudflare](https://www.cloudflare.com/)

# Guides

https://samhobbs.co.uk/2014/02/how-install-wordpress-raspberry-pi

https://fireship.io/lessons/host-website-raspberry-pi/

https://www.nano-editor.org/dist/latest/cheatsheet.html

https://github.com/ddclient/ddclient/blob/main/ddclient.conf.in

# Documentation

DNS Lookup occurs behind the scenes. 

https://www.cloudflare.com/learning/dns/what-is-dns/

In order to host a website on an IOT device, it needs a static External IP.
The IP needs to be registed with a DNS service so that it can be looked up externally.

![image](https://github.com/user-attachments/assets/595e05ef-6d58-46f3-8031-4514255d7c3a)

# Prerequisites
1. nginx - install, to configure: add server block to nginx.conf (`sudo nano /etc/nginx/sites-enabled/default`)
2. add port forwarding to router config for ports 80, 443
3. ddclient - checks dyndns (http://checkip.dyndns.org/) to get current IP and updates to cloudflare server
using API Token (privileges: Zone.DNS.Edit, Zone.Zone.Read). Configured to run as service every 600s (`sudo nano /etc/ddclient.conf`)
4. certbot - install, new cert `sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com`, auto-renew `sudo certbot renew --dry-run`
5. cloudflare - DNS A Records (yourdomain.com, www.yourdomain.com) with external IP, set SSL/TLS to Full(strict)
6. ufw - linux uncomplicated firewall - ensure nginx is allowed `sudo ufw allow 'Nginx Full'`
7. timeshift - run and retain atleast 1 backup of server on separate disk/usb drive
