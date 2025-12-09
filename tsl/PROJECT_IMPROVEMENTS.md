# Landing Page Optimization Playbook

**Purpose:** Reference guide for AI to implement standard optimizations on product landing pages.

**How to Use This File:**
1. Copy this file to your new project root directory
2. Update the [VARIABLES] section below with your project-specific values
3. Tell AI: "Please implement improvements from PROJECT_IMPROVEMENTS.md"
4. AI will automatically apply all optimizations using the variables you defined

---

## 📋 [VARIABLES] - UPDATE FOR EACH PROJECT

```yaml
PROJECT_NAME: Keto
COMPANY_NAME: PillowPotion Naturals
PRODUCT_FULL_NAME: PillowPotion Keto
CANONICAL_URL: https://keto.pillowpotion.com/
PRODUCT_IMAGE_URL: https://pillowpotion.com/offers/2x_-Keto-1-1024x1024.webp
LOGO_URL: https://pillowpotion.com/offers/pillowpotionlogo.webp
CONTACT_URL: https://bloodboost.pillowpotion.com/dtc/v1/contact.html

# Brand Colors (from JavaScript colors object)
BRAND_COLOR_DARK: #4A2E73
BRAND_COLOR_MID: #7B55B7
BRAND_COLOR_LIGHT: #A187D0
BRAND_COLOR_BG: #f2ecfa

# Hero Section Gradients
HERO_GRADIENT_START: #e8e0fa
HERO_GRADIENT_MID: #d7c6f9
HERO_GRADIENT_END: #bca8f7

# SEO Content
PRODUCT_DESCRIPTION: Natural ketosis support supplement with MCT Oil, Ginger Root Extract, Apple Cider Vinegar, Garcinia Cambogia, and Green Tea Extract
KEYWORDS: keto, ketosis, weight loss, fat burning, keto supplement, BHB, ketogenic diet, natural supplement, energy booster, metabolism

# Product Offers
OFFER_1_NAME: 2 Bottles - 60 Day Supply
OFFER_1_PRICE: 130
OFFER_1_PRODUCT_CODE: ket2

OFFER_2_NAME: 6 Bottles - 180 Day Supply
OFFER_2_PRICE: 270
OFFER_2_PRODUCT_CODE: ket6

OFFER_3_NAME: 4 Bottles - 120 Day Supply
OFFER_3_PRICE: 220
OFFER_3_PRODUCT_CODE: ket4

CHECKOUT_BASE_URL: https://buygoods.com/secure/checkout.html?account_id=11694&product_codename=
```

---

## 🎨 TASK 1: Fix Color Flash/FOUC

### Problem
JavaScript applies colors after page load, causing visible flash from CSS default colors to brand colors.

### AI Instructions

1. **Find JavaScript colors** (in `<head>` `<script>` tag)
   - Look for: `const colors = { productDarkColour: ..., navbar: ..., heroStart: ... }`
   - Note all color values

2. **Update inline CSS variables** (in `<head>` `<style>` tag)
   - Find: `* { --dk-blue: ...; --md-blue: ...; }`
   - Replace with values from VARIABLES above

3. **Add critical CSS** (before `@media` queries in inline `<style>`)
   - Insert the code block below
   - Use values from VARIABLES section

4. **Update external CSS** (`css/styles.css`)
   - Update same CSS variables as inline styles

### Code Template

**Insert in `<head>` `<style>` tag (after CSS variables, before @media):**

```css
/* Prevent FOUC - Apply colors immediately */
.top-nav-wrapper,
.top-nav {
    background-color: {{BRAND_COLOR_BG}} !important;
}

.main-hero-container {
    background: linear-gradient(135deg, {{HERO_GRADIENT_START}} 0%, {{HERO_GRADIENT_MID}} 35%, {{HERO_GRADIENT_END}} 100%) !important;
}

.mob-space {
    background-color: {{BRAND_COLOR_DARK}} !important;
}

.product-card-left .product-header,
.product-card-right .product-header {
    background: {{BRAND_COLOR_DARK}} !important;
}

.product-card:not(.product-card-left):not(.product-card-right) .product-header {
    background: linear-gradient(90deg, {{BRAND_COLOR_DARK}} 0%, {{BRAND_COLOR_LIGHT}} 100%) !important;
}

.best-badge {
    background-color: {{BRAND_COLOR_DARK}} !important;
}

.product-price h4,
.product-bonus {
    color: {{BRAND_COLOR_DARK}} !important;
}

.headline,
.headline span {
    color: {{BRAND_COLOR_DARK}} !important;
}

.ingredient-label {
    background-color: {{BRAND_COLOR_MID}} !important;
}

.discount-banner {
    background-color: {{BRAND_COLOR_DARK}} !important;
}

.guarantee-box,
.bioheal-faq-container,
.bioheal-ref-bg,
.bioheal-footer {
    background: linear-gradient(135deg, {{BRAND_COLOR_DARK}} 0%, {{BRAND_COLOR_LIGHT}} 100%) !important;
}

.bioheal-faq-content {
    background: {{BRAND_COLOR_DARK}} !important;
}

.div1 {
    background: {{BRAND_COLOR_DARK}} !important;
}
```

