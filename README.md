# Indexer CSV Export

This project uses the FASTNEAR resources to generate a CSV file for a given account id.

See this issue from the Infrastructure Working Group:

https://github.com/near/Infrastructure-Working-Group/issues/143

Here are a few rows from a CSV generated from a former indexer.

```
date,account_id,method_name,block_timestamp,from_account,block_height,args,transaction_hash,amount_transferred,currency_transferred,ft_amount_out,ft_currency_out,ft_amount_in,ft_currency_in,to_account,amount_staked,onchain_balance,onchain_balance_token,metadata
"October 25, 2023",pierre-dev.near,TRANSFER,1698243233045024706,pierre-dev.near,104180870,{},GMPLSE6AX3FyB6NJtdUqaTebcb5VQiHxSzpPkCn6StgD,1.00000,NEAR,,,,,01d4456fa249ed1a8c2fd3013153b37504f4ee95.lockup.near,0.00000,13.11316,NEAR,
"October 25, 2023",pierre-dev.near,TRANSFER,1698243233045024706,pierre-dev.near,104180870,{},GMPLSE6AX3FyB6NJtdUqaTebcb5VQiHxSzpPkCn6StgD,-1.00000,NEAR,,,,,01d4456fa249ed1a8c2fd3013153b37504f4ee95.lockup.near,0.00000,13.11316,NEAR,
"October 25, 2023",pierre-dev.near,TRANSFER,1698245077859280904,pierre-dev.near,104182363,{},5EwME4z5MGnFKKSc1EVTQ14gBpnaBijuWXzpyP4eu1Pf,-1.00000,NEAR,,,,,pierre.sputnik-dao.near,0.00000,12.11312,NEAR,
"October 26, 2023",pierre-dev.near,TRANSFER,1698332768932437430,pierre-dev.near,104252769,{},F1AexGUeK9q6NcjNaJExBajTtdtLzCiim4eo2ZAEyUf5,-0.10000,NEAR,,,,,auto-near.sputnik-dao.near,0.00000,12.01307,NEAR,
"October 26, 2023",pierre-dev.near,TRANSFER,1698332852127219605,pierre-dev.near,104252832,{},5BS88zBq3itTUfYgaybqUQ2vUqVHJxF8XgduwtCte9B1,-1.00000,NEAR,,,,,auto-near.sputnik-dao.near,0.00000,11.01303,NEAR,
"October 26, 2023",pierre-dev.near,TRANSFER,1698333322056049407,pierre-dev.near,104253197,{},4JBreiWrxNVGxeHnR7m5bwe1qwcSLCmcb2dKNqJpyaj7,-0.10000,NEAR,,,,,12717af3df195702bab3e791cb196a7505f5d1b7bb78299c5dadf9247913a377,0.00000,10.91218,NEAR,
"October 26, 2023",pierre-dev.near,TRANSFER,1698333581945847589,pierre-dev.near,104253405,{},4bwiQCYYRb2D8w1RwFzwjZ2qnSF3AdiojdeZhv9h4XVa,-0.10000,NEAR,,,,,12717af3df195702bab3e791cb196a7505f5d1b7bb78299c5dadf9247913a377,0.00000,10.81135,NEAR,
"October 26, 2023",pierre-dev.near,TRANSFER,1698347173064387765,auto-near.sputnik-dao.near,104264541,{},C6aA8ZiqhjkDpqSTDZVmS7UXHoDYsogWb5yNoKmUfqP2,8.00000,NEAR,,,,,pierre-dev.near,0.00000,18.81136,NEAR,
"October 26, 2023",pierre-dev.near,TRANSFER,1698347472492454241,pierre-dev.near,104264773,{},Ft7XjeFpJCUdhwUVGcqU6EmVZgFrgX5CgVowrJitvxog,-8.00000,NEAR,,,,,auto-near.sputnik-dao.near,0.00000,10.81131,NEAR,
```

## Potential Improvements

- It's unclear what this CSV is most useful for, since it doesn't seem to capture gains/losses. Add clarity and leave room for adjustments.
- Rows show native NEAR transfers, storage deposits, fungible token transfers, NFT purchases, NFT minting, and "donation" through the quadratic funding project Potluck. Some of these seem incongruent in the sense that they likely don't belong in the same category. For instance, a storage deposit is a very protocol-specific event and would likely not be classified with native/fungible transfers, and is almost certainly not taxable in the same way that gas costs would not be considered a purchase or value exchange.
- Given that NEAR fungible tokens may not have a standard price discovery mechanism, and that digital tokens can be used in a variety of ways (pure utility token, for instance) it may be advisable to separate the native and non-native transfers instead of lumping them together. This might also help separate logic in a meaningful way, since native token transfers use the [Transfer Action](https://github.com/near/nearcore/blob/2d5dd968832d4df7ff9f78a7eae7c834f93c8371/core/primitives/src/action/mod.rs#L213) and fungible/non-fungible tokens are smart contracts, and hence use [FunctionCall Actions](https://github.com/near/nearcore/blob/2d5dd968832d4df7ff9f78a7eae7c834f93c8371/core/primitives/src/action/mod.rs#L212)
