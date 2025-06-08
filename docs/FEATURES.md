# Features Specification

## Overview
This document details all features and functionality of WikiMediaTree, a hierarchical visualization tool for exploring relationships between Wikimedia Commons categories and Wikidata items.

## Core Features

### Feature 1: User-Driven Interactive Canvas
**Description:** A pannable, zoomable canvas that serves as the primary interface for user-controlled exploration of hierarchical relationships in all directions. **Critical**: No automatic tree generation due to the chaotic nature of real hierarchies.

**User Story:** As a Wikimedian, I want to freely navigate and manually explore hierarchical structures so that I can understand complex category and item relationships through controlled, step-by-step discovery rather than overwhelming automatic displays.

**Acceptance Criteria:**
- [ ] Canvas supports smooth panning in all directions (up, down, left, right, diagonal)
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
- [ ] **File count prominently displayed** (pictures/files in Commons category, as shown on Commons)
- [ ] **Subcategory count prominently displayed** (as shown on Commons category pages)
- [ ] Connection status indicators clearly show Commons category presence
- [ ] Connection status indicators clearly show Wikidata item presence
- [ ] **Parent relationship type explicitly shown** (connected via Commons, Wikidata, or both)
- [ ] **Blocks default to reduced size** but expandable with button
- [ ] **Direct clickable links** to respective Commons and Wikidata pages
- [ ] Visual indication when both category and Wikidata item exist for same concept

**Technical Notes:** Use different CSS classes/styles for block types. Display counts exactly as Commons shows them. Implement clear connection indicators and expandable UI elements.

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

### Feature 5: Photo Organization and Hierarchy Editing
**Description:** Expandable block details with specific drag-and-drop functionality for organizing photos/files within hierarchies, plus general editing capabilities for moving things around in hierarchical structures.

**User Story:** As a Commons contributor, I want to **sort photos/files in hierarchies by drag and drop** and **make edits by moving things around in the hierarchy** so that I can efficiently restructure content organization and clean up hierarchical relationships.

**Acceptance Criteria:**
- [ ] Blocks can be expanded to show detailed information
- [ ] **Specific drag and drop**: Sort photos/files within hierarchies using drag and drop
- [ ] **General editing**: Make edits by moving things around in hierarchy
- [ ] **Hierarchy cleaning**: Tools for generally understanding, exploring, and cleaning hierarchies
- [ ] Context menus provide relevant actions for each block type
- [ ] Visual feedback during drag operations for photos/files
- [ ] Support for organizing content within hierarchical structures
- [ ] Undo/redo functionality for hierarchy changes

**Technical Notes:** Implement HTML5 drag and drop API specifically for photo/file organization within hierarchies. Include hierarchy editing tools for moving and reorganizing content. Focus on cleaning and organizing capabilities.

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
- [ ] **Panel behavior**: Can be expanded and collapsed
- [ ] **Direct linking**: Include link to full Wikidata item page
- [ ] Site panel integrates smoothly with block interface

**Technical Notes:** Create expandable site panel component (not just side panel) with SPARQL query integration. Focus on basic properties: label, description, P31 (instance of), P18 (image), plus other relevant properties.

---

### Feature 7: Specific Visual Hierarchy Indicators
**Description:** Precise visual elements showing additional parent categories with specific positioning, hover behaviors, and navigation actions that transform the entire hierarchy view.

**User Story:** As a general Wikimedian, I want to see **lines on the top left side moving up and disappearing** that show additional parent categories, with category names appearing on hover and click navigation that completely changes the hierarchy view.

**Acceptance Criteria:**
- [ ] **Top left lines**: Lines positioned on top left side of blocks, moving up and disappearing
- [ ] **Hover tooltips**: Category names appear when hovering over the top left lines
- [ ] **Click navigation behavior**: Clicking lines makes **hierarchy move, new blocks appear, old hierarchy disappears**
- [ ] **Expandable arrows**: Little arrows pointing down for children categories/items
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

### Feature 9: Transition Tool for Structured Data
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
| Visual Hierarchy Indicators | Medium | Low | Medium | ✓ |
| Wikidata Information Panel | Medium | Medium | Medium | ✓ |
| Hierarchy View Switching | Medium | Medium | High | |
| Advanced Block Interactions | Low | High | Medium | |
| Tree Manipulation Tools | Low | Medium | Low | |
| Mismatch Detection | Low | High | High | |
| Search and Navigation | Medium | Medium | Medium | |

---
*This document should be updated as features are defined and implemented*