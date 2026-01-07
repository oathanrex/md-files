---

### Step 1: Building the "Widget Script" (Engine)
First, we'll create the script that the user will embed in their Blogger template.

**Prompt 1 (Copy into AI):**
> "I want to create a lightweight standalone JavaScript SDK (widget.js) for Blogger (Blogspot) sites.
>
> **Functionality:**
> 1.  It should run on `window.onload`.
> 2.  **Auto TOC:** Scan the page for `<h2>` and `<h3>` tags. If a `<div id='my-tool-toc'></div>` exists, inject a hierarchical Table of Contents there. Add smooth scroll.
> 3.  **Schema Injection:** Parse the page content. If specific classes like `.faq-block` exist, generate JSON-LD FAQ Schema and inject it into `<head>`. Also generate generic Article Schema based on the Page Title and First Image.
> 4.  **Link Formatter:** Scan all external links within `.post-body`. Add `target='_blank'` and `rel='nofollow noopener'`.
>
> Write clean, vanilla JavaScript code for this. No external dependencies like jQuery. Keep it minified and fast." *(AI will give you code. Save this file with the name `widget.js`.)*

, ### Step 2: The Visual Editor (The "SaaS" Frontend)
Now we have to create a website where the user will design the post. We will use **Next.js** and **Tiptap Editor**.

**Prompt 2 (Setup):**
> "Create a new Next.js project with Tailwind CSS. I want to build a 'Notion-style' WYSIWYG editor using the **Tiptap** library.
, > Set up the basic TipTap editor on the homepage. Allow standard formatting (Bold, Italic, H2, H3, Lists)."

**Prompt 3 (Custom Blocks - The Magic Part):**
> "Now, I need custom Tiptap extensions for my specific features. Please create:
, > 1. **Call-to-Action Block:** A styled box with a button.
> 2. **FAQ Block:** A repeating section where I can add Question and Answer pairs.
> 3. **Pros/Cons Block:** A 2-column layout (Green background for Pros, Red for Cons).
, >Ensure these blocks look good visually in the editor using Tailwind classes."

, ### Step 3: Export Logic (HTML Generation)
Now we have to convert the content of Editor into HTML which can be used in `widget.js`.

**Prompt 4:**
> "I need an 'Export' function. When the user clicks 'Generate Code', I need to get the HTML from Tiptap.
, > **Critical Requirement:**
> Instead of exporting complex inline styles, export 'Clean HTML' with specific class names.
> * For FAQs: Use `<div class='faq-block' data-question='...' data-answer='...'>`
> * For TOC: Just insert `<div id='my-tool-toc'></div>` at the top.
, > The styling will be handled by my external `widget.js`. Just give me the raw HTML structure."

---

### Step 4: Connecting the Dots (Using Existing Code)
We won't waste the code you already have on GitHub (Keyword density, Word count).

**Prompt 5 (Enter this prompt in Cursor and reference your old file):**
> "@old_script.js (tag the file)
> Check the logic for 'Keyword Density' and 'Reading Time' from my old script. Integrate this logic into my new Next.js sidebar. As the user types in the Tiptap editor, update these stats in real-time."

---

### Step 5: Final Polish & Deployment

**Prompt 6:**
> "Create a 'Copy to Clipboard' button. When clicked, it should copy two things:
> 1. The script tag: `<script src='https://cdn.yourdomain.com/widget.js'></script>` (Instructions: Add this once to the Layout).
> 2. The generated HTML content from the editor."

---

### How to Execute (Summary):

1. Install **Cursor**.
2. Run the **Prompts** given above in sequence.
3. If the AI ​​gets stuck, copy and paste the error and say *"Fix this error"*.
4. **Deployment:**
* Host `widget.js` on **Cloudflare R2** or **AWS S3** (Public access). 
* Host the Next.js app on **Vercel**.

This way, you will build a full-stack SaaS without deep coding knowledge that will be 10x better than your previous tool.
