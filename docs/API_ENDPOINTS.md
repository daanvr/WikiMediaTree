# API Endpoints Documentation

## Overview
This document describes all external API endpoints used by WikiMediaTree for fetching Commons category and Wikidata item information, including hierarchical relationships and metadata.

## API Guidelines
- All requests must handle CORS appropriately using `origin=*` parameter
- Include comprehensive error handling for all endpoints
- Implement rate limiting to respect API usage policies
- Cache responses when appropriate to improve performance
- Use HTTPS for all API requests

## Wikimedia Commons API

### Base URL
`https://commons.wikimedia.org/w/api.php`

### Get Category Information
Retrieve basic information about a Commons category including file count and subcategories.

```javascript
const getCategoryInfo = async (categoryName) => {
  const params = {
    action: 'query',
    format: 'json',
    prop: 'categoryinfo',
    titles: `Category:${categoryName}`,
    origin: '*'
  };
  
  const url = `https://commons.wikimedia.org/w/api.php?${new URLSearchParams(params)}`;
  const response = await fetch(url);
  return response.json();
};
```

**Parameters:**
- `action`: `query`
- `prop`: `categoryinfo`
- `titles`: Category name with "Category:" prefix
- `format`: `json`
- `origin`: `*` (for CORS)

**Response Format:**
```json
{
  "query": {
    "pages": {
      "12345": {
        "pageid": 12345,
        "title": "Category:Example",
        "categoryinfo": {
          "size": 42,
          "pages": 35,
          "files": 7,
          "subcats": 8
        }
      }
    }
  }
}
```

---

### Get Category Members
Retrieve subcategories and files within a specific category.

```javascript
const getCategoryMembers = async (categoryName, type = 'subcat|file') => {
  const params = {
    action: 'query',
    format: 'json',
    list: 'categorymembers',
    cmtitle: `Category:${categoryName}`,
    cmtype: type,
    cmlimit: 'max',
    origin: '*'
  };
  
  const url = `https://commons.wikimedia.org/w/api.php?${new URLSearchParams(params)}`;
  const response = await fetch(url);
  return response.json();
};
```

**Parameters:**
- `action`: `query`
- `list`: `categorymembers`
- `cmtitle`: Category name with "Category:" prefix
- `cmtype`: `subcat` (subcategories), `file` (files), or `subcat|file` (both)
- `cmlimit`: Number of results (`max` for maximum allowed)
- `origin`: `*` (for CORS)

---

### Get Parent Categories
Retrieve parent categories for a given category.

```javascript
const getParentCategories = async (categoryName) => {
  const params = {
    action: 'query',
    format: 'json',
    prop: 'categories',
    titles: `Category:${categoryName}`,
    cllimit: 'max',
    origin: '*'
  };
  
  const url = `https://commons.wikimedia.org/w/api.php?${new URLSearchParams(params)}`;
  const response = await fetch(url);
  return response.json();
};
```

---

### Search Categories
Search for categories by name or pattern.

```javascript
const searchCategories = async (searchTerm) => {
  const params = {
    action: 'query',
    format: 'json',
    list: 'search',
    srsearch: `incategory:"${searchTerm}"`,
    srnamespace: 14, // Category namespace
    srlimit: 50,
    origin: '*'
  };
  
  const url = `https://commons.wikimedia.org/w/api.php?${new URLSearchParams(params)}`;
  const response = await fetch(url);
  return response.json();
};
```

---

## Wikidata Query Service (SPARQL)

### Base URL
`https://query.wikidata.org/sparql`

### Get Item Basic Information
Retrieve basic properties for a Wikidata item.

