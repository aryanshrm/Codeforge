# 🚀 CodeForge — Competitive Programming Platform

[![React](https://img.shields.io/badge/React-18.x-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactjs.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.x-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Docker](https://img.shields.io/badge/Docker-Containers-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.x-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)
[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

> A full-stack, enterprise-grade competitive programming and coding practice platform featuring a Docker-isolated code judge, interactive Monaco Editor, real-time leaderboard analytics, AI-powered problem hints, and a sleek glassmorphic beehive UI.

---

## 📖 Overview

**CodeForge** is an end-to-end coding platform designed for competitive programmers and software engineers. It allows developers to solve algorithmic challenges, test their code in secure, isolated Docker containers, track problem-solving statistics, and climb a global leaderboard.

Built with a modern stack featuring **React, Vite, TypeScript, FastAPI, PostgreSQL**, and a containerized **Docker Judge Engine**, CodeForge balances low-latency execution with sandbox security.

---

## ✨ Features

### 🛠️ For Coders
- **Monaco Code Editor**: Full-featured code editor with syntax highlighting, auto-completion, line numbers, and multi-language support powered by VS Code's core editor engine.
- **Multi-Language Sandbox**: Submit code in **Python 3.12**, **C++17**, **Java 17**, and **Node.js (JavaScript)**.
- **Docker-Isolated Judge Engine**: Code runs inside ephemeral, resource-constrained Docker containers (CPU, memory, time-limit bounded) preventing unauthorized system calls or malicious scripts.
- **Test Case Validation**: Supports both public sample test cases and hidden evaluation test cases with precise execution feedback (Time Limit Exceeded, Memory Limit Exceeded, Wrong Answer, Accepted, Runtime Error).
- **AI-Powered Problem Hints**: Contextual, non-spoiling AI guidance to assist developers when stuck on complex algorithms.
- **Custom Execution Runner**: Execute arbitrary test cases directly from the browser before submitting for final evaluation.

### 🏆 Community & Competition
- **Real-Time Leaderboard**: Dynamically ranked leaderboard based on solved count, total submission score, and time penalty with tag/time filtering.
- **User Profiles & Analytics**: Comprehensive stats tracking solved problems categorized by difficulty (Easy, Medium, Hard) and topic tag.
- **Problem Bank**: Filterable list of algorithm problems with tags, acceptance rates, difficulty badges, and search functionality.

### 🎨 Modern UX & Admin Control
- **Frosted Glass UI & Beehive Canvas**: Stunning glassmorphic aesthetics powered by dynamic canvas background animations and Tailwind CSS.
- **Light & Dark Mode**: Native theme toggling with custom CSS tokens and persistent user preferences.
- **Google OAuth & JWT Authentication**: Secure user authentication supporting passwordless Google OAuth 2.0 and JWT session tokens.
- **Admin Dashboard**: Dedicated administration interface for creating problems, uploading test cases, managing user accounts, and monitoring judge queues.

---

## 🏗️ Architecture

```
                       +------------------------+
                       |    React + Vite Client |
                       |  (Monaco Editor + UI)  |
                       +-----------+------------+
                                   | HTTP / REST
                                   v
                       +------------------------+
                       |    FastAPI Backend     |
                       |  (JWT Auth, API Routes)|
                       +-----+------------+-----+
                             |            |
             +---------------+            +---------------+
             |                                            |
             v                                            v
+------------------------+                    +------------------------+
|  PostgreSQL / SQLite   |                    |  Docker Judge Worker   |
|   (Users, Problems)    |                    |  (Isolated Container)  |
+------------------------+                    +------------------------+
```

---

## 🛠️ Tech Stack

| Layer | Technologies |
| :--- | :--- |
| **Frontend** | React 18, Vite, TypeScript, Tailwind CSS, Framer Motion, Monaco Editor, Lucide Icons |
| **Backend** | FastAPI, Python 3.12, Pydantic v2, SQLAlchemy 2.0, Alembic |
| **Database** | PostgreSQL (Production) / SQLite (Development) |
| **Judge Engine** | Docker Python SDK, Secure Linux Container Sandbox (cgroups + rlimit constraints) |
| **Admin** | Python Flask Admin Dashboard |
| **Infra & DevOps** | Docker, Docker Compose, Uvicorn |

---

## ⚡ Quick Start

### Option 1: Docker Compose (Recommended)

Run the entire platform (Frontend, Backend, Admin, Database) with a single command:

```bash
# 1. Clone the repository
git clone https://github.com/aryanshrm/Codeforge.git
cd Codeforge

# 2. Configure environment variables
cp .env.example .env

# 3. Launch with Docker Compose
docker-compose up -d --build
```

Access the platform services:
- **Frontend App**: `http://localhost:3000`
- **FastAPI Docs**: `http://localhost:8000/docs`
- **Admin Dashboard**: `http://localhost:5000`

---

### Option 2: Local Development (Manual Setup)

#### 1. Backend Setup

```bash
cd backend

# Create & activate virtual environment
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run database migrations
alembic upgrade head

# Seed initial problem dataset (optional)
python ../scripts/seed_db.py

# Start FastAPI server
uvicorn app.main:app --reload --port 8000
```

#### 2. Frontend Setup

```bash
cd frontend

# Install Node dependencies
npm install

# Start Vite development server
npm run dev
```

#### 3. Judge Sandbox Engine

Ensure Docker Desktop is running locally. The backend will automatically communicate with the local Docker daemon (`unix:///var/run/docker.sock` or `npipe:////./pipe/docker_engine` on Windows) to spin up ephemeral evaluation containers.

---

## 🔑 Environment Variables

Create a `.env` file in the root directory:

| Variable | Description | Default |
| :--- | :--- | :--- |
| `DATABASE_URL` | PostgreSQL or SQLite connection string | `sqlite:///./codeforge.db` |
| `SECRET_KEY` | JWT signing secret | `your-super-secret-key` |
| `ALGORITHM` | JWT encryption algorithm | `HS256` |
| `ACCESS_TOKEN_EXPIRE_MINUTES` | Token lifetime in minutes | `10080` |
| `GOOGLE_CLIENT_ID` | Google OAuth Client ID | `your-google-client-id` |
| `DOCKER_HOST` | Docker daemon socket URL | System default |

---

## 🔌 API Endpoints Summary

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/api/v1/auth/login` | Authenticate user & receive JWT token |
| `POST` | `/api/v1/auth/register` | Register new user account |
| `GET` | `/api/v1/problems` | List all available coding problems |
| `GET` | `/api/v1/problems/{id}` | Retrieve problem details & public test cases |
| `POST` | `/api/v1/run` | Execute code against sample test cases |
| `POST` | `/api/v1/submissions` | Submit code for full evaluation |
| `GET` | `/api/v1/users/me` | Fetch active user profile and submission stats |
| `GET` | `/api/v1/users/leaderboard` | Get ranked global user standings |

---

## 🌟 Why CodeForge Stands Out

1. **True Execution Isolation**: Unlike simple `exec()` evaluation models, CodeForge provisions dedicated Linux cgroup boundaries per code execution with strict CPU quotas, network disablement (`--net=none`), and read-only root filesystems.
2. **Production-Ready Architecture**: Decoupled REST architecture separating UI, authentication middleware, database models, and worker queues.
3. **Developer-First Design**: Custom glassmorphism aesthetic built specifically for long coding sessions with minimal eye strain.

---

## 👨‍💻 Author

**Aryan Sharma** (*aryanshrm*)
- GitHub: [@aryanshrm](https://github.com/aryanshrm)
- LinkedIn: [https://www.linkedin.com/in/aryanshrm](https://www.linkedin.com/in/aryanshrm)

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
