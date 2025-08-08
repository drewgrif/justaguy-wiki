# JustAGuy Wiki

This repository automatically syncs content from the [JustAGuyLinux documentation wiki](https://github.com/drewgrif/docs.justaguylinux.com/wiki) and publishes it to [justaguy.wiki](https://justaguy.wiki).

## How it works

1. **Content is edited** in the source wiki: https://github.com/drewgrif/docs.justaguylinux.com/wiki
2. **GitHub Action syncs** the content daily (or when manually triggered)
3. **Jekyll builds** the site with proper navigation and styling
4. **GitHub Pages deploys** to the custom domain: https://justaguy.wiki

## Setup Instructions

### 1. Create the Repository

1. Create a new repository called `wiki` (or `justaguy-wiki`)
2. Copy all files from the `new-repo-setup/` folder to the root of the new repository
3. Commit and push to the `main` branch

### 2. Configure GitHub Pages

1. Go to **Settings** → **Pages** in your repository
2. Set **Source** to "Deploy from a branch"
3. Set **Branch** to `main` and folder to `/ (root)`
4. The **Custom domain** should automatically be set to `justaguy.wiki`

### 3. Configure DNS at Porkbun

Add these DNS records for `justaguy.wiki`:

```
Type: CNAME
Name: @
Target: yourusername.github.io
```

OR use A records:

```
Type: A
Name: @
Target: 185.199.108.153

Type: A  
Name: @
Target: 185.199.109.153

Type: A
Name: @  
Target: 185.199.110.153

Type: A
Name: @
Target: 185.199.111.153
```

### 4. Test the Setup

1. **Trigger the sync**: Go to **Actions** → **Sync Wiki Content** → **Run workflow**
2. **Wait for build**: GitHub Pages will build and deploy
3. **Visit your site**: https://justaguy.wiki

## Features

- ✅ **Auto-sync** from source wiki daily
- ✅ **Custom domain** support (justaguy.wiki)
- ✅ **Mobile responsive** design
- ✅ **Sidebar navigation** from wiki sidebar
- ✅ **SEO optimized** with proper meta tags
- ✅ **Dark mode** support
- ✅ **Fast loading** static site

## Manual Sync

To manually sync content:
1. Go to the **Actions** tab
2. Click **Sync Wiki Content**
3. Click **Run workflow**

## Customization

- Edit `_config.yml` to update site settings
- Modify `_layouts/default.html` to change the design
- Add Google Analytics by setting `google_analytics` in `_config.yml`

## Troubleshooting

- **Links not working?** The sync process converts wiki links to Jekyll format
- **Changes not showing?** Check the Actions tab for sync/build status  
- **Domain not working?** Verify DNS settings and GitHub Pages configuration

---

**Source Wiki**: https://github.com/drewgrif/docs.justaguylinux.com/wiki  
**Live Site**: https://justaguy.wiki  
**Main Site**: https://justaguylinux.com