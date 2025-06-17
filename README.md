# ecommerce-aks

This project contains all the files you need to deploy our e-commerce microservices app to Azure Kubernetes Service (AKS).

## ⚙️ What’s Inside

We have the following services:

- store-front
- store-admin
- order-service
- product-service
- makeline-service
- mongodb
- rabbitmq

All services are containerized and deployed using Kubernetes.

---

## Steps to Run This Project

### 1. Clone the Repo

```bash
git clone https://github.com/dejmartins/ecommerce-aks.git
cd ecommerce-aks
```

### 2. Create Your Own MongoDB on Azure

> ⚠️ **Important:** You MUST create your own Azure MongoDB (vCore preferred).

Steps:
1. Go to Azure Portal
2. Search: **Azure Cosmos DB for MongoDB**
3. Create a new **vCore** MongoDB instance
4. Copy the connection string

Then, update the Kubernetes deployment files:

- `product-service`: update `PRODUCT_DB_URI`
- `order-service`: update `ORDER_DB_URI`
- `makeline-service`: update `ORDER_DB_URI`

---

### 3. Create AKS Cluster (Via Azure Portal)

> Go to the Azure Portal and create an AKS cluster manually.

Once done, connect to the cluster using:

```bash
az aks get-credentials --resource-group YOUR_RESOURCE_GROUP --name YOUR_AKS_NAME
```

---

### 4. Create a Namespace

Apply the provided namespace YAML:

```bash
kubectl apply -f namespace/dev-namespace.yaml
```

---

### 5. Deploy the Services

Each service folder contains its own `deployment.yaml` and `service.yaml`.

Apply all files in each folder to the correct namespace:

```bash
kubectl apply -f ./makeline-service -n dev
kubectl apply -f ./product-service -n dev
kubectl apply -f ./order-service -n dev
kubectl apply -f ./store-admin -n dev
kubectl apply -f ./store-front -n dev
kubectl apply -f ./rabbitmq -n dev
kubectl apply -f ./mongodb -n dev
```

Make sure the MongoDB connection strings are correctly updated before applying.

---

### 6. Verify Deployment

Check that all pods and services are running:

```bash
kubectl get pods -n dev
kubectl get svc -n dev
```

Look for `EXTERNAL-IP` for the following services:

- `store-front`
- `store-admin`

These should be of type `LoadBalancer`.

---