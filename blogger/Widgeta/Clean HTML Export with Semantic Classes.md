# Clean HTML Export with Semantic Classes

Its create a comprehensive export system that generates clean, semantic HTML with specific class names for your widget.js to style.

## Project Structure Update

```
src/
├── components/
│   └── editor/
│       ├── export/
│       │   ├── ExportManager.ts
│       │   ├── HTMLSerializer.ts
│       │   ├── ExportModal.tsx
│       │   └── index.ts
│       ├── Editor.tsx (updated)
│       └── ...
└── ...
```

---

## 1. HTML Serializer - Core Export Logic

### `src/components/editor/export/HTMLSerializer.ts`

```typescript
/**
 * Clean HTML Serializer for Tiptap Editor
 * Generates semantic HTML with specific class names for external widget.js
 */

import { JSONContent } from '@tiptap/react';

export interface SerializerOptions {
  includeTOC: boolean;
  includeSchemaMarkup: boolean;
  minify: boolean;
  tocPosition: 'top' | 'afterFirstHeading';
}

export interface ExportResult {
  html: string;
  faqSchema: object | null;
  articleSchema: object | null;
  stats: {
    headings: number;
    faqs: number;
    ctas: number;
    prosConsBlocks: number;
    wordCount: number;
  };
}

const defaultOptions: SerializerOptions = {
  includeTOC: true,
  includeSchemaMarkup: false,
  minify: false,
  tocPosition: 'top',
};

/**
 * Escape HTML special characters
 */
function escapeHtml(text: string): string {
  const map: Record<string, string> = {
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&#039;',
  };
  return text.replace(/[&<>"']/g, (char) => map[char]);
}

/**
 * Generate unique ID from text
 */
function generateId(text: string, index: number): string {
  const slug = text
    .toLowerCase()
    .replace(/[^a-z0-9]+/g, '-')
    .replace(/(^-|-$)/g, '');
  return `${slug}-${index}` || `heading-${index}`;
}

/**
 * Serialize inline marks (bold, italic, etc.)
 */
function serializeMarks(node: JSONContent): string {
  let text = node.text || '';
  
  if (!node.marks || node.marks.length === 0) {
    return escapeHtml(text);
  }

  // Apply marks in order
  node.marks.forEach((mark) => {
    switch (mark.type) {
      case 'bold':
        text = `<strong>${text}</strong>`;
        break;
      case 'italic':
        text = `<em>${text}</em>`;
        break;
      case 'strike':
        text = `<s>${text}</s>`;
        break;
      case 'code':
        text = `<code class="inline-code">${escapeHtml(node.text || '')}</code>`;
        return; // Already escaped
      case 'link':
        const href = mark.attrs?.href || '#';
        const target = mark.attrs?.target || '_blank';
        text = `<a href="${escapeHtml(href)}" target="${target}" rel="noopener noreferrer">${text}</a>`;
        break;
    }
  });

  return text;
}

/**
 * Serialize node content (children)
 */
function serializeContent(content: JSONContent[] | undefined, context: SerializerContext): string {
  if (!content) return '';
  return content.map((node) => serializeNode(node, context)).join('');
}

interface SerializerContext {
  headingIndex: number;
  headings: Array<{ id: string; text: string; level: number }>;
  faqs: Array<{ question: string; answer: string }>;
  ctas: number;
  prosConsBlocks: number;
  wordCount: number;
}

/**
 * Serialize a single node to clean HTML
 */
function serializeNode(node: JSONContent, context: SerializerContext): string {
  switch (node.type) {
    case 'doc':
      return serializeContent(node.content, context);

    case 'text':
      const text = node.text || '';
      context.wordCount += text.split(/\s+/).filter(Boolean).length;
      return serializeMarks(node);

    case 'paragraph':
      const pContent = serializeContent(node.content, context);
      if (!pContent.trim()) return '';
      return `<p>${pContent}</p>\n`;

    case 'heading':
      const level = node.attrs?.level || 2;
      const headingText = extractText(node);
      const headingId = generateId(headingText, context.headingIndex++);
      
      context.headings.push({
        id: headingId,
        text: headingText,
        level,
      });

      return `<h${level} id="${headingId}" class="heading-${level}">${serializeContent(node.content, context)}</h${level}>\n`;

    case 'bulletList':
      return `<ul class="bullet-list">\n${serializeContent(node.content, context)}</ul>\n`;

    case 'orderedList':
      return `<ol class="ordered-list">\n${serializeContent(node.content, context)}</ol>\n`;

    case 'listItem':
      return `  <li>${serializeContent(node.content, context).replace(/<\/?p>/g, '')}</li>\n`;

    case 'blockquote':
      return `<blockquote class="quote-block">\n${serializeContent(node.content, context)}</blockquote>\n`;

    case 'codeBlock':
      const language = node.attrs?.language || '';
      const codeContent = extractText(node);
      return `<pre class="code-block" data-language="${escapeHtml(language)}"><code>${escapeHtml(codeContent)}</code></pre>\n`;

    case 'horizontalRule':
      return `<hr class="divider" />\n`;

    case 'hardBreak':
      return `<br />\n`;

    // Custom Blocks
    case 'callToAction':
      context.ctas++;
      return serializeCallToAction(node.attrs);

    case 'faqBlock':
      return serializeFaqBlock(node.attrs, context);

    case 'prosConsBlock':
      context.prosConsBlocks++;
      return serializeProsConsBlock(node.attrs);

    default:
      // Handle unknown nodes by serializing their content
      if (node.content) {
        return serializeContent(node.content, context);
      }
      return '';
  }
}

/**
 * Extract plain text from a node
 */
function extractText(node: JSONContent): string {
  if (node.type === 'text') {
    return node.text || '';
  }
  if (node.content) {
    return node.content.map(extractText).join('');
  }
  return '';
}

/**
 * Serialize Call to Action block
 */
function serializeCallToAction(attrs: Record<string, any> | undefined): string {
  const {
    title = 'Get Started',
    description = '',
    buttonText = 'Click Here',
    buttonUrl = '#',
    variant = 'primary',
  } = attrs || {};

  return `
