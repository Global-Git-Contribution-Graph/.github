# Global Git Contribution Graph

A distributed system for aggregating, visualizing, and sharing Git contribution data across providers (GitHub, GitLab, Forgejo, and custom providers), with support for web integrations, mobile synchronization via QR code, and public sharing.

---

## Introduction

**Global Git Contribution Graph** is a multi-service architecture designed to:

* Aggregate Git contribution statistics from multiple providers
* Securely manage user configurations
* Provide embeddable HTML integrations
* Enable mobile app synchronization via QR codes
* Support public and shared contribution pages

The system is composed of:

* A **Next.js Web Application**
* A **Next.js API Gateway**
* A **Stateless Rust Aggregation API**
* A **MariaDB database**
* A **Redis cache layer**
* **Mobile applications**
* Public and embeddable integrations

This repository organization hosts all related services and components.

---

## Table of Contents

* [Architecture Overview](#architecture-overview)
* [Core Components](#core-components)
* [Authentication & Account Linking](#authentication--account-linking)
* [QR Code Synchronization](#qr-code-synchronization)
* [Public & Shared Pages](#public--shared-pages)
* [Installation](#installation)
* [Configuration](#configuration)
* [Usage](#usage)
* [Integrations](#integrations)
* [Data Flow](#data-flow)
* [Dependencies](#dependencies)
* [Troubleshooting](#troubleshooting)
* [Contributors](#contributors)

---

# Architecture Overview

The system follows a **layered, service-oriented architecture**:

```
WebApp (Next.js)
        │
        ▼
API (Next.js - Auth + Config Encryption)
        │
        ▼
Stateless Rust API (Aggregation Engine)
        │
        ├── Redis (temporary cached user configs)
        └── Git Providers (GitHub, GitLab, Forgejo, custom)
        
MariaDB (persistent storage)
Mobile Apps (QR sync & local storage)
HTML Integrations (embeddable widgets)
```

### Key Architectural Principles

* Stateless aggregation engine (Rust)
* Secure config encryption before external calls
* Redis-based temporary config storage (TTL)
* Separation between identity, config management, and aggregation
* Provider-agnostic design
* QR-code-based device synchronization

---

# Core Components

## 1. Web Application (Next.js)

Responsible for:

* User dashboard
* Displaying aggregated statistics
* Managing provider configurations
* Generating integration HTML snippets
* Generating QR codes for mobile synchronization

### Stored Client-Side Config Metadata

* Provider ID
* Provider type
* Public provider settings (Account Linking) 
* Custom provider settings (name, username, URL)

---

## 2. API Gateway (Next.js API)

Handles:

* User authentication
* Account linking via Auth.js (NextAuth)
* Encryption of user configurations
* Construction of integration URLs
* Forwarding requests to Rust API

### Responsibilities

* Secure config handling
* Token validation
* Public link verification
* Stats request routing

---

## 3. Stateless Rust API

The aggregation engine:

* Decrypts configs (if required)
* Requests Git providers
* Aggregates contribution data
* Returns normalized statistics
* Uses Redis for temporary user config resolution

### Characteristics

* Stateless
* HTTPS communication only
* Cache-enabled
* Multi-provider aggregation support

---

## 4. Database – MariaDB

Persistent storage for:

* User identities
* Account associations
* Provider configurations

---

## 5. Redis

Used for:

* Temporary storage of user configuration by ID
* QR-code synchronization flow
* TTL-based config expiration
* Reducing DB load

---

## 6. Mobile Applications

Capabilities:

* Scan QR code
* Extract temporary config ID and API endpoint
* Fetch decrypted configuration
* Store full configuration locally
* Request stats directly from Rust API

Authentication is embedded in the QR synchronization process.

---

# Authentication & Account Linking

Authentication is handled by **Auth.js (NextAuth)**.

Supported flows:

* Email/password
* OAuth providers (GitHub, GitLab, etc.)
* Account linking
* Custom provider registration

User identity and linked accounts are stored in MariaDB.

---

# QR Code Synchronization

The QR synchronization flow enables secure device linking:

1. WebApp generates:

   * Temporary config ID
   * API endpoint reference
2. Config is stored in Redis with TTL
3. QR code contains:

   * Temporary config ID
   * Target API URL
4. Mobile app:

   * Scans QR
   * Retrieves config from API
   * Stores configuration locally
   * Can fetch stats independently

Temporary IDs expire automatically.

---

# Public & Shared Pages

The system supports:

## Public User Page

* Generate public link
* Disable public link
* World-view mode
* Simple public sharing

## Repository Sharing Mode

* Share repository-specific data
* Token-based access
* API verifies token validity
* Graph rendered if token is valid

Token presence implies authorization.

---

# Installation

Each service is maintained in its dedicated repository.

General setup:

### 1. Clone Organization Repositories

```bash
git clone https://github.com/Global-Git-Contribution-Graph/<repo-name>
```

### 2. Setup Redis

Launch the container that contains the Rust environment and the Redis service 
```bash
docker compose -f .devcontainer/docker-compose.yml up --build
```

### 3. Install Dependencies

For Rust API:

In the Rust + Redis container
```bash
cargo build --release
```

For Next.js services:

```bash
npm install
```

### 4. Setup Database

```bash
# Not yet implemented...
```

---

# Configuration

Environment variables typically include:

### Next.js API

* `DATABASE_URL`
* `REDIS_URL`
* `NEXTAUTH_SECRET`
* OAuth provider keys
* Encryption keys for configs

### Rust API

* `REDIS_URL`
* Private key for config decryption

---

# Integrations

## HTML Integration

Users can embed contribution graphs via:

```html
<!-- Not yet implemented... -->
```

The API constructs integration URLs dynamically.

## Supported Providers

* GitHub (public instance only)
* GitLab (public instance & user's self-hosted instance)
* Forgejo (user's self-hosted instance)
* More to come

---

# Data Flow

### Stats Request Flow

1. WebApp requests stats (via API)
2. API validates user
4. Rust API fetches provider data
5. Data aggregated
6. Response returned
7. UI renders graph

### Mobile Flow

1. Mobile scans QR
2. Retrieves temporary config ID
3. Calls Next API
4. Retrieves user config
5. Calls Rust API
6. Rust fetches provider data
7. Stats returned to mobile

---

# Dependencies

* Next.js
* Auth.js (NextAuth)
* Rust (Tokio / Reqwest ecosystem)
* MariaDB
* Redis
* OAuth providers
* Git provider APIs

---

# Troubleshooting

### OAuth Issues

* Validate provider credentials
* Confirm callback URLs

### Stats Not Updating

* Clear Redis cache
* Verify provider API limits

---

# Contributors

This organization contains multiple repositories maintained collaboratively.

Contributions are welcome. Please open issues and pull requests in the relevant repository.
