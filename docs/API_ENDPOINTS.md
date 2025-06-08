# API Endpoints Documentation

## Overview
This document describes all external API endpoints used by WikiMediaTree for fetching Commons category and Wikidata item information, including hierarchical relationships and metadata. **Critical**: The tool is built on the flexible assumption that most Commons categories have Wikidata item equivalents, but this is not one-to-one true - the API integration must handle categories without items and items without categories gracefully.

## API Guidelines
- All requests must handle CORS appropriately using `origin=*` parameter
- Include comprehensive error handling for all endpoints
- Implement rate limiting to respect API usage policies
- **Cache responses when reasonable and size is not too big** (as per exploration-first requirements)
- **Show clear indication to users when data is partial, stale, or cached**
- **API activity must be clearly visible to users** through status indicators
- Use HTTPS for all API requests
- **Graceful degradation**: Work with cached data when APIs are slow or down

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

## Flexible Relationship Handling

### Core Principle
The tool operates on the **flexible assumption** that most Commons categories have Wikidata item equivalents, but this is **not one-to-one true**. The API integration must gracefully handle:
- Commons categories without corresponding Wikidata items
- Wikidata items without corresponding Commons categories  
- Cases where both exist but may not be properly linked
- Hierarchical mismatches between Commons and Wikidata structures

### Find Category-Item Relationships
Combine Commons and Wikidata APIs to find corresponding entities, handling missing relationships gracefully.

```javascript
const findCorrespondingEntities = async (categoryName) => {
  try {
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
    
    // Check for missing relationships
    const hasWikidataItem = wikidataData.results.bindings.length > 0;
    const hasCommonsCategory = commonsData.query && Object.keys(commonsData.query.pages).length > 0;
    
    return {
      commons: commonsData,
      wikidata: wikidataData,
      relationship: {
        hasCommonsCategory,
        hasWikidataItem,
        type: hasCommonsCategory && hasWikidataItem ? 'hybrid' : 
              hasCommonsCategory ? 'commons' : 
              hasWikidataItem ? 'wikidata' : 'none',
        mismatch: hasCommonsCategory !== hasWikidataItem
      }
    };
  } catch (error) {
    console.error(`Error finding relationships for ${categoryName}:`, error);
    // Return partial data even if one API fails
    return {
      commons: null,
      wikidata: null,
      relationship: { type: 'error', mismatch: true },
      error: error.message
    };
  }
};
```

### Handle Missing Wikidata Items
When Commons categories exist but no corresponding Wikidata item is found.

