# Real-Time Stats Sidebar Integration

Analyze standard keyword density and reading time calculation logic, then integrate it into my Next.js + Tiptap editor as a real-time statistics sidebar that updates while the user types.

## Project Structure Update

```
src/
├── components/
│   └── editor/
│       ├── sidebar/
│       │   ├── StatsSidebar.tsx
│       │   ├── KeywordDensity.tsx
│       │   ├── ReadingTime.tsx
│       │   ├── ContentStats.tsx
│       │   └── index.ts
│       ├── utils/
│       │   ├── textAnalyzer.ts
│       │   └── index.ts
│       ├── Editor.tsx (updated)
│       └── EditorWithSidebar.tsx (new)
└── ...
```

---

## 1. Text Analyzer Utility

### `src/components/editor/utils/textAnalyzer.ts`

```typescript
/**
 * Text Analysis Utilities
 * Keyword Density & Reading Time calculations
 * Based on common SEO analysis patterns
 */

export interface TextStats {
  // Basic counts
  characterCount: number;
  characterCountNoSpaces: number;
  wordCount: number;
  sentenceCount: number;
  paragraphCount: number;
  
  // Reading metrics
  readingTime: {
    minutes: number;
    seconds: number;
    text: string;
  };
  speakingTime: {
    minutes: number;
    seconds: number;
    text: string;
  };
  
  // Content structure
  headingCount: {
    h1: number;
    h2: number;
    h3: number;
    total: number;
  };
  linkCount: {
    internal: number;
    external: number;
    total: number;
  };
  imageCount: number;
}

export interface KeywordAnalysis {
  keyword: string;
  count: number;
  density: number;
  prominence: number;
  inTitle: boolean;
  inFirstParagraph: boolean;
  inHeadings: boolean;
  distribution: 'good' | 'front-heavy' | 'back-heavy' | 'poor';
  status: 'optimal' | 'low' | 'high' | 'missing';
}

export interface TopKeyword {
  word: string;
  count: number;
  density: number;
}

// Common stop words to exclude from keyword analysis
const STOP_WORDS = new Set([
  'a', 'an', 'the', 'and', 'or', 'but', 'in', 'on', 'at', 'to', 'for',
  'of', 'with', 'by', 'from', 'as', 'is', 'was', 'are', 'were', 'been',
  'be', 'have', 'has', 'had', 'do', 'does', 'did', 'will', 'would', 'could',
  'should', 'may', 'might', 'must', 'shall', 'can', 'need', 'dare', 'ought',
  'used', 'it', 'its', "it's", 'this', 'that', 'these', 'those', 'i', 'you',
  'he', 'she', 'we', 'they', 'what', 'which', 'who', 'whom', 'whose',
  'where', 'when', 'why', 'how', 'all', 'each', 'every', 'both', 'few',
  'more', 'most', 'other', 'some', 'such', 'no', 'nor', 'not', 'only',
  'own', 'same', 'so', 'than', 'too', 'very', 'just', 'also', 'now',
  'here', 'there', 'then', 'once', 'if', 'because', 'about', 'into',
  'through', 'during', 'before', 'after', 'above', 'below', 'between',
  'under', 'again', 'further', 'while', 'your', 'my', 'his', 'her', 'our'
]);

// Reading speed constants (words per minute)
const READING_SPEED = {
  slow: 150,
  average: 200,
  fast: 250,
};

const SPEAKING_SPEED = {
  slow: 100,
  average: 130,
  fast: 160,
};

/**
 * Extract plain text from HTML
 */
export function extractTextFromHTML(html: string): string {
  const div = document.createElement('div');
  div.innerHTML = html;
  return div.textContent || div.innerText || '';
}

/**
 * Count words in text
 */
export function countWords(text: string): number {
  const words = text
    .trim()
    .split(/\s+/)
    .filter(word => word.length > 0);
  return words.length;
}

/**
 * Count sentences in text
 */
export function countSentences(text: string): number {
  const sentences = text
    .split(/[.!?]+/)
    .filter(sentence => sentence.trim().length > 0);
  return sentences.length;
}

/**
 * Count paragraphs in HTML
 */
export function countParagraphs(html: string): number {
  const div = document.createElement('div');
  div.innerHTML = html;
  const paragraphs = div.querySelectorAll('p');
  let count = 0;
  paragraphs.forEach(p => {
    if (p.textContent && p.textContent.trim().length > 0) {
      count++;
    }
  });
  return Math.max(count, 1);
}

/**
 * Calculate reading time
 */
export function calculateReadingTime(
  wordCount: number,
  speed: 'slow' | 'average' | 'fast' = 'average'
): { minutes: number; seconds: number; text: string } {
  const wpm = READING_SPEED[speed];
  const totalSeconds = Math.ceil((wordCount / wpm) * 60);
  const minutes = Math.floor(totalSeconds / 60);
  const seconds = totalSeconds % 60;

  let text = '';
  if (minutes === 0) {
    text = seconds <= 30 ? 'Less than a minute' : '< 1 min read';
  } else if (minutes === 1) {
    text = '1 min read';
  } else {
    text = `${minutes} min read`;
  }

  return { minutes, seconds, text };
}

/**
 * Calculate speaking time
 */
export function calculateSpeakingTime(
  wordCount: number,
  speed: 'slow' | 'average' | 'fast' = 'average'
): { minutes: number; seconds: number; text: string } {
  const wpm = SPEAKING_SPEED[speed];
  const totalSeconds = Math.ceil((wordCount / wpm) * 60);
  const minutes = Math.floor(totalSeconds / 60);
  const seconds = totalSeconds % 60;

  let text = '';
  if (minutes === 0) {
    text = `${seconds}s speaking time`;
  } else if (minutes === 1) {
    text = `1 min ${seconds}s speaking`;
  } else {
    text = `${minutes} min ${seconds}s speaking`;
  }

  return { minutes, seconds, text };
}

/**
 * Count headings in HTML
 */
export function countHeadings(html: string): { h1: number; h2: number; h3: number; total: number } {
  const div = document.createElement('div');
  div.innerHTML = html;
  
  const h1 = div.querySelectorAll('h1').length;
  const h2 = div.querySelectorAll('h2').length;
  const h3 = div.querySelectorAll('h3').length;
  
  return {
    h1,
    h2,
    h3,
    total: h1 + h2 + h3,
  };
}

/**
 * Count links in HTML
 */
export function countLinks(html: string): { internal: number; external: number; total: number } {
  const div = document.createElement('div');
  div.innerHTML = html;
  const links = div.querySelectorAll('a[href]');
  
  let internal = 0;
  let external = 0;
  const currentHost = typeof window !== 'undefined' ? window.location.hostname : '';
  
  links.forEach(link => {
    const href = link.getAttribute('href') || '';
    if (href.startsWith('http')) {
      try {
        const url = new URL(href);
        if (url.hostname === currentHost || url.hostname === '') {
          internal++;
        } else {
          external++;
        }
      } catch {
        internal++;
      }
    } else if (href.startsWith('#') || href.startsWith('/')) {
      internal++;
    }
  });
  
  return { internal, external, total: internal + external };
}

/**
 * Count images in HTML
 */
export function countImages(html: string): number {
  const div = document.createElement('div');
  div.innerHTML = html;
  return div.querySelectorAll('img').length;
}

/**
 * Get complete text statistics
 */
export function getTextStats(html: string): TextStats {
  const text = extractTextFromHTML(html);
  const wordCount = countWords(text);
  
  return {
    characterCount: text.length,
    characterCountNoSpaces: text.replace(/\s/g, '').length,
    wordCount,
    sentenceCount: countSentences(text),
    paragraphCount: countParagraphs(html),
    readingTime: calculateReadingTime(wordCount),
    speakingTime: calculateSpeakingTime(wordCount),
    headingCount: countHeadings(html),
    linkCount: countLinks(html),
    imageCount: countImages(html),
  };
}

/**
 * Tokenize text into words
 */
function tokenize(text: string): string[] {
  return text
    .toLowerCase()
    .replace(/[^a-z0-9\s]/g, ' ')
    .split(/\s+/)
    .filter(word => word.length > 2 && !STOP_WORDS.has(word));
}

/**
 * Calculate keyword density
 */
export function calculateKeywordDensity(
  keyword: string,
  text: string,
  html: string
): KeywordAnalysis {
  const normalizedKeyword = keyword.toLowerCase().trim();
  const normalizedText = text.toLowerCase();
  const wordCount = countWords(text);
  
  if (!normalizedKeyword || wordCount === 0) {
    return {
      keyword: normalizedKeyword,
      count: 0,
      density: 0,
      prominence: 0,
      inTitle: false,
      inFirstParagraph: false,
      inHeadings: false,
      distribution: 'poor',
      status: 'missing',
    };
  }
  
  // Count keyword occurrences (including phrases)
  const keywordRegex = new RegExp(normalizedKeyword.replace(/[.*+?^${}()|[\]\\]/g, '\\$&'), 'gi');
  const matches = normalizedText.match(keywordRegex) || [];
  const count = matches.length;
  
  // Calculate density (percentage)
  const keywordWordCount = normalizedKeyword.split(/\s+/).length;
  const density = (count * keywordWordCount / wordCount) * 100;
  
  // Check if keyword is in important places
  const div = document.createElement('div');
  div.innerHTML = html;
  
  // Check title/h1
  const h1Elements = div.querySelectorAll('h1');
  let inTitle = false;
  h1Elements.forEach(h1 => {
    if (h1.textContent?.toLowerCase().includes(normalizedKeyword)) {
      inTitle = true;
    }
  });
  
  // Check first paragraph
  const firstParagraph = div.querySelector('p');
  const inFirstParagraph = firstParagraph?.textContent?.toLowerCase().includes(normalizedKeyword) || false;
  
  // Check all headings
  const headings = div.querySelectorAll('h1, h2, h3, h4, h5, h6');
  let inHeadings = false;
  headings.forEach(heading => {
    if (heading.textContent?.toLowerCase().includes(normalizedKeyword)) {
      inHeadings = true;
    }
  });
  
  // Calculate prominence (position-based score)
  const firstOccurrence = normalizedText.indexOf(normalizedKeyword);
  const prominence = firstOccurrence === -1 ? 0 : Math.max(0, 100 - (firstOccurrence / normalizedText.length) * 100);
  
  // Analyze distribution
  const textLength = normalizedText.length;
  const positions = [];
  let pos = normalizedText.indexOf(normalizedKeyword);
  while (pos !== -1) {
    positions.push(pos / textLength);
    pos = normalizedText.indexOf(normalizedKeyword, pos + 1);
  }
  
  let distribution: 'good' | 'front-heavy' | 'back-heavy' | 'poor' = 'poor';
  if (positions.length >= 3) {
    const avgPosition = positions.reduce((a, b) => a + b, 0) / positions.length;
    const hasStart = positions.some(p => p < 0.2);
    const hasMiddle = positions.some(p => p >= 0.3 && p <= 0.7);
    const hasEnd = positions.some(p => p > 0.8);
    
    if (hasStart && hasMiddle && hasEnd) {
      distribution = 'good';
    } else if (avgPosition < 0.4) {
      distribution = 'front-heavy';
    } else if (avgPosition > 0.6) {
      distribution = 'back-heavy';
    }
  } else if (positions.length > 0) {
    distribution = 'poor';
  }
  
  // Determine status based on density
  let status: 'optimal' | 'low' | 'high' | 'missing' = 'missing';
  if (count === 0) {
    status = 'missing';
  } else if (density < 0.5) {
    status = 'low';
  } else if (density > 3) {
    status = 'high';
  } else {
    status = 'optimal';
  }
  
  return {
    keyword: normalizedKeyword,
    count,
    density: Math.round(density * 100) / 100,
    prominence: Math.round(prominence * 100) / 100,
    inTitle,
    inFirstParagraph,
    inHeadings,
    distribution,
    status,
  };
}

/**
 * Get top keywords from text
 */
export function getTopKeywords(text: string, limit: number = 10): TopKeyword[] {
  const words = tokenize(text);
  const wordCount = countWords(text);
  const frequency: Record<string, number> = {};
  
  words.forEach(word => {
    frequency[word] = (frequency[word] || 0) + 1;
  });
  
  const sorted = Object.entries(frequency)
    .map(([word, count]) => ({
      word,
      count,
      density: Math.round((count / wordCount) * 100 * 100) / 100,
    }))
    .sort((a, b) => b.count - a.count)
    .slice(0, limit);
  
  return sorted;
}

/**
 * Get two-word phrases (bigrams)
 */
export function getTopPhrases(text: string, limit: number = 5): TopKeyword[] {
  const words = tokenize(text);
  const wordCount = countWords(text);
  const phrases: Record<string, number> = {};
  
  for (let i = 0; i < words.length - 1; i++) {
    const phrase = `${words[i]} ${words[i + 1]}`;
    phrases[phrase] = (phrases[phrase] || 0) + 1;
  }
  
  const sorted = Object.entries(phrases)
    .filter(([, count]) => count >= 2)
    .map(([phrase, count]) => ({
      word: phrase,
      count,
      density: Math.round((count * 2 / wordCount) * 100 * 100) / 100,
    }))
    .sort((a, b) => b.count - a.count)
    .slice(0, limit);
  
  return sorted;
}

export default {
  getTextStats,
  calculateKeywordDensity,
  getTopKeywords,
  getTopPhrases,
  extractTextFromHTML,
  countWords,
  calculateReadingTime,
  calculateSpeakingTime,
};
```

