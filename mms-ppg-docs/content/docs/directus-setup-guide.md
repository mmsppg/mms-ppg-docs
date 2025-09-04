---
title: 'Directus Setup Guide v2'
description: 'How to use Directus to get up and running and editing the site'
---
# Directus CMS Setup Guide for MMS PPG Website

## Overview

This guide will help you set up and configure Directus CMS for the MMS PPG website. Directus is already running at `https://mmsppg.themainhost.co.uk`.

## 🚀 Quick Setup Steps

### 1. Access Directus Admin Panel

1. Open your browser and go to: `https://mmsppg.themainhost.co.uk/admin`
2. Log in with your Directus admin credentials
3. If you don't have credentials, contact your hosting provider

### 2. Create API Token

1. In the Directus admin panel, go to **Settings** → **Access Tokens**
2. Click **Create Token**
3. Configure the token:
   - **Name**: `MMS PPG Website API`
   - **Role**: Select a role with appropriate permissions (or create a new one)
   - **Expires**: Set to never expire or a far future date
4. Copy the generated token
5. Update your `.env.local` file:
   ```env
   DIRECTUS_TOKEN=your-actual-token-here
   ```

### 3. Create Required Collections

Create the following collections in Directus with these fields:

#### Blog Posts Collection (`blog_posts`)
- **id** (Primary Key, Auto-increment)
- **title** (String, Required)
- **slug** (String, Required, Unique)
- **content** (Text/Rich Text)
- **excerpt** (Text)
- **featured_image** (File/Image)
- **author** (String)
- **published_date** (DateTime)
- **status** (Dropdown: published, draft)
- **tags** (JSON or Tags relation)
- **category** (String)

#### Events Collection (`events`)
- **id** (Primary Key, Auto-increment)
- **title** (String, Required)
- **slug** (String, Required, Unique)
- **description** (Text/Rich Text)
- **event_date** (Date, Required)
- **event_time** (Time)
- **location** (String)
- **featured_image** (File/Image)
- **registration_required** (Boolean)
- **registration_url** (String/URL)
- **status** (Dropdown: published, draft)
- **category** (String)

#### Pages Collection (`pages`)
- **id** (Primary Key, Auto-increment)
- **title** (String, Required)
- **slug** (String, Required, Unique)
- **content** (Text/Rich Text)
- **meta_title** (String)
- **meta_description** (Text)
- **featured_image** (File/Image)
- **status** (Dropdown: published, draft)

### 4. Configure Permissions

Set up permissions for your API token's role:

#### Public Role (for unauthenticated access)
- **Read** access to all collections where `status = published`
- **No** create, update, or delete permissions

#### API Role (for your token)
- **Read** access to all collections
- **Create, Update, Delete** access as needed
- Access to **Files** collection for image uploads

### 5. Test the Connection

After setting up the token and collections, test the connection:

```bash
npm run test:directus
```

You should see all tests pass if everything is configured correctly.

## 🔧 Advanced Configuration

### Custom Fields

You can add additional fields to any collection as needed:

- **SEO fields**: meta_keywords, og_image, etc.
- **Content fields**: summary, call_to_action, etc.
- **Organizational fields**: priority, featured, etc.

### File Storage

Configure file storage for images:

1. Go to **Settings** → **Project Settings** → **Storage**
2. Configure your preferred storage adapter (local, S3, etc.)
3. Set up image transformations for different sizes

### Webhooks (Optional)

Set up webhooks to trigger Next.js revalidation:

1. Go to **Settings** → **Webhooks**
2. Create webhook for content updates
3. Point to your Next.js revalidation endpoint

## 📝 Content Management

### Creating Content

1. **Blog Posts**: Go to **Content** → **Blog Posts** → **Create Item**
2. **Events**: Go to **Content** → **Events** → **Create Item**
3. **Pages**: Go to **Content** → **Pages** → **Create Item**

### Publishing Workflow

1. Create content with `status = draft`
2. Preview content on your website
3. Set `status = published` when ready
4. Content will appear on your live website

## 🔍 Troubleshooting

### Common Issues

1. **401 Unauthorized**: Check your API token and permissions
2. **404 Collection Not Found**: Ensure collections are created with correct names
3. **403 Forbidden**: Check role permissions for your API token
4. **CORS Issues**: Configure CORS settings in Directus if needed

### Testing Commands

```bash
# Test Directus connection
npm run test:directus

# Test Next.js integration
npm run dev
```

### Useful Directus URLs

- **Admin Panel**: `https://mmsppg.themainhost.co.uk/admin`
- **API Documentation**: `https://mmsppg.themainhost.co.uk/server/specs/oas`
- **Server Info**: `https://mmsppg.themainhost.co.uk/server/info`

## 📚 Resources

- [Directus Documentation](https://docs.directus.io/)
- [Directus SDK Documentation](https://docs.directus.io/reference/sdk/)
- [Next.js Integration Guide](https://docs.directus.io/blog/getting-started-directus-nextjs/)

## 🎯 Next Steps

After completing this setup:

1. ✅ Test the connection with `npm run test:directus`
2. ✅ Create some sample content in Directus
3. ✅ Run your Next.js development server with `npm run dev`
4. ✅ Verify content appears on your website
5. ✅ Set up automated deployments (optional)

Your Directus CMS integration will be ready to use!