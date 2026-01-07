# Notion-Style WYSIWYG Editor with Next.js, Tailwind CSS & Tiptap

## Project Setup

### Step 1: Create Next.js Project with Tailwind CSS

```bash
# Create new Next.js project
npx create-next-app@latest notion-editor --typescript --tailwind --eslint --app --src-dir --import-alias "@/*"

# Navigate to project
cd notion-editor

# Install Tiptap dependencies
npm install @tiptap/react @tiptap/pm @tiptap/starter-kit @tiptap/extension-placeholder @tiptap/extension-heading @tiptap/extension-bullet-list @tiptap/extension-ordered-list @tiptap/extension-list-item @tiptap/extension-bold @tiptap/extension-italic @tiptap/extension-strike @tiptap/extension-code @tiptap/extension-blockquote @tiptap/extension-horizontal-rule @tiptap/extension-hard-break

# Install icons
npm install lucide-react
```

### Step 2: Project Structure

```
notion-editor/
├── src/
│   ├── app/
│   │   ├── globals.css
│   │   ├── layout.tsx
│   │   └── page.tsx
│   ├── components/
│   │   ├── editor/
│   │   │   ├── Editor.tsx
│   │   │   ├── MenuBar.tsx
│   │   │   ├── EditorBubbleMenu.tsx
│   │   │   └── EditorContent.tsx
│   │   └── ui/
│   │       └── Button.tsx
│   ├── hooks/
│   │   └── useEditor.ts
│   └── styles/
│       └── editor.css
├── tailwind.config.ts
└── package.json
```

---

## Complete Source Code

### `tailwind.config.ts`

```typescript
import type { Config } from "tailwindcss";

const config: Config = {
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        notion: {
          default: '#37352F',
          gray: '#787774',
          brown: '#9F6B53',
          orange: '#D9730D',
          yellow: '#CB912F',
          green: '#448361',
          blue: '#337EA9',
          purple: '#9065B0',
          pink: '#C14C8A',
          red: '#D44C47',
          bg: {
            default: '#FFFFFF',
            hover: '#F7F6F3',
            gray: '#F1F1EF',
            selected: '#E8E7E4',
          },
          border: '#E9E9E7',
        },
      },
      fontFamily: {
        sans: [
          'ui-sans-serif',
          '-apple-system',
          'BlinkMacSystemFont',
          '"Segoe UI"',
          'Helvetica',
          '"Apple Color Emoji"',
          'Arial',
          'sans-serif',
          '"Segoe UI Emoji"',
          '"Segoe UI Symbol"',
        ],
      },
      typography: {
        DEFAULT: {
          css: {
            maxWidth: 'none',
            color: '#37352F',
            lineHeight: '1.5',
          },
        },
      },
    },
  },
  plugins: [],
};

export default config;
```

