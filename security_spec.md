# TalkFlow Security Specification

## Data Invariants
1. A user can only modify their own profile data.
2. A chat room must have at least one member (the creator) upon creation.
3. Only members of a room can read or write messages in that room.
4. Messages are immutable once sent (no editing for this simple version, or very restricted).
5. All IDs must be strictly validated.

## The Dirty Dozen (Payload-First Attack Targets)
1. **Identity Spoofing**: Attempt to create a room with `createdBy` pointing to another user.
2. **Membership Bypass**: Attempt to read messages in a room where the user is not in the `members` list.
3. **Ghost Room**: Create a room without the creator in the `members` array.
4. **Message Forgery**: Send a message with a `senderId` that doesn't match the authenticated user.
5. **PII Leak**: Attempt to read any room metadata without being authenticated.
6. **Room Hijack**: A non-creator attempting to rename a room or delete it.
7. **Orphaned Message**: Attempting to write a message to a room ID that doesn't exist.
8. **Shadow Field Injection**: Adding an `isAdmin: true` field to a user profile or room.
9. **Timestamp Manipulation**: Sending a client-side timestamp instead of a server-side one.
10. **ID Poisoning**: Using a massive 1MB string as a Room ID to bloat the database.
11. **Bulk Scrape**: Attempting to list all rooms without any filter (should be restricted).
12. **Member Escalation**: A user adding themselves to a room they weren't invited to.

## Test Runner (Logic Check)
I will implement `firestore.rules` that block these 12 patterns specifically using `isValidId()`, `hasOnly()`, and relational checks.
