# SDK Usage

This section explains how the Sprinklr Live Chat SDK works, its main APIs, and how to initialize and use it in your application.

## Initialization

To use the SDK, import it into your project and initialize it with your organization’s credentials:

```js
import { SprinklrChatSDK } from 'sprinklr-live-chat-sdk';

const chat = new SprinklrChatSDK({
  orgId: 'YOUR_ORG_ID',
  apiKey: 'YOUR_API_KEY',
  environment: 'production', // or 'staging'
});
```

## Main APIs

- **connect()**: Establishes a secure connection to Sprinklr SaaS.
- **startChat(sessionToken, userInfo)**: Initiates a chat session for the authenticated user.
- **sendMessage(message)**: Sends a message to the chat.
- **onMessage(callback)**: Registers a callback to receive incoming messages.
- **disconnect()**: Closes the chat session and cleans up resources.

## Event Handling

The SDK provides event hooks for chat lifecycle events:

```js
chat.on('connected', () => { /* handle connection */ });
chat.on('message', (msg) => { /* handle incoming message */ });
chat.on('disconnected', () => { /* handle disconnect */ });
chat.on('error', (err) => { /* handle errors */ });
```

## Security

- All communication is encrypted (TLS/mTLS).
- Session tokens and API keys must be kept secure and never exposed in client-side code.

See the platform-specific integration guides for more details.
