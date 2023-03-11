# Docker and HTTPS 

## What is HTTPS 

HTTPS stands for Hypertext Transfer Protocol Secure, which is an extension of the HTTP protocol used for secure communication over the internet. It is a way of encrypting the data that is transmitted between a web browser and a web server, making it more difficult for attackers to intercept and steal sensitive information, such as login credentials or credit card numbers.

**To enable HTTPS on a website, you need to obtain an SSL (Secure Sockets Layer) certificate. An SSL certificate is a digital certificate that verifies the identity of a website and encrypts the data transmitted between the web server and the client's browser.** SSL certificates are issued by trusted certificate authorities (CA), such as Let's Encrypt, Comodo, and Symantec.

There are several ways to get an SSL certificate, depending on your needs and budget. Here are some options:

- Let's Encrypt: Let's Encrypt is a free and open certificate authority that provides SSL certificates for websites. It is widely used and trusted, and can be easily integrated with many web servers, including Apache and Nginx.
- Paid SSL certificates: There are many companies that offer paid SSL certificates, including Comodo, Symantec, and DigiCert. These certificates usually provide a higher level of validation and come with more advanced features, such as extended validation and wildcard certificates.
- Cloud hosting providers: Many cloud hosting providers, such as AWS, Google Cloud, and Azure, offer SSL certificates as part of their hosting packages. These certificates are often managed by the hosting provider, making it easier to install and renew them.

To obtain an SSL certificate, you typically need to generate a certificate signing request (CSR) on your web server, which contains information about your website and your public key. You then submit the CSR to a certificate authority, which will verify your identity and issue a certificate. Once you receive the certificate, you need to install it on your web server and configure your server to use HTTPS.

Keep in mind that SSL certificates have expiration dates and need to be renewed periodically, usually every one or two years. It's also important to ensure that your web server and applications are configured correctly to use HTTPS, and to keep your server and software up to date to address any security vulnerabilities.


## Add HTTPS to our Python/Postgres app

Let's take our project from the previous section and add an Nginx service 🤓

### Nginx

Nginx is a popular open-source web server that can also function as a reverse proxy, load balancer, and HTTP cache. It is known for its high performance, stability, and ability to handle a large number of simultaneous connections.

Developers should be familiar with Nginx because it is commonly used as a frontend web server in production environments. In addition to its performance benefits, Nginx is also highly customizable and can be used to handle complex routing, authentication, and security configurations.

Nginx also integrates well with many popular web frameworks and technologies, making it a valuable tool for developers who are building web applications. By leveraging Nginx's features, developers can improve the performance, scalability, and security of their applications.


### Generate a free ssl certificate 

First go to the root of your project and create a new directory call `certs` and then run this command :
```
openssl req -x509 -newkey rsa:4096 -keyout certs/key.pem -out certs/cert.pem -days 365 -nodes
```
This will create a certs folder and generate a self-signed SSL certificate with a private key (`key.pem`) and a public certificate (`cert.pem`) that are valid for 365 days.

Then add execution right to the private key in order to be executed by our nginx service inside our `Dockerfile` with the following command : 
```
chmod +x certs/key.pem
```

### Create an `nginx.conf` file 

Go to the root of your project and create a new file called `nginx.conf`

```perl title="nginx.conf"
events {}

http {
  upstream app {
    server app:8000;
  }

  server {
    listen 80;
    listen [::]:80;
    server_name yourdomain.com;
    return 301 https://$host$request_uri;
  }

  server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name yourdomain.com;

    ssl_certificate /etc/ssl/certs/cert.pem;
    ssl_certificate_key /etc/ssl/certs/key.pem;

    location / {
      proxy_pass http://app;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-Proto https;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  }
}
```

We will not dig in detail this script in this course section just make sure to replace `yourdomain.com` with your actual domain name, and update the SSL certificate file paths to match your file names and folder locations.


### Refactor our `docker-compose.yml` file 

```yaml title="docker-compose.yml" hl_lines="51 53-71"
version: '3'

services:
  db:
    container_name: db
    image: postgres:latest
    environment:
      POSTGRES_DB: northwind
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    volumes:
      - postgresql_bin:/usr/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
      - ./northwind.sql:/docker-entrypoint-initdb.d/northwind.sql
      - ./files:/files
    ports:
      - 55432:5432
    networks:
      - db

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4
    environment:
      PGADMIN_DEFAULT_EMAIL: pgadmin4@pgadmin.org
      PGADMIN_DEFAULT_PASSWORD: postgres
      PGADMIN_LISTEN_PORT: 5050
      PGADMIN_CONFIG_SERVER_MODE: 'False'
    volumes:
      - postgresql_bin:/usr/lib/postgresql
      - pgadmin_root_prefs:/root/.pgadmin
      - pgadmin_working_dir:/var/lib/pgadmin
      - ./files:/files
    ports:
      - 5050:5050
    networks:
      - db

  app:
    build:
      context: .
      dockerfile: Dockerfile
    environment:
      DB_HOST: db
    depends_on:
      - db
    ports:
      - "8000:8000"
    networks:
      - db
      - mynetwork

  nginx:
    image: nginx:latest
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./certs:/etc/ssl/certs
    networks:
      - mynetwork
    depends_on:
      - app
    restart: always

networks:
  db:
    driver: bridge
  mynetwork:
    driver: bridge


volumes:
  pgadmin_root_prefs:
    driver: local
  pgadmin_working_dir:
    driver: local
  postgresql_data:
    driver: local
  postgresql_bin:
    driver: local
```

Let's talk about the ppart of the docker-compose file defines a service named `nginx` that is based on the `nginx:latest` Docker image. Here is a detailed explanation of each section:

- `image: nginx:latest`: This line specifies the Docker image to use for the nginx service, which is `nginx:latest`.
- `ports`: This section maps the container ports to the host ports. It exposes the container ports `80` and `443` to the host machine.
- `volumes`: This section maps the host directories or files to the container directories or files. Here, it mounts the `nginx.conf` file from the current directory into the container's `/etc/nginx/nginx.conf` path. It also mounts the certs directory from the current directory into the container's `/etc/ssl/certs` path.
- `networks`: This section specifies the networks to which this service is attached. In this case, it is attached to the mynetwork network.
- `depends_on`: This section specifies that the `nginx` service depends on the `app` service to start. This means that the `app` service will be started before the nginx service.
- `restart`: This line specifies that the container should always be restarted if it stops for any reason.

The `networks` section defines two networks: `db` and `mynetwork`. The networks are bridge driver type

Then you can run `docker-compose up -d` and you should see the nginx forwarding our app and all our containers like this : 
```
fe86842a56e1   nginx:latest               "/docker-entrypoint.…"   19 seconds ago   Up 18 seconds           0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp
d05f1ca65c2f   northwind_psql_app         "uvicorn app:app --h…"   19 seconds ago   Up 18 seconds           80/tcp, 0.0.0.0:8000->8000/tcp, :::8000->8000/tcp                                                                  northwind_psql_app_1
51553cc3b247   postgres:latest            "docker-entrypoint.s…"   9 hours ago      Up 19 seconds           0.0.0.0:55432->5432/tcp, :::55432->5432/tcp                                                                        db
a76abdcbf8da   dpage/pgadmin4             "/entrypoint.sh"         9 hours ago      Up 19 seconds           80/tcp, 443/tcp, 0.0.0.0:5050->5050/tcp, :::5050->5050/tcp                                                         pgadmin
```

Congrats you code a multi-container application and is now running in HTTPS 🥳


