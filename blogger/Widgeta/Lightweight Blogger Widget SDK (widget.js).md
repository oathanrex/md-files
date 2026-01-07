# Lightweight Blogger Widget SDK (widget.js)

Here's a complete, clean, and optimized JavaScript SDK for Blogger sites:

## Full Source Code (widget.js)

```javascript
/**
 * Blogger Widget SDK v1.0.0
 * Lightweight, standalone JavaScript SDK for Blogger (Blogspot) sites
 * Features: Auto TOC, Schema Injection, Link Formatter
 * No external dependencies
 */
(function() {
    'use strict';

    const BloggerSDK = {
        config: {
            tocContainer: 'my-tool-toc',
            contentSelector: '.post-body',
            faqBlockClass: 'faq-block',
            faqQuestionClass: 'faq-question',
            faqAnswerClass: 'faq-answer',
            headingSelectors: ['h2', 'h3'],
            smoothScrollOffset: 20,
            smoothScrollDuration: 500
        },

        /**
         * Initialize the SDK
         */
        init: function() {
            this.generateTOC();
            this.injectSchemas();
            this.formatExternalLinks();
        },

        /**
         * AUTO TOC GENERATOR
         * Scans for h2/h3 tags and creates hierarchical Table of Contents
         */
        generateTOC: function() {
            const tocContainer = document.getElementById(this.config.tocContainer);
            if (!tocContainer) return;

            const content = document.querySelector(this.config.contentSelector);
            if (!content) return;

            const headings = content.querySelectorAll(
                this.config.headingSelectors.join(', ')
            );

            if (headings.length === 0) return;

            // Build TOC structure
            const tocData = this.buildTOCData(headings);
            const tocHTML = this.renderTOC(tocData);

            // Inject TOC
            tocContainer.innerHTML = tocHTML;
            tocContainer.classList.add('sdk-toc-wrapper');

            // Setup smooth scroll
            this.setupSmoothScroll(tocContainer);
        },

        /**
         * Build hierarchical TOC data structure
         */
        buildTOCData: function(headings) {
            const tocData = [];
            let currentH2 = null;
            let headingIndex = 0;

            headings.forEach(function(heading) {
                // Generate unique ID if not exists
                if (!heading.id) {
                    heading.id = 'sdk-heading-' + headingIndex++;
                }

                const item = {
                    id: heading.id,
                    text: heading.textContent.trim(),
                    level: heading.tagName.toLowerCase(),
                    children: []
                };

                if (item.level === 'h2') {
                    tocData.push(item);
                    currentH2 = item;
                } else if (item.level === 'h3' && currentH2) {
                    currentH2.children.push(item);
                } else if (item.level === 'h3') {
                    // H3 without parent H2
                    tocData.push(item);
                }
            });

            return tocData;
        },

        /**
         * Render TOC HTML
         */
        renderTOC: function(tocData) {
            let html = '<nav class="sdk-toc" aria-label="Table of Contents">';
            html += '<div class="sdk-toc-title">Table of Contents</div>';
            html += '<ul class="sdk-toc-list">';

            tocData.forEach(function(item) {
                html += '<li class="sdk-toc-item sdk-toc-' + item.level + '">';
                html += '<a href="#' + item.id + '" class="sdk-toc-link">' + 
                        this.escapeHTML(item.text) + '</a>';

                if (item.children && item.children.length > 0) {
                    html += '<ul class="sdk-toc-sublist">';
                    item.children.forEach(function(child) {
                        html += '<li class="sdk-toc-item sdk-toc-' + child.level + '">';
                        html += '<a href="#' + child.id + '" class="sdk-toc-link">' + 
                                this.escapeHTML(child.text) + '</a>';
                        html += '</li>';
                    }, this);
                    html += '</ul>';
                }

                html += '</li>';
            }, this);

            html += '</ul></nav>';
            return html;
        },

        /**
         * Setup smooth scroll for TOC links
         */
        setupSmoothScroll: function(container) {
            const self = this;
            const links = container.querySelectorAll('.sdk-toc-link');

            links.forEach(function(link) {
                link.addEventListener('click', function(e) {
                    e.preventDefault();
                    const targetId = this.getAttribute('href').substring(1);
                    const target = document.getElementById(targetId);

                    if (target) {
                        self.smoothScrollTo(target);
                        // Update URL hash without jumping
                        history.pushState(null, null, '#' + targetId);
                    }
                });
            });
        },

        /**
         * Smooth scroll animation
         */
        smoothScrollTo: function(element) {
            const targetPosition = element.getBoundingClientRect().top + 
                                   window.pageYOffset - this.config.smoothScrollOffset;
            const startPosition = window.pageYOffset;
            const distance = targetPosition - startPosition;
            const duration = this.config.smoothScrollDuration;
            let startTime = null;

            function animation(currentTime) {
                if (!startTime) startTime = currentTime;
                const elapsed = currentTime - startTime;
                const progress = Math.min(elapsed / duration, 1);
                
                // Easing function (easeInOutCubic)
                const ease = progress < 0.5
                    ? 4 * progress * progress * progress
                    : 1 - Math.pow(-2 * progress + 2, 3) / 2;

                window.scrollTo(0, startPosition + distance * ease);

                if (elapsed < duration) {
                    requestAnimationFrame(animation);
                }
            }

            requestAnimationFrame(animation);
        },

        /**
         * SCHEMA INJECTION
         * Generates and injects JSON-LD schemas
         */
        injectSchemas: function() {
            // Generate FAQ Schema if FAQ blocks exist
            this.injectFAQSchema();
            
            // Generate Article Schema
            this.injectArticleSchema();
        },

        /**
         * Generate and inject FAQ Schema
         */
        injectFAQSchema: function() {
            const faqBlocks = document.querySelectorAll('.' + this.config.faqBlockClass);
            if (faqBlocks.length === 0) return;

            const faqItems = [];

            faqBlocks.forEach(function(block) {
                const question = block.querySelector('.' + this.config.faqQuestionClass);
                const answer = block.querySelector('.' + this.config.faqAnswerClass);

                if (question && answer) {
                    faqItems.push({
                        "@type": "Question",
                        "name": question.textContent.trim(),
                        "acceptedAnswer": {
                            "@type": "Answer",
                            "text": answer.textContent.trim()
                        }
                    });
                }
            }, this);

            if (faqItems.length === 0) return;

            const faqSchema = {
                "@context": "https://schema.org",
                "@type": "FAQPage",
                "mainEntity": faqItems
            };

            this.injectSchema(faqSchema, 'sdk-faq-schema');
        },

        /**
         * Generate and inject Article Schema
         */
        injectArticleSchema: function() {
            const content = document.querySelector(this.config.contentSelector);
            if (!content) return;

            // Get page title
            const title = document.title || 
                          document.querySelector('h1')?.textContent?.trim() || 
                          'Untitled Article';

            // Get first image
            const firstImage = content.querySelector('img');
            const imageUrl = firstImage ? firstImage.src : '';

            // Get meta description or first paragraph
            const metaDesc = document.querySelector('meta[name="description"]');
            const firstParagraph = content.querySelector('p');
            const description = metaDesc?.content || 
                               firstParagraph?.textContent?.trim().substring(0, 160) || '';

            // Get publish date from Blogger's date element
            const dateElement = document.querySelector('.date-header') || 
                               document.querySelector('.published') ||
                               document.querySelector('time');
            const publishDate = dateElement?.getAttribute('datetime') || 
                               dateElement?.textContent?.trim() || 
                               new Date().toISOString();

            // Get author
            const authorElement = document.querySelector('.author') || 
                                 document.querySelector('[rel="author"]');
            const author = authorElement?.textContent?.trim() || 'Anonymous';

            const articleSchema = {
                "@context": "https://schema.org",
                "@type": "Article",
                "headline": title.substring(0, 110),
                "description": description,
                "author": {
                    "@type": "Person",
                    "name": author
                },
                "datePublished": this.parseDate(publishDate),
                "dateModified": this.parseDate(publishDate),
                "publisher": {
                    "@type": "Organization",
                    "name": window.location.hostname,
                    "url": window.location.origin
                },
                "mainEntityOfPage": {
                    "@type": "WebPage",
                    "@id": window.location.href
                }
            };

            // Add image if available
            if (imageUrl) {
                articleSchema.image = {
                    "@type": "ImageObject",
                    "url": imageUrl
                };
            }

            this.injectSchema(articleSchema, 'sdk-article-schema');
        },

        /**
         * Inject schema into document head
         */
        injectSchema: function(schema, id) {
            // Remove existing schema with same ID
            const existing = document.getElementById(id);
            if (existing) existing.remove();

            const script = document.createElement('script');
            script.type = 'application/ld+json';
            script.id = id;
            script.textContent = JSON.stringify(schema);
            document.head.appendChild(script);
        },

        /**
         * Parse date string to ISO format
         */
        parseDate: function(dateStr) {
            try {
                const date = new Date(dateStr);
                if (!isNaN(date.getTime())) {
                    return date.toISOString();
                }
            } catch (e) {}
            return new Date().toISOString();
        },

        /**
         * LINK FORMATTER
         * Adds target and rel attributes to external links
         */
        formatExternalLinks: function() {
            const content = document.querySelector(this.config.contentSelector);
            if (!content) return;

            const links = content.querySelectorAll('a[href]');
            const currentHost = window.location.hostname;

            links.forEach(function(link) {
                try {
                    const url = new URL(link.href, window.location.origin);
                    
                    // Check if external link
                    if (url.hostname && url.hostname !== currentHost) {
                        link.setAttribute('target', '_blank');
                        
                        // Build rel attribute
                        const relValues = new Set();
                        const existingRel = link.getAttribute('rel');
                        
                        if (existingRel) {
                            existingRel.split(/\s+/).forEach(function(val) {
                                relValues.add(val.toLowerCase());
                            });
                        }
                        
                        relValues.add('nofollow');
                        relValues.add('noopener');
                        
                        link.setAttribute('rel', Array.from(relValues).join(' '));
                    }
                } catch (e) {
                    // Invalid URL, skip
                }
            });
        },

        /**
         * Utility: Escape HTML to prevent XSS
         */
        escapeHTML: function(str) {
            const div = document.createElement('div');
            div.textContent = str;
            return div.innerHTML;
        }
    };

    /**
     * Initialize on window load
     */
    if (document.readyState === 'complete') {
        BloggerSDK.init();
    } else {
        window.addEventListener('load', function() {
            BloggerSDK.init();
        });
    }

    // Expose SDK globally for manual access if needed
    window.BloggerSDK = BloggerSDK;

})();
```

