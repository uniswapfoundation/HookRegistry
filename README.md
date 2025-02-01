# HookRegistry

HookRegistry is an open-source platform that unites curated hooks for the Uniswap V4 ecosystem, enabling developers, companies, and products to share and manage custom hooks efficiently. This repository serves as the primary location for HookList JSON schemas, community-contributed HookLists, and version control of HookLists curated by trusted entities.

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
List URL: 
List Name: 
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

