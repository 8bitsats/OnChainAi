---
description: Decentralized On-Chain AI Private Messaging on Solana
---

# ChatMCP

### What is ChatMCP? (Explained Like You're 5)

Imagine you have a magic box where you can send secret messages to your friends. Only your friends can open the box with their special key. Now, imagine this magic box is super smart and can talk back to you! That's ChatMCP!

* It's a **private messaging app** where your messages are kept secret
* It uses **blockchain magic** (Solana) to make sure no one can cheat
* It has a **smart computer brain** (AI) that can help you with things
* Your messages are stored in a **special vault** (AstraDB) that only you can unlock

### Why ChatMCP is Special

1. **Super Private**: Your messages are locked with special codes that only you and your friends have
2. **No Spam**: People need special tokens to send you messages, so no unwanted messages
3. **Smart AI**: The system has AI that can help you understand messages or create new ones
4. **Decentralized**: No single company controls your messages - they're spread across many computers
5. **Blockchain Powered**: Uses Solana blockchain for speed and security

### System Architecture (The Big Picture)

```
┌─────────────────────────────────────┐     ┌──────────────────┐
│ User's Device                        │     │ Solana Blockchain│
│  ┌─────────────┐    ┌─────────────┐ │     │  ┌────────────┐  │
│  │ ChatMCP App │    │ Wallet      │ │     │  │ Rust       │  │
│  │ (React/     │    │ Connection  │ │     │  │ Smart      │  │
│  │  Next.js)   │◄───┤ (Phantom)   │ │     │  │ Contracts  │  │
│  └─────┬───────┘    └─────────────┘ │     │  └────┬───────┘  │
│        │                            │     │       │          │
│  ┌─────▼───────┐    ┌─────────────┐ │     │       │          │
│  │ Encryption  │    │ Client-side │ │◄────┼───────┘          │
│  │ Module      │◄───┤ MCP Agent   │ │     │                  │
│  └─────┬───────┘    └─────────────┘ │     └──────────────────┘
└────────┼─────────────────────────────┘              ▲
         │                                            │
         ▼                                            │
┌──────────────────┐     ┌────────────────────────────┴───┐
│ ChatMCP Services │     │ MCP Server Network             │
│ ┌──────────────┐ │     │ ┌────────────┐ ┌────────────┐  │
│ │ Auth Module  │ │     │ │ AI Model   │ │ Validator  │  │
│ └──────────────┘ │     │ │ Execution  │ │ Nodes      │  │
│ ┌──────────────┐ │     │ └────────────┘ └────────────┘  │
│ │ Encryption   │◄┼─────┼─►┌────────────┐ ┌────────────┐  │
│ │ Key Registry │ │     │ │ AstraDB    │ │ Proof      │  │
│ └──────────────┘ │     │ │ Storage    │ │ Generation │  │
└──────────────────┘     │ └────────────┘ └────────────┘  │
                         └─────────────────────────────────┘
```

### How It Works (Step by Step)

#### 1. User Registration

```
┌──────────┐     ┌────────────┐     ┌─────────────┐
│  User    │     │  Solana    │     │  ChatMCP    │
│  Device  │     │ Blockchain │     │  Services   │
└────┬─────┘     └─────┬──────┘     └──────┬──────┘
     │                 │                    │
     │ Connect Wallet  │                    │
     ├────────────────►│                    │
     │                 │                    │
     │ Create Username │                    │
     │ & Password      │                    │
     ├─────────────────┼───────────────────►│
     │                 │                    │
     │                 │  Create User       │
     │                 │  Account On-Chain  │
     │                 │◄───────────────────┤
     │                 │                    │
     │ Confirmation    │                    │
     │◄────────────────┤                    │
     │                 │                    │
```

#### 2. Sending a Message

```
┌──────────┐     ┌────────────┐     ┌─────────────┐     ┌─────────┐
│  Sender  │     │  Solana    │     │  ChatMCP    │     │ AstraDB │
│  Device  │     │ Blockchain │     │  Services   │     │ Storage │
└────┬─────┘     └─────┬──────┘     └──────┬──────┘     └────┬────┘
     │                 │                    │                 │
     │ Encrypt Message │                    │                 │
     ├─────────────────┼────────────────────┼────────────────►│
     │                 │                    │                 │
     │ Pay Token Fee   │                    │                 │
     ├────────────────►│                    │                 │
     │                 │                    │                 │
     │                 │ Verify Payment     │                 │
     │                 │ & Record Message   │                 │
     │                 ├───────────────────►│                 │
     │                 │                    │                 │
     │ Confirmation    │                    │                 │
     │◄────────────────┤                    │                 │
     │                 │                    │                 │
```

#### 3. Reading Messages