<div class="cta-block" data-variant="${escapeHtml(variant)}">
  <div class="cta-content">
    <h3 class="cta-title">${escapeHtml(title)}</h3>
    <p class="cta-description">${escapeHtml(description)}</p>
  </div>
  <a href="${escapeHtml(buttonUrl)}" class="cta-button" target="_blank" rel="noopener noreferrer">
    ${escapeHtml(buttonText)}
  </a>
</div>
`;
}

/**
 * Serialize FAQ block with proper structure for widget.js
 */
function serializeFaqBlock(attrs: Record<string, any> | undefined, context: SerializerContext): string {
  const {
    title = 'Frequently Asked Questions',
    items = [],
  } = attrs || {};

  const faqItems = items as Array<{ id: string; question: string; answer: string }>;
  
  // Add to context for schema generation
  faqItems.forEach((item) => {
    context.faqs.push({
      question: item.question,
      answer: item.answer,
    });
  });

  const faqItemsHtml = faqItems
    .map(
      (item, index) => `
  <div class="faq-item" data-faq-index="${index}">
    <div class="faq-question" data-question="${escapeHtml(item.question)}">
      <span class="faq-question-text">${escapeHtml(item.question)}</span>
      <span class="faq-toggle-icon"></span>
    </div>
    <div class="faq-answer" data-answer="${escapeHtml(item.answer)}">
      <p>${escapeHtml(item.answer)}</p>
    </div>
  </div>`
    )
    .join('\n');

  return `
<div class="faq-block" data-faq-count="${faqItems.length}">
  <h3 class="faq-title">${escapeHtml(title)}</h3>
  <div class="faq-list">
${faqItemsHtml}
  </div>
</div>
`;
}

/**
 * Serialize Pros/Cons block
 */
function serializeProsConsBlock(attrs: Record<string, any> | undefined): string {
  const {
    title = 'Pros & Cons',
    pros = [],
    cons = [],
  } = attrs || {};

  const prosHtml = (pros as string[])
    .map((pro) => `      <li class="pros-item">${escapeHtml(pro)}</li>`)
    .join('\n');

  const consHtml = (cons as string[])
    .map((con) => `      <li class="cons-item">${escapeHtml(con)}</li>`)
    .join('\n');

  return `
<div class="pros-cons-block" data-pros-count="${pros.length}" data-cons-count="${cons.length}">
  <h3 class="pros-cons-title">${escapeHtml(title)}</h3>
  <div class="pros-cons-grid">
    <div class="pros-column">
      <h4 class="pros-heading">
        <span class="pros-icon"></span>
        Pros
      </h4>
      <ul class="pros-list">
${prosHtml}
      </ul>
    </div>
    <div class="cons-column">
      <h4 class="cons-heading">
        <span class="cons-icon"></span>
        Cons
      </h4>
      <ul class="cons-list">
${consHtml}
      </ul>
    </div>
  </div>
</div>
`;
}

/**
 * Generate TOC HTML placeholder
 */
function generateTOCPlaceholder(): string {
  return `<!-- Table of Contents - Auto-generated by widget.js -->
<div id="my-tool-toc" class="toc-container"></div>
`;
}

/**
 * Generate FAQ Schema JSON-LD
 */
function generateFaqSchema(faqs: Array<{ question: string; answer: string }>): object | null {
  if (faqs.length === 0) return null;

  return {
    '@context': 'https://schema.org',
    '@type': 'FAQPage',
    mainEntity: faqs.map((faq) => ({
      '@type': 'Question',
      name: faq.question,
      acceptedAnswer: {
        '@type': 'Answer',
        text: faq.answer,
      },
    })),
  };
}

/**
 * Minify HTML output
 */
function minifyHtml(html: string): string {
  return html
    .replace(/\n\s*\n/g, '\n') // Remove empty lines
    .replace(/>\s+</g, '><') // Remove whitespace between tags
    .replace(/\s{2,}/g, ' ') // Collapse multiple spaces
    .trim();
}

/**
 * Main export function - serialize JSON content to clean HTML
 */
export function serializeToCleanHTML(
  content: JSONContent,
  options: Partial<SerializerOptions> = {}
): ExportResult {
  const opts = { ...defaultOptions, ...options };
  
  const context: SerializerContext = {
    headingIndex: 0,
    headings: [],
    faqs: [],
    ctas: 0,
    prosConsBlocks: 0,
    wordCount: 0,
  };

  // Serialize the content
  let html = serializeNode(content, context);

  // Add TOC placeholder
  if (opts.includeTOC) {
    const tocHtml = generateTOCPlaceholder();
    
    if (opts.tocPosition === 'top') {
      html = tocHtml + '\n' + html;
    } else if (opts.tocPosition === 'afterFirstHeading') {
      // Insert after first heading
      const firstHeadingMatch = html.match(/<\/h[1-6]>/);
      if (firstHeadingMatch && firstHeadingMatch.index !== undefined) {
        const insertPosition = firstHeadingMatch.index + firstHeadingMatch[0].length;
        html = html.slice(0, insertPosition) + '\n' + tocHtml + html.slice(insertPosition);
      } else {
        html = tocHtml + '\n' + html;
      }
    }
  }

  // Wrap in article container
  html = `<article class="post-body">\n${html}</article>`;

  // Minify if requested
  if (opts.minify) {
    html = minifyHtml(html);
  }

  // Generate schemas
  const faqSchema = generateFaqSchema(context.faqs);

  return {
    html,
    faqSchema,
    articleSchema: null, // Will be generated by widget.js based on page context
    stats: {
      headings: context.headings.length,
      faqs: context.faqs.length,
      ctas: context.ctas,
      prosConsBlocks: context.prosConsBlocks,
      wordCount: context.wordCount,
    },
  };
}

/**
 * Generate standalone schema script tag
 */
export function generateSchemaScript(schema: object): string {
  return `<script type="application/ld+json">
