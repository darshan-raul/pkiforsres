# Lab 2.2: HTTPS Server

**Goal:** Configure a web server (Nginx) with the certificates created in Lab 2.1.

## Prerequisites
*   Docker
*   Certificates from Lab 2.1 (`leaf/server-key.pem`, `leaf/fullchain.pem`)

## 1. Create Nginx config

Create a file named `nginx.conf`:

```nginx
events {}
http {
    server {
        listen 443 ssl;
        server_name mysite.local;

        ssl_certificate /etc/nginx/certs/fullchain.pem;
        ssl_certificate_key /etc/nginx/certs/server-key.pem;

        location / {
            return 200 "Hello from Secure Nginx!\n";
        }
    }
}
```

## 2. Run Nginx via Docker

Mount the certs and config.

```bash
docker run --rm -d --name my-nginx \
  -p 8443:443 \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
  -v $(pwd)/leaf:/etc/nginx/certs:ro \
  nginx:alpine
```

## 3. Test with Curl

If you simply curl, it will fail because your OS doesn't trust "MyFakeOrg Root CA".

```bash
curl -v https://localhost:8443
# Fails with SSL certificate problem
```

## 4. Test forcing trust

Tell curl to trust your Root CA.

```bash
curl -v --cacert root/root-cert.pem --resolve mysite.local:8443:127.0.0.1 https://mysite.local:8443
```

**Success!** You should see the handshake verify successfully.
