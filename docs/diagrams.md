# 🗺️ Architecture and Product Diagrams

These diagrams describe checked-in implementation unless marked **Proposed**. Update them when routes, data records, or service boundaries change.

## 1. User journey

```mermaid
flowchart TD
  A([Start]) --> B[Landing page]
  B --> C[Create trip]
  C --> D[Enter preferences]
  D --> E{Profile cached?}
  E -- No --> F[Google OAuth]
  F --> G[Fetch Google profile]
  E -- Yes --> H[Build prompt]
  G --> H
  H --> I[Call Gemini from browser]
  I --> J[Receive generated text]
  J --> K[Parse JSON]
  K --> L[Write AITrips document]
  L --> M[Open trip details]
  M --> N[Render overview, hotels and places]
  M --> O[My Trips page]
```

## 2. Current component and service map

```mermaid
flowchart TB
  subgraph Browser[Browser application]
    Main[src/main.jsx]
    Routes[React Router]
    Hero[Landing / Hero]
    Create[CreateTrip]
    List[MyTrips]
    Detail[Trip detail page]
    Services[Service modules]
    Main --> Routes
    Routes --> Hero
    Routes --> Create
    Routes --> List
    Routes --> Detail
    Create --> Services
    List --> Services
    Detail --> Services
  end
  Services --> OAuth[Google OAuth / User Info]
  Services --> Gemini[Gemini SDK]
  Services --> Firestore[(Cloud Firestore)]
```

## 3. Current trip data lifecycle

```mermaid
sequenceDiagram
  actor User
  participant Form as CreateTrip
  participant OAuth as Google OAuth
  participant Model as Gemini
  participant DB as Firestore
  participant Detail as Trip Details
  User->>Form: Submit trip preferences
  Form->>Form: Check cached profile
  opt Profile missing
    Form->>OAuth: Start sign-in
    OAuth-->>Form: Access token
    Form->>OAuth: Fetch profile
    OAuth-->>Form: Profile data
  end
  Form->>Model: Send AI prompt
  Model-->>Form: Generated text
  Form->>Form: JSON.parse response
  Form->>DB: setDoc(AITrips/{id})
  DB-->>Form: Write completes
  Form->>Detail: Navigate to /view-trip/{id}
  Detail->>DB: getDoc(AITrips/{id})
  DB-->>Detail: Trip document
  Detail-->>User: Render saved itinerary
```

## 4. Current Firestore record shape

```mermaid
erDiagram
  AITrips {
    string document_id
    string id
    string userEmail
    object userSelection
    object tripData
  }
```

This reflects fields written by the client. It does **not** mean `userEmail` securely enforces ownership.

## 5. Proposed trusted request boundary

```mermaid
flowchart LR
  Client[React client] --> Session[Verified identity]
  Client --> Endpoint[Server-side trip endpoint]
  Endpoint --> Limits[Validation and rate limit]
  Limits --> Model[Gemini API with server secret]
  Endpoint --> Rules[Authorized Firestore access]
  Model --> Schema[Validate model response]
  Schema --> Store[(Firestore)]
  Rules --> Store
  Store --> Endpoint
  Endpoint --> Client
```

## 6. CI workflow

```mermaid
flowchart TD
  Push[Push to main] --> Checkout[Checkout]
  PR[Pull request] --> Checkout
  Manual[Manual dispatch] --> Checkout
  Checkout --> Node[Set up Node 20]
  Node --> Install[npm ci]
  Install --> Lint[npm run lint]
  Lint --> Build[npm run build]
  Build --> Verify[Check dist/index.html and dist/assets]
  Verify --> Pass([Workflow succeeds])
```

## 7. Generation failure and recovery path

```mermaid
flowchart TD
  Submit[Submit trip] --> Auth{Profile available?}
  Auth -- No --> Login[Sign in]
  Auth -- Yes --> Generate[Generate AI response]
  Login --> Generate
  Generate --> Response{Non-empty response?}
  Response -- No --> Error[Show actionable error]
  Response -- Yes --> Parse[Parse and validate JSON]
  Parse --> Valid{Schema valid?}
  Valid -- No --> Error
  Valid -- Yes --> Save[Save trip]
  Save --> Saved{Write successful?}
  Saved -- No --> Retry[Retry safely or show error]
  Saved -- Yes --> Open[Open details]
```

The explicit error and validation nodes are recommended behavior. They are not all implemented in the current source.

## Diagram maintenance

- Label proposed architecture as proposed.
- Do not draw integrations absent from source.
- Keep route labels synchronized with React Router.
- Align data fields with the Firestore write shape.
- Never put secrets, personal data, or real trip records in diagrams.
