Premium Funding Trade - Firebase Firestore Rules

Paste this in Firebase Console > Firestore Database > Rules, then Publish.

rules_version = '2';

service cloud.firestore {
  match /databases/{database}/documents {

    // Each logged-in user can read/write only their own profile document.
    match /users/{userId} {
      allow read, create, update: if request.auth != null && request.auth.uid == userId;
    }

    // Public waitlist form can create waitlist entries.
    // Optional: tighten this later with App Check / reCAPTCHA.
    match /waitlist/{entryId} {
      allow create: if true;
      allow read, update, delete: if false;
    }
  }
}

Important:
- The profile page cannot bypass Firestore security rules.
- If you see "permission-denied" or "Could not save profile", the Rules above must be published.
- KYC Base64 is stored inside users/{userId}.kycDetails.documentImageStr.
