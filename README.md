# Project #3: Cross-Project IAM Identity & Private Network Infrastructure

## 📋 Real-World Operational Scenario
* **The Business Challenge:** An external compliance audit team requires an automated data collector engine to capture cloud asset configurations daily. Enterprise security constraints strictly dictate that this engine must run under a non-human identity, hold zero public internet access pathways to prevent external data scraping, and possess restrictive read-only permissions to prevent infrastructure tampering.
* **The Technical Resolution:** Implemented a dedicated headless Service Account utilizing strict least-privilege IAM bindings. Provisioned an isolated custom-mode VPC framework paired with a regional subnetwork utilizing Private Google Access, allowing secure, internal-only connections directly to Google core APIs.

## ⚡ Flattened One-Liner Execution Commands

```bash
# 1. Initialize active environment project context
export MY_PROJ="project-c1a05de0-ba4b-4764-93a"

# 2. Provision the automated security service identity
gcloud iam service-accounts create deployment-executor --description="Secure deployment runner for isolated zones" --display-name="Deployment Executor" --project=\$MY_PROJ

# 3. Create a clean production-grade custom VPC network base
gcloud compute networks create secure-isolated-vpc --subnet-mode=custom --project=\$MY_PROJ

# 4. Create a secure regional subnet with Private Google Access enabled
gcloud compute networks subnets create secure-subnet --network=secure-isolated-vpc --region=us-central1 --range=10.5.0.0/24 --enable-private-ip-google-access --project=\$MY_PROJ

# 5. Securely bind the service account to the Predefined Compute Viewer role
gcloud projects add-iam-policy-binding \(MY_PROJ --member="serviceAccount:deployment-executor@\)MY_://gserviceaccount.com" --role="roles/compute.viewer"
```

## 🔍 Validation Protocol
Execute this verification command to confirm the IAM policy binding table reads correctly:
```bash
gcloud projects get-iam-policy \$MY_PROJ --flatten="bindings[].members" --format='table(bindings.role, bindings.members)' | grep "deployment-executor"
```
