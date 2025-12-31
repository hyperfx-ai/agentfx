---
name: nextjs-seo
description: This skill optimizes Next.js applications for search engines using JSON-LD structured data, metadata API, and layout best practices. This skill should be used when building new Next.js applications that need search visibility, optimizing existing sites for better SEO, implementing structured data for rich search results, or setting up proper metadata across pages and layouts.
category: "Development & Code Tools"
keywords: ["Next.js", "SEO", "JSON-LD", "structured data", "metadata", "schema.org", "search optimization"]
license: MIT
---

# Next.js SEO Optimization

Comprehensive guide to optimizing Next.js applications for search engines using JSON-LD structured data, the Metadata API, and SEO best practices.

## When to Use This Skill

This skill should be used when:
- Building a new Next.js application that requires search visibility
- Optimizing an existing Next.js site for improved SEO performance
- Implementing structured data for rich search results in Google
- Setting up proper metadata across pages and layout hierarchies
- Improving site crawlability and search engine indexing

## Core Principles

**Structured Data = Rich Results**
Search engines use JSON-LD to understand your content and display rich results (ratings, prices, events, etc.)

**Hierarchy Matters**
Next.js layouts create an SEO hierarchy: root layout → nested layouts → pages

**Static > Dynamic**
Generate metadata and structured data at build time when possible for better performance

## How to Use This Skill

### Step 1: Implement JSON-LD Structured Data