${JSON.stringify(schema, null, 2)}
</script>`;
}

export default serializeToCleanHTML;
```

---

## 2. Export Manager

### `src/components/editor/export/ExportManager.ts`

```typescript
/**
 * Export Manager - Handles various export formats and operations
 */

import { Editor } from '@tiptap/react';
import { serializeToCleanHTML, SerializerOptions, ExportResult, generateSchemaScript } from './HTMLSerializer';

export interface ExportOptions extends Partial<SerializerOptions> {
  format: 'html' | 'html-with-schema' | 'json' | 'markdown';
  filename?: string;
}

export interface FullExport {
  html: string;
  schemaScripts: string;
  combinedOutput: string;
  json: object;
  stats: ExportResult['stats'];
}

/**
 * Export content from Tiptap editor
 */
export function exportContent(editor: Editor, options: ExportOptions): FullExport {
  const jsonContent = editor.getJSON();
  
  const result = serializeToCleanHTML(jsonContent, {
    includeTOC: options.includeTOC ?? true,
    includeSchemaMarkup: options.includeSchemaMarkup ?? false,
    minify: options.minify ?? false,
    tocPosition: options.tocPosition ?? 'top',
  });

  // Generate schema scripts
  let schemaScripts = '';
  if (result.faqSchema) {
    schemaScripts += generateSchemaScript(result.faqSchema) + '\n';
  }

  // Combined output with schemas
  const combinedOutput = result.html + (schemaScripts ? '\n\n' + schemaScripts : '');

  return {
    html: result.html,
    schemaScripts,
    combinedOutput,
    json: jsonContent,
    stats: result.stats,
  };
}

/**
 * Copy text to clipboard
 */
export async function copyToClipboard(text: string): Promise<boolean> {
  try {
    await navigator.clipboard.writeText(text);
    return true;
  } catch (err) {
    // Fallback for older browsers
    try {
      const textArea = document.createElement('textarea');
      textArea.value = text;
      textArea.style.position = 'fixed';
      textArea.style.left = '-9999px';
      document.body.appendChild(textArea);
      textArea.select();
      document.execCommand('copy');
      document.body.removeChild(textArea);
      return true;
    } catch (e) {
      console.error('Failed to copy to clipboard:', e);
      return false;
    }
  }
}

/**
 * Download content as file
 */
export function downloadAsFile(content: string, filename: string, mimeType: string): void {
  const blob = new Blob([content], { type: mimeType });
  const url = URL.createObjectURL(blob);
  const link = document.createElement('a');
  link.href = url;
  link.download = filename;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  URL.revokeObjectURL(url);
}

/**
 * Generate Blogger-ready HTML with all required elements
 */
export function generateBloggerHTML(exportResult: FullExport): string {
  const header = `<!-- 
  Generated by Notion-Style Editor
  Include widget.js for interactive features
  Stats: ${exportResult.stats.headings} headings, ${exportResult.stats.faqs} FAQs, ${exportResult.stats.wordCount} words
-->

`;

  const widgetNote = `
<!-- Add this before </body> in your Blogger template -->
<!-- <script src="https://your-cdn.com/widget.js"></script> -->
`;

  return header + exportResult.html + widgetNote;
}

export default exportContent;
```

---

## 3. Export Modal Component

### `src/components/editor/export/ExportModal.tsx`

