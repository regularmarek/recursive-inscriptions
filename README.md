# What are inscriptions?

The idea of putting noteworthy data on the Bitcoin blockchain has been with us since the genesis block:
``The Times 03/Jan/2009 Chancellor on brink of second bailout for banks.``

Ordinals is a system of individually identifying and numbering satoshis, commonly referred to as “sats.” Satoshis are the smallest unit of accounting in Bitcoin (100 million sats make up one Bitcoin). The official Ordinals wallet software, known as “ORD,” is a protocol client and wallet that incorporates a unique feature. It assigns an ordinal number to each Satoshi based on the specific details of when, where, and how it was mined.

Inscriptions are a way of storing arbitrary data in the Bitcoin blockchain and attaching these to "sats".

The inscriptions protocol is entirely client-side validated, with the Bitcoin blockchain serving as a method of both storage, and canonical ordering. The ORD wallet is the client for working with inscriptions directly, as well as the tool to perform inscriptions. It involves a two-step process involving a commit transaction and a reveal transaction which are utilized to write data or metadata to the witness data section of a taproot transaction. This process serves the purpose of creating NFTs, and in certain cases, fungible tokens.

The Ordinals system of rarity is arbitrary, meaning assets such NFTs, security tokens, accounts or stablecoins can be attached to Ordinals. Alternative approaches to assess "rarity" have been suggested, including the recognition of “early” satoshis mined in blocks within the first year of Bitcoin’s launch or prioritizing Satoshis that possess distinctive mathematical properties, such as being prime numbers or exhibiting other unique characteristics. Recently, a market has emerged for “uncommon” satoshis, specifically the first satoshis in ordinary blocks, with a current selling price of 0.03 BTC or $750. Learn more:
https://docs.ordinals.com/overview.html

Like it or not, inscriptions are becoming part of a prototypical "circular economy" of Bitcoin - entirely contained within the Bitcoin ecosystem (value creation, value transfer, etc.). Use of the ordinals protocol requires people to run their own _economic nodes_ which is probably a good thing. Additionally, the Ordinals phenomenon is contributing to making people more aware of Bitcoin fees (sats/byte) as well as understanding the UTXO model.

## BRC20

On March 8th, the BRC-20 standard was introduced, taking its name after the famous ERC-20 fungible token standard. However it is important to note that BRC-20 tokens are fundamentally different from ERC-20 tokens in several ways. BRC-20 tokens are essentially ordinal inscriptions with a specific type of text embedded into them, providing a set of rules and specifications for creating and managing fungible (semi-fungible, technically) tokens. BRC-20 tokens do not make use of smart contracts like popular token standards on EVM blockchains. Instead, they enable users to store Ordinal inscriptions of JSON (JavaScript Object Notation) transactions to deploy token contracts, mint, and transfer tokens. The blockchain serves to order and timestamp transactions. The transactions themselves are interpreted by the client software which rejects any invalid transactions according to the rules of the protocol (such as minting more tokens than the maximum supply).

## Some useful resources

"Official" Inscriptions Explorer w/ links to wallet and other tools

https://ordinals.com/

Another explorer and marketplace:

https://ord.io

A web-based tool for finding rare sats:
https://sating.io
Instructions for finding rare sats yourself:
https://docs.ordinals.com/guides/sat-hunting.html

There are lots of custodial and semi-custodial wallets, marketplaces, and services popping up, with Binance also recently entering the fray.
e.g.
https://unisat.io/market

## Noteworthy Ordinals Collections

### Self-referential Bitcoin stuff

People are "calling" particular bitcoin blocks for some reason:
https://ordinalswallet.com/collection/bitmap

### Games
https://www.ord.io/1015

https://www.ord.io/1049

https://www.ord.io/1110043

### Historical Documents
Cypherpunk Manifesto:

https://www.ord.io/1094462

King James Bible (thanks for the find, @SophiaSpirlock)
https://ordinals.com/inscription/bd71fe5056db486acffaac31febcf70fc71326e844480f342b2e48aef7f57b9ei0

## Some notes

Although inscription users are playing by the rules of the Bitcoin protocol, paying fees to miners for blockspace they consider valuable; it has been noted that inscriptions contribute to UTXO bloat, although that's going to occur with (and possibly difficult to distinguish from) mass adoption regardless.

Arguably, BRC-20 is responsible for most of the UTXO bloat being observed.



# What are recursive inscriptions?

Although probably not the best name (what is well named in this space, anyway?) recursive inscriptions are a way to look-up and re-use inscriptions to make efficient and cool new  inscribed artifacts possible _in a highly composible way_.

Example of a recursive inscription using a 3d rendering engine:
https://ord.osura.com/inscription/1b8b99c061dfeb875524228523906be678c87a4427308732e9d7ce6e6bc1487fi0

Look-ups are a fundamental “divide and conquer” approach to solving programming challenges. This approach was pioneered in NFT collections by projects such as @OnChainMonkey among others.
The first thing to recall is that the ordinals/inscription protocol themselves are interpreted and rendered on the client side (in this case the browser).
This means that inscriptions can run any code that can run in a browser, which these days can mean a lot of things. For example folks <example> have inscribed 3d rendering engines that can be used to render other inscriptions. 

