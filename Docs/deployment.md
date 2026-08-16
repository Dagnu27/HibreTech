# HiberTech — Deployment

## 1. Purpose

This document defines the deployment architecture and process for
HiberTech MVP.

It describes the hosting platforms, deployment flow, environment
configuration, production services, and basic deployment requirements.

---

## 2. Deployment Architecture

HiberTech uses a web-based deployment architecture.

| Component | Recommended Platform | Purpose |
|---|---|---|
| Frontend | Vercel | Host Next.js application |
| Backend | Render or Railway | Run Express and Socket.io |
| Database | MongoDB Atlas | Store application data |
| File Storage | Cloudinary or AWS S3 | Store uploaded files |
| Email | Email Provider | Password reset and invitations |
| Source Control | GitHub | Store source code and documentation |

---

## 3. Production Architecture

```text
                         Users
                           │
                           ▼
                    HTTPS / Browser
                           │
                           ▼
                    Vercel Frontend
                       Next.js
                           │
                 ┌─────────┴─────────┐
                 │                   │
               REST              WebSocket
                 │                   │
                 ▼                   ▼
          Express Backend      Socket.io Server
                 │                   │
                 └─────────┬─────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        MongoDB Atlas   Cloud Storage  Email
                         Cloudinary/
                           S3