```tsx
'use client';

import React, { useState, useEffect } from 'react';
import { Editor } from '@tiptap/react';
import {
  X,
  Copy,
  Download,
  Check,
  Code,
  FileJson,
  FileText,
  Settings,
  ChevronDown,
  ChevronUp,
  Zap,
  AlertCircle,
  Sparkles,
} from 'lucide-react';
import { exportContent, copyToClipboard, downloadAsFile, generateBloggerHTML, FullExport } from './ExportManager';

interface ExportModalProps {
  editor: Editor;
  isOpen: boolean;
  onClose: () => void;
}

type TabType = 'html' | 'schema' | 'json' | 'blogger';

export const ExportModal: React.FC<ExportModalProps> = ({ editor, isOpen, onClose }) => {
  const [activeTab, setActiveTab] = useState<TabType>('html');
  const [exportResult, setExportResult] = useState<FullExport | null>(null);
  const [copied, setCopied] = useState(false);
  const [showOptions, setShowOptions] = useState(false);
  
  // Export options
  const [includeTOC, setIncludeTOC] = useState(true);
  const [tocPosition, setTocPosition] = useState<'top' | 'afterFirstHeading'>('top');
  const [minify, setMinify] = useState(false);

  useEffect(() => {
    if (isOpen && editor) {
      const result = exportContent(editor, {
        format: 'html',
        includeTOC,
        tocPosition,
        minify,
      });
      setExportResult(result);
    }
  }, [isOpen, editor, includeTOC, tocPosition, minify]);

  useEffect(() => {
    if (copied) {
      const timer = setTimeout(() => setCopied(false), 2000);
      return () => clearTimeout(timer);
    }
  }, [copied]);

  if (!isOpen || !exportResult) return null;

  const handleCopy = async (content: string) => {
    const success = await copyToClipboard(content);
    if (success) {
      setCopied(true);
    }
  };

  const handleDownload = (content: string, extension: string) => {
    const filename = `export-${Date.now()}.${extension}`;
    const mimeType = extension === 'json' ? 'application/json' : 'text/html';
    downloadAsFile(content, filename, mimeType);
  };

  const getTabContent = (): string => {
    switch (activeTab) {
      case 'html':
        return exportResult.html;
      case 'schema':
        return exportResult.schemaScripts || '<!-- No schema data available -->';
      case 'json':
        return JSON.stringify(exportResult.json, null, 2);
      case 'blogger':
        return generateBloggerHTML(exportResult);
      default:
        return '';
    }
  };

  const tabs = [
    { id: 'html' as TabType, label: 'Clean HTML', icon: Code },
    { id: 'schema' as TabType, label: 'Schema', icon: Zap },
    { id: 'json' as TabType, label: 'JSON', icon: FileJson },
    { id: 'blogger' as TabType, label: 'Blogger Ready', icon: FileText },
  ];

  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm">
      <div 
        className="w-full max-w-4xl max-h-[90vh] bg-white rounded-2xl shadow-2xl overflow-hidden flex flex-col"
        onClick={(e) => e.stopPropagation()}
      >
        {/* Header */}
        <div className="px-6 py-4 border-b border-gray-200 bg-gradient-to-r from-blue-50 to-indigo-50">
          <div className="flex items-center justify-between">
            <div className="flex items-center gap-3">
              <div className="p-2 bg-blue-600 rounded-lg">
                <Sparkles className="w-5 h-5 text-white" />
              </div>
              <div>
                <h2 className="text-xl font-bold text-gray-800">Export Content</h2>
                <p className="text-sm text-gray-500">Generate clean HTML for your blog</p>
              </div>
            </div>
            <button
              onClick={onClose}
              className="p-2 text-gray-400 hover:text-gray-600 hover:bg-gray-100 rounded-lg transition-colors"
            >
              <X className="w-5 h-5" />
            </button>
          </div>

          {/* Stats */}
          <div className="flex gap-4 mt-4 text-sm">
            <div className="px-3 py-1 bg-white rounded-full border border-blue-200 text-blue-700">
              📝 {exportResult.stats.wordCount} words
            </div>
            <div className="px-3 py-1 bg-white rounded-full border border-purple-200 text-purple-700">
              📑 {exportResult.stats.headings} headings
            </div>
            <div className="px-3 py-1 bg-white rounded-full border border-amber-200 text-amber-700">
              ❓ {exportResult.stats.faqs} FAQs
            </div>
            <div className="px-3 py-1 bg-white rounded-full border border-emerald-200 text-emerald-700">
              📢 {exportResult.stats.ctas} CTAs
            </div>
          </div>
        </div>

        {/* Options Toggle */}
        <div className="px-6 py-3 border-b border-gray-100 bg-gray-50">
          <button
            onClick={() => setShowOptions(!showOptions)}
            className="flex items-center gap-2 text-sm text-gray-600 hover:text-gray-800"
          >
            <Settings className="w-4 h-4" />
            Export Options
            {showOptions ? <ChevronUp className="w-4 h-4" /> : <ChevronDown className="w-4 h-4" />}
          </button>

          {showOptions && (
            <div className="mt-3 grid grid-cols-3 gap-4">
              {/* Include TOC */}
              <label className="flex items-center gap-2 cursor-pointer">
                <input
                  type="checkbox"
                  checked={includeTOC}
                  onChange={(e) => setIncludeTOC(e.target.checked)}
                  className="w-4 h-4 text-blue-600 rounded border-gray-300 focus:ring-blue-500"
                />
                <span className="text-sm text-gray-700">Include TOC Placeholder</span>
              </label>

              {/* TOC Position */}
              <div className="flex items-center gap-2">
                <span className="text-sm text-gray-600">TOC Position:</span>
                <select
                  value={tocPosition}
                  onChange={(e) => setTocPosition(e.target.value as 'top' | 'afterFirstHeading')}
                  disabled={!includeTOC}
                  className="text-sm border border-gray-300 rounded px-2 py-1 disabled:opacity-50"
                >
                  <option value="top">Top</option>
                  <option value="afterFirstHeading">After First Heading</option>
                </select>
              </div>

              {/* Minify */}
              <label className="flex items-center gap-2 cursor-pointer">
                <input
                  type="checkbox"
                  checked={minify}
                  onChange={(e) => setMinify(e.target.checked)}
                  className="w-4 h-4 text-blue-600 rounded border-gray-300 focus:ring-blue-500"
                />
                <span className="text-sm text-gray-700">Minify Output</span>
              </label>
            </div>
          )}
        </div>

        {/* Tabs */}
        <div className="flex border-b border-gray-200">
          {tabs.map((tab) => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`
                flex items-center gap-2 px-6 py-3 text-sm font-medium transition-colors
                ${activeTab === tab.id
                  ? 'text-blue-600 border-b-2 border-blue-600 bg-blue-50/50'
                  : 'text-gray-500 hover:text-gray-700 hover:bg-gray-50'
                }
              `}
            >
              <tab.icon className="w-4 h-4" />
              {tab.label}
            </button>
          ))}
        </div>

        {/* Tab Content */}
        <div className="flex-1 overflow-hidden flex flex-col">
          {/* Tab Description */}
          <div className="px-6 py-3 bg-gray-50 border-b border-gray-100">
            {activeTab === 'html' && (
              <div className="flex items-start gap-2 text-sm text-gray-600">
                <AlertCircle className="w-4 h-4 mt-0.5 text-blue-500" />
                <span>
                  Clean HTML with semantic class names. Your <code className="px-1 bg-gray-200 rounded">widget.js</code> will handle styling and interactivity.
                </span>
              </div>
            )}
            {activeTab === 'schema' && (
              <div className="flex items-start gap-2 text-sm text-gray-600">
                <AlertCircle className="w-4 h-4 mt-0.5 text-purple-500" />
                <span>
                  JSON-LD Schema markup for SEO. Add this to your page's <code className="px-1 bg-gray-200 rounded">&lt;head&gt;</code> section.
                </span>
              </div>
            )}
            {activeTab === 'json' && (
              <div className="flex items-start gap-2 text-sm text-gray-600">
                <AlertCircle className="w-4 h-4 mt-0.5 text-emerald-500" />
                <span>
                  Raw JSON format. Useful for storing content in a database or CMS.
                </span>
              </div>
            )}
            {activeTab === 'blogger' && (
              <div className="flex items-start gap-2 text-sm text-gray-600">
                <AlertCircle className="w-4 h-4 mt-0.5 text-orange-500" />
                <span>
                  Blogger-optimized HTML with comments and instructions. Ready to paste into your post.
                </span>
              </div>
            )}
          </div>

          {/* Code Preview */}
          <div className="flex-1 overflow-auto p-4 bg-gray-900">
            <pre className="text-sm text-gray-100 font-mono whitespace-pre-wrap break-words">
              <code>{getTabContent()}</code>
            </pre>
          </div>
        </div>

        {/* Footer Actions */}
        <div className="px-6 py-4 border-t border-gray-200 bg-gray-50 flex items-center justify-between">
          <div className="text-sm text-gray-500">
            {getTabContent().length.toLocaleString()} characters
          </div>
          <div className="flex gap-3">
            <button
              onClick={() => handleDownload(
                getTabContent(),
                activeTab === 'json' ? 'json' : 'html'
              )}
              className="flex items-center gap-2 px-4 py-2 text-sm font-medium text-gray-700 
                       bg-white border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors"
            >
              <Download className="w-4 h-4" />
              Download
            </button>
            <button
              onClick={() => handleCopy(getTabContent())}
              className={`
                flex items-center gap-2 px-4 py-2 text-sm font-medium rounded-lg transition-all
                ${copied
                  ? 'bg-green-600 text-white'
                  : 'bg-blue-600 text-white hover:bg-blue-700'
                }
              `}
            >
              {copied ? (
                <>
                  <Check className="w-4 h-4" />
                  Copied!
                </>
              ) : (
                <>
                  <Copy className="w-4 h-4" />
                  Copy to Clipboard
                </>
              )}
            </button>
          </div>
        </div>
      </div>
    </div>
  );
};

export default ExportModal;
```

