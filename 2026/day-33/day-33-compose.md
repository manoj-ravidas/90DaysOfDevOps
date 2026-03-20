# Day 33 – Docker Compose

## What is Docker Compose
Docker Compose allows running multi-container applications using a single YAML file.

## Key Benefits
- Single command to run multiple containers
- Automatic network creation
- Easy service communication

## First Compose Example
Ran Nginx container using docker-compose.yml.

## WordPress + MySQL Setup
- Two containers connected automatically
- Used named volume for database persistence
- WordPress connected using service name "db"

## Important Commands

docker compose up -d

docker compose down

docker compose ps

docker compose logs -f

docker compose stop

docker compose up -d --build

## Environment Variables
Used .env file to store sensitive data.

## Key Learning
Compose simplifies multi-container setup and networking.
