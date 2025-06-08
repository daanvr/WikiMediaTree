# Features Specification

## Overview
This document details all features and functionality of WikiMediaTree, a hierarchical visualization tool for exploring relationships between Wikimedia Commons categories and Wikidata items. The tool focuses primarily on **exploration and navigation** with integration to existing Wikimedia editing interfaces.

## Core Features

### Feature 1: User-Driven Interactive Canvas
**Description:** A pannable, zoomable canvas that serves as the primary interface for user-controlled exploration of hierarchical relationships in all directions. **Critical**: No automatic tree generation because hierarchies have multiple parents and children per category/item, making automatic trees overwhelming with hundreds of interwoven nodes.

**User Story:** As a manual editor who needs hierarchical structure overview, I want to freely navigate and manually explore hierarchical structures so that I can understand complex category and item relationships through controlled, step-by-step discovery rather than overwhelming automatic displays.

**Acceptance Criteria:**
- [ ] Canvas supports smooth panning in all directions (up, down, left, right, diagonal)
- [ ] **Canvas boundaries**: Never shows empty canvas - always keeps at least one block visible to prevent users getting lost
- [ ] Users can zoom in and out to view different levels of detail
- [ ] **User-driven exploration**: Tool does NOT automatically create or display entire trees
- [ ] Canvas performance remains smooth with user-controlled hierarchy growth
- [ ] Users control which parts of hierarchy to explore and display
- [ ] Ability to remove/quit parts of tree from current view

**Technical Notes:** Implement using Canvas API with coordinate transformation system and viewport management. Include requestAnimationFrame for smooth animations. Emphasize user control over automatic generation.

---

### Feature 2: Detailed Block-Based Visualization
**Description:** Visual representation of Commons categories and Wikidata items as interactive blocks showing comprehensive information exactly as displayed on Commons, with clear indicators for connection types and relationships.

**User Story:** As a content creator, I want to see clear visual distinctions between Commons categories, Wikidata items, and hybrid entities with detailed file/subcategory counts (as shown on Commons) so that I can understand the structure and identify missing connections.

**Acceptance Criteria:**
- [ ] Blocks visually distinguish between Commons-only, Wikidata-only, and hybrid entities
- [ ] **File count prominently displayed** (files in Commons category, as shown on Commons)
- [ ] **Subcategory count prominently displayed** (as shown on Commons category pages)
- [ ] Connection status indicators clearly show Commons category presence
- [ ] Connection status indicators clearly show Wikidata item presence
- [ ] **Parent relationship type explicitly shown** (connected via Commons, Wikidata, or both)
- [ ] **Blocks default to reduced size** but expandable with dedicated expand button (shows additional details)
- [ ] **Direct clickable links** to respective Commons and Wikidata pages
- [ ] Visual indication when both category and Wikidata item exist for same concept

**Technical Notes:** Use different CSS classes/styles for block types (e.g., .block--commons, .block--wikidata, .block--hybrid). Display counts exactly as Commons shows them. Implement clear connection indicators with icons or color coding. Create expandable UI elements with smooth animations and dedicated expand/collapse buttons.

---

### Feature 3: Hierarchical Tree Navigation
**Description:** Family tree-like visualization showing parent-child relationships with the ability to expand and collapse branches dynamically.

**User Story:** As a Wikimedia developer, I want to explore hierarchical structures as expandable trees so that I can understand complex relationships and identify areas needing organization.

**Acceptance Criteria:**
- [ ] Trees display in family tree format with clear parent-child connections
- [ ] Users can expand branches by clicking on expandable arrows
- [ ] Users can collapse branches to reduce visual complexity
- [ ] Trees can grow in any direction based on user exploration
- [ ] Visual indicators show unexplored branches
- [ ] Root blocks are clearly identified

**Technical Notes:** Implement tree data structure with lazy loading. Use SVG lines or Canvas drawing for connections. Include expand/collapse animations.

---

### Feature 4: Hierarchy View Switching
**Description:** Ability to switch between Commons category hierarchy and Wikidata item hierarchy when both exist for the same conceptual entity.

**User Story:** As a Wikidata editor, I want to compare Commons category hierarchies with Wikidata item hierarchies so that I can identify inconsistencies and improve structured data organization.

