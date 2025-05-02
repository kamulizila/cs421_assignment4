# CS 421 - Assignment 4: High Availability Frontend with Load Balancing
## Assignment Overview

This assignment implements a high-availability frontend for the university API (from Assignment 3) with NGINX load balancing across three frontend instances. The system displays student and course data while indicating which frontend node served each request.

**Docker Hub:** [https://hub.docker.com/u/kamulizila](https://hub.docker.com/u/kamulizila) 
 
**GitHub Repository:** [https://github.com/kamulizila/cs421_assignment4](https://github.com/kamulizila/cs421_assignment4)
  
**Live Deployment:** [http://ec2-52-87-172-176.compute-1.amazonaws.com/](http://ec2-52-87-172-176.compute-1.amazonaws.com/)  

## Client → NGINX Load Balancer → [Frontend1, Frontend2, Frontend3] → API → PostgreSQL

## Prerequisites

- Docker Engine
- Docker Compose
- AWS EC2 Ubuntu instance (t2.micro or t3.micro)

## Setup Instructions

### 1. Clone the Repository

git clone https://github.com/kamulizila/cs421_assignment4.git

cd cs421_assignment4

## Build and Run Containers

docker-compose up --build -d

## Access the Application

Frontend: http://ec2-52-87-172-176.compute-1.amazonaws.com/

API Endpoints:

Students: /api/students/

Subjects: /api/subjects/

## Load Balancer Configuration

NGINX is configured with:

Round-robin algorithm

Health checks every 5 seconds

Custom X-Node-ID header to identify serving nodes

Configuration file: load-balancer/nginx.conf

## Frontend Features

Homepage with:

"Students" and "Courses" buttons

Responding node indicator ("Frontend Node1,Frontend Node2,Frontend Node3")

Student List showing:

Student names

Enrolled programs

Course List organized by academic year


## Docker Images

All images are available on Docker Hub:

Frontend: kamulizila/cs421-frontend

API: kamulizila/cs421-api

Load Balancer: kamulizila/cs421-loadbalancer

Database: postgres:13

## Testing Procedures

Round-robin Verification:

Refresh the homepage multiple times

Observe the changing "Responding Node" indicator

## Troubleshooting

Common Issues:

Frontend not loading:

Check container status: docker ps

View logs: docker logs <container_name>

Node ID not displaying:

Verify headers: curl -I http://localhost

Check nginx config for X-Node-ID header

API connection errors:

Verify API container is running

Check network connectivity between containers

# CS 421_assignment

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vite.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
## 🛠️ Setup Instructions

### Prerequisites
- Docker Engine 20.10+
- Docker Compose 2.12+
- AWS EC2 Ubuntu 22.04 LTS (t2.micro)

### Deployment Steps

1. **Clone the repository**:
   
   git clone https://github.com/kamulizila/cs421_assignment4.git
   
   cd cs421_assignment4
