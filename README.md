# Zero Trust Authentication using Kubernetes, Istio, Keycloak, and OPA

## 🔒 Overview

This project demonstrates a practical implementation of **Zero Trust Architecture (ZTA)** in a cloud-native environment using:

- **Kubernetes** for container orchestration
- **Istio** for service mesh and mutual TLS
- **Keycloak** for identity and access management (OIDC-based)
- **Open Policy Agent (OPA)** for fine-grained policy enforcement

It showcases how to secure inter-service communication, authenticate users, and authorize access using modern Zero Trust principles.

---

## 🚀 Features

- ✅ Kubernetes-based microservices architecture
- 🔐 Mutual TLS between services using Istio
- 👤 User authentication with Keycloak via OpenID Connect (OIDC)
- 📜 Policy-based authorization with OPA (Rego policies)
- 🔄 Centralized identity and access management
- 📦 Easily deployable using `kubectl`, `istioctl`, and `helm`

---

## 🧱 Architecture

```
[ User ] --> [ Istio Ingress Gateway ] --> [ Service A ] <--> [ Service B ]
     |                        |                    |
     ↓                        ↓                    ↓
[ Keycloak (OIDC) ]     [ Istio mTLS ]        [ OPA Authorization ]
```

---

## 🛠️ Technologies Used

- **Kubernetes** (v1.28+)
- **Istio** (v1.20+)
- **Keycloak** (v23+)
- **OPA/Gatekeeper** (latest)
- **Helm**, `kubectl`, `istioctl`

---

## 🧪 Project Setup

### 1. Prerequisites

- Minikube or any Kubernetes cluster
- Helm
- Istio CLI (`istioctl`)
- kubectl

### 2. Install Istio

```bash
istioctl install --set profile=demo -y
kubectl label namespace default istio-injection=enabled
```

### 3. Deploy Keycloak

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install keycloak bitnami/keycloak --set auth.adminPassword=adminpassword
```

Access Keycloak at: `http://localhost:8080` (port-forward or NodePort)

### 4. Enable mTLS

```yaml
# peer-authentication.yaml
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: default
spec:
  mtls:
    mode: STRICT
```

```bash
kubectl apply -f peer-authentication.yaml
```

### 5. Deploy Example Services

Deploy a couple of simple services (e.g., `httpbin` or custom app) with sidecars injected.

### 6. Configure OPA

- Deploy OPA as a sidecar or external service.
- Write and apply custom Rego policies to enforce access rules.

---

## 🔐 Keycloak Configuration

- Create a realm and client for your service
- Enable OIDC and generate client credentials
- Configure Istio's `RequestAuthentication` and `AuthorizationPolicy` for JWT validation

---

## 📄 Example Policy (OPA)

```rego
package http.authz

default allow = false

allow {
    input.method = "GET"
    input.path = [ "service-a", "data" ]
    input.user == "alice@example.com"
}
```

---

## 📊 Use Cases

- Cloud-native application security
- Secure microservice-to-microservice communication
- Policy-driven access control in Kubernetes

---

## 📌 Future Improvements

- Add support for fine-grained RBAC via Keycloak roles
- Automate setup using Helm charts or Skaffold
- Expand demo to multiple namespaces and services

---

## 🧑‍💻 Author
Ajmeera Srikanth  
IITR  
Project for academic submission on **Zero Trust Security in Kubernetes**

---
