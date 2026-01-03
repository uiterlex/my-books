# My Book Collection

A personal book collection website displaying Dutch books I've read.

**Live site:** [books.uiterlex.nl](https://books.uiterlex.nl)

## Tech Stack

- **Frontend:** Single `index.html` file with vanilla JavaScript
- **Database:** Firebase Firestore (real-time sync)
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
  │   ├── dateRead: string ("YYYY-MM")
  │   ├── firstEdition: string ("YYYY")
  │   ├── preview: string
  │   └── plot: string
```

## Features

- Public read access for all visitors
- Google sign-in for owner (uiterlex@gmail.com)
- Add/Edit/Delete books when signed in
- Color-coded category badges
- Preview and plot text with popup modals
- Sorted by date read (newest first)
- Real-time updates via Firestore onSnapshot

## Security Rules

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
