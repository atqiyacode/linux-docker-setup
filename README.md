# Docker Compose Setup for Multiple Services

This Docker Compose setup includes several services: MySQL, Redis, PostgreSQL, MongoDB, Mailhog, MinIO, and Mongo Express. The setup also includes health checks and uses environment variables stored in a `.env` file for flexibility and security.

## List

- [Prerequisites](#prerequisites)
- [Clone Repository](#clone-the-repository)
- [Environment Variables](#environment-variables)
- [Build and Run the Containers](#build-and-run-the-containers)
- [Accessing Services](#accessing-services)
- [Service Overview](#service-overview)
  - [1. MySQL](#1-mysql)
  - [2. Redis](#2-redis)
  - [3. PostgreSQL](#3-postgresql)
  - [4. MongoDB](#4-mongodb)
  - [5. Mongo Express](#5-mongo-express)
  - [6. Mailhog](#6-mailhog)
  - [7. MinIO](#7-minio)
  - [8. Zookeeper](#8-zookeeper)
  - [9. Kafka](#9-kafka)
  - [10. Kafka UI](#10-kafka-ui)
  - [11. Debezium](#11-debezium)
  - [12. Keycloak](#12-keycloak)
  - [13. Nginx Proxy Manager](#13-nginx-proxy-manager)
  - [14. Grafana](#14-grafana)
  - [15. Prometheus](#15-prometheus)
- [Health Checks](#health-checks)
- [Volumes](#volumes)
- [Stopping Services](#stopping-services)
- [License](#license)

## Prerequisites

- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Clone the Repository

```bash
git clone https://github.com/your-username/your-repository.git
cd your-repository
```

## Environment Variables

Copy `.env.example` to `.env`:

```bash
cp .env.example .env
```

## Build and Run the Containers

To build and start the containers, run:

```bash
docker-compose up --build -d
```

This will pull the necessary images, build the services, and run them in the background.
Verify the Setup

You can verify that the services are running properly by checking the logs and using health checks:

```bash
docker-compose ps
```

If you want to see detailed logs for a specific service, use:

```bash
docker-compose logs <service-name>
```

For example:

```bash
docker-compose logs mongodb
```

## Accessing Services

- MySQL: localhost:`${MYSQL_FORWARD_PORT}` (Default: 3306)
- Redis: localhost:`${REDIS_FORWARD_PORT}` (Default: 6379)
- PostgreSQL: localhost:`${POSTGRES_FORWARD_PORT}` (Default: 5432)
- MongoDB: localhost:`${MONGODB_FORWARD_PORT}` (Default: 27017)
- Mongo Express: <http://localhost:`${MONGO_EXPRESS_FORWARD_PORT}`> (Default: 8081)
- Mailhog Console: <http://localhost:`${MAILHOG_CONSOLE_FORWARD_PORT}`> (Default: 8025)
- MinIO Console: <http://localhost:`${MINIO_CONSOLE_FORWARD_PORT}`> (Default: 8900)

## Service Overview

## 1. MySQL

The MySQL service is configured with default credentials provided in the .env file. It is mapped to port `${MYSQL_FORWARD_PORT}` and uses a volume for data persistence at `${MYSQL_DATA_VOLUME}`.

```bash
MYSQL_VERSION=8.2
MYSQL_NAME=MYSQL
MYSQL_ROOT_PASSWORD=root-password
MYSQL_USER=username
MYSQL_PASSWORD=password
MYSQL_FORWARD_PORT=3306
MYSQL_DATA_VOLUME=./data/mysql-data
```

## 2. Redis

Redis requires a password, which is specified in the .env file **REDIS_PASSWORD**. The Redis service also includes a health check that pings the server.

```bash
REDIS_VERSION=alpine
REDIS_NAME=REDIS-ALPINE
REDIS_FORWARD_PORT=6379
REDIS_PASSWORD=redis-password
REDIS_DATA_VOLUME=./data/redis-data
```

## 3. PostgreSQL

PostgreSQL is set up with the user and password defined in the .env file. The database data is persisted in `${POSTGRES_DATA_VOLUME}`.

```bash
POSTGRES_VERSION=latest
POSTGRES_NAME=POSTGRESQL
POSTGRES_USER=username
POSTGRES_PASSWORD=password
POSTGRES_FORWARD_PORT=5432
POSTGRES_DATA_VOLUME=./data/postgres-data
```

## 4. MongoDB

MongoDB is running on `${MONGODB_FORWARD_PORT}` and initialized with the root username and password from the .env file. It includes a health check to ensure the service is responsive.

```bash
MONGODB_VERSION=latest
MONGODB_NAME=MongoDB
MONGODB_ROOT_USERNAME=root
MONGODB_ROOT_PASSWORD=password
MONGODB_FORWARD_PORT=27017
MONGODB_VOLUME=./data/mongodb-data
```

## 5. Mongo Express

Mongo Express provides a web interface for interacting with MongoDB. Access it at <http://localhost:`${MONGO_EXPRESS_FORWARD_PORT}`>.

```bash
MONGO_EXPRESS_VERSION=latest
MONGO_EXPRESS_NAME=MONGO-EXPRESS
MONGO_EXPRESS_FORWARD_PORT=8081
MONGO_EXPRESS_DATA_VOLUME=./data/mongo-express-data
MONGO_EXPRESS_CONFIG_MONGODB_ADMINUSERNAME=${MONGODB_ROOT_USERNAME}
MONGO_EXPRESS_CONFIG_MONGODB_ADMINPASSWORD=${MONGODB_ROOT_PASSWORD}
MONGO_EXPRESS_CONFIG_MONGODB_URL=mongodb://${MONGODB_ROOT_USERNAME}:${MONGODB_ROOT_PASSWORD}@mongodb:27017/
MONGO_EXPRESS_BASIC_AUTH_USERNAME=root
MONGO_EXPRESS_BASIC_AUTH_PASSWORD=password
MONGO_EXPRESS_CONFIG_BASICAUTH=true
```

## 6. Mailhog

Mailhog is a testing email server. You can send emails to it on port `${MAILHOG_FORWARD_PORT}` and view them through the web console on `${MAILHOG_CONSOLE_FORWARD_PORT}`.

```bash
MAILHOG_VERSION=latest
MAILHOG_NAME=MAILHOG
MAILHOG_SMTP_PORT=587
MAILHOG_UI_PORT=8025
MAILHOG_DATA_VOLUME=./data/mailhog-data
```

## 7. MinIO

MinIO is an object storage service compatible with Amazon S3. The service is running on `${MINIO_FORWARD_PORT}` for API access and `${MINIO_CONSOLE_FORWARD_PORT}` for the management console.

```bash
MINIO_VERSION=latest
MINIO_NAME=MINIO-S3
MINIO_FORWARD_PORT=9000
MINIO_CONSOLE_FORWARD_PORT=8900
MINIO_ROOT_USER=root
MINIO_ROOT_PASSWORD=password
MINIO_DATA_VOLUME=./data/minio-data
```

## 8. Zookeeper

Write Description.

```bash
ZOOKEEPER_VERSION=latest
ZOOKEEPER_NAME=ZOOKEEPER
ZOOKEEPER_CLIENT_PORT=2181
ZOOKEEPER_TICK_TIME=2000
```

## 9. Kafka

Write Description.

```bash
KAFKA_VERSION=latest
KAFKA_NAME=KAFKA
KAFKA_FORWARD_PORT=9092
KAFKA_BROKER_ID=1
KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1
```

## 10. Kafka UI

Write Description.

```bash
KAFKA_UI_VERSION=latest
KAFKA_UI_NAME=KAFKA-UI
KAFKA_UI_FORWARD_PORT=9090
```

## 11. Debezium

Write Description.

```bash
DEBEZIUM_VERSION=latest
DEBEZIUM_NAME=DEBEZIUM
DEBEZIUM_FORWARD_PORT=8083
```

## 12. Keycloak

Write Description.

```bash
KEYCLOAK_VERSION=24.0
KEYCLOAK_NAME=KEYCLOAK
KEYCLOAK_HTTP_PORT=7080
KEYCLOAK_HTTPS_PORT=7443
KEYCLOAK_ADMIN_USERNAME=admin
KEYCLOAK_ADMIN_PASSWORD=admin
```

## 13. Nginx Proxy Manager

Write Description.

```bash
NGINX_PM_VERSION=latest
NGINX_PM_NAME=NGINX-PROXY-MANAGER
NGINX_PM_HTTP_PORT=80
NGINX_PM_HTTPS_PORT=443
NGINX_PM_ADMIN_WEB_PORT=81
NGINX_PM_FTP_PORT=21
NGINX_PM_DATA_VOLUME=./data/nginx-proxy-manager
NGINX_PM_SSL_DATA_VOLUME=./data/letsencrypt
```

## 14. Grafana

Write Description.

```bash
GF_VERSION=latest
GF_NAME=GRAFANA
GF_FORWARD_PORT=5000
GF_SECURITY_ADMIN_USER=admin
GF_SECURITY_ADMIN_PASSWORD=admin
GF_DATA_VOLUME=./data/grafana-data
```

## 15. Prometheus

Write Description.

```bash
PROMETHEUS_VERSION=latest
PROMETHEUS_NAME=PROMETHEUS
PROMETHEUS_FORWARD_PORT=9090
PROMETHEUS_DATA_VOLUME=./data/prometheus-data
```

## Health Checks

Health checks have been added for each service to ensure they are running correctly. Docker will automatically restart any unhealthy services based on the defined restart policy.

## Volumes

Persistent data for each service is stored in the following volumes:

```bash
"MySQL": `${MYSQL_DATA_VOLUME}`
"Redis": `${REDIS_DATA_VOLUME}`
"PostgreSQL": `${POSTGRES_DATA_VOLUME}`
"MongoDB": `${MONGODB_VOLUME}`
"Mailhog": `${MAILHOG_VOLUME}`
"MinIO": `${MINIO_DATA_VOLUME}`
"MongoDB": `${MONGODB_DATA_VOLUME}`
"Mongo Express": `${MONGO_EXPRESS_DATA_VOLUME}`
```

## Stopping Services

To stop the services, use:

```bash
docker-compose down
```

## To stop and remove all containers, networks, and volumes, use

```bash
docker-compose down -v
```

## License

This project is licensed under the MIT License - see the LICENSE file for details