### `src/app/globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Notion-style Editor Styles */
@layer components {
  /* Editor container */
  .notion-editor {
    @apply w-full min-h-[500px] outline-none;
  }

  /* Placeholder styling */
  .tiptap p.is-editor-empty:first-child::before {
    @apply text-notion-gray pointer-events-none float-left h-0;
    content: attr(data-placeholder);
  }

  .tiptap .is-empty::before {
    @apply text-notion-gray pointer-events-none float-left h-0;
    content: attr(data-placeholder);
  }

  /* Basic text styles */
  .tiptap {
    @apply outline-none;
  }

  .tiptap > * + * {
    @apply mt-1;
  }

  /* Headings */
  .tiptap h1 {
    @apply text-4xl font-bold mt-8 mb-1 text-notion-default;
  }

  .tiptap h2 {
    @apply text-2xl font-semibold mt-6 mb-1 text-notion-default;
  }

  .tiptap h3 {
    @apply text-xl font-semibold mt-4 mb-1 text-notion-default;
  }

  /* Paragraphs */
  .tiptap p {
    @apply text-base leading-relaxed text-notion-default my-1;
  }

  /* Bold, Italic, Strike */
  .tiptap strong {
    @apply font-bold;
  }

  .tiptap em {
    @apply italic;
  }

  .tiptap s {
    @apply line-through;
  }

  /* Code */
  .tiptap code {
    @apply bg-notion-bg-gray text-notion-red px-1 py-0.5 rounded text-sm font-mono;
  }

  .tiptap pre {
    @apply bg-notion-bg-gray p-4 rounded-md my-4 overflow-x-auto;
  }

  .tiptap pre code {
    @apply bg-transparent p-0 text-notion-default;
  }

  /* Blockquote */
  .tiptap blockquote {
    @apply border-l-4 border-notion-default pl-4 my-4 text-notion-gray italic;
  }

  /* Lists */
  .tiptap ul {
    @apply list-disc list-outside ml-6 my-2;
  }

  .tiptap ol {
    @apply list-decimal list-outside ml-6 my-2;
  }

  .tiptap li {
    @apply my-1;
  }

  .tiptap li > p {
    @apply my-0;
  }

  .tiptap li::marker {
    @apply text-notion-default;
  }

  /* Horizontal Rule */
  .tiptap hr {
    @apply border-t border-notion-border my-6;
  }

  /* Selection */
  .tiptap ::selection {
    @apply bg-blue-200;
  }

  /* Focus state for blocks */
  .tiptap .ProseMirror-selectednode {
    @apply outline-2 outline-blue-500 outline-offset-2;
  }
}

/* Menu Bar Styles */
@layer components {
  .menu-button {
    @apply p-2 rounded hover:bg-notion-bg-hover text-notion-default 
           transition-colors duration-150 flex items-center justify-center;
  }

  .menu-button.is-active {
    @apply bg-notion-bg-selected text-notion-blue;
  }

  .menu-button:disabled {
    @apply opacity-40 cursor-not-allowed;
  }

  .menu-divider {
    @apply w-px h-6 bg-notion-border mx-1;
  }
}

/* Bubble Menu Styles */
@layer components {
  .bubble-menu {
    @apply flex items-center gap-1 p-1 bg-white rounded-lg shadow-lg 
           border border-notion-border;
  }

  .bubble-button {
    @apply p-1.5 rounded hover:bg-notion-bg-hover text-notion-default 
           transition-colors duration-150;
  }

  .bubble-button.is-active {
    @apply bg-notion-bg-selected text-notion-blue;
  }
}

/* Scrollbar styling */
@layer utilities {
  .scrollbar-thin::-webkit-scrollbar {
    @apply w-2;
  }

  .scrollbar-thin::-webkit-scrollbar-track {
    @apply bg-transparent;
  }

  .scrollbar-thin::-webkit-scrollbar-thumb {
    @apply bg-notion-border rounded-full;
  }

  .scrollbar-thin::-webkit-scrollbar-thumb:hover {
    @apply bg-notion-gray;
  }
}
```

### `src/components/ui/Button.tsx`

```tsx
import React from 'react';
import { cn } from '@/lib/utils';

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  isActive?: boolean;
  tooltip?: string;
  children: React.ReactNode;
}

export const Button: React.FC<ButtonProps> = ({
  isActive = false,
  tooltip,
  children,
  className,
  ...props
}) => {
  return (
    <button
      className={cn(
        'menu-button',
        isActive && 'is-active',
        className
      )}
      title={tooltip}
      {...props}
    >
      {children}
    </button>
  );
};

export default Button;
```

### `src/lib/utils.ts`

```typescript
import { type ClassValue, clsx } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

> **Note:** Install clsx and tailwind-merge:
> ```bash
> npm install clsx tailwind-merge
> ```

### `src/components/editor/MenuBar.tsx`

