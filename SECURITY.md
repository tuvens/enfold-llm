# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in this project, please report it responsibly:

1. **Do NOT open a public issue**
2. Email the maintainers directly or use GitHub's private vulnerability reporting
3. Include details about the vulnerability and steps to reproduce

We will respond within 48 hours and work with you to understand and address the issue.

## Security Best Practices

When using this project:

### Credentials
- **Never commit credentials** to your repository
- Use GitHub Secrets for `USERNAME` and `APP_PASSWORD`
- Rotate Application Passwords periodically
- Use a dedicated WordPress user with minimal required permissions

### WordPress
- Keep WordPress, Enfold, and plugins updated
- Use HTTPS (required for Application Passwords)
- Consider IP restrictions on wp-admin if possible

### Repository
- Enable branch protection on `main`
- Require pull request reviews for production changes
- Enable GitHub's security features (Dependabot, secret scanning)

## Supported Versions

| Version | Supported |
|---------|-----------|
| Latest  | ✅ Yes    |
| < 1.0   | ❌ No     |

## Known Limitations

- Application Passwords grant full API access for the user's permission level
- The GitHub Actions workflow has write access to your WordPress site
- Meta files contain WordPress resource IDs (not sensitive, but be aware)
