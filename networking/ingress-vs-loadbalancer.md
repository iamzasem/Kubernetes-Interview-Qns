# Kubernetes Ingress vs LoadBalancer

This project explains the difference between Kubernetes Ingress and LoadBalancer type services in a simple way. It also lists some things Kubernetes cannot do on its own. The goal is to help beginners understand how Kubernetes works in real production environments.

## What is Kubernetes Ingress

Ingress is used to manage external access to services in a Kubernetes cluster. It usually handles HTTP and HTTPS traffic. Ingress allows you to route traffic to different services based on paths or domain names. You only need one external IP or Load Balancer for all your services when using Ingress.

Example:
- `example.com` goes to the frontend
- `example.com/api/` goes to the backend
- `example.com/admin/` goes to the admin dashboard

Ingress is a better choice when you have many services and want to control routing in one place.

## What is Kubernetes LoadBalancer Service

A LoadBalancer service exposes one service to the internet. When you create it, your cloud provider creates a separate load balancer with an external IP. This service type is simple but not good when you have many services, because each one will get a separate load balancer, which can cost more.

Use LoadBalancer when you only have one or two services and need a quick setup.

## Main Differences

| Feature | Ingress | LoadBalancer Service |
|--------|---------|----------------------|
| External IP | One for all services | One per service |
| Routing | Based on domain and path | No smart routing |
| Cost | Low | High if many services |
| HTTPS | Easy to add | Needs manual setup |

## What Kubernetes Cannot Do

Kubernetes is strong in managing containers, but it has some limits. Here are some things it cannot do by itself:

- It does not build Docker images or run CI/CD pipelines
- It does not store data permanently. It needs cloud storage or external volumes
- It does not secure your applications by default. You must set up RBAC, secrets encryption and network policies
- It does not handle full database setup, backup or scaling
- It does not give full monitoring or alerting. You need tools like Prometheus and Grafana
- It does not do cost optimization. You need tools to manage that
- It does not do GitOps or auto deployment from your repository
- It is not the best option for Windows or GPU workloads without extra setup

## When to Use Ingress

- You want to route traffic to many services from one IP
- You need to support HTTPS and custom domains
- You want cleaner and cheaper traffic management

## When to Use LoadBalancer Service

- You only need to expose one service
- You are testing or building a small setup

## Conclusion

Kubernetes is a powerful tool to manage container-based applications. But it works best when combined with other tools for CI/CD, storage, security, monitoring and cost management. Ingress is a good way to manage traffic in production. LoadBalancer services are useful for simple use cases.

