---
title: 'Performance Optimisation Guide'
description: 'How the site has been optimised for performance'
---
# Performance Optimization Guide

This document outlines the comprehensive performance optimizations implemented for the MMS PPG website to achieve optimal Core Web Vitals and user experience.

## Overview

The website has been optimized to meet the following performance targets:

- **Lighthouse Performance Score**: 90+
- **Largest Contentful Paint (LCP)**: < 2.5s
- **First Input Delay (FID)**: < 100ms
- **Cumulative Layout Shift (CLS)**: < 0.1
- **First Contentful Paint (FCP)**: < 1.8s
- **Time to First Byte (TTFB)**: < 800ms

## Implemented Optimizations

### 1. Next.js Configuration Optimizations

#### Static Site Generation (SSG) and Incremental Static Regeneration (ISR)

```javascript
// Homepage with 1-hour revalidation
export const revalidate = 3600;

// Services page with 30-minute revalidation
export const revalidate = 1800;
```

**Benefits:**
- Pre-rendered pages for instant loading
- Automatic background updates
- Reduced server load
- Better SEO performance

#### Bundle Optimization

```javascript
// next.config.js
experimental: {
  optimizeCss: true,
  optimizePackageImports: ['@heroicons/react', '@directus/sdk'],
}
```

**Features:**
- CSS optimization and minification
- Package import optimization
- Tree shaking for unused code
- Bundle analysis with webpack-bundle-analyzer

### 2. Code Splitting and Lazy Loading

#### Dynamic Component Loading

```javascript
// Lazy-loaded components with fallbacks
export const LazyServiceDirectory = dynamic(
  () => import('@/components/content/ServiceDirectory'),
  {
    loading: () => <ComponentLoader name="Service Directory" />,
    ssr: false,
  }
);
```

**Implementation:**
- Heavy components loaded on-demand
- Intersection Observer for viewport-based loading
- Graceful fallbacks for loading states
- Error boundaries for failed loads

#### Route-Based Code Splitting

- Automatic code splitting by Next.js
- Dynamic imports for non-critical features
- Lazy loading below-the-fold content

### 3. Image Optimization

#### Next.js Image Component

```javascript
import Image from 'next/image';

<Image
  src="/images/hero.webp"
  alt="PPG Community Support"
  width={800}
  height={400}
  priority // For LCP images
  placeholder="blur"
  blurDataURL="data:image/jpeg;base64,..."
/>
```

**Features:**
- Automatic format optimization (WebP, AVIF)
- Responsive image sizing
- Lazy loading with intersection observer
- Blur placeholders to prevent CLS

#### Image Caching Strategy

```javascript
// Service Worker caching
if (isImageRequest(request)) {
  event.respondWith(cacheFirst(request, DYNAMIC_CACHE_NAME));
}
```

### 4. Font Optimization

#### Font Loading Strategy

```javascript
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
  preload: true,
  fallback: ['system-ui', 'arial'],
});
```

**Optimizations:**
- Font display swap to prevent FOIT
- Preloading critical fonts
- System font fallbacks
- Separate font cache in service worker

### 5. Caching Strategies

#### Service Worker Implementation

```javascript
// Cache-first for static assets
async function cacheFirst(request, cacheName) {
  const cachedResponse = await caches.match(request);
  if (cachedResponse) return cachedResponse;
  
  const networkResponse = await fetch(request);
  if (networkResponse.ok) {
    const cache = await caches.open(cacheName);
    cache.put(request, networkResponse.clone());
  }
  return networkResponse;
}
```

**Cache Types:**
- **Static Cache**: HTML pages, CSS, JS (1 year)
- **Dynamic Cache**: API responses, images (30 days)
- **Font Cache**: Web fonts (1 year)

#### HTTP Caching Headers

```javascript
// middleware.ts
if (request.nextUrl.pathname.startsWith('/_next/static/')) {
  headers.set('Cache-Control', 'public, max-age=31536000, immutable');
}
```

### 6. Core Web Vitals Optimization

#### Largest Contentful Paint (LCP)

**Optimizations:**
- Preload critical resources (fonts, hero images)
- Optimize server response times
- Remove render-blocking resources
- Use priority hints for critical images

```javascript
// Preload critical resources
<link rel="preload" href="/fonts/inter-var.woff2" as="font" type="font/woff2" crossorigin />
<link rel="preload" href="/images/ppg-logo.svg" as="image" />
```

#### First Input Delay (FID)

**Optimizations:**
- Break up long tasks with scheduler.postTask
- Use passive event listeners
- Defer non-critical JavaScript
- Optimize event handlers with debouncing

```javascript
// Break up long tasks
if ('scheduler' in window && 'postTask' in window.scheduler) {
  scheduler.postTask(() => {
    initializeNonCriticalFeatures();
  }, { priority: 'background' });
}
```

#### Cumulative Layout Shift (CLS)

**Optimizations:**
- Set explicit dimensions for images
- Reserve space for dynamic content
- Use transform/opacity for animations
- Optimize font loading

