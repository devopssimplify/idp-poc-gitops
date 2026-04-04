# Infrastructure Deletion Instructions

**Resource Type**: CockroachDB Database
**Resource Name**: `pds-501d-db`
**Environment**: `staging`
**Deletion Reason**: 
**Requested**: ${{ '' | now }}

---

## Files to Delete

Based on the resource type **CockroachDB Database**, you need to delete the following files:

### CockroachDB Database Files

```bash
# Delete these files from the repository:
git rm infra/cockroachdb-pds-501d-db.yaml
git rm infra/cockroachdb-pds-501d-db-backup.yaml
```

**Files to remove**:
- `infra/cockroachdb-pds-501d-db.yaml` - Main StatefulSet, Services, and Jobs
- `infra/cockroachdb-pds-501d-db-backup.yaml` - Backup configuration (if exists)

---

## Step-by-Step Deletion Process

### 1. Checkout the Branch

```bash
git fetch origin
git checkout delete-pds-501d-db
```

### 2. Delete the Files

Run the deletion commands listed above, or manually delete the files.

### 3. Verify Files are Staged for Deletion

```bash
git status
```

You should see the files marked for deletion.

### 4. Commit the Changes

```bash
git commit -m "chore: Remove pds-501d-db from staging"
```

### 5. Push to Remote

```bash
git push origin delete-pds-501d-db
```

### 6. Merge the Pull Request

Approve and merge the PR in GitHub. Config Sync will detect the file removal and delete the resources from the cluster.

---

## Verification Commands

After the PR is merged and Config Sync has synced (wait ~1-2 minutes), verify the resources are deleted:

```bash
# Check StatefulSet is gone
kubectl get statefulset cockroachdb-pds-501d-db -n staging

# Check Services are gone
kubectl get svc -n staging | grep cockroachdb-pds-501d-db

# Check PVCs (may need manual cleanup)
kubectl get pvc -n staging | grep cockroachdb-pds-501d-db
```

**Note**: Persistent Volume Claims (PVCs) may need to be manually deleted:
```bash
kubectl delete pvc -n staging -l app=cockroachdb-pds-501d-db
```

---

## Rollback (If Needed)

If you need to rollback this deletion:

1. **Before merging the PR**: Simply close the PR
2. **After merging**: You'll need to recreate the resource using the appropriate Backstage template

---

## ⚠️ WARNING

- **Data Loss**: All data in this resource will be permanently deleted
- **No Automatic Backups**: Ensure you have backups if needed
- **Dependencies**: Check if any applications depend on this resource before deletion

---

## Support

If you encounter issues:
1. Check Config Sync status: `kubectl get rootsync -n config-management-system`
2. Check for stuck resources: `kubectl get all -n staging`
3. Manually delete stuck resources if needed

---

**Deletion requested by**: Backstage Scaffolder
**Generated**: ${{ '' | now }}
