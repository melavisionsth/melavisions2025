# Deployment Guide

## Cloudflare Pages Deployment

This Astro blog is configured for automatic deployment to Cloudflare Pages via GitHub Actions.

### Prerequisites

1. A Cloudflare account
2. A Cloudflare Pages project
3. GitHub repository secrets configured

### Setup Steps

#### 1. Get Cloudflare Credentials

**Get your Account ID:**
- Log in to the [Cloudflare Dashboard](https://dash.cloudflare.com)
- Go to Workers & Pages > Overview
- Find your Account ID on the right sidebar

**Create an API Token:**
- Go to [API Tokens](https://dash.cloudflare.com/profile/api-tokens)
- Click "Create Token"
- Use the "Edit Cloudflare Workers" template
- Add "Cloudflare Pages" permissions:
  - Account > Cloudflare Pages > Edit
- Click "Continue to summary" and "Create Token"
- Copy the token (you won't see it again!)

#### 2. Create Cloudflare Pages Project

- Go to Workers & Pages > Create
- Click "Create Application" > "Pages"
- Connect your GitHub repository
- Configure build settings:
  - **Framework preset:** Astro
  - **Build command:** `npm run build`
  - **Build output directory:** `dist`
  - **Root directory:** `/`
- Click "Save and Deploy"

#### 3. Configure GitHub Secrets

Go to your GitHub repository > Settings > Secrets and variables > Actions

Add these secrets:
- `CLOUDFLARE_API_TOKEN`: Your API token from step 1
- `CLOUDFLARE_ACCOUNT_ID`: Your Account ID from step 1

### Manual Deployment

You can also deploy manually using the Wrangler CLI:

```bash
# Install dependencies
npm install

# Build the project
npm run build

# Deploy to Cloudflare Pages
npm run deploy
```

### Automatic Deployment

The GitHub Actions workflow (`.github/workflows/deploy.yml`) automatically:
- Runs on every push to the `main` branch
- Installs dependencies
- Builds the Astro site
- Deploys to Cloudflare Pages

You can also trigger a manual deployment from the Actions tab.

### Environment Variables

If you need environment variables in production:

1. Go to your Cloudflare Pages project
2. Navigate to Settings > Environment variables
3. Add your variables
4. Redeploy to apply changes

### Troubleshooting

**Build fails:**
- Check the Actions logs in GitHub
- Verify all dependencies are in `package.json`
- Ensure Node version matches (currently set to v20)

**Deployment fails:**
- Verify your Cloudflare API token is valid
- Check that your Account ID is correct
- Ensure the project name matches in the workflow file

### Custom Domain

To add a custom domain:

1. Go to your Cloudflare Pages project
2. Click "Custom domains"
3. Add your domain
4. Update DNS records as instructed
5. Update the `site` field in `astro.config.mjs` to your custom domain

## Performance Optimizations

- Static assets are cached at Cloudflare's edge
- Astro generates optimized static HTML
- Images are automatically optimized
- CSS and JS are minified

## Monitoring

View deployment logs:
- GitHub Actions: Repository > Actions tab
- Cloudflare: Dashboard > Workers & Pages > Your project > Deployments
