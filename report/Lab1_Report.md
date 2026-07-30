# IKB42603 Cloud Computing Security Essentials

# Lab 1 Report

## Student Information

**Name:** Muhammad Akif Najmi bin Shamsul Bahar

---

# Title

AWS IAM and Kubernetes RBAC using LocalStack and Kind

---

# Objective

The objectives of this lab are:

- Configure AWS CLI to communicate with LocalStack.
- Create IAM groups and users.
- Assign IAM policies to users and groups.
- Generate and manage IAM access keys.
- Deploy a Kubernetes cluster using Kind.
- Configure Kubernetes namespaces.
- Implement Role-Based Access Control (RBAC).
- Verify namespace-specific permissions.

---

# Software and Tools

- Ubuntu (WSL)
- Docker Desktop
- LocalStack v3.8.1
- AWS CLI
- Kubernetes CLI (kubectl)
- Kind
- Git
- GitHub

---

# Part A – AWS IAM using LocalStack

## Step 1 – Configure AWS CLI

Configured AWS CLI using LocalStack endpoint and test credentials.

### Verification

Executed:

```bash
aws --endpoint-url=$EP sts get-caller-identity
```

Result:

- Successfully connected to LocalStack.
- Returned a dummy AWS Account ID.
- Confirmed LocalStack IAM service was operational.

---

## Step 2 – Create IAM Group

Created an administrator group named:

```
Admins
```

Verified the group creation using:

```bash
aws iam get-group
```

---

## Step 3 – Attach Administrator Policy

Attached the AWS managed AdministratorAccess policy to the Admins group.

Verification confirmed the policy attachment was successful.

---

## Step 4 – Create Administrator User

Created administrator user:

```
CloudAdmin_Akif
```

Added the user into the Admins group.

Verification showed the user was successfully associated with the group.

---

## Step 5 – Create Least Privilege User

Created another IAM user:

```
Analyst_Akif
```

Attached the following policy:

```
AmazonS3ReadOnlyAccess
```

Verification confirmed only the S3 read-only policy was attached.

---

## Step 6 – Generate Access Keys

Generated an access key for CloudAdmin_Akif.

Verified the key status.

Updated the access key status from:

```
Active
```

to

```
Inactive
```

Verification confirmed the status update.

---

# Part B – Kubernetes RBAC

## Step 1 – Create Kubernetes Cluster

Created a Kind Kubernetes cluster.

Verification:

```bash
kubectl get nodes
```

The control plane node was successfully created and reached the Ready state.

---

## Step 2 – Create Namespaces

Created two namespaces:

- dev
- prod

Verified using:

```bash
kubectl get namespaces
```

---

## Step 3 – Create Service Account

Created ServiceAccount:

```
developer
```

inside the dev namespace.

Verification:

```bash
kubectl get serviceaccounts -n dev
```

---

## Step 4 – Create RBAC Role

Created Role:

```
developer-role
```

Permissions granted:

- get
- list
- watch
- create
- update
- delete

Resources:

- Pods

Applied using:

```bash
kubectl apply -f developer-role.yaml
```

Verification confirmed successful creation.

---

## Step 5 – Create RoleBinding

Created RoleBinding:

```
developer-binding
```

Bound the developer ServiceAccount to developer-role.

Applied using:

```bash
kubectl apply -f developer-rolebinding.yaml
```

Verification confirmed successful creation.

---

## Step 6 – RBAC Verification

### Test 1

Command:

```bash
kubectl auth can-i get pods \
--as=system:serviceaccount:dev:developer \
-n dev
```

Result:

```
yes
```

---

### Test 2

Command:

```bash
kubectl auth can-i delete pods \
--as=system:serviceaccount:dev:developer \
-n dev
```

Result:

```
yes
```

---

### Test 3

Command:

```bash
kubectl auth can-i get pods \
--as=system:serviceaccount:dev:developer \
-n prod
```

Result:

```
no
```

The developer ServiceAccount only has permissions within the **dev** namespace.

---

# Discussion

This lab demonstrated practical Identity and Access Management (IAM) using LocalStack as an AWS emulator and Role-Based Access Control (RBAC) using Kubernetes. IAM users, groups, policies, and access keys were successfully configured. Kubernetes RBAC was implemented to enforce namespace-level permissions. The verification results confirmed that the developer ServiceAccount could manage resources within the dev namespace but was denied access to resources in the prod namespace, demonstrating the Principle of Least Privilege.

---

# Conclusion

This lab successfully demonstrated the implementation of AWS IAM using LocalStack and Kubernetes RBAC using Kind. The objectives of creating IAM identities, assigning policies, generating access keys, configuring Kubernetes roles, and verifying namespace-based access control were achieved. The exercise provided practical experience in implementing secure access management techniques commonly used in cloud environments.

---

# Screenshots

Insert the following screenshots:

1. AWS CLI configuration
2. STS Get Caller Identity
3. IAM Group creation
4. Administrator policy attachment
5. CloudAdmin user creation
6. Analyst user creation
7. Access key generation
8. Access key status update
9. Kind cluster creation
10. Kubernetes node verification
11. Namespace creation
12. ServiceAccount creation
13. Role creation
14. RoleBinding creation
15. RBAC verification (yes)
16. RBAC verification (no)
