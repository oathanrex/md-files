# Copy to Clipboard with Script Tag & Instructions

Create a comprehensive copy-to-clipboard functionality that includes the CDN script tag with clear setup instructions, along with the generated HTML content.

## Updated Export Components

### `src/components/editor/export/CopyButton.tsx`

```tsx
'use client';

import React, { useState, useCallback } from 'react';
import {
  Copy,
  Check,
  Code,
  FileText,
  Layers,
  ChevronDown,
  ExternalLink,
  Info,
  Clipboard,
  Package,
} from 'lucide-react';

interface CopyButtonProps {
  htmlContent: string;
  variant?: 'default' | 'compact' | 'full';
  className?: string;
}

type CopyMode = 'all' | 'script' | 'html';

const SCRIPT_TAG = `<script src="https://cdn.jsdelivr.net/gh/oathanrex/script@v1.0.0/widget.js"></script>`;

const SCRIPT_INSTRUCTIONS = `<!-- ============================================
   WIDGET.JS INSTALLATION INSTRUCTIONS
   ============================================
   
   Add this script tag ONCE to your Blogger Layout:
   
   1. Go to Blogger Dashboard → Theme → Edit HTML
   2. Find the closing </body> tag
   3. Paste the script tag just BEFORE </body>
   4. Save your theme
   
   This only needs to be done once. After that,
   just paste the HTML content into your posts.
   ============================================ -->

${SCRIPT_TAG}`;

const generateFullOutput = (htmlContent: string): string => {
  const timestamp = new Date().toISOString();
  
  return `<!-- ============================================
   BLOGGER POST CONTENT
   Generated: ${timestamp}
   ============================================
   
   STEP 1: Add Widget.js (One-time setup)
   ----------------------------------------
   Add this script to your Blogger Layout:
   Go to Theme → Edit HTML → Before </body>
   
${SCRIPT_TAG}
   
   STEP 2: Paste Content Below Into Your Post
   ----------------------------------------
============================================ -->

${htmlContent}