---

## 2. Stats Sidebar Components

### `src/components/editor/sidebar/ContentStats.tsx`

```tsx
'use client';

import React from 'react';
import {
  FileText,
  Type,
  AlignLeft,
  Hash,
  Link,
  Image,
  Heading,
} from 'lucide-react';
import { TextStats } from '../utils/textAnalyzer';

interface ContentStatsProps {
  stats: TextStats;
}

export const ContentStats: React.FC<ContentStatsProps> = ({ stats }) => {
  const statItems = [
    {
      icon: Type,
      label: 'Characters',
      value: stats.characterCount.toLocaleString(),
      subValue: `${stats.characterCountNoSpaces.toLocaleString()} without spaces`,
      color: 'text-blue-600 bg-blue-100',
    },
    {
      icon: FileText,
      label: 'Words',
      value: stats.wordCount.toLocaleString(),
      color: 'text-emerald-600 bg-emerald-100',
    },
    {
      icon: AlignLeft,
      label: 'Sentences',
      value: stats.sentenceCount.toLocaleString(),
      color: 'text-purple-600 bg-purple-100',
    },
    {
      icon: Hash,
      label: 'Paragraphs',
      value: stats.paragraphCount.toLocaleString(),
      color: 'text-orange-600 bg-orange-100',
    },
  ];

  return (
    <div className="space-y-4">
      {/* Main Stats Grid */}
      <div className="grid grid-cols-2 gap-3">
        {statItems.map((item) => (
          <div
            key={item.label}
            className="bg-white rounded-lg border border-gray-200 p-3 hover:shadow-sm transition-shadow"
          >
            <div className="flex items-center gap-2 mb-1">
              <div className={`p-1 rounded ${item.color}`}>
                <item.icon className="w-3 h-3" />
              </div>
              <span className="text-xs text-gray-500">{item.label}</span>
            </div>
            <div className="text-lg font-bold text-gray-800">{item.value}</div>
            {item.subValue && (
              <div className="text-xs text-gray-400 mt-0.5">{item.subValue}</div>
            )}
          </div>
        ))}
      </div>

      {/* Structure Stats */}
      <div className="bg-white rounded-lg border border-gray-200 p-4">
        <h4 className="text-sm font-semibold text-gray-700 mb-3 flex items-center gap-2">
          <Heading className="w-4 h-4" />
          Content Structure
        </h4>
        
        <div className="space-y-2">
          {/* Headings */}
          <div className="flex items-center justify-between text-sm">
            <span className="text-gray-600">Headings</span>
            <div className="flex items-center gap-2">
              <span className="px-2 py-0.5 bg-blue-100 text-blue-700 rounded text-xs">
                H2: {stats.headingCount.h2}
              </span>
              <span className="px-2 py-0.5 bg-purple-100 text-purple-700 rounded text-xs">
                H3: {stats.headingCount.h3}
              </span>
            </div>
          </div>

          {/* Links */}
          <div className="flex items-center justify-between text-sm">
            <span className="text-gray-600 flex items-center gap-1">
              <Link className="w-3 h-3" />
              Links
            </span>
            <div className="flex items-center gap-2">
              <span className="px-2 py-0.5 bg-emerald-100 text-emerald-700 rounded text-xs">
                Internal: {stats.linkCount.internal}
              </span>
              <span className="px-2 py-0.5 bg-orange-100 text-orange-700 rounded text-xs">
                External: {stats.linkCount.external}
              </span>
            </div>
          </div>

          {/* Images */}
          <div className="flex items-center justify-between text-sm">
            <span className="text-gray-600 flex items-center gap-1">
              <Image className="w-3 h-3" />
              Images
            </span>
            <span className="font-medium text-gray-800">{stats.imageCount}</span>
          </div>
        </div>
      </div>
    </div>
  );
};

export default ContentStats;
```

