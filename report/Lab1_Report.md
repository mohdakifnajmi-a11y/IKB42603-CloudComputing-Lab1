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

1. AWS CLI configuration <img width="1636" height="298" alt="image" src="https://github.com/user-attachments/assets/a10dc7de-7510-4200-ba81-57842a183dad" />


2. STS Get Caller Identity <img width="2032" height="222" alt="image" src="https://github.com/user-attachments/assets/90e7c913-68e4-4e8e-a272-ab9b84a23719" />

3. IAM Group creation <img width="1910" height="746" alt="image" src="https://github.com/user-attachments/assets/0eb20fdb-2784-450e-85fc-15c33edc82f2" />

4. Administrator policy attachment <img width="2304" height="370" alt="image" src="https://github.com/user-attachments/assets/7b68ea19-34ac-49b4-8484-d3fc668f0dfb" />

5. CloudAdmin user creation <img width="1412" height="456" alt="image" src="https://github.com/user-attachments/assets/5b59c956-0e22-478e-bb8d-1a63b384ca30" />

6. Analyst user creation <img width="1398" height="892" alt="image" src="https://github.com/user-attachments/assets/7d67e511-221e-480e-8a54-1af25460fa8d" />

7. Access key generation <img width="1280" height="442" alt="WhatsApp Image 2026-07-30 at 20 33 33" src="https://github.com/user-attachments/assets/081cfc47-c1a9-46e5-8eac-3d2f03a20072" />

8. Access key status update <img width="1280" height="436" alt="WhatsApp Image 2026-07-30 at 20 35 47" src="https://github.com/user-attachments/assets/5152acdf-117b-45ab-82a0-d2453bfbb0df" />

9. Kind cluster creation <img width="1400" height="458" alt="image" src="https://github.com/user-attachments/assets/be2ec4ca-b6ac-4be9-9044-58da65ea79f6" />

10. Kubernetes node verification <img width="1388" height="148" alt="image" src="https://github.com/user-attachments/assets/bd728191-8da2-4615-be3a-1302d3b07246" />

11. Namespace creation <img width="1396" height="370" alt="image" src="https://github.com/user-attachments/assets/90327647-17f4-45ad-803a-877ed0b3b03e" />

12. ServiceAccount creation <img width="1384" height="196" alt="image" src="https://github.com/user-attachments/assets/aa2e2301-a9b2-4cb6-8f0e-f5ba8ee38835" />

13. Role creation <img width="1404" height="138" alt="image" src="https://github.com/user-attachments/assets/c6be9fe3-c594-463e-8f9e-bfbdf873a53c" />

14. RoleBinding creation <img width="1414" height="160" alt="image" src="https://github.com/user-attachments/assets/4ec3288a-3298-449e-8876-61ad02d8c0fe" />

15. RBAC verification (yes) <img width="1374" height="188" alt="image" src="https://github.com/user-attachments/assets/4a1f7ad6-6865-4927-9a14-7ec476b10d7e" />

16. RBAC verification (no) <img width="1418" height="180" alt="image" src="https://github.com/user-attachments/assets/b8302e29-b3b8-4321-a808-7403eadf3756" />

