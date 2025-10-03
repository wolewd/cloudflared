# Cloudflare Tunnel with Docker Compose

[![Docker](https://img.shields.io/badge/Docker-✔-2496ED?logo=docker&logoColor=white)](https://www.docker.com/) [![Cloudflare](https://img.shields.io/badge/Cloudflare-Tunnel-F38020?logo=cloudflare&logoColor=white)](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/)  

A lightweight and secure setup for Cloudflare Tunnel using Docker Compose.  
This project is designed to keep your VPS safe by hiding its IP address and only exposing necessary ports, while allowing internal services to communicate securely without opening extra ports.  

---

## Purpose

I don’t want to expose my VPS IP. For security reasons, I only allow specific ports through UFW:

- `22` → SSH *(sometimes disabled when fail2ban is not active)*
- `80` → HTTP  
- `443` → HTTPS  
- `587` → Email services  

After setting up UFW rules, I route VPS traffic through Cloudflare Tunnel.  

To simplify this setup, I created a Docker Compose configuration for Cloudflare Tunnel.  
I also included a custom Docker network called `application_network` so that internal services can communicate securely over this network — without exposing any ports.  

When adding new services, simply attach them to `application_network` for private internal access.  

---

## Prerequisites

- A VPS with Linux (tested on Ubuntu & Archlinux)  
- Docker and Docker Compose installed  
- A [Cloudflare Tunnel](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/) configured from your Cloudflare account  

---

## Installation & Usage

1. Clone this repository:
   ```bash
   git clone https://github.com/wolewd/cloudflared.git
   cd cloudflared
   ```

2. Create an `.env` file:
   ```bash
   touch .env
   ```

3. Add your Cloudflare Tunnel token inside `.env`:
   ```env
   TUNNEL_TOKEN=put-your-tunnel-token-here
   ```
   Refer to the provided [.env.example](https://github.com/wolewd/cloudflared/blob/main/.env.example).

4. Start the container:
   ```bash
   docker compose up -d
   ```

---
## Adding New Services

When you create a new service (e.g., a web app, API, or database), attach it to the same Docker network:

```yaml
networks:
  - application_network
```

This ensures it can communicate securely with other services without opening ports.  

---

## Security

- No unnecessary ports are exposed  
- Only essential ports (22, 80, 443, 587) are allowed via UFW  
- Services communicate internally using `application_network`  
- All external access is routed through Cloudflare Tunnel  

---
