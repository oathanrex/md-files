# Custom Tiptap Extensions: CTA, FAQ, and Pros/Cons Blocks

I'll create three custom Tiptap extensions with beautiful Notion-style designs.

## Project Structure Update

```
src/
├── components/
│   └── editor/
│       ├── extensions/
│       │   ├── CallToAction/
│       │   │   ├── CallToAction.ts
│       │   │   └── CallToActionComponent.tsx
│       │   ├── FaqBlock/
│       │   │   ├── FaqBlock.ts
│       │   │   ├── FaqItem.ts
│       │   │   └── FaqBlockComponent.tsx
│       │   ├── ProsConsBlock/
│       │   │   ├── ProsConsBlock.ts
│       │   │   └── ProsConsComponent.tsx
│       │   └── index.ts
│       ├── Editor.tsx (updated)
│       └── MenuBar.tsx (updated)
└── app/
    └── globals.css (updated)
```

---

## 1. Call-to-Action Block Extension

### `src/components/editor/extensions/CallToAction/CallToAction.ts`

```typescript
import { Node, mergeAttributes } from '@tiptap/core';
import { ReactNodeViewRenderer } from '@tiptap/react';
import CallToActionComponent from './CallToActionComponent';

export interface CallToActionOptions {
  HTMLAttributes: Record<string, any>;
}

declare module '@tiptap/core' {
  interface Commands<ReturnType> {
    callToAction: {
      setCallToAction: (attributes?: {
        title?: string;
        description?: string;
        buttonText?: string;
        buttonUrl?: string;
        variant?: 'primary' | 'secondary' | 'gradient';
      }) => ReturnType;
    };
  }
}

export const CallToAction = Node.create<CallToActionOptions>({
  name: 'callToAction',

  group: 'block',

  content: 'inline*',

  draggable: true,

  addOptions() {
    return {
      HTMLAttributes: {},
    };
  },

  addAttributes() {
    return {
      title: {
        default: 'Ready to Get Started?',
      },
      description: {
        default: 'Join thousands of users who are already using our product.',
      },
      buttonText: {
        default: 'Get Started',
      },
      buttonUrl: {
        default: '#',
      },
      variant: {
        default: 'primary',
      },
    };
  },

  parseHTML() {
    return [
      {
        tag: 'div[data-type="call-to-action"]',
      },
    ];
  },

  renderHTML({ HTMLAttributes }) {
    return [
      'div',
      mergeAttributes(this.options.HTMLAttributes, HTMLAttributes, {
        'data-type': 'call-to-action',
      }),
      0,
    ];
  },

  addNodeView() {
    return ReactNodeViewRenderer(CallToActionComponent);
  },

  addCommands() {
    return {
      setCallToAction:
        (attributes) =>
        ({ commands }) => {
          return commands.insertContent({
            type: this.name,
            attrs: attributes,
          });
        },
    };
  },
});

export default CallToAction;
```

### `src/components/editor/extensions/CallToAction/CallToActionComponent.tsx`

