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

## 💽 UML
```mermaid
classDiagram
    class User {
        -ObjectId _id
        -String username
        -String email
        -String passwordHash
        -String avatar
        -String authProvider
        -String googleId
        -Boolean isVerified
        -String verificationToken
        -ObjectId[] badges
        -ObjectId[] friendships
        -String about
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +getQR()
        +resetPassword()
        +verify()
        +findById()
        +findByEmail()
    }

    class Badge {
        -ObjectId _id
        -String name
        -String iconUrl
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +findById()
    }

    class Friendship {
        -ObjectId _id
        -ObjectId requester
        -ObjectId recipient
        -String status
        -ObjectId blocker
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +findById()
        +updateStatus()
    }

    class Post {
        -ObjectId _id
        -ObjectId owner
        -String imageUrl
        -String caption
        -ObjectId[] visibleTo
        -Reaction[] reactions
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +addReaction()
        +removeReaction()
    }

    class Reaction {
        -ObjectId user
        -String emoji
    }

    class Chat {
        -ObjectId _id
        -Boolean isGroup
        -String name
        -String avatar
        -ObjectId[] participants
        -ObjectId[] admins
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +addParticipant()
        +removeParticipant()
    }

    class Message {
        -ObjectId _id
        -ObjectId chat
        -ObjectId sender
        -String text
        -ObjectId[] readBy
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +markAsRead()
    }

    class Deck {
        -ObjectId _id
        -String owner
        -String title
        -String description
        -String img
        -Boolean isDone
        -ObjectId[] tags
        -ObjectId[] cards
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +addCard()
        +removeCard()
        +addTag()
    }

    class Tag {
        -ObjectId _id
        -String name
        -String description
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +findByName()
    }

    class Dictionary {
        -ObjectId _id
        -String word
        -String pronunciation
        -String[] img
        -WordType[] wordTypes
        -String difficulty
        -Example[] examples
        +save()
        +findByWord()
        +searchByDifficulty()
    }

    class WordType {
        -String type
        -String[] definitions
    }

    class Example {
        -String phrase
        -String translation
    }

    class DictionaryVn {
        -ObjectId _id
        -String word
        -String[] pronunciations
        -String[] img
        -Meaning[] meanings
        +save()
        +findByWord()
    }

    class Meaning {
        -PartOfSpeech part_of_speech
        -Definition[] definitions
    }

    class PartOfSpeech {
        -String type
    }

    class Definition {
        -String text
        -Example[] examples
    }

    class Chatbothistory {
        -ObjectId _id
        -String userId
        -HistoryEntry[] history
        -DateTime createdAt
        -DateTime updatedAt
        +save()
        +addEntry()
        +clearHistory()
    }

    class HistoryEntry {
        -String role
        -MessagePart[] parts
    }

    class MessagePart {
        -String text
    }

    User "1" --> "*" Badge : has
    User "1" --> "*" Friendship : participates
    User "1" --> "*" Post : owns
    User "*" --> "*" Chat : participates
    User "1" --> "*" Message : sends
    User "1" --> "1" Chatbothistory : has

    Friendship --> User : requester
    Friendship --> User : recipient
    Friendship --> User : blocker

    Post --> User : owner
    Post --> User : visibleTo
    Post "1" *-- "*" Reaction : contains

    Chat "1" --> "*" Message : contains
    Chat "*" --> "*" User : participants
    Chat "*" --> "*" User : admins

    Message --> Chat : belongsTo
    Message --> User : sender
    Message --> User : readBy

    Deck "*" --> "*" Tag : taggedWith
    Deck "*" --> "*" Dictionary : contains

    Dictionary "1" *-- "*" WordType : has
    Dictionary "1" *-- "*" Example : contains

    DictionaryVn "1" *-- "*" Meaning : has
    Meaning "1" *-- "1" PartOfSpeech : has
    Meaning "1" *-- "*" Definition : contains
    Definition "1" *-- "*" Example : has

    Chatbothistory "1" *-- "*" HistoryEntry : contains
    HistoryEntry "1" *-- "*" MessagePart : has
```