### `src/components/editor/sidebar/ReadingTime.tsx`

```tsx
'use client';

import React from 'react';
import { Clock, Volume2, BookOpen, Zap } from 'lucide-react';
import { TextStats } from '../utils/textAnalyzer';

interface ReadingTimeProps {
  stats: TextStats;
}

export const ReadingTime: React.FC<ReadingTimeProps> = ({ stats }) => {
  const { readingTime, speakingTime, wordCount } = stats;

  // Calculate progress for visual indicator
  const getReadingLevel = (words: number): { level: string; color: string; progress: number } => {
    if (words < 300) {
      return { level: 'Quick Read', color: 'text-green-600', progress: 25 };
    } else if (words < 1000) {
      return { level: 'Short Article', color: 'text-blue-600', progress: 50 };
    } else if (words < 2000) {
      return { level: 'Medium Article', color: 'text-purple-600', progress: 75 };
    } else {
      return { level: 'Long Form', color: 'text-orange-600', progress: 100 };
    }
  };

  const readingLevel = getReadingLevel(wordCount);

  return (
    <div className="space-y-4">
      {/* Main Reading Time Card */}
      <div className="bg-gradient-to-br from-blue-50 to-indigo-50 rounded-xl border border-blue-200 p-4">
        <div className="flex items-center justify-between mb-3">
          <div className="flex items-center gap-2">
            <div className="p-2 bg-blue-600 rounded-lg">
              <Clock className="w-4 h-4 text-white" />
            </div>
            <span className="text-sm font-medium text-blue-900">Reading Time</span>
          </div>
          <span className={`text-xs px-2 py-1 rounded-full font-medium ${
            readingLevel.color} bg-white/80`}>
            {readingLevel.level}
          </span>
        </div>

        <div className="text-3xl font-bold text-blue-900 mb-1">
          {readingTime.text}
        </div>
        
        <div className="text-sm text-blue-700">
          ~{readingTime.minutes > 0 ? `${readingTime.minutes}m ${readingTime.seconds}s` : `${readingTime.seconds}s`} at 200 wpm
        </div>

        {/* Progress Bar */}
        <div className="mt-3 h-2 bg-blue-200 rounded-full overflow-hidden">
          <div
            className="h-full bg-gradient-to-r from-blue-500 to-indigo-500 transition-all duration-500"
            style={{ width: `${Math.min(readingLevel.progress, 100)}%` }}
          />
        </div>
      </div>

      {/* Speaking Time Card */}
      <div className="bg-white rounded-lg border border-gray-200 p-4">
        <div className="flex items-center gap-2 mb-2">
          <div className="p-1.5 bg-purple-100 rounded">
            <Volume2 className="w-3.5 h-3.5 text-purple-600" />
          </div>
          <span className="text-sm font-medium text-gray-700">Speaking Time</span>
        </div>
        <div className="text-xl font-bold text-gray-800">
          {speakingTime.minutes > 0 
            ? `${speakingTime.minutes}m ${speakingTime.seconds}s`
            : `${speakingTime.seconds}s`
          }
        </div>
        <div className="text-xs text-gray-500 mt-1">
          At average speaking pace (130 wpm)
        </div>
      </div>

      {/* Quick Stats */}
      <div className="grid grid-cols-2 gap-3">
        <div className="bg-white rounded-lg border border-gray-200 p-3 text-center">
          <BookOpen className="w-4 h-4 text-emerald-600 mx-auto mb-1" />
          <div className="text-lg font-bold text-gray-800">
            {Math.ceil(wordCount / 250)}
          </div>
          <div className="text-xs text-gray-500">Pages (250 wpg)</div>
        </div>

        <div className="bg-white rounded-lg border border-gray-200 p-3 text-center">
          <Zap className="w-4 h-4 text-amber-600 mx-auto mb-1" />
          <div className="text-lg font-bold text-gray-800">
            {Math.round(wordCount / Math.max(stats.sentenceCount, 1))}
          </div>
          <div className="text-xs text-gray-500">Words/Sentence</div>
        </div>
      </div>
    </div>
  );
};

export default ReadingTime;
```