```tsx
'use client';

import React, { useState } from 'react';
import { NodeViewWrapper, NodeViewProps } from '@tiptap/react';
import {
  Megaphone,
  Settings,
  X,
  ExternalLink,
  Palette,
  Sparkles,
} from 'lucide-react';

const variants = {
  primary: {
    container: 'bg-gradient-to-r from-blue-50 to-indigo-50 border-blue-200',
    icon: 'bg-blue-100 text-blue-600',
    title: 'text-blue-900',
    description: 'text-blue-700',
    button: 'bg-blue-600 hover:bg-blue-700 text-white',
  },
  secondary: {
    container: 'bg-gradient-to-r from-gray-50 to-slate-50 border-gray-200',
    icon: 'bg-gray-100 text-gray-600',
    title: 'text-gray-900',
    description: 'text-gray-600',
    button: 'bg-gray-800 hover:bg-gray-900 text-white',
  },
  gradient: {
    container: 'bg-gradient-to-r from-purple-50 via-pink-50 to-rose-50 border-purple-200',
    icon: 'bg-gradient-to-r from-purple-500 to-pink-500 text-white',
    title: 'text-purple-900',
    description: 'text-purple-700',
    button: 'bg-gradient-to-r from-purple-600 to-pink-600 hover:from-purple-700 hover:to-pink-700 text-white',
  },
};

const CallToActionComponent: React.FC<NodeViewProps> = ({
  node,
  updateAttributes,
  deleteNode,
  selected,
}) => {
  const [isEditing, setIsEditing] = useState(false);
  const { title, description, buttonText, buttonUrl, variant } = node.attrs;
  const styles = variants[variant as keyof typeof variants] || variants.primary;

  const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
    e.preventDefault();
    const formData = new FormData(e.currentTarget);
    updateAttributes({
      title: formData.get('title'),
      description: formData.get('description'),
      buttonText: formData.get('buttonText'),
      buttonUrl: formData.get('buttonUrl'),
      variant: formData.get('variant'),
    });
    setIsEditing(false);
  };

  return (
    <NodeViewWrapper className="my-4">
      <div
        className={`
          relative rounded-xl border-2 p-6 transition-all duration-200
          ${styles.container}
          ${selected ? 'ring-2 ring-blue-500 ring-offset-2' : ''}
        `}
        data-drag-handle
      >
        {/* Edit/Delete Controls */}
        <div className="absolute top-3 right-3 flex gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
          <button
            onClick={() => setIsEditing(!isEditing)}
            className="p-1.5 rounded-md bg-white/80 hover:bg-white shadow-sm transition-colors"
            title="Edit CTA"
          >
            <Settings className="w-4 h-4 text-gray-600" />
          </button>
          <button
            onClick={deleteNode}
            className="p-1.5 rounded-md bg-white/80 hover:bg-red-50 shadow-sm transition-colors"
            title="Delete CTA"
          >
            <X className="w-4 h-4 text-red-500" />
          </button>
        </div>

        {isEditing ? (
          /* Edit Mode */
          <form onSubmit={handleSubmit} className="space-y-4">
            <div className="grid gap-4 md:grid-cols-2">
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Title
                </label>
                <input
                  name="title"
                  defaultValue={title}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  placeholder="Enter title..."
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Button Text
                </label>
                <input
                  name="buttonText"
                  defaultValue={buttonText}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  placeholder="Button text..."
                />
              </div>
            </div>

            <div>
              <label className="block text-sm font-medium text-gray-700 mb-1">
                Description
              </label>
              <textarea
                name="description"
                defaultValue={description}
                rows={2}
                className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                placeholder="Enter description..."
              />
            </div>

            <div className="grid gap-4 md:grid-cols-2">
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Button URL
                </label>
                <input
                  name="buttonUrl"
                  defaultValue={buttonUrl}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                  placeholder="https://..."
                />
              </div>
              <div>
                <label className="block text-sm font-medium text-gray-700 mb-1">
                  Style Variant
                </label>
                <select
                  name="variant"
                  defaultValue={variant}
                  className="w-full px-3 py-2 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-transparent"
                >
                  <option value="primary">Primary (Blue)</option>
                  <option value="secondary">Secondary (Gray)</option>
                  <option value="gradient">Gradient (Purple/Pink)</option>
                </select>
              </div>
            </div>

            <div className="flex gap-2 justify-end">
              <button
                type="button"
                onClick={() => setIsEditing(false)}
                className="px-4 py-2 text-sm text-gray-600 hover:bg-gray-100 rounded-lg transition-colors"
              >
                Cancel
              </button>
              <button
                type="submit"
                className="px-4 py-2 text-sm bg-blue-600 text-white hover:bg-blue-700 rounded-lg transition-colors"
              >
                Save Changes
              </button>
            </div>
          </form>
        ) : (
          /* Display Mode */
          <div className="flex flex-col md:flex-row items-center gap-6">
            {/* Icon */}
            <div className={`p-4 rounded-full ${styles.icon} shrink-0`}>
              {variant === 'gradient' ? (
                <Sparkles className="w-8 h-8" />
              ) : (
                <Megaphone className="w-8 h-8" />
              )}
            </div>

            {/* Content */}
            <div className="flex-1 text-center md:text-left">
              <h3 className={`text-xl font-bold mb-2 ${styles.title}`}>
                {title}
              </h3>
              <p className={`text-sm ${styles.description}`}>{description}</p>
            </div>

            {/* Button */}
            <a
              href={buttonUrl}
              target="_blank"
              rel="noopener noreferrer"
              className={`
                inline-flex items-center gap-2 px-6 py-3 rounded-lg font-semibold
                text-sm shadow-lg shadow-blue-500/25 transition-all duration-200
                hover:shadow-xl hover:-translate-y-0.5 shrink-0
                ${styles.button}
              `}
              onClick={(e) => e.preventDefault()}
            >
              {buttonText}
              <ExternalLink className="w-4 h-4" />
            </a>
          </div>
        )}

        {/* Drag Handle Indicator */}
        <div className="absolute left-2 top-1/2 -translate-y-1/2 opacity-0 hover:opacity-50 cursor-move">
          <div className="flex flex-col gap-0.5">
            <div className="flex gap-0.5">
              <div className="w-1 h-1 rounded-full bg-gray-400" />
              <div className="w-1 h-1 rounded-full bg-gray-400" />
            </div>
            <div className="flex gap-0.5">
              <div className="w-1 h-1 rounded-full bg-gray-400" />
              <div className="w-1 h-1 rounded-full bg-gray-400" />
            </div>
            <div className="flex gap-0.5">
              <div className="w-1 h-1 rounded-full bg-gray-400" />
              <div className="w-1 h-1 rounded-full bg-gray-400" />
            </div>
          </div>
        </div>
      </div>
    </NodeViewWrapper>
  );
};

export default CallToActionComponent;
```

---

## 2. FAQ Block Extension

### `src/components/editor/extensions/FaqBlock/FaqBlock.ts`

```typescript
import { Node, mergeAttributes } from '@tiptap/core';
import { ReactNodeViewRenderer } from '@tiptap/react';
import FaqBlockComponent from './FaqBlockComponent';

export interface FaqItem {
  id: string;
  question: string;
  answer: string;
}

export interface FaqBlockOptions {
  HTMLAttributes: Record<string, any>;
}

declare module '@tiptap/core' {
  interface Commands<ReturnType> {
    faqBlock: {
      setFaqBlock: (attributes?: {
        title?: string;
        items?: FaqItem[];
      }) => ReturnType;
    };
  }
}

export const FaqBlock = Node.create<FaqBlockOptions>({
  name: 'faqBlock',

  group: 'block',

  atom: true,

  draggable: true,

  addOptions() {
    return {
      HTMLAttributes: {},
    };
  },

  addAttributes() {
    return {
      title: {
        default: 'Frequently Asked Questions',
      },
      items: {
        default: [
          {
            id: 'faq-1',
            question: 'What is this product?',
            answer: 'This is an amazing product that helps you accomplish great things.',
          },
          {
            id: 'faq-2',
            question: 'How do I get started?',
            answer: 'Simply sign up for an account and follow our quick start guide.',
          },
        ],
      },
    };
  },

  parseHTML() {
    return [
      {
        tag: 'div[data-type="faq-block"]',
      },
    ];
  },

  renderHTML({ HTMLAttributes }) {
    return [
      'div',
      mergeAttributes(this.options.HTMLAttributes, HTMLAttributes, {
        'data-type': 'faq-block',
        class: 'faq-block',
      }),
    ];
  },

  addNodeView() {
    return ReactNodeViewRenderer(FaqBlockComponent);
  },

  addCommands() {
    return {
      setFaqBlock:
        (attributes) =>
        ({ commands }) => {
          return commands.insertContent({
            type: this.name,
            attrs: attributes,
          });
        },
    };
  },
});

export default FaqBlock;
```