<!-- End of generated content -->`;
};

export const CopyButton: React.FC<CopyButtonProps> = ({
  htmlContent,
  variant = 'default',
  className = '',
}) => {
  const [copied, setCopied] = useState<CopyMode | null>(null);
  const [isDropdownOpen, setIsDropdownOpen] = useState(false);

  const copyToClipboard = useCallback(async (text: string, mode: CopyMode) => {
    try {
      await navigator.clipboard.writeText(text);
      setCopied(mode);
      setTimeout(() => setCopied(null), 2500);
      setIsDropdownOpen(false);
    } catch (err) {
      // Fallback for older browsers
      const textArea = document.createElement('textarea');
      textArea.value = text;
      textArea.style.position = 'fixed';
      textArea.style.left = '-9999px';
      textArea.style.top = '-9999px';
      document.body.appendChild(textArea);
      textArea.focus();
      textArea.select();
      
      try {
        document.execCommand('copy');
        setCopied(mode);
        setTimeout(() => setCopied(null), 2500);
      } catch (e) {
        console.error('Failed to copy:', e);
      }
      
      document.body.removeChild(textArea);
      setIsDropdownOpen(false);
    }
  }, []);

  const handleCopyAll = useCallback(() => {
    const fullOutput = generateFullOutput(htmlContent);
    copyToClipboard(fullOutput, 'all');
  }, [htmlContent, copyToClipboard]);

  const handleCopyScript = useCallback(() => {
    copyToClipboard(SCRIPT_INSTRUCTIONS, 'script');
  }, [copyToClipboard]);

  const handleCopyHtml = useCallback(() => {
    copyToClipboard(htmlContent, 'html');
  }, [htmlContent, copyToClipboard]);

  // Compact variant - single button
  if (variant === 'compact') {
    return (
      <button
        onClick={handleCopyAll}
        className={`
          inline-flex items-center gap-2 px-4 py-2 rounded-lg font-medium
          transition-all duration-200
          ${copied === 'all'
            ? 'bg-green-600 text-white'
            : 'bg-blue-600 text-white hover:bg-blue-700'
          }
          ${className}
        `}
      >
        {copied === 'all' ? (
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
    );
  }

  // Full variant - detailed card
  if (variant === 'full') {
    return (
      <div className={`bg-white rounded-xl border border-gray-200 overflow-hidden ${className}`}>
        {/* Header */}
        <div className="px-5 py-4 bg-gradient-to-r from-blue-50 to-indigo-50 border-b border-blue-100">
          <div className="flex items-center gap-3">
            <div className="p-2 bg-blue-600 rounded-lg">
              <Clipboard className="w-5 h-5 text-white" />
            </div>
            <div>
              <h3 className="font-bold text-gray-800">Copy to Clipboard</h3>
              <p className="text-sm text-gray-500">Choose what to copy</p>
            </div>
          </div>
        </div>

        {/* Copy Options */}
        <div className="p-4 space-y-3">
          {/* Copy All Button */}
          <button
            onClick={handleCopyAll}
            className={`
              w-full flex items-center justify-between p-4 rounded-lg border-2 transition-all
              ${copied === 'all'
                ? 'border-green-500 bg-green-50'
                : 'border-gray-200 hover:border-blue-300 hover:bg-blue-50'
              }
            `}
          >
            <div className="flex items-center gap-3">
              <div className={`p-2 rounded-lg ${copied === 'all' ? 'bg-green-500' : 'bg-blue-600'}`}>
                <Layers className="w-5 h-5 text-white" />
              </div>
              <div className="text-left">
                <div className="font-semibold text-gray-800">
                  Copy Everything
                </div>
                <div className="text-sm text-gray-500">
                  Script tag + Instructions + HTML content
                </div>
              </div>
            </div>
            {copied === 'all' ? (
              <Check className="w-5 h-5 text-green-600" />
            ) : (
              <Copy className="w-5 h-5 text-gray-400" />
            )}
          </button>

          {/* Divider */}
          <div className="relative">
            <div className="absolute inset-0 flex items-center">
              <div className="w-full border-t border-gray-200"></div>
            </div>
            <div className="relative flex justify-center text-sm">
              <span className="px-2 bg-white text-gray-400">or copy separately</span>
            </div>
          </div>

          {/* Script Tag Button */}
          <button
            onClick={handleCopyScript}
            className={`
              w-full flex items-center justify-between p-3 rounded-lg border transition-all
              ${copied === 'script'
                ? 'border-green-500 bg-green-50'
                : 'border-gray-200 hover:border-purple-300 hover:bg-purple-50'
              }
            `}
          >
            <div className="flex items-center gap-3">
              <div className={`p-1.5 rounded ${copied === 'script' ? 'bg-green-500' : 'bg-purple-500'}`}>
                <Package className="w-4 h-4 text-white" />
              </div>
              <div className="text-left">
                <div className="font-medium text-gray-700 text-sm">
                  Widget.js Script Tag
                </div>
                <div className="text-xs text-gray-400">
                  Add once to your Blogger Layout
                </div>
              </div>
            </div>
            {copied === 'script' ? (
              <Check className="w-4 h-4 text-green-600" />
            ) : (
              <Copy className="w-4 h-4 text-gray-400" />
            )}
          </button>

          {/* HTML Content Button */}
          <button
            onClick={handleCopyHtml}
            className={`
              w-full flex items-center justify-between p-3 rounded-lg border transition-all
              ${copied === 'html'
                ? 'border-green-500 bg-green-50'
                : 'border-gray-200 hover:border-emerald-300 hover:bg-emerald-50'
              }
            `}
          >
            <div className="flex items-center gap-3">
              <div className={`p-1.5 rounded ${copied === 'html' ? 'bg-green-500' : 'bg-emerald-500'}`}>
                <FileText className="w-4 h-4 text-white" />
              </div>
              <div className="text-left">
                <div className="font-medium text-gray-700 text-sm">
                  HTML Content Only
                </div>
                <div className="text-xs text-gray-400">
                  Paste into your blog post
                </div>
              </div>
            </div>
            {copied === 'html' ? (
              <Check className="w-4 h-4 text-green-600" />
            ) : (
              <Copy className="w-4 h-4 text-gray-400" />
            )}
          </button>
        </div>

        {/* Info Footer */}
        <div className="px-4 py-3 bg-amber-50 border-t border-amber-100">
          <div className="flex items-start gap-2 text-xs text-amber-700">
            <Info className="w-4 h-4 shrink-0 mt-0.5" />
            <span>
              The script tag only needs to be added once to your Blogger theme. 
              After that, just paste the HTML content into each post.
            </span>
          </div>
        </div>
      </div>
    );
  }

  // Default variant - dropdown button
  return (
    <div className={`relative ${className}`}>
      <div className="flex">
        {/* Main Copy Button */}
        <button
          onClick={handleCopyAll}
          className={`
            flex items-center gap-2 px-4 py-2 rounded-l-lg font-medium transition-all
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

        {/* Dropdown Toggle */}
        <button
          onClick={() => setIsDropdownOpen(!isDropdownOpen)}
          className={`
            px-2 py-2 rounded-r-lg border-l transition-all
            ${copied
              ? 'bg-green-600 text-white border-green-500'
              : 'bg-blue-600 text-white hover:bg-blue-700 border-blue-500'
            }
          `}
        >
          <ChevronDown className={`w-4 h-4 transition-transform ${isDropdownOpen ? 'rotate-180' : ''}`} />
        </button>
      </div>

      {/* Dropdown Menu */}
      {isDropdownOpen && (
        <>
          {/* Backdrop */}
          <div
            className="fixed inset-0 z-10"
            onClick={() => setIsDropdownOpen(false)}
          />

          {/* Menu */}
          <div className="absolute right-0 mt-2 w-72 bg-white rounded-lg shadow-xl border border-gray-200 z-20 overflow-hidden">
            <div className="p-2 bg-gray-50 border-b border-gray-100">
              <span className="text-xs font-medium text-gray-500 uppercase tracking-wider">
                Copy Options
              </span>
            </div>

            <div className="p-1">
              {/* Copy All */}
              <button
                onClick={handleCopyAll}
                className="w-full flex items-center gap-3 px-3 py-2.5 hover:bg-blue-50 rounded-lg transition-colors text-left"
              >
                <div className="p-1.5 bg-blue-100 rounded">
                  <Layers className="w-4 h-4 text-blue-600" />
                </div>
                <div>
                  <div className="text-sm font-medium text-gray-800">Copy Everything</div>
                  <div className="text-xs text-gray-500">Script + Instructions + HTML</div>
                </div>
              </button>

              {/* Copy Script */}
              <button
                onClick={handleCopyScript}
                className="w-full flex items-center gap-3 px-3 py-2.5 hover:bg-purple-50 rounded-lg transition-colors text-left"
              >
                <div className="p-1.5 bg-purple-100 rounded">
                  <Code className="w-4 h-4 text-purple-600" />
                </div>
                <div>
                  <div className="text-sm font-medium text-gray-800">Script Tag Only</div>
                  <div className="text-xs text-gray-500">For Blogger Layout (one-time)</div>
                </div>
              </button>

              {/* Copy HTML */}
              <button
                onClick={handleCopyHtml}
                className="w-full flex items-center gap-3 px-3 py-2.5 hover:bg-emerald-50 rounded-lg transition-colors text-left"
              >
                <div className="p-1.5 bg-emerald-100 rounded">
                  <FileText className="w-4 h-4 text-emerald-600" />
                </div>
                <div>
                  <div className="text-sm font-medium text-gray-800">HTML Content Only</div>
                  <div className="text-xs text-gray-500">For blog post content</div>
                </div>
              </button>
            </div>

            {/* CDN Preview */}
            <div className="p-3 bg-gray-900 border-t border-gray-200">
              <div className="flex items-center justify-between mb-1">
                <span className="text-xs text-gray-400">CDN Script:</span>
                <a
                  href="https://cdn.jsdelivr.net/gh/oathanrex/script@v1.0.0/widget.js"
                  target="_blank"
                  rel="noopener noreferrer"
                  className="text-xs text-blue-400 hover:text-blue-300 flex items-center gap-1"
                >
                  View <ExternalLink className="w-3 h-3" />
                </a>
              </div>
              <code className="text-xs text-green-400 break-all">
                cdn.jsdelivr.net/gh/oathanrex/script@v1.0.0/widget.js
              </code>
            </div>
          </div>
        </>
      )}
    </div>
  );
};