```tsx
'use client';

import React from 'react';
import { Editor } from '@tiptap/react';
import {
  Bold,
  Italic,
  Strikethrough,
  Code,
  Heading2,
  Heading3,
  List,
  ListOrdered,
  Quote,
  Minus,
  Undo,
  Redo,
  Type,
} from 'lucide-react';
import Button from '@/components/ui/Button';

interface MenuBarProps {
  editor: Editor | null;
}

export const MenuBar: React.FC<MenuBarProps> = ({ editor }) => {
  if (!editor) {
    return null;
  }

  const menuItems = [
    {
      group: 'text',
      items: [
        {
          icon: Type,
          title: 'Paragraph',
          action: () => editor.chain().focus().setParagraph().run(),
          isActive: () => editor.isActive('paragraph'),
        },
        {
          icon: Heading2,
          title: 'Heading 2',
          action: () => editor.chain().focus().toggleHeading({ level: 2 }).run(),
          isActive: () => editor.isActive('heading', { level: 2 }),
        },
        {
          icon: Heading3,
          title: 'Heading 3',
          action: () => editor.chain().focus().toggleHeading({ level: 3 }).run(),
          isActive: () => editor.isActive('heading', { level: 3 }),
        },
      ],
    },
    {
      group: 'formatting',
      items: [
        {
          icon: Bold,
          title: 'Bold (Ctrl+B)',
          action: () => editor.chain().focus().toggleBold().run(),
          isActive: () => editor.isActive('bold'),
        },
        {
          icon: Italic,
          title: 'Italic (Ctrl+I)',
          action: () => editor.chain().focus().toggleItalic().run(),
          isActive: () => editor.isActive('italic'),
        },
        {
          icon: Strikethrough,
          title: 'Strikethrough',
          action: () => editor.chain().focus().toggleStrike().run(),
          isActive: () => editor.isActive('strike'),
        },
        {
          icon: Code,
          title: 'Inline Code',
          action: () => editor.chain().focus().toggleCode().run(),
          isActive: () => editor.isActive('code'),
        },
      ],
    },
    {
      group: 'lists',
      items: [
        {
          icon: List,
          title: 'Bullet List',
          action: () => editor.chain().focus().toggleBulletList().run(),
          isActive: () => editor.isActive('bulletList'),
        },
        {
          icon: ListOrdered,
          title: 'Numbered List',
          action: () => editor.chain().focus().toggleOrderedList().run(),
          isActive: () => editor.isActive('orderedList'),
        },
      ],
    },
    {
      group: 'blocks',
      items: [
        {
          icon: Quote,
          title: 'Blockquote',
          action: () => editor.chain().focus().toggleBlockquote().run(),
          isActive: () => editor.isActive('blockquote'),
        },
        {
          icon: Minus,
          title: 'Horizontal Rule',
          action: () => editor.chain().focus().setHorizontalRule().run(),
          isActive: () => false,
        },
      ],
    },
    {
      group: 'history',
      items: [
        {
          icon: Undo,
          title: 'Undo (Ctrl+Z)',
          action: () => editor.chain().focus().undo().run(),
          isActive: () => false,
          disabled: () => !editor.can().undo(),
        },
        {
          icon: Redo,
          title: 'Redo (Ctrl+Y)',
          action: () => editor.chain().focus().redo().run(),
          isActive: () => false,
          disabled: () => !editor.can().redo(),
        },
      ],
    },
  ];

  return (
    <div className="sticky top-0 z-10 bg-white border-b border-notion-border">
      <div className="flex flex-wrap items-center gap-1 p-2">
        {menuItems.map((group, groupIndex) => (
          <React.Fragment key={group.group}>
            {groupIndex > 0 && <div className="menu-divider" />}
            {group.items.map((item) => (
              <Button
                key={item.title}
                onClick={item.action}
                isActive={item.isActive()}
                disabled={item.disabled?.()}
                tooltip={item.title}
              >
                <item.icon className="w-4 h-4" />
              </Button>
            ))}
          </React.Fragment>
        ))}
      </div>
    </div>
  );
};

export default MenuBar;
```

### `src/components/editor/EditorBubbleMenu.tsx`

