# DNS Migration Details: Squarespace → GitHub Pages

This document provides the exact DNS records for migrating esabbr.com from Squarespace to GitHub Pages while preserving Google Workspace email functionality.

## Current DNS Records (Squarespace)

| Host | Type | Priority | TTL | Data |
|------|------|----------|-----|------|
| @ | A | 0 | 4 hrs | 198.49.23.144 |
| @ | A | 0 | 4 hrs | 198.185.159.144 |
| @ | A | 0 | 4 hrs | 198.185.159.145 |
| @ | A | 0 | 4 hrs | 198.49.23.145 |
| www | CNAME | 0 | 4 hrs | ext-sq.squarespace.com |
| @ | HTTPS | 0 | 4 hrs | alpn="h2,http/1.1" ipv4hint="198.185.159.144,198.185.159.145,198.49.23.144,198.49.23.145" |
| _domainconnect | CNAME | 0 | 4 hrs | _domainconnect.domains.squarespace.com |
| google._domainkey | TXT | N/A | 1 hr | v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAmSB1ZSYXMvmKUD9CDewuGe2XGLm664C525+/lVwzjcbDBM9gQkk/W3beAUran2PgiOlAtymEReHGR3gv5ZIOIlWFWR2n6kRxGe3aJcWWr6i1YZ4hu4NZ6ryrBr9CgeVO42Q4bZSOv4yXYaEj+pyIHsq0Nn0DVBuY74k7FwttqnvHfhxm7BCcd3aeCWbVXX5miKF5rwliRHT8LI84qHC5Waatz//jEWnJ9aMDrJ2hBkpwRhAh2iYDLV6WzcxGldr/EUF/YDGpXqaWNspsbhCEISOX0Cc4FbiV8QyQp0tHb8H/RsPaCEVUFRYGMOuhk2qJRVRiVr8lSFhAMtzJhVIqvwIDAQAB |
| @ | TXT | N/A | 1 hr | v=spf1 include:_spf.google.com ~all |
| @ | MX | 1 | 1 hr | smtp.google.com |

## New DNS Configuration (GitHub Pages)

### Records to Keep (Google Workspace Email)
These records **MUST** be preserved to maintain email functionality:

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| MX | @ | 1 smtp.google.com | Email routing |
| TXT | @ | v=spf1 include:_spf.google.com ~all | Email authentication |
| TXT | google._domainkey | v=DKIM1; k=rsa; p=MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAmSB1ZSYXMvmKUD9CDewuGe2XGLm664C525+/lVwzjcbDBM9gQkk/W3beAUran2PgiOlAtymEReHGR3gv5ZIOIlWFWR2n6kRxGe3aJcWWr6i1YZ4hu4NZ6ryrBr9CgeVO42Q4bZSOv4yXYaEj+pyIHsq0Nn0DVBuY74k7FwttqnvHfhxm7BCcd3aeCWbVXX5miKF5rwliRHT8LI84qHC5Waatz//jEWnJ9aMDrJ2hBkpwRhAh2iYDLV6WzcxGldr/EUF/YDGpXqaWNspsbhCEISOX0Cc4FbiV8QyQp0tHb8H/RsPaCEVUFRYGMOuhk2qJRVRiVr8lSFhAMtzJhVIqvwIDAQAB | DKIM signing |
| CNAME | _domainconnect | _domainconnect.domains.squarespace.com | Domain management |

### Records to Add (GitHub Pages)

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| A | @ | 185.199.108.153 | GitHub Pages hosting |
| A | @ | 185.199.109.153 | GitHub Pages hosting |
| A | @ | 185.199.110.153 | GitHub Pages hosting |
| A | @ | 185.199.111.153 | GitHub Pages hosting |
| CNAME | www | poisontr33s.github.io | WWW subdomain |
| TXT | @ | github-domain-verification=[CODE] | Domain verification |

### Records to Remove

- **Current A records**: 198.49.23.144, 198.185.159.144, 198.185.159.145, 198.49.23.145
- **HTTPS record**: Remove completely (GitHub handles SSL)
- **Current www CNAME**: ext-sq.squarespace.com

## Post-DNS Migration Steps

1. Add a `CNAME` file in your repo with `esabbr.com`
2. In GitHub Pages settings, set the custom domain to `esabbr.com`
3. Wait for DNS propagation (minutes to hours)
4. Enable "Enforce HTTPS" once the certificate is issued

## Migration Checklist

- [ ] Keep MX record for smtp.google.com
- [ ] Keep SPF TXT record
- [ ] Keep DKIM TXT record for google._domainkey
- [ ] Keep _domainconnect CNAME
- [ ] Replace A records with GitHub Pages IPs
- [ ] Update www CNAME to point to poisontr33s.github.io
- [ ] Remove HTTPS record
- [ ] Add GitHub domain verification TXT record
- [ ] Configure GitHub Pages custom domain
- [ ] Wait for DNS propagation
- [ ] Test email functionality
- [ ] Verify website loads correctly

## Support

- **DNS Issues**: Contact your domain registrar
- **GitHub Pages**: Check [GitHub Pages documentation](https://docs.github.com/en/pages)
- **Email Issues**: Verify Google Workspace DNS records are intact