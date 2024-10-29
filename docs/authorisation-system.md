# Documentation: Authorization System and GDPR Compliance

## Overview
This document explains the implementation of the authorization system used in the project and how it adheres to the requirements of the EU GDPR regulations. The system is built using NextAuth.js for authentication, utilizing Google OAuth as the identity provider, and it aims to ensure that user data is handled in a secure, compliant, and minimally invasive manner.

## Authorization Flow
The authorization system uses NextAuth.js to handle user authentication. The main provider for authentication is Google OAuth, configured to request minimal scopes for privacy compliance.

### Authentication Process
1. **OAuth Login**: Users authenticate using their Google account. The NextAuth configuration requests only the `openid` scope, ensuring that the information collected is limited to the unique identifier (`sub`) provided by Google.
2. **User Identifier Handling**: After successful authentication, the Google `sub` identifier is hashed using a SHA-256 hashing mechanism. This hashed value is used as the unique identifier for users in the system, ensuring that no personal data such as email, name, or profile information is collected or stored.
3. **User Database Interaction**: The hashed `sub` is then checked against the database:
   - If a user with this identifier already exists, the system retrieves the user's internal ID.
   - If no user is found, a new internal user ID (UUID) is generated and stored in the database along with the hashed identifier.
4. **Session and Token Management**:
   - The hashed identifier and the internal user ID are stored in the session and JWT token, respectively, allowing for seamless user session management without the need to store personally identifiable information.

### Data Storage
- **Hashed Identifier**: Only the hashed version of Google’s `sub` identifier is stored. This ensures that the data cannot be linked back to the user without access to the original Google identifier and the hashing function.
- **Internal User ID**: A newly generated internal user ID (UUID) is stored in the database. This ID is used internally to manage user accounts.
- **No Personal Data**: The system does not store any personal data such as emails, names, or profile pictures, complying with the GDPR's principle of data minimization.

## GDPR Compliance
The authorization system is designed with GDPR compliance in mind. Below are the key points demonstrating compliance:

1. **Data Minimization**: The system only requests the `openid` scope from Google OAuth, which provides a unique identifier (`sub`). This identifier is hashed before being stored, and no other personal data is collected or stored.

2. **Pseudonymization**: The use of hashed identifiers and internal user IDs ensures that user data is pseudonymized. The hashing process makes it practically impossible to reverse-engineer the original identifier, adding an extra layer of protection.

3. **No Personally Identifiable Information (PII)**: The system explicitly avoids storing any PII, such as email addresses or names. This reduces the risk of data breaches and ensures that even in the event of unauthorized access, sensitive user information is not exposed.

4. **User Rights**:
   - **Right to Access**: Users can be provided with information about their internal user ID without exposing any personal data.
   - **Right to Be Forgotten**: Since the system does not store personal data, users can be effectively “forgotten” by deleting their internal user ID and hashed identifier from the database.

5. **Data Security**: The hashed identifier is generated using a secure SHA-256 hashing function, ensuring that the data is adequately protected both in transit and at rest. The database is accessed using secure connections, and only necessary fields are retrieved or stored.

## Summary
The current implementation of the authorization system ensures GDPR compliance through the following mechanisms:
- Limited data collection (`openid` scope only).
- Hashing the Google-provided `sub` identifier to prevent reverse identification.
- No storage of personally identifiable information (PII).
- Pseudonymization of user data through internal user IDs.

These measures together ensure that the application can provide a secure and GDPR-compliant authentication mechanism for the users while minimizing the data handling requirements and adhering to privacy regulations.

## Abstract
The authorization system in my project uses Google OAuth via NextAuth.js, focusing on GDPR compliance through data minimization and pseudonymization. Only the openid scope is requested, and the Google-provided sub identifier is hashed before being stored, ensuring no personal information like emails or names is retained. A unique internal user ID is generated and used for managing user sessions. This approach ensures privacy by avoiding the storage of personally identifiable information and using secure data handling methods that comply with GDPR regulations.