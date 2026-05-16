# ecommerce-microservices-backend

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Node.js](https://img.shields.io/badge/node-%3E%3D18-green.svg)](https://nodejs.org/)
[![Kafka](https://img.shields.io/badge/Kafka-event--driven-231F20.svg)](https://kafka.apache.org/)

Built this locally to experiment with event-driven architectures. Pushing the core API gateway and Kafka producer logic here.

It's a backend for an e-commerce platform broken down into 8 microservices: Auth, Product, Order, Payment, Notification, Inventory, Search, and Analytics. The biggest bottleneck in the old synchronous design was the checkout flow — waiting for payment processing and inventory reservation sequentially took too long.

I switched it to an event-driven architecture using Apache Kafka. Now the checkout endpoint just validates the request and drops an order-created event onto a Kafka topic, returning a 202 Accepted immediately. The Payment and Inventory services consume that event asynchronously. This dropped the p95 checkout latency by about 45%.

Also integrated Elasticsearch for full-text product search, getting sub-100ms query response times across a catalog of 500,000+ SKUs. Containerized everything with Docker and orchestrated via Kubernetes on AWS EKS with Horizontal Pod Autoscaling, which kept us at 99.9% uptime during peak traffic events.

You'll need Node.js and a local Kafka broker running (or use Docker Compose) to run this end to end.

```bash
npm install express kafkajs mongoose
node server.js
```
