# 📘 EasyStack Builder – Technical Overview Document

## 📑 Table of Contents

- 📘 EasyStack Builder – Technical Overview Document
- ❓ What is EasyStack Builder?
- 💡 Core Philosophy
  - 🛠️ Before EasyStack Builder
  - 🚀 After EasyStack Builder
- 🏗️ System Architecture
- 🌟 Why It Matters
- 🗑️ Uninstallation Support
- 🔮 What’s Next
- 📝 Summary
- License

## ❓ What is EasyStack Builder?

EasyStack Builder is a suite of deployment modules built directly into ORCE, our visual orchestration engine. At runtime, these modules are registered as custom nodes within ORCE’s internal registry and appear as drag-and-drop components inside the orchestration flow editor. When a user adds a module to a flow and deploys it, ORCE executes the module's backend logic asynchronously. Each module listens for input events, processes parameters passed through the flow, and returns structured output (like deployment results or error messages) back to ORCE using standard flow message conventions. This allows seamless integration with other logic, chaining, or decision-making steps in the pipeline.

These modules are designed to automate the full deployment process of federated cloud services such as:
- Federated Catalogue
- AA (Authentication Authority)
- OCM (Organization Credential Manager)
- PCM Cloud
- TSA E1
- SD Wizard

## 💡 Core Philosophy

**One visual node = one full-stack deployment.**

The goal is to make federated infrastructure accessible to all contributors — technical and non-technical — by reducing the barrier of Kubernetes and identity management operations.

All the complexity of Helm, Kubernetes, Keycloak, ingress, TLS, and namespace setup is wrapped into a single EasyStack Builder module. In a traditional manual deployment, users would need to:
- Write or adapt Helm chart values
- Create Kubernetes namespaces
- Configure ingress controllers and obtain external IPs
- Generate and store TLS secrets securely
- Set up Keycloak realms, clients, and users via its REST API or GUI

### 🛠️ Before EasyStack Builder

```bash
kubectl create ns fed-cat-demo
helm install fc-service . -n fed-cat-demo
kubectl create secret tls certificates ...
# Set up Keycloak: series of curl calls or web admin steps to create client, user, assign roles
```

### 🚀 After EasyStack Builder

1. Upload kubeconfig and certs in the UI, then click **Deploy**.
2. EasyStack Builder validates inputs, executes Helm deployments, configures secrets and ingress, prepares Keycloak resources, and returns URLs and credentials into the flow.

## 🏗️ System Architecture

1. **Execution Context: ORCE**  
   ORCE is a Node.js-based visual orchestrator that runs our flows.

2. **Frontend UI (in ORCE)**  
   Each module exposes a rich form-based UI with configuration tabs for Instance, Deployment, Domain, Credentials, and Information. Users upload kubeconfig and TLS certificates, specify instance names and domains, provide Keycloak admin credentials, and receive real-time validation.

3. **Backend Logic**  
   - Written in Node.js with a Node-RED-compatible handler.  
   - On **Deploy**, ORCE serializes parameters and invokes the module’s runtime.  
   - Uses `tmp` to generate secure temporary files and constructs a shell command to run `deploy.sh`.  
   - Executes cluster operations (Helm, kubectl, curl, jq) and parses stdout to return results to the flow.

4. **Kubernetes Interactions**  
   - Checks for `ingress-nginx`.  
   - Creates namespaces dynamically.  
   - Installs Helm charts.  
   - Applies TLS secrets and configures custom services.  
   - Waits for readiness and HTTP availability.

5. **Keycloak Integration**  
   - Authenticates and fetches an admin token via REST.  
   - Creates or ensures the realm (`gaia-x`).  
   - Generates service clients and client secrets.  
   - Creates users and assigns roles automatically.  
   - Disables default password policies for integration ease.

6. **Output Returned to ORCE**  
   - External IP of ingress  
   - Service URLs (e.g., fc-service)  
   - Keycloak realm URL  
   - Generated client secrets

## 🌟 Why It Matters

- Simplifies 30+ CLI steps into a single action  
- Enables non-DevOps users to provision production-level services  
- Ensures repeatability across environments (dev, staging, prod)  
- Eliminates the need for Helm/YAML/K8s/Keycloak expertise

## 🗑️ Uninstallation Support

EasyStack Builder modules also support automated uninstallation via:
- A dedicated script (`uninstall.sh`)  
- Kubernetes namespace cleanup  
- Removal of Helm releases, services, and secrets

## 🔮 What’s Next

- Builder modules for OCM, TSA, PCM, and AA are in active development  
- Version 2 will support deployment chaining, multi-cluster logic, and error recovery  
- Integration with AI-based FAP Builder will enable dynamic module composition and self-optimizing infrastructure graphs

## 📝 Summary

EasyStack Builder turns ORCE into a low-code cloud deployment engine, bridging the gap between infrastructure complexity and user simplicity. It lets anyone deploy trusted, Gaia-X compliant services in minutes.

## License

This project is licensed under the Apache License 2.0.  
See the [LICENSE](../LICENSE) file for details.