```javascript
const getWikidataItem = async (itemId) => {
  const query = `
    SELECT ?item ?itemLabel ?itemDescription ?instanceOf ?instanceOfLabel ?image WHERE {
      VALUES ?item { wd:${itemId} }
      OPTIONAL { ?item wdt:P31 ?instanceOf }
      OPTIONAL { ?item wdt:P18 ?image }
      SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
    }
  `;
  
  const url = `https://query.wikidata.org/sparql?query=${encodeURIComponent(query)}&format=json`;
  const response = await fetch(url);
  return response.json();
};
```

**Response Format:**
```json
{
  "results": {
    "bindings": [
      {
        "item": {"type": "uri", "value": "http://www.wikidata.org/entity/Q12345"},
        "itemLabel": {"type": "literal", "value": "Example Item"},
        "itemDescription": {"type": "literal", "value": "An example item"},
        "instanceOf": {"type": "uri", "value": "http://www.wikidata.org/entity/Q54321"},
        "instanceOfLabel": {"type": "literal", "value": "example class"},
        "image": {"type": "uri", "value": "http://commons.wikimedia.org/wiki/Special:FilePath/Example.jpg"}
      }
    ]
  }
}
```

---

### Get Item Hierarchy (Subclass Relations)
Retrieve parent and child items based on subclass relationships.

```javascript
const getItemHierarchy = async (itemId) => {
  const query = `
    SELECT ?parent ?parentLabel ?child ?childLabel WHERE {
      {
        # Get parents
        wd:${itemId} wdt:P279+ ?parent .
      } UNION {
        # Get children
        ?child wdt:P279+ wd:${itemId} .
      }
      SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
    }
    LIMIT 100
  `;
  
  const url = `https://query.wikidata.org/sparql?query=${encodeURIComponent(query)}&format=json`;
  const response = await fetch(url);
  return response.json();
};
```

---

### Find Commons Category Link
Find the corresponding Commons category for a Wikidata item.

```javascript
const findCommonsCategory = async (itemId) => {
  const query = `
    SELECT ?commonsCategory WHERE {
      wd:${itemId} wdt:P373 ?commonsCategory .
    }
  `;
  
  const url = `https://query.wikidata.org/sparql?query=${encodeURIComponent(query)}&format=json`;
  const response = await fetch(url);
  return response.json();
};
```

---

### Search Wikidata Items
Search for Wikidata items by label.

```javascript
const searchWikidataItems = async (searchTerm) => {
  const query = `
    SELECT ?item ?itemLabel ?itemDescription WHERE {
      ?item rdfs:label ?itemLabel .
      ?item schema:description ?itemDescription .
      FILTER(LANG(?itemLabel) = "en")
      FILTER(LANG(?itemDescription) = "en")
      FILTER(CONTAINS(LCASE(?itemLabel), LCASE("${searchTerm}")))
    }
    LIMIT 50
  `;
  
  const url = `https://query.wikidata.org/sparql?query=${encodeURIComponent(query)}&format=json`;
  const response = await fetch(url);
  return response.json();
};
```

---

## MediaWiki API (General)

### Base URL
`https://www.wikidata.org/w/api.php` (for Wikidata)
`https://commons.wikimedia.org/w/api.php` (for Commons)

### Get Page Information
Retrieve basic page information for categories or items.

```javascript
const getPageInfo = async (title, baseUrl) => {
  const params = {
    action: 'query',
    format: 'json',
    prop: 'info|extracts',
    titles: title,
    exintro: true,
    explaintext: true,
    origin: '*'
  };
  
  const url = `${baseUrl}?${new URLSearchParams(params)}`;
  const response = await fetch(url);
  return response.json();
};
```

---

## Data Mapping and Correlation

### Find Category-Item Relationships
Combine Commons and Wikidata APIs to find corresponding entities.

