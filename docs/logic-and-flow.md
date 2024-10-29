# Application Logic and Flow

## Authorization System
The system uses NextAuth for authentication, with Google OAuth as the primary provider. Minimal data is collected to comply with GDPR.

1. **Google OAuth Login**:
   - Users authenticate via Google.
   - The Google `sub` identifier is hashed and stored to preserve privacy.

2. **Session and Token Management**:
   - Token includes hashed identifier, internal user ID, and user status (student, professor, admin).

## Data Retrieval and Storage
- Tasks, lessons, and topics are fetched based on user status.
- Users can view topics, exercises, and feedback based on access rights.

## Navigation Logic
1. **Sidebar Navigation**:
   - Highlights active topics based on the exact URL match to avoid partial matches.
2. **Component-Based Access**:
   - Components adapt based on the user’s status.