```javascript
// Reserve space for dynamic content
<div data-dynamic-content data-min-height="200px">
  {/* Dynamic content */}
</div>
```

### 7. Performance Monitoring

#### Web Vitals Reporting

```javascript
import { reportWebVitals } from '@/lib/performance';

// Report to analytics
onCLS(reportWebVitals);
onFID(reportWebVitals);
onLCP(reportWebVitals);
```

**Features:**
- Real-time Core Web Vitals monitoring
- Performance API integration
- Custom analytics endpoint
- Development vs production reporting

#### Lighthouse CI Integration

```bash
# Performance audit script
npm run audit:performance
```

**Automated Testing:**
- Lighthouse performance audits
- Core Web Vitals validation
- Accessibility compliance checks
- Performance regression detection

### 8. Network Optimization

#### Resource Hints

```html
<!-- Preconnect to external domains -->
<link rel="preconnect" href="https://fonts.googleapis.com" crossorigin>
<link rel="dns-prefetch" href="https://api.directus.io">
```

#### Compression

```javascript
// next.config.js
compress: true,

// Middleware
headers.set('Vary', 'Accept-Encoding');
```

### 9. JavaScript Optimization

#### Bundle Splitting

```javascript
// Dynamic imports for heavy libraries
const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <Spinner />,
});
```

#### Tree Shaking

```javascript
// Import only what's needed
import { debounce } from '@/lib/performance';
// Instead of: import * as utils from '@/lib/performance';
```

### 10. CSS Optimization

#### Critical CSS Inlining

```javascript
// Inline critical CSS in head
<style dangerouslySetInnerHTML={{
  __html: `
    /* Critical above-the-fold styles */
    header { background-color: #2E7D32; }
  `
}} />
```

#### CSS-in-JS Optimization

- Tailwind CSS with purging
- Critical CSS extraction
- Minimal runtime CSS

## Performance Monitoring

### Scripts Available

```bash
# Build with bundle analysis
npm run build:analyze

# Performance audit
npm run audit:performance

# Lighthouse performance test
npm run lighthouse:performance

# Full Lighthouse audit
npm run lighthouse:full
```

### Metrics Dashboard

The performance monitoring system tracks:

- Core Web Vitals metrics
- Resource loading times
- Bundle sizes
- Cache hit rates
- User experience metrics

### Performance Budget

| Metric | Target | Warning | Error |
|--------|--------|---------|-------|
| LCP | < 2.5s | 2.5-4s | > 4s |
| FID | < 100ms | 100-300ms | > 300ms |
| CLS | < 0.1 | 0.1-0.25 | > 0.25 |
| Bundle Size | < 250KB | 250-500KB | > 500KB |
| Images | < 1MB | 1-2MB | > 2MB |

## Best Practices

### Development

1. **Use performance profiling tools**
   - React DevTools Profiler
   - Chrome DevTools Performance tab
   - Lighthouse audits

2. **Optimize images before deployment**
   - Use appropriate formats (WebP, AVIF)
   - Compress images
   - Generate multiple sizes

3. **Monitor bundle size**
   - Regular bundle analysis
   - Remove unused dependencies
   - Use dynamic imports

### Deployment

1. **Enable compression**
   - Gzip/Brotli compression
   - CDN optimization
   - Cache headers

2. **Monitor performance**
   - Real User Monitoring (RUM)
   - Synthetic testing
   - Performance budgets

### Maintenance

1. **Regular audits**
   - Monthly Lighthouse audits
   - Performance regression testing
   - Dependency updates

2. **User feedback**
   - Performance user surveys
   - Analytics monitoring
   - Error tracking

## Troubleshooting

### Common Issues

1. **High LCP**
   - Check server response times
   - Optimize critical resources
   - Remove render-blocking resources

2. **High FID**
   - Break up long tasks
   - Optimize JavaScript execution
   - Use web workers for heavy computation

3. **High CLS**
   - Set image dimensions
   - Reserve space for ads/widgets
   - Avoid inserting content above existing content

### Debug Tools

```javascript
// Performance debugging
console.log('Performance entries:', performance.getEntries());

// Web Vitals debugging
import { getCLS, getFID, getLCP } from 'web-vitals';
getCLS(console.log);
getFID(console.log);
getLCP(console.log);
```

## Future Optimizations

### Planned Improvements

1. **Edge computing**
   - Cloudflare Workers
   - Edge-side includes
   - Geographic optimization

2. **Advanced caching**
   - Service worker updates
   - Background sync
   - Offline-first approach

3. **Performance AI**
   - Predictive prefetching
   - Adaptive loading
   - User behavior optimization

### Experimental Features

1. **React 18 features**
   - Concurrent rendering
   - Selective hydration
   - Streaming SSR

2. **Web platform APIs**
   - Navigation API
   - Scheduler API
   - Import maps

## Conclusion

The implemented performance optimizations provide a solid foundation for excellent user experience while maintaining accessibility and functionality. Regular monitoring and continuous improvement ensure the website remains performant as it grows and evolves.

For questions or suggestions about performance optimizations, please refer to the development team or create an issue in the project repository.