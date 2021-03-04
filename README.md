# PeerTube for Reclaim Cloud
PeerTube, developed by Framasoft, is the free and decentralized alternative to video platforms. Anyone with a modicum of technical skills can host a PeerTube server, aka an instance. Each instance hosts its users and their videos. In this way, every instance is created, moderated and maintained independently by various administrators.

## Deploy to Reclaim Cloud
[Click here to deploy to Reclaim Cloud](https://app.my.reclaim.cloud/?app=peertube)

## Installation Instructions
You can find PeerTube by clicking Marketplace and then Applications to show all or searching by name.

![Screen Shot 2020-06-04 at 11.29.34 AM|530x103](https://community.reclaimhosting.com/uploads/default/original/2X/6/66fffe086313e6975f16e1afe89e18c34510c6c8.png) 

![Screen Shot 2021-03-03 at 8.41.03 PM|690x282](https://community.reclaimhosting.com/uploads/default/optimized/2X/e/e4ce7d83ff89e2a8b2a0d6d60c3a8b88c0a5b024_2_1380x564.png) 

Installing is as easy as setting an environment name and region for the install. SSL will be provisioned automatically for the environment and your install will be up and running in just a few minutes. Your username and password for the install will be sent by email as well as displayed in the confirmation dialog.

![Screen Shot 2021-03-03 at 8.42.24 PM|690x448](https://community.reclaimhosting.com/uploads/default/original/2X/5/5a10b95a309cf013cec1ad8b16ddbbb2d967b3a0.png) 

![Screen Shot 2021-03-03 at 8.42.43 PM|690x234](https://community.reclaimhosting.com/uploads/default/original/2X/1/1ec4ba0152e707c8aa02ae91e35c12a0f734f7de.png) 

![Screen Shot 2021-03-03 at 8.41.57 PM|690x295](https://community.reclaimhosting.com/uploads/default/original/2X/4/4545f6cdd93061506ac31a08761532352a0f1de7.png) 

Once the install is complete you can login with root credentials and begin setting up your instance. 

![Screen Shot 2021-03-03 at 8.43.35 PM|690x402](https://community.reclaimhosting.com/uploads/default/optimized/2X/5/53dd09c39bc6a03350cddb504e51f2aa2ea35d1b_2_1380x804.jpeg) 

## Domain Mapping with PeerTube

The automated installer makes it very easy to get up and running with a development subdomain of reclaim.cloud. Should you wish to use a more permanent top level domain or subdomain you own, we strongly recommend setting that up immediately after install as there are some extra steps involved and changing the URL after your PeerTube install has been running is not advised due to how federation and permalinks work in the system.

To get started you will need to point a DNS A record to the dedicated IP address for your install. You can find the IP by expanding the node in the interface dropdowns to show details and selecting the IPv4 address (internal addresses will begin with 10.x, you want the external address which is usually listed second).

![Screen Shot 2021-03-03 at 8.45.58 PM|690x203](https://community.reclaimhosting.com/uploads/default/original/2X/0/017289ae1ea60d81e5a3a041491fa37665c4c252.png) 

After setting up the DNS record and giving it time to resolve, you will need to update the .env file in the root of the server. You can access the files through the Web SSH area and edit this file with `nano .env`. You will replace all instances of reclaim.cloud subdomains with the new domain. When finished type CTRL-X and Y to confirm the changes.

![Screen Shot 2021-03-03 at 8.49.01 PM|690x163](https://community.reclaimhosting.com/uploads/default/original/2X/a/a4fe677c3ec5656cc23497441179433693a48f40.png) 

![Screen Shot 2021-03-03 at 8.49.32 PM|690x204](https://community.reclaimhosting.com/uploads/default/optimized/2X/8/879c7ccd97bad82f78ecbfd2784075229a68ed32_2_1380x408.png) 

Go ahead and stop the current environment by running:

`docker-compose down`

You will also need to provision a new certificate for the domain. Copy and paste this command changing the domain to your own and use your own email address.

`docker run -it --rm --name certbot -p 80:80 -v "$(pwd)/docker-volume/certbot/conf:/etc/letsencrypt" certbot/certbot certonly --standalone -d yourdomain.com --non-interactive --agree-tos -m your@email.com`

Once you've provisioned the certificate the last step is to rebuild the environment to utilize the new domain. Run the following to rebuild and restart the application:

`docker-compose up -d --build`