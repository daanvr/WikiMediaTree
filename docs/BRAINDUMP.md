# Braindump

## June 8 2025 23:00
General descripton: All the things the tool should be able to do as of my first ideas describing this tool. My first design, my first description. 
BRaindump by @daanvr:
dump: Ok, WikimediaTree is going to be a tool to visualize the hierarchy of both Commons categories and Wikidata items. It is going to be a tool for developers, content creators, for the Wikimedia ecosystem and also general Wikimedians or people that edit Wikimedia stuff such as Wikidata and Wikimedia Commons. I want it to be a thing, this is called a canvas. It should be a thing where you can pan in all directions and where you can see the hierarchy almost as a family tree and explore categories and items. When I talk about items it's always wiki data items, when I talk about categories it's always columns, categories. Please note that somewhere. This tool should not create the tree by itself because the tree is pretty chaotic, the real stuff is pretty chaotic. The tree should grow in which direction. It should also be able to quit or remove parts of the trees. This whole tool is also based on the flexible assumption that most categories in Commons have a wiki data item equivalent. This is not one on one true, so it should be flexible and a category that's not linked to an item should be able to be displayed and an item which is not a category should be able to be displayed. This is not a category that should be displayed. When a category and a wiki data item both exist for the same thing, that should be indicated. This is not a category that should be displayed. The goal of this tool is to help the user understand the hierarchy, to also make edits by moving things around in the hierarchy, maybe to sort things like photos in this hierarchy by drag and drop, and to generally understand it and explore it and possibly clean it, and also explore where the hierarchy does not match between Wikimedia Commons and Wikidata, such that if applicable, the corresponding category or item can be created and enriched. In my opinion, this could also be a nice tool for helping the transition on Wikimedia Commons, that is traditionally commons category based. Transition it to structured data on commons. This whole thing should be built from blocks. These blocks are either a commands category or a wiki data item or both when they exist both. These blocks are the building blocks of this project. Each block has a parent block and potential children blocks. Only the top block in the tree would not have a parent block, like in a family tree. These blocks should display that they have a... If they have a commons category matching this block, if they have a wiki data item matching this block. They should also display if the parent block is linked through a commands category, or a wiki data item, or both. The previously mentioned commons categories and wikidata links should be linking to their respective pages. On the block there should also be displayed how many pictures are in this block meaning how many pictures are in the commons category I mean files and they should also mention how many subcategories there are like they do on commons. On commons when you see a link to another category you always see how many pictures are in this category and how many subcategories are there. In principle these blocks are in their reduced size but they can also be expanded with a button. Also on the top left side there should be lines moving up and disappearing that show that there are other parent categories that are not displayed right now but do exist. When hovering the category name should appear. When clicked the hierarchy should move and new blocks should appear and the old hierarchy should disappear. If the line from the parent category is only a commons category or a wikidata item because they are different but both exist then both should be displayed and you should then be able to switch from commons to wikidata hierarchy. When a wikidata item exists for this block make an expandable site panel that shows the basic information for the wikidata item like label description instance of an image and whatever other things are relevant. Also if the block has children categories or items they should have a little arrow pointing down that is clickable to extend the hierarchy in that direction. Okay, please fill in the relevant information in all the different documentation files. And also in the main readme markdown file. Make sure to delicately express all the things I've discussed above. These are a lot I understand, but make sure to document them all in detail. And some things might appear in different files. That is okay. Thank you.

## June 9 2025 00:00 - Q&Q
###   Core Vision & Purpose
#### 1. What specific pain points in current Wikimedia workflows motivated you to create this tool? What can't users do today that this solves?
The ability to see the more complete picture of the hierarchy is really limited today. In the Commons website you can see subcategories and you can see parent categories. But they are both just lists. 
  Sometimes when you go down in subcategories you don't know where you came from and how you should go back to the previous categories for example. And in Wikidata it's even more complicated because it is 
  covered by different properties. You have subclass, you have located in administrative area, you have all kinds of properties. Wikidata properties that define some form of hierarchy. This is also som. 
  ething we should identify and list. To make sure we can properly identify and propose parent and children items. On Wikidata it is really complicated to find the correct parents and child items. So this 
  is a real pain point for users. 

