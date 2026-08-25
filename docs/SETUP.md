# Setup Guide — Running OWASP Juice Shop Locally (Docker)

This guide walks through installing, running, testing, and stopping OWASP Juice Shop using Docker. Verified on Windows (Command Prompt); works identically on macOS/Linux shells.

## Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop/) installed and running
- Git (optional — only needed if you want the source code)

## 1. Verify Docker Installation
```bash
docker --version
```

## 2. Pull the OWASP Juice Shop Image
```bash
docker pull bkimminich/juice-shop
```

## 3. Run the Juice Shop Container
```bash
docker run -d -p 3000:3000 --name juice-shop bkimminich/juice-shop
```
- `-d` → runs in the background
- `-p 3000:3000` → maps container port to `localhost:3000`
- `--name juice-shop` → names the container for easy reference

## 4. Access the Application
Open your browser and go to:
```
http://localhost:3000
```

## 5. Check the Running Container
```bash
docker ps
```

## 6. View Logs (optional)
```bash
docker logs juice-shop
```

## 7. Stop the Container
```bash
docker stop juice-shop
```

## 8. Remove the Container
```bash
docker rm juice-shop
```

## 9. (Optional) Clone the Source Code
```bash
git clone https://github.com/juice-shop/juice-shop.git
cd juice-shop
```

## 10. Restart an Existing Container
```bash
docker start juice-shop
```

## Notes
- Default port: `3000`
- If port 3000 is already in use, map to another port, e.g. `-p 4000:3000`
- Ensure Docker Desktop is running before executing any commands