Let’s work out an example of a hypothetical wizard pfp 10k collection each having some combination of traits.
```
100kb x 10000 = 1000000kb ~= 1GB
```
Let’s also say for example, that there are ten base wizards; 

```average, fat, skinny, zombie, alien; each having a male and female variant for a total of 10```

Let’s also say there are eleven traits for example: 

```hat, bald, glasses, earring, tattoo, beard, cigarette, mohawk, eyepatch, clown nose, freckles```

Just to make things fun, assume the head tops: Hat, bald, Mohawk, are mutually exclusive traits.

Can have a variable number of traits, either three, four, five, or six traits.

The number of unique combinations for this setup are: 

```10 bases * 3 head-tops * ( (9 choose 6) + (9 choose 5) + (9 choose 4) + (9 choose 3) ) = 12600 ```
possible variations.
(We’ll only need to use 10000 of these)

Again the base wizards are each 100kb requiring 10*100kb ~= 1MB to inscribe.

Then each trait needs to be inscribed once and let’s say the images for the traits are smaller, 10kb each for a total of 110k.  So far we have used ~ 1.1 MB

As we can see from <example> it’s possible to then create each of the 10000 unique variants that are a combination of traits using a text/html/css inscription that references the previously inscribed base wizards and a combination of traits. Each of the 10k collection needs less 2kb each, because the features inscribed earlier are reused. So generating the 10k final collection requires 2k * 10000 = 20000kb ~= 20mb

In total this collection requires 21.1MB only
```
21100 kb / 1000000 = 0.0211
```
Requiring only 2.1% of the space of the original hypothetical “naïve” collection for a space saving of about 98%. Cool, right?!
Currently there’s no permissions layer to control which inscriptions can be reused in a  particular recursive context.
Please note I don’t own any idea the inscriptions used in these examples, but they are used with permission of the original inscribers.

# how to create your own recursive inscriptions

Bitcoin Ordinals Inscriptions are both client-side-validated (protocol) as well as client-side-rendered, meaning that the content can be anything that's recognized by a modern browser; HTML5 being the _lingua franca_ of the web.

In this OnChainMonkey example we see that it's generated declaratively via HTML5 rather than being an inscribed image itself. The browser renders it locally:
https://ordinals.com/inscription/fb162a46943e5d7d31d72ee2c8c850e66c1ca5d0d453068aa63883528285ed21i0

The WildTangz project has created a nifty sandbox where you can experiment without "polluting" the blockchain with your prototypes:

https://recursive.wildtangz.com/

A simple example of recursion:
This inscription:
https://ordinals.com/inscription/f425d535b86edd5421a63bf95cefac155e67b3ebeba2755b0972bea8a57b248ei0

Reuses this:
https://ordinals.com/inscription/ed17fc66b14b654426287043bf2d8a22eb3064e4725d761d45f8d0a2d60ab30di0
and this:
https://ordinals.com/inscription/5dd5438b2e2663a02fab2ec7d3cf58d5c3f25f7c22da25b39c80d69ee66e6315i0

Although this example is simple, we can reuse previously inscribed artifacts in an HTML5 context in potentially unlimited complexity (metaverses?).

## setup

Install and sync Bitcoin Core

https://bitcoin.org/en/download

The ``ord`` wallet requires Bitcoin Core for indexing transactions so make sure that:
```txindex=1```
is found in your 

Install Ordinals Wallet

https://github.com/ordinals/ord

DO NOT use Bitcoin Core for anything Ordinal/Inscription related besides signing transactions. Bitcoin Core is not aware (probably rightly so) of Ordinals/Inscription protocol and it's easy to burn or otherwise make mistakes from there.

Once Bitcoin Core is synced with the chain tip, you can start using ord wallet for inscription related activity. 

Start by creating a new wallet for inscriptions - do not mix wallets that are used for ordinals and ordinary "cardinal" Bitcoin. In the terminal:

```ord wallet create```

Fund the wallet you created, by getting it's address and sending it some sats ($50 bucks worth should be enough to experiment)

```ord wallet receive```

once the transaction is confirmed you should be able to see some sats here:

```ord wallet outputs```

Assuming you have the file you want to inscribe in the same directory as the ``ord`` wallet inscribe using:

```ord wallet inscribe --fee-rate <FEE_RATE> <INSCRIPTION_FILE>```

With ``FEE_RATE`` being the sats-per-byte you're willing to pay miners, and ``INSCRIPTION_FILE`` being the path to the content you wish to inscribe.

You should then be presented with information in the terminal including the txid of the commit transaction, the txid for the the reveal transaction, and the inscription ID. 

Check the progress using your favorite block explorer, and after it's mined you should be able to find it by searching for your inscription ID on https://ordinals.com/



Further reading:
https://docs.ordinals.com/guides/inscriptions.html