JSON-LD tells search engines what your page is about using [schema.org](https://schema.org) types.

**Basic Implementation Pattern:**

```tsx
// app/page.tsx
export default function Page() {
  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'WebSite',
    name: 'Your Site Name',
    url: 'https://yoursite.com',
  }

  return (
    <>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{
          __html: JSON.stringify(jsonLd).replace(/</g, '\\u003c'),
        }}
      />
      {/* Your page content */}
    </>
  )
}
```

**Important Security Note:**
Always use `.replace(/</g, '\\u003c')` to prevent XSS injection attacks when using `dangerouslySetInnerHTML` with JSON-LD.

Reference: [Next.js JSON-LD Guide](https://nextjs.org/docs/app/guides/json-ld)

### Step 2: Set Up Metadata API

Use Next.js Metadata API for title, description, Open Graph, and Twitter cards.

**Static Metadata (Best for most pages):**

```tsx
// app/layout.tsx or app/page.tsx
import { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Your Page Title',
  description: 'Your page description',
  openGraph: {
    title: 'Your Page Title',
    description: 'Your page description',
    url: 'https://yoursite.com',
    siteName: 'Your Site Name',
    images: [
      {
        url: 'https://yoursite.com/og-image.jpg',
        width: 1200,
        height: 630,
      },
    ],
    locale: 'en_US',
    type: 'website',
  },
  twitter: {
    card: 'summary_large_image',
    title: 'Your Page Title',
    description: 'Your page description',
    images: ['https://yoursite.com/og-image.jpg'],
  },
}
```

**Dynamic Metadata (For dynamic routes):**

```tsx
// app/products/[id]/page.tsx
export async function generateMetadata({ params }): Promise<Metadata> {
  const { id } = await params
  const product = await getProduct(id)
  
  return {
    title: product.name,
    description: product.description,
    openGraph: {
      images: [product.image],
    },
  }
}
```

### Step 3: Common Schema Types

#### WebSite (Home Page)

```tsx
const jsonLd = {
  '@context': 'https://schema.org',
  '@type': 'WebSite',
  name: 'Your Site Name',
  url: 'https://yoursite.com',
  potentialAction: {
    '@type': 'SearchAction',
    target: 'https://yoursite.com/search?q={search_term_string}',
    'query-input': 'required name=search_term_string',
  },
}
```

#### Organization (About Page)

```tsx
const jsonLd = {
  '@context': 'https://schema.org',
  '@type': 'Organization',
  name: 'Your Company',
  url: 'https://yoursite.com',
  logo: 'https://yoursite.com/logo.png',
  sameAs: [
    'https://twitter.com/yourcompany',
    'https://linkedin.com/company/yourcompany',
  ],
}
```

#### Product (E-commerce)

```tsx
const jsonLd = {
  '@context': 'https://schema.org',
  '@type': 'Product',
  name: product.name,
  image: product.image,
  description: product.description,
  sku: product.sku,
  offers: {
    '@type': 'Offer',
    price: product.price,
    priceCurrency: 'USD',
    availability: 'https://schema.org/InStock',
    url: `https://yoursite.com/products/${product.id}`,
  },
  aggregateRating: {
    '@type': 'AggregateRating',
    ratingValue: product.rating,
    reviewCount: product.reviewCount,
  },
}
```

#### Article (Blog Post)

```tsx
const jsonLd = {
  '@context': 'https://schema.org',
  '@type': 'Article',
  headline: article.title,
  image: article.image,
  author: {
    '@type': 'Person',
    name: article.author,
  },
  publisher: {
    '@type': 'Organization',
    name: 'Your Site Name',
    logo: {
      '@type': 'ImageObject',
      url: 'https://yoursite.com/logo.png',
    },
  },
  datePublished: article.publishedDate,
  dateModified: article.modifiedDate,
}
```

#### Event

```tsx
const jsonLd = {
  '@context': 'https://schema.org',
  '@type': 'Event',
  name: event.name,
  startDate: event.startDate,
  endDate: event.endDate,
  location: {
    '@type': 'Place',
    name: event.venueName,
    address: {
      '@type': 'PostalAddress',
      streetAddress: event.street,
      addressLocality: event.city,
      addressRegion: event.state,
      postalCode: event.zip,
      addressCountry: event.country,
    },
  },
  description: event.description,
  offers: {
    '@type': 'Offer',
    price: event.price,
    priceCurrency: 'USD',
    availability: 'https://schema.org/InStock',
    url: event.ticketUrl,
  },
}
```

### Step 4: Layout Hierarchy for SEO

**Root Layout (app/layout.tsx):**
- Site-wide metadata (title template, description, viewport)
- Global JSON-LD (Organization, WebSite)
- Language and locale settings

```tsx
export const metadata: Metadata = {
  metadataBase: new URL('https://yoursite.com'),
  title: {
    default: 'Your Site Name',
    template: '%s | Your Site Name',
  },
  description: 'Your site description',
  robots: {
    index: true,
    follow: true,
  },
}
```

**Nested Layouts:**
- Section-specific metadata
- Breadcrumb JSON-LD

**Individual Pages:**
- Page-specific metadata and JSON-LD
- Dynamic content structured data

## Example Usage

**Scenario:** E-commerce product page with full SEO

```tsx
// app/products/[id]/page.tsx
import { Metadata } from 'next'

export async function generateMetadata({ params }): Promise<Metadata> {
  const { id } = await params
  const product = await getProduct(id)
  
  return {
    title: product.name,
    description: product.description,
    openGraph: {
      title: product.name,
      description: product.description,
      images: [
        {
          url: product.image,
          width: 1200,
          height: 630,
          alt: product.name,
        },
      ],
      type: 'website',
    },
  }
}

