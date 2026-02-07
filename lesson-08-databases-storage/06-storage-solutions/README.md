# Storage Solutions

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Caching](../05-caching/README.md) | [Next: Database Selection Guide](../07-database-selection-guide/README.md)

---

## Object Storage

**Azure Blob Storage / S3 / GCS**

```
+-------------------------------------------------------------+
|                  Object Storage Tiers                        |
|                                                             |
|  Hot        | Frequent access    | Highest cost            |
|  ------------------------------------------------------------
|  Cool       | Infrequent access  | Lower storage cost      |
|             | (30+ days)         | Higher access cost      |
|  ------------------------------------------------------------
|  Archive    | Rare access        | Lowest storage cost     |
|             | (180+ days)        | Retrieval delays        |
+-------------------------------------------------------------+
```

**Use Cases**:
- Static website hosting
- Backup and archival
- Data lakes
- Media storage

## Block vs File vs Object

| Type | Protocol | Use Case |
|------|----------|----------|
| **Block** | iSCSI, FC | Databases, VMs |
| **File** | NFS, SMB | Shared files, legacy apps |
| **Object** | HTTP/REST | Unstructured data, scalability |

## Storage Types Explained

### Block Storage
- Low-level storage that presents raw storage volumes
- Best for applications requiring high performance and low latency
- Examples: Azure Managed Disks, AWS EBS, Google Persistent Disk

### File Storage
- Hierarchical file system accessible over network protocols
- Best for shared access and traditional applications
- Examples: Azure Files, AWS EFS, Google Filestore

### Object Storage
- Flat namespace with unlimited scalability
- Best for unstructured data and cloud-native applications
- Examples: Azure Blob Storage, AWS S3, Google Cloud Storage

## Choosing the Right Storage

| Requirement | Recommended Storage |
|-------------|-------------------|
| Database workloads | Block (SSD) |
| Shared file access | File (NFS/SMB) |
| Static content/media | Object |
| Backup/archive | Object (cold tier) |
| High IOPS | Block (premium SSD) |
| Cost optimization | Object with lifecycle policies |

---

## Diagrams in This Section

- [6.1-storage-types.drawio](./6.1-storage-types.drawio)
