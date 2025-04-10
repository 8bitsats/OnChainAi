---
description: AstraDB Integration Guide with Privy
---

# Astra DB

### Introduction

This guide explains how we've integrated AstraDB with the Privy application, a privacy-focused messaging platform built on Solana. AstraDB provides a scalable, cloud-native database service that complements Privy's on-chain operations with efficient off-chain data storage.

### What is AstraDB?

AstraDB is a cloud-native NoSQL database service built on Apache Cassandra. It offers:

* Serverless, pay-as-you-go database
* Vector search capabilities
* REST and GraphQL APIs
* Global distribution
* Enterprise-grade security

### Why AstraDB for Privy?

Privy is a Solana application for spam-free private messaging through sharable user links. While the Solana blockchain handles the core functionality like user authentication and token management, storing large amounts of message data on-chain would be:

1. **Expensive** - Each transaction on Solana costs fees
2. **Inefficient** - Blockchain storage is optimized for verification, not data retrieval
3. **Limited** - On-chain storage has size constraints

AstraDB solves these problems by providing:

1. **Cost-effective storage** for message data
2. **Fast retrieval** of messages
3. **Scalability** to handle growing message volumes
4. **Flexible querying** to search messages by various criteria

### Architecture Overview

Our integration follows this architecture:

1. **Solana Blockchain**: Handles user authentication, token management, and core application logic
2. **Rust Server**: Acts as a bridge between the client and both Solana and AstraDB
3. **AstraDB**: Stores and retrieves message data
4. **Client**: Encrypts messages before sending to the server for storage

### Implementation Details

#### Core Components

1. **AstraDB Module** (`/server/src/db/astra_db.rs`):
   * Configuration management
   * Collection creation
   * Document storage and retrieval
   * Message-specific operations
2. **Message Store** (`/server/src/db/message_store.rs`):
   * High-level interface for message operations
   * Delegates to AstraDB module for actual storage
3. **Test Scripts**:
   * TypeScript test (`test_astra_ts.js`)
   * Rust test (`test_astra_rust.rs`)

#### Key Features

* **End-to-end encryption**: Messages are encrypted client-side before storage
* **User-based message retrieval**: Efficiently fetch messages for a specific user
* **Metadata support**: Store additional information with messages (sender fingerprint, passkey status)
* **Unique message IDs**: Generate collision-resistant IDs for messages

### Setup Instructions

#### Prerequisites

1. Create an AstraDB account at [astra.datastax.com](https://astra.datastax.com)
2. Create a database and obtain your API credentials
3. Install required dependencies:
   * Rust and Cargo
   * Node.js and npm (for TypeScript test)
   * PostgreSQL client library (`libpq`)

#### Environment Configuration

Add the following to your `.env` file:

```
ASTRA_DB_TOKEN=AstraCS:your-token-here
DATABASE_URL="https://your-database-id-here-us-east-2.apps.astra.datastax.com"
```

**Important**: The `DATABASE_URL` should be the direct URL to your AstraDB instance, which you can find in your AstraDB dashboard. Make sure to include the full URL including the `https://` prefix and `.apps.astra.datastax.com` suffix.

#### Running the Tests

**TypeScript Test**

```bash
cd /path/to/privy/server
node test_astra_ts.js
```

**Rust Test**

```bash
cd /path/to/privy/server
cargo run --bin test_astra_rust
```

### Using the AstraDB Integration

#### Storing a Message

```rust
use server::db::astra_db::{self, AstraConfig, MessageData, MessageMetadata};
use server::db::message_store;
use chrono::Utc;

// Load configuration
let config = astra_db::load_astra_config()?;

// Create a message
let message = message_store::create_message(
    "user_address",
    0,
    "encrypted_message_content",
    "sender_fingerprint",
    false
);

// Store the message
message_store::store_message(&config, &message).await?
```

#### Retrieving a Message

```rust
// Retrieve by ID
let message = message_store::get_message_by_id(&config, "message_id").await?;

// Search by user address
let messages = message_store::search_messages_by_user(&config, "user_address").await?;
```

### Troubleshooting

#### Common Issues

1. **Connection Errors**: If you see `ENOTFOUND` errors, check that your `DATABASE_URL` is correct and properly formatted. It should be the direct URL to your AstraDB instance.
2. **Authentication Errors**: Ensure your `ASTRA_DB_TOKEN` is valid and has the correct permissions.
3. **Collection Already Exists**: This is normal if you've run the tests before. Our code handles this case gracefully.
4.  **Compilation Errors**: If you encounter linker errors with the Rust code, make sure you have the PostgreSQL client library installed:

    ```bash
    # On macOS
    brew install libpq
    brew link --force libpq
    ```

### Why This Integration is Innovative

#### For Solana

This integration demonstrates a hybrid approach that leverages the strengths of both blockchain and traditional database technologies:

1. **Blockchain for what it does best**: Security, authentication, and financial transactions
2. **Database for what it does best**: Efficient data storage and retrieval

This pattern can be applied to many Solana applications that need to store large amounts of data while maintaining the security and decentralization benefits of blockchain.

#### For AI

The integration creates opportunities for AI-powered features:

1. **Message analysis**: AI can analyze message patterns to detect spam or abuse
2. **Smart categorization**: Automatically categorize messages based on content
3. **Personalized experiences**: Learn user preferences to enhance the messaging experience

#### For User Privacy

This architecture enhances user privacy in several ways:

1. **Client-side encryption**: Messages are encrypted before leaving the user's device
2. **Minimal on-chain footprint**: Less public blockchain data means more privacy
3. **Controlled data access**: Only authorized users can access their messages
4. **Separation of concerns**: Authentication on blockchain, content in database

### Conclusion

The AstraDB integration with Privy demonstrates how modern applications can combine the best of blockchain technology with cloud-native databases. This hybrid approach provides the security and decentralization benefits of Solana while leveraging the scalability and efficiency of AstraDB for data storage.

By following this pattern, developers can build applications that are both decentralized and scalable, providing users with the privacy and performance they expect from modern applications.
