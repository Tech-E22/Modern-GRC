# IAM MFA Enforcement: Evidence Playbook

**Controls**: NIST IA-2, AC-7; ISO A.5.17; SOC2 CC6.3  
**Purpose**: Demonstrate MFA enforcement for human users (IAM or Identity Center).

## Commands (IAM Users)
```bash
aws iam list-users --query 'Users[].UserName' --output text | tr '\t' '\n' | while read U; do
  aws iam list-mfa-devices --user-name "$U"
done
```

## Commands (AWS IAM Identity Center)
Export Identity Center assignments & MFA policy per your IdP / SCIM.

## Acceptance Criteria
- 0 IAM users without MFA (or a formal exception).
- Quarterly attestation signed off.