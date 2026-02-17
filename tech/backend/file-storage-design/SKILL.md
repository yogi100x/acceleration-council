# File Storage Design

Design file upload, storage, processing, and delivery systems.

## Research Protocol

### Web Search
- "presigned URL upload pattern [current year]"
- "image processing pipeline [current year]"
- "S3 vs R2 vs GCS comparison [current year]"

### WebFetch
- https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html

---

## Context Sync Protocol

### Read Upstream
- `.claude/outputs/system-architecture.md` — cloud provider, CDN
- `.claude/outputs/api-design.md` — upload endpoints

---

## Decision Tree: Upload Pattern

```
How are files uploaded?
├── Small files (<5MB) — profile photos, documents
│   └── Direct upload to API server → forward to storage
├── Large files (5MB-5GB) — videos, datasets
│   └── Presigned URL: Client uploads directly to S3/R2
│       → Server generates presigned URL, client uploads, server processes
├── Multiple files — gallery, attachments
│   └── Parallel presigned URLs + batch confirmation
├── Streaming uploads — real-time recording
│   └── Multipart upload with chunk tracking
└── User-generated content — avatars, posts
    └── Presigned URL + server-side validation + processing pipeline
```

## Storage Architecture

```
Upload Flow:
  Client → Presigned URL request → API generates URL
  Client → Direct upload to S3/R2 → Storage
  S3/R2 → Event notification → Processing pipeline
  Pipeline → Resize/optimize → CDN-ready versions
  CDN → Serve to users

Storage layers:
  Hot: S3/R2 (frequently accessed)
  Warm: S3 Infrequent Access (older files)
  Cold: Glacier/Archive (backups, compliance)
```

## Security Checklist

- [ ] File type validation (server-side, not just extension)
- [ ] File size limits enforced
- [ ] Virus/malware scanning for user uploads
- [ ] Presigned URLs have short expiry (5-15 min)
- [ ] Private files require authentication to access
- [ ] No directory listing on storage buckets
- [ ] CDN URLs are signed for private content

## Anti-Patterns

| ID | Anti-Pattern | Impact |
|----|-------------|--------|
| T35 | Storing files in database | Database bloat, slow queries |
| T36 | No file type validation | Malware uploads, XSS via SVG |
| T37 | Long-lived presigned URLs | Unauthorized access after share |

## Quality Rubric (35 points)

| Dimension | 5 pts | 3 pts | 1 pt |
|-----------|-------|-------|------|
| **Upload UX** | Progress bar, retry, drag-drop | Basic upload | No feedback |
| **Security** | Type validation, size limits, malware scan | Basic validation | No validation |
| **Processing** | Automatic resize, optimize, format conversion | Manual processing | No processing |
| **Delivery** | CDN with proper cache headers | Direct from storage | No CDN |
| **Cost** | Lifecycle policies, tiered storage | Single tier | No cost management |
| **Reliability** | Resumable uploads, retry on failure | Basic retry | Upload lost on failure |
| **Monitoring** | Storage usage, upload success rate, cost tracking | Basic metrics | No monitoring |

**28+ = Robust file system | 21-27 = Works but gaps | <21 = Security/reliability risk**

## Output Protocol

Write to `.claude/outputs/file-storage-design.md`:
- Upload pattern chosen
- Storage provider and bucket structure
- Processing pipeline design
- CDN configuration
