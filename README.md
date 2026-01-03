# My Book Collection

A personal book collection website displaying Dutch books I've read.

**Live site:** [books.uiterlex.nl](https://books.uiterlex.nl)

## Tech Stack

- **Frontend:** Single `index.html` file with vanilla JavaScript
- **Database:** Firebase Firestore (real-time sync)
- **Storage:** Firebase Storage (book cover images)
- **Auth:** Firebase Google Authentication
- **Hosting:** Vercel (auto-deploys from GitHub)

## Firebase Project

- **Project ID:** my-books-4ef15
- **Console:** [console.firebase.google.com](https://console.firebase.google.com/project/my-books-4ef15)

## Firestore Structure

```
books/
  ├── {documentId}/
  │   ├── title: string (required)
  │   ├── author: string (required)
  │   ├── pages: number
  │   ├── rating: number (1-5)
  │   ├── series: string
  │   ├── category: string ("thriller" | "detective" | "other")
  │   ├── dateRead: string (free format, e.g. "Jun 2025")
  │   ├── firstEdition: string ("YYYY")
  │   ├── preview: string
  │   ├── plot: string
  │   └── coverUrl: string (Firebase Storage URL)
```

## Storage Structure

```
covers/
  └── {bookId}.jpg
```

Cover images are automatically resized to max 300×450px before upload.

## Features

- Public read access for all visitors
- Google sign-in for owner (uiterlex@gmail.com)
- Add/Edit/Delete books when signed in
- Book cover image upload with automatic resizing
- Color-coded category badges (thriller/detective/other)
- Series display with "Series:" prefix
- Preview text (first 30 characters with info icon popup)
- Plot text with popup modal
- Flexible date format for "Date Read" field
- Sorted by date read (newest first)
- Real-time updates via Firestore onSnapshot

## Security Rules

### Firestore

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /books/{bookId} {
      allow read: if true;
      allow write: if request.auth != null 
        && request.auth.token.email == "uiterlex@gmail.com";
    }
  }
}
```

### Storage

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /covers/{imageId} {
      allow read: if true;
      allow write: if request.auth != null 
        && request.auth.token.email == "uiterlex@gmail.com";
    }
  }
}
```
