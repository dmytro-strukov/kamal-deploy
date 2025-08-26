# Kamal Deployment Guide

This repository contains two applications (Go and Rails) that are deployed using Kamal. This guide will help you set up and deploy both applications.

## Prerequisites

- Docker installed and running
- Ruby 3.0+ installed (for Kamal gem)
- Access to the target server (SSH key configured)
- GitHub Container Registry (ghcr.io) access
- Git installed

## Installation

1. Install Kamal:
```bash
gem install kamal
```

2. Set up environment variables:
```bash
export KAMAL_REGISTRY_PASSWORD="your_github_token"
```

For Rails application, additional variables are needed:
```bash
export RAILS_MASTER_KEY="your_rails_master_key"
export POSTGRES_PASSWORD="your_postgres_password"
```

## Go Application Deployment

### Configuration

The Go application uses a simple deployment setup with the following configuration:
- Service name: `goapp`
- Registry: GitHub Container Registry (ghcr.io)
- No SSL configuration (development setup)

### Deployment Steps

1. Build and push the Docker image:
```bash
cd go
kamal build
```

2. Deploy the application:
```bash
kamal deploy
```

3. Check deployment status:
```bash
kamal status
```

## Rails Application Deployment

### Configuration

The Rails application has a more complex setup with:
- Service name: `rails-app`
- PostgreSQL database as an accessory service
- Registry: GitHub Container Registry (ghcr.io)
- No SSL configuration (development setup)

### Deployment Steps

1. First-time setup for PostgreSQL:
```bash
cd rails
kamal accessory boot db
```

2. Build and push the Docker image:
```bash
kamal build
```

3. Deploy the application:
```bash
kamal deploy
```

4. Check deployment status:
```bash
kamal status
```

## Common Kamal Commands

- `kamal setup`: Prepare servers for first deploy
- `kamal deploy`: Deploy the application
- `kamal rollback`: Rollback to the previous version
- `kamal restart`: Restart the application
- `kamal logs`: View application logs
- `kamal accessory reboot [name]`: Restart an accessory service
- `kamal lock`: Lock deployments
- `kamal unlock`: Unlock deployments

## Configuration Files

- Go application: `go/config/deploy.yml`
- Rails application: `rails/config/deploy.yml`

## Server Requirements

- Ubuntu 22.04 LTS (recommended)
- Docker installed
- Open ports:
  - 80/443 for web traffic
  - 5432 for PostgreSQL (Rails app only)
  - SSH access

## Troubleshooting

1. If deployment fails:
   - Check server connectivity
   - Verify environment variables are set
   - Review logs with `kamal logs`
   - Ensure Docker registry access is configured

2. Database issues (Rails):
   - Check PostgreSQL container status
   - Verify database credentials
   - Check database logs

3. Container issues:
   - Use `kamal app exec` to debug inside containers
   - Check Docker logs on the server
   - Verify disk space and resource usage