#### 2. Who are your primary user personas beyond the general categories listed? Can you describe 2-3 specific user archetypes with their goals and technical expertise?
The primary users are people that make edits by hand and need to get some overview of the structure. They can be technical but they are probably more focused on the data side and the contribution side

#### 3. What does "transition from category-based to structured data" actually mean in practice? What specific workflows change?
For a very long time, Wikimedia Commons structured its media files through categories. These categories are very chaotic and messy. Every user can create any category and move things there. These are 
  inconsistent. Some categories have circuitries based on year. There are also categories for users who uploaded the picture. There are so many different types of categories that it is really complicated 
  and messy. So a good solution for it to make it also because basically that's only human readable and understandable. And it's complicated to create/modify these hierarchies because they are so delicate. 
  But in structured data for Commons or Wikidata you can identify things very specifically in pictures and about pictures. For example, I talk often about pictures but the most important are media files. 
  Some media files are different but most of them are pictures so that's why I focus on them. But we should always talk about media files. Some media files can be described in very high accuracy using 
  structured data on Commons. And these are human and machine readable and editable. which is in stark contrast with the Commons categories. These are not machine readable or understandable or editable.

### The "Chaotic Nature" of Hierarchies
#### 4. What makes Wikimedia hierarchies "chaotic" specifically? Can you give concrete examples of the problems?
Commons hierarchy is coyote because sometimes it can be based on for example location or building or things in the picture or when the picture was taken or specific items in the picture. Or all kinds 
  of different stuff. So these hierarchies can be very unclear and chaotic and everyone can create its own hierarchy, subcategories and ban categories for everything. So there are different range of 
  thoughts and stuff for different areas in Wikimedia Commons. So they grew historically. It was the best solution back then but now with structured data on Commons we can make it so much more and better 
  structured. 

#### 5. Why is automatic tree generation problematic? What would happen if users tried it? Performance issues? Overwhelming UI? Incorrect relationships?
Automatic tree generation is problematic because there are usually multiple parents per category and multiple children per category. Meaning that the tree becomes very wide very quickly in both 
  directions and can expand very quickly. This makes it for the user more complicated to understand, I believe. I think performance wise it should be feasible but it will be an overwhelming number of 
  blocks. So to keep it readable and let the user have the oversight we should make it expand by user input. 

#### 6. How large do these hierarchies get? Are we talking tens, hundreds, thousands of nodes? What's a typical vs. extreme case?
Oh, that's hard for me to describe. I have no idea how many categories are in one hierarchy, but they can be hundreds probably. Maybe in some cases way more, but I think hundreds in general. And they 
  are interwoven, right? So from one file moving up apparent categories, you can go to planet or Hitler or like anything, species, I don't know. Like the general things, usually the more you go up, the more
   general it is, the more you go down, the more specific it is. But you can, many pictures can end up in very different categories if you go up, up, up, up in the tree. 

### Visual Design & Interaction Details
#### 7. The "top left lines moving up and disappearing" - can you describe this visually? How far do they extend? What triggers them appearing/disappearing? What do they look like?
 I was thinking of just some lines. There will be one line in the middle, right? Coming from above, going down to the block to describe where it came from, where the parent block is. I was thinking of 
  designing lines that leave the block from the top left area on the top side, but very small and disappearing pretty quickly. On hover you can see more the name of the parent hierarchy and you can click 
  them and if there are too many they should show the most important and maybe a drop down to select them or something. Basically it's to let the user know if there are and how many there are parents, 
  categories or items for this block that could be opened and explored. 

#### 8. Canvas navigation boundaries - can users pan infinitely? Are there limits? How does performance affect the canvas size?
I don't know how big the canvas should be but I think the canvas should not pan beyond visible things. So basically there should always be a visible block whenever you pan too much outside. So there 
  should be never an empty canvas visible such that the user cannot get lost. I have no idea on expected performance. So let's just go for it, make it as nice as possible. And we'll see if it's too big or 
  too slow. We can limit it later. 

#### 9. Block visual states - beyond Commons/Wikidata/hybrid, what other states exist? Loading? Error? Conflicted data? Missing relationships?
Blogs should have some things like Indeed Commons Wikidata or Hybrid but they should also show maybe the category title or the Wikidata label maybe the description of the Wikidata item and of course if
   something is wrong there could be an error or while loading it could show it is loading when missing a Wikidata item or a category they could show that Qtimp for now will find new things to add to it