---

## 4. Export Index File

### `src/components/editor/export/index.ts`

```typescript
export { serializeToCleanHTML, generateSchemaScript } from './HTMLSerializer';
export type { SerializerOptions, ExportResult } from './HTMLSerializer';

export { exportContent, copyToClipboard, downloadAsFile, generateBloggerHTML } from './ExportManager';
export type { ExportOptions, FullExport } from './ExportManager';

export { ExportModal } from './ExportModal';
```

---

## 5. Updated Editor Component with Export

### `src/components/editor/Editor.tsx` (Updated)

```tsx
'use client';

import React, { useCallback, useState } from 'react';
import { useEditor, EditorContent } from '@tiptap/react';
import StarterKit from '@tiptap/starter-kit';
import Placeholder from '@tiptap/extension-placeholder';
import MenuBar from './MenuBar';
import EditorBubbleMenu from './EditorBubbleMenu';
import { ExportModal } from './export';

// Custom Extensions
import { CallToAction } from './extensions/CallToAction/CallToAction';
import { FaqBlock } from './extensions/FaqBlock/FaqBlock';
import { ProsConsBlock } from './extensions/ProsConsBlock/ProsConsBlock';

interface NotionEditorProps {
  initialContent?: string;
  onChange?: (html: string, json: object) => void;
  placeholder?: string;
}

export const NotionEditor: React.FC<NotionEditorProps> = ({
  initialContent = '',
  onChange,
  placeholder = "Type '/' for commands, or start writing...",
}) => {
  const [isExportModalOpen, setIsExportModalOpen] = useState(false);

  const editor = useEditor({
    extensions: [
      StarterKit.configure({
        heading: {
          levels: [1, 2, 3],
        },
        bulletList: {
          keepMarks: true,
          keepAttributes: false,
        },
        orderedList: {
          keepMarks: true,
          keepAttributes: false,
        },
      }),
      Placeholder.configure({
        placeholder: ({ node }) => {
          if (node.type.name === 'heading') {
            return `Heading ${node.attrs.level}`;
          }
          return placeholder;
        },
        emptyEditorClass: 'is-editor-empty',
        emptyNodeClass: 'is-empty',
      }),
      // Custom Extensions
      CallToAction,
      FaqBlock,
      ProsConsBlock,
    ],
    content: initialContent,
    editorProps: {
      attributes: {
        class: 'notion-editor prose prose-sm sm:prose lg:prose-lg focus:outline-none',
        spellcheck: 'true',
      },
    },
    onUpdate: ({ editor }) => {
      if (onChange) {
        const html = editor.getHTML();
        const json = editor.getJSON();
        onChange(html, json);
      }
    },
    immediatelyRender: false,
  });

  const handleClearContent = useCallback(() => {
    if (editor) {
      editor.commands.clearContent();
      editor.commands.focus();
    }
  }, [editor]);

  return (
    <div className="w-full max-w-4xl mx-auto">
      <div className="bg-white rounded-lg border border-notion-border shadow-sm overflow-hidden">
        <MenuBar editor={editor} />

        <div className="p-8 min-h-[500px]">
          {editor && <EditorBubbleMenu editor={editor} />}
          <EditorContent editor={editor} />
        </div>

        {/* Footer with Generate Code button */}
        <div className="border-t border-notion-border px-4 py-3 bg-notion-bg-gray flex items-center justify-between">
          <div className="text-sm text-notion-gray">
            {editor && (
              <span>
                Ready to export
              </span>
            )}
          </div>
          <div className="flex gap-2">
            <button
              onClick={handleClearContent}
              className="px-3 py-1.5 text-sm text-notion-gray hover:text-notion-default 
                         hover:bg-notion-bg-hover rounded transition-colors"
            >
              Clear
            </button>
            <button
              onClick={() => setIsExportModalOpen(true)}
              className="flex items-center gap-2 px-4 py-2 text-sm font-semibold 
                       bg-gradient-to-r from-blue-600 to-indigo-600 text-white 
                       hover:from-blue-700 hover:to-indigo-700 rounded-lg 
                       shadow-lg shadow-blue-500/25 transition-all duration-200
                       hover:shadow-xl hover:-translate-y-0.5"
            >
              <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" />
              </svg>
              Generate Code
            </button>
          </div>
        </div>
      </div>

      {/* Export Modal */}
      {editor && (
        <ExportModal
          editor={editor}
          isOpen={isExportModalOpen}
          onClose={() => setIsExportModalOpen(false)}
        />
      )}
    </div>
  );
};

export default NotionEditor;
```

