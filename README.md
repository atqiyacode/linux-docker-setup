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

## Configuration

### MySQL Configuration

```bash
MYSQL_IMAGE_VERSION=latest
MYSQL_CONTAINER_NAME=MySQL
MYSQL_ROOT_PASSWORD=root
MYSQL_USER=user
MYSQL_PASSWORD=password
MYSQL_FORWARD_PORT=3306
MYSQL_DATA_VOLUME=./data/mysql-data
```

### Redis Configuration

```bash
REDIS_IMAGE_VERSION=latest
REDIS_CONTAINER_NAME=Redis
REDIS_PASSWORD=redis-password
REDIS_FORWARD_PORT=6379
REDIS_DATA_VOLUME=./data/redis-data
```

### PostgreSQL Configuration

```bash
POSTGRES_IMAGE_VERSION=latest
POSTGRES_CONTAINER_NAME=PostgreSQL
POSTGRES_USER=postgres-user
POSTGRES_PASSWORD=postgres-password
POSTGRES_FORWARD_PORT=5432
POSTGRES_DATA_VOLUME=./data/postgres-data
```

### MongoDB Configuration

```bash
MONGODB_IMAGE_VERSION=latest
MONGODB_CONTAINER_NAME=MongoDB
MONGODB_ROOT_USERNAME=root
MONGODB_ROOT_PASSWORD=password
MONGODB_FORWARD_PORT=27017
MONGODB_VOLUME=./data/mongodb-data
```

### Mongo Express Configuration

```bash
ME_IMAGE_VERSION=latest
ME_CONTAINER_NAME=MongoExpress
ME_FORWARD_PORT=8081
```

### Mailhog Configuration

```bash
MAILHOG_IMAGE_VERSION=latest
MAILHOG_CONTAINER_NAME=Mailhog
MAILHOG_FORWARD_PORT=587
MAILHOG_CONSOLE_FORWARD_PORT=8025
MAILHOG_VOLUME=./data/mailhog-Maildir
```

### MinIO Configuration

```bash
MINIO_IMAGE_VERSION=latest
MINIO_CONTAINER_NAME=MinIO
MINIO_ROOT_USER=minioadmin
MINIO_ROOT_PASSWORD=miniopassword
MINIO_FORWARD_PORT=9000
MINIO_CONSOLE_FORWARD_PORT=8900
MINIO_DATA_VOLUME=./data/minio-data
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
- Mongo Express: <http://localhost:`${ME_FORWARD_PORT}`> (Default: 8081)
- Mailhog Console: <http://localhost:`${MAILHOG_CONSOLE_FORWARD_PORT}`> (Default: 8025)
- MinIO Console: <http://localhost:`${MINIO_CONSOLE_FORWARD_PORT}`> (Default: 8900)

## Service Overview

## 1. MySQL

The MySQL service is configured with default credentials provided in the .env file. It is mapped to port `${MYSQL_FORWARD_PORT}` and uses a volume for data persistence at `${MYSQL_DATA_VOLUME}`.

## 2. Redis

Redis requires a password, which is specified in the .env file **REDIS_PASSWORD**. The Redis service also includes a health check that pings the server.

## 3. PostgreSQL

PostgreSQL is set up with the user and password defined in the .env file. The database data is persisted in `${POSTGRES_DATA_VOLUME}`.

## 4. MongoDB

MongoDB is running on `${MONGODB_FORWARD_PORT}` and initialized with the root username and password from the .env file. It includes a health check to ensure the service is responsive.

## 5. Mongo Express

Mongo Express provides a web interface for interacting with MongoDB. Access it at <http://localhost:`${ME_FORWARD_PORT}`>.

## 6. Mailhog

Mailhog is a testing email server. You can send emails to it on port `${MAILHOG_FORWARD_PORT}` and view them through the web console on `${MAILHOG_CONSOLE_FORWARD_PORT}`.

## 7. MinIO

MinIO is an object storage service compatible with Amazon S3. The service is running on `${MINIO_FORWARD_PORT}` for API access and `${MINIO_CONSOLE_FORWARD_PORT}` for the management console.

## Health Checks

Health checks have been added for each service to ensure they are running correctly. Docker will automatically restart any unhealthy services based on the defined restart policy.

## Volumes

Persistent data for each service is stored in the following volumes:

```bash
MySQL: `${MYSQL_DATA_VOLUME}`
Redis: `${REDIS_DATA_VOLUME}`
PostgreSQL: `${POSTGRES_DATA_VOLUME}`
MongoDB: `${MONGODB_VOLUME}`
Mailhog: `${MAILHOG_VOLUME}`
MinIO: `${MINIO_DATA_VOLUME}`
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