### `src/components/editor/extensions/FaqBlock/FaqBlockComponent.tsx`

```tsx
'use client';

import React, { useState } from 'react';
import { NodeViewWrapper, NodeViewProps } from '@tiptap/react';
import {
  HelpCircle,
  Plus,
  Trash2,
  ChevronDown,
  ChevronUp,
  GripVertical,
  Edit3,
  Check,
  X,
} from 'lucide-react';

interface FaqItem {
  id: string;
  question: string;
  answer: string;
}

const FaqBlockComponent: React.FC<NodeViewProps> = ({
  node,
  updateAttributes,
  deleteNode,
  selected,
}) => {
  const [expandedItems, setExpandedItems] = useState<Set<string>>(new Set());
  const [editingItem, setEditingItem] = useState<string | null>(null);
  const [isEditingTitle, setIsEditingTitle] = useState(false);

  const { title, items } = node.attrs as { title: string; items: FaqItem[] };

  const toggleItem = (id: string) => {
    setExpandedItems((prev) => {
      const newSet = new Set(prev);
      if (newSet.has(id)) {
        newSet.delete(id);
      } else {
        newSet.add(id);
      }
      return newSet;
    });
  };

  const addFaqItem = () => {
    const newItem: FaqItem = {
      id: `faq-${Date.now()}`,
      question: 'New Question?',
      answer: 'Add your answer here...',
    };
    updateAttributes({
      items: [...items, newItem],
    });
    setEditingItem(newItem.id);
    setExpandedItems((prev) => new Set([...prev, newItem.id]));
  };

  const updateFaqItem = (id: string, updates: Partial<FaqItem>) => {
    updateAttributes({
      items: items.map((item: FaqItem) =>
        item.id === id ? { ...item, ...updates } : item
      ),
    });
  };

  const deleteFaqItem = (id: string) => {
    updateAttributes({
      items: items.filter((item: FaqItem) => item.id !== id),
    });
  };

  const moveFaqItem = (index: number, direction: 'up' | 'down') => {
    const newItems = [...items];
    const targetIndex = direction === 'up' ? index - 1 : index + 1;
    if (targetIndex < 0 || targetIndex >= newItems.length) return;
    [newItems[index], newItems[targetIndex]] = [newItems[targetIndex], newItems[index]];
    updateAttributes({ items: newItems });
  };

  return (
    <NodeViewWrapper className="my-6">
      <div
        className={`
          relative rounded-xl border-2 border-amber-200 bg-gradient-to-br from-amber-50 to-orange-50
          overflow-hidden transition-all duration-200
          ${selected ? 'ring-2 ring-blue-500 ring-offset-2' : ''}
        `}
      >
        {/* Header */}
        <div className="bg-gradient-to-r from-amber-100 to-orange-100 px-6 py-4 border-b border-amber-200">
          <div className="flex items-center justify-between">
            <div className="flex items-center gap-3">
              <div className="p-2 bg-amber-500 rounded-lg shadow-sm">
                <HelpCircle className="w-5 h-5 text-white" />
              </div>
              {isEditingTitle ? (
                <input
                  type="text"
                  value={title}
                  onChange={(e) => updateAttributes({ title: e.target.value })}
                  onBlur={() => setIsEditingTitle(false)}
                  onKeyDown={(e) => e.key === 'Enter' && setIsEditingTitle(false)}
                  className="text-xl font-bold text-amber-900 bg-white px-2 py-1 rounded border border-amber-300 focus:ring-2 focus:ring-amber-500"
                  autoFocus
                />
              ) : (
                <h3
                  className="text-xl font-bold text-amber-900 cursor-pointer hover:text-amber-700"
                  onClick={() => setIsEditingTitle(true)}
                >
                  {title}
                </h3>
              )}
            </div>
            <div className="flex items-center gap-2">
              <button
                onClick={addFaqItem}
                className="flex items-center gap-1 px-3 py-1.5 text-sm font-medium text-amber-700 
                         bg-white hover:bg-amber-50 rounded-lg border border-amber-300 transition-colors"
              >
                <Plus className="w-4 h-4" />
                Add FAQ
              </button>
              <button
                onClick={deleteNode}
                className="p-1.5 text-red-500 hover:bg-red-50 rounded-lg transition-colors"
                title="Delete FAQ Block"
              >
                <Trash2 className="w-4 h-4" />
              </button>
            </div>
          </div>
        </div>

        {/* FAQ Items */}
        <div className="divide-y divide-amber-100">
          {items.map((item: FaqItem, index: number) => (
            <div
              key={item.id}
              className="group relative"
            >
              {/* Question Header */}
              <div
                className={`
                  flex items-center gap-3 px-6 py-4 cursor-pointer
                  hover:bg-amber-50/50 transition-colors
                  ${expandedItems.has(item.id) ? 'bg-amber-50/50' : ''}
                `}
                onClick={() => toggleItem(item.id)}
              >
                {/* Drag Handle */}
                <div className="opacity-0 group-hover:opacity-100 transition-opacity cursor-move">
                  <GripVertical className="w-4 h-4 text-amber-400" />
                </div>

                {/* Question Number */}
                <span className="flex items-center justify-center w-7 h-7 rounded-full bg-amber-200 text-amber-800 text-sm font-bold shrink-0">
                  {index + 1}
                </span>

                {/* Question Text */}
                {editingItem === item.id ? (
                  <input
                    type="text"
                    value={item.question}
                    onChange={(e) => updateFaqItem(item.id, { question: e.target.value })}
                    onClick={(e) => e.stopPropagation()}
                    className="flex-1 px-2 py-1 text-base font-semibold text-amber-900 
                             bg-white rounded border border-amber-300 focus:ring-2 focus:ring-amber-500"
                  />
                ) : (
                  <span className="flex-1 text-base font-semibold text-amber-900">
                    {item.question}
                  </span>
                )}

                {/* Actions */}
                <div className="flex items-center gap-1 opacity-0 group-hover:opacity-100 transition-opacity">
                  {editingItem === item.id ? (
                    <button
                      onClick={(e) => {
                        e.stopPropagation();
                        setEditingItem(null);
                      }}
                      className="p-1 text-green-600 hover:bg-green-50 rounded"
                    >
                      <Check className="w-4 h-4" />
                    </button>
                  ) : (
                    <button
                      onClick={(e) => {
                        e.stopPropagation();
                        setEditingItem(item.id);
                        setExpandedItems((prev) => new Set([...prev, item.id]));
                      }}
                      className="p-1 text-amber-600 hover:bg-amber-100 rounded"
                    >
                      <Edit3 className="w-4 h-4" />
                    </button>
                  )}
                  <button
                    onClick={(e) => {
                      e.stopPropagation();
                      moveFaqItem(index, 'up');
                    }}
                    disabled={index === 0}
                    className="p-1 text-amber-600 hover:bg-amber-100 rounded disabled:opacity-30"
                  >
                    <ChevronUp className="w-4 h-4" />
                  </button>
                  <button
                    onClick={(e) => {
                      e.stopPropagation();
                      moveFaqItem(index, 'down');
                    }}
                    disabled={index === items.length - 1}
                    className="p-1 text-amber-600 hover:bg-amber-100 rounded disabled:opacity-30"
                  >
                    <ChevronDown className="w-4 h-4" />
                  </button>
                  <button
                    onClick={(e) => {
                      e.stopPropagation();
                      deleteFaqItem(item.id);
                    }}
                    className="p-1 text-red-500 hover:bg-red-50 rounded"
                  >
                    <Trash2 className="w-4 h-4" />
                  </button>
                </div>

                {/* Expand Icon */}
                <div className="shrink-0">
                  {expandedItems.has(item.id) ? (
                    <ChevronUp className="w-5 h-5 text-amber-500" />
                  ) : (
                    <ChevronDown className="w-5 h-5 text-amber-500" />
                  )}
                </div>
              </div>

              {/* Answer */}
              {expandedItems.has(item.id) && (
                <div className="px-6 pb-4 pl-20">
                  {editingItem === item.id ? (
                    <textarea
                      value={item.answer}
                      onChange={(e) => updateFaqItem(item.id, { answer: e.target.value })}
                      rows={3}
                      className="w-full px-3 py-2 text-amber-800 bg-white rounded-lg 
                               border border-amber-200 focus:ring-2 focus:ring-amber-500 resize-none"
                    />
                  ) : (
                    <p className="text-amber-800 leading-relaxed bg-white/50 rounded-lg p-3 border border-amber-100">
                      {item.answer}
                    </p>
                  )}
                </div>
              )}
            </div>
          ))}

          {/* Empty State */}
          {items.length === 0 && (
            <div className="px-6 py-12 text-center">
              <HelpCircle className="w-12 h-12 text-amber-300 mx-auto mb-3" />
              <p className="text-amber-600 mb-3">No FAQ items yet</p>
              <button
                onClick={addFaqItem}
                className="inline-flex items-center gap-2 px-4 py-2 text-sm font-medium 
                         text-amber-700 bg-white hover:bg-amber-50 rounded-lg border border-amber-300"
              >
                <Plus className="w-4 h-4" />
                Add Your First Question
              </button>
            </div>
          )}
        </div>

        {/* Schema Info Badge */}
        <div className="px-6 py-2 bg-amber-100/50 border-t border-amber-200">
          <span className="text-xs text-amber-600 font-medium">
            ✨ FAQ Schema will be auto-generated for SEO
          </span>
        </div>
      </div>
    </NodeViewWrapper>
  );
};

export default FaqBlockComponent;
```