```tsx
'use client';

import React from 'react';
import { BubbleMenu, Editor } from '@tiptap/react';
import {
  Bold,
  Italic,
  Strikethrough,
  Code,
  Heading2,
  Heading3,
} from 'lucide-react';

interface EditorBubbleMenuProps {
  editor: Editor;
}

export const EditorBubbleMenu: React.FC<EditorBubbleMenuProps> = ({ editor }) => {
  const bubbleMenuItems = [
    {
      icon: Bold,
      title: 'Bold',
      action: () => editor.chain().focus().toggleBold().run(),
      isActive: () => editor.isActive('bold'),
    },
    {
      icon: Italic,
      title: 'Italic',
      action: () => editor.chain().focus().toggleItalic().run(),
      isActive: () => editor.isActive('italic'),
    },
    {
      icon: Strikethrough,
      title: 'Strikethrough',
      action: () => editor.chain().focus().toggleStrike().run(),
      isActive: () => editor.isActive('strike'),
    },
    {
      icon: Code,
      title: 'Code',
      action: () => editor.chain().focus().toggleCode().run(),
      isActive: () => editor.isActive('code'),
    },
    { type: 'divider' },
    {
      icon: Heading2,
      title: 'Heading 2',
      action: () => editor.chain().focus().toggleHeading({ level: 2 }).run(),
      isActive: () => editor.isActive('heading', { level: 2 }),
    },
    {
      icon: Heading3,
      title: 'Heading 3',
      action: () => editor.chain().focus().toggleHeading({ level: 3 }).run(),
      isActive: () => editor.isActive('heading', { level: 3 }),
    },
  ];

  return (
    <BubbleMenu
      editor={editor}
      tippyOptions={{ duration: 100 }}
      className="bubble-menu"
    >
      {bubbleMenuItems.map((item, index) => {
        if (item.type === 'divider') {
          return <div key={index} className="w-px h-5 bg-notion-border mx-1" />;
        }

        const Icon = item.icon!;
        return (
          <button
            key={item.title}
            onClick={item.action}
            className={`bubble-button ${item.isActive?.() ? 'is-active' : ''}`}
            title={item.title}
          >
            <Icon className="w-4 h-4" />
          </button>
        );
      })}
    </BubbleMenu>
  );
};

export default EditorBubbleMenu;
```

### `src/components/editor/Editor.tsx`

```tsx
'use client';

import React, { useCallback } from 'react';
import { useEditor, EditorContent } from '@tiptap/react';
import StarterKit from '@tiptap/starter-kit';
import Placeholder from '@tiptap/extension-placeholder';
import MenuBar from './MenuBar';
import EditorBubbleMenu from './EditorBubbleMenu';

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
    // Prevent SSR hydration issues
    immediatelyRender: false,
  });

  // Keyboard shortcuts info
  const keyboardShortcuts = [
    { keys: 'Ctrl/Cmd + B', action: 'Bold' },
    { keys: 'Ctrl/Cmd + I', action: 'Italic' },
    { keys: 'Ctrl/Cmd + Shift + S', action: 'Strikethrough' },
    { keys: 'Ctrl/Cmd + E', action: 'Code' },
    { keys: 'Ctrl/Cmd + Shift + 8', action: 'Bullet List' },
    { keys: 'Ctrl/Cmd + Shift + 7', action: 'Numbered List' },
  ];

  const handleClearContent = useCallback(() => {
    if (editor) {
      editor.commands.clearContent();
      editor.commands.focus();
    }
  }, [editor]);

  const handleExportHTML = useCallback(() => {
    if (editor) {
      const html = editor.getHTML();
      console.log('Exported HTML:', html);
      // Copy to clipboard
      navigator.clipboard.writeText(html);
      alert('HTML copied to clipboard!');
    }
  }, [editor]);

  const handleExportJSON = useCallback(() => {
    if (editor) {
      const json = editor.getJSON();
      console.log('Exported JSON:', json);
      // Copy to clipboard
      navigator.clipboard.writeText(JSON.stringify(json, null, 2));
      alert('JSON copied to clipboard!');
    }
  }, [editor]);

  return (
    <div className="w-full max-w-4xl mx-auto">
      {/* Editor Container */}
      <div className="bg-white rounded-lg border border-notion-border shadow-sm overflow-hidden">
        {/* Menu Bar */}
        <MenuBar editor={editor} />

        {/* Editor Content */}
        <div className="p-8 min-h-[500px]">
          {editor && <EditorBubbleMenu editor={editor} />}
          <EditorContent editor={editor} />
        </div>

        {/* Footer with actions */}
        <div className="border-t border-notion-border px-4 py-3 bg-notion-bg-gray flex items-center justify-between">
          <div className="text-sm text-notion-gray">
            {editor && (
              <span>
                {editor.storage.characterCount?.characters?.() || 0} characters
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
              onClick={handleExportHTML}
              className="px-3 py-1.5 text-sm text-notion-gray hover:text-notion-default 
                         hover:bg-notion-bg-hover rounded transition-colors"
            >
              Export HTML
            </button>
            <button
              onClick={handleExportJSON}
              className="px-3 py-1.5 text-sm bg-notion-blue text-white 
                         hover:bg-blue-600 rounded transition-colors"
            >
              Export JSON
            </button>
          </div>
        </div>
      </div>

      {/* Keyboard Shortcuts Help */}
      <div className="mt-6 p-4 bg-notion-bg-gray rounded-lg">
        <h3 className="text-sm font-semibold text-notion-default mb-3">
          Keyboard Shortcuts
        </h3>
        <div className="grid grid-cols-2 md:grid-cols-3 gap-2">
          {keyboardShortcuts.map((shortcut) => (
            <div key={shortcut.action} className="text-sm">
              <kbd className="px-2 py-1 bg-white rounded border border-notion-border text-xs mr-2">
                {shortcut.keys}
              </kbd>
              <span className="text-notion-gray">{shortcut.action}</span>
            </div>
          ))}
        </div>
      </div>
    </div>
  );
};

export default NotionEditor;
```