export default async function ProductPage({ params }) {
  const { id } = await params
  const product = await getProduct(id)

  const jsonLd = {
    '@context': 'https://schema.org',
    '@type': 'Product',
    name: product.name,
    image: product.image,
    description: product.description,
    sku: product.sku,
    brand: {
      '@type': 'Brand',
      name: product.brand,
    },
    offers: {
      '@type': 'Offer',
      price: product.price,
      priceCurrency: 'USD',
      availability: 'https://schema.org/InStock',
      url: `https://yoursite.com/products/${id}`,
    },
    aggregateRating: {
      '@type': 'AggregateRating',
      ratingValue: product.rating,
      reviewCount: product.reviewCount,
    },
  }

  return (
    <div>
      <script
        type="application/ld+json"
        dangerouslySetInnerHTML={{
          __html: JSON.stringify(jsonLd).replace(/</g, '\\u003c'),
        }}
      />
      <h1>{product.name}</h1>
      <p>{product.description}</p>
      {/* Rest of product page */}
    </div>
  )
}
```

**Result:**
- Rich product results in Google Search (price, rating, availability)
- Proper Open Graph tags for social sharing
- Structured data for search engines to understand product details

## TypeScript Support

Use `schema-dts` for type-safe JSON-LD:

```bash
npm install schema-dts
```

```tsx
import { Product, WithContext } from 'schema-dts'

const jsonLd: WithContext<Product> = {
  '@context': 'https://schema.org',
  '@type': 'Product',
  name: 'Next.js Sticker',
  image: 'https://nextjs.org/imgs/sticker.png',
  description: 'Dynamic at the speed of static.',
}
```

## Validation & Testing

Always validate structured data using these tools:

1. **Google Rich Results Test**: [https://search.google.com/test/rich-results](https://search.google.com/test/rich-results)
2. **Schema Markup Validator**: [https://validator.schema.org/](https://validator.schema.org/)
3. **Google Search Console**: Monitor rich results performance

## SEO Checklist

- [ ] Root layout has proper metadata base and title template
- [ ] Each page has unique title and description
- [ ] Open Graph tags implemented for social sharing
- [ ] Twitter card meta tags added
- [ ] JSON-LD structured data on relevant pages
- [ ] JSON-LD uses XSS-safe stringify method
- [ ] Canonical URLs set correctly
- [ ] Robots meta tags configured appropriately
- [ ] Images have alt text
- [ ] Validated with Google Rich Results Test

## Common Schema Types Reference

| Page Type | Schema Type | Use Case |
|-----------|-------------|----------|
| Homepage | WebSite | Site identity + search action |
| About | Organization | Company info |
| Product | Product | E-commerce products |
| Blog Post | Article | Content pages |
| Event | Event | Conferences, webinars |
| FAQ | FAQPage | FAQ sections |
| Recipe | Recipe | Cooking sites |
| Review | Review | Product/service reviews |
| Video | VideoObject | Video content |
| Job Posting | JobPosting | Careers page |

## Best Practices

✅ **Do:**
- Generate metadata at build time when possible for better performance
- Use the Metadata API over manual meta tags for consistency
- Include structured data on every relevant page type
- Test structured data with Google's validation tools
- Use meaningful, unique descriptions for each page
- Implement breadcrumbs with JSON-LD for navigation clarity
- Add Open Graph images (1200×630px) for social sharing

❌ **Don't:**
- Put JSON-LD in `<head>` (place in body instead)
- Forget XSS protection when using JSON.stringify
- Duplicate metadata across layouts and pages unnecessarily
- Use generic descriptions that apply to multiple pages
- Skip validation of structured data before deployment
- Ignore mobile viewport settings

## Performance Considerations

- **Static Generation**: Use `generateStaticParams` for product pages
- **Edge Runtime**: Consider for dynamic metadata
- **Image Optimization**: Use Next.js Image component
- **Caching**: Cache API responses for metadata generation

## Reference Resources

Note: For detailed schema.org examples and advanced patterns, refer to:
- [Official Next.js JSON-LD documentation](https://nextjs.org/docs/app/guides/json-ld)
- [Schema.org type reference](https://schema.org/docs/schemas.html)
- [Google Search Central structured data guide](https://developers.google.com/search/docs/appearance/structured-data)

## Prerequisites

- Next.js 13+ with App Router
- Basic understanding of SEO concepts and terminology
- Access to Google Search Console for monitoring (recommended)

## Keywords

Next.js, SEO, JSON-LD, structured data, metadata, schema.org, search optimization, rich results, Open Graph, Twitter cards, web development