**Update CSS variables in both files:**

```css
* {
    --dk-blue: {{BRAND_COLOR_DARK}};
    --md-blue: {{BRAND_COLOR_MID}};
    --lt-blue: {{BRAND_COLOR_LIGHT}};
    --super-lt-blue: {{BRAND_COLOR_BG}};
}
```

### Success Criteria
✅ No color flash when page loads
✅ Brand colors visible immediately
✅ Test in incognito mode

---

## 🔍 TASK 2: SEO Optimization

### Problem
Missing critical SEO elements for search visibility and social sharing.

### AI Instructions

1. **Locate `<head>` section** in `index.html`

2. **Remove duplicates:**
   - Delete duplicate `<meta charset>` tags (keep only one)
   - Delete duplicate `<meta viewport>` tags (keep only one)

3. **Add/Replace meta tags** at top of `<head>` (use template below)

4. **Add JSON-LD schemas** before tracking scripts (use templates below)

5. **Extract FAQs from page** and add to FAQ Schema

### Code Templates

**Meta Tags (replace/add at top of `<head>`):**

```html
<!-- Basic Meta -->
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0">

<!-- SEO Meta -->
<title>{{PROJECT_NAME}} | {{COMPANY_NAME}} - Natural Supplement</title>
<meta name="description" content="{{PRODUCT_DESCRIPTION}}. Start your journey today!">
<meta name="keywords" content="{{KEYWORDS}}">
<meta name="author" content="{{COMPANY_NAME}}">
<meta name="robots" content="index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1">
<link rel="canonical" href="{{CANONICAL_URL}}">
<meta name="theme-color" content="{{BRAND_COLOR_DARK}}">

<!-- Open Graph (Social Media) -->
<meta property="og:type" content="website">
<meta property="og:title" content="{{PROJECT_NAME}} | {{COMPANY_NAME}}">
<meta property="og:description" content="{{PRODUCT_DESCRIPTION}}. 60-Day Money-Back Guarantee!">
<meta property="og:url" content="{{CANONICAL_URL}}">
<meta property="og:image" content="{{PRODUCT_IMAGE_URL}}">
<meta property="og:image:width" content="1024">
<meta property="og:image:height" content="1024">
<meta property="og:site_name" content="{{PRODUCT_FULL_NAME}}">
<meta property="og:locale" content="en_US">

<!-- Twitter Card -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{{PROJECT_NAME}} | {{COMPANY_NAME}}">
<meta name="twitter:description" content="{{PRODUCT_DESCRIPTION}}. 60-Day Guarantee!">
<meta name="twitter:image" content="{{PRODUCT_IMAGE_URL}}">
```

**Structured Data (add before tracking scripts):**