### `src/components/editor/sidebar/KeywordDensity.tsx`

```tsx
'use client';

import React, { useState, useMemo } from 'react';
import {
  Search,
  Target,
  TrendingUp,
  AlertTriangle,
  CheckCircle,
  XCircle,
  ChevronDown,
  ChevronUp,
  Sparkles,
  BarChart3,
} from 'lucide-react';
import {
  KeywordAnalysis,
  TopKeyword,
  calculateKeywordDensity,
  getTopKeywords,
  getTopPhrases,
  extractTextFromHTML,
} from '../utils/textAnalyzer';

interface KeywordDensityProps {
  html: string;
  focusKeyword: string;
  onFocusKeywordChange: (keyword: string) => void;
}

export const KeywordDensity: React.FC<KeywordDensityProps> = ({
  html,
  focusKeyword,
  onFocusKeywordChange,
}) => {
  const [showTopKeywords, setShowTopKeywords] = useState(true);
  const [showPhrases, setShowPhrases] = useState(false);

  const text = useMemo(() => extractTextFromHTML(html), [html]);

  const keywordAnalysis = useMemo(() => {
    if (!focusKeyword.trim()) return null;
    return calculateKeywordDensity(focusKeyword, text, html);
  }, [focusKeyword, text, html]);

  const topKeywords = useMemo(() => getTopKeywords(text, 8), [text]);
  const topPhrases = useMemo(() => getTopPhrases(text, 5), [text]);

  const getStatusIcon = (status: string) => {
    switch (status) {
      case 'optimal':
        return <CheckCircle className="w-4 h-4 text-green-500" />;
      case 'low':
        return <AlertTriangle className="w-4 h-4 text-amber-500" />;
      case 'high':
        return <AlertTriangle className="w-4 h-4 text-red-500" />;
      default:
        return <XCircle className="w-4 h-4 text-gray-400" />;
    }
  };

  const getStatusColor = (status: string) => {
    switch (status) {
      case 'optimal':
        return 'bg-green-100 border-green-300 text-green-800';
      case 'low':
        return 'bg-amber-100 border-amber-300 text-amber-800';
      case 'high':
        return 'bg-red-100 border-red-300 text-red-800';
      default:
        return 'bg-gray-100 border-gray-300 text-gray-600';
    }
  };

  const getDensityBarWidth = (density: number) => {
    // Optimal range is 1-3%
    const maxDensity = 5;
    return Math.min((density / maxDensity) * 100, 100);
  };

  const getDensityBarColor = (density: number) => {
    if (density === 0) return 'bg-gray-300';
    if (density < 0.5) return 'bg-amber-400';
    if (density <= 3) return 'bg-green-500';
    return 'bg-red-500';
  };

  return (
    <div className="space-y-4">
      {/* Focus Keyword Input */}
      <div className="bg-white rounded-lg border border-gray-200 p-4">
        <label className="block text-sm font-medium text-gray-700 mb-2">
          <div className="flex items-center gap-2">
            <Target className="w-4 h-4 text-blue-600" />
            Focus Keyword
          </div>
        </label>
        <div className="relative">
          <Search className="absolute left-3 top-1/2 -translate-y-1/2 w-4 h-4 text-gray-400" />
          <input
            type="text"
            value={focusKeyword}
            onChange={(e) => onFocusKeywordChange(e.target.value)}
            placeholder="Enter your target keyword..."
            className="w-full pl-10 pr-4 py-2 text-sm border border-gray-300 rounded-lg
                     focus:ring-2 focus:ring-blue-500 focus:border-transparent
                     placeholder:text-gray-400"
          />
        </div>
      </div>

      {/* Keyword Analysis Results */}
      {keywordAnalysis && focusKeyword.trim() && (
        <div className={`rounded-lg border p-4 ${getStatusColor(keywordAnalysis.status)}`}>
          <div className="flex items-center justify-between mb-3">
            <div className="flex items-center gap-2">
              {getStatusIcon(keywordAnalysis.status)}
              <span className="font-semibold text-sm">
                "{keywordAnalysis.keyword}"
              </span>
            </div>
            <span className="text-xs font-medium px-2 py-1 bg-white/50 rounded">
              {keywordAnalysis.status.toUpperCase()}
            </span>
          </div>

          {/* Density Meter */}
          <div className="mb-4">
            <div className="flex items-center justify-between text-sm mb-1">
              <span>Keyword Density</span>
              <span className="font-bold">{keywordAnalysis.density}%</span>
            </div>
            <div className="h-3 bg-white/50 rounded-full overflow-hidden">
              <div
                className={`h-full ${getDensityBarColor(keywordAnalysis.density)} transition-all duration-300`}
                style={{ width: `${getDensityBarWidth(keywordAnalysis.density)}%` }}
              />
            </div>
            <div className="flex justify-between text-xs mt-1 opacity-75">
              <span>0%</span>
              <span className="text-green-700">Optimal: 1-3%</span>
              <span>5%+</span>
            </div>
          </div>

          {/* Stats Grid */}
          <div className="grid grid-cols-2 gap-2 mb-3">
            <div className="bg-white/50 rounded p-2 text-center">
              <div className="text-lg font-bold">{keywordAnalysis.count}</div>
              <div className="text-xs opacity-75">Occurrences</div>
            </div>
            <div className="bg-white/50 rounded p-2 text-center">
              <div className="text-lg font-bold">{keywordAnalysis.prominence}%</div>
              <div className="text-xs opacity-75">Prominence</div>
            </div>
          </div>

          {/* Checklist */}
          <div className="space-y-2 text-sm">
            <div className="flex items-center gap-2">
              {keywordAnalysis.inTitle ? (
                <CheckCircle className="w-4 h-4 text-green-600" />
              ) : (
                <XCircle className="w-4 h-4 text-gray-400" />
              )}
              <span className={keywordAnalysis.inTitle ? 'text-green-800' : 'opacity-60'}>
                In title/H1
              </span>
            </div>
            <div className="flex items-center gap-2">
              {keywordAnalysis.inFirstParagraph ? (
                <CheckCircle className="w-4 h-4 text-green-600" />
              ) : (
                <XCircle className="w-4 h-4 text-gray-400" />
              )}
              <span className={keywordAnalysis.inFirstParagraph ? 'text-green-800' : 'opacity-60'}>
                In first paragraph
              </span>
            </div>
            <div className="flex items-center gap-2">
              {keywordAnalysis.inHeadings ? (
                <CheckCircle className="w-4 h-4 text-green-600" />
              ) : (
                <XCircle className="w-4 h-4 text-gray-400" />
              )}
              <span className={keywordAnalysis.inHeadings ? 'text-green-800' : 'opacity-60'}>
                In subheadings
              </span>
            </div>
            <div className="flex items-center gap-2">
              {keywordAnalysis.distribution === 'good' ? (
                <CheckCircle className="w-4 h-4 text-green-600" />
              ) : (
                <AlertTriangle className="w-4 h-4 text-amber-500" />
              )}
              <span>
                Distribution: <span className="font-medium">{keywordAnalysis.distribution}</span>
              </span>
            </div>
          </div>
        </div>
      )}

      {/* Top Keywords */}
      <div className="bg-white rounded-lg border border-gray-200 overflow-hidden">
        <button
          onClick={() => setShowTopKeywords(!showTopKeywords)}
          className="w-full flex items-center justify-between p-3 hover:bg-gray-50 transition-colors"
        >
          <div className="flex items-center gap-2">
            <BarChart3 className="w-4 h-4 text-purple-600" />
            <span className="text-sm font-medium text-gray-700">Top Keywords</span>
          </div>
          {showTopKeywords ? (
            <ChevronUp className="w-4 h-4 text-gray-400" />
          ) : (
            <ChevronDown className="w-4 h-4 text-gray-400" />
          )}
        </button>

        {showTopKeywords && (
          <div className="border-t border-gray-100 p-3">
            {topKeywords.length > 0 ? (
              <div className="space-y-2">
                {topKeywords.map((kw, index) => (
                  <div
                    key={kw.word}
                    className="flex items-center gap-2 group cursor-pointer"
                    onClick={() => onFocusKeywordChange(kw.word)}
                  >
                    <span className="w-5 h-5 flex items-center justify-center text-xs font-medium text-gray-400">
                      {index + 1}
                    </span>
                    <div className="flex-1 min-w-0">
                      <div className="flex items-center justify-between">
                        <span className="text-sm text-gray-700 truncate group-hover:text-blue-600 transition-colors">
                          {kw.word}
                        </span>
                        <span className="text-xs text-gray-500 ml-2">
                          {kw.count}× ({kw.density}%)
                        </span>
                      </div>
                      <div className="h-1 bg-gray-100 rounded-full mt-1 overflow-hidden">
                        <div
                          className="h-full bg-purple-400 rounded-full"
                          style={{
                            width: `${(kw.count / topKeywords[0].count) * 100}%`,
                          }}
                        />
                      </div>
                    </div>
                  </div>
                ))}
              </div>
            ) : (
              <p className="text-sm text-gray-500 text-center py-2">
                Start typing to see top keywords
              </p>
            )}
          </div>
        )}
      </div>

      {/* Top Phrases */}
      <div className="bg-white rounded-lg border border-gray-200 overflow-hidden">
        <button
          onClick={() => setShowPhrases(!showPhrases)}
          className="w-full flex items-center justify-between p-3 hover:bg-gray-50 transition-colors"
        >
          <div className="flex items-center gap-2">
            <Sparkles className="w-4 h-4 text-amber-600" />
            <span className="text-sm font-medium text-gray-700">Top Phrases</span>
          </div>
          {showPhrases ? (
            <ChevronUp className="w-4 h-4 text-gray-400" />
          ) : (
            <ChevronDown className="w-4 h-4 text-gray-400" />
          )}
        </button>

        {showPhrases && (
          <div className="border-t border-gray-100 p-3">
            {topPhrases.length > 0 ? (
              <div className="space-y-2">
                {topPhrases.map((phrase, index) => (
                  <div
                    key={phrase.word}
                    className="flex items-center justify-between text-sm cursor-pointer hover:bg-gray-50 p-1 rounded"
                    onClick={() => onFocusKeywordChange(phrase.word)}
                  >
                    <span className="text-gray-700">"{phrase.word}"</span>
                    <span className="text-gray-500">{phrase.count}×</span>
                  </div>
                ))}
              </div>
            ) : (
              <p className="text-sm text-gray-500 text-center py-2">
                Write more content to see phrases
              </p>
            )}
          </div>
        )}
      </div>
    </div>
  );
};

export default KeywordDensity;
```