## Minified Version (widget.min.js)

```javascript
!function(){"use strict";const e={config:{tocContainer:"my-tool-toc",contentSelector:".post-body",faqBlockClass:"faq-block",faqQuestionClass:"faq-question",faqAnswerClass:"faq-answer",headingSelectors:["h2","h3"],smoothScrollOffset:20,smoothScrollDuration:500},init:function(){this.generateTOC(),this.injectSchemas(),this.formatExternalLinks()},generateTOC:function(){const e=document.getElementById(this.config.tocContainer);if(!e)return;const t=document.querySelector(this.config.contentSelector);if(!t)return;const n=t.querySelectorAll(this.config.headingSelectors.join(", "));if(0===n.length)return;const s=this.buildTOCData(n),o=this.renderTOC(s);e.innerHTML=o,e.classList.add("sdk-toc-wrapper"),this.setupSmoothScroll(e)},buildTOCData:function(e){const t=[];let n=null,s=0;return e.forEach((function(e){e.id||(e.id="sdk-heading-"+s++);const o={id:e.id,text:e.textContent.trim(),level:e.tagName.toLowerCase(),children:[]};"h2"===o.level?(t.push(o),n=o):"h3"===o.level&&n?n.children.push(o):"h3"===o.level&&t.push(o)})),t},renderTOC:function(e){let t='<nav class="sdk-toc" aria-label="Table of Contents">';return t+='<div class="sdk-toc-title">Table of Contents</div>',t+='<ul class="sdk-toc-list">',e.forEach((function(e){t+='<li class="sdk-toc-item sdk-toc-'+e.level+'">',t+='<a href="#'+e.id+'" class="sdk-toc-link">'+this.escapeHTML(e.text)+"</a>",e.children&&e.children.length>0&&(t+='<ul class="sdk-toc-sublist">',e.children.forEach((function(e){t+='<li class="sdk-toc-item sdk-toc-'+e.level+'">',t+='<a href="#'+e.id+'" class="sdk-toc-link">'+this.escapeHTML(e.text)+"</a>",t+="</li>"}),this),t+="</ul>"),t+="</li>"}),this),t+="</ul></nav>"},setupSmoothScroll:function(e){const t=this;e.querySelectorAll(".sdk-toc-link").forEach((function(e){e.addEventListener("click",(function(e){e.preventDefault();const n=this.getAttribute("href").substring(1),s=document.getElementById(n);s&&(t.smoothScrollTo(s),history.pushState(null,null,"#"+n))}))}))},smoothScrollTo:function(e){const t=e.getBoundingClientRect().top+window.pageYOffset-this.config.smoothScrollOffset,n=window.pageYOffset,s=t-n,o=this.config.smoothScrollDuration;let c=null;function i(e){c||(c=e);const a=e-c,r=Math.min(a/o,1),l=r<.5?4*r*r*r:1-Math.pow(-2*r+2,3)/2;window.scrollTo(0,n+s*l),a<o&&requestAnimationFrame(i)}requestAnimationFrame(i)},injectSchemas:function(){this.injectFAQSchema(),this.injectArticleSchema()},injectFAQSchema:function(){const e=document.querySelectorAll("."+this.config.faqBlockClass);if(0===e.length)return;const t=[];if(e.forEach((function(e){const n=e.querySelector("."+this.config.faqQuestionClass),s=e.querySelector("."+this.config.faqAnswerClass);n&&s&&t.push({"@type":"Question",name:n.textContent.trim(),acceptedAnswer:{"@type":"Answer",text:s.textContent.trim()}})}),this),0===t.length)return;const n={"@context":"https://schema.org","@type":"FAQPage",mainEntity:t};this.injectSchema(n,"sdk-faq-schema")},injectArticleSchema:function(){const e=document.querySelector(this.config.contentSelector);if(!e)return;const t=document.title||document.querySelector("h1")?.textContent?.trim()||"Untitled Article",n=e.querySelector("img"),s=n?n.src:"",o=document.querySelector('meta[name="description"]'),c=e.querySelector("p"),i=o?.content||c?.textContent?.trim().substring(0,160)||"",a=document.querySelector(".date-header")||document.querySelector(".published")||document.querySelector("time"),r=a?.getAttribute("datetime")||a?.textContent?.trim()||new Date().toISOString(),l=document.querySelector(".author")||document.querySelector('[rel="author"]'),u=l?.textContent?.trim()||"Anonymous",d={"@context":"https://schema.org","@type":"Article",headline:t.substring(0,110),description:i,author:{"@type":"Person",name:u},datePublished:this.parseDate(r),dateModified:this.parseDate(r),publisher:{"@type":"Organization",name:window.location.hostname,url:window.location.origin},mainEntityOfPage:{"@type":"WebPage","@id":window.location.href}};s&&(d.image={"@type":"ImageObject",url:s}),this.injectSchema(d,"sdk-article-schema")},injectSchema:function(e,t){const n=document.getElementById(t);n&&n.remove();const s=document.createElement("script");s.type="application/ld+json",s.id=t,s.textContent=JSON.stringify(e),document.head.appendChild(s)},parseDate:function(e){try{const t=new Date(e);if(!isNaN(t.getTime()))return t.toISOString()}catch(e){}return new Date().toISOString()},formatExternalLinks:function(){const e=document.querySelector(this.config.contentSelector);if(!e)return;const t=e.querySelectorAll("a[href]"),n=window.location.hostname;t.forEach((function(e){try{const t=new URL(e.href,window.location.origin);if(t.hostname&&t.hostname!==n){e.setAttribute("target","_blank");const s=new Set,o=e.getAttribute("rel");o&&o.split(/\s+/).forEach((function(e){s.add(e.toLowerCase())})),s.add("nofollow"),s.add("noopener"),e.setAttribute("rel",Array.from(s).join(" "))}}catch(e){}}))},escapeHTML:function(e){const t=document.createElement("div");return t.textContent=e,t.innerHTML}};"complete"===document.readyState?e.init():window.addEventListener("load",(function(){e.init()})),window.BloggerSDK=e}();
```

