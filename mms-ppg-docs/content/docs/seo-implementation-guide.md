---
title: 'SEO Optimisation Guide'
description: 'How the SEO has been optimised for the site'
---
# SEO Implementation Guide

This document describes the SEO and metadata management system implemented for the MMS PPG website using Directus CMS integration.

## Overview

The SEO system provides:
- Dynamic metadata generation from Directus content
- Structured data (JSON-LD) for better search visibility
- XML sitemap generation from CMS collections
- Open Graph and Twitter Card support with PPG branding
- Automated robots.txt generation

## Components

### 1. SEO Service (`src/lib/seo/seo-service.ts`)

The main service that handles all SEO functionality:

```typescript
import { seoService } from '@/lib/seo/seo-service';

// Generate metadata for a page
const metadata = await seoService.generateMetadata(contentMetadata, customMetadata);

// Generate structured data
const structuredData = await seoService.generateStructuredData(contentMetadata, 'article');

// Generate sitemap entries
const sitemapEntries = await seoService.generateSitemapEntries();
```

### 2. Content Metadata Extraction (`src/lib/seo/content-metadata.ts`)

Utilities to extract SEO metadata from Directus content:

```typescript
import { extractBlogPostMetadata, extractEventMetadata } from '@/lib/seo/content-metadata';

// Extract metadata from blog post
const blogMetadata = extractBlogPostMetadata(blogPost);

// Extract metadata from event
const eventMetadata = extractEventMetadata(event);
```

### 3. Metadata Helpers (`src/lib/seo/metadata-helpers.ts`)

Helper functions for generating Next.js metadata:

```typescript
import { generateBlogPostMetadata, generateStaticPageMetadata } from '@/lib/seo/metadata-helpers';

// In a page component
export async function generateMetadata({ params }) {
  const post = await getBlogPost(params.slug);
  return await generateBlogPostMetadata(post);
}
```

### 4. Structured Data Component (`src/components/seo/StructuredData.tsx`)

React component for injecting structured data:

```tsx
import { StructuredDataComponent } from '@/components/seo/StructuredData';

// In a page component
<StructuredDataComponent data={structuredData} />
```

## Usage Examples

### Blog Post Page

```tsx
// src/app/news/[slug]/page.tsx
import { generateBlogPostMetadata } from '@/lib/seo/metadata-helpers';
import { StructuredDataComponent } from '@/components/seo/StructuredData';

export async function generateMetadata({ params }) {
  const post = await getBlogPost(params.slug);
  return await generateBlogPostMetadata(post);
}

export default async function BlogPostPage({ params }) {
  const post = await getBlogPost(params.slug);
  const contentMetadata = extractBlogPostMetadata(post);
  const structuredData = await seoService.generateStructuredData(contentMetadata, 'article');

  return (
    <>
      <StructuredDataComponent data={structuredData} />
      <article>
        {/* Page content */}
      </article>
    </>
  );
}
```

### Static Page

```tsx
// src/app/about/page.tsx
import { generateStaticPageMetadata } from '@/lib/seo/metadata-helpers';

export async function generateMetadata() {
  return await generateStaticPageMetadata(
    'About Us - MMS PPG',
    'Learn about our mission and impact in Mid Sussex healthcare.',
    '/about'
  );
}
```

## API Routes

### Sitemap (`/sitemap.xml`)

Automatically generates XML sitemap from Directus content:
- Static pages (homepage, about, services, etc.)
- Dynamic blog posts
- Events
- Custom pages from CMS

### Robots.txt (`/robots.txt`)

Generates robots.txt with:
- Sitemap reference
- Crawl delay settings
- Disallowed paths for admin areas

### Open Graph Images (`/api/og-image`)

Generates branded Open Graph images with PPG styling:
- Dynamic title and description
- PPG color scheme and branding
- Optimized 1200x630 format

## Configuration

### Environment Variables

```env
NEXT_PUBLIC_SITE_URL=https://mmsppg.org
DIRECTUS_URL=https://your-directus-instance.com
DIRECTUS_TOKEN=your-directus-token
```

### PPG Branding

The system uses PPG brand colors throughout:
- Primary Green: `#7CB342`
- Secondary Green: `#4CAF50`
- Accent Green: `#2E7D32`
- Supporting Green: `#A5D6A7`

## Testing

Run the SEO test suite:

```bash
npm run test:seo
```

This tests:
- Sitemap generation
- Structured data creation
- Metadata generation
- Content extraction
- XML format validation

## Best Practices

### 1. Metadata Optimization

- Keep titles under 60 characters
- Keep descriptions between 150-160 characters
- Use relevant keywords naturally
- Include location-specific terms (Mid Sussex, Midsussex)

### 2. Structured Data

- Always include Organization structured data on homepage
- Use Article structured data for blog posts
- Use Event structured data for events
- Include breadcrumb data for deep pages

### 3. Images

- Use PPG logo for default Open Graph images
- Optimize featured images for social sharing (1200x630)
- Always include alt text
- Use WebP format with fallbacks

### 4. Performance

- Enable ISR (Incremental Static Regeneration) for content pages
- Cache sitemap for 1 hour
- Use appropriate cache headers for SEO assets

## Monitoring

### Search Console Setup

1. Add property for https://mmsppg.org
2. Submit sitemap: https://mmsppg.org/sitemap.xml
3. Monitor crawl errors and indexing status

### Analytics Integration

The system integrates with Cloudflare Web Analytics for privacy-focused monitoring of:
- Page views and traffic sources
- Search query performance
- Core Web Vitals metrics

## Troubleshooting

### Common Issues

1. **Sitemap not updating**: Check Directus connection and content status
2. **Missing structured data**: Verify content metadata extraction
3. **OG images not loading**: Check API route and image generation
4. **Metadata not appearing**: Verify Next.js metadata generation

### Debug Tools

- Use `npm run test:seo` to verify implementation
- Check browser dev tools for structured data
- Use Google's Rich Results Test
- Validate sitemap with XML validators

## Future Enhancements

- Multi-language SEO support
- Advanced schema markup for healthcare content
- Integration with Google Analytics 4
- Automated SEO auditing and reporting
- Dynamic breadcrumb generation
- Enhanced social media integration