### `src/components/editor/sidebar/StatsSidebar.tsx`

```tsx
'use client';

import React, { useState, useMemo, useEffect } from 'react';
import {
  BarChart3,
  Clock,
  Search,
  ChevronLeft,
  ChevronRight,
  Settings,
  X,
} from 'lucide-react';
import { getTextStats, TextStats } from '../utils/textAnalyzer';
import ContentStats from './ContentStats';
import ReadingTime from './ReadingTime';
import KeywordDensity from './KeywordDensity';

type TabType = 'stats' | 'reading' | 'keywords';

interface StatsSidebarProps {
  html: string;
  isOpen: boolean;
  onToggle: () => void;
}

export const StatsSidebar: React.FC<StatsSidebarProps> = ({
  html,
  isOpen,
  onToggle,
}) => {
  const [activeTab, setActiveTab] = useState<TabType>('stats');
  const [focusKeyword, setFocusKeyword] = useState('');

  // Calculate stats whenever HTML changes
  const stats = useMemo<TextStats>(() => {
    if (typeof window === 'undefined') {
      // Return default stats for SSR
      return {
        characterCount: 0,
        characterCountNoSpaces: 0,
        wordCount: 0,
        sentenceCount: 0,
        paragraphCount: 0,
        readingTime: { minutes: 0, seconds: 0, text: '0 min read' },
        speakingTime: { minutes: 0, seconds: 0, text: '0s speaking' },
        headingCount: { h1: 0, h2: 0, h3: 0, total: 0 },
        linkCount: { internal: 0, external: 0, total: 0 },
        imageCount: 0,
      };
    }
    return getTextStats(html);
  }, [html]);

  const tabs = [
    { id: 'stats' as TabType, label: 'Stats', icon: BarChart3 },
    { id: 'reading' as TabType, label: 'Reading', icon: Clock },
    { id: 'keywords' as TabType, label: 'Keywords', icon: Search },
  ];

  return (
    <>
      {/* Toggle Button (when sidebar is closed) */}
      {!isOpen && (
        <button
          onClick={onToggle}
          className="fixed right-0 top-1/2 -translate-y-1/2 z-40
                   bg-white border border-r-0 border-gray-200 rounded-l-lg
                   p-2 shadow-lg hover:bg-gray-50 transition-colors"
          title="Open Stats Sidebar"
        >
          <ChevronLeft className="w-5 h-5 text-gray-600" />
        </button>
      )}

      {/* Sidebar */}
      <div
        className={`
          fixed right-0 top-0 h-full bg-gray-50 border-l border-gray-200 
          shadow-xl z-50 transition-transform duration-300 ease-in-out
          ${isOpen ? 'translate-x-0' : 'translate-x-full'}
        `}
        style={{ width: '340px' }}
      >
        {/* Header */}
        <div className="flex items-center justify-between px-4 py-3 bg-white border-b border-gray-200">
          <h2 className="text-lg font-bold text-gray-800 flex items-center gap-2">
            <BarChart3 className="w-5 h-5 text-blue-600" />
            Content Analysis
          </h2>
          <button
            onClick={onToggle}
            className="p-1.5 text-gray-400 hover:text-gray-600 hover:bg-gray-100 rounded-lg transition-colors"
          >
            <X className="w-5 h-5" />
          </button>
        </div>

        {/* Quick Stats Bar */}
        <div className="px-4 py-2 bg-gradient-to-r from-blue-50 to-indigo-50 border-b border-blue-100">
          <div className="flex items-center justify-between text-sm">
            <span className="text-blue-700 font-medium">
              {stats.wordCount.toLocaleString()} words
            </span>
            <span className="text-blue-600">
              {stats.readingTime.text}
            </span>
          </div>
        </div>

        {/* Tabs */}
        <div className="flex border-b border-gray-200 bg-white">
          {tabs.map((tab) => (
            <button
              key={tab.id}
              onClick={() => setActiveTab(tab.id)}
              className={`
                flex-1 flex items-center justify-center gap-1.5 py-3 text-sm font-medium
                transition-colors border-b-2 -mb-px
                ${activeTab === tab.id
                  ? 'text-blue-600 border-blue-600 bg-blue-50/50'
                  : 'text-gray-500 border-transparent hover:text-gray-700 hover:bg-gray-50'
                }
              `}
            >
              <tab.icon className="w-4 h-4" />
              {tab.label}
            </button>
          ))}
        </div>

        {/* Tab Content */}
        <div className="flex-1 overflow-y-auto p-4" style={{ height: 'calc(100vh - 180px)' }}>
          {activeTab === 'stats' && <ContentStats stats={stats} />}
          {activeTab === 'reading' && <ReadingTime stats={stats} />}
          {activeTab === 'keywords' && (
            <KeywordDensity
              html={html}
              focusKeyword={focusKeyword}
              onFocusKeywordChange={setFocusKeyword}
            />
          )}
        </div>

        {/* Footer */}
        <div className="absolute bottom-0 left-0 right-0 px-4 py-2 bg-white border-t border-gray-200">
          <div className="text-xs text-gray-400 text-center">
            Stats update as you type
          </div>
        </div>
      </div>
    </>
  );
};

export default StatsSidebar;
```

