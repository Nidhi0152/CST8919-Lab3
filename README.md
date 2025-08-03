# Cloud Governance Gone Rogue – Azure Policy Lab

##  Lab Summary
This lab focused on enforcing governance and compliance in Azure using **custom Azure Policies**. At MapleTech Solutions, I was tasked with restricting deployments to Canada, enforcing tagging, and preventing exposure through public IPs. I created three custom policies, grouped them into a policy initiative, and tested them through various deployment scenarios.

---

##  Policy Definitions and Code

### 1. **Only-CanadaCentral**
- **Name**: `Only-CanadaCentral`
- **Effect**: Deny
- **Purpose**: Deny creation of resources in any region other than Canada Central.

```json
{
  "properties": {
    "displayName": "Only-CanadaCentral",
    "policyType": "Custom",
    "mode": "All",
    "description": "Denies any resource that is not being deployed in the Canada Central region.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "equals": "canadacentral"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```
### 2. **Require-ProjectName-Tag**
- **Name**: `Require-ProjectName-Tag`
- **Effect**: Deny
- **Purpose**: Deny creation of resources in any region other than Canada Central.

```json
{
  "properties": {
    "displayName": "Require-ProjectName-Tag",
    "policyType": "Custom",
    "mode": "All",
    "description": "Requires all resources to include a ProjectName tag.",
    "policyRule": {
      "if": {
        "field": "[concat('tags[', 'ProjectName', ']')]",
        "exists": "false"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```
### 3. **Deny-Public-IP**
- **Name**: `Deny-Public-IPg`
- **Effect**: Deny
- **Purpose**: Deny creation of resources in any region other than Canada Central.

```json
{
  "properties": {
    "displayName": "Deny-Public-IP",
    "policyType": "Custom",
    "mode": "All",
    "description": "Prevents creation of Public IP addresses.",
    "policyRule": {
      "if": {
        "field": "type",
        "equals": "Microsoft.Network/publicIPAddresses"
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}

```
##  Challenges and Lessons Learned
### Challenges
- I initially wrote incorrect JSON for the policy definitions by omitting the required `"properties"` wrapper.
- Forgot to disable public IP creation while testing VM deployments, which triggered a denial.

### Lessons Learned
- Azure Policy provides a scalable way to enforce organizational standards automatically.
- Tagging and regional restrictions are easy to enforce once structured correctly.
  
## Demo
📹 [Watch the video demo](https://youtu.be/363dLZ0euW0)

## Folder Structure

```bash
/policy-lab          
│
├── policy-definitions/             
│   ├── Only-CanadaCentral.json
│   ├── Require-ProjectName-Tag.json
│   └── Deny-Public-IP.json
│
├── screenshots/                    
│   ├── Vm-Failed.png
│   ├── storage-failed.png
│   ├── IpAddress-Failed.png
│   └── Succesful-Attempt.png
```




