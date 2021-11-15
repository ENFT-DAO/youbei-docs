# Deployer contract

The deployer contract is used to deploy minter contracts used for collections minting directly on chain. It makes the setup much easier for a non-technical person, as all the steps needed can be performed from the Erdsea platform, by filling the parameters explained in this guide.

There are 4 steps that have to be done in order to deploy the minting contract. If you don't want to read all the steps described below, although recommended, we prepared a video in which we follow the steps needed to create a collection :point\_down:

{% embed url="https://www.youtube.com/watch?v=0Bdzri3U878" %}

### 1. NFT issue

This step issues the NFT token placeholder (collection). The user action needed here is to copy the smart contract execution result and use it for the next step.

Required parameters:

* Token Name. Example: AliceTokens
* Token Ticker. Example: ALC

More about the parameters boundaries can be found on the Elrond official [docs](https://docs.elrond.com/developers/nft-tokens/#issuance-of-non-fungible-tokens) page.

After performing the issue NFT transaction the resulting token identifier will look like: ALC-6258d2. From the Elrond webwallet you will be able to see the transaction and see the hex encoded version of the token identifier: 414c432d363235386432.

To check, go to the transactions sections from the wallet, expand it, click the magnifier and from the explorer page take the token identifier as pictured below:

![](<../.gitbook/assets/WhatsApp Image 2021-11-12 at 00.39.23.jpeg>)

### 2. Deploy contract

This step will be performed to deploy the minter contract.&#x20;

Required parameters:

* TokenIdentifier. For convenience the creator only has to copy and paste the value described in step 1.
* Royalties. The creator royalties that will be configured in the contract. Accepted values are in the range of 0.00% to 10.00%.
* Token Base Name. This is the base value that will be set to each NFT. The full name will have the following format: "TokenBaseName #None". Example: Alice #10.
* Image Base Link. Example: [https://ipfs.io/ipfs/QmfNcU57Jg88Xr38P6x8jvL7g8nwiK21fft9izmmvjRUMW](https://ipfs.io/ipfs/QmfNcU57Jg88Xr38P6x8jvL7g8nwiK21fft9izmmvjRUMW)
* Image Extension. Example: **png**

Each NFT's first URL will have to represent the NFT image. The final URL will have the following format: [https://ipfs.io/ipfs/QmfNcU57Jg88Xr38P6x8jvL7g8nwiK21fft9izmmvjRUMW/7.png](https://ipfs.io/ipfs/QmfNcU57Jg88Xr38P6x8jvL7g8nwiK21fft9izmmvjRUMW/7.png). The index value is by default the NFT nonce value, although this is up to the creator. These indexes can be shuffled. See [NFT minter contract](minter-contract.md#shuffle) for more details on shuffling.

* Price. Example: 1 (all prices in EGLD).
* Max supply. Example: 10,000. The max supply in the collection representing the total numbers of items available for minting.
* Sale start time. The time after which the contract can start minting tokens.
* Metadata Base Link. (Optiona). Each NFT can have a second URL that will represent the metadata. The final metadata link will have the following format "MetadataBaseLink/Index". Example: [https://ipfs.io/ipfs/QmSnQ4CKfkmUx9bs8yTymmU1nDBkboCWCV2LN33UdDR2XT/7](https://ipfs.io/ipfs/QmSnQ4CKfkmUx9bs8yTymmU1nDBkboCWCV2LN33UdDR2XT/7). This final metadata URL has to provide metadata for a specfic NFT. The response from the metadata has to follow the standard described in [ERC-721 section](erd-721-standard.md#erc-721-structure). Example:

```
{
	description: "DAW is an NFT collection of 10,000 Desperate ApeWives. Each Desperate ApeWife is unique and algorithmically generated from 221 traits. All ApeWives are hot but some are supermodels. Your Desperate ApeWife is also your exclusive DAW membership card. Ownership and commercial usage rights are given to you, the owner, over your NFT.",
	image: "ipfs://QmfNcU57Jg88Xr38P6x8jvL7g8nwiK21fft9izmmvjRUMW/7.png",
	name: "Desperate ApeWife #7",
	attributes: [
		{
			trait_type: "BACKGROUND",
			value: "Pumpkin"
		},
		{
			trait_type: "FUR",
			value: "Snow White"
		},
		{
			trait_type: "EYES",
			value: "Angry"
		},
		{
			trait_type: "CLOTHING",
			value: "Rock T shirt"
		},
		{
			trait_type: "NECKLACE",
			value: "Tennis Necklace"
		},
		{
			trait_type: "HAIR",
			value: "Kate Hair"
		},
		{
			trait_type: "MOUTH",
			value: "Pout Gummy Snake"
		}
	]
}
```

At the moment, only the attributes exposed by the metadata link are interpreted.

### 3. Change ownership

This step is required so the ownership of the contract deployed at step 2. is transfered to you, giving you full control of the contract.&#x20;

Required parameters:

* The contract address from [step 2.)](deployer-contract.md#2.-deploy-contract) For convenience the creator should directly copy and paste the address value received after performing [step 2.)](deployer-contract.md#2.-deploy-contract) Go through the same process as for issuing the NFT and get the address value, as pictured below:

![](<../.gitbook/assets/WhatsApp Image 2021-11-12 at 00.39.44.jpeg>)

### 4. Set roles

This step is needed so the contract deployed at [step 2.)](deployer-contract.md#2.-deploy-contract) has the necessary roles (mint role) on the NFT deployed at [step 1.)](deployer-contract.md#1.-nft-issue).



Required parameters:

* The contract address, as described in [step 3.)](deployer-contract.md#2.-change-ownership). The value should be the same with the one provided at step 3.