### `src/components/editor/sidebar/index.ts`

```typescript
export { StatsSidebar } from './StatsSidebar';
export { ContentStats } from './ContentStats';
export { ReadingTime } from './ReadingTime';
export { KeywordDensity } from './KeywordDensity';
```

---

## 3. Editor With Sidebar Component

### `src/components/editor/EditorWithSidebar.tsx`

```tsx
'use client';

import React, { useState, useCallback, useEffect } from 'react';
import { useEditor, EditorContent } from '@tiptap/react';
import StarterKit from '@tiptap/starter-kit';
import Placeholder from '@tiptap/extension-placeholder';
import MenuBar from './MenuBar';
import EditorBubbleMenu from './EditorBubbleMenu';
import { ExportModal } from './export';
import { StatsSidebar } from './sidebar';

// Custom Extensions
import { CallToAction } from './extensions/CallToAction/CallToAction';
import { FaqBlock } from './extensions/FaqBlock/FaqBlock';
import { ProsConsBlock } from './extensions/ProsConsBlock/ProsConsBlock';

interface EditorWithSidebarProps {
  initialContent?: string;
  onChange?: (html: string, json: object) => void;
  placeholder?: string;
}

export const EditorWithSidebar: React.FC<EditorWithSidebarProps> = ({
  initialContent = '',
  onChange,
  placeholder = "Start typing your content...",
}) => {
  const [isExportModalOpen, setIsExportModalOpen] = useState(false);
  const [isSidebarOpen, setIsSidebarOpen] = useState(true);
  const [currentHtml, setCurrentHtml] = useState(initialContent);

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
      const html = editor.getHTML();
      const json = editor.getJSON();
      setCurrentHtml(html);
      if (onChange) {
        onChange(html, json);
      }
    },
    immediatelyRender: false,
  });

  // Update currentHtml when initialContent changes
  useEffect(() => {
    if (editor && initialContent) {
      setCurrentHtml(editor.getHTML());
    }
  }, [editor, initialContent]);

  const handleClearContent = useCallback(() => {
    if (editor) {
      editor.commands.clearContent();
      editor.commands.focus();
    }
  }, [editor]);

  const toggleSidebar = useCallback(() => {
    setIsSidebarOpen((prev) => !prev);
  }, []);

  return (
    <div className="flex min-h-screen bg-gray-100">
      {/* Main Editor Area */}
      <div
        className={`
          flex-1 transition-all duration-300
          ${isSidebarOpen ? 'mr-[340px]' : 'mr-0'}
        `}
      >
        <div className="max-w-4xl mx-auto py-8 px-4">
          {/* Header */}
          <div className="mb-6 text-center">
            <h1 className="text-3xl font-bold text-gray-800 mb-2">
              📝 Notion-Style Editor
            </h1>
            <p className="text-gray-500">
              With Real-Time Analytics Sidebar
            </p>
          </div>

          {/* Editor Card */}
          <div className="bg-white rounded-xl border border-gray-200 shadow-lg overflow-hidden">
            <MenuBar editor={editor} />

            <div className="p-8 min-h-[500px]">
              {editor && <EditorBubbleMenu editor={editor} />}
              <EditorContent editor={editor} />
            </div>

            {/* Footer */}
            <div className="border-t border-gray-200 px-4 py-3 bg-gray-50 flex items-center justify-between">
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

              <div className="flex gap-2">
                <button
                  onClick={handleClearContent}
                  className="px-3 py-1.5 text-sm text-gray-600 hover:text-gray-800 
                           hover:bg-gray-200 rounded-lg transition-colors"
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
        </div>
      </div>

      {/* Stats Sidebar */}
      <StatsSidebar
        html={currentHtml}
        isOpen={isSidebarOpen}
        onToggle={toggleSidebar}
      />

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

export default EditorWithSidebar;
```