export default CopyButton;
```

---

### `src/components/editor/export/CopyInstructions.tsx`

```tsx
'use client';

import React, { useState } from 'react';
import {
  Copy,
  Check,
  ChevronDown,
  ChevronUp,
  ExternalLink,
  AlertCircle,
  Zap,
  Layout,
  FileEdit,
} from 'lucide-react';

interface CopyInstructionsProps {
  className?: string;
}

const SCRIPT_TAG = `<script src="https://cdn.jsdelivr.net/gh/oathanrex/script@v1.0.0/widget.js"></script>`;

export const CopyInstructions: React.FC<CopyInstructionsProps> = ({ className = '' }) => {
  const [isExpanded, setIsExpanded] = useState(false);
  const [copied, setCopied] = useState(false);

  const copyScriptTag = async () => {
    try {
      await navigator.clipboard.writeText(SCRIPT_TAG);
      setCopied(true);
      setTimeout(() => setCopied(false), 2000);
    } catch (err) {
      console.error('Failed to copy:', err);
    }
  };

  return (
    <div className={`bg-gradient-to-r from-purple-50 to-indigo-50 rounded-xl border border-purple-200 overflow-hidden ${className}`}>
      {/* Header */}
      <button
        onClick={() => setIsExpanded(!isExpanded)}
        className="w-full flex items-center justify-between p-4 hover:bg-white/50 transition-colors"
      >
        <div className="flex items-center gap-3">
          <div className="p-2 bg-purple-600 rounded-lg">
            <Zap className="w-5 h-5 text-white" />
          </div>
          <div className="text-left">
            <h3 className="font-bold text-purple-900">Widget.js Setup</h3>
            <p className="text-sm text-purple-600">One-time installation for Blogger</p>
          </div>
        </div>
        {isExpanded ? (
          <ChevronUp className="w-5 h-5 text-purple-600" />
        ) : (
          <ChevronDown className="w-5 h-5 text-purple-600" />
        )}
      </button>

      {/* Expanded Content */}
      {isExpanded && (
        <div className="px-4 pb-4 space-y-4">
          {/* Script Tag Box */}
          <div className="bg-gray-900 rounded-lg p-4">
            <div className="flex items-center justify-between mb-2">
              <span className="text-xs text-gray-400 font-medium">SCRIPT TAG</span>
              <button
                onClick={copyScriptTag}
                className={`
                  flex items-center gap-1.5 px-2.5 py-1 rounded text-xs font-medium transition-all
                  ${copied
                    ? 'bg-green-600 text-white'
                    : 'bg-gray-700 text-gray-300 hover:bg-gray-600'
                  }
                `}
              >
                {copied ? (
                  <>
                    <Check className="w-3 h-3" />
                    Copied!
                  </>
                ) : (
                  <>
                    <Copy className="w-3 h-3" />
                    Copy
                  </>
                )}
              </button>
            </div>
            <code className="text-sm text-green-400 break-all font-mono">
              {SCRIPT_TAG}
            </code>
          </div>

          {/* Steps */}
          <div className="space-y-3">
            <h4 className="font-semibold text-purple-900 flex items-center gap-2">
              <Layout className="w-4 h-4" />
              Installation Steps
            </h4>

            <div className="space-y-2">
              {[
                {
                  step: 1,
                  title: 'Open Blogger Dashboard',
                  desc: 'Go to your Blogger account and select your blog',
                },
                {
                  step: 2,
                  title: 'Navigate to Theme',
                  desc: 'Click on "Theme" in the left sidebar',
                },
                {
                  step: 3,
                  title: 'Edit HTML',
                  desc: 'Click the dropdown arrow next to "Customize" → "Edit HTML"',
                },
                {
                  step: 4,
                  title: 'Find </body> tag',
                  desc: 'Press Ctrl+F and search for </body>',
                },
                {
                  step: 5,
                  title: 'Paste the script',
                  desc: 'Add the script tag just BEFORE the </body> tag',
                },
                {
                  step: 6,
                  title: 'Save theme',
                  desc: 'Click the save icon (💾) to save changes',
                },
              ].map((item) => (
                <div
                  key={item.step}
                  className="flex gap-3 p-2 rounded-lg hover:bg-white/50 transition-colors"
                >
                  <div className="w-6 h-6 rounded-full bg-purple-600 text-white text-xs font-bold flex items-center justify-center shrink-0">
                    {item.step}
                  </div>
                  <div>
                    <div className="font-medium text-purple-900 text-sm">{item.title}</div>
                    <div className="text-xs text-purple-600">{item.desc}</div>
                  </div>
                </div>
              ))}
            </div>
          </div>

          {/* Info Note */}
          <div className="flex items-start gap-2 p-3 bg-amber-100 rounded-lg border border-amber-200">
            <AlertCircle className="w-4 h-4 text-amber-600 shrink-0 mt-0.5" />
            <div className="text-xs text-amber-800">
              <strong>Important:</strong> You only need to add the script tag once. 
              After installation, just paste the generated HTML content directly into your blog posts.
            </div>
          </div>

          {/* CDN Link */}
          <div className="flex items-center justify-between p-3 bg-white/50 rounded-lg">
            <span className="text-sm text-purple-700">Hosted on jsDelivr CDN</span>
            <a
              href="https://cdn.jsdelivr.net/gh/oathanrex/script@v1.0.0/widget.js"
              target="_blank"
              rel="noopener noreferrer"
              className="text-sm text-purple-600 hover:text-purple-800 flex items-center gap-1 font-medium"
            >
              View Script <ExternalLink className="w-3 h-3" />
            </a>
          </div>
        </div>
      )}
    </div>
  );
};

