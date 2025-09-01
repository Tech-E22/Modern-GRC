# S3 Public Access: Evidence Playbook

**Controls**: NIST AC-3, SC-7; ISO A.5.15; SOC2 CC6.1  
**Purpose**: Show that S3 buckets block public access org-wide and no buckets are publicly readable.

## Commands
List accounts (if using Organizations):
```bash
aws organizations list-accounts --query 'Accounts[].Id' --output text
```

Check org-level Public Access Block (PAB):
```bash
aws s3control get-public-access-block --account-id <ACCOUNT_ID>
```

Scan all buckets for public ACLs and bucket policies:
```bash
aws s3api list-buckets --query 'Buckets[].Name' --output text | tr '\t' '\n' | while read B; do
  echo "Bucket: $B"
  aws s3api get-bucket-acl --bucket "$B" || true
  aws s3api get-bucket-policy-status --bucket "$B" || true
done
```

## Acceptance Criteria
- Org or account-level PAB = **true** for all settings.
- No buckets with `PolicyStatus.IsPublic = true`.
- Evidence: CLI output saved to `evidence/YYYY-MM/s3_public_access.json` with timestamp.

## Redaction
- Remove account IDs and bucket names if considered sensitive; keep counts and flags.