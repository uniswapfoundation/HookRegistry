# Table of Contents

- [Table of Contents](#table-of-contents)
- [HookRegistry](#hookregistry)
  - [📖 Overview](#-overview)
    - [HookRegistry is designed to bring structure and discoverability to the growing number of Uniswap V4 hooks, providing:](#hookregistry-is-designed-to-bring-structure-and-discoverability-to-the-growing-number-of-uniswap-v4-hooks-providing)
  - [What are HookLists?](#what-are-hooklists)
  - [🧩 Create your own HookList](#-create-your-own-hooklist)
    - [Via our website:](#via-our-website)
    - [If you want to create it manually:](#if-you-want-to-create-it-manually)
  - [Review Process:](#review-process)
  - [Track Your HookList:](#track-your-hooklist)
  - [📜 JSON Schema Example](#-json-schema-example)
- [Hook Metadata Flow](#hook-metadata-flow)
  - [1. Required Metadata](#1-required-metadata)
  - [2. Audit Summaries](#2-audit-summaries)
  - [3. EIP-712 Domain Information](#3-eip-712-domain-information)
  - [4. Signature Types](#4-signature-types)
  - [5. Auditor Process](#5-auditor-process)
  - [6. Developer / Deployer Process](#6-developer--deployer-process)

---

# HookRegistry

HookRegistry is an open-source platform that allows for the creation of curated hook lists for the Uniswap V4 ecosystem. Developers, companies, and products can share and manage custom lists of hooks efficiently and more securely. This repository serves as the primary location for community-contributed HookLists.

## 📖 Overview

### HookRegistry is designed to bring structure and discoverability to the growing number of Uniswap V4 hooks, providing:

- Standardized JSON Schema for HookLists to ensure consistency and interoperability;
- GitHub-Driven Management for collaborative HookList contributions and reviews;

## What are HookLists?

Just as Token Lists organize and standardize metadata for tokens in a way that any dapp can easily utilize, HookLists do the same for hooks within the Uniswap V4 ecosystem.

Think of Token Lists as a curated playlist of songs, where each song (token) has metadata like the artist, album, genre, and release date. This makes it easy for listeners to find songs by genre or album. Similarly, HookLists are like curated playlists of "hooks", each with metadata that describes its purpose, version, and compatible blockchains.

By adhering to a standardized schema, HookLists ensure that each hook is easily discoverable, consistently structured, and ready to be used in any dapp that integrates with Uniswap V4. This standardization helps developers, companies, and users to find and implement custom hooks that are reliable, transparent, and tailored for specific use cases, just like how a playlist organizes music for easy listening.

## 🧩 Create your own HookList

HookRegistry welcomes contributions of new HookLists! Follow the steps below to submit yours.

### Via our website:

Go to the hookregistry.com and follow the instructions.

### If you want to create it manually:

Schema Reference:

1. Check the HookList JSON Schema here to understand the required structure and validation rules and create your own HookList JSON;
2. Ensure your HookList passes all validation checks;
3. Upload your HookList JSON to IPFS (optionally you can link your ENS to it);
4. Go to the Issues tab and open a new issue with the title New HookList Proposal: [Your HookList Name];
5. Include the details and justification for the HookList in the issue description by the following template:

```
List Name: 
List URL: 
Link to the official homepage of the list manager:
```

## Review Process:

Our team will review your submission. Upon approval, your HookList will be added to the hooklists.json file via a pull request (PR).

## Track Your HookList:

Approved HookLists are added to hooklists.json and can be browsed via the HookRegistry platform interface.

## 📜 JSON Schema Example

Below is an example structure of a HookList JSON file:

```json
{
    "name": "Uniswap V4",
    "timestamp":"2022-04-06T22:19:09+00:00",
    "version":{"major":145,"minor":0,"patch":0},
    "keywords":["Uniswap V4","default","list"],
    "description": "A very awesome hook list, hell yeah",
    "logoURI": "https://ethereum-optimism.github.io/data/UNI/logo.png",
    "chains": {
      "1": [{
         "address": "0x7174486cDD1E9cBecd977134A10d55f357A50Ac0",
         "name": "Hook X",
         "repository": "Repository for Hook X",
         "logoURI": "https://ethereum-optimism.github.io/data/UNI/Hook.png",
         "description": "A very awesome hook, hell yeah",
         "audit": "https://audit/link",
         "version":{"major":145,"minor":0,"patch":0}
       }]
    }
}
```

For more details, see the full schema definition.

---

# Hook Metadata Flow

In addition to providing a structure for сategorizing hooks, HookRegistry indexes hooks that implement IHookMetadata to obtain hook information (metadata and completed audits).

## 1. Required Metadata

In order for the HookRegistry to fetch the hook's metadata, the hook contract must implement the following view/pure methods:

- **name()** -- The name of the hook.
- **repository()** -- A link (URI) to the repository containing the hook's source code.
- **logoURI()** -- A link (URI) to the hook's logo.
- **websiteURI()** -- A link (URI) to the hook's website.
- **description()** -- A textual description of the hook or an IPFS link containing detailed information.
- **version()** -- A version identifier or commit hash representing the hook's version.

These metadata fields are indexed once at the time of hook deployment, so treat them as effectively immutable.

---

## 2. Audit Summaries

The **auditSummaries(uint256 auditId)** method should return a completed audit summary by auditId. Unlike the core metadata, the list of audits can be extended over time, so you can add new audit summaries *after* the hook has been deployed.

- Each new audit summary can be appended to the contract's internal audit record.
- **Each added audit summary—even those included at the time of hook deployment—must emit the event `AuditSummaryRegistered` to ensure external indexers are notified of the update.**

---

## 3. EIP-712 Domain Information

To allow auditors to produce valid EIP‑712 signatures for your audit summaries, **the hook contract must return an EIP‑712[^1] DOMAIN_SEPARATOR**.

Auditors will use this domain information to generate a type hash, then sign your audit summary. Make sure you store or provide this domain information consistently so that anyone can reproduce the exact signing context.

---

## 4. Signature Types

When providing or verifying signatures over the audit summary, you might support multiple signature standards. For instance:

```solidity
enum SignatureType {
    SECP256K1,
    BLS,
    ERC1271,
    SECP256R1
}
```

The auditor will pick one of these types (e.g., SECP256K1) and produce a valid cryptographic signature accordingly.

---

## 5. Auditor Process

1. **Choose a SignatureType** (e.g., SECP256K1) that matches the contract's expected signing mechanism.

2. **Generate the EIP-712 signature** for the audit summary:
   - Fill in the EIP-712 domain fields (including the verifying contract address if necessary).
   - Hash the data (the summary) according to the domain.
   - Produce a digital signature for that hash.

3. **Deliver the signed audit summary** (including the signature and any metadata) to the hook owner or deployer, so they can store it in the hook's contract.

---

## 6. Developer / Deployer Process

1. **Implement the interface**: In your hook contract, ensure all required view methods (listed above) are present and store the metadata accordingly.

2. **Add an event for new audits**: Emit an event (AuditSummaryRegistered) whenever a new audit is added, so indexers can track updates.

3. **Deploy the contract**: Since the core metadata (name, repository, etc.) is indexed at deployment, treat it as immutable afterward.

4. **Append new audits** after deployment, if necessary. Each time an auditor provides a signed audit summary, add it to your contract's audit storage (e.g., via a function like `addAuditSummary(...)`) **and emit the AuditSummaryRegistered event to notify external indexers of the update. Note that every audit addition, including those made during the initial deployment, must trigger this event.**

---

[^1]: [EIP‑7512](https://eips.ethereum.org/EIPS/eip-7512)
