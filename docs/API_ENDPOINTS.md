# API Endpoints Documentation

## Overview
This document describes all external API endpoints used by WikiMediaTree and how to interact with them.

## API Guidelines
- All requests should handle CORS appropriately
- Include proper error handling for all endpoints
- Use appropriate HTTP methods
- Handle rate limiting

## Wikipedia/Wikimedia APIs

### MediaWiki API
**Base URL:** `https://en.wikipedia.org/w/api.php`

#### Search Articles
```javascript
// Example usage
const searchArticles = async (query) => {
  const url = `https://en.wikipedia.org/w/api.php?action=query&list=search&srsearch=${encodeURIComponent(query)}&format=json&origin=*`;
  const response = await fetch(url);
  return response.json();
};
```

**Parameters:**
- `action`: `query`
- `list`: `search`
- `srsearch`: Search term
- `format`: `json`
- `origin`: `*` (for CORS)

**Response Format:**
```json
{
  "query": {
    "search": [
      {
        "title": "Article Title",
        "snippet": "Article snippet...",
        "size": 12345,
        "timestamp": "2023-01-01T00:00:00Z"
      }
    ]
  }
}
```

---

### Wikimedia Commons API
**Base URL:** `https://commons.wikimedia.org/w/api.php`

#### Get Media Files
<!-- Add endpoint details -->

---

## Error Handling

### Common Error Responses
```javascript
// Standard error handling pattern
const handleApiError = (response) => {
  if (!response.ok) {
    throw new Error(`API request failed: ${response.status}`);
  }
  return response.json();
};
```

### Rate Limiting
- Implement request throttling
- Handle 429 status codes
- Use exponential backoff for retries

## Usage Examples

### Complete Request Pattern
```javascript
const makeApiRequest = async (endpoint, params) => {
  try {
    const url = new URL(endpoint);
    Object.keys(params).forEach(key => 
      url.searchParams.append(key, params[key])
    );
    
    const response = await fetch(url.toString());
    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    console.error('API request failed:', error);
    throw error;
  }
};
```

---
*This document should be updated as new API endpoints are integrated*