---

## 4. Updated Page Component

### `src/app/page.tsx`

```tsx
'use client';

import { useState } from 'react';
import EditorWithSidebar from '@/components/editor/EditorWithSidebar';

export default function Home() {
  const handleEditorChange = (html: string, json: object) => {
    // You can handle changes here (e.g., auto-save)
    console.log('Content updated');
  };

  const sampleContent = `
    <h2>Welcome to the Notion-Style Editor</h2>
    <p>This editor now includes a <strong>real-time analytics sidebar</strong> that tracks your content as you type.</p>
    
    <h3>Features Available</h3>
    <ul>
      <li>📊 Word, character, and sentence counts</li>
      <li>⏱️ Reading and speaking time estimates</li>
      <li>🔍 Keyword density analysis with SEO tips</li>
      <li>📈 Top keywords and phrases detection</li>
    </ul>
    
    <p>Try typing more content and watch the sidebar update in real-time. You can also enter a focus keyword to analyze its density and distribution throughout your content.</p>
    
    <h3>Tips for Better SEO</h3>
    <p>Keep your keyword density between 1-3% for optimal results. Make sure to include your focus keyword in the title, first paragraph, and at least one subheading.</p>
    
    <blockquote>
      <p>Great content is not just about keywords—it's about providing value to your readers while being mindful of SEO best practices.</p>
    </blockquote>
    
    <p>The sidebar automatically detects the most frequently used words and phrases in your content, helping you identify unintentional keyword stuffing or opportunities for better optimization.</p>
  `;

  return (
    <main className="min-h-screen">
      <EditorWithSidebar
        initialContent={sampleContent}
        onChange={handleEditorChange}
        placeholder="Start typing here..."
      />
    </main>
  );
}
```