---

## 6. Sample Exported HTML Output

Here's what the exported HTML looks like for each custom block:

### Clean HTML Output Example

```html
<article class="post-body">
<!-- Table of Contents - Auto-generated by widget.js -->
<div id="my-tool-toc" class="toc-container"></div>

<h2 id="getting-started-0" class="heading-2">Getting Started</h2>
<p>Welcome to our <strong>amazing</strong> product. Here's what you need to know.</p>

<h3 id="features-1" class="heading-3">Features</h3>
<ul class="bullet-list">
  <li>Easy to use interface</li>
  <li>Powerful customization</li>
  <li>Fast performance</li>
</ul>

<div class="cta-block" data-variant="primary">
  <div class="cta-content">
    <h3 class="cta-title">Ready to Get Started?</h3>
    <p class="cta-description">Join thousands of happy customers today.</p>
  </div>
  <a href="https://example.com" class="cta-button" target="_blank" rel="noopener noreferrer">
    Sign Up Now
  </a>
</div>

<div class="faq-block" data-faq-count="2">
  <h3 class="faq-title">Frequently Asked Questions</h3>
  <div class="faq-list">
  <div class="faq-item" data-faq-index="0">
    <div class="faq-question" data-question="What is this product?">
      <span class="faq-question-text">What is this product?</span>
      <span class="faq-toggle-icon"></span>
    </div>
    <div class="faq-answer" data-answer="This is an amazing product that helps you accomplish great things.">
      <p>This is an amazing product that helps you accomplish great things.</p>
    </div>
  </div>
  <div class="faq-item" data-faq-index="1">
    <div class="faq-question" data-question="How do I get started?">
      <span class="faq-question-text">How do I get started?</span>
      <span class="faq-toggle-icon"></span>
    </div>
    <div class="faq-answer" data-answer="Simply sign up and follow the guide.">
      <p>Simply sign up and follow the guide.</p>
    </div>
  </div>
  </div>
</div>

<div class="pros-cons-block" data-pros-count="3" data-cons-count="2">
  <h3 class="pros-cons-title">Pros &amp; Cons</h3>
  <div class="pros-cons-grid">
    <div class="pros-column">
      <h4 class="pros-heading">
        <span class="pros-icon"></span>
        Pros
      </h4>
      <ul class="pros-list">
      <li class="pros-item">Easy to use</li>
      <li class="pros-item">Great support</li>
      <li class="pros-item">Affordable pricing</li>
      </ul>
    </div>
    <div class="cons-column">
      <h4 class="cons-heading">
        <span class="cons-icon"></span>
        Cons
      </h4>
      <ul class="cons-list">
      <li class="cons-item">Limited features</li>
      <li class="cons-item">Requires internet</li>
      </ul>
    </div>
  </div>
</div>

</article>
```

---

## 7. Updated Widget.js for Styling Exported HTML

### `widget.js` (Updated to style exported blocks)

