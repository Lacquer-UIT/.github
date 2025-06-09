# Lacquer 🇻🇳
Ever looked at a bowl of phở and thought “damn, I wish I knew what’s goin on”?

Boom. **Lacquer**.

Learn Vietnamese and English, flex AI that recognizes food, landmarks, and weird-ass cultural stuff just by pointing your camera.

Snap. Post. React. Chat. Add friends.

It’s Duolingo but make it ✨social media✨ with Vietnamese spice.

You even earn badges for being outside. Like Pokémon GO but for heritage 😭

Download it before Gen Alpha starts gatekeeping Vietnamese culture =)))
## Tech Stack

- **Frontend:** [Flutter](https://flutter.dev)  
- **Backend:** [Express.js](https://expressjs.com)  
- **Auth:** [Google OAuth 2.0](https://developers.google.com/identity)  
- **Database:** [MongoDB + Mongoose](https://mongoosejs.com)  
- **Image Hosting:** [Cloudinary](https://cloudinary.com)  
- **AI/ML:** [Google Gemini](https://deepmind.google/technologies/gemini/)  
- **NLP & Vision:**  
  - [Google Translate API](https://cloud.google.com/translate)  
  - [Google Vision API](https://cloud.google.com/vision)  
- **Deployment:** [Railway](https://railway.app)

## Features

- 🧠 AI-assisted translations and image recognition  
- 🔐 Google login with OAuth  
- 🗣 Dictionary with pronunciation + image support  
- 💬 Real-time chat (DMs + groups)  
- 📸 Social posts with emoji reactions  
- 📚 Flashcard decks for vocab learning  
- 🏆 Achievement badges  
- 🤝 Friend system (request, accept, block)

## 🗃 Database Schema

```mermaid
erDiagram
    User {
        ObjectId _id PK
        String username
        String email UK
        String passwordHash
        String avatar
        String authProvider
        String googleId
        Boolean isVerified
        String verificationToken
        ObjectId[] badges FK
        ObjectId[] friendships FK
        String about
        DateTime createdAt
        DateTime updatedAt
    }
    Badge {
        ObjectId _id PK
        String name
        String iconUrl
        DateTime createdAt
        DateTime updatedAt
    }
    Friendship {
        ObjectId _id PK
        ObjectId requester FK
        ObjectId recipient FK
        String status
        ObjectId blocker FK
        DateTime createdAt
        DateTime updatedAt
    }
    Post {
        ObjectId _id PK
        ObjectId owner FK
        String imageUrl
        String caption
        ObjectId[] visibleTo FK
        Object[] reactions
        DateTime createdAt
        DateTime updatedAt
    }
    Chat {
        ObjectId _id PK
        Boolean isGroup
        String name
        String avatar
        ObjectId[] participants FK
        ObjectId[] admins FK
        DateTime createdAt
        DateTime updatedAt
    }
    Message {
        ObjectId _id PK
        ObjectId chat FK
        ObjectId sender FK
        String text
        ObjectId[] readBy FK
        DateTime createdAt
        DateTime updatedAt
    }
    Deck {
        ObjectId _id PK
        String owner
        String title
        String description
        String img
        Boolean isDone
        ObjectId[] tags FK
        ObjectId[] cards FK
        DateTime createdAt
        DateTime updatedAt
    }
    Tag {
        ObjectId _id PK
        String name UK
        String description
        DateTime createdAt
        DateTime updatedAt
    }
    Dictionary {
        ObjectId _id PK
        String word
        String pronunciation
        String[] img
        Object[] wordTypes
        String difficulty
        Object[] examples
    }
    DictionaryVn {
        ObjectId _id PK
        String word
        String[] pronunciations
        String[] img
        Object[] meanings
    }
    Chatbothistory {
        ObjectId _id PK
        String userId
        Object[] history
        DateTime createdAt
        DateTime updatedAt
    }
    User ||--o{ Badge : "has many badges"
    User ||--o{ Friendship : "has many friendships"
    User ||--o{ Post : "owns posts"
    User ||--o{ Chat : "participates in chats"
    User ||--o{ Message : "sends messages"
    Friendship }o--|| User : "requester"
    Friendship }o--|| User : "recipient"
    Friendship }o--o| User : "blocker"
    Post }o--|| User : "owned by user"
    Post }o--o{ User : "visible to users"
    Chat ||--o{ Message : "contains messages"
    Chat }o--o{ User : "has participants"
    Chat }o--o{ User : "has admins"
    Message }o--|| Chat : "belongs to chat"
    Message }o--|| User : "sent by user"
    Message }o--o{ User : "read by users"
    Deck }o--o{ Tag : "has tags"
    Deck }o--o{ Dictionary : "contains cards"
    Chatbothistory }o--|| User : "belongs to user (via userId)"

```

