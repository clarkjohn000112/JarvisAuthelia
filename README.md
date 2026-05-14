# Authelia with Public LDAP Test Server

This setup uses the public LDAP test server from forumsys.com for testing Authelia with LDAP authentication and role-based access control.

## LDAP Server Information
- **Server**: ldap.forumsys.com:389
- **Bind DN**: cn=read-only-admin,dc=example,dc=com
- **Bind Password**: password
- **Base DN**: dc=example,dc=com

## Test Users (all passwords: `password`)

### Mathematicians Group
- **riemann** - Bernhard Riemann
- **gauss** - Carl Gauss
- **euler** - Leonhard Euler
- **euclid** - Euclid

### Scientists Group
- **einstein** - Albert Einstein
- **newton** - Isaac Newton
- **galieleo** - Galileo Galilei
- **tesla** - Nikola Tesla

## Available Test Applications

1. **auth.example.local** → Bypass (no auth required)
2. **jarvis.example.local** → Pedeng one_factor/two_factor

## Testing

Access Authelia at: `https://auth.example.local`

**Test Users** (password: `password`):
- **riemann**, **gauss**, **euler**, **euclid** (mathematicians)
- **einstein**, **newton**, **galieleo**, **tesla** (scientists)

## Setup

1. Create environment file `.env`:

DOMAIN=example.local
TZ=UTC
ACME_EMAIL=your-email@example.com

2. Create external network:

docker network create proxy

3. Run the services:
docker-compose up -d

4. Add to `/etc/hosts`:
127.0.0.1 example.local auth.example.local jarvis.example.local traefik.example.local

## Testing
Access Authelia at: `https://auth.example.local`
Log in with any test user credentials above.

## Useful LDAP Commands
Query all users:
ldapsearch -x -H ldap://ldap.forumsys.com:389 -D cn=read-only-admin,dc=example,dc=com -w password -b dc=example,dc=com

Query groups:
ldapsearch -x -H ldap://ldap.forumsys.com:389 -D cn=read-only-admin,dc=example,dc=com -w password -b dc=example,dc=com "(objectClass=groupOfUniqueNames)"

For searching, editing and maintaining your own LDAP server, or for connecting to this Online Test LDAP instance, we recommend Apache Directory Studio.
