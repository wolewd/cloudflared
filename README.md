## Purpose
I  don't want to expose my VPS IP and for security reason I only allow some specific port inside my UFW tables:
1. 22 for SSH *#sometimes i also block this in case I don't use fail2ban*
2. 80 for http
3. 443 for secure http
4. 587 for email services

After finishing my ufw tables setup, I tunnel my VPS to cloudflare using their service called [cloudflare tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/).

To make the process simple, i prepare this simple cloudflare tunnel setup using docker compose.

I also include docker network called `application_network` inside this setup to allow my application comunicate using this network.

When I create another services, I can simply comunicate using `application_network` instead exposing ports.

## Usage
1. clone this repo
   ```
   https://github.com/wolewd/cloudflared.git
   ```
2. create **.env** file
   ```
   touch .env
   ```
4. add this line inside your **.env** file and change the **TUNNEL_TOKEN** value to your tunnel token from Cloudflare tunnel
   ```
   TUNNEL_TOKEN=put-your-tunnel-token-here 
   ```
5. start the docker service
   ```
   docker compose up -d
   ```
