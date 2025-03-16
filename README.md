# Reverse Proxy with Docker and Nginx

This repository sets up a **Node.js** container, a **PHP-Nginx** container, and an **Nginx Reverse Proxy** to manage traffic between them. All containers are connected via a Docker network.

---

## **Prerequisites**

- Docker installed
- Basic knowledge of containerization

## **Create a Docker Network**

All containers must be on the same network:

```bash
docker network create reverse
```

---

## **Setup and Run the Node.js App**

### **Build the Node.js Container**

```bash
docker build -t hello-node -f Dockerfile-nodejs .
```

### **Run the Container**

- The Node.js app listens on **port 7000**

```bash
docker run -d --network reverse -p 7000:7000 --name hello-node hello-node
```

---

## **Setup and Run the PHP-Nginx App**

### **Build the PHP Container**

```bash
docker build -t hello-php -f Dockerfile-php .
```

### **Run the Container**

- The PHP app listens on **port 80** internally

```bash
docker run -d --network reverse -p 7001:80 --name hello-php hello-php
```

---

## **Setup and Run Nginx Reverse Proxy**

### **Important Notes:**

- Ensure that the **container names match the hostnames** used in the `nginx.conf`
- The reverse proxy should forward traffic to the **exposed ports of the Node.js and PHP apps**

### **Build the Reverse Proxy Container**

```bash
docker build -t reverse-proxy -f Dockerfile-reverse-proxy .
```

### **Run the Reverse Proxy Container**

```bash
docker run -d -p 7002:80 --network reverse --name nginx_proxy reverse-proxy
```

---

## **🔍 Testing the Setup**

Once all containers are running, test access:

### **Test Node.js App**

```bash
curl http://localhost:7000
```

### **Test PHP App**

```bash
curl http://localhost:7001
```

### **Test Reverse Proxy**

```bash
curl http://localhost:7002/node
curl http://localhost:7002/php
```

If everything is set up correctly, the reverse proxy should direct requests to the respective applications.

---

## **Stopping and Cleaning Up**

To stop and remove all containers:

```bash
docker stop hello-node hello-php nginx_proxy
docker rm hello-node hello-php nginx_proxy
docker network rm reverse
```
