# Educational Book Platform API Contracts

## Book Service

### Get all books
- **Endpoint**: `GET /api/books`
- **Description**: Retrieve a list of all available educational books
- **Request Parameters**:
  - `category` (optional, string): Filter by category
  - `difficulty` (optional, string): Filter by difficulty level
  - `limit` (optional, number): Number of results to return
  - `offset` (optional, number): Number of results to skip
- **Response**:
  - `200 OK`: Array of Book objects with metadata
  - `400 Bad Request`: Invalid query parameters
  - `500 Internal Server Error`: Server error

### Get book by ID
- **Endpoint**: `GET /api/books/{bookId}`
- **Description**: Retrieve a specific educational book with its chapters
- **Path Parameters**:
  - `bookId` (string): Unique identifier of the book
- **Response**:
  - `200 OK`: Single Book object with all chapters
  - `404 Not Found`: Book with given ID not found
  - `500 Internal Server Error`: Server error

## Chapter Service

### Get chapter by ID
- **Endpoint**: `GET /api/chapters/{chapterId}`
- **Description**: Retrieve a specific chapter content
- **Path Parameters**:
  - `chapterId` (string): Unique identifier of the chapter
- **Response**:
  - `200 OK`: Single Chapter object with content
  - `404 Not Found`: Chapter with given ID not found
  - `500 Internal Server Error`: Server error

### Get next chapter
- **Endpoint**: `GET /api/chapters/{chapterId}/next`
- **Description**: Get the next chapter in the sequence for a book
- **Path Parameters**:
  - `chapterId` (string): Current chapter ID
- **Response**:
  - `200 OK`: Next Chapter object
  - `404 Not Found`: No next chapter (current chapter is the last)
  - `500 Internal Server Error`: Server error

### Get previous chapter
- **Endpoint**: `GET /api/chapters/{chapterId}/previous`
- **Description**: Get the previous chapter in the sequence for a book
- **Path Parameters**:
  - `chapterId` (string): Current chapter ID
- **Response**:
  - `200 OK`: Previous Chapter object
  - `404 Not Found`: No previous chapter (current chapter is the first)
  - `500 Internal Server Error`: Server error

## User Progress Service

### Get user progress for a book
- **Endpoint**: `GET /api/users/{userId}/books/{bookId}/progress`
- **Description**: Retrieve a user's progress in a specific book
- **Path Parameters**:
  - `userId` (string): Unique identifier of the user
  - `bookId` (string): Unique identifier of the book
- **Response**:
  - `200 OK`: UserProgress object with completed chapters and current position
  - `404 Not Found`: User or book not found
  - `500 Internal Server Error`: Server error

### Update user progress
- **Endpoint**: `PUT /api/users/{userId}/books/{bookId}/progress`
- **Description**: Update a user's progress in a specific book
- **Path Parameters**:
  - `userId` (string): Unique identifier of the user
  - `bookId` (string): Unique identifier of the book
- **Request Body**:
  - `currentChapterId` (string): Current chapter the user is on
  - `completedChapters` (array of strings): IDs of completed chapters
  - `timeSpent` (number): Time spent in seconds
- **Response**:
  - `200 OK`: Updated UserProgress object
  - `400 Bad Request`: Invalid request body
  - `404 Not Found`: User or book not found
  - `500 Internal Server Error`: Server error

### Mark chapter as completed
- **Endpoint**: `POST /api/users/{userId}/chapters/{chapterId}/complete`
- **Description**: Mark a chapter as completed for a user
- **Path Parameters**:
  - `userId` (string): Unique identifier of the user
  - `chapterId` (string): Unique identifier of the chapter
- **Response**:
  - `200 OK`: Updated UserProgress object
  - `404 Not Found`: User or chapter not found
  - `500 Internal Server Error`: Server error

## Search Service

### Search content
- **Endpoint**: `GET /api/search`
- **Description**: Search across all educational content
- **Request Parameters**:
  - `q` (string): Search query
  - `bookId` (optional, string): Limit search to specific book
  - `limit` (optional, number): Number of results to return
- **Response**:
  - `200 OK`: Array of search results with content snippets and links
  - `400 Bad Request`: Missing search query
  - `500 Internal Server Error`: Server error

## Bookmark Service

### Create bookmark
- **Endpoint**: `POST /api/users/{userId}/bookmarks`
- **Description**: Create a new bookmark for the user
- **Path Parameters**:
  - `userId` (string): Unique identifier of the user
- **Request Body**:
  - `chapterId` (string): Chapter to bookmark
  - `position` (number): Position in the chapter (0-100)
  - `title` (optional, string): Custom title for the bookmark
  - `notes` (optional, string): Notes about the bookmarked content
- **Response**:
  - `201 Created`: Created Bookmark object
  - `400 Bad Request`: Invalid request body
  - `404 Not Found`: User or chapter not found
  - `500 Internal Server Error`: Server error

### Get user bookmarks
- **Endpoint**: `GET /api/users/{userId}/bookmarks`
- **Description**: Retrieve all bookmarks for a user
- **Path Parameters**:
  - `userId` (string): Unique identifier of the user
- **Response**:
  - `200 OK`: Array of Bookmark objects
  - `404 Not Found`: User not found
  - `500 Internal Server Error`: Server error

### Delete bookmark
- **Endpoint**: `DELETE /api/users/{userId}/bookmarks/{bookmarkId}`
- **Description**: Remove a specific bookmark
- **Path Parameters**:
  - `userId` (string): Unique identifier of the user
  - `bookmarkId` (string): Unique identifier of the bookmark
- **Response**:
  - `204 No Content`: Bookmark successfully deleted
  - `404 Not Found`: User or bookmark not found
  - `500 Internal Server Error`: Server error

## Error Response Format
All error responses follow this format:
```json
{
  "error": {
    "code": "string",
    "message": "string",
    "details": "string (optional)"
  }
}
```