## Optional CSS Styles (widget.css)

```css
/* Blogger SDK - TOC Styles */
.sdk-toc-wrapper {
    margin: 20px 0;
    padding: 15px 20px;
    background: #f8f9fa;
    border: 1px solid #e9ecef;
    border-radius: 8px;
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
}

.sdk-toc {
    max-width: 100%;
}

.sdk-toc-title {
    font-size: 1.1em;
    font-weight: 600;
    color: #333;
    margin-bottom: 12px;
    padding-bottom: 8px;
    border-bottom: 2px solid #007bff;
}

.sdk-toc-list {
    list-style: none;
    margin: 0;
    padding: 0;
}

.sdk-toc-item {
    margin: 6px 0;
    line-height: 1.4;
}

.sdk-toc-h2 {
    font-weight: 500;
}

.sdk-toc-h3 {
    font-size: 0.95em;
}

.sdk-toc-link {
    color: #495057;
    text-decoration: none;
    transition: color 0.2s ease, padding-left 0.2s ease;
    display: inline-block;
}

.sdk-toc-link:hover {
    color: #007bff;
    padding-left: 4px;
}

.sdk-toc-sublist {
    list-style: none;
    margin: 4px 0 4px 20px;
    padding: 0;
    border-left: 2px solid #dee2e6;
    padding-left: 12px;
}

/* Dark mode support */
@media (prefers-color-scheme: dark) {
    .sdk-toc-wrapper {
        background: #1a1a2e;
        border-color: #2d2d44;
    }
    
    .sdk-toc-title {
        color: #e9ecef;
    }
    
    .sdk-toc-link {
        color: #adb5bd;
    }
    
    .sdk-toc-link:hover {
        color: #4dabf7;
    }
    
    .sdk-toc-sublist {
        border-color: #495057;
    }
}

/* Mobile responsive */
@media (max-width: 768px) {
    .sdk-toc-wrapper {
        padding: 12px 15px;
        margin: 15px 0;
    }
    
    .sdk-toc-title {
        font-size: 1em;
    }
    
    .sdk-toc-sublist {
        margin-left: 15px;
        padding-left: 10px;
    }
}
```

