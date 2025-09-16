# firefly-iii Installation Guide

firefly-iii is a free and open-source personal finance manager. Firefly III helps track expenses and manage personal finances

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

## 1. Prerequisites

- **Hardware Requirements**:
  - CPU: 1 core minimum
  - RAM: 512MB minimum
  - Storage: 1GB for data
  - Network: HTTP/HTTPS access
- **Operating System**: 
  - Linux: Any modern distribution (RHEL, Debian, Ubuntu, CentOS, Fedora, Arch, Alpine, openSUSE)
  - macOS: 10.14+ (Mojave or newer)
  - Windows: Windows Server 2016+ or Windows 10
  - FreeBSD: 11.0+
- **Network Requirements**:
  - Port 80 (default firefly-iii port)
  - None
- **Dependencies**:
  - See official documentation for specific requirements
- **System Access**: root or sudo privileges required


## 2. Supported Operating Systems

This guide supports installation on:
- RHEL 8/9 and derivatives (CentOS Stream, Rocky Linux, AlmaLinux)
- Debian 11/12
- Ubuntu 20.04/22.04/24.04 LTS
- Arch Linux (rolling release)
- Alpine Linux 3.18+
- openSUSE Leap 15.5+ / Tumbleweed
- SUSE Linux Enterprise Server (SLES) 15+
- macOS 12+ (Monterey and later) 
- FreeBSD 13+
- Windows 10/11/Server 2019+ (where applicable)

## 3. Installation

### RHEL/CentOS/Rocky Linux/AlmaLinux

```bash
# Install EPEL repository if needed
sudo dnf install -y epel-release

# Install firefly-iii
sudo dnf install -y firefly-iii

# Enable and start service
sudo systemctl enable --now firefly-iii

# Configure firewall
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# Verify installation
firefly-iii --version
```

### Debian/Ubuntu

```bash
# Update package index
sudo apt update

# Install firefly-iii
sudo apt install -y firefly-iii

# Enable and start service
sudo systemctl enable --now firefly-iii

# Configure firewall
sudo ufw allow 80

# Verify installation
firefly-iii --version
```

### Arch Linux

```bash
# Install firefly-iii
sudo pacman -S firefly-iii

# Enable and start service
sudo systemctl enable --now firefly-iii

# Verify installation
firefly-iii --version
```

### Alpine Linux

```bash
# Install firefly-iii
apk add --no-cache firefly-iii

# Enable and start service
rc-update add firefly-iii default
rc-service firefly-iii start

# Verify installation
firefly-iii --version
```

### openSUSE/SLES

```bash
# Install firefly-iii
sudo zypper install -y firefly-iii

# Enable and start service
sudo systemctl enable --now firefly-iii

# Configure firewall
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# Verify installation
firefly-iii --version
```

### macOS

```bash
# Using Homebrew
brew install firefly-iii

# Start service
brew services start firefly-iii

# Verify installation
firefly-iii --version
```

### FreeBSD

```bash
# Using pkg
pkg install firefly-iii

# Enable in rc.conf
echo 'firefly-iii_enable="YES"' >> /etc/rc.conf

# Start service
service firefly-iii start

# Verify installation
firefly-iii --version
```

### Windows

```bash
# Using Chocolatey
choco install firefly-iii

# Or using Scoop
scoop install firefly-iii

# Verify installation
firefly-iii --version
```

## Initial Configuration

### Basic Configuration

```bash
# Create configuration directory
sudo mkdir -p /etc/firefly-iii

# Set up basic configuration
# See official documentation for detailed configuration options

# Test configuration
firefly-iii --version
```

## 5. Service Management

### systemd (RHEL, Debian, Ubuntu, Arch, openSUSE)

```bash
# Enable service
sudo systemctl enable firefly-iii

# Start service
sudo systemctl start firefly-iii

# Stop service
sudo systemctl stop firefly-iii

# Restart service
sudo systemctl restart firefly-iii

# Check status
sudo systemctl status firefly-iii

# View logs
sudo journalctl -u firefly-iii -f
```

### OpenRC (Alpine Linux)

```bash
# Enable service
rc-update add firefly-iii default

# Start service
rc-service firefly-iii start

# Stop service
rc-service firefly-iii stop

# Restart service
rc-service firefly-iii restart

# Check status
rc-service firefly-iii status
```

### rc.d (FreeBSD)

```bash
# Enable in /etc/rc.conf
echo 'firefly-iii_enable="YES"' >> /etc/rc.conf

# Start service
service firefly-iii start

# Stop service
service firefly-iii stop

# Restart service
service firefly-iii restart

# Check status
service firefly-iii status
```

### launchd (macOS)

```bash
# Using Homebrew services
brew services start firefly-iii
brew services stop firefly-iii
brew services restart firefly-iii

# Check status
brew services list | grep firefly-iii
```

### Windows Service Manager

```powershell
# Start service
net start firefly-iii

# Stop service
net stop firefly-iii

# Using PowerShell
Start-Service firefly-iii
Stop-Service firefly-iii
Restart-Service firefly-iii

# Check status
Get-Service firefly-iii
```

## Advanced Configuration

See the official documentation for advanced configuration options.

## Reverse Proxy Setup

### nginx Configuration

