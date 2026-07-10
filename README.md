# Real-Time Chat & Search Assessment

I have completed both challenges for this assessment. Below is a summary of the implementations and instructions on how to set up and test the project.

## Completed Challenges

### 1. Real-Time Messages with Pusher
- **Backend**: Integrated the official `pusher` SDK. The text message and image-upload routes now broadcast a `new-message` event to the `chat-channel`. Deletions trigger a `message-deleted` event containing the message ID.
- **Frontend**: Configured the React app with `pusher-js` to listen for these events. The message feed automatically appends new messages (with duplicate checks for the sender) and removes deleted messages in real-time.
- **Graceful Fallback**: If Pusher environment variables are not set, the app logs a warning but continues to function normally using standard HTTP requests.

### 2. Message Search & Real-Time Filter
- **Backend Endpoint**: Implemented `GET /api/messages/search?q=term`. It validates the query parameter, trims it, performs a case-insensitive search across usernames and message bodies, and returns up to 100 of the newest matches.
- **Frontend UI & Debounce**: Added a search bar above the message feed. Typing triggers the search endpoint with a `300ms` debounce to prevent overloading the backend.
- **Feed Toggle & Live Sync**: Clearing the search query instantly reverts the UI to the live feed. If a message is deleted while in search mode, it is immediately removed from the search results in real-time.

---

## Setup & Running the App

### Prerequisites
- Node.js 18+ (Node 22+ recommended)
- `pnpm` (recommended) or `npm` / `yarn`

### 1. Environment Variables

Before running, you need to configure Pusher credentials. Copy the example environment files in both folders:

**Backend (`api/.env`)**:
```env
PORT=3001
PUSHER_APP_ID=your_pusher_app_id
PUSHER_KEY=your_pusher_key
PUSHER_SECRET=your_pusher_secret
PUSHER_CLUSTER=your_pusher_cluster
```

**Frontend (`app/.env`)**:
```env
REACT_APP_API_URL=http://localhost:3001
REACT_APP_PUSHER_KEY=your_pusher_key
REACT_APP_PUSHER_CLUSTER=your_pusher_cluster
```

### 2. Running Locally

Start the backend and frontend in separate terminals:

**Backend Setup**:
```bash
cd api
pnpm install
pnpm dev # starts on http://localhost:3001
```

**Frontend Setup**:
```bash
cd app
pnpm install
pnpm dev # starts on http://localhost:3000
```

---

## Verification

To verify the real-time functionality:
1. Open `http://localhost:3000` in two separate browser windows.
2. Send a message in one window; it will instantly appear in the other without a page refresh.
3. Delete a message; it will immediately disappear from both windows.
4. Type a query in the search bar to test the debounced filtering, and verify that deletions also apply to the active search results.
