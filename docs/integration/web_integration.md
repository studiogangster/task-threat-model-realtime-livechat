# Web Integration Guide

This section provides step-by-step instructions for integrating the Sprinklr Live Chat SDK into a web application.

## Prerequisites

- Access to the Sprinklr Live Chat SDK (npm package or CDN)
- Organization credentials (orgId, apiKey)
- A modern JavaScript/TypeScript web application

## Installation

Install the SDK via npm:

```bash
npm install sprinklr-live-chat-sdk
```

Or include via CDN:

```html
<script src="https://cdn.sprinklr.com/live-chat-sdk.min.js"></script>
```

## Initialization

Import and initialize the SDK in your app:

```js
import { SprinklrChatSDK } from 'sprinklr-live-chat-sdk';

const chat = new SprinklrChatSDK({
  orgId: 'YOUR_ORG_ID',
  apiKey: 'YOUR_API_KEY',
  environment: 'production',
});
```

## Authentication

- Obtain a session token for the end user from your backend.
- Pass the session token to the SDK when starting a chat.

## Starting a Chat

```js
chat.connect();
chat.startChat(sessionToken, { name: 'User Name', email: 'user@example.com' });
```

## Handling Messages

```js
chat.on('message', (msg) => {
  // Display message in your chat UI
});
```

## Disconnecting

```js
chat.disconnect();
```

## Security Best Practices

- Never expose API keys or sensitive credentials in client-side code.
- Use HTTPS for all communications.
- Validate and sanitize all user input.

See the troubleshooting section for common integration issues.
