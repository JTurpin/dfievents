# GitHub Pages Setup Guide for dfievents.com

This guide will walk you through configuring GitHub Pages to host your Hugo site with the custom domain `dfievents.com`.

## Prerequisites

✅ DNS records have been updated for `dfievents.com` (you mentioned this is already done)
✅ GitHub Actions workflow is configured (`.github/workflows/hugo.yml`)
✅ CNAME file is created (`static/CNAME`)

## Step-by-Step Configuration

### 1. Enable GitHub Pages in Repository Settings

1. Go to your GitHub repository: `https://github.com/YOUR_USERNAME/dfievents`
2. Click on **Settings** (top navigation bar)
3. Scroll down to **Pages** in the left sidebar
4. Under **Source**, select:
   - **Source**: `GitHub Actions`
   - This will use the workflow file we've created

### 2. Configure Custom Domain

1. Still in the **Pages** settings section
2. Under **Custom domain**, enter: `dfievents.com`
3. Check the box **Enforce HTTPS** (recommended)
4. Click **Save**

### 3. Verify DNS Configuration

GitHub Pages requires specific DNS records. Since you've already updated DNS, verify:

**For apex domain (dfievents.com):**
- **A Records** pointing to GitHub Pages IPs:
  - `185.199.108.153`
  - `185.199.109.153`
  - `185.199.110.153`
  - `185.199.111.153`

**For www subdomain (www.dfievents.com):**
- **CNAME Record** pointing to: `YOUR_USERNAME.github.io`

**Note:** GitHub will automatically create a CNAME file in your repository when you configure the custom domain. The one we created in `static/CNAME` will also be included in the build output.

### 4. Trigger the First Deployment

1. Commit and push the changes (workflow file and CNAME):
   ```bash
   git add .github/workflows/hugo.yml static/CNAME
   git commit -m "Configure GitHub Pages deployment"
   git push origin master
   ```

2. Go to the **Actions** tab in your GitHub repository
3. You should see the workflow running: "Deploy Hugo site to Pages"
4. Wait for it to complete (usually takes 1-2 minutes)

### 5. Verify Deployment

1. Once the workflow completes successfully:
   - Go back to **Settings** → **Pages**
   - You should see a green checkmark indicating the site is published
   - The URL should show: `https://dfievents.com`

2. Visit `https://dfievents.com` in your browser
3. It may take a few minutes for DNS propagation, but the site should be accessible

## Troubleshooting

### Workflow Fails
- Check the **Actions** tab for error messages
- Ensure Hugo version compatibility (currently using 0.128.0)
- Verify the theme submodule is properly configured

### DNS Issues
- Use `dig dfievents.com` or `nslookup dfievents.com` to verify DNS records
- DNS propagation can take up to 24-48 hours, but usually happens within minutes

### Custom Domain Not Working
- Ensure the CNAME file exists in the deployed site (check the `public` folder after build)
- Verify HTTPS is enabled in GitHub Pages settings
- Check that DNS records are correct

### Site Not Updating
- The workflow runs automatically on every push to `master`
- You can also manually trigger it from the **Actions** tab → **Deploy Hugo site to Pages** → **Run workflow**

## What Happens on Each Push

1. GitHub Actions detects a push to `master` branch
2. Workflow runs and:
   - Installs Hugo Extended (0.128.0)
   - Checks out your code (including theme submodule)
   - Builds the site with `hugo --minify --baseURL "https://dfievents.com/"`
   - Uploads the `public` folder as an artifact
   - Deploys to GitHub Pages
3. Your site is live at `https://dfievents.com`

## Files Created/Modified

- ✅ `.github/workflows/hugo.yml` - GitHub Actions workflow
- ✅ `static/CNAME` - Custom domain configuration file

## Additional Notes

- The workflow uses Hugo Extended version 0.128.0 (supports SCSS/SASS)
- The site is built with `--minify` flag for optimized output
- The `public` folder is automatically generated during build (don't commit it)
- Theme is included as a submodule, so ensure it's properly initialized

## Need Help?

- [GitHub Pages Documentation](https://docs.github.com/en/pages)
- [Hugo Documentation](https://gohugo.io/documentation/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)

