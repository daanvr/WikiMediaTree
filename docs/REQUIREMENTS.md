# Requirements Specification

## Overview
WikiMediaTree is a hierarchical visualization tool for exploring the relationships between Wikimedia Commons categories and Wikidata items. It provides an interactive canvas for understanding, navigating, and managing the complex hierarchies within the Wikimedia ecosystem.

## Terminology
- **Items**: Always refers to Wikidata items
- **Categories**: Always refers to Wikimedia Commons categories
- **Blocks**: Building blocks representing either a Commons category, Wikidata item, or both
- **Canvas**: The pannable, interactive visualization area

## Functional Requirements

### Core Functionality
1. **Hierarchical Visualization**
   - Display hierarchies as family tree-like structures on an interactive canvas
   - Support panning in all directions across the canvas
   - Show parent-child relationships between blocks
   - Allow growing the tree in any direction (not automated)

2. **Block Management**
   - Each block represents a Commons category, Wikidata item, or both
   - Display block connection status (Commons category, Wikidata item, or both)
   - Show parent relationship type (linked through Commons, Wikidata, or both)
   - Allow removing/quitting parts of the tree

3. **Data Integration**
   - Handle flexible mapping between Commons categories and Wikidata items
   - Support categories without corresponding Wikidata items
   - Support Wikidata items without corresponding categories
   - Indicate when both category and item exist for the same concept

### User Requirements
1. **Target Users**
   - Developers in the Wikimedia ecosystem
   - Content creators and editors
   - General Wikimedians
   - Commons and Wikidata editors

2. **User Goals**
   - Understand hierarchical relationships
   - Make edits by moving items in hierarchies
   - Sort photos/files by drag and drop
   - Explore hierarchy mismatches between Commons and Wikidata
   - Clean and organize hierarchical structures
   - Support transition from category-based to structured data approach

### Block Features
1. **Display Information**
   - File count (number of pictures/files in Commons category)
   - Subcategory count
   - Connection status indicators
   - Parent relationship indicators
   - Links to respective Commons/Wikidata pages

2. **Interaction Features**
   - Expandable/collapsible detailed view
   - Hover tooltips showing category names
   - Click navigation to change hierarchy focus
   - Hierarchy switching between Commons and Wikidata views

3. **Visual Indicators**
   - Lines showing additional parent categories (not currently displayed)
   - Expandable arrows for children categories/items
   - Different visual states for Commons-only, Wikidata-only, or combined blocks

### Wikidata Integration
1. **Side Panel**
   - Expandable panel for Wikidata item details
   - Display label, description, instance of
   - Show associated images
   - Include other relevant Wikidata properties

### Performance Requirements
- Smooth panning and zooming on canvas
- Responsive interaction with blocks and UI elements
- Efficient loading of hierarchy data
- Handle large hierarchical structures without performance degradation

### Security Requirements
- Read-only access to public Wikimedia APIs
- No authentication required for viewing
- Secure handling of external API requests

## Technical Requirements

### Browser Support
- Desktop browsers only (no mobile/responsive needed)
- Modern browsers supporting ES6+
- Canvas/SVG support for visualization

### Technology Stack
- Client-side only architecture
- Modern JavaScript (ES6+)
- HTML5/CSS3
- Canvas or SVG for visualization
- Fetch API for external requests

### API Integration
- Wikimedia Commons API
- Wikidata Query Service (SPARQL)
- MediaWiki API

### Constraints
- Client-side only (no backend server)
- Must handle CORS for external API calls
- Real-time hierarchy data fetching
- No user authentication system needed

## Acceptance Criteria
1. Users can visualize hierarchies as interactive family trees
2. Users can pan freely across the canvas in all directions
3. Blocks correctly display Commons category and Wikidata item status
4. Users can expand/collapse hierarchy branches
5. Side panel displays comprehensive Wikidata information
6. Hierarchy switching between Commons and Wikidata works seamlessly
7. File counts and subcategory counts are accurately displayed
8. Links to Commons and Wikidata pages function correctly
9. Visual indicators clearly show relationship types and additional parents
10. Drag and drop functionality works for organizing items

---
*This document should be updated as requirements evolve*