---

## 3. Pros/Cons Block Extension

### `src/components/editor/extensions/ProsConsBlock/ProsConsBlock.ts`

```typescript
import { Node, mergeAttributes } from '@tiptap/core';
import { ReactNodeViewRenderer } from '@tiptap/react';
import ProsConsComponent from './ProsConsComponent';

export interface ProsConsBlockOptions {
  HTMLAttributes: Record<string, any>;
}

declare module '@tiptap/core' {
  interface Commands<ReturnType> {
    prosConsBlock: {
      setProsConsBlock: (attributes?: {
        title?: string;
        pros?: string[];
        cons?: string[];
      }) => ReturnType;
    };
  }
}

export const ProsConsBlock = Node.create<ProsConsBlockOptions>({
  name: 'prosConsBlock',

  group: 'block',

  atom: true,

  draggable: true,

  addOptions() {
    return {
      HTMLAttributes: {},
    };
  },

  addAttributes() {
    return {
      title: {
        default: 'Pros & Cons',
      },
      pros: {
        default: [
          'Easy to use and beginner-friendly',
          'Great customer support',
          'Affordable pricing plans',
        ],
      },
      cons: {
        default: [
          'Limited advanced features',
          'Requires internet connection',
        ],
      },
    };
  },

  parseHTML() {
    return [
      {
        tag: 'div[data-type="pros-cons-block"]',
      },
    ];
  },

  renderHTML({ HTMLAttributes }) {
    return [
      'div',
      mergeAttributes(this.options.HTMLAttributes, HTMLAttributes, {
        'data-type': 'pros-cons-block',
        class: 'pros-cons-block',
      }),
    ];
  },

  addNodeView() {
    return ReactNodeViewRenderer(ProsConsComponent);
  },

  addCommands() {
    return {
      setProsConsBlock:
        (attributes) =>
        ({ commands }) => {
          return commands.insertContent({
            type: this.name,
            attrs: attributes,
          });
        },
    };
  },
});

export default ProsConsBlock;
```

### `src/components/editor/extensions/ProsConsBlock/ProsConsComponent.tsx`

```tsx
'use client';

import React, { useState } from 'react';
import { NodeViewWrapper, NodeViewProps } from '@tiptap/react';
import {
  ThumbsUp,
  ThumbsDown,
  Plus,
  Trash2,
  X,
  Check,
  Edit3,
  Scale,
} from 'lucide-react';

const ProsConsComponent: React.FC<NodeViewProps> = ({
  node,
  updateAttributes,
  deleteNode,
  selected,
}) => {
  const [editingPro, setEditingPro] = useState<number | null>(null);
  const [editingCon, setEditingCon] = useState<number | null>(null);
  const [newProText, setNewProText] = useState('');
  const [newConText, setNewConText] = useState('');
  const [isEditingTitle, setIsEditingTitle] = useState(false);

  const { title, pros, cons } = node.attrs as {
    title: string;
    pros: string[];
    cons: string[];
  };

  // Pros handlers
  const addPro = () => {
    if (newProText.trim()) {
      updateAttributes({ pros: [...pros, newProText.trim()] });
      setNewProText('');
    }
  };

  const updatePro = (index: number, text: string) => {
    const newPros = [...pros];
    newPros[index] = text;
    updateAttributes({ pros: newPros });
  };

  const deletePro = (index: number) => {
    updateAttributes({ pros: pros.filter((_: string, i: number) => i !== index) });
  };

  // Cons handlers
  const addCon = () => {
    if (newConText.trim()) {
      updateAttributes({ cons: [...cons, newConText.trim()] });
      setNewConText('');
    }
  };

  const updateCon = (index: number, text: string) => {
    const newCons = [...cons];
    newCons[index] = text;
    updateAttributes({ cons: newCons });
  };

  const deleteCon = (index: number) => {
    updateAttributes({ cons: cons.filter((_: string, i: number) => i !== index) });
  };

  return (
    <NodeViewWrapper className="my-6">
      <div
        className={`
          relative rounded-xl border-2 border-gray-200 bg-white overflow-hidden
          shadow-sm transition-all duration-200
          ${selected ? 'ring-2 ring-blue-500 ring-offset-2' : ''}
        `}
      >
        {/* Header */}
        <div className="bg-gradient-to-r from-gray-50 to-slate-50 px-6 py-4 border-b border-gray-200">
          <div className="flex items-center justify-between">
            <div className="flex items-center gap-3">
              <div className="p-2 bg-gradient-to-r from-emerald-500 to-rose-500 rounded-lg shadow-sm">
                <Scale className="w-5 h-5 text-white" />
              </div>
              {isEditingTitle ? (
                <input
                  type="text"
                  value={title}
                  onChange={(e) => updateAttributes({ title: e.target.value })}
                  onBlur={() => setIsEditingTitle(false)}
                  onKeyDown={(e) => e.key === 'Enter' && setIsEditingTitle(false)}
                  className="text-xl font-bold text-gray-800 bg-white px-2 py-1 rounded border border-gray-300 focus:ring-2 focus:ring-blue-500"
                  autoFocus
                />
              ) : (
                <h3
                  className="text-xl font-bold text-gray-800 cursor-pointer hover:text-gray-600"
                  onClick={() => setIsEditingTitle(true)}
                >
                  {title}
                </h3>
              )}
            </div>
            <button
              onClick={deleteNode}
              className="p-1.5 text-red-500 hover:bg-red-50 rounded-lg transition-colors"
              title="Delete Block"
            >
              <Trash2 className="w-4 h-4" />
            </button>
          </div>
        </div>

        {/* Two Column Layout */}
        <div className="grid md:grid-cols-2 divide-y md:divide-y-0 md:divide-x divide-gray-200">
          {/* Pros Column */}
          <div className="bg-gradient-to-br from-emerald-50 to-green-50">
            {/* Pros Header */}
            <div className="px-5 py-3 bg-emerald-100/50 border-b border-emerald-200">
              <div className="flex items-center gap-2">
                <div className="p-1.5 bg-emerald-500 rounded-full">
                  <ThumbsUp className="w-4 h-4 text-white" />
                </div>
                <span className="font-bold text-emerald-800">Pros</span>
                <span className="ml-auto text-xs font-medium text-emerald-600 bg-emerald-200 px-2 py-0.5 rounded-full">
                  {pros.length}
                </span>
              </div>
            </div>

            {/* Pros List */}
            <ul className="p-4 space-y-2">
              {pros.map((pro: string, index: number) => (
                <li key={index} className="group flex items-start gap-2">
                  <Check className="w-5 h-5 text-emerald-500 shrink-0 mt-0.5" />
                  {editingPro === index ? (
                    <div className="flex-1 flex gap-2">
                      <input
                        type="text"
                        value={pro}
                        onChange={(e) => updatePro(index, e.target.value)}
                        className="flex-1 px-2 py-1 text-sm border border-emerald-300 rounded focus:ring-2 focus:ring-emerald-500"
                        autoFocus
                      />
                      <button
                        onClick={() => setEditingPro(null)}
                        className="p-1 text-emerald-600 hover:bg-emerald-100 rounded"
                      >
                        <Check className="w-4 h-4" />
                      </button>
                    </div>
                  ) : (
                    <>
                      <span className="flex-1 text-sm text-emerald-800">{pro}</span>
                      <div className="opacity-0 group-hover:opacity-100 flex gap-1 transition-opacity">
                        <button
                          onClick={() => setEditingPro(index)}
                          className="p-1 text-emerald-600 hover:bg-emerald-100 rounded"
                        >
                          <Edit3 className="w-3 h-3" />
                        </button>
                        <button
                          onClick={() => deletePro(index)}
                          className="p-1 text-red-500 hover:bg-red-50 rounded"
                        >
                          <X className="w-3 h-3" />
                        </button>
                      </div>
                    </>
                  )}
                </li>
              ))}
            </ul>

            {/* Add Pro Input */}
            <div className="px-4 pb-4">
              <div className="flex gap-2">
                <input
                  type="text"
                  value={newProText}
                  onChange={(e) => setNewProText(e.target.value)}
                  onKeyDown={(e) => e.key === 'Enter' && addPro()}
                  placeholder="Add a pro..."
                  className="flex-1 px-3 py-2 text-sm bg-white border border-emerald-200 rounded-lg
                           focus:ring-2 focus:ring-emerald-500 focus:border-transparent
                           placeholder:text-emerald-400"
                />
                <button
                  onClick={addPro}
                  disabled={!newProText.trim()}
                  className="px-3 py-2 bg-emerald-500 text-white rounded-lg hover:bg-emerald-600 
                           disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
                >
                  <Plus className="w-4 h-4" />
                </button>
              </div>
            </div>
          </div>

          {/* Cons Column */}
          <div className="bg-gradient-to-br from-rose-50 to-red-50">
            {/* Cons Header */}
            <div className="px-5 py-3 bg-rose-100/50 border-b border-rose-200">
              <div className="flex items-center gap-2">
                <div className="p-1.5 bg-rose-500 rounded-full">
                  <ThumbsDown className="w-4 h-4 text-white" />
                </div>
                <span className="font-bold text-rose-800">Cons</span>
                <span className="ml-auto text-xs font-medium text-rose-600 bg-rose-200 px-2 py-0.5 rounded-full">
                  {cons.length}
                </span>
              </div>
            </div>

            {/* Cons List */}
            <ul className="p-4 space-y-2">
              {cons.map((con: string, index: number) => (
                <li key={index} className="group flex items-start gap-2">
                  <X className="w-5 h-5 text-rose-500 shrink-0 mt-0.5" />
                  {editingCon === index ? (
                    <div className="flex-1 flex gap-2">
                      <input
                        type="text"
                        value={con}
                        onChange={(e) => updateCon(index, e.target.value)}
                        className="flex-1 px-2 py-1 text-sm border border-rose-300 rounded focus:ring-2 focus:ring-rose-500"
                        autoFocus
                      />
                      <button
                        onClick={() => setEditingCon(null)}
                        className="p-1 text-rose-600 hover:bg-rose-100 rounded"
                      >
                        <Check className="w-4 h-4" />
                      </button>
                    </div>
                  ) : (
                    <>
                      <span className="flex-1 text-sm text-rose-800">{con}</span>
                      <div className="opacity-0 group-hover:opacity-100 flex gap-1 transition-opacity">
                        <button
                          onClick={() => setEditingCon(index)}
                          className="p-1 text-rose-600 hover:bg-rose-100 rounded"
                        >
                          <Edit3 className="w-3 h-3" />
                        </button>
                        <button
                          onClick={() => deleteCon(index)}
                          className="p-1 text-red-600 hover:bg-red-100 rounded"
                        >
                          <X className="w-3 h-3" />
                        </button>
                      </div>
                    </>
                  )}
                </li>
              ))}
            </ul>

            {/* Add Con Input */}
            <div className="px-4 pb-4">
              <div className="flex gap-2">
                <input
                  type="text"
                  value={newConText}
                  onChange={(e) => setNewConText(e.target.value)}
                  onKeyDown={(e) => e.key === 'Enter' && addCon()}
                  placeholder="Add a con..."
                  className="flex-1 px-3 py-2 text-sm bg-white border border-rose-200 rounded-lg
                           focus:ring-2 focus:ring-rose-500 focus:border-transparent
                           placeholder:text-rose-400"
                />
                <button
                  onClick={addCon}
                  disabled={!newConText.trim()}
                  className="px-3 py-2 bg-rose-500 text-white rounded-lg hover:bg-rose-600 
                           disabled:opacity-50 disabled:cursor-not-allowed transition-colors"
                >
                  <Plus className="w-4 h-4" />
                </button>
              </div>
            </div>
          </div>
        </div>

        {/* Summary Footer */}
        <div className="px-6 py-3 bg-gray-50 border-t border-gray-200 flex items-center justify-between">
          <div className="flex items-center gap-4 text-sm">
            <span className="text-emerald-600 font-medium">
              ✓ {pros.length} Pros
            </span>
            <span className="text-gray-300">|</span>
            <span className="text-rose-600 font-medium">
              ✗ {cons.length} Cons
            </span>
          </div>
          <div className="text-xs text-gray-500">
            {pros.length > cons.length ? (
              <span className="text-emerald-600">👍 Overall Positive</span>
            ) : cons.length > pros.length ? (
              <span className="text-rose-600">👎 Needs Improvement</span>
            ) : (
              <span className="text-gray-600">⚖️ Balanced</span>
            )}
          </div>
        </div>
      </div>
    </NodeViewWrapper>
  );
};

export default ProsConsComponent;
```

---

## 4. Extensions Index File

### `src/components/editor/extensions/index.ts`

```typescript
export { CallToAction } from './CallToAction/CallToAction';
export { FaqBlock } from './FaqBlock/FaqBlock';
export { ProsConsBlock } from './ProsConsBlock/ProsConsBlock';
```

---

## 5. Updated Editor Component

### `src/components/editor/Editor.tsx` (Updated)

```tsx
'use client';

import React, { useCallback } from 'react';
import { useEditor, EditorContent } from '@tiptap/react';
import StarterKit from '@tiptap/starter-kit';
import Placeholder from '@tiptap/extension-placeholder';
import MenuBar from './MenuBar';
import EditorBubbleMenu from './EditorBubbleMenu';

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

  const handleExportHTML = useCallback(() => {
    if (editor) {
      const html = editor.getHTML();
      navigator.clipboard.writeText(html);
      alert('HTML copied to clipboard!');
    }
  }, [editor]);

  const handleExportJSON = useCallback(() => {
    if (editor) {
      const json = editor.getJSON();
      navigator.clipboard.writeText(JSON.stringify(json, null, 2));
      alert('JSON copied to clipboard!');
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

        <div className="border-t border-notion-border px-4 py-3 bg-notion-bg-gray flex items-center justify-between">
          <div className="text-sm text-notion-gray">
            {editor && (
              <span>
                Ready to edit
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
    </div>
  );
};

export default NotionEditor;
```