## Implementation in Blogger

### Step 1: Add TOC Container in Post Template
```html
<!-- Add this where you want the TOC to appear -->
<div id="my-tool-toc"></div>
```

### Step 2: FAQ Block HTML Structure
```html
<!-- FAQ blocks in your post content -->
<div class="faq-block">
    <div class="faq-question">What is this SDK?</div>
    <div class="faq-answer">A lightweight JavaScript SDK for Blogger sites.</div>
</div>

<div class="faq-block">
    <div class="faq-question">How do I install it?</div>
    <div class="faq-answer">Add the script before the closing body tag.</div>
</div>
```

### Step 3: Add Script to Blogger Theme
```html
<!-- Add before </body> in your Blogger theme -->
<script src="https://your-cdn.com/widget.min.js"></script>

<!-- Or inline the minified code -->
<script>
// Paste minified code here
</script>
```

### Step 4: Add Styles (Optional)
```html
<!-- Add in <head> section -->
<style>
/* Paste CSS here or link to external file */
</style>
```

## Configuration Options

You can customize the SDK before it initializes:

```html
<script>
// Custom configuration (add before loading widget.js)
window.BloggerSDKConfig = {
    tocContainer: 'custom-toc-id',
    contentSelector: '.custom-post-body',
    faqBlockClass: 'my-faq',
    faqQuestionClass: 'my-question',
    faqAnswerClass: 'my-answer',
    smoothScrollOffset: 50,
    smoothScrollDuration: 300
};
</script>
<script src="widget.min.js"></script>
```

## Features Summary

| Feature | Description |
|---------|-------------|
| **Auto TOC** | Hierarchical H2/H3 scanning, smooth scroll, URL hash updates |
| **FAQ Schema** | Automatic JSON-LD FAQPage schema generation |
| **Article Schema** | Auto-generated with title, image, author, dates |
| **Link Formatter** | External links get `target="_blank"` and `rel="nofollow noopener"` |
| **XSS Protection** | HTML escaping for all user content |
| **Performance** | No dependencies, ~3KB minified |
| **Accessibility** | ARIA labels, semantic HTML |