**Acceptance Criteria:**
- [ ] Users can switch between Commons and Wikidata hierarchy views
- [ ] Switching preserves current focus and position context
- [ ] Visual indicators show which hierarchy view is currently active
- [ ] Mismatches between hierarchies are highlighted
- [ ] Smooth transitions between hierarchy views

**Technical Notes:** Implement view state management with transition animations. Store both hierarchy structures in memory for quick switching.

---

### Feature 5: File Organization and Hierarchy Editing (Future/Research Phase)
**Description:** Expandable block details with specific drag-and-drop functionality for organizing files within hierarchies, plus general editing capabilities for moving things around in hierarchical structures. **Note**: This feature is parked for future development pending UI/UX research and user testing.

**User Story:** As a Commons contributor, I want to **sort files in hierarchies by drag and drop** and **make edits by moving things around in the hierarchy** so that I can efficiently restructure content organization and clean up hierarchical relationships.

**Acceptance Criteria:**
- [ ] Blocks can be expanded to show detailed information
- [ ] **Specific drag and drop**: Sort files within hierarchies using drag and drop
- [ ] **General editing**: Make edits by moving things around in hierarchy
- [ ] **Hierarchy cleaning**: Tools for generally understanding, exploring, and cleaning hierarchies
- [ ] Context menus provide relevant actions for each block type
- [ ] Visual feedback during drag operations for files (highlight drop zones, show drag preview)
- [ ] Support for organizing content within hierarchical structures
- [ ] Undo/redo functionality for hierarchy changes

**Technical Notes:** **FUTURE IMPLEMENTATION** - Requires UI/UX research and user testing. Would implement HTML5 drag and drop API specifically for file organization within hierarchies. May require side panel or alternative UI approach for showing file hierarchies. Authentication needed for actual editing operations.

---

### Feature 6: Wikidata Site Panel
**Description:** Expandable site panel triggered when a Wikidata item exists for a block, showing basic Wikidata information including label, description, instance of, image, and other relevant properties.

**User Story:** As a researcher, I want an **expandable site panel** that shows basic Wikidata item information (label, description, instance of, image, and other relevant properties) so that I can understand entity details without leaving the visualization interface.

**Acceptance Criteria:**
- [ ] **Trigger condition**: Site panel becomes available when Wikidata item exists for block
- [ ] **Basic information display**: Label (item name)
- [ ] **Basic information display**: Description
- [ ] **Basic information display**: Instance of (P31) 
- [ ] **Basic information display**: Associated image (P18)
- [ ] **Basic information display**: Other relevant Wikidata properties and values
- [ ] **Panel behavior**: Can be expanded and collapsed, triggered by clicking on blocks with Wikidata items
- [ ] **Direct linking**: Include link to full Wikidata item page
- [ ] Site panel integrates smoothly with block interface

**Technical Notes:** Create expandable site panel component (not just side panel) with SPARQL query integration. Focus on basic properties: label, description, P31 (instance of), P18 (image), plus other relevant properties.

---

### Feature 6: Ecosystem Toggle Functionality
**Description:** Toggle capability allowing users to focus exploration on Commons-only, Wikidata-only, or combined ecosystem views to reduce complexity and enable focused analysis of specific hierarchical systems.

**User Story:** As a hierarchical researcher, I want to toggle between Commons categories, Wikidata items, or combined views so that I can focus my exploration on specific ecosystems and understand each system independently before analyzing their relationships.

**Acceptance Criteria:**
- [ ] **Toggle options**: Three view modes - Commons-only, Wikidata-only, Combined (both)
- [ ] **Visual filtering**: Blocks and connections change based on selected ecosystem focus
- [ ] **State persistence**: Current toggle selection maintained during navigation
- [ ] **Clear indication**: UI clearly shows which ecosystem mode is currently active  
- [ ] **Smooth transitions**: Switching between modes provides smooth visual transitions
- [ ] **Mode-specific navigation**: Top left lines and parent/child relationships respect current ecosystem focus
- [ ] **Search integration**: Search results filtered by current ecosystem selection

**Technical Notes:** Implement view state management with ecosystem filtering logic. Update block rendering, connection drawing, and navigation systems to respect current ecosystem focus. Include visual indicators and smooth transition animations.

---

### Feature 7: API Status and Activity Indicators
**Description:** Clear visual feedback system showing API loading states, cached data status, and network activity to keep users informed about data freshness and system responsiveness.

**User Story:** As a user exploring hierarchies, I want to understand when data is loading, cached, or stale so that I can make informed decisions about the reliability of the information I'm viewing and understand system responsiveness.