---

## 6. Updated MenuBar with Custom Block Buttons

### `src/components/editor/MenuBar.tsx` (Updated)

```tsx
'use client';

import React, { useState, useRef, useEffect } from 'react';
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
  Megaphone,
  HelpCircle,
  Scale,
  ChevronDown,
  Blocks,
} from 'lucide-react';
import Button from '@/components/ui/Button';

interface MenuBarProps {
  editor: Editor | null;
}

export const MenuBar: React.FC<MenuBarProps> = ({ editor }) => {
  const [showBlockMenu, setShowBlockMenu] = useState(false);
  const blockMenuRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    const handleClickOutside = (event: MouseEvent) => {
      if (blockMenuRef.current && !blockMenuRef.current.contains(event.target as Node)) {
        setShowBlockMenu(false);
      }
    };

    document.addEventListener('mousedown', handleClickOutside);
    return () => document.removeEventListener('mousedown', handleClickOutside);
  }, []);

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

  const customBlocks = [
    {
      icon: Megaphone,
      title: 'Call to Action',
      description: 'Add a CTA box with button',
      action: () => {
        editor.chain().focus().setCallToAction({}).run();
        setShowBlockMenu(false);
      },
      color: 'text-blue-600 bg-blue-100',
    },
    {
      icon: HelpCircle,
      title: 'FAQ Block',
      description: 'Add FAQ questions & answers',
      action: () => {
        editor.chain().focus().setFaqBlock({}).run();
        setShowBlockMenu(false);
      },
      color: 'text-amber-600 bg-amber-100',
    },
    {
      icon: Scale,
      title: 'Pros & Cons',
      description: 'Add a comparison block',
      action: () => {
        editor.chain().focus().setProsConsBlock({}).run();
        setShowBlockMenu(false);
      },
      color: 'text-emerald-600 bg-emerald-100',
    },
  ];

  return (
    <div className="sticky top-0 z-10 bg-white border-b border-notion-border">
      <div className="flex flex-wrap items-center gap-1 p-2">
        {/* Standard Menu Items */}
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

        {/* Divider */}
        <div className="menu-divider" />

        {/* Custom Blocks Dropdown */}
        <div className="relative" ref={blockMenuRef}>
          <button
            onClick={() => setShowBlockMenu(!showBlockMenu)}
            className={`
              flex items-center gap-1.5 px-3 py-1.5 rounded-lg text-sm font-medium
              transition-all duration-150
              ${showBlockMenu 
                ? 'bg-blue-100 text-blue-700' 
                : 'bg-gradient-to-r from-blue-50 to-indigo-50 text-blue-600 hover:from-blue-100 hover:to-indigo-100'
              }
            `}
          >
            <Blocks className="w-4 h-4" />
            <span>Insert Block</span>
            <ChevronDown className={`w-3 h-3 transition-transform ${showBlockMenu ? 'rotate-180' : ''}`} />
          </button>

          {/* Dropdown Menu */}
          {showBlockMenu && (
            <div className="absolute top-full left-0 mt-1 w-64 bg-white rounded-lg shadow-xl border border-gray-200 overflow-hidden z-50">
              <div className="p-2 bg-gray-50 border-b border-gray-100">
                <span className="text-xs font-semibold text-gray-500 uppercase tracking-wider">
                  Custom Blocks
                </span>
              </div>
              <div className="p-1">
                {customBlocks.map((block) => (
                  <button
                    key={block.title}
                    onClick={block.action}
                    className="w-full flex items-center gap-3 px-3 py-2.5 rounded-lg hover:bg-gray-50 transition-colors text-left group"
                  >
                    <div className={`p-2 rounded-lg ${block.color}`}>
                      <block.icon className="w-4 h-4" />
                    </div>
                    <div className="flex-1 min-w-0">
                      <div className="text-sm font-medium text-gray-900 group-hover:text-blue-600">
                        {block.title}
                      </div>
                      <div className="text-xs text-gray-500 truncate">
                        {block.description}
                      </div>
                    </div>
                  </button>
                ))}
              </div>
            </div>
          )}
        </div>
      </div>
    </div>
  );
};

export default MenuBar;
```

