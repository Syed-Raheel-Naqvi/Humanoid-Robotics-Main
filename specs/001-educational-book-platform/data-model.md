# Data Model: Educational Book Platform

## Core Entities

### Book
**Description**: Represents an educational book with metadata and content organization

**Fields**:
- `id`: string (unique identifier)
- `title`: string (book title)
- `author`: string (author name)
- `description`: string (brief description)
- `publicationDate`: Date (when the book was published)
- `difficultyLevel`: string (beginner, intermediate, advanced)
- `category`: string (programming, robotics, science, etc.)
- `tags`: string[] (relevant tags for search and categorization)
- `targetAudience`: string (students, professionals, hobbyists)
- `estimatedReadingTime`: number (in minutes)
- `chapters`: Chapter[] (array of chapters in the book)
- `createdAt`: Date (when the book entry was created)
- `updatedAt`: Date (when the book entry was last modified)

**Relationships**:
- One-to-many with Chapter (one book contains many chapters)
- Owned by User (author/creator)

**Validation Rules**:
- `title` is required and must be 1-200 characters
- `author` is required
- `difficultyLevel` must be one of the predefined values
- `estimatedReadingTime` must be a positive number

### Chapter
**Description**: A section of content within a book that contains educational material

**Fields**:
- `id`: string (unique identifier)
- `bookId`: string (reference to the parent book)
- `title`: string (chapter title)
- `content`: string (the actual educational content in MDX format)
- `position`: number (order in the book)
- `slug`: string (URL-friendly identifier)
- `prerequisites`: string[] (concepts to understand before reading this chapter)
- `learningObjectives`: string[] (what the learner will gain from this chapter)
- `createdAt`: Date (when the chapter was created)
- `updatedAt`: Date (when the chapter was last modified)

**Relationships**:
- Many-to-one with Book (many chapters belong to one book)
- One-to-many with InteractiveElement (one chapter may contain multiple interactive elements)

**Validation Rules**:
- `title` is required and must be 1-200 characters
- `position` must be a positive integer
- `slug` must be unique within the book
- `content` is required

### InteractiveElement (Optional)
**Description**: Elements within chapters that provide interactive learning experiences

**Fields**:
- `id`: string (unique identifier)
- `chapterId`: string (reference to the parent chapter)
- `type`: string (code-block, quiz, demo, simulation)
- `content`: string (the interactive element content)
- `position`: number (order within the chapter)
- `config`: object (configuration options for the interactive element)
- `createdAt`: Date (when the element was created)
- `updatedAt`: Date (when the element was last modified)

**Relationships**:
- Many-to-one with Chapter (many elements belong to one chapter)

**Validation Rules**:
- `type` must be one of the predefined values
- `position` must be a positive integer
- `config` must be a valid JSON object

### User (Optional)
**Description**: A person accessing the educational platform

**Fields**:
- `id`: string (unique identifier)
- `email`: string (email address for login)
- `name`: string (display name)
- `role`: string (student, educator, admin)
- `progress`: UserProgress[] (tracking progress through books)
- `createdAt`: Date (when the user account was created)
- `updatedAt`: Date (when the account was last updated)

**Relationships**:
- One-to-many with UserProgress (one user has progress in many books)
- One-to-many with Bookmark (one user can have many bookmarks)

**Validation Rules**:
- `email` is required and must be a valid email format
- `role` must be one of the predefined values

### UserProgress
**Description**: Tracks a user's progress through a specific book

**Fields**:
- `id`: string (unique identifier)
- `userId`: string (reference to the user)
- `bookId`: string (reference to the book)
- `currentChapterId`: string (which chapter the user is currently on)
- `completedChapters`: string[] (IDs of completed chapters)
- `lastAccessedAt`: Date (when the user last accessed the book)
- `completionPercentage`: number (percentage of book completed)
- `timeSpent`: number (time spent in seconds)

**Relationships**:
- Many-to-one with User (many progress entries for one user)
- Many-to-one with Book (many progress entries for one book)
- Many-to-many with Chapter through completedChapters

**Validation Rules**:
- `completionPercentage` must be between 0 and 100
- `timeSpent` must be a non-negative number

### Bookmark
**Description**: Allows users to save their current reading position or interesting sections

**Fields**:
- `id`: string (unique identifier)
- `userId`: string (reference to the user who created the bookmark)
- `chapterId`: string (reference to the specific chapter)
- `position`: number (position in the chapter, such as scroll percentage or section)
- `title`: string (optional title for the bookmark)
- `notes`: string (optional notes from the user about the bookmarked content)
- `createdAt`: Date (when the bookmark was created)
- `updatedAt`: Date (when the bookmark was last updated)

**Relationships**:
- Many-to-one with User (many bookmarks for one user)
- Many-to-one with Chapter (many bookmarks for one chapter)

**Validation Rules**:
- `position` must be between 0 and 100 if it's a percentage

## State Transitions

### Chapter State Management
- `draft` → `review` → `published`: Content creation workflow
- `published` → `archived`: When content is deprecated but kept for reference

### User Progress Tracking
- `not-started` → `in-progress` → `completed`: Chapter completion states
- `active` → `paused`: When a user stops/resumes studying

## Additional Considerations

### Content Storage
Since this is a Docusaurus-based educational book platform, the core content (chapters) will be stored as MDX files in the file system. The data model primarily focuses on metadata and relationships that would be stored in any database for user tracking, book organization, and personalization features.

### Search Indexing
Content from the MDX files will be indexed for search functionality. The search system will likely work directly with the file system content rather than a database, though search metadata could be stored separately for performance.

### Caching Strategy
Frequently accessed content and user progress data should be cached to improve performance, with appropriate invalidation strategies when content is updated.