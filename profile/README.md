# 🌍 Global Git Contribution Graph (GGCG)

> A universal, privacy-first Git contribution aggregator.

**Global Git Contribution Graph (GGCG)** is an open-source ecosystem designed to unify and visualize developer activity across multiple Git platforms — including GitHub, GitLab, and self-hosted instances such as Forgejo or Gitea — while ensuring full control over access tokens and personal data.

---

## 📌 Product Vision

GGCG empowers developers to consolidate their distributed Git activity into a single, unified contribution graph.

Unlike centralized dashboards, GGCG follows a **Privacy-First philosophy**:

- 🔐 No permanent storage of sensitive tokens on public servers  
- 🏠 Fully self-hostable architecture  
- 📱 Mobile-first secure local storage  
- 🔎 Transparent and auditable data flows  

---

## 🏗️ Technical Architecture

GGCG is built around a strict Separation of Concerns (SoC) principle and a modular provider-based architecture.

### 1️⃣ Backend – The Logical Core

- **Language:** Rust
- **API:** GraphQL
- **Provider System:** Adapter-based architecture  
  Each forge (GitHub, GitLab, Forgejo, etc.) implements a shared `GitProvider` interface.
- **Smart Caching:** Redis is used to:
  - Prevent API rate-limit issues
  - Improve dashboard loading times
  - Reduce unnecessary external API calls

This design allows easy integration of new Git providers without affecting the core API.

---

### 2️⃣ Frontend – The User Interface

#### 🌐 Web Application (Next.js)

- Single Page Application (SPA)
- Integrated with the self-hosted backend
- Interactive contribution heatmaps
- Real-time aggregated statistics
- Configuration of fetch forges

#### 📱 Mobile Application

- Native / cross-platform implementation
- Secure local storage for access tokens
- Designed for easy viewing of statistics anywhere
- Configuration of fetch forges
- Can use a public API or a self-hosted instance

---

### 3️⃣ Infrastructure & Deployment

#### 🐳 Containerized Deployment

- Multi-architecture Docker images (AMD64 / ARM64)
- Designed for lightweight VPS hosting
- Optimized memory footprint

#### 🔁 Hybrid Operating Modes

**🔹 Self-Hosted Mode**
- User deploys the full stack (API + Redis + Web)
- Secrets and tokens remain under user control
- Ideal for privacy-focused developers

**🔹 Public API Mode**
- Mobile app connects to a central instance
- Tokens are never persisted server-side (secure pass-through)
- Users may also connect the mobile app to their own instance

---

## 🔒 Security & Data Flow

Security is a core differentiator of GGCG.

### 1. Token Transmission
- Tokens are only sent at request time
- Tokens are never logged
- No persistent storage in public mode

### 2. Storage Model
- **Self-hosted:** Redis acts as a temporary volatile cache
- **Mobile:** Tokens remain inside the device’s Secure Enclave / secure storage

### 3. Principle of Least Privilege
- Users are encouraged to use **read-only tokens**
- GGCG performs **no write operations**
- No repository modifications are possible

---

## 📦 Organization Repositories

The organization contains multiple components of the GGCG ecosystem:

- 🔧 Backend API (Rust + GraphQL)
- 🌐 Web frontend (Next.js)
- 📱 Mobile application
- 🐳 Deployment & infrastructure tooling
- 📚 Documentation and provider integrations

Each repository follows the same principles:
- Clean modular architecture
- Clear separation of responsibilities
- Privacy-first design

---

## 🚀 Why GGCG Matters

GGCG demonstrates:

- 🦀 **Rust backend engineering**
- 🔗 **GraphQL schema design**
- 🧩 **Multi-provider data aggregation**
- 🐳 **Production-ready Docker deployment**
- ⚡ **Performance-oriented caching strategies (Redis)**
- 🔐 **Security-first architectural thinking**
- 📊 Optimized data structures for contribution heatmap computation

This project showcases expertise across the full development lifecycle — from backend performance engineering to frontend UX and infrastructure deployment.

---

## 🤝 Contributing

We welcome contributions in:

- New Git provider integrations
- Performance improvements
- UI/UX enhancements
- Security audits
- Documentation improvements

Please open issues or pull requests in the relevant repositories.