```javascript
const findCorrespondingEntities = async (categoryName) => {
  // First, search for Wikidata items with matching Commons category
  const wikidataQuery = `
    SELECT ?item ?itemLabel WHERE {
      ?item wdt:P373 "${categoryName}" .
      SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
    }
  `;
  
  const wikidataUrl = `https://query.wikidata.org/sparql?query=${encodeURIComponent(wikidataQuery)}&format=json`;
  const wikidataResponse = await fetch(wikidataUrl);
  const wikidataData = await wikidataResponse.json();
  
  // Also get Commons category information
  const commonsData = await getCategoryInfo(categoryName);
  
  return {
    commons: commonsData,
    wikidata: wikidataData
  };
};
```

---

## Error Handling

### Common Error Responses
```javascript
class WikiMediaAPIError extends Error {
  constructor(message, status, endpoint) {
    super(message);
    this.name = 'WikiMediaAPIError';
    this.status = status;
    this.endpoint = endpoint;
  }
}

const handleApiError = (response, endpoint) => {
  if (!response.ok) {
    throw new WikiMediaAPIError(
      `API request failed: ${response.statusText}`,
      response.status,
      endpoint
    );
  }
  return response.json();
};
```

### Rate Limiting
```javascript
class RateLimiter {
  constructor(requestsPerSecond = 5) {
    this.requestsPerSecond = requestsPerSecond;
    this.requests = [];
  }
  
  async throttle() {
    const now = Date.now();
    this.requests = this.requests.filter(time => now - time < 1000);
    
    if (this.requests.length >= this.requestsPerSecond) {
      const oldestRequest = Math.min(...this.requests);
      const waitTime = 1000 - (now - oldestRequest);
      await new Promise(resolve => setTimeout(resolve, waitTime));
    }
    
    this.requests.push(now);
  }
}

const rateLimiter = new RateLimiter(5); // 5 requests per second
```

### Retry Logic
```javascript
const makeApiRequest = async (url, maxRetries = 3) => {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      await rateLimiter.throttle();
      
      const response = await fetch(url);
      if (!response.ok) {
        if (response.status === 429 && attempt < maxRetries) {
          // Rate limited, wait longer
          await new Promise(resolve => setTimeout(resolve, Math.pow(2, attempt) * 1000));
          continue;
        }
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      return await response.json();
    } catch (error) {
      if (attempt === maxRetries) {
        throw error;
      }
      // Exponential backoff
      await new Promise(resolve => setTimeout(resolve, Math.pow(2, attempt) * 1000));
    }
  }
};
```

## Caching Strategy

### Response Caching
```javascript
class APICache {
  constructor(ttl = 300000) { // 5 minutes default TTL
    this.cache = new Map();
    this.ttl = ttl;
  }
  
  set(key, value) {
    this.cache.set(key, {
      value,
      timestamp: Date.now()
    });
  }
  
  get(key) {
    const item = this.cache.get(key);
    if (!item) return null;
    
    if (Date.now() - item.timestamp > this.ttl) {
      this.cache.delete(key);
      return null;
    }
    
    return item.value;
  }
  
  clear() {
    this.cache.clear();
  }
}

const apiCache = new APICache();
```

## Usage Examples

### Complete Block Data Fetching
```javascript
const fetchBlockData = async (identifier, type) => {
  try {
    let blockData = {};
    
    if (type === 'commons' || type === 'hybrid') {
      // Fetch Commons category data
      const categoryInfo = await getCategoryInfo(identifier);
      const parentCategories = await getParentCategories(identifier);
      const categoryMembers = await getCategoryMembers(identifier);
      
      blockData.commons = {
        info: categoryInfo,
        parents: parentCategories,
        members: categoryMembers
      };
    }
    
    if (type === 'wikidata' || type === 'hybrid') {
      // Fetch Wikidata item data
      const itemInfo = await getWikidataItem(identifier);
      const itemHierarchy = await getItemHierarchy(identifier);
      
      blockData.wikidata = {
        info: itemInfo,
        hierarchy: itemHierarchy
      };
    }
    
    return blockData;
  } catch (error) {
    console.error(`Failed to fetch block data for ${identifier}:`, error);
    throw error;
  }
};
```

---
*This document should be updated as new API endpoints are integrated*