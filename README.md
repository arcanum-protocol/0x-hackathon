# 0x, AWS, Family, Polygon | Decreasing Frictions in DeFi
Hackathon [link](https://dorahacks.io/hackathon/decreasing-frictions-in-defi/detail)

## What is Arcanum?

Arcanum is a highly scalable fully decentralized ETF protocol.

Arcanum protocol acts as a platform for indexed exchange-traded funds in a form of multipools enforced by unique balancing mechanisms approved by the governance. Multipools are the foundational concept of Arcanum, enabling access to a multi asset liquidity with just a single token without dilution or imbalance and augmenting capital efficiency.

Arcanum enables generation of decentralised ETFs tracking the prices of crypto assets within multipools. 

The ETF balance stabilization is enforced by the inbuilt algorithms determining the shares of assets in the equilibrium state of the pool in response to the changing market conditions, while dynamic pricing is supported by oracles and an aggregate of prices from leading volume exchanges. 

$AAA, the governance token for Arcanum, is an ERC-20 token used for voting for the new ETFs issued by the protocol and their composition as well as can be farmed for providing liquidity for $AAA token and Arcanum ETFs. More on that in our future articles.

## The concept behind Arcanum

The number of ETFs in the TradiFi world is growing every year; now there are more than 7 thousand of them, and the volume of their total assets exceeds 9 trillion US dollars.
However, by now the ETF as a financial tool hasn't found its place in the DeFi arena and crypto in general. We see only a few products released by centralized institutional players which are greatly limited to be accessed by the retail users. 

Surprisingly few attempts to create truly decentralized ETFs can be explained by the complexity of sustaining the ETFs price on the volatile crypto market. We do not claim absolute correctness of our approach but DeFi’s capabilities enables us to create a decentralized, permissionless indexes protocol with farming and trading opportunities where every ETF is managed by the DAO. 

The ultimate goal of Arcanum is to provide greater asset management opportunities for users and expand the frontiers of DeFi through employing the best TradiFi concepts for the benefit of decentralized web. 

Arcanum is highly scalable and establishes an explicit framework through its protocol to set up new pools and thus issue ETFs which by itself make the space diverse and more accessible.

## The mechanics

The DeFi nature of Arcanum enables us to combine two primitives in one - the ETF as an ultimate asset management tool, where a user can provide liquidity in the asset of their convenience which is diversified into a more profitable portfolio that is managed by the public and governance-managed rules, and a multipool with the decentralized swapping mechanics where users can interact with protocol directly and bring it to balance by swapping, minting/burning and leverage reward for their stabilizing actions.

By merging these two primitives, we get an on-chain self-balanced system combining a DEX and an asset management platform in one.

The core products of Arcanum are DeFi ETFs - fully decentralized exchange-traded funds that are implemented in a form of multipools enabling swapping of assets within the pools. 

The first ETF by Arcanum is $ARBI - Arbitrum ecosystem ETF tracking the prices of the top 10 Arbitrum-native DeFi tokens ranked by their revenue. The MVP on testnet is implemented on Polygon though with $CPT tracking top 5 DeFi tokens on Polygon.

The ETF's main goal is to keep assets shares in the multipool close to the equilibrium and encourage traders to perform actions aimed at eliminating deviations. Unlike traditional DEXs implementing xy=k formula, where traders mainly leverage speculations coming from the price volatility and sandwich attacks, in Arcanum you can direct your trades to balance the multipool assets and earn a portion of fees for your actions.

ETFs on Arcanum can be minted using any index asset within a corresponding multi pool and burnt to redeem any index asset. The price for minting and redemption is calculated based on (total worth of assets in index) / ETF supply.

With an ETF token, users basically get a portion of all underlying assets (index tokens) in the pool. If index tokens prices are increasing, then the price of an ETF will increase as well, if declining - ETF’s price declines too, however, in both cases, the manageable composition of the ETF’s underlying portfolio can eliminate the risks of distinctive assets’ extreme volatility. 

The ETF contract itself represents a portfolio of assets with a specific set of rules which accepts your deposit, then routes it through algorithms which calculate users’ fees based on current ETF balance. 

# Read more about Arcanum

* [Whitepaper](https://arcanum.to/whitepaper.pdf)
* [Presentation]()
* [Full hello world article]()
* [Website](https://arcanum.to)
* [App](https://app.arcanum.to)

# 0x api integration 

There are two cases of using 0x api. First one is to utilise 0x-api to search for maximum efficient balancing orders (Might be interesting for arbitragers). Second one is our light [implementation](https://github.com/arcanum-protocol/arcanum-backend-services/tree/master/0x-api) code for /swap/v1/quote method. 

## Input

| Params | Description | Example |
| --- | --- | --- |
| sellToken | The ERC20 token address or symbol of the token you want to send. Native token such as "ETH" can be provided as a valid sellToken. If the symbol given is not supported, try using token address instead. | sellToken=0x6b175474e89094c44da98b954eedeac495271d0f |
| buyToken | The ERC20 token address or symbol of the token you want to receive. Native token such as "ETH" can be provided as a valid buyToken.If the symbol given is not supported, try using token address instead. | buyToken=0x6b175474e89094c44da98b954eedeac495271d0f |
| sellAmount | (Optional) The amount of sellToken (in sellToken base units) you want to send. | sellAmount=100000000000 |
| buyAmount | (Optional) The amount of buyToken(in buyToken base units) you want to receive. | buyAmount=100000000000 |
| slippagePercentage | (Optional, default is 0.01 for 1%) The maximum acceptable slippage of the buyToken amount if sellAmount is provided; The maximum acceptable slippage of the sellAmount amount if buyAmount is provided (e.g. 0.03 for 3% slippage allowed). The lowest possible value that can be set for this parameter is 0; in other words, no amount of slippage would be allowed. If no value for this optional parameter is provided in the API request, the default slippage percentage is 1%. | slippagePercentage=0.03 |

## Output

| Field | Description |
| --- | --- |
| to | The address of the contract to send call data to. |
| data | The call data required to be sent to the to contract address. |
| buyAmount | The amount of buyToken (in buyToken units) that would be bought in this swap. Certain on-chain sources do not allow specifying buyAmount, when using buyAmount these sources are excluded. |
| sellAmount | The amount of sellToken (in sellToken units) that would be sold in this swap. Specifying sellAmount is the recommended way to interact with 0xAPI as it covers all on-chain sources. |
| buyTokenAddress | The ERC20 token address of the token you want to receive in quote. |
| sellTokenAddress | The ERC20 token address of the token you want to sell with quote. |
| allowanceAddress | The target contract address for which the user needs to have an allowance in order to be able to complete the swap. For swaps with "ETH" as sellToken, wrapping "ETH" to "WETH" or unwrapping "WETH" to "ETH" no allowance is needed, a null address of 0x0000000000000000000000000000000000000000 is then returned instead. |

# Connect Kit

Connect kit is used as a web3 wallet sdk on our [app frontend](https://api.arcanum.to)

# Router API

<details>

<summary> Call methods (requires asset approval to router) </summary>

```solidity
function mintWithSharesOut(
   address _pool, // pool address
   address _asset, // asset address
   UD60x18 _sharesOut, // shares that you want to get
   uint _amountInMax, // maximum amount in
   address _to, // token receiver
   uint deadline // transaction deadline
) external;
```

```solidity
function burnWithSharesIn(
   address _pool, // pool address
   address _asset, // asset address
   UD60x18 _sharesIn, // shares that you want to sell
   UD60x18 _amountOutMin, // minium amount to receive
   address _to, // token receiver
   uint deadline // transaction deadline
) external;
```

```solidity
function swap(
   address _pool, // pool address
   address _assetIn, // asset in address
   address _assetOut, // asset out address
   UD60x18 _amountInMax,
   UD60x18 _amountOutMin,
   UD60x18 _shares, // shares that are minted and burned within operation 
   address _to, // token receiver
   uint deadline // transaction deadline
) external;
```

```solidity
    function mintWithAmountIn(
       address _pool, // pool address
       address _asset, // asset address
       UD60x18 _amountIn,
       UD60x18 _sharesOutMin,
       address _to, // token receiver
       uint deadline // transaction deadline
    ) external;
```

```solidity
    function burnWithAmountOut(
       address _pool, // pool address
       address _asset, // asset address
       UD60x18 _amountOut,
       UD60x18 _sharesInMax,
       address _to, // token receiver
       uint deadline // transaction deadline
    ) external;

```

```solidity
    function swapWithAmountIn(
       address _pool, // pool address
       address _assetIn, // asset in address
       address _assetOut, // asset out address
       UD60x18 _amountIn,
       UD60x18 _amountOutMin,
       address _to, // token receiver
       uint deadline // transaction deadline
    ) external;

```

```solidity
    function swapWithAmountOut(
       address _pool, // pool address
       address _assetIn, // asset in address
       address _assetOut, // asset out address
       UD60x18 _amountOut,
       UD60x18 _amountInMax,
       address _to, // token receiver
       uint deadline // transaction deadline
    ) external;
```

</details>

<details>

<summary> View methods </summary>

```solidity
    function estimateMintSharesOut(
       address _pool, // pool address
       address _asset, // asset address
       UD60x18 _amountIn
    ) public view returns(UD60x18 sharesOut);
```

```solidity
    function estimateMintAmountIn(
       address _pool, // pool address
       address _asset, // asset address
       UD60x18 _sharesOut
    ) public view returns(UD60x18 _amountIn);
```

```solidity
    function estimateBurnAmountOut(
       address _pool, // pool address
       address _asset, // asset address
       UD60x18 _sharesIn
    ) public view returns(UD60x18 _amountOut);

```

```solidity
    function estimateBurnSharesIn(
       address _pool, // pool address
       address _asset, // asset address
       UD60x18 _amountOut
    ) public view returns(UD60x18 _sharesIn);

```

```solidity
    function estimateSwapSharesByAmountIn(
        address _pool, // pool address
        address _assetIn, // asset in address
        address _assetOut, // asset out address
        UD60x18 _amountIn
    ) public view returns(UD60x18 shares, UD60x18 amountOut);
```

```solidity
    function estimateSwapSharesByAmountOut(
        address _pool, // pool address
        address _assetIn, // asset in address
        address _assetOut, // asset out address
        UD60x18 _amountOut
    ) public view returns(UD60x18 shares, UD60x18 amountIn);
```

</details>

# Deployment addresses
| Contract | Address |
| --- | --- |
| Multipool core | [0x33657896740F7BA132553EeE2efF38C8748F035C](https://mumbai.polygonscan.com/address/0x33657896740F7BA132553EeE2efF38C8748F035C) |
| Multipool router | [0x6c0528008A74AcCfF3A203670E94ddD822D8Cb44](https://mumbai.polygonscan.com/address/0x6c0528008A74AcCfF3A203670E94ddD822D8Cb44) |
| TOKEN 1 |[0x54ce1aE0DcC17d8316F6526A9D6fD6949d1B134f](https://mumbai.polygonscan.com/address/0x54ce1aE0DcC17d8316F6526A9D6fD6949d1B134f) |
| TOKEN 2 |[0xc071C52749f0238C0FbB6668eBA8D019b822A6C0](https://mumbai.polygonscan.com/address/0xc071C52749f0238C0FbB6668eBA8D019b822A6C0) |
| TOKEN 3 |[0x4Fe2A907e61d1F0ff7AafDfc754D9EeF1b6D7870](https://mumbai.polygonscan.com/address/0x4Fe2A907e61d1F0ff7AafDfc754D9EeF1b6D7870) |
| TOKEN 4 |[0x7A27FDAB8A0bb5728e9229d69180e671db766Ebd](https://mumbai.polygonscan.com/address/0x7A27FDAB8A0bb5728e9229d69180e671db766Ebd) |
| TOKEN 5 |[0x167b876620dD8531c7A47dA89Ea32A1A37680407](https://mumbai.polygonscan.com/address/0x167b876620dD8531c7A47dA89Ea32A1A37680407) |