### User Workflow & Experience
#### 10. What's a typical user session? Can you walk through a start-to-finish workflow for each user type?
I think the user will search for a category or wiki data item and start from there and probably click on subcategories and items and sub blocks basically and explore those. They might click on the links to their category page or item page. They might explore a bit more. They might make some edits. But they will mostly use it for navigating the tree and exploring, I believe. Editing, moving, creating, using things like drag and drop will come later. These are not crystallized yet and they must be user tested before.

### Hierarchy Editing & Organization  
#### 19. File organization within hierarchies - is this about reordering files within a category page, or actually moving files between categories?
File organization and moving files, tracking and dropping them within the hierarchy. I was envisioning something like you can see all the files inside one category and then move them to child categories. This is not crystallized yet and I think there should be a side panel to the whole page or something to show the hierarchy. I'm unsure yet, I don't know exactly how this should work, so let's park this for now. I'm going to focus on the exploration of this tool.

#### 20. "Make edits by moving things around" - what specific edit operations should be supported?
This is the one editing thing that should be implemented first whenever we start on the editing things. We should not start on that yet because these API calls need authentication which we will not implement yet in this tool but this first implementation would be I think a sort of a button or grip area on a block where you can grab it and move it around and when you move it for example up or down and connect it to a different category then it should be moved there by changing parent or end or child categories.

### Search & Navigation Integration
#### 15. Missing entity creation - should the tool enable creating new categories/items directly, or just identify gaps?
When a block does not have either a category or an item, it should display that and let the user edit it. Because we do not do authentication and we should have it. Go to the item creation or new category page. If possible we can add as URL variables the parent category or parent item if applicable.

#### 11. Wikimedia Integration - does this tool guide users to make edits in existing Wikimedia interfaces?
It should work as an exploration tool first and it should also add links to the Commons categories and Wikidata items for editing. But the tool is for exploration and navigation and the original existing Wikimedia interface will be used for editing.

### Error Handling & Edge Cases
#### 25. When APIs are down - should the tool work with cached data? How stale is acceptable?
We should cache data whenever reasonable and the size is not too big. And it should be clear to the user that it is partial or stale. And that it is waiting for a load. API activity should be clear to the user at any time. Maybe with a status, activity status or something in one of the corners of the canvas for example. Or just underneath it.

### Data Relationships & Conflicts
#### 1. (Follow-up) Which key Wikidata properties should be considered as hierarchical relationships for this tool?
I do not know which Wikidata properties clearly describe hierarchy going up or down the tree. This should be researched and explored. Mark this as a to-do. That should be done pretty early with a relatively high priority because this is core functionality of this tool.

### Additional Requirements
#### Ecosystem Toggle Functionality
The user should be able to toggle Wikidata and Wikimedia Commons data in this canvas such that they can focus on only Wikidata things or only categories. Thus, having the possibility to split both ecosystems to focus on either one or both.

## June 9 2025 01:00 - Development Strategy Discussion
### User Requirements for Development Approach
Let's now create a development strategy document in markdown and documentation. This document should describe how we go about building this project. For me the most important thing is that we start with small steps and maybe start visual first and then build further. Meaning that we might start with a skeleton user interface with minimum features but which shows how it could look like what it could look like and how the basics would be. Then build on separate features building on further. Furthermore I would also have the strategy of testing maybe building mini simple prototypes for testing API interactions and checking if everything works as expected and before implementing them in the user interface or in the main project I mean. And also I think it's very important to track basically tickets through github issues and we should always build have a bunch of the most important next issues built in GitHub such that we can read the issues and start building but to avoid having more than 25 issues let's build make the issues not all at once. So choose a few most important ones create those and along the way when issues get closed we can create new ones let's not make it 25 let's keep it smaller maybe 10 of the most important next issues built or created. Issues should describe in quite a lot of detail what and how they work such that the human in the loop can check them edit them refine them and see if the AI created issues are doing what is expected Write this strategy file describing how we're going to build this and add incomplete details maybe beyond what I've said to make it a complete strategy.

