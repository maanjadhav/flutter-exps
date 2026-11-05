# Experiment 10: Secure User Authentication and Session Management using Cloud Firestore

## Aim
To develop a Flutter application that implements secure user registration, login, and authenticated session management using Google Cloud Firestore and cryptographic password hashing with SHA-256 and unique salting.

## Description
This experiment demonstrates how to implement an authentication system in Flutter using Google Cloud Firestore as a cloud backend. The application features user registration, credential hashing with cryptographically secure random salting (`Random.secure()` + `SHA-256`), secure document storage in Firestore, credential verification, and navigation to an authenticated dashboard session.

## Technologies Used
- Flutter
- Dart
- Firebase Core (`firebase_core`)
- Google Cloud Firestore (`cloud_firestore`)
- Crypto package (`crypto`)
- Visual Studio Code

## Features
- **User Registration:** Validates email and minimum 6-character password length.
- **Cryptographic Security:** Generates a 16-byte random salt and computes SHA-256 password hashes to prevent storing plain-text passwords.
- **Deterministic Document IDs:** Computes deterministic SHA-256 email hashes for O(1) user document lookup in Firestore.
- **User Login & Verification:** Re-hashes incoming password with stored salt and validates match against Firestore records.
- **State Feedback:** Shows progress indicators and SnackBar error/success alerts.
- **Authenticated Dashboard:** Shows login confirmation, email identity, and session logout functionality.

## Database Structure
**Collection:** `users`

**Document ID:** SHA-256 hash of lowercase email string

| Field | Data Type | Description |
|---|---|---|
| `email` | String | User email address |
| `salt` | String | Base64-encoded 16-byte random salt |
| `passwordHash` | String | SHA-256 hash of (`salt` + `password`) |
| `createdAt` | Timestamp | Server timestamp at account registration |

## Procedure
1. Create a Flutter project named `flutter_application_10`.
2. Add dependencies: `firebase_core`, `cloud_firestore`, and `crypto` in `pubspec.yaml`.
3. Configure the Flutter application with Firebase using FlutterFire CLI.
4. Implement cryptographic utility functions (`getUserId`, `generateSalt`, `hashPassword`).
5. Design the authentication UI with toggleable Login/Register modes and password visibility toggle.
6. Implement registration logic to write hashed credentials and salts into the `users` Firestore collection.
7. Implement login verification logic by querying the user document and comparing SHA-256 hashes.
8. Navigate authenticated users to `DashboardPage` and handle session logout.

## Algorithm
1. Start the application and initialize Firebase.
2. Render the `AuthPage` with Email and Password text fields.
3. On **Register**:
   - Validate that email format is valid and password length >= 6.
   - Query Firestore to verify whether the account already exists.
   - Generate a 16-byte secure random salt.
   - Compute `passwordHash = SHA256(salt + password)`.
   - Store email, salt, passwordHash, and server timestamp in Firestore.
   - Switch UI to Login mode.
4. On **Login**:
   - Query the `users` collection for document ID `SHA256(email)`.
   - If not found, prompt user to register.
   - If found, retrieve stored salt and compute `SHA256(salt + enteredPassword)`.
   - If computed hash matches `passwordHash`, navigate to `DashboardPage`.
   - If mismatch, display "Incorrect password".
5. On **Logout**:
   - Navigate back to `AuthPage`.
6. Stop.

---

## 👤 Author
- **Author:** [maanjadhav](https://github.com/maanjadhav)
- **Repository:** [maanjadhav/flutter-exps](https://github.com/maanjadhav/flutter-exps)
