---
title: 'Logo Integration Guide'
description: 'A quick guide on setting up the logo to the site'
---
## Logo Integration Guide

This guide explains how to integrate the MMS PPG logo from Directus into the website header.

## Overview

The website now supports dynamic logo loading from Directus CMS. The logo is fetched from the `site_settings` collection and displayed in the header with automatic fallback to a stylized placeholder if the logo is unavailable.

## Setup Instructions

### 1. Set up Directus Site Settings Collection

First, create the site settings collection in your Directus instance:

```bash
npm run setup:site-settings
```

This script will create a `site_settings` collection with the following fields:
- `site_name` - The name of the website
- `site_description` - Brief description
- `logo` - Site logo image (file upload)
- `favicon` - Site favicon (file upload)
- `primary_color` - Primary brand color
- `secondary_color` - Secondary brand color
- `contact_email` - Main contact email
- `contact_phone` - Main contact phone
- `address` - Physical address
- `social_media` - Social media links (JSON)
- `seo_settings` - SEO settings (JSON)

### 2. Upload Your Logo

1. Go to your Directus admin panel
2. Navigate to **Site Settings**
3. Click on the **Logo** field
4. Upload your MMS PPG logo image
5. Save the settings

### 3. Configure Environment Variables

Make sure your `.env.local` file contains the Directus configuration:

```env
DIRECTUS_URL=http://localhost:8055
DIRECTUS_TOKEN=your_directus_token_here
```

## How It Works

### Logo Component

The `Logo` component (`src/components/ui/Logo.tsx`) handles:
- Fetching the logo URL from Directus
- Loading states with skeleton animation
- Error handling with fallback display
- Automatic image optimization via Next.js Image component
- Responsive sizing and styling

### Site Settings Service

The `SiteSettingsService` (`src/lib/cms/site-settings-service.ts`) provides:
- Cached logo fetching (5-minute cache)
- Asset URL generation with transformations
- Error handling and retry logic
- TypeScript interfaces for type safety

### Header Integration

The header component now uses the Logo component:
```tsx
<Logo 
  width={48} 
  height={48} 
  priority={true}
  className="rounded-lg shadow-md"
/>
```

## Features

### Automatic Fallback

If the logo fails to load or isn't available, the component automatically shows a stylized placeholder that represents the PPG community figures pattern.

### Image Optimization

The logo is automatically optimized by Next.js Image component:
- Responsive sizing
- WebP format conversion
- Lazy loading (except when `priority={true}`)
- Proper alt text for accessibility

### Caching

Logo URLs are cached for 5 minutes to improve performance and reduce API calls to Directus.

### Error Handling

The component gracefully handles:
- Network errors
- Missing logo files
- Directus connection issues
- Invalid image formats

## Usage Examples

### Basic Usage
```tsx
import Logo from '@/components/ui/Logo';

<Logo />
```

### Custom Sizing
```tsx
<Logo width={64} height={64} />
```

### High Priority (for above-the-fold content)
```tsx
<Logo priority={true} />
```

### Custom Styling
```tsx
<Logo className="rounded-full border-2 border-blue-500" />
```

### Without Fallback
```tsx
<Logo showFallback={false} />
```

## Troubleshooting

### Logo Not Displaying

1. **Check Directus Connection**
   ```bash
   # Test Directus connection
   curl http://localhost:8055/server/info
   ```

2. **Verify Environment Variables**
   - Ensure `DIRECTUS_URL` and `DIRECTUS_TOKEN` are set correctly
   - Check that the Directus instance is running

3. **Check Site Settings**
   - Go to Directus admin panel
   - Verify that Site Settings exists and has a logo uploaded
   - Check that the logo file is accessible

4. **Browser Console**
   - Open browser developer tools
   - Check for any error messages in the console
   - Look for network errors in the Network tab

### Performance Issues

1. **Enable Caching**
   - The service automatically caches logo URLs for 5 minutes
   - Clear cache if needed: `siteSettingsService.clearCache()`

2. **Image Optimization**
   - Use appropriate image sizes (recommended: 48x48 to 128x128 pixels)
   - Use modern formats (WebP, PNG with transparency)
   - Compress images before uploading

### Accessibility

The Logo component includes:
- Proper alt text: "MMS PPG Logo"
- Screen reader support
- High contrast fallback options
- Keyboard navigation support (when used in links)

## API Reference

### Logo Component Props

| Prop | Type | Default | Description |
|------|------|---------|-------------|
| `className` | `string` | `''` | Additional CSS classes |
| `width` | `number` | `48` | Logo width in pixels |
| `height` | `number` | `48` | Logo height in pixels |
| `priority` | `boolean` | `false` | Load image with high priority |
| `showFallback` | `boolean` | `true` | Show fallback when logo unavailable |

### Site Settings Service Methods

| Method | Description |
|--------|-------------|
| `getSiteSettings()` | Get all site settings |
| `getLogoUrl()` | Get logo URL only |
| `getAsset(id)` | Get asset information |
| `getAssetUrl(id, transforms)` | Get asset URL with transformations |
| `clearCache()` | Clear cached settings |

## Best Practices

1. **Logo Requirements**
   - Use square or rectangular logos (1:1 or 16:9 ratio)
   - Minimum size: 48x48 pixels
   - Maximum size: 512x512 pixels
   - Formats: PNG (with transparency), SVG, or WebP

2. **Performance**
   - Use `priority={true}` for above-the-fold logos
   - Keep logo file sizes under 100KB
   - Use appropriate dimensions for your use case

3. **Accessibility**
   - Ensure logos have sufficient contrast
   - Test with screen readers
   - Provide meaningful alt text if customizing

4. **Fallback Design**
   - The fallback represents community figures
   - Maintains brand colors and styling
   - Provides visual consistency when logo unavailable