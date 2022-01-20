# ERD-721 standard

{% hint style="info" %}
**Good to know:** ERD-721 is the standard that ErdSea uses to enhance the ESDT standard and support the metadata standard enforced on Ethereum through the ERC-721 token contract.
{% endhint %}

## Introduction

ERC-721 is a standard that allows for the implementation of a standard API for NFTs inside a smart contract. The standard declares the basic functionality that needs to be implemented in order to track and transfer NFTs. In order to combine the functionality declared in ERC-721 and the structure of ESDT-NFT standard, the ERD-721 was created.

## ESDT structure

***

**ESDT** stands for Elrond Standard Digital Token.

The Elrond network natively supports the issuance of custom tokens, without the need for contracts such as ERC20. This makes all ESDT tokens scalable and easy to manage in a multishard setup. The protocol supports NFTs natively by extending the ESDT standard, adding attributes on top of the already existing ESDTs.

When issuing an ESDT-NFT one sends a request to the metachain, which then registers the token under the Account that sent the transaction. After the issue action one has to set roles on the issued token. The allowed roles are as follows:

* ESDT-NFT create
* ESDT-NFT burn
* ESDT-AddQuantity (for SFTs only)

These roles define what actions can be performed on the issued token. After the create role is set, minting tokens can be done by creating a transaction containing the token payload.

The general structure of an ESDT-NFT is as follows:

| Name       | Test                                      |
| ---------- | ----------------------------------------- |
| Quantity   | 1                                         |
| Royalties  | 750 (7.5%)                                |
| Hash       | 9834...2312                               |
| Attributes | {"background": "green", "hair": "blonde"} |
| URIs       | \[ uri-1, uri-2 ]                         |

## ERC-721 structure

ERC-721 is a standard that allows for the implementation of a standard API for NFTs on smart contracts. The standard exposes the functionality that needs to be implemented in order to track ad transfer NFTs. The structure of a smart contract implementing this standard allows for composability and enforces a clear metadata standard.

The ERC-721 full standard definition can be found [here](https://docs.openzeppelin.com/contracts/2.x/api/token/erc721).

#### Metadata standard

In order to allow applications to pull and display complex information about digital assets on a given contract, metadata is needed so these assets have additional properties. The method that each ERC-721 contract has to implement in order to be compliant (opensea compatible) is pictured below.

![](<../.gitbook/assets/image (3).png>)

The tokenURI method returns the base url to which the tokenID (nonce) is concatenated. The resource found at the URI is a json-like payload which holds information about the token. The accepted standard for this payload is shown below:

```
{
    description: "some example description",
    image: "ipfs://Qmsmv...mvjRUMW/1.png",
    name: "EXAMPLE #1",
    attributes: [
        {
            trait_type: "name",
            value: "example name"
        },
        {
            trait_type: "background",
            value: "blue"
        },
        {
            trait_type: "mouth",
            value: "surprised"
        }
    ]
}
```

## ERD-721 structure

Given that ESDT tokens are native to the Elrond protocol, managing metadata at smart contract level becomes difficult. The standard has to be extended in order to accomodate the ERC-721 features without breaking the base functionality of the underlying ESDT token.

Erdsea achieves this by treating the metadata standard as a first class citizen, the ESDTs standard being used as fallback. In order for the platform to be able to display properties that follow the structure mentioned in the ERC-721 section, it stores two URLs in the token, the first being the image itself and the second the metadata url.

A token following the ERD-721 structure can be minted standalone or using the minter template contract that will be offered through the platform. This contract will be deployed by a proxy contract and ownership transfered to the creator.

The minter template contract includes features such as:

* registering the base and metadata URIs once, without requiring to send them everytime a user mints tokens.
* shuffling directly on-chain in order to ensure randomness and a fair distribution by avoiding users knowing beforehand what the IDs of the rarest pieces are.
* giveaway functionality, so creators can reward the community in an easy and transparent way.

The minter contract will be explained in detail alongside a How-to tutorial in the next section.