**Acceptance Criteria:**
- [ ] **Status indicator placement**: Visible indicator in corner of canvas or underneath it
- [ ] **Loading states**: Clear indication when API calls are in progress
- [ ] **Cached data indication**: Visual indication when displaying cached/stale data
- [ ] **Network activity feedback**: Show active API requests and completion status
- [ ] **Error state display**: Clear messaging for API failures or network issues
- [ ] **Data freshness indicators**: Timestamp or freshness indicators for cached content
- [ ] **Non-intrusive design**: Status indicators don't interfere with exploration workflow

**Technical Notes:** Implement status indicator component with API call monitoring. Include caching layer status tracking and user-friendly error messaging. Position indicators to be visible but non-intrusive.

---

### Feature 8: Specific Visual Hierarchy Indicators  
**Description:** Small visual lines extending from top left area of blocks, extending upward and fading to indicate additional parent categories not currently displayed. Includes hover tooltips and click navigation that completely replaces the current hierarchy view.

**User Story:** As a general Wikimedian exploring hierarchies, I want to see **small lines from the top left area extending upward and disappearing** that show additional parent categories, with category names appearing on hover and click navigation that makes the hierarchy move to show new blocks while old hierarchy disappears.

**Acceptance Criteria:**
- [ ] **Small top left lines**: Lines extending from top left area on top side of blocks, small and disappearing quickly
- [ ] **Hover tooltips**: Parent category/item names appear when hovering over the top left lines
- [ ] **Click navigation behavior**: Clicking lines makes **hierarchy move, new blocks appear, old hierarchy disappears**
- [ ] **Multiple parent handling**: If too many parents exist, show most important ones with dropdown to select others
- [ ] **Parent indication purpose**: Let users know if there are and how many additional parent categories/items for this block that could be opened and explored
- [ ] **Expandable arrows**: Little arrows pointing down for children categories/items (click to load and display child blocks)
- [ ] **Hierarchy switching**: When parent lines show different connection types (Commons vs Wikidata), enable switching between hierarchy views
- [ ] Visual distinction between Commons and Wikidata relationship lines
- [ ] Clear indication of which parent relationships are currently hidden

**Technical Notes:** Implement specific top-left positioning for parent indicator lines. Create hover detection system for tooltips. Implement complete hierarchy transition system where clicking replaces entire view rather than just adding elements.

---

### Feature 8: Tree Manipulation Tools
**Description:** Tools for removing/quitting parts of trees, adding new branches, and cleaning up hierarchical structures.

**User Story:** As a content organizer, I want to remove irrelevant branches from my view and add new connections so that I can focus on specific areas and improve organizational structure.

**Acceptance Criteria:**
- [ ] Users can remove/hide specific branches from current view
- [ ] Ability to add new blocks and connections
- [ ] Undo functionality for tree modifications
- [ ] Save and load custom tree configurations
- [ ] Export tree structures for documentation
- [ ] Validation to prevent invalid hierarchical relationships

**Technical Notes:** Implement tree manipulation commands with validation logic. Include local storage for saving configurations.

---

### Feature 9: External Editing Integration
**Description:** Seamless integration with existing Wikimedia editing interfaces through direct links and missing entity creation workflows, supporting the exploration-first approach by guiding users to appropriate editing tools.

**User Story:** As an editor exploring hierarchies, I want direct access to existing Wikimedia editing interfaces and creation tools so that I can seamlessly transition from exploration to actual editing without losing context or having to search for the right pages.

**Acceptance Criteria:**
- [ ] **Direct links**: Clickable links to Commons category pages and Wikidata item pages from all blocks
- [ ] **Missing entity indication**: Clear visual indication when a block lacks either a category or item
- [ ] **Creation workflow links**: Direct links to creation pages for missing categories or items  
- [ ] **URL parameter passing**: Include parent category or item information as URL variables when linking to creation pages
- [ ] **Context preservation**: Maintain exploration context when users return from external editing
- [ ] **Authentication awareness**: Clear indication that actual editing happens in external interfaces (no authentication in WikiMediaTree)
- [ ] **Exploration focus**: Tool serves primarily as exploration and navigation interface, not direct editing tool

**Technical Notes:** Implement direct linking system to appropriate Wikimedia interfaces. Create URL parameter generation for creation workflows. Design clear visual indicators for missing entities and available editing actions.

