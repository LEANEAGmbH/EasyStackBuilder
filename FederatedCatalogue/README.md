[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](../LICENSE)

# Federated Catalogue

An automated orchestration workspace that deploys a [Federated Catalogue](https://github.com/eclipse-xfsc/federated-catalogue) instance to a Kubernetes cluster.

---

## 🚀 Overview

The Federated Catalogue (CAT) is a core component of the Gaia-X ecosystem that enables discovery and access to resources, assets, and participants through their Self-Descriptions. These Self-Descriptions, written in JSON-LD, are either stored as raw documents or integrated into a graph that supports advanced cross-entity queries. The goal is to empower users to find the most relevant services and monitor their evolution over time.

Key components of the Federated Catalogue include:
- Self-Description Storage and Lifecycle
- Self-Description Graph
- REST Interface
- Self-Description Verification
- Schema Management

This module allows you to set up and interact with the Federated Catalogue visually inside the ORCE environment. You don’t need to write any code or handle any complex API integration manually—just install the Node-RED node for Federated Catalogue, drop it into your flow, and configure the endpoint and query.

Thanks to ORCE’s orchestration features, deploying a Federated Catalogue instance and querying it happens in just a few clicks. Upload your configs, drag your node, and start querying the Gaia-X catalogue ecosystem—all inside your Node-RED UI.

---

## 🛠️ How to Use

### 1. Prepare the environment and prerequisites
You'll need:
1.1. A Kubernetes cluster to host the child instances
1.2. A local ORCE as the parent to host the initial developing environment

### 1.1. Kubernetes
Requires a working Kubernetes cluster with ingress installed:
```bash
export KUBECONFIG=`<YOUR KUBECONFIG PATH>`
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.3/deploy/static/provider/cloud/deploy.yaml
```

### 1.2. Local ORCE
Install ORCE as described in the [ORCE page](https://github.com/eclipse-xfsc/orchestration-engine):
```bash
docker run -d --name xfsc-orce-instance -p 1880:1880 leanea/xfsc-orce:1.0.8
```
Go to [http://localhost:1880](http://localhost:1880).

### Install Federated Catalogue Node
Click on "New Node" in the sidebar.

![new button](./docImages/add-new-node.jpg?raw=true)

Upload `node-red-leanea-federated-catalogue-1.0.0.tgz` from this repository and install. Refresh Node-RED to activate the node.

### 2. Create your flow
Drag in an Inject node, the **Federated Catalogue** node, and a Debug node. Connect them:

![step one (flow)](https://github.com/LEANEAGmbH/magic-module-builder/blob/main/ORCE/docImages/photo_2_2025-06-12_15-30-18.jpg?raw=true)

### 3. Configure the Federated Catalogue Node
Double-click to configure:
- Set **Catalogue API URL** (e.g., `https://your-catalogue-endpoint.com`) 
- Add optional **Query Parameters**
- Add **Authorization** if needed

The response will be returned in `msg.payload`.

### 4. Deploy and Trigger
Click **Done** and then **Deploy**. Activate the Inject node.

You should see JSON output in the Debug panel, showing catalogue entries.

---

## ⚙️ Configuration

Before running:

1. **Catalogue URL**  
   Set the URL of your federated catalogue instance.

2. **Query Parameters**  
   Provide any filters or search strings in the node editor or in `msg.payload`.

3. **Authorization Token (optional)**  
   Some catalogue endpoints require auth headers (Bearer token).

---

## 📁 Directory Contents
```
.
├── node-red-leanea-federated-catalogue-1.0.0.tgz
├── FederatedCatalogue.html
├── FederatedCatalogue.js
├── package.json
```

- **node-red-leanea-federated-catalogue-1.0.0.tgz**  
  Installable node package.

- **FederatedCatalogue.html**  
  Node-RED UI form.

- **FederatedCatalogue.js**  
  Backend logic to send API requests and return results.

- **package.json**  
  Metadata and dependencies.

---

## 📦 Dependencies

```json
"node": ">=14.0.0",
"node-red": ">=3.0.0"
```

---

## 🔗 Links & References

- [Federated Catalogue - XFSC](https://github.com/eclipse-xfsc/federated-catalogue)
- [Node-RED](https://nodered.org/)

---

## 📝 License

This project is licensed under the Apache License 2.0. See the [LICENSE](../LICENSE) file for details.
