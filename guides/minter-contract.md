# Minter Contract

The minter smart contract is aimed at creators to help them mint and sell large collections with little to no effort. It provides an instant and secure way of minting tokens from a collection.&#x20;

This section presents the main endpoints that the contract exposes and explains how these work.

{% hint style="info" %}
Good to know: The source code for this contract will not be made public in the days following the mainnet launch to prevent malicious usage. Its code will be governed by the deployer contract.
{% endhint %}

### Public endpoints

### _**giveaway**_

The giveway endpoint is used to distribute tokens that were previously sold/reserved outside of the contract. Its only limitation is the gas limit, so those wishing to use this feature should first test their specific use-case on a testnet.

```
    #[only_owner]
    #[endpoint(giveaway)]
    fn giveaway(
        &self,
        #[var_args] addr_amount_args: MultiArgVec<MultiArg2<Address, u16>>,
    ) -> SCResult<u16>
```

### _mintTokens_

The mintTokens endpoint is used to mint and send tokens. It receives a payment in EGLD and the amount of desired NFTs to mint and send. The amount is optional and defaults to 1 (one). If the endpoint is called with a payment greater than the cost configured, the surplus is refunded.

```
    #[payable("EGLD")]
    #[endpoint(mintTokens)]
    fn mint_tokens(
        &self,
        #[payment_amount] payment: Self::BigUint,
        #[var_args] number_of_tokens_desired_opt: OptionalArg<u16>,
    ) -> SCResult<()>
```

### _shuffle_

Shuffle is used to ensure a fair distribution, by shuffling the token positions in the contract. The way tokens are built is the following:&#x20;

* the token with nonce = 1 will receive Index = 1. This means that the image will be: image\_base + 1 + image\_extension.&#x20;

This has the advantage of being easy to setup, but has the disadvantage of being predictable. Users can see the tokens images before minting, and this could ruin all the fun. The owner can chose to randomly shuffle the _nonce_ by calling this endpoint. After doing several calls (up to the owner), the nonce will be assigned another index. Each nonce would have an equally random corresponding index between 1 and MaxSupply. This step is optional.

```
    #[only_owner]
    #[endpoint]
    fn shuffle(&self) -> SCResult<u64>
```

### _requestWithdraw_

Because the creator of each token would be this contract, it will receive the royalties. The royalties would be locked in the marketplace for a period of 30 days. This is for the safety of the users, to try **preventing rug pulls as much as possible**. This discourages the scammers. If the community signals a rug pull, the owner address will be blacklisted and the scammer will not receive its royalties. This endpoint requests the withdrawal of royalties from the marketplace smart contract. Be mindful that this call cannot be done at any time, only once a month.

```
    #[only_owner]
    #[endpoint(requestWithdraw)]
    fn request_withdraw(&self, marketplace: Address) -> AsyncCall<Self::SendApi>
```

### _withdraw_

The owner of the contract can withdraw at any time the EGLD stored in this contract.

```
    #[only_owner]
    #[endpoint(withdraw)]
    fn withdraw(&self)
```

The rest of public endpoints and views are self explanatory

```
#[only_owner]
#[endpoint(setPrice)]
fn set_price(&self, price: Self::BigUint) {
    self.price().set(&price);
}

#[only_owner]
#[endpoint(pauseSale)]
fn pause_sale(&self) {
    self.sale_paused().set(&true);
}

#[only_owner]
#[endpoint(resumeSale)]
fn resume_sale(&self) {
    self.sale_paused().set(&false);
}

#[view(getLeftForSale)]
fn get_left_for_sale(&self) -> u16 {
    self.max_supply().get() - self.total_sold().get()
}

#[view(getMaxSupplyAndTotalSold)]
fn get_max_supply_and_total_sold(&self) -> MultiResult2<u16, u16> {
    MultiResult2::from((self.max_supply().get(), self.total_sold().get()))
}

#[view(getTotalSold)]
#[storage_mapper("total_sold")]
fn total_sold(&self) -> SingleValueMapper<Self::Storage, u16>;

#[view(getMaxSupply)]
#[storage_mapper("max_supply")]
fn max_supply(&self) -> SingleValueMapper<Self::Storage, u16>;

#[view(getPrice)]
#[storage_mapper("price")]
fn price(&self) -> SingleValueMapper<Self::Storage, Self::BigUint>;

#[view(getRoyalties)]
#[storage_mapper("royalties")]
fn royalties(&self) -> SingleValueMapper<Self::Storage, Self::BigUint>;

#[view(getImageBaseUri)]
#[storage_mapper("image_base_uri")]
fn image_base_uri(&self) -> SingleValueMapper<Self::Storage, BoxedBytes>;

#[view(getImageExtension)]
#[storage_mapper("image_extension")]
fn image_extension(&self) -> SingleValueMapper<Self::Storage, BoxedBytes>;

#[view(getMetadataBaseUri)]
#[storage_mapper("metadata_base_uri")]
fn metadata_base_uri(&self) -> SingleValueMapper<Self::Storage, BoxedBytes>;

#[view(getTokenNameBase)]
#[storage_mapper("token_name_base")]
fn token_name_base(&self) -> SingleValueMapper<Self::Storage, BoxedBytes>;

#[view(getTokenId)]
#[storage_mapper("token_id")]
fn token_id(&self) -> SingleValueMapper<Self::Storage, TokenIdentifier>;

#[view(getSaleStart)]
#[storage_mapper("sale_start")]
fn sale_start(&self) -> SingleValueMapper<Self::Storage, u64>;

#[view(isSalePaused)]
#[storage_mapper("sale_paused")]
fn sale_paused(&self) -> SingleValueMapper<Self::Storage, bool>;

#[storage_mapper("nonce_to_index")]
fn nonce_to_index(&self, nonce_as_u16: u16) -> SingleValueMapper<Self::Storage, u16>;
```
