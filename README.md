# 🛒 Verslun

> **Verslun** (Icelandic for *"commerce"*) is a ground-up e-commerce platform built as a practice project to develop proficiency in Ruby on Rails 8, Domain-Driven Design, agentic AI-assisted development, and containerized workflows. I (The developer) am not Icelandic. I learned an strategie from an friend in college for chosing names for projects that I quite like. Put the core word describing what the project is (Or means) and put it on Google Translate, going from language to language, until you find one that you like.

---

## Table of Contents

- [About the Project](#about-the-project)
- [Tech Stack](#tech-stack)
- [Development Setup Guide](#development-setup-guide)
  - [Prerequisites Summary](#prerequisites-summary)
  - [Step 1 - Enable WSL2 (Windows Only)](#step-1---enable-wsl2-windows-only)
  - [Step 2 - Install System Dependencies](#step-2---install-system-dependencies)
  - [Step 3 - Install rbenv & Ruby](#step-3---install-rbenv--ruby)
  - [Step 4 - Install Rails](#step-4---install-rails)
  - [Step 5 - Install Docker](#step-5---install-docker)
  - [Step 6 - Install Node.js (Optional)](#step-6---install-nodejs-optional)
  - [All Platforms - Docker Services](#all-platforms---docker-services)
  - [All Platforms - Project Bootstrap](#all-platforms---project-bootstrap)
- [Running the Application](#running-the-application)
- [Running Tests & CI](#running-tests--ci)

---

## About the Project

The goal of this project is to incrementally build a real-world e-commerce product, feature by feature, while deliberately practicing modern software craftsmanship techniques.

This isn't a throwaway tutorial app. Its purpose is to simulate building a production product from scratch, complete with proper architecture, CI, deployment configuration (Kamal), and domain modeling, while remaining a safe sandbox for learning.

## Tech Stack

| Component | Technology |
| --- | --- |
| Language | Ruby 4.0.5 |
| Framework | Rails 8.1.3 |
| Database | PostgreSQL 17 |
| Cache / Queues | Redis 7 (development), Solid Cache / Solid Queue / Solid Cable (production) |
| Frontend | Hotwire (Turbo + Stimulus), Tailwind CSS 4, Importmaps |
| Asset Pipeline | Propshaft |
| App Server | Puma 8 |
| Deployment | Kamal 2 + Thruster |
| CI | GitHub Actions |
| Linting | RuboCop (Rails Omakase), Brakeman, Bundler Audit |
| Package Manager | pnpm 11 (for JS tooling) |

---

## Development Setup Guide

This guide walks you through setting up Verslun for development **from a clean OS install**. Each step has OS-specific instructions where needed - pick the subheading for your platform, then follow the shared Docker and project bootstrap steps at the end.

### Prerequisites Summary

| Requirement | Version |
| --- | --- |
| Ruby | 4.0.5 (via rbenv) |
| Rails | 8.1.3 |
| Node.js | 24.16.0 (optional, for pnpm/JS tooling) |
| Docker & Docker Compose | Latest stable |
| Git | Any recent version |
| PostgreSQL client libs | Required for the `pg` gem native extension |

---

### Step 1 - Enable WSL2 (Windows Only)

> **Rails development on Windows requires WSL2.** Native Windows is not supported. All subsequent commands run inside your WSL2 Ubuntu terminal.

Open **PowerShell as Administrator** and run:

```powershell
wsl --install -d Ubuntu
```

Restart your machine if prompted. After reboot, the Ubuntu terminal will open and ask you to create a UNIX username and password.

From this point forward, **all commands run inside the WSL2 Ubuntu terminal** (which behaves like Ubuntu - follow the Ubuntu instructions below).

---

### Step 2 - Install System Dependencies

#### Ubuntu / Windows (WSL2)

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git curl gcc g++ make autoconf automake bison \
  build-essential libssl-dev libyaml-dev libreadline-dev zlib1g-dev \
  libncurses-dev libffi-dev libgdbm-dev libdb-dev uuid-dev \
  libpq-dev libjemalloc-dev libvips-dev
```

#### Fedora

```bash
sudo dnf update -y
sudo dnf install -y git-core gcc gcc-c++ make autoconf automake bison \
  patch readline-devel zlib-devel libffi-devel openssl-devel \
  libyaml-devel gdbm-devel ncurses-devel \
  postgresql-devel libvips-devel jemalloc-devel ruby-devel
```

---

### Step 3 - Install rbenv & Ruby

These commands are the same across all platforms.

```bash
# Install rbenv
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
echo 'eval "$(~/.rbenv/bin/rbenv init - bash)"' >> ~/.bashrc
source ~/.bashrc

# Install ruby-build plugin
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build

# Install Ruby 4.0.5
rbenv install 4.0.5
rbenv global 4.0.5

# Verify
ruby -v   # -> ruby 4.0.5
```

> **Zsh users (common on Fedora):** Replace `~/.bashrc` with `~/.zshrc` in the commands above.

---

### Step 4 - Install Rails

```bash
gem install rails -v 8.1.3
rbenv rehash
rails -v  # -> Rails 8.1.3
```

---

### Step 5 - Install Docker

#### Windows (WSL2)

1. Download and install [Docker Desktop for Windows](https://docs.docker.com/desktop/install/windows-install/).
2. In Docker Desktop -> **Settings -> Resources -> WSL Integration**, enable your Ubuntu distro.
3. Verify inside WSL2:

```bash
docker --version          # -> Docker version 28.x
docker compose version    # -> Docker Compose version v2.x
```

#### Ubuntu

```bash
# Add Docker's official GPG key and repository
sudo apt install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
  https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker Engine + Compose
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Allow running Docker without sudo
sudo usermod -aG docker $USER
newgrp docker

# Verify
docker --version          # -> Docker version 28.x
docker compose version    # -> Docker Compose version v2.x
```

#### Fedora - Docker

```bash
# Install Docker via dnf
sudo dnf install -y dnf-plugins-core
sudo dnf-3 config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Start and enable Docker
sudo systemctl start docker
sudo systemctl enable docker

# Allow running Docker without sudo
sudo usermod -aG docker $USER
newgrp docker

# Verify
docker --version          # -> Docker version 28.x
docker compose version    # -> Docker Compose version v2.x
```

---

### Step 6 - Install Node.js (Optional)

These commands are the same across all platforms.

```bash
curl -fsSL https://fnm.vercel.app/install | bash
source ~/.bashrc
fnm install 24.16.0
fnm use 24.16.0
node -v  # -> v24.16.0

# Install pnpm
corepack enable
corepack prepare pnpm@11.5.0 --activate
```

**-> Now continue to the Docker and project setup below.**

---

### All Platforms - Docker Services

These steps are the same on every OS (including WSL2).

#### 1. Clone the Repository

```bash
git clone https://github.com/<your-username>/verslun.git
cd verslun
```

#### 2. Create Your Environment File

```bash
cp .env.example .env
```

The defaults work out-of-the-box for local development. Edit `.env` if you need custom ports or credentials.

#### 3. Start PostgreSQL & Redis

```bash
docker compose up -d
```

Verify both services are healthy:

```bash
docker compose ps
```

You should see `verslun_postgres` and `verslun_redis` with status **healthy**.

#### 4. Test the Database Connection

```bash
docker compose exec postgres psql -U verslun -d verslun_development -c "SELECT 1;"
```

If this returns `1`, PostgreSQL is ready.

---

### All Platforms - Project Bootstrap

#### 1. Install Ruby Dependencies

```bash
bundle install
```

#### 2. Prepare the Database

```bash
bin/rails db:prepare
```

This creates the `verslun_development` and `verslun_test` databases and runs any pending migrations.

#### 3. Install Frontend Dependencies (Optional)

```bash
pnpm install
```

#### 4. Verify Everything Works

```bash
bin/rails runner "puts 'Rails is alive on Ruby ' + RUBY_VERSION"
```

Expected output: `Rails is alive on Ruby 4.0.5`

---

## Running the Application

The project uses [Foreman](https://github.com/ddollar/foreman) to run the Rails server and Tailwind CSS watcher simultaneously:

```bash
bin/dev
```

This starts:

- **Rails server** on `http://localhost:3000`
- **Tailwind CSS** watcher for live style compilation

> **Note:** Make sure Docker services are running first: `docker compose up -d`

To run the Rails server alone:

```bash
bin/rails server
```

---

## Running Tests & CI

```bash
# Run all tests
bin/rails test

# Run the full CI pipeline locally (style, security, tests)
bin/ci

# Individual checks
bin/rubocop              # Ruby style (Omakase)
bin/brakeman             # Security analysis
bin/bundler-audit         # Gem vulnerability audit
```

---