```
┌──────────┐     ┌────────────┐     ┌─────────────┐     ┌─────────┐
│ Recipient │     │  Solana    │     │  ChatMCP    │     │ AstraDB │
│  Device   │     │ Blockchain │     │  Services   │     │ Storage │
└────┬─────┘     └─────┬──────┘     └──────┬──────┘     └────┬────┘
     │                 │                    │                 │
     │ Request Messages│                    │                 │
     ├─────────────────┼────────────────────┼────────────────►│
     │                 │                    │                 │
     │                 │                    │ Fetch Encrypted │
     │                 │                    │ Messages        │
     │                 │                    │◄────────────────┤
     │                 │                    │                 │
     │                 │                    │ Return Messages │
     │◄────────────────┼────────────────────┤                 │
     │                 │                    │                 │
     │ Decrypt Messages│                    │                 │
     │ with User Keys  │                    │                 │
     ├────────────────►│                    │                 │
     │                 │                    │                 │
     │ View Messages   │                    │                 │
     │                 │                    │                 │
```

### Key Components

#### 1. User Interface

The ChatMCP interface features a sleek, futuristic design with neon green and purple gradients, providing an immersive cyberpunk experience while maintaining usability.

* **Connect Wallet**: Users connect their Solana wallet (like Phantom) to access the app
* **Message Dashboard**: View and manage all received messages
* **Crate System**: Organize messages into categories (called "crates")
* **Token Management**: Purchase and manage ChatMCP tokens for receiving messages

#### 2. Blockchain Layer

The Solana blockchain provides the foundation for ChatMCP's security and decentralization:

* **Smart Contracts**: Written in Rust using the Anchor framework
* **On-chain Storage**: Stores user accounts, token balances, and message metadata
* **Transaction Verification**: Ensures all actions follow the rules of the system
* **Token Economy**: Manages the ChatMCP token system for spam prevention

#### 3. AstraDB Integration

AstraDB serves as the scalable, decentralized database for message storage:

* **Encrypted Storage**: Securely stores all message content in encrypted form
* **Fast Retrieval**: Enables quick access to messages when needed
* **Scalability**: Can handle millions of messages without performance issues
* **Decentralized**: Data is distributed across multiple nodes for reliability

#### 4. AI Capabilities

The MCP (Master Control Program) provides AI capabilities to enhance the messaging experience:

* **Message Analysis**: Helps categorize and prioritize messages
* **Content Generation**: Assists users in drafting responses
* **Language Translation**: Automatically translates messages between languages
* **Spam Detection**: Identifies potential spam even within encrypted messages

### Technical Deep Dive

#### Authentication Flow

ChatMCP uses a unique authentication system that combines blockchain wallet authentication with password-based encryption:

1. **Wallet Connection**: Proves ownership of a Solana address
2. **Password Creation**: Used to derive encryption keys (not stored anywhere)
3. **Account Creation**: On-chain account linked to wallet address
4. **Key Derivation**: Encryption keys derived from password when needed

#### Message Encryption

All messages in ChatMCP are encrypted using a hybrid encryption scheme:

1. **Symmetric Encryption**: AES-256 for message content
2. **Asymmetric Encryption**: RSA for key exchange
3. **Key Management**: Keys derived from user password, never stored
4. **End-to-End**: Messages can only be decrypted by the intended recipient

#### Token Economy

The ChatMCP token system prevents spam and funds the network:

1. **Token Purchase**: Users buy tokens with Solana (100 tokens per SOL)
2. **Message Cost**: Each message received costs one token
3. **Token Distribution**: Tokens are distributed to validators and AI nodes
4. **Refund Mechanism**: Tokens for spam messages can be refunded

### Getting Started

#### For Users

1. **Install a Solana Wallet**: Download Phantom or another Solana wallet
2. **Visit ChatMCP**: Go to https://chatmcp.vercel.app
3. **Connect Your Wallet**: Click "Connect to MCP" and approve the connection
4. **Create an Account**: Choose a username and secure password
5. **Buy Tokens**: Purchase ChatMCP tokens to receive messages
6. **Share Your Link**: Share your unique ChatMCP link with others

#### For Developers

1. **Clone the Repository**: `git clone https://github.com/your-org/chatmcp.git`
2. **Install Dependencies**: `cd chatmcp && yarn install`
3. **Set Up Environment**: Create `.env` files with your API keys
4. **Run Development Server**: `yarn dev`
5. **Explore the Code**: Check out the `server` and `frontend` directories

### Future Roadmap

* **Mobile App**: Native mobile applications for iOS and Android
* **Advanced AI**: More sophisticated AI capabilities for message processing
* **Group Messaging**: Secure group conversations with shared encryption
* **File Sharing**: Encrypted file transfer capabilities
* **DAO Governance**: Community governance of the ChatMCP protocol

### Conclusion

ChatMCP represents the future of secure, private communication in the web3 era. By combining the security of blockchain, the privacy of end-to-end encryption, and the power of AI, ChatMCP creates a messaging platform that respects user privacy while providing advanced features.

Join us in building a more private, secure, and intelligent communication platform for the decentralized web!