```html
<!-- Product Schema -->
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Product",
    "name": "{{PRODUCT_FULL_NAME}}",
    "description": "{{PRODUCT_DESCRIPTION}}",
    "image": "{{PRODUCT_IMAGE_URL}}",
    "brand": {
        "@type": "Brand",
        "name": "{{COMPANY_NAME}}"
    },
    "offers": [
        {
            "@type": "Offer",
            "name": "{{OFFER_1_NAME}}",
            "price": "{{OFFER_1_PRICE}}",
            "priceCurrency": "USD",
            "availability": "https://schema.org/InStock",
            "url": "{{CHECKOUT_BASE_URL}}{{OFFER_1_PRODUCT_CODE}}",
            "priceValidUntil": "2025-12-31"
        },
        {
            "@type": "Offer",
            "name": "{{OFFER_2_NAME}}",
            "price": "{{OFFER_2_PRICE}}",
            "priceCurrency": "USD",
            "availability": "https://schema.org/InStock",
            "url": "{{CHECKOUT_BASE_URL}}{{OFFER_2_PRODUCT_CODE}}",
            "priceValidUntil": "2025-12-31"
        },
        {
            "@type": "Offer",
            "name": "{{OFFER_3_NAME}}",
            "price": "{{OFFER_3_PRICE}}",
            "priceCurrency": "USD",
            "availability": "https://schema.org/InStock",
            "url": "{{CHECKOUT_BASE_URL}}{{OFFER_3_PRODUCT_CODE}}",
            "priceValidUntil": "2025-12-31"
        }
    ],
    "aggregateRating": {
        "@type": "AggregateRating",
        "ratingValue": "4.8",
        "reviewCount": "5",
        "bestRating": "5",
        "worstRating": "1"
    }
}
</script>

<!-- Organization Schema -->
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Organization",
    "name": "{{COMPANY_NAME}}",
    "url": "{{CANONICAL_URL}}",
    "logo": "{{LOGO_URL}}",
    "contactPoint": {
        "@type": "ContactPoint",
        "contactType": "Customer Service",
        "url": "{{CONTACT_URL}}"
    }
}
</script>

<!-- WebPage Schema -->
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "WebPage",
    "name": "{{PROJECT_NAME}} | {{COMPANY_NAME}}",
    "description": "{{PRODUCT_DESCRIPTION}}",
    "url": "{{CANONICAL_URL}}"
}
</script>

<!-- FAQ Schema: AI - Extract all FAQs from page and format as schema -->
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [
        {
            "@type": "Question",
            "name": "[EXTRACT FROM PAGE]",
            "acceptedAnswer": {
                "@type": "Answer",
                "text": "[EXTRACT FROM PAGE]"
            }
        }
    ]
}
</script>
```

### Success Criteria
✅ All meta tags present (no duplicates)
✅ All JSON-LD schemas added
✅ FAQs extracted and added to schema
✅ Test with Google Rich Results Test
✅ Test with Schema Validator

---

## 📝 Implementation Checklist for AI

When user says "implement improvements from PROJECT_IMPROVEMENTS.md", follow these steps:

### Phase 1: Preparation
- [ ] Read PROJECT_IMPROVEMENTS.md
- [ ] Extract all variables from [VARIABLES] section
- [ ] Locate index.html file
- [ ] Locate css/styles.css file

### Phase 2: Color Fix (TASK 1)
- [ ] Update CSS variables in inline `<style>` tag (in `<head>`)
- [ ] Add critical CSS before @media queries
- [ ] Update CSS variables in external css/styles.css
- [ ] Verify no syntax errors

### Phase 3: SEO (TASK 2)
- [ ] Remove duplicate meta tags
- [ ] Add/replace all meta tags at top of `<head>`
- [ ] Add Product Schema before tracking scripts
- [ ] Add Organization Schema
- [ ] Add WebPage Schema
- [ ] Find FAQ section in HTML
- [ ] Extract all FAQ questions and answers
- [ ] Add FAQ Schema with extracted content
- [ ] Verify all schemas have correct syntax

### Phase 4: Validation
- [ ] Check for any {{VARIABLE}} placeholders not replaced
- [ ] Verify closing tags match
- [ ] Confirm no duplicate IDs or elements
- [ ] Report completion to user

---

## 🎯 Benefits

### Color Fix
- Eliminates color flash on page load
- Professional, polished user experience
- Faster perceived load time

### SEO Optimization
- **Better Rankings** - Rich meta data
- **Rich Snippets** - Star ratings, prices, FAQs in search
- **Social Sharing** - Beautiful preview cards
- **Voice Search** - FAQ schema optimization
- **Mobile UX** - Theme color integration

---

## 🧪 Testing Tools

After implementation, test with:
- [Google Rich Results Test](https://search.google.com/test/rich-results)
- [Schema Markup Validator](https://validator.schema.org/)
- [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/)
- [Twitter Card Validator](https://cards-dev.twitter.com/validator)

---

## 💡 Usage Example

```
User: Please implement improvements from PROJECT_IMPROVEMENTS.md

AI: I'll apply all optimizations from the playbook:
1. Fixing color flash (TASK 1)
2. Adding SEO elements (TASK 2)

[AI reads variables, applies all changes]

Done! Applied:
✅ Color flash fix (inline CSS + external CSS updated)
✅ Enhanced meta tags (title, description, keywords, OG, Twitter)
✅ Product Schema with 3 pricing offers
✅ Organization Schema
✅ WebPage Schema
✅ FAQ Schema with 7 questions extracted from page

Test your SEO with the tools listed in the playbook.
```

---

*Playbook Version: 1.0*
*Last Updated: 2025-01-29*