```javascript
const handleMissingWikidataItem = async (categoryName) => {
  // Get Commons category info
  const commonsData = await getCategoryInfo(categoryName);
  
  // Search for potential Wikidata items with similar names
  const searchQuery = `
    SELECT ?item ?itemLabel ?itemDescription WHERE {
      ?item rdfs:label ?itemLabel .
      FILTER(LANG(?itemLabel) = "en")
      FILTER(CONTAINS(LCASE(?itemLabel), LCASE("${categoryName.replace('Category:', '')}")))
      OPTIONAL { ?item schema:description ?itemDescription . FILTER(LANG(?itemDescription) = "en") }
    }
    LIMIT 10
  `;
  
  const searchUrl = `https://query.wikidata.org/sparql?query=${encodeURIComponent(searchQuery)}&format=json`;
  const searchResponse = await fetch(searchUrl);
  const potentialMatches = await searchResponse.json();
  
  return {
    commonsData,
    wikidataItem: null,
    potentialMatches: potentialMatches.results.bindings,
    status: 'commons_only',
    suggestion: 'Consider creating Wikidata item or linking to existing item'
  };
};
```

### Handle Missing Commons Categories  
When Wikidata items exist but no corresponding Commons category is found.

```javascript
const handleMissingCommonsCategory = async (wikidataItemId) => {
  // Get Wikidata item info
  const wikidataData = await getWikidataItem(wikidataItemId);
  
  // Check if item should have a Commons category
  const categoryCheckQuery = `
    SELECT ?item ?itemLabel ?instanceOf ?instanceOfLabel WHERE {
      VALUES ?item { wd:${wikidataItemId} }
      OPTIONAL { ?item wdt:P31 ?instanceOf }
      SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE],en". }
    }
  `;
  
  const checkUrl = `https://query.wikidata.org/sparql?query=${encodeURIComponent(categoryCheckQuery)}&format=json`;
  const checkResponse = await fetch(checkUrl);
  const itemDetails = await checkResponse.json();
  
  return {
    wikidataData,
    commonsCategory: null,
    itemDetails: itemDetails.results.bindings[0],
    status: 'wikidata_only',
    suggestion: 'Consider creating Commons category if appropriate for this item type'
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

## API Status Indicators

### Status Indicator Requirements
Based on exploration-first approach, users need clear visibility into API activity:

```javascript
class APIStatusManager {
  constructor() {
    this.currentRequests = new Set();
    this.cache = new Map();
    this.statusElement = null; // DOM element for status display
  }
  
  startRequest(requestId, endpoint) {
    this.currentRequests.add(requestId);
    this.updateStatus(`Loading ${endpoint}...`, 'loading');
  }
  
  completeRequest(requestId, cached = false) {
    this.currentRequests.delete(requestId);
    if (this.currentRequests.size === 0) {
      this.updateStatus(cached ? 'Showing cached data' : 'Data loaded', 'complete');
    }
  }
  
  handleError(requestId, error) {
    this.currentRequests.delete(requestId);
    this.updateStatus(`API Error: ${error.message}`, 'error');
  }
  
  updateStatus(message, type) {
    // Update status indicator in corner of canvas or underneath
    if (this.statusElement) {
      this.statusElement.textContent = message;
      this.statusElement.className = `api-status api-status--${type}`;
    }
  }
}
```

## Caching Strategy with User Transparency

### Response Caching with Status Indicators
```javascript
class APICache {
  constructor(ttl = 300000, maxSize = 50) { // 5 minutes TTL, reasonable size limit
    this.cache = new Map();
    this.ttl = ttl;
    this.maxSize = maxSize;
    this.statusManager = new APIStatusManager();
  }
  
  set(key, value) {
    // Respect size limits from Q&A #25
    if (this.cache.size >= this.maxSize) {
      const firstKey = this.cache.keys().next().value;
      this.cache.delete(firstKey);
    }
    
    this.cache.set(key, {
      value,
      timestamp: Date.now(),
      cached: true
    });
  }
  
  get(key) {
    const item = this.cache.get(key);
    if (!item) return null;
    
    const isStale = Date.now() - item.timestamp > this.ttl;
    
    if (isStale) {
      // Keep stale data but indicate it's stale
      item.stale = true;
      this.statusManager.updateStatus('Showing stale data', 'stale');
    } else if (item.cached) {
      this.statusManager.updateStatus('Showing cached data', 'cached');
    }
    
    return item;
  }
  
  clear() {
    this.cache.clear();
  }
}

const apiCache = new APICache();
```

## External Link Generation

### Missing Entity Creation Links
Based on Q&A #15, when entities are missing, provide direct links to creation pages with URL parameters:

```javascript
class ExternalLinkGenerator {
  
  static generateCommonsCreationLink(parentCategory = null, suggestedName = null) {
    const baseUrl = 'https://commons.wikimedia.org/wiki/Special:CreatePage';
    const params = new URLSearchParams();
    
    if (parentCategory) {
      params.append('parent', parentCategory);
    }
    if (suggestedName) {
      params.append('title', `Category:${suggestedName}`);
    }
    
    return `${baseUrl}?${params.toString()}`;
  }
  
  static generateWikidataCreationLink(instanceOf = null, label = null) {
    const baseUrl = 'https://www.wikidata.org/wiki/Special:NewItem';
    const params = new URLSearchParams();
    
    if (instanceOf) {
      params.append('instanceOf', instanceOf);
    }
    if (label) {
      params.append('label', label);
    }
    
    return `${baseUrl}?${params.toString()}`;
  }
  
  static generateDirectLinks(blockData) {
    const links = {
      commons: null,
      wikidata: null,
      createCommons: null,
      createWikidata: null
    };
    
    // Direct links to existing pages
    if (blockData.commonsData) {
      links.commons = blockData.commonsData.url;
    }
    if (blockData.wikidataData) {
      links.wikidata = blockData.wikidataData.url;
    }
    
    // Creation links for missing entities
    if (!blockData.commonsData && blockData.wikidataData) {
      links.createCommons = this.generateCommonsCreationLink(
        blockData.relationships?.parentId, 
        blockData.wikidataData.label
      );
    }
    if (!blockData.wikidataData && blockData.commonsData) {
      links.createWikidata = this.generateWikidataCreationLink(
        null, // Would need to determine appropriate instance of
        blockData.commonsData.title.replace('Category:', '')
      );
    }
    
    return links;
  }
}
```

## Usage Examples

### Complete Block Data Fetching with API Status Integration
```javascript
const fetchBlockData = async (identifier, initialType = 'unknown') => {
  try {
    let blockData = {
      id: identifier,
      type: 'unknown',
      commons: null,
      wikidata: null,
      relationship: { mismatch: false }
    };
    
    // First, determine what we're dealing with and find corresponding entities
    if (identifier.startsWith('Category:') || initialType === 'commons') {
      // Starting with Commons category
      const relationshipData = await findCorrespondingEntities(identifier);
      blockData.commons = relationshipData.commons;
      blockData.wikidata = relationshipData.wikidata;
      blockData.relationship = relationshipData.relationship;
      blockData.type = relationshipData.relationship.type;
      
      // Get detailed Commons data if available
      if (relationshipData.relationship.hasCommonsCategory) {
        const parentCategories = await getParentCategories(identifier);
        const categoryMembers = await getCategoryMembers(identifier);
        
        blockData.commons.parents = parentCategories;
        blockData.commons.members = categoryMembers;
      }
      
      // Get detailed Wikidata data if corresponding item found
      if (relationshipData.relationship.hasWikidataItem) {
        const wikidataItem = relationshipData.wikidata.results.bindings[0];
        const itemId = wikidataItem.item.value.split('/').pop();
        const itemHierarchy = await getItemHierarchy(itemId);
        
        blockData.wikidata.hierarchy = itemHierarchy;
        blockData.wikidata.itemId = itemId;
      }
      
    } else if (identifier.startsWith('Q') || initialType === 'wikidata') {
      // Starting with Wikidata item
      const itemInfo = await getWikidataItem(identifier);
      const itemHierarchy = await getItemHierarchy(identifier);
      const commonsCategory = await findCommonsCategory(identifier);
      
      blockData.wikidata = {
        info: itemInfo,
        hierarchy: itemHierarchy,
        itemId: identifier
      };
      
      // Check for corresponding Commons category
      if (commonsCategory.results.bindings.length > 0) {
        const categoryName = commonsCategory.results.bindings[0].commonsCategory.value;
        const categoryInfo = await getCategoryInfo(categoryName);
        const parentCategories = await getParentCategories(categoryName);
        
        blockData.commons = {
          info: categoryInfo,
          parents: parentCategories
        };
        blockData.type = 'hybrid';
      } else {
        blockData.type = 'wikidata';
        blockData.relationship = {
          hasWikidataItem: true,
          hasCommonsCategory: false,
          type: 'wikidata',
          mismatch: true
        };
      }
    }
    
    // Handle cases where neither or both are unknown
    if (blockData.type === 'unknown') {
      console.warn(`Unable to determine type for identifier: ${identifier}`);
      blockData.relationship.type = 'error';
    }
    
    // Add visual properties for block rendering
    blockData.visual = {
      expanded: false,
      showSitePanel: blockData.wikidata !== null,
      hasChildren: (blockData.commons?.members?.query?.categorymembers?.length > 0) || 
                   (blockData.wikidata?.hierarchy?.results?.bindings?.length > 0),
      canSwitchHierarchy: blockData.type === 'hybrid'
    };
    
    return blockData;
    
  } catch (error) {
    console.error(`Failed to fetch block data for ${identifier}:`, error);
    
    // Return error block rather than throwing
    return {
      id: identifier,
      type: 'error',
      commons: null,
      wikidata: null,
      relationship: { type: 'error', mismatch: true },
      error: error.message,
      visual: { expanded: false, showSitePanel: false, hasChildren: false }
    };
  }
};
```

---
*This document should be updated as new API endpoints are integrated*