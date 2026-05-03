
# 🚀 Kubernetes Policy Enforcement using OPA Gatekeeper (Admission Controller)

## 📌 Project Overview

This project demonstrates the implementation of **OPA Gatekeeper in Kubernetes** to enforce security and compliance policies at the cluster level.

Gatekeeper acts as a **policy enforcement layer (Admission Controller)** that validates every request before it is accepted by the Kubernetes API Server. It ensures that only **secure, compliant, and well-defined resources** are deployed in the cluster.

This project focuses on:

* Preventing insecure configurations
* Enforcing best practices
* Validating policies through real-world test cases

---

## 🧠 S – Situation

In a default Kubernetes cluster, users can deploy anything:

* Containers running as root ❌
* No resource limits ❌
* Images from untrusted registries ❌
* Missing labels ❌

This creates **serious security, compliance, and cost risks**.

To address this, I implemented a **policy enforcement mechanism** to control what gets deployed in the cluster.

---

## 🎯 T – Task

My objective was to:

* Install **OPA Gatekeeper** in the Kubernetes cluster
* Define and enforce security policies
* Block non-compliant resources
* Validate policies through testing
* Monitor violations via logs and events

---

## ⚙️ A – Action

I implemented a full policy enforcement workflow:

### 🔹 Installed Gatekeeper

```bash
kubectl apply -f gatekeeper.yaml
```

---

### 🔹 Configured Policies

Created policies using:

* **ConstraintTemplate** → Defines rule logic
* **Constraint** → Applies rule to resources

### 🔐 Policies Implemented:

* Only allow approved container images
* Mandatory labels (team, env)
* Deny privileged containers
* Enforce CPU & memory limits

---

### 🔹 Tested Policy Enforcement

Performed multiple test cases:

* ❌ Deploy invalid YAML → Blocked
* ⚠️ Partial compliance → Rejected with error
* ✅ Valid configuration → Allowed

---

### 🔹 Monitored Logs & Events

```bash
kubectl get events -A
kubectl logs -n gatekeeper-system -l control-plane=controller-manager
```

Also validated:

* Audit violations
* Admission decisions
* Cluster-wide policy impact

---

## 📊 R – Result

The implementation successfully achieved:

* 🚫 Blocking of insecure configurations
* ✅ Enforcement of Kubernetes best practices
* 📊 Visibility into policy violations
* 🔍 Audit of existing resources

This project strengthened my understanding of:

* Kubernetes admission controllers
* Policy-as-Code using OPA
* Cluster security governance

---

## 🏗️ Architecture Overview

```
User → kubectl apply
        ↓
Kubernetes API Server
        ↓
OPA Gatekeeper (Admission Controller)
        ↓
Policy Check (Constraint + Template)
        ↓
❌ Deny → Error + Event + Logs
✅ Allow → Resource Created in Cluster
```

---

## 🔍 Key Insight

* Gatekeeper works at **admission level (before deployment)**
* Prevents bad configs **before they reach cluster**
* Provides both:

  * **Enforcement (deny requests)**
  * **Audit (flag existing resources)**

---

## 🔧 Environment Setup

### 1️⃣ Verify Access

```bash
kubectl get nodes
```

---

### 2️⃣ Verify Gatekeeper Installation

```bash
kubectl get pods -n gatekeeper-system
```

Expected:

* `gatekeeper-controller-manager`
* `gatekeeper-audit`

---

### 3️⃣ Check Policies

```bash
kubectl get constrainttemplates
kubectl get constraints
```

---

### 4️⃣ Inspect Policy

```bash
kubectl describe k8srequiredlabels require-team-label
```

---

## 🧪 Testing Scenarios

### ❌ Test Case 1: Missing Labels

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: test-ns
```

👉 Expected:

* ❌ Blocked
* Error: missing labels

---

### ⚠️ Test Case 2: Partial Labels

```yaml
labels:
  team: dev
```

👉 Expected:

* ❌ Blocked
* Error: missing env

---

### ✅ Test Case 3: Valid Resource

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: good-ns
  labels:
    team: dev
    env: prod
```

👉 Expected:

* ✅ Successfully created

---

## 📊 Validation Method

| Result            | Meaning                         |
| ----------------- | ------------------------------- |
| Denied            | Policy working                  |
| Allowed           | Valid configuration             |
| Violation (audit) | Existing non-compliant resource |

---

## 📘 What I Learned

* How admission controllers work in Kubernetes
* Policy enforcement using OPA Gatekeeper
* Writing ConstraintTemplates & Constraints
* Difference between enforcement vs audit
* Debugging policy violations

---

## 🔐 Security Insights

* Preventing issues is better than detecting later
* Policy-as-Code ensures consistency
* Gatekeeper enforces **shift-left security**
* Centralized policy control improves governance

---

## 📚 Key Learnings

* Deep understanding of Kubernetes security controls
* Hands-on experience with OPA Gatekeeper
* Real-world policy validation scenarios
* Troubleshooting denied requests

---

## 🧠 Use Cases

* Enterprise Kubernetes governance
* DevSecOps policy enforcement
* Multi-team cluster management
* Compliance (SOC2, ISO, etc.)

---

## 🚀 Future Enhancements

* Integrate with CI/CD pipelines
* Add mutation policies
* Implement policy versioning
* Integrate with SIEM tools
* Use Rego for advanced policies

---

## 🧑‍💻 Author

**Nipun Bhardwaj**
DevSecOps & Cloud Enthusiast

📌 GitHub: [https://github.com/nipun-10](https://github.com/nipun-10)

---

⭐ If you found this project helpful, consider giving it a star!

---
