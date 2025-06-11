# Claude Tokenizer Project Overview

## Project Description
This is a Next.js web application that provides a tokenizer interface for Claude 3.5 Sonnet. It uses Anthropic's official token counting API to accurately count tokens in user-provided text input.

## Architecture Overview

### Frontend (Next.js App Router)
- **Main Page**: `app/page.tsx` - Simple landing page with title and description
- **Components**: `components/tokenComponents.tsx` - Core tokenizer functionality
- **UI Components**: Uses shadcn/ui components (Textarea, Button, Badge)

### Backend API
- **API Route**: `app/api/route.ts` - Handles token counting requests to Anthropic's API

## How the Tokenizer Works

### Current Implementation
1. **User Input**: User types text into a textarea component
2. **Debounced API Calls**: Input is debounced (200ms) to avoid excessive API calls
3. **Token Counting**: Text is sent to `/api` endpoint which calls Anthropic's beta token counting API
4. **Display Results**: Shows token count and character count in real-time

### Key Technical Details

#### API Integration (`app/api/route.ts`)
- Uses `@anthropic-ai/sdk` package
- Calls `anthropic.beta.messages.countTokens()` with:
  - Model: `claude-3-5-sonnet-20241022`
  - Beta feature: `token-counting-2024-11-01`
  - Input as user message content
- Returns raw token count data

#### Frontend Logic (`components/tokenComponents.tsx`)
- **State Management**: 
  - `text`: Current input text
  - `stats`: Object containing token count and character count
  - `error`: Error state for API failures
- **Token Adjustment**: Subtracts 7 tokens from API response (likely accounting for message structure overhead)
- **Real-time Updates**: Uses useEffect with debounced API calls
- **Error Handling**: Graceful error handling with user feedback

#### Current Display
- Shows only final numbers:
  - Token count (large display)
  - Character count (large display)
- No step-by-step tokenization visualization

## Limitations for Graphical Interface Enhancement

### Current State
- Only shows final token count numbers
- No visibility into individual tokens
- No step-by-step tokenization process
- No token boundaries or segmentation

### API Limitations
The current Anthropic token counting API (`anthropic.beta.messages.countTokens()`) only returns:
- `input_tokens`: Total number of input tokens
- Does NOT return individual tokens or token boundaries

### Potential Enhancement Approaches

#### Option 1: Use Anthropic's Tokenizer Library
- Anthropic has released tokenizer libraries that can show individual tokens
- Would need to integrate client-side tokenization
- Could show token boundaries and individual token visualization

#### Option 2: Hybrid Approach
- Use current API for accurate count
- Use client-side tokenizer for visualization
- Compare results for accuracy validation

#### Option 3: Token Highlighting
- Implement approximate token boundary detection
- Highlight text segments that correspond to tokens
- Show progressive tokenization as user types

## Key Files for Enhancement
- `components/tokenComponents.tsx:78-89` - TokenMetrics component (displays results)
- `components/tokenComponents.tsx:18-47` - handleAnalyze function (API calls)
- `app/api/route.ts:14-21` - Token counting API call

## Environment Requirements
- `ANTHROPIC_API_KEY` environment variable required
- Next.js 14+ with App Router
- Tailwind CSS for styling
- shadcn/ui components

## Current User Flow
1. User enters text in textarea
2. After 200ms delay, API call is made
3. Token count is calculated and displayed
4. Character count is updated in real-time
5. Results shown as large numbers below input