# ESABBR.com - GitHub Pages Static Site

Static site for [esabbr.com](https://www.esabbr.com) migrated from Squarespace to GitHub Pages.

## DNS Migration Guide: Squarespace → GitHub Pages

This guide helps you migrate your domain from Squarespace to GitHub Pages while preserving Google Workspace email functionality.

### 🎯 Migration Overview

When migrating from Squarespace to GitHub Pages, you need to:
1. **Preserve** Google Workspace email records (MX, TXT, DKIM)
2. **Add** GitHub domain verification TXT record
3. **Replace** A records with GitHub Pages IPs
4. **Update** www CNAME to point to GitHub Pages
5. **Remove** HTTPS records (GitHub handles SSL automatically)
6. **Keep** _domainconnect records

### 📋 Step-by-Step DNS Configuration

#### 1. Keep These Google Workspace Records (DO NOT DELETE)

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| MX | @ | 1 aspmx.l.google.com | Primary mail server |
| MX | @ | 5 alt1.aspmx.l.google.com | Backup mail server |
| MX | @ | 5 alt2.aspmx.l.google.com | Backup mail server |
| MX | @ | 10 alt3.aspmx.l.google.com | Backup mail server |
| MX | @ | 10 alt4.aspmx.l.google.com | Backup mail server |
| TXT | @ | v=spf1 include:_spf.google.com ~all | Email authentication |
| TXT | google._domainkey | v=DKIM1; k=rsa; p=[YOUR_DKIM_KEY] | DKIM signing |
| CNAME | _domainconnect | _domainconnect.gd.domaincontrol.com | Domain management |

> **⚠️ Important**: The exact DKIM key value will be provided after repository creation.

#### 2. Add GitHub Domain Verification

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| TXT | @ | github-domain-verification=[VERIFICATION_CODE] | GitHub domain ownership |

> **📝 Note**: Get your verification code from GitHub Pages settings in your repository.

#### 3. Replace A Records with GitHub Pages IPs

**Remove any existing A records** and add these GitHub Pages IP addresses:

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| A | @ | 185.199.108.153 | GitHub Pages IP |
| A | @ | 185.199.109.153 | GitHub Pages IP |
| A | @ | 185.199.110.153 | GitHub Pages IP |
| A | @ | 185.199.111.153 | GitHub Pages IP |

#### 4. Update www CNAME

| Type | Name | Value | Purpose |
|------|------|-------|---------|
| CNAME | www | poisontr33s.github.io | Redirect www to GitHub Pages |

#### 5. Remove These Records

- **HTTPS records**: GitHub Pages handles SSL certificates automatically
- **Old Squarespace A records**: Replace with GitHub Pages IPs above
- **Old www CNAME**: Update to point to your GitHub Pages domain

### 🔧 DNS Provider Configuration

#### Common DNS Providers

**Cloudflare:**
1. Go to DNS tab in your domain dashboard
2. Delete old A and CNAME records
3. Add new records as specified above
4. Set Proxy Status to "DNS Only" (grey cloud) for GitHub Pages

**GoDaddy:**
1. Go to DNS Management
2. Delete existing A and CNAME records
3. Add new records with specified values
4. TTL can be set to 1 hour (3600 seconds)

**Namecheap:**
1. Go to Advanced DNS
2. Remove old Host Records
3. Add new records as specified
4. Save changes

### ✅ Verification Steps

1. **DNS Propagation**: Wait 24-48 hours for full DNS propagation
2. **GitHub Pages**: Verify custom domain is working in repository settings
3. **SSL Certificate**: GitHub will automatically provision SSL (may take a few hours)
4. **Email Testing**: Send test emails to ensure Google Workspace still works
5. **Domain Verification**: Confirm GitHub domain verification passes

### 🚨 Troubleshooting

**Email not working?**
- Verify MX records are correctly configured
- Check SPF record includes `include:_spf.google.com`
- Ensure DKIM record is properly formatted

**Site not loading?**
- Check A records point to correct GitHub Pages IPs
- Verify CNAME file contains correct domain name
- Confirm GitHub Pages is enabled in repository settings

**SSL certificate issues?**
- Wait up to 24 hours for automatic certificate provisioning
- Ensure no conflicting HTTPS records exist
- Check domain verification is complete

### 📞 Support

For DNS-specific issues, contact your domain provider. For GitHub Pages issues, check the [GitHub Pages documentation](https://docs.github.com/en/pages).

---

## Development

This is a static site deployed via GitHub Pages. The main files are:

- `index.html` - Main landing page
- `CNAME` - Custom domain configuration  
- `.github/workflows/pages.yml` - Deployment workflow

### Local Development

Simply open `index.html` in your browser or use a local server:

```bash
python -m http.server 8000
# or
npx serve .
```

### Deployment

Deployment is automatic via GitHub Actions when pushing to the `main` branch.
