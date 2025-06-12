[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](../LICENSE)

# ORCE

An automated orchestration workspace that deploys an [ORCE](https://github.com/eclipse-xfsc/orchestration-engine)  instance to a Kubernetes cluster.

---

## 🚀 Overview

ORCE is a streamlined orchestration workspace that provisions and tears down your ORCE runtime on any compliant Kubernetes cluster with zero manual steps. It abstracts away environment setup—simply upload your kubeconfig, TLS credentials, and desired domain address—and then leverages the included deploy.sh and uninstall.sh scripts through a custom Node-RED node to automate resource provisioning, certificate management, and cleanup. A built-in static HTML dashboard surfaces real‐time deployment status, logs, and rollback controls, so you can focus on designing your flows and integrating XFSC modules rather than wrestling with Kubernetes manifests or shell commands.

Whether you need to support different scenarios utilizing XFSC modules, the fastest and easiest way to manage your working environment is to orchestrate and develop complex architectures in no- or low-code. ORCE’s drag-and-drop interface and prebuilt building blocks let you compose, deploy, and iterate on multi-step workflows seamlessly—adapting on the fly to new requirements. By uniting full automation, a graphical interface, and script-driven deployment logic, ORCE accelerates development and simplifies the management of sophisticated orchestration scenarios.

---

## 🛠️ How to Use
### 1. Prepare the environment and prerequisites
You'll need:
1.1. A Kubernetes cluster to host the child instances
1.2. A local ORCE as the parent to host the initial developing environment
### 1.1. Kubernetes
"Orchestration Engine" node requires a working Kubernetes cluster with ingress installed on it. Initiate a K8s cluster and install nginx-ingress on it using this command.
```bash
export KUBECONFIG=`<YOUR KUBECONFIG PATH>`
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.12.3/deploy/static/provider/cloud/deploy.yaml
```
After this step, you can proceed to step 1.2 (Installing a local ORCE)
### 1.2. Local ORCE
As you can read in [ORCE page](https://github.com/eclipse-xfsc/orchestration-engine), you can install it on your local machine with this command:
```bash
docker run -d --name xfsc-orce-instance -p 1880:1880 leanea/xfsc-orce:1.0.8  # ORCE 1.0.8 latest as of June, 2025.
```
After pulling and deploying the image, you can go to [http://localhost:1880](http://localhost:1880) to access your local Orchestration Engine. Now you have to install "Orchestration Engine" node. To do so, you have to click on "New Node" button in the left sidebar as shown here.

![new button](./docImages/photo_2025-06-12_18-16-42.jpg?raw=true)

Then, in the new window upload the node package (`node-red-leanea-orce-2.0.0.tgz` in this repository) and install it. Refresh the page and if everything is done correctly and without errors you can proceed to step2 (creating your flow).
### 2. Create your flow
Drag and drop inject, Orchestration Engine and a debug node. Connect them like below so Orchestration Engine can be triggered by the inject node.
![step one (flow)](./docImages/photo_2_2025-06-12_15-30-18.jpg?raw=true)
### 3. Name your instance and choose authentication method
Enter your instance name. This name is going to be the suburl of the instance destination. For example, if we name an instance `leanea`, the final url of the instance is going to be `www.example.com/leanea`.
![step two (instance tab)](./docImages/photo_7_2025-06-12_15-30-18.jpg?raw=true)
### 4. Choose deployment type and supply necessary credentials
In this tab you can select deployment type. Docker is not available as of June 12th, 2025. You also have to supply the kubeconfig file of the destination cluster.
![step three (deployment type)](./docImages/photo_5_2025-06-12_15-30-18.jpg?raw=true)
### 5. Provide your desired domain address and supply TLS credentials
![step four (domain)](./docImages/photo_8_2025-06-12_15-30-18.jpg?raw=true)
### 6. You can see the destination URL in Information tab
After you have entered everything, you can see the final path of the instance as if it's deployed.
![step five (information)](./docImages/photo_1_2025-06-12_15-30-18.jpg?raw=true)
### 7. Click done and then deploy
Click done on top right of the editor and then click on Save&Deploy in top right of the page.
Then you have to trigger Orchestration Engine via activating the inject node in this scenario.
![step six (deploy)](./docImages/photo_4_2025-06-12_15-30-18.jpg?raw=true)
### 8. Your instance is up!
After a few seconds of waiting, when the status under the node is changed to "deployed" you can access the instance.
![step seven (instance is up)](./docImages/photo_3_2025-06-12_15-30-18.jpg?raw=true)

- ***Instance Removal:*** Click the trash icon next to your ORCE instance node, then click Deploy in the upper right. You should see your instance (in this case, `xfsc-orce-leanea`) getting terminated in ~1min.
![instance termination](./docImages/terminating.jpg?raw=true)

---

## ⚙️ Configuration

Before you deploy, you’ll need to provide:

1. **Kubeconfig**  
   Upload your Kubernetes configuration.

2. **TLS Credentials**  
   Place your destination cluster’s certificate (`.crt`) and private key (`.key`) in the node UI.

---

## 📁 Directory Contents
```
.
├── node-red-leanea-orce-2.0.0.tgz
├── deploy.sh
├── ORCE.html
├── ORCE.js
├── package.json
└── uninstall.sh
```

- **deploy.sh**  
  This file is responsible of executing deployment commands. It creates the namespace, serviceaccount, clusterrole, clusterrolebinding, configmap, pods, service, adds TLS secrets and finally installs namespaced ingress. (The ingress shares a single IP with other instances of this kind in an IngressClass called nginx-orce-cluster.)

- **uninstall.sh**  
  Safely removes the ORCE instance and cleans up all associated Kubernetes resources. It cleans up everything that `deploy.sh` has created in the process of initiating.

- **package.json**  
  Defines ORCE’s Node.js dependencies and versioning.

- **ORCE.js**  
  Entry point for ORCE’s orchestration logic—handles installation parameters and rollbacks. The node's back-end.

- **ORCE.html**  
  A static dashboard for monitoring ORCE’s deployment status and logs. The node's front-end.

- **node-red-leanea-orce-2.0.0.tgz**  
  Latest version of ORCE node.

---

## 📦 Dependencies

```json
"node"    : ">=14.0.0",
"node-red": ">=3.0.0",
"tmp"     : "^0.2.1"
```

---

## 🔗 Links & References

- [XFSC ORCE](https://github.com/eclipse-xfsc/orchestration-engine)  

---

## License

This project is licensed under the Apache License 2.0.  
See the [LICENSE](../LICENSE) file for details.
