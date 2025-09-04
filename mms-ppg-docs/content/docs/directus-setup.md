---
title: 'Directus Setup Guide'
description: 'How Directus has been set up to help administer the MMS PPH site'
---
# Directus CMS Setup Guide for MMS PPG Website

This guide will help you set up Directus CMS for the MMS PPG website, replacing the previous Cockpit CMS integration.

## Why Directus?

Directus offers several advantages over Cockpit:
- **Better reliability**: More stable and actively maintained
- **Superior API**: Modern REST and GraphQL APIs
- **Excellent admin UI**: Intuitive content management interface
- **Flexible hosting**: Self-hosted or cloud options
- **Strong community**: Extensive documentation and support
- **Modern architecture**: Built with modern technologies

## Setup Options

### Option 1: Directus Cloud (Recommended)

1. **Sign up for Directus Cloud**
   - Go to [directus.cloud](https://directus.cloud)
   - Create a new project
   - Choose your region (UK/Europe recommended)

2. **Get your connection details**
   - Project URL: `https://your-project.directus.app`
   - Admin token: Generate in Settings > Access Tokens

### Option 2: Self-Hosted Directus

1. **Install Directus locally**
   ```bash
   npm create directus-project@latest mms-ppg-cms
   cd mms-ppg-cms
   npm start
   ```

2. **Or use Docker**
   ```bash
   docker run -d \
     --name directus \
     -p 8055:8055 \
     -e KEY="your-secret-key" \
     -e SECRET="your-secret" \
     -e DB_CLIENT="sqlite3" \
     -e DB_FILENAME="/directus/database/data.db" \
     -e ADMIN_EMAIL="admin@example.com" \
     -e ADMIN_PASSWORD="password" \
     -v directus_data:/directus/database \
     directus/directus:latest
   ```

## Configuration

### 1. Update Environment Variables

Update your `.env.local` file:

```env
# Directus Configuration
DIRECTUS_URL=https://your-project.directus.app
DIRECTUS_TOKEN=your-admin-token-here

# Optional
DIRECTUS_PREVIEW_SECRET=your-preview-secret
```

### 2. Create Collections

In your Directus admin panel, create these collections:

#### Blog Posts Collection (`blog_posts`)

| Field Name | Type | Options |
|------------|------|---------|
| `id` | UUID | Primary Key, Auto-generated |
| `title` | String | Required |
| `slug` | String | Required, Unique |
| `content` | Text (Rich Text) | Required |
| `excerpt` | Text | Optional |
| `featured_image` | File | Optional |
| `author` | String | Optional |
| `published_date` | DateTime | Required |
| `status` | Select | Options: draft, published |
| `tags` | JSON | Optional |
| `category` | String | Optional |

#### Events Collection (`events`)

| Field Name | Type | Options |
|------------|------|---------|
| `id` | UUID | Primary Key, Auto-generated |
| `title` | String | Required |
| `slug` | String | Required, Unique |
| `description` | Text (Rich Text) | Required |
| `event_date` | Date | Required |
| `event_time` | Time | Optional |
| `location` | String | Optional |
| `featured_image` | File | Optional |
| `registration_required` | Boolean | Default: false |
| `registration_url` | String | Optional |
| `status` | Select | Options: draft, published |
| `category` | String | Optional |

#### Pages Collection (`pages`)

| Field Name | Type | Options |
|------------|------|---------|
| `id` | UUID | Primary Key, Auto-generated |
| `title` | String | Required |
| `slug` | String | Required, Unique |
| `content` | Text (Rich Text) | Required |
| `meta_title` | String | Optional |
| `meta_description` | Text | Optional |
| `featured_image` | File | Optional |
| `status` | Select | Options: draft, published |

### 3. Set Permissions

For each collection, configure permissions:

1. **Public Role** (for website visitors):
   - Read: Allow for items where `status = published`
   - Create/Update/Delete: Deny

2. **Administrator Role**:
   - Full access to all operations

### 4. Configure API Access

1. Create a new role called "API Access"
2. Grant read permissions to all collections
3. Create a static token for this role
4. Use this token in your environment variables

## Testing the Connection

Run the connection test:

```bash
npm run test:directus
```

This will verify:
- ✅ Server connectivity
- ✅ Authentication
- ✅ Collection access
- ✅ Data retrieval

## Migration from Cockpit

### Data Migration

If you have existing content in Cockpit, you can migrate it:

1. **Export from Cockpit**: Use Cockpit's export functionality
2. **Transform data**: Convert to Directus format
3. **Import to Directus**: Use Directus import tools or API

### Code Migration

The code has been updated to use Directus, but maintains compatibility:
- All existing components will continue to work
- The CMS adapter handles the transformation
- No changes needed to your React components

## Content Management

### Adding Content

1. **Blog Posts**:
   - Go to Content > Blog Posts
   - Click "Create Item"
   - Fill in title, content, and other fields
   - Set status to "published" when ready

2. **Events**:
   - Go to Content > Events
   - Click "Create Item"
   - Add event details and date
   - Set status to "published" when ready

3. **Pages**:
   - Go to Content > Pages
   - Create static pages like "About", "Contact", etc.

### Media Management

- Upload images in Files & Media
- Reference them in your content
- Directus automatically handles image optimization

## Advanced Features

### GraphQL API

Directus also provides a GraphQL endpoint:
```
https://your-project.directus.app/graphql
```

### Webhooks

Set up webhooks to trigger builds when content changes:
1. Go to Settings > Webhooks
2. Add webhook URL (e.g., Vercel deploy hook)
3. Configure triggers (create, update, delete)

### Custom Fields

Add custom fields as needed:
- SEO fields (meta title, description)
- Social media images
- Custom taxonomies

## Troubleshooting

### Common Issues

1. **Connection Failed**
   - Check DIRECTUS_URL is correct
   - Verify token has proper permissions
   - Ensure collections exist

2. **Empty Results**
   - Check collection permissions
   - Verify items are published
   - Check field names match schema

3. **Image Issues**
   - Ensure files are uploaded to Directus
   - Check file permissions
   - Verify asset URLs are correct

### Getting Help

- [Directus Documentation](https://docs.directus.io/)
- [Directus Discord Community](https://discord.gg/directus)
- [GitHub Issues](https://github.com/directus/directus/issues)

## Next Steps

1. Set up your Directus instance
2. Create the required collections
3. Configure permissions
4. Test the connection
5. Start adding content
6. Deploy your website

Your MMS PPG website is now powered by Directus! 🎉