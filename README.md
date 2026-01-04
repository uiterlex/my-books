# My Books read list

A personal book collection website to catalog and display Dutch books I've read.

**Live site:** <https://books.uiterlex.nl>

## Features

* Public read access for all visitors
* Google sign-in for owner authentication
* Add, edit, and delete books when signed in
* Book cover image upload with automatic resizing (max 300×450px)
* Filter books by title, author, series, category, rating, and date read
* Sort books by date read, category, author, rating, and first edition
* Sticky header with filter & sort panel (stays visible while scrolling)
* Category badges (thriller, detective, other)
* Preview text with popup modal
* Plot summary with popup modal
* Date read and first edition display
* Real-time updates via Firestore
* Responsive design with left-aligned book covers on mobile

## Tech Stack

* HTML, CSS, JavaScript (single file)
* Firebase Firestore (database)
* Firebase Storage (cover images)
* Firebase Authentication (Google sign-in)
* Hosted on Vercel

## Firestore Structure

```
books/
  └── {bookId}/
      ├── title: string
      ├── author: string
      ├── pages: number
      ├── rating: number (1-5)
      ├── series: string
      ├── category: string ("thriller" | "detective" | "other")
      ├── dateRead: string ("YYYY-MM")
      ├── firstEdition: string ("YYYY")
      ├── preview: string
      ├── plot: string
      └── coverUrl: string (Firebase Storage URL)
```

## Storage Structure

```
covers/
  └── {bookId}.jpg
```

## Security Rules

### Firestore

```
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

```
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

## Filter Options

| Filter | Type | Description |
| --- | --- | --- |
| Title | Text | Partial match, case-insensitive |
| Author | Text | Partial match, case-insensitive |
| Series | Text | Partial match, case-insensitive |
| Category | Dropdown | Exact match (thriller, detective, other) |
| Rating | Operator + Value | =, ≥, ≤ with star rating |
| Date Read | Operator + Month | =, ≥, ≤ with month picker |

## Sort Options

| Sort | Description |
| --- | --- |
| Date read | Newest first (default) |
| Category → Author → First edition | Alphabetical cascade |
| Author → Series → First edition | Alphabetical cascade |
| Rating → Author → Series → First edition | Highest rating first |

All sort options have a reverse checkbox to flip the order.

## Setup

1. Clone this repository
2. Create a Firebase project
3. Enable Firestore, Storage, and Authentication (Google provider)
4. Update the Firebase config in `index.html`
5. Set up security rules as shown above
6. Deploy to Vercel or any static host
