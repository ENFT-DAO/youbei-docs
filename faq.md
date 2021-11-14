# FAQ

### What is ERD-721 standard ?

An ERD-721 token is an ESDT-NFT token with the following properties:

* First URI of the token represents the asset's image
* The second URI is the metadata link that represents the asset's attributes

### Does Erdsea accept just ERD-721 tokens ?

Erdsea accepts both ESDT-NFT and ERD-721 tokens. The only requirements are:

* The NFT needs to have at least one URI
* The maximum royalties allowed are 10%

### Why should I mint my collection on Erdsea ?

ERD-721 are compatible with Elrond standard for NFTS, as they are ESDT-NFT tokens. Besides that, this model of having the attributes in external decentralized storage allows for ease of collection creation.

### How does Erdsea help me with minting my collection ?

We offer full contract deployment for creators through our platform. By using the contract users can mint and buy tokens directly on chain, while being easy for the creator.

### Is Erdsea compatible with other marketplaces ?

ERD-721 tokens are ESFT-NFT tokens and should be compatible with other marketplaces that accept ESDT-NFT tokens.

### What are the royalties on Erdsea ?

Platform royalties are 2.5%. Accepted token royalties are maximum 10%.

### I listed my NFT minted using Elrond Wallet. Why don't the tags appear ?

Erdsea does not interpret tags since their format is not standardized and there is no general solution to interpret them. We encourage having the attributes in the metadata link and stored on decentralized storage networks. We might also interpret certain token attributes which follow a standardized format.

### How to add attributes to my already minted Collection/Tokens ?

Attributes are accessible through the metadata link which is stored in the token data. The token data is not mutable, so once minted, one cannot add a new link.

### How to add metadata to my Collection/Tokens before minting ?

Attributes are accessible through the metadata link. So at the mint time, the creator must store the metadata link as the second URI in the token data. By doing so, the attributes will be successfuly displayed on the platform. Check the upload metadata guide for more info.

### I cannot list my NFT. Why ?&#x20;

There are two requirements:

* Royalties have to be maximum 10%
* The token must have at least one URI

### How to sell my NFT ?

There are two ways:

* List it and specify a fixes price
* Start an auction and specify the auction start date, dealine and minimum bid

### How to buy a NFT ?

Depending on whether the token is listed with a fixed price or it was put up for auction, you can:

* Directly buy the token by paying the fixed price
* Place a bid if an auction is ongoing

### Listed my NFT with a fixed price, how do I change the price ?

At the moment, the way to do this is by withdrawing the NFT and placing it on sale with a new price.

### I placed my NFT on sale and I don't see it listed or in my wallet. Where is it ?

Block finality takes around 30 seconds on Elrond. Your NFT is in the marketplace smart contract, but the result is not displayed because Erdsea waits for block finality before updating its structures. After the 30 seconds finality you will see your token listed on the platform.

### I tried placing a bid and it failed. Why ?

There can be some cases like:

* Check the minimum bid, it might be that you placed a lower bid than accepted
* Check the highest bid, maybe you did not place a higher bid than that
* Check the start time of the auction, the auction might not have started yet
* Check the deadline of the auction, the auction might have already ended

### I placed a bid. How can I cancel it ?

You cannot cancel your bid. There are two scenarios for getting back the funds:

* Seller withdrawing the NFT before the auction deadline
* Another user placing a higher bid than yours

### I started an auction.  How can I accept a bid ?

You cannot accept bids. If your auction is active you can only withdraw the token or wait for either of the following events:

* End the auction if there is a winner (highest bidder)
* Withdraw the NFT if there is no winner (highest bidder)
* Accept an offer if there is no winner (highest bidder)

### I started an auction. How can I accept an offer ?

Offers for a token that is up for auction can only be accepted in two cases:

* The auction did not start
* The auction ended with no winner (no bids)

### What is the difference between an offer and a bid ?

By making an offer, one's deposit does not decrease, unless it's accepted. This is in contrast to a bid, where the deposit decreases immediately after placing the bid. Another difference is that the creator can choose which offer to accept, whereas for a bid, the winner is considered to be the highest bidder.

### I want to make an offer and transaction fails, but I have EGLD in my wallet. Why ?

Making an offer requires the offeror to have the EGLD stored in a deposit inside the markeplace smart contract. The reasoning is that if the offer is accepted, the tokens should be sent to the of the token directly.

### My deposit amount increased without any action. Why ?

An offer that you have placed was accepted.

### What is the difference between Collection ID and Token ID ?

They are the same thing.

### If Token ID and Collection ID are the same, what is Nonce ?

Nonce brings the uniqueness to the NFT. It is the equivalent of Index on Ethereum's ERC-721, with the difference that it starts from 1, not from 0.
