# Task: To create a detailed thread model and architecutre review.
1. We will do the threat model in depth, in multiple phases. 
2. Start with iniital overview, and then we will deep dive to each omponent


# Service: Sprinklr Live Chat: A centralized SAAS platform that is used by clients (orgnizarions) to implmenet live chat feature in their web-app. 

1. The client first logs in to sprinklr dashboard, authenticates
2. The backend team at sprinklr verifies the client, which is semi-autoated
3. Once done, they can download the SDK, and generate API Key and secret to embed the SDK

# Use-case:
For this live chat to work smartly, the client can upload list of FAQ, and knowledge graph, geenric to there customer. THe client would be a finanical insitution (HDFC< ICICI, etc.) .

At sprinkklr, wethey have a LLM model, that ingestsx knowlege graphs / FAQ and details , to give context to LLM fopr each client. This will be used to answer queries of uses coming from that client

# Flow
1. the end-user logs in to a client, and avails a valid session.
2. The live chat opens with authneitcated handshake with sprinklr,  and gets a live chat window to get his queries resolved.
3. FOr real-time chat, sprinlklr uses websockets.
Note: The user for each client should be trated as unique and isolated


# Task 1: Create a system design and threat model for this. Use mermaid for components and DFD.