### `src/app/layout.tsx`

```tsx
import type { Metadata } from "next";
import "./globals.css";

export const metadata: Metadata = {
  title: "Notion-Style Editor | Next.js + Tiptap",
  description: "A beautiful Notion-style WYSIWYG editor built with Next.js, Tailwind CSS, and Tiptap",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="en">
      <body className="antialiased bg-notion-bg-gray min-h-screen">
        {children}
      </body>
    </html>
  );
}
```

### `src/app/page.tsx`

```tsx
'use client';

import { useState } from 'react';
import NotionEditor from '@/components/editor/Editor';

export default function Home() {
  const [output, setOutput] = useState<{ html: string; json: object } | null>(null);

  const handleEditorChange = (html: string, json: object) => {
    setOutput({ html, json });
    // You can save to database here
    console.log('Content updated:', { html, json });
  };

  const sampleContent = `
    <h2>Welcome to the Notion-Style Editor</h2>
    <p>This is a <strong>beautiful</strong> and <em>powerful</em> WYSIWYG editor built with:</p>
    <ul>
      <li>Next.js 14 with App Router</li>
      <li>Tailwind CSS for styling</li>
      <li>Tiptap for rich text editing</li>
    </ul>
    <h3>Features</h3>
    <p>Try out these formatting options:</p>
    <ol>
      <li><strong>Bold</strong> text with Ctrl+B</li>
      <li><em>Italic</em> text with Ctrl+I</li>
      <li><s>Strikethrough</s> for deleted text</li>
      <li><code>Inline code</code> for code snippets</li>
    </ol>
    <blockquote>
      <p>This is a blockquote. Perfect for highlighting important information!</p>
    </blockquote>
    <hr>
    <p>Start typing below to create your own content...</p>
  `;

  return (
    <main className="min-h-screen py-8 px-4">
      {/* Header */}
      <div className="max-w-4xl mx-auto mb-8">
        <div className="text-center">
          <h1 className="text-3xl font-bold text-notion-default mb-2">
            📝 Notion-Style Editor
          </h1>
          <p className="text-notion-gray">
            A beautiful WYSIWYG editor built with Next.js, Tailwind CSS, and Tiptap
          </p>
        </div>
      </div>

      {/* Editor */}
      <NotionEditor
        initialContent={sampleContent}
        onChange={handleEditorChange}
        placeholder="Start typing here... Use '/' for commands"
      />

      {/* Debug Output (optional - remove in production) */}
      {output && (
        <div className="max-w-4xl mx-auto mt-8 p-4 bg-white rounded-lg border border-notion-border">
          <h3 className="text-sm font-semibold text-notion-default mb-2">
            Live Output Preview
          </h3>
          <div className="grid md:grid-cols-2 gap-4">
            <div>
              <h4 className="text-xs font-medium text-notion-gray mb-1">HTML</h4>
              <pre className="p-3 bg-notion-bg-gray rounded text-xs overflow-auto max-h-48 scrollbar-thin">
                {output.html}
              </pre>
            </div>
            <div>
              <h4 className="text-xs font-medium text-notion-gray mb-1">JSON</h4>
              <pre className="p-3 bg-notion-bg-gray rounded text-xs overflow-auto max-h-48 scrollbar-thin">
                {JSON.stringify(output.json, null, 2)}
              </pre>
            </div>
          </div>
        </div>
      )}
    </main>
  );
}
```