---

### Feature 10: Transition Tool for Structured Data
**Description:** Detection and highlighting of inconsistencies between Commons category hierarchies and Wikidata item hierarchies, specifically designed as a tool for helping Wikimedia Commons transition from traditional category-based organization to structured data approach.

**User Story:** As a structured data advocate, I want to **explore where the hierarchy does not match between Wikimedia Commons and Wikidata** so that I can **create and enrich missing corresponding categories or items** and help **transition Commons from category-based to structured data organization**.

**Acceptance Criteria:**
- [ ] **Hierarchy mismatch exploration**: Identify where Commons and Wikidata hierarchies don't align
- [ ] **Missing entity identification**: Highlight where corresponding categories or items are missing
- [ ] **Creation workflow support**: Tools for creating missing categories or items where applicable
- [ ] **Transition support**: Help Commons transition from category-based to structured data approach
- [ ] **Enrichment tools**: Support for enriching existing categories and items
- [ ] Visual highlighting of inconsistent relationships
- [ ] **Flexible assumption handling**: Work with assumption that most Commons categories have Wikidata equivalents (but not always)
- [ ] Statistics on hierarchy alignment quality

**Technical Notes:** Implement comparison algorithms between hierarchy structures. Focus on supporting the transition from category-based to structured data organization. Include tools for creating and enriching missing entities.

---

### Feature 10: Search and Navigation
**Description:** Search functionality to find specific categories or items and navigate directly to them within the hierarchical visualization.

**User Story:** As a user, I want to search for specific categories or items so that I can quickly navigate to areas of interest without manually browsing through large hierarchies.

**Acceptance Criteria:**
- [ ] Search autocomplete for Commons categories and Wikidata items
- [ ] Direct navigation to search results within canvas
- [ ] Search history and bookmarking functionality
- [ ] Filter search results by type (Commons, Wikidata, or both)
- [ ] Search within current visible hierarchy subset
- [ ] Quick navigation shortcuts and keyboard controls

**Technical Notes:** Integrate with Commons and Wikidata search APIs. Implement client-side search index for visited items.

---

## Future Features

### Phase 2 Features
- **Collaborative Editing**: Multi-user editing sessions with real-time synchronization
- **Advanced Analytics**: Statistics and reports on hierarchy health and structure
- **Mobile Interface**: Touch-optimized interface for mobile devices
- **API Integration**: RESTful API for external tool integration
- **Batch Operations**: Bulk editing tools for large-scale reorganization

### Phase 3 Features
- **Machine Learning Suggestions**: AI-powered suggestions for hierarchy improvements
- **Version Control**: Track changes and history of hierarchical modifications
- **Advanced Visualizations**: Alternative visualization modes (circular, timeline, etc.)
- **Integration Tools**: Direct editing capabilities within external Wikimedia tools

## Feature Dependencies

| Feature | Depends On | Reason |
|---------|------------|---------|
| Hierarchy View Switching | Block-Based Visualization | Requires block system to switch between views |
| Wikidata Information Panel | Advanced Block Interactions | Triggered by block expansion |
| Tree Manipulation Tools | Hierarchical Tree Navigation | Requires tree structure to manipulate |
| Mismatch Detection | Hierarchy View Switching | Needs both hierarchies to compare |
| Search and Navigation | Interactive Canvas | Requires canvas for navigation targets |

## Feature Priority Matrix

| Feature | Priority | Effort | Impact | MVP |
|---------|----------|--------|--------|-----|
| Interactive Canvas | High | High | High | ✓ |
| Block-Based Visualization | High | Medium | High | ✓ |
| Hierarchical Tree Navigation | High | Medium | High | ✓ |
| Ecosystem Toggle | High | Medium | High | ✓ |
| API Status Indicators | High | Low | Medium | ✓ |
| Visual Hierarchy Indicators | High | Medium | High | ✓ |
| External Editing Integration | High | Low | High | ✓ |
| Wikidata Site Panel | Medium | Medium | Medium | ✓ |
| Hierarchy View Switching | Medium | Medium | High | |
| Transition Tool for Structured Data | Medium | High | High | |
| Search and Navigation | Medium | Medium | Medium | |
| Tree Manipulation Tools | Low | Medium | Low | |
| File Organization (Future) | Low | High | Medium | |

---
*This document should be updated as features are defined and implemented*