# Custom API Gateway

## Project Overview

This project is a custom API Gateway built using FastAPI to demonstrate how modern backend systems manage authentication, traffic control, and communication between services.

Instead of allowing clients to directly access backend services, all requests pass through a central gateway. The gateway is responsible for validating users, controlling request traffic, routing requests to the correct service, and distributing load across multiple service instances.

The goal of this project was to gain hands-on experience with backend architecture concepts that are commonly used in microservices and distributed systems.

---

## What This Project Does

The gateway acts as the single entry point for all client requests and provides the following functionality:

* JWT-based authentication
* Request rate limiting
* Request routing
* Round-robin load balancing
* Request logging and monitoring

Two independent user services are deployed behind the gateway, allowing requests to be distributed across multiple instances.

---

## System Architecture

```text
Client
   |
   v
+----------------------+
|     API Gateway      |
|----------------------|
| JWT Authentication   |
| Rate Limiting        |
| Request Routing      |
| Load Balancing       |
| Request Logging      |
+----------------------+
      |         |
      |         |
      v         v

User Service 1   User Service 2
Port 8001        Port 8002
```

---

## Key Features

### JWT Authentication

Users must authenticate before accessing protected endpoints.

The gateway generates JWT tokens through a login endpoint. These tokens are then validated on every protected request to ensure that only authorized users can access backend services.

This helped me understand:

* Authentication vs Authorization
* Stateless security
* Token-based access control

---

### Rate Limiting

The gateway tracks incoming requests and limits how many requests a user can make within a fixed time window.

If the limit is exceeded, the gateway responds with:

```text
429 Too Many Requests
```

This feature simulates how production systems protect themselves from abuse, excessive traffic, and denial-of-service attacks.

---

### Request Routing

Instead of exposing backend services directly, all traffic flows through the gateway.

The gateway determines where requests should be sent and forwards them to the correct service.

Benefits include:

* Centralized traffic management
* Simplified client interactions
* Improved security

---

### Round-Robin Load Balancing

To simulate horizontal scaling, two user service instances are deployed.

The gateway distributes requests in a round-robin pattern:

```text
Request 1 → User Service 1
Request 2 → User Service 2
Request 3 → User Service 1
Request 4 → User Service 2
```

This ensures traffic is shared evenly across available service instances.

---

### Request Logging

A middleware layer records:

* Incoming requests
* Response status codes
* Processing times

Logging is a critical part of production systems because it helps developers monitor traffic and troubleshoot issues.

---

## Technologies Used

* Python
* FastAPI
* Uvicorn
* HTTPX
* Python-JOSE (JWT)

---

## Project Structure

```text
custom-api-gateway/
│
├── gateway.py
├── user1.py
├── user2.py
│
├── requirements.txt
│
└── README.md
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/yourusername/custom-api-gateway.git

cd custom-api-gateway
```

Install dependencies:

```bash
pip install fastapi uvicorn httpx python-jose
```

---

## Running the Project

### Terminal 1 – User Service 1

```bash
uvicorn user1:app --host 0.0.0.0 --port 8001
```

### Terminal 2 – User Service 2

```bash
uvicorn user2:app --host 0.0.0.0 --port 8002
```

### Terminal 3 – API Gateway

```bash
uvicorn gateway:app --host 0.0.0.0 --port 8000
```

---

## Testing

### Verify Backend Services

```bash
curl http://localhost:8001/profile
```

```bash
curl http://localhost:8002/profile
```

### Verify Gateway

```bash
curl http://localhost:8000/
```

---

## Authentication Example

Generate a token:

```bash
curl -X POST http://localhost:8000/login
```

Example response:

```json
{
  "access_token": "YOUR_TOKEN"
}
```

Use the token:

```bash
curl \
-H "Authorization: Bearer YOUR_TOKEN" \
http://localhost:8000/users/profile
```

---

## Rate Limiting Test

```bash
for i in {1..12}
do
curl -s \
-H "Authorization: Bearer YOUR_TOKEN" \
http://localhost:8000/users/profile
echo
done
```

After exceeding the limit:

```json
{
  "detail": "Rate Limit Exceeded"
}
```

---

## What I Learned

Building this project helped me understand several important backend engineering concepts:

* API Gateway Architecture
* JWT Authentication
* Middleware Development
* Rate Limiting Strategies
* Request Routing
* Load Balancing
* Service-to-Service Communication
* Distributed Systems Fundamentals
* FastAPI Development

More importantly, it demonstrated how modern applications handle security, scalability, and traffic management using a centralized gateway layer.

---

## Future Enhancements

Possible improvements include:

* Redis-based distributed rate limiting
* Service discovery
* Circuit breakers
* Health checks
* API analytics dashboard
* Distributed logging
* Response caching
* Role-Based Access Control (RBAC)
* Docker containerization
* Kubernetes deployment

---

## Author

**Pratheek Pai**

B.E. Computer Science (AI & ML)

Interested in Backend Engineering, Distributed Systems, AI Applications, and Scalable Software Architecture.