---

## Additional Extensions (Optional)

### `src/components/editor/SlashCommands.tsx` (Bonus: Slash Commands)

```tsx
'use client';

import React, { useState, useEffect, useCallback } from 'react';
import { Extension } from '@tiptap/core';
import { ReactRenderer } from '@tiptap/react';
import Suggestion from '@tiptap/suggestion';
import tippy, { Instance as TippyInstance } from 'tippy.js';
import {
  Heading2,
  Heading3,
  List,
  ListOrdered,
  Quote,
  Minus,
  Type,
  Code,
} from 'lucide-react';

interface CommandItem {
  title: string;
  description: string;
  icon: React.ComponentType<{ className?: string }>;
  command: ({ editor, range }: { editor: any; range: any }) => void;
}

const getSuggestionItems = (): CommandItem[] => [
  {
    title: 'Text',
    description: 'Just start writing with plain text.',
    icon: Type,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).setParagraph().run();
    },
  },
  {
    title: 'Heading 2',
    description: 'Large section heading.',
    icon: Heading2,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).setHeading({ level: 2 }).run();
    },
  },
  {
    title: 'Heading 3',
    description: 'Medium section heading.',
    icon: Heading3,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).setHeading({ level: 3 }).run();
    },
  },
  {
    title: 'Bullet List',
    description: 'Create a simple bullet list.',
    icon: List,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).toggleBulletList().run();
    },
  },
  {
    title: 'Numbered List',
    description: 'Create a numbered list.',
    icon: ListOrdered,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).toggleOrderedList().run();
    },
  },
  {
    title: 'Quote',
    description: 'Capture a quote.',
    icon: Quote,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).toggleBlockquote().run();
    },
  },
  {
    title: 'Code Block',
    description: 'Capture a code snippet.',
    icon: Code,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).toggleCodeBlock().run();
    },
  },
  {
    title: 'Divider',
    description: 'Add a horizontal divider.',
    icon: Minus,
    command: ({ editor, range }) => {
      editor.chain().focus().deleteRange(range).setHorizontalRule().run();
    },
  },
];

interface CommandListProps {
  items: CommandItem[];
  command: (item: CommandItem) => void;
}

const CommandList: React.FC<CommandListProps> = ({ items, command }) => {
  const [selectedIndex, setSelectedIndex] = useState(0);

  useEffect(() => {
    setSelectedIndex(0);
  }, [items]);

  const selectItem = useCallback(
    (index: number) => {
      const item = items[index];
      if (item) {
        command(item);
      }
    },
    [items, command]
  );

  useEffect(() => {
    const handleKeyDown = (e: KeyboardEvent) => {
      if (e.key === 'ArrowUp') {
        e.preventDefault();
        setSelectedIndex((prev) => (prev - 1 + items.length) % items.length);
      } else if (e.key === 'ArrowDown') {
        e.preventDefault();
        setSelectedIndex((prev) => (prev + 1) % items.length);
      } else if (e.key === 'Enter') {
        e.preventDefault();
        selectItem(selectedIndex);
      }
    };

    document.addEventListener('keydown', handleKeyDown);
    return () => document.removeEventListener('keydown', handleKeyDown);
  }, [items, selectedIndex, selectItem]);

  if (items.length === 0) {
    return (
      <div className="p-2 text-sm text-notion-gray">No results found</div>
    );
  }

  return (
    <div className="bg-white rounded-lg shadow-lg border border-notion-border overflow-hidden w-72">
      <div className="p-2 text-xs text-notion-gray font-medium uppercase tracking-wider border-b border-notion-border">
        Basic Blocks
      </div>
      <div className="max-h-64 overflow-y-auto">
        {items.map((item, index) => {
          const Icon = item.icon;
          return (
            <button
              key={item.title}
              onClick={() => selectItem(index)}
              className={`w-full flex items-center gap-3 px-3 py-2 text-left hover:bg-notion-bg-hover transition-colors ${
                index === selectedIndex ? 'bg-notion-bg-selected' : ''
              }`}
            >
              <div className="p-1.5 bg-notion-bg-gray rounded">
                <Icon className="w-4 h-4 text-notion-default" />
              </div>
              <div>
                <div className="text-sm font-medium text-notion-default">
                  {item.title}
                </div>
                <div className="text-xs text-notion-gray">
                  {item.description}
                </div>
              </div>
            </button>
          );
        })}
      </div>
    </div>
  );
};

export const SlashCommands = Extension.create({
  name: 'slashCommands',

  addOptions() {
    return {
      suggestion: {
        char: '/',
        command: ({ editor, range, props }: any) => {
          props.command({ editor, range });
        },
      },
    };
  },

  addProseMirrorPlugins() {
    return [
      Suggestion({
        editor: this.editor,
        ...this.options.suggestion,
        items: ({ query }: { query: string }) => {
          return getSuggestionItems().filter((item) =>
            item.title.toLowerCase().startsWith(query.toLowerCase())
          );
        },
        render: () => {
          let component: ReactRenderer;
          let popup: TippyInstance[];

          return {
            onStart: (props: any) => {
              component = new ReactRenderer(CommandList, {
                props,
                editor: props.editor,
              });

              popup = tippy('body', {
                getReferenceClientRect: props.clientRect,
                appendTo: () => document.body,
                content: component.element,
                showOnCreate: true,
                interactive: true,
                trigger: 'manual',
                placement: 'bottom-start',
              });
            },
            onUpdate: (props: any) => {
              component.updateProps(props);
              popup[0].setProps({
                getReferenceClientRect: props.clientRect,
              });
            },
            onKeyDown: (props: any) => {
              if (props.event.key === 'Escape') {
                popup[0].hide();
                return true;
              }
              return false;
            },
            onExit: () => {
              popup[0].destroy();
              component.destroy();
            },
          };
        },
      }),
    ];
  },
});
```

> **Note:** For slash commands, install tippy.js:
> ```bash
> npm install tippy.js @tiptap/suggestion
> ```

---

## Running the Project

```bash
# Development
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

---

## Features Summary

| Feature | Status | Description |
|---------|--------|-------------|
| **Bold** | ✅ | Ctrl+B |
| **Italic** | ✅ | Ctrl+I |
| **Strikethrough** | ✅ | Ctrl+Shift+S |
| **Inline Code** | ✅ | Ctrl+E |
| **Heading 2** | ✅ | Via menu/bubble |
| **Heading 3** | ✅ | Via menu/bubble |
| **Bullet List** | ✅ | Ctrl+Shift+8 |
| **Numbered List** | ✅ | Ctrl+Shift+7 |
| **Blockquote** | ✅ | Via menu |
| **Horizontal Rule** | ✅ | Via menu |
| **Undo/Redo** | ✅ | Ctrl+Z / Ctrl+Y |
| **Bubble Menu** | ✅ | Select text |
| **Placeholder** | ✅ | Context-aware |
| **Export HTML/JSON** | ✅ | Via buttons |

This creates a clean, performant, Notion-style editor ready for production use! 🚀