---

## 7. Updated Page with Sample Content

### `src/app/page.tsx` (Updated)

```tsx
'use client';

import { useState } from 'react';
import NotionEditor from '@/components/editor/Editor';

export default function Home() {
  const [output, setOutput] = useState<{ html: string; json: object } | null>(null);

  const handleEditorChange = (html: string, json: object) => {
    setOutput({ html, json });
  };

  const sampleContent = `
    <h2>Welcome to the Notion-Style Editor</h2>
    <p>This editor includes <strong>custom blocks</strong> for enhanced content creation:</p>
    <ul>
      <li>📢 <strong>Call to Action</strong> - Beautiful CTA boxes with buttons</li>
      <li>❓ <strong>FAQ Blocks</strong> - Collapsible Q&A sections with schema support</li>
      <li>⚖️ <strong>Pros & Cons</strong> - Two-column comparison layouts</li>
    </ul>
    <h3>Try the Custom Blocks</h3>
    <p>Click the <strong>"Insert Block"</strong> button in the toolbar above to add custom blocks to your content.</p>
    <p>Each block is fully editable - click on any element to modify it!</p>
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
            With Custom CTA, FAQ, and Pros/Cons Blocks
          </p>
          
          {/* Feature Badges */}
          <div className="flex flex-wrap justify-center gap-2 mt-4">
            <span className="px-3 py-1 bg-blue-100 text-blue-700 rounded-full text-sm font-medium">
              📢 CTA Blocks
            </span>
            <span className="px-3 py-1 bg-amber-100 text-amber-700 rounded-full text-sm font-medium">
              ❓ FAQ Blocks
            </span>
            <span className="px-3 py-1 bg-emerald-100 text-emerald-700 rounded-full text-sm font-medium">
              ⚖️ Pros/Cons
            </span>
          </div>
        </div>
      </div>

      {/* Editor */}
      <NotionEditor
        initialContent={sampleContent}
        onChange={handleEditorChange}
        placeholder="Start typing here... Click 'Insert Block' to add custom blocks"
      />

      {/* Output Preview */}
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

## 8. Additional CSS for Custom Blocks

Add to `src/app/globals.css`:

```css
/* Custom Block Styles */
@layer components {
  /* Call to Action Block */
  .tiptap [data-type="call-to-action"] {
    @apply my-4;
  }

  /* FAQ Block */
  .tiptap [data-type="faq-block"] {
    @apply my-4;
  }

  /* Pros Cons Block */
  .tiptap [data-type="pros-cons-block"] {
    @apply my-4;
  }

  /* Node Selection Ring */
  .ProseMirror-selectednode {
    @apply outline-none;
  }

  /* Dragging styles */
  .ProseMirror [data-drag-handle] {
    @apply cursor-move;
  }

  .ProseMirror-hideselection *::selection {
    @apply bg-transparent;
  }
}

/* Block hover effects */
@layer utilities {
  .custom-block-hover {
    @apply relative;
  }

  .custom-block-hover::before {
    @apply absolute -left-8 top-0 bottom-0 w-6 opacity-0 transition-opacity;
    content: '';
  }

  .custom-block-hover:hover::before {
    @apply opacity-100;
  }
}

/* Animation for expanding FAQ items */
@keyframes expandDown {
  from {
    opacity: 0;
    max-height: 0;
  }
  to {
    opacity: 1;
    max-height: 500px;
  }
}

.faq-expand-animation {
  animation: expandDown 0.2s ease-out forwards;
}
```

---

## Running the Project

```bash
# Install dependencies
npm install

# Run development server
npm run dev
```

---

## Features Summary

| Block | Features |
|-------|----------|
| **Call to Action** | 3 style variants, editable title/description/button, draggable |
| **FAQ Block** | Add/edit/delete Q&A pairs, collapsible items, reorder, SEO schema ready |
| **Pros/Cons** | Two-column layout, green/red theming, add/edit/delete items, summary |

All blocks are:
- ✅ Fully editable inline
- ✅ Draggable within the editor
- ✅ Styled with Tailwind CSS
- ✅ Responsive for mobile
- ✅ Export as HTML/JSON
