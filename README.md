# Redis Sentinel High Availability Setup with Ansible

This Ansible playbook configures Redis Sentinel for high availability on Rocky Linux 9.4.

## Architecture
- Master: 172.16.90.160 (Redis on port 6379, Sentinel on port 26379)  
- Slave 1: 172.16.90.161 (Redis on port 6379, Sentinel on port 26379)
- Slave 2: 172.16.90.162 (Redis on port 6379, Sentinel on port 26379)

## Prerequisites
- Rocky Linux 9.4 installed on all servers
- SSH access with root user configured
- Ansible installed on control machine

## Quick Start
1. Copy vars.yml.example to vars.yml and configure variables
2. Update inventory file with your server IPs
3. Run: `ansible-playbook -i inventory setup-redis-sentinel.yml`

## Configuration Files
- `inventory`: Ansible inventory with server groups
- `setup-redis-sentinel.yml`: Main playbook
- `redis.conf.j2`: Redis server configuration template  
- `sentinel.conf.j2`: Redis Sentinel configuration template
- `vars.yml.example`: Example variables file

## Security
- Redis authentication enabled with password
- Firewall configured for required ports
- SELinux disabled for simplicity (production environments should configure properly)

## Testing
After deployment:
```bash
# Test Redis connectivity
redis-cli -h 172.16.90.160 -a abcdefgh12 ping

# Test Sentinel
redis-cli -h 172.16.90.160 -p 26379 sentinel masters
```

## Monitoring
Monitor logs at:
- Redis: /var/log/redis/redis.log  
- Sentinel: /var/log/redis/sentinel.log