```nginx
upstream firefly-iii_backend {
    server 127.0.0.1:80;
}

server {
    listen 80;
    server_name firefly-iii.example.com;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name firefly-iii.example.com;

    ssl_certificate /etc/ssl/certs/firefly-iii.example.com.crt;
    ssl_certificate_key /etc/ssl/private/firefly-iii.example.com.key;

    location / {
        proxy_pass http://firefly-iii_backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Apache Configuration

```apache
<VirtualHost *:80>
    ServerName firefly-iii.example.com
    Redirect permanent / https://firefly-iii.example.com/
</VirtualHost>

<VirtualHost *:443>
    ServerName firefly-iii.example.com
    
    SSLEngine on
    SSLCertificateFile /etc/ssl/certs/firefly-iii.example.com.crt
    SSLCertificateKeyFile /etc/ssl/private/firefly-iii.example.com.key
    
    ProxyRequests Off
    ProxyPreserveHost On
    
    ProxyPass / http://127.0.0.1:80/
    ProxyPassReverse / http://127.0.0.1:80/
</VirtualHost>
```

### HAProxy Configuration

```haproxy
frontend firefly-iii_frontend
    bind *:80
    bind *:443 ssl crt /etc/ssl/certs/firefly-iii.pem
    redirect scheme https if !{ ssl_fc }
    default_backend firefly-iii_backend

backend firefly-iii_backend
    balance roundrobin
    server firefly-iii1 127.0.0.1:80 check
```

## Security Configuration

### Basic Security Setup

```bash
# Set appropriate permissions
sudo chown -R firefly-iii:firefly-iii /etc/firefly-iii
sudo chmod 750 /etc/firefly-iii

# Configure firewall
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --reload

# Enable SELinux policies (if applicable)
sudo setsebool -P httpd_can_network_connect on
```

## Database Setup

See official documentation for database configuration requirements.

## Performance Optimization

### System Tuning

```bash
# Basic system tuning
echo 'net.core.somaxconn = 65535' | sudo tee -a /etc/sysctl.conf
echo 'net.ipv4.tcp_max_syn_backlog = 65535' | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## Monitoring

### Basic Monitoring

```bash
# Check service status
sudo systemctl status firefly-iii

# View logs
sudo journalctl -u firefly-iii -f

# Monitor resource usage
top -p $(pgrep firefly-iii)
```

## 9. Backup and Restore

### Backup Script

```bash
#!/bin/bash
# Basic backup script
BACKUP_DIR="/backup/firefly-iii"
DATE=$(date +%Y%m%d_%H%M%S)

mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/firefly-iii-backup-$DATE.tar.gz" /etc/firefly-iii /var/lib/firefly-iii

echo "Backup completed: $BACKUP_DIR/firefly-iii-backup-$DATE.tar.gz"
```

### Restore Procedure

```bash
# Stop service
sudo systemctl stop firefly-iii

# Restore from backup
tar -xzf /backup/firefly-iii/firefly-iii-backup-*.tar.gz -C /

# Start service
sudo systemctl start firefly-iii
```

## 6. Troubleshooting

### Common Issues

1. **Service won't start**:
```bash
# Check logs
sudo journalctl -u firefly-iii -n 100
sudo tail -f /var/log/firefly-iii/firefly-iii.log

# Check configuration
firefly-iii --version

# Check permissions
ls -la /etc/firefly-iii
```

2. **Connection issues**:
```bash
# Check if service is listening
sudo ss -tlnp | grep 80

# Test connectivity
telnet localhost 80

# Check firewall
sudo firewall-cmd --list-all
```

3. **Performance issues**:
```bash
# Check resource usage
top -p $(pgrep firefly-iii)

# Check disk I/O
iotop -p $(pgrep firefly-iii)

# Check connections
ss -an | grep 80
```

## Integration Examples

### Docker Compose Example

```yaml
version: '3.8'
services:
  firefly-iii:
    image: firefly-iii:latest
    ports:
      - "80:80"
    volumes:
      - ./config:/etc/firefly-iii
      - ./data:/var/lib/firefly-iii
    restart: unless-stopped
```

## Maintenance

### Update Procedures

```bash
# RHEL/CentOS/Rocky/AlmaLinux
sudo dnf update firefly-iii

# Debian/Ubuntu
sudo apt update && sudo apt upgrade firefly-iii

# Arch Linux
sudo pacman -Syu firefly-iii

# Alpine Linux
apk update && apk upgrade firefly-iii

# openSUSE
sudo zypper update firefly-iii

# FreeBSD
pkg update && pkg upgrade firefly-iii

# Always backup before updates
tar -czf /backup/firefly-iii-pre-update-$(date +%Y%m%d).tar.gz /etc/firefly-iii

# Restart after updates
sudo systemctl restart firefly-iii
```

### Regular Maintenance

```bash
# Log rotation
sudo logrotate -f /etc/logrotate.d/firefly-iii

# Clean old logs
find /var/log/firefly-iii -name "*.log" -mtime +30 -delete

# Check disk usage
du -sh /var/lib/firefly-iii
```

## Additional Resources

- Official Documentation: https://docs.firefly-iii.org/
- GitHub Repository: https://github.com/firefly-iii/firefly-iii
- Community Forum: https://forum.firefly-iii.org/
- Best Practices Guide: https://docs.firefly-iii.org/best-practices

---

**Note:** This guide is part of the [HowToMgr](https://howtomgr.github.io) collection. Always refer to official documentation for the most up-to-date information.