export default CopyInstructions;
```

---

### `src/components/editor/export/ExportModal.tsx` (Updated)

```tsx
'use client';

import React, { useState, useEffect } from 'react';
import { Editor } from '@tiptap/react';
import {
  X,
  Download,
  Code,
  FileJson,
  FileText,
  Settings,
  ChevronDown,
  ChevronUp,
  Zap,
  AlertCircle,
  Sparkles,
  Eye,
} from 'lucide-react';
import { exportContent, downloadAsFile, generateBloggerHTML, FullExport } from './ExportManager';
import CopyButton from './CopyButton';
import CopyInstructions from './CopyInstructions';

interface ExportModalProps {
  editor: Editor;
  isOpen: boolean;
  onClose: () => void;
}

type TabType = 'html' | 'preview' | 'json';

export const ExportModal: React.FC<ExportModalProps> = ({ editor, isOpen, onClose }) => {
  const [activeTab, setActiveTab] = useState<TabType>('html');
  const [exportResult, setExportResult] = useState<FullExport | null>(null);
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

  if (!isOpen || !exportResult) return null;

  const handleDownload = (content: string, extension: string) => {
    const filename = `blogger-post-${Date.now()}.${extension}`;
    const mimeType = extension === 'json' ? 'application/json' : 'text/html';
    downloadAsFile(content, filename, mimeType);
  };

  const getTabContent = (): string => {
    switch (activeTab) {
      case 'html':
        return exportResult.html;
      case 'json':
        return JSON.stringify(exportResult.json, null, 2);
      default:
        return exportResult.html;
    }
  };

  const tabs = [
    { id: 'html' as TabType, label: 'HTML Code', icon: Code },
    { id: 'preview' as TabType, label: 'Preview', icon: Eye },
    { id: 'json' as TabType, label: 'JSON', icon: FileJson },
  ];

  return (
    <div 
      className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm"
      onClick={onClose}
    >
      <div 
        className="w-full max-w-5xl max-h-[90vh] bg-white rounded-2xl shadow-2xl overflow-hidden flex flex-col"
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
                <h2 className="text-xl font-bold text-gray-800">Generate Code</h2>
                <p className="text-sm text-gray-500">Export your content for Blogger</p>
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
          <div className="flex flex-wrap gap-3 mt-4">
            <div className="px-3 py-1.5 bg-white rounded-full border border-blue-200 text-blue-700 text-sm font-medium">
              📝 {exportResult.stats.wordCount} words
            </div>
            <div className="px-3 py-1.5 bg-white rounded-full border border-purple-200 text-purple-700 text-sm font-medium">
              📑 {exportResult.stats.headings} headings
            </div>
            {exportResult.stats.faqs > 0 && (
              <div className="px-3 py-1.5 bg-white rounded-full border border-amber-200 text-amber-700 text-sm font-medium">
                ❓ {exportResult.stats.faqs} FAQs
              </div>
            )}
            {exportResult.stats.ctas > 0 && (
              <div className="px-3 py-1.5 bg-white rounded-full border border-emerald-200 text-emerald-700 text-sm font-medium">
                📢 {exportResult.stats.ctas} CTAs
              </div>
            )}
          </div>
        </div>

        {/* Main Content - Two Column Layout */}
        <div className="flex-1 flex overflow-hidden">
          {/* Left Column - Code/Preview */}
          <div className="flex-1 flex flex-col border-r border-gray-200">
            {/* Options Toggle */}
            <div className="px-4 py-2 border-b border-gray-100 bg-gray-50">
              <button
                onClick={() => setShowOptions(!showOptions)}
                className="flex items-center gap-2 text-sm text-gray-600 hover:text-gray-800"
              >
                <Settings className="w-4 h-4" />
                Export Options
                {showOptions ? <ChevronUp className="w-4 h-4" /> : <ChevronDown className="w-4 h-4" />}
              </button>

              {showOptions && (
                <div className="mt-3 flex flex-wrap gap-4">
                  <label className="flex items-center gap-2 cursor-pointer">
                    <input
                      type="checkbox"
                      checked={includeTOC}
                      onChange={(e) => setIncludeTOC(e.target.checked)}
                      className="w-4 h-4 text-blue-600 rounded border-gray-300 focus:ring-blue-500"
                    />
                    <span className="text-sm text-gray-700">Include TOC</span>
                  </label>

                  <div className="flex items-center gap-2">
                    <span className="text-sm text-gray-600">Position:</span>
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

                  <label className="flex items-center gap-2 cursor-pointer">
                    <input
                      type="checkbox"
                      checked={minify}
                      onChange={(e) => setMinify(e.target.checked)}
                      className="w-4 h-4 text-blue-600 rounded border-gray-300 focus:ring-blue-500"
                    />
                    <span className="text-sm text-gray-700">Minify</span>
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
                    flex items-center gap-2 px-4 py-2.5 text-sm font-medium transition-colors
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
            <div className="flex-1 overflow-auto">
              {activeTab === 'preview' ? (
                <div className="p-6">
                  <div 
                    className="prose prose-sm max-w-none"
                    dangerouslySetInnerHTML={{ __html: exportResult.html }}
                  />
                </div>
              ) : (
                <div className="p-4 bg-gray-900 h-full">
                  <pre className="text-sm text-gray-100 font-mono whitespace-pre-wrap break-words">
                    <code>{getTabContent()}</code>
                  </pre>
                </div>
              )}
            </div>

            {/* Download Button */}
            <div className="px-4 py-3 border-t border-gray-200 bg-gray-50 flex items-center justify-between">
              <span className="text-sm text-gray-500">
                {getTabContent().length.toLocaleString()} characters
              </span>
              <button
                onClick={() => handleDownload(
                  getTabContent(),
                  activeTab === 'json' ? 'json' : 'html'
                )}
                className="flex items-center gap-2 px-4 py-2 text-sm font-medium text-gray-700 
                         bg-white border border-gray-300 rounded-lg hover:bg-gray-50 transition-colors"
              >
                <Download className="w-4 h-4" />
                Download File
              </button>
            </div>
          </div>

          {/* Right Column - Copy & Instructions */}
          <div className="w-96 flex flex-col bg-gray-50 overflow-y-auto">
            <div className="p-4 space-y-4">
              {/* Copy Button Component */}
              <CopyButton 
                htmlContent={exportResult.html} 
                variant="full" 
              />

              {/* Installation Instructions */}
              <CopyInstructions />

              {/* Quick Tips */}
              <div className="bg-white rounded-xl border border-gray-200 p-4">
                <h4 className="font-semibold text-gray-800 mb-3 flex items-center gap-2">
                  <Zap className="w-4 h-4 text-amber-500" />
                  Quick Tips
                </h4>
                <ul className="space-y-2 text-sm text-gray-600">
                  <li className="flex items-start gap-2">
                    <span className="text-green-500 mt-0.5">✓</span>
                    Widget.js handles all styling automatically
                  </li>
                  <li className="flex items-start gap-2">
                    <span className="text-green-500 mt-0.5">✓</span>
                    TOC is generated from your headings
                  </li>
                  <li className="flex items-start gap-2">
                    <span className="text-green-500 mt-0.5">✓</span>
                    FAQ blocks include SEO schema
                  </li>
                  <li className="flex items-start gap-2">
                    <span className="text-green-500 mt-0.5">✓</span>
                    External links get nofollow automatically
                  </li>
                </ul>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

export default ExportModal;
```

---

### `src/components/editor/export/index.ts` (Updated)

```typescript
export { serializeToCleanHTML, generateSchemaScript } from './HTMLSerializer';
export type { SerializerOptions, ExportResult } from './HTMLSerializer';

export { exportContent, copyToClipboard, downloadAsFile, generateBloggerHTML } from './ExportManager';
export type { ExportOptions, FullExport } from './ExportManager';

export { ExportModal } from './ExportModal';
export { CopyButton } from './CopyButton';
export { CopyInstructions } from './CopyInstructions';
```

---

## Quick Copy Button for Editor Footer

### Updated Footer in `EditorWithSidebar.tsx`

```tsx
// Add to imports
import { CopyButton } from './export';

// Update the footer section in EditorWithSidebar.tsx
<div className="border-t border-gray-200 px-4 py-3 bg-gray-50 flex items-center justify-between">
  <div className="flex items-center gap-3">
    <button
      onClick={toggleSidebar}
      className="flex items-center gap-2 px-3 py-1.5 text-sm text-gray-600 
               hover:text-gray-800 hover:bg-gray-200 rounded-lg transition-colors"
    >
      {isSidebarOpen ? (
        <>
          <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M13 5l7 7-7 7M5 5l7 7-7 7" />
          </svg>
          Hide Stats
        </>
      ) : (
        <>
          <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M11 19l-7-7 7-7m8 14l-7-7 7-7" />
          </svg>
          Show Stats
        </>
      )}
    </button>
    
    <button
      onClick={handleClearContent}
      className="px-3 py-1.5 text-sm text-gray-600 hover:text-gray-800 
               hover:bg-gray-200 rounded-lg transition-colors"
    >
      Clear
    </button>
  </div>

  <div className="flex items-center gap-2">
    {/* Quick Copy Button */}
    <CopyButton 
      htmlContent={currentHtml} 
      variant="compact"
    />
    
    {/* Full Export Modal Button */}
    <button
      onClick={() => setIsExportModalOpen(true)}
      className="flex items-center gap-2 px-4 py-2 text-sm font-semibold 
               bg-gradient-to-r from-indigo-600 to-purple-600 text-white 
               hover:from-indigo-700 hover:to-purple-700 rounded-lg 
               shadow-lg shadow-indigo-500/25 transition-all duration-200
               hover:shadow-xl hover:-translate-y-0.5"
    >
      <svg className="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24">
        <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M10 20l4-16m4 4l4 4-4 4M6 16l-4-4 4-4" />
      </svg>
      Generate Code
    </button>
  </div>
</div>
```

---

## Sample Clipboard Output

When user clicks "Copy Everything", they get:

```html
<!-- ============================================
   BLOGGER POST CONTENT
   Generated: 2024-01-15T10:30:00.000Z
   ============================================
   
   STEP 1: Add Widget.js (One-time setup)
   ----------------------------------------
   Add this script to your Blogger Layout:
   Go to Theme → Edit HTML → Before </body>
   
<script src="https://cdn.jsdelivr.net/gh/oathanrex/script@v1.0.0/widget.js"></script>
   
   STEP 2: Paste Content Below Into Your Post
   ----------------------------------------
============================================ -->

<article class="post-body">
<!-- Table of Contents - Auto-generated by widget.js -->
<div id="my-tool-toc" class="toc-container"></div>

<h2 id="getting-started-0" class="heading-2">Getting Started</h2>
<p>Welcome to our <strong>amazing</strong> product...</p>

<!-- ... rest of HTML content ... -->

</article>

<!-- End of generated content -->
```

---

## Features Summary

| Copy Option | What's Included |
|-------------|-----------------|
| **Copy Everything** | Script tag + Instructions + HTML content |
| **Script Tag Only** | CDN script tag with installation guide |
| **HTML Content Only** | Clean HTML for blog post |

| Component | Features |
|-----------|----------|
| **CopyButton** | 3 variants (compact, default dropdown, full card) |
| **CopyInstructions** | Expandable step-by-step Blogger guide |
| **ExportModal** | Full export interface with preview |

All copy operations include clear, formatted instructions! 📋
