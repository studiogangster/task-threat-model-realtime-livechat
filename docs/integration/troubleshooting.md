# Troubleshooting & FAQ

This section provides solutions to common issues encountered during the integration of the Sprinklr Live Chat SDK, as well as frequently asked questions.

## Common Issues

### 1. SDK Fails to Connect

- **Check network connectivity and firewall settings.**
- **Verify orgId and apiKey are correct.**
- **Ensure the environment (production/staging) is set properly.**
- **Check for typos in initialization code.**

### 2. Authentication Errors

- **Ensure the session token is valid and not expired.**
- **Confirm the backend is issuing tokens correctly.**
- **Check time synchronization between client and server.**

### 3. Messages Not Received

- **Verify the chat is connected before sending/receiving messages.**
- **Check event handler registration (onMessage, on).**
- **Inspect browser or device console/logs for errors.**

### 4. UI Not Updating

- **Ensure your chat UI is listening for SDK events.**
- **Debug with sample data to isolate UI issues from SDK issues.**

### 5. CORS or HTTPS Errors (Web)

- **Use HTTPS for all endpoints.**
- **Check CORS headers on your backend and Sprinklr endpoints.**

## Debugging Tips

- Enable verbose logging in the SDK if available.
- Use browser dev tools or mobile device logs to inspect network requests.
- Test with sample credentials and a minimal app to isolate issues.

## Support

If issues persist, contact Sprinklr support with logs, error messages, and integration details.