```javascript
/**
 * Blogger Widget SDK v2.0.0
 * Styles and enhances exported HTML from Notion-Style Editor
 */
(function() {
    'use strict';

    const BloggerSDK = {
        config: {
            tocContainer: 'my-tool-toc',
            contentSelector: '.post-body',
            headingSelectors: ['h2', 'h3'],
            smoothScrollOffset: 80,
        },

        init: function() {
            this.injectStyles();
            this.generateTOC();
            this.initFaqBlocks();
            this.initCtaBlocks();
            this.initProsConsBlocks();
            this.formatExternalLinks();
            this.injectSchemas();
        },

        /**
         * Inject CSS styles for all custom blocks
         */
        injectStyles: function() {
            const styles = `
                /* TOC Styles */
                .toc-container {
                    background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
                    border: 1px solid #e2e8f0;
                    border-radius: 12px;
                    padding: 20px 24px;
                    margin: 24px 0;
                }
                .toc-container .toc-title {
                    font-size: 1.1rem;
                    font-weight: 700;
                    color: #1e293b;
                    margin-bottom: 12px;
                    display: flex;
                    align-items: center;
                    gap: 8px;
                }
                .toc-container .toc-title::before {
                    content: '📑';
                }
                .toc-container ul {
                    list-style: none;
                    padding: 0;
                    margin: 0;
                }
                .toc-container li {
                    margin: 8px 0;
                }
                .toc-container a {
                    color: #475569;
                    text-decoration: none;
                    transition: all 0.2s;
                    display: inline-block;
                }
                .toc-container a:hover {
                    color: #3b82f6;
                    transform: translateX(4px);
                }
                .toc-container .toc-h3 {
                    margin-left: 20px;
                    font-size: 0.95em;
                }

                /* CTA Block Styles */
                .cta-block {
                    background: linear-gradient(135deg, #eff6ff 0%, #dbeafe 100%);
                    border: 2px solid #bfdbfe;
                    border-radius: 16px;
                    padding: 32px;
                    margin: 32px 0;
                    display: flex;
                    flex-wrap: wrap;
                    align-items: center;
                    gap: 24px;
                }
                .cta-block[data-variant="secondary"] {
                    background: linear-gradient(135deg, #f8fafc 0%, #f1f5f9 100%);
                    border-color: #e2e8f0;
                }
                .cta-block[data-variant="gradient"] {
                    background: linear-gradient(135deg, #faf5ff 0%, #fce7f3 50%, #fff1f2 100%);
                    border-color: #e9d5ff;
                }
                .cta-content {
                    flex: 1;
                    min-width: 200px;
                }
                .cta-title {
                    font-size: 1.5rem;
                    font-weight: 700;
                    color: #1e40af;
                    margin: 0 0 8px 0;
                }
                .cta-block[data-variant="gradient"] .cta-title {
                    color: #7c3aed;
                }
                .cta-description {
                    color: #3b82f6;
                    margin: 0;
                }
                .cta-button {
                    display: inline-flex;
                    align-items: center;
                    gap: 8px;
                    padding: 14px 28px;
                    background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
                    color: white;
                    font-weight: 600;
                    text-decoration: none;
                    border-radius: 10px;
                    transition: all 0.2s;
                    box-shadow: 0 4px 14px rgba(59, 130, 246, 0.4);
                }
                .cta-button:hover {
                    transform: translateY(-2px);
                    box-shadow: 0 6px 20px rgba(59, 130, 246, 0.5);
                }
                .cta-block[data-variant="gradient"] .cta-button {
                    background: linear-gradient(135deg, #8b5cf6 0%, #d946ef 100%);
                    box-shadow: 0 4px 14px rgba(139, 92, 246, 0.4);
                }

                /* FAQ Block Styles */
                .faq-block {
                    background: linear-gradient(135deg, #fffbeb 0%, #fef3c7 100%);
                    border: 2px solid #fcd34d;
                    border-radius: 16px;
                    overflow: hidden;
                    margin: 32px 0;
                }
                .faq-title {
                    background: linear-gradient(135deg, #fbbf24 0%, #f59e0b 100%);
                    color: white;
                    padding: 16px 24px;
                    margin: 0;
                    font-size: 1.25rem;
                    display: flex;
                    align-items: center;
                    gap: 10px;
                }
                .faq-title::before {
                    content: '❓';
                }
                .faq-list {
                    padding: 0;
                }
                .faq-item {
                    border-bottom: 1px solid #fcd34d;
                }
                .faq-item:last-child {
                    border-bottom: none;
                }
                .faq-question {
                    padding: 16px 24px;
                    cursor: pointer;
                    display: flex;
                    justify-content: space-between;
                    align-items: center;
                    font-weight: 600;
                    color: #92400e;
                    transition: background 0.2s;
                }
                .faq-question:hover {
                    background: rgba(251, 191, 36, 0.2);
                }
                .faq-toggle-icon {
                    width: 24px;
                    height: 24px;
                    border-radius: 50%;
                    background: #fbbf24;
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    transition: transform 0.3s;
                }
                .faq-toggle-icon::after {
                    content: '+';
                    color: white;
                    font-weight: bold;
                }
                .faq-item.active .faq-toggle-icon {
                    transform: rotate(45deg);
                }
                .faq-answer {
                    max-height: 0;
                    overflow: hidden;
                    transition: max-height 0.3s ease, padding 0.3s ease;
                    background: white;
                }
                .faq-item.active .faq-answer {
                    max-height: 500px;
                    padding: 16px 24px;
                }
                .faq-answer p {
                    margin: 0;
                    color: #78350f;
                    line-height: 1.6;
                }

                /* Pros/Cons Block Styles */
                .pros-cons-block {
                    border: 2px solid #e5e7eb;
                    border-radius: 16px;
                    overflow: hidden;
                    margin: 32px 0;
                }
                .pros-cons-title {
                    background: linear-gradient(135deg, #f3f4f6 0%, #e5e7eb 100%);
                    padding: 16px 24px;
                    margin: 0;
                    font-size: 1.25rem;
                    color: #374151;
                    display: flex;
                    align-items: center;
                    gap: 10px;
                }
                .pros-cons-title::before {
                    content: '⚖️';
                }
                .pros-cons-grid {
                    display: grid;
                    grid-template-columns: 1fr 1fr;
                }
                @media (max-width: 640px) {
                    .pros-cons-grid {
                        grid-template-columns: 1fr;
                    }
                }
                .pros-column {
                    background: linear-gradient(135deg, #ecfdf5 0%, #d1fae5 100%);
                    padding: 20px 24px;
                }
                .cons-column {
                    background: linear-gradient(135deg, #fef2f2 0%, #fecaca 100%);
                    padding: 20px 24px;
                }
                .pros-heading, .cons-heading {
                    display: flex;
                    align-items: center;
                    gap: 8px;
                    margin: 0 0 12px 0;
                    font-size: 1.1rem;
                }
                .pros-heading {
                    color: #065f46;
                }
                .cons-heading {
                    color: #991b1b;
                }
                .pros-icon::before {
                    content: '👍';
                }
                .cons-icon::before {
                    content: '👎';
                }
                .pros-list, .cons-list {
                    list-style: none;
                    padding: 0;
                    margin: 0;
                }
                .pros-item, .cons-item {
                    padding: 8px 0;
                    padding-left: 24px;
                    position: relative;
                }
                .pros-item::before {
                    content: '✓';
                    position: absolute;
                    left: 0;
                    color: #059669;
                    font-weight: bold;
                }
                .cons-item::before {
                    content: '✗';
                    position: absolute;
                    left: 0;
                    color: #dc2626;
                    font-weight: bold;
                }
                .pros-item {
                    color: #065f46;
                }
                .cons-item {
                    color: #991b1b;
                }
            `;

            const styleElement = document.createElement('style');
            styleElement.id = 'blogger-sdk-styles';
            styleElement.textContent = styles;
            document.head.appendChild(styleElement);
        },

        /**
         * Generate Table of Contents
         */
        generateTOC: function() {
            const tocContainer = document.getElementById(this.config.tocContainer);
            if (!tocContainer) return;

            const content = document.querySelector(this.config.contentSelector);
            if (!content) return;

            const headings = content.querySelectorAll(this.config.headingSelectors.join(', '));
            if (headings.length < 2) {
                tocContainer.style.display = 'none';
                return;
            }

            let html = '<div class="toc-title">Table of Contents</div><ul>';
            
            headings.forEach(function(heading) {
                const level = heading.tagName.toLowerCase();
                const text = heading.textContent;
                const id = heading.id || heading.textContent.toLowerCase().replace(/\s+/g, '-');
                heading.id = id;
                
                html += `<li class="toc-${level}"><a href="#${id}">${text}</a></li>`;
            });
            
            html += '</ul>';
            tocContainer.innerHTML = html;

            // Smooth scroll
            tocContainer.querySelectorAll('a').forEach(link => {
                link.addEventListener('click', (e) => {
                    e.preventDefault();
                    const target = document.querySelector(link.getAttribute('href'));
                    if (target) {
                        const offset = target.offsetTop - this.config.smoothScrollOffset;
                        window.scrollTo({ top: offset, behavior: 'smooth' });
                    }
                });
            });
        },

        /**
         * Initialize FAQ accordion functionality
         */
        initFaqBlocks: function() {
            const faqItems = document.querySelectorAll('.faq-item');
            
            faqItems.forEach(function(item) {
                const question = item.querySelector('.faq-question');
                
                question.addEventListener('click', function() {
                    // Close other items in same block
                    const parentBlock = item.closest('.faq-block');
                    parentBlock.querySelectorAll('.faq-item.active').forEach(function(activeItem) {
                        if (activeItem !== item) {
                            activeItem.classList.remove('active');
                        }
                    });
                    
                    // Toggle current item
                    item.classList.toggle('active');
                });
            });
        },

        /**
         * Initialize CTA blocks (add click tracking, etc.)
         */
        initCtaBlocks: function() {
            const ctaButtons = document.querySelectorAll('.cta-button');
            
            ctaButtons.forEach(function(button) {
                button.addEventListener('click', function() {
                    // Track click (you can integrate with analytics)
                    console.log('CTA clicked:', button.textContent);
                });
            });
        },

        /**
         * Initialize Pros/Cons blocks (optional animations)
         */
        initProsConsBlocks: function() {
            // Add any interactive features here
        },

        /**
         * Format external links
         */
        formatExternalLinks: function() {
            const content = document.querySelector(this.config.contentSelector);
            if (!content) return;

            const links = content.querySelectorAll('a[href^="http"]');
            const currentHost = window.location.hostname;

            links.forEach(function(link) {
                try {
                    const url = new URL(link.href);
                    if (url.hostname !== currentHost) {
                        link.setAttribute('target', '_blank');
                        link.setAttribute('rel', 'nofollow noopener');
                    }
                } catch (e) {}
            });
        },

        /**
         * Inject FAQ Schema from data attributes
         */
        injectSchemas: function() {
            const faqBlocks = document.querySelectorAll('.faq-block');
            if (faqBlocks.length === 0) return;

            const faqItems = [];
            
            document.querySelectorAll('.faq-item').forEach(function(item) {
                const questionEl = item.querySelector('.faq-question');
                const answerEl = item.querySelector('.faq-answer');
                
                if (questionEl && answerEl) {
                    faqItems.push({
                        "@type": "Question",
                        "name": questionEl.getAttribute('data-question'),
                        "acceptedAnswer": {
                            "@type": "Answer",
                            "text": answerEl.getAttribute('data-answer')
                        }
                    });
                }
            });

            if (faqItems.length > 0) {
                const schema = {
                    "@context": "https://schema.org",
                    "@type": "FAQPage",
                    "mainEntity": faqItems
                };

                const script = document.createElement('script');
                script.type = 'application/ld+json';
                script.textContent = JSON.stringify(schema);
                document.head.appendChild(script);
            }
        }
    };

    // Initialize
    if (document.readyState === 'complete') {
        BloggerSDK.init();
    } else {
        window.addEventListener('load', function() {
            BloggerSDK.init();
        });
    }

    window.BloggerSDK = BloggerSDK;
})();
```

---

## Summary

| Feature | Export Format |
|---------|--------------|
| **TOC** | `<div id="my-tool-toc" class="toc-container"></div>` |
| **Headings** | `<h2 id="slug-0" class="heading-2">Title</h2>` |
| **CTA Block** | `<div class="cta-block" data-variant="primary">...</div>` |
| **FAQ Block** | `<div class="faq-block" data-faq-count="N">...</div>` |
| **FAQ Item** | `<div class="faq-item" data-faq-index="0">...</div>` |
| **Pros/Cons** | `<div class="pros-cons-block" data-pros-count="N" data-cons-count="N">...</div>` |

All styling is handled by `widget.js` - the exported HTML is clean and semantic! 🎉
