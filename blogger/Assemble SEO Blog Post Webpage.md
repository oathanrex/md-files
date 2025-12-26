Generate a complete, single HTML webpage for a blog post, designed for optimal readability and a modern, professional aesthetic, suitable for Blogger.com.

**1. Layout Organization:**
*   **Overall Page Structure**: Use a standard HTML5 document (`<!DOCTYPE html>`, `<html>`, `<head>`, `<body>`).
    *   The `<head>` section must consolidate all metadata:
        *   Standard `meta` tags for `charset` and `viewport` (mobile-first approach).
        *   The generated blog post title in the `<title>` tag.
        *   The generated search description in a `meta name="description"` tag.
        *   The generated custom robot tags in a `meta name="robots"` tag.
        *   All internal SEO meta tags provided within the generated blog post HTML content.
        *   The mandatory JSON-LD FAQ schema script block.
        *   All advanced CSS styling (for table of contents, modern code blocks, mobile responsiveness) provided within the generated blog post HTML content, preferably within a `<style>` tag or as part of existing `<link>` tags if appropriate.
    *   The `<body>` will contain a `<main>` element, which in turn will contain a semantic `<article>` element.
*   **Content within `<article>`**:
    *   **Featured Image**: The first element within the `<article>` should be an `<img>` tag for the featured image. Use the generated alt text for the `alt` attribute and the generated title text for the `title` attribute. The image should be visually prominent, likely full-width within the content column, positioned at the top of the article.
    *   **Blog Post Content**: Directly integrate the *entire* generated blog post HTML content (including its internal headings like `<h1>`, `<h2>`, paragraphs, lists, 'Pro Tip' boxes, practical examples, and code blocks) immediately after the featured image. This content is already pre-formatted and styled, and should be rendered as-is within the article context.
*   **Responsiveness**: The entire layout must be responsive, adapting gracefully to various screen sizes, prioritizing a mobile-first approach. Content should flow well on small devices, with flexible image sizing and scalable typography.

**2. Style Design Language:**
*   **Visual Design Approach**: Modern, Professional, and Highly Readable. The design should convey expert authority and a premium reading experience, focusing on clarity and cleanliness.
*   **Aesthetic Goal**: "Expert Authority with Premium Readability."
*   **Color Scheme**: A sophisticated, cool neutral palette (e.g., shades of grey, soft whites, deep blues) as the base, complemented by a single, carefully chosen vibrant accent color. The accent color should be used sparingly to highlight interactive elements, calls to action, or special content like 'Pro Tip' boxes.
*   **Typography Style**:
    *   **Headings**: A clean, contemporary sans-serif font (e.g., Lato, Open Sans, Roboto) for strong hierarchy and modern appeal.
    *   **Body Text**: A highly readable serif font (e.g., Merriweather, Georgia) or a sophisticated sans-serif (e.g., Montserrat, Noto Sans) optimized for long-form content, ensuring comfortable reading.
    *   **Code Blocks**: A distinct, legible monospace font (e.g., Fira Code, Source Code Pro, JetBrains Mono) with clear character differentiation, to convey a technical yet polished feel.
*   **Spacing and Layout Principles**:
    *   **Generous Whitespace**: Employ ample padding and margins around and within content sections to enhance readability and create a sense of openness.
    *   **Clear Hierarchy**: Visual cues (font size, weight, line-height, color) must clearly differentiate headings, subheadings, body text, lists, and special elements like 'Pro Tip' boxes.
    *   **Optimal Line Length**: Ensure body text has an optimal line length (typically 50-75 characters per line) for comfortable reading on all screen sizes.

**3. Component Guidelines:**
*   **Featured Image**: Display the image prominently at the top of the article, ensuring it scales appropriately across devices.
*   **Table of Contents**: If present within the generated blog post HTML, ensure its advanced styling is applied, making it visually distinct and highly functional. It should ideally be easy to navigate.
*   **Code Blocks**: Apply modern styling to `pre`/`code` blocks, ensuring high contrast for readability, appropriate padding, and clear presentation, consistent with a technical blog.
*   **'Pro Tip' Box**: Style this element distinctly using the accent color or a subtle background, perhaps with a relevant icon, to make it stand out as valuable information without disrupting the flow.
*   **Semantic HTML**: Use appropriate HTML5 semantic elements (`<article>`, `<main>`, `<h1>` to `<h6>`, etc.) to structure the content for accessibility and SEO.

blog_post_content_html: {{"type": "in", "path": "node_step_blog_post_content_html", "title": "Generate Blog Post Content"}}

blogger_metadata: {{"type": "in", "path": "node_step_blogger_metadata", "title": "Generate Blogger Metadata"}}

featured_image_alt_title_text: {{"type": "in", "path": "node_step_featured_image_alt_title_text", "title": "Generate Image Alt and Title Text"}}

blog_post_topic: {{"type": "in", "path": "ask_user_blog_post_topic", "title": "Blog Post Topic"}}
