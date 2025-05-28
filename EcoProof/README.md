# EcoProof Smart Contract

A decentralized environmental proof marketplace built on the Stacks blockchain using Clarity smart contracts. This system enables the creation, minting, trading, and retirement of EcoProof credits tied to environmental projects.

## Overview

This smart contract provides a complete ecosystem for EcoProof management, including:
- Project registration for environmental initiatives
- EcoProof credit minting by authorized issuers
- Marketplace functionality for trading EcoProof credits
- Credit retirement system for offsetting environmental impact
- Role-based access control

## Features

### Core Functionality
- **Project Management**: Create and store environmental projects with names and descriptions
- **EcoProof Minting**: Authorized issuers can mint EcoProof credits tied to specific projects
- **Marketplace Trading**: Users can list EcoProof credits for sale and purchase from others
- **Credit Retirement**: Permanent retirement of EcoProof credits to offset environmental impact
- **Authorization System**: Admin-controlled issuer authorization

### Security Features
- Input validation for all parameters
- Ownership verification for credit operations
- Prevention of self-trading
- Balance verification before purchases
- Protection against invalid principal addresses

## Contract Architecture

### Data Structures

#### Maps
- `eco-projects`: Stores project information (ID → {name, description})
- `authorized-issuers`: Tracks authorized EcoProof issuers (principal → bool)
- `carbon-credits`: Stores EcoProof credit details (ID → {project-id, quantity, owner, is-retired})
- `marketplace`: Active EcoProof listings (ID → {credit-id, seller, price})

#### Variables
- `project-counter`: Auto-incrementing project ID counter
- `credit-counter`: Auto-incrementing credit ID counter
- `listing-counter`: Auto-incrementing listing ID counter

### Constants
- `system-admin`: Contract deployer with administrative privileges
- `max-project-name-length`: 100 characters
- `max-project-desc-length`: 300 characters
- `min-credit-amount`: 1 credit minimum
- `max-credit-amount`: 1,000,000 credits maximum
- `min-sale-price`: 1 microSTX minimum

## Public Functions

### Administrative Functions

#### `authorize-issuer`
```clarity
(authorize-issuer (new-issuer principal))
```
Authorizes a new principal to mint EcoProof credits. Only callable by system admin.

**Parameters:**
- `new-issuer`: Principal address to authorize

**Returns:** `(ok true)` on success

### Project Management

#### `create-project`
```clarity
(create-project (project-name (string-ascii 100)) (project-desc (string-ascii 300)))
```
Creates a new environmental project.

**Parameters:**
- `project-name`: Name of the project (1-100 characters)
- `project-desc`: Project description (1-300 characters)

**Returns:** Project ID on success

### Credit Operations

#### `mint-credits`
```clarity
(mint-credits (project-id uint) (credit-amount uint) (recipient principal))
```
Mints new EcoProof credits for a project. Only callable by authorized issuers.

**Parameters:**
- `project-id`: Valid project ID
- `credit-amount`: Number of credits to mint (1-1,000,000)
- `recipient`: Principal to receive the credits

**Returns:** Credit ID on success

#### `retire-credit`
```clarity
(retire-credit (credit-id uint))
```
Permanently retires an EcoProof credit. Only callable by credit owner.

**Parameters:**
- `credit-id`: ID of credit to retire

**Returns:** `(ok true)` on success

### Marketplace Functions

#### `list-for-sale`
```clarity
(list-for-sale (credit-id uint) (sale-price uint))
```
Lists an EcoProof credit for sale. Only callable by credit owner.

**Parameters:**
- `credit-id`: ID of credit to sell
- `sale-price`: Price in microSTX

**Returns:** Listing ID on success

#### `purchase-credit`
```clarity
(purchase-credit (listing-id uint))
```
Purchases a listed EcoProof credit.

**Parameters:**
- `listing-id`: ID of marketplace listing

**Returns:** `(ok true)` on success

## Read-Only Functions

### `get-project`
Returns project information by ID.

### `get-credit`
Returns EcoProof credit details by ID.

### `get-listing`
Returns marketplace listing details by ID.

### `is-authorized-issuer`
Checks if a principal is authorized to mint EcoProof credits.

## Error Codes

| Code | Description |
|------|-------------|
| u100 | Not system admin |
| u101 | Not authorized issuer |
| u102 | Not credit owner |
| u103 | Credit not found |
| u104 | Not credit owner for listing |
| u105 | Credit already retired |
| u106 | Credit not found for listing |
| u107 | Owner/seller mismatch |
| u108 | Insufficient balance |
| u109 | Credit not found for purchase |
| u110 | Listing not found |
| u111 | Cannot buy own credit |
| u200 | Invalid principal |
| u201 | Cannot authorize admin |
| u202 | Invalid project name |
| u203 | Invalid project description |
| u204 | Project not found |
| u205 | Invalid credit amount |
| u206 | Invalid recipient |
| u207 | Credit not found for retirement |
| u208 | Credit already retired |
| u209 | Credit not found for listing |
| u210 | Invalid sale price |
| u211 | Listing not found for purchase |
| u212 | Cannot purchase retired credit |

## Usage Examples

### Setting up the system
1. Deploy the contract
2. Admin authorizes issuers: `(authorize-issuer 'SP1234...)`
3. Create projects: `(create-project "Solar Farm Project" "Renewable energy initiative")`
4. Mint credits: `(mint-credits u1 u1000 'SP5678...)`

### Trading EcoProof credits
1. List for sale: `(list-for-sale u1 u500)`
2. Purchase EcoProof credit: `(purchase-credit u1)`
3. Retire EcoProof credit: `(retire-credit u1)`

## Security Considerations

- Only system admin can authorize issuers
- EcoProof credits can only be operated on by their owners
- Retired EcoProof credits cannot be traded
- Users cannot purchase their own EcoProof credits
- All inputs are validated before processing
- STX balance is verified before purchases

## Development

### Prerequisites
- Stacks blockchain node
- Clarinet development environment
- Basic understanding of Clarity smart contract language
