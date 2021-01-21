# react-firebase-authentication

## Features

- uses:
  - only React (create-react-app)
  - firebase
  - react-router
  - **semantic UI**
- features:
  - Sign In
  - Sign Up
  - Sign Out
  - Password Forget
  - Password Change
  - Verification Email
  - Protected Routes with Authorization
  - Roles-based Authorization
  - Social Logins with Google, Facebook and Twitter
  - Linking of Social Logins on Account dashboard
  - Auth Persistence with Local Storage
  - Database with Users and Messages

## Installation

- `git clone https://github.com/ComradeCat24/react-firebase-authentication.git`
- `cd react-firebase-authentication`
- `npm install`
- `npm start`
- visit http://localhost:3000

### Firebase Configuration

- copy/paste your configuration from your Firebase project's dashboard into one of these files
  - _src/components/Firebase/firebase.js_ file
  - _.env_ file
  - _.env.development_ and _.env.production_ files

The _.env_ or _.env.development_ and _.env.production_ files could look like the following then:

```
REACT_APP_FIREBASE_API_KEY=""
REACT_APP_FIREBASE_AUTH_DOMAIN=""
REACT_APP_FIREBASE_DATABASE_URL= ""
REACT_APP_FIREBASE_PROJECT_ID=""
REACT_APP_FIREBASE_STORAGE_BUCKET=""
REACT_APP_FIREBASE_MESSAGING_SENDER_ID=""
REACT_APP_FIREBASE_APP_ID=""
```

### Activate Verification E-Mail

- add a redirect URL for redirecting a user after an email verification into one of these files
  - _src/components/Firebase/firebase.js_ file
  - _.env_ file
  - _.env.development_ and _.env.production_ files

The _.env_ or _.env.development_ and _.env.production_ files could look like the following then (excl. the Firebase configuration).

**Development:**

```
REACT_APP_FIREBASE_CONFIRMATION_EMAIL_REDIRECT=http://localhost:3000

```

**Production:**

```
REACT_APP_CONFIRMATION_EMAIL_REDIRECT=https://mydomain.com
```

### Security Rules

```
{
  "rules": {
    ".read": false,
    ".write": false,
    "users": {
      "$uid": {
        ".read": "$uid === auth.uid || root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])",
        ".write": "$uid === auth.uid || root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])"
      },
      ".read": "root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])",
      ".write": "root.child('users/'+auth.uid).child('roles').hasChildren(['ADMIN'])"
    },
    "messages": {
      ".indexOn": ["createdAt"],
      "$uid": {
        ".write": "data.exists() ? data.child('userId').val() === auth.uid : newData.child('userId').val() === auth.uid"
      },
      ".read": "auth != null",
      ".write": "auth != null",
    },
  }
}
```