---

## 5. Additional Styles

Add to `src/app/globals.css`:

```css
/* Sidebar Scrollbar Styles */
@layer utilities {
  .sidebar-scroll {
    scrollbar-width: thin;
    scrollbar-color: #cbd5e1 transparent;
  }

  .sidebar-scroll::-webkit-scrollbar {
    width: 6px;
  }

  .sidebar-scroll::-webkit-scrollbar-track {
    background: transparent;
  }

  .sidebar-scroll::-webkit-scrollbar-thumb {
    background-color: #cbd5e1;
    border-radius: 3px;
  }

  .sidebar-scroll::-webkit-scrollbar-thumb:hover {
    background-color: #94a3b8;
  }
}

/* Stat Card Animations */
@layer components {
  .stat-card {
    @apply transition-all duration-200;
  }

  .stat-card:hover {
    @apply shadow-md -translate-y-0.5;
  }
}

/* Keyword Badge Animations */
@keyframes pulseKeyword {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.7; }
}

.keyword-pulse {
  animation: pulseKeyword 2s ease-in-out infinite;
}

/* Density Bar Animation */
@keyframes growWidth {
  from { width: 0; }
}

.density-bar-animate {
  animation: growWidth 0.5s ease-out;
}
```

---

## Summary of Features

### Real-Time Statistics
| Metric | Description |
|--------|-------------|
| **Characters** | Total & without spaces |
| **Words** | Total word count |
| **Sentences** | Sentence count |
| **Paragraphs** | Paragraph count |
| **Reading Time** | Based on 200 WPM average |
| **Speaking Time** | Based on 130 WPM average |

### Keyword Density Analysis
| Feature | Description |
|---------|-------------|
| **Focus Keyword** | Set your target keyword |
| **Density %** | Percentage of keyword usage |
| **Prominence** | Position-based score |
| **Distribution** | Good/Front-heavy/Back-heavy/Poor |
| **SEO Checklist** | In title, first paragraph, headings |
| **Top Keywords** | Auto-detected frequent words |
| **Top Phrases** | Two-word phrase analysis |

### Status Indicators
| Status | Density Range | Color |
|--------|---------------|-------|
| Missing | 0% | Gray |
| Low | < 0.5% | Amber |
| Optimal | 0.5% - 3% | Green |
| High | > 3% | Red |

All stats update instantly as you type! 🚀
