<div align="center">
  <img height="170x" src="https://cdn.discordapp.com/attachments/1421957855048634521/1424461131992268810/ChatGPT_Image_Oct_5_2025_07_06_32_PM.png?ex=68e4084f&is=68e2b6cf&hm=e3524ead38cd7681a424b76b8da89014e7acff90247d16cdc4e56952e78716b9&" />

  <h1>Forked Coin</h1>

  <p>
    <strong>Unified Cross-Chain Program Framework</strong>
  </p>

  <p>
    <a href="https://github.com/Forked Coin/Forked Coin/actions"><img alt="Build Status" src="https://github.com/Forked Coin/Forked Coin/actions/workflows/tests.yaml/badge.svg" /></a>
    <a href="https://Forked Coin-lang.com"><img alt="Tutorials" src="https://img.shields.io/badge/docs-tutorials-blueviolet" /></a>
    <a href="https://discord.gg/Forked Coin"><img alt="Discord Chat" src="https://img.shields.io/discord/889577356681945098?color=blueviolet" /></a>
    <a href="https://opensource.org/licenses/Apache-2.0"><img alt="License" src="https://img.shields.io/github/license/Forked Coin/Forked Coin?color=blueviolet" /></a>
  </p>
</div>

## The Vision

Forked Coin realizes Anatoly Yakovenko's vision of a truly unified blockchain ecosystem. By bridging Solana and BNB Chain, Forked Coin enables developers to create tokens and deploy programs across both networks using a single, unified API. Write once, deploy everywhere.

## What is Forked Coin?

Forked Coin is a groundbreaking framework that connects Solana and BNB Chain, providing developers with seamless tools for writing cross-chain programs and creating tokens on both networks simultaneously.

- **Unified API**: One codebase deploys to both Solana and BNB Chain
- **Cross-Chain Token Creation**: Create tokens on both networks with a single command
- **Rust eDSL**: Familiar Solana-style development experience
- **[IDL](https://en.wikipedia.org/wiki/Interface_description_language) specification**: Generate clients for both chains
- **TypeScript package**: Type-safe clients from IDL for multi-chain interaction
- **CLI and workspace management**: Complete cross-chain application development

Forked Coin is the first framework to truly unify Solana and BNB Chain development.

> [!NOTE]
> Forked Coin brings the best of both worlds: Solana's speed and BNB Chain's massive ecosystem. If you're familiar with Anchor, Truffle, or web3.js, you'll feel right at home with Forked Coin's unified approach to cross-chain development.

## Key Features

- **Single API, Dual Deployment**: Write your program once, deploy to both Solana and BNB Chain
- **Unified Token Standard**: Create tokens that work seamlessly across both networks
- **Cross-Chain State Management**: Synchronize state between Solana and BNB Chain
- **Developer Experience**: Familiar Anchor-like syntax with cross-chain superpowers

## Getting Started

For a quickstart guide and in-depth tutorials, see the [Forked Coin book](https://book.Forked Coin-lang.com) and the [Forked Coin documentation](https://Forked Coin-lang.com).

To jump straight to examples, go [here](https://github.com/Forked Coin/Forked Coin/tree/master/examples). For the latest Rust and TypeScript API documentation, see [docs.rs](https://docs.rs/Forked Coin-lang) and the [typedoc](https://www.Forked Coin-lang.com/docs/clients/typescript).

## Packages

| Package                 | Description                                              | Version                                                                                                                          | Docs                                                                                                            |
| :---------------------- | :------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------- |
| `Forked Coin-lang`           | Rust primitives for writing cross-chain programs         | [![Crates.io](https://img.shields.io/crates/v/Forked Coin-lang?color=blue)](https://crates.io/crates/Forked Coin-lang)                     | [![Docs.rs](https://docs.rs/Forked Coin-lang/badge.svg)](https://docs.rs/Forked Coin-lang)                                |
| `Forked Coin-spl`            | CPI clients for SPL and BEP programs                     | [![crates](https://img.shields.io/crates/v/Forked Coin-spl?color=blue)](https://crates.io/crates/Forked Coin-spl)                          | [![Docs.rs](https://docs.rs/Forked Coin-spl/badge.svg)](https://docs.rs/Forked Coin-spl)                                  |
| `Forked Coin-client`         | Rust client for Forked Coin cross-chain programs              | [![crates](https://img.shields.io/crates/v/Forked Coin-client?color=blue)](https://crates.io/crates/Forked Coin-client)                    | [![Docs.rs](https://docs.rs/Forked Coin-client/badge.svg)](https://docs.rs/Forked Coin-client)                            |
| `@Forked Coin/sdk`           | TypeScript client for Forked Coin programs                    | [![npm](https://img.shields.io/npm/v/@Forked Coin/sdk.svg?color=blue)](https://www.npmjs.com/package/@Forked Coin/sdk)                     | [![Docs](https://img.shields.io/badge/docs-typedoc-blue)](https://Forked Coin.github.io/Forked Coin/ts/index.html)        |
| `@Forked Coin/cli`           | CLI to support building and managing cross-chain apps    | [![npm](https://img.shields.io/npm/v/@Forked Coin/cli.svg?color=blue)](https://www.npmjs.com/package/@Forked Coin/cli)                     | [![Docs](https://img.shields.io/badge/docs-typedoc-blue)](https://Forked Coin.github.io/Forked Coin/cli/commands.html)    |

## Note

- **Forked Coin is in active development, so all APIs are subject to change.**
- **This code is unaudited. Use at your own risk.**

## Examples

Here's a cross-chain counter program that deploys to both Solana and BNB Chain, where only the designated `authority` can increment the count on both networks:

```rust
use Forked Coin_lang::prelude::*;

declare_id!("Fg6PaFpoGXkYsidMpWTK6W2BeZ7FEfcYkg476zPFsLnS");

#[program]
mod counter {
    use super::*;

    pub fn initialize(ctx: Context<Initialize>, start: u64) -> Result<()> {
        let counter = &mut ctx.accounts.counter;
        counter.authority = *ctx.accounts.authority.key;
        counter.count = start;
        Ok(())
    }

    pub fn increment(ctx: Context<Increment>) -> Result<()> {
        let counter = &mut ctx.accounts.counter;
        counter.count += 1;
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize<'info> {
    #[account(init, payer = authority, space = 48)]
    pub counter: Account<'info, Counter>,
    pub authority: Signer<'info>,
    pub system_program: Program<'info, System>,
}

#[derive(Accounts)]
pub struct Increment<'info> {
    #[account(mut, has_one = authority)]
    pub counter: Account<'info, Counter>,
    pub authority: Signer<'info>,
}

#[account]
pub struct Counter {
    pub authority: Pubkey,
    pub count: u64,
}
```

### Creating Cross-Chain Tokens

```bash
# Create a token on both Solana and BNB Chain with one command
Forked Coin token create --name "MyToken" --symbol "MTK" --networks solana,bnb

# Deploy to both chains
Forked Coin deploy --target all
```

For more, see the [examples](https://github.com/Forked Coin/Forked Coin/tree/master/examples)
and [tests](https://github.com/Forked Coin/Forked Coin/tree/master/tests) directories.

## Architecture

Forked Coin uses a unified runtime that translates your program logic into native operations for both Solana and BNB Chain. The framework handles:

- **Cross-chain account management**: Seamless state synchronization
- **Token bridging**: Automatic token creation and management across both chains
- **Transaction routing**: Intelligent routing to the appropriate network
- **Unified wallet integration**: Single wallet interface for both chains

## Roadmap

- [ ] Enhanced cross-chain messaging
- [ ] Native DEX integration
- [ ] Advanced bridge mechanics
- [ ] Support for additional chains
- [ ] Cross-chain NFT standards

## License

Forked Coin is licensed under [Apache 2.0](./LICENSE).

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in Forked Coin by you, as defined in the Apache-2.0 license, shall be
licensed as above, without any additional terms or conditions.

## Contribution

Thank you for your interest in contributing to Forked Coin!
Please see the [CONTRIBUTING.md](./CONTRIBUTING.md) to learn how.

## The Future is Cross-Chain

Forked Coin represents the future that Anatoly envisioned: a world where blockchain networks work together seamlessly, where developers aren't constrained by chain boundaries, and where users experience the best of multiple ecosystems through a single, unified interface.

### Thanks ❤️

<div align="center">
  <a href="https://github.com/Forked Coin/Forked Coin/graphs/contributors">
    <img src="https://contrib.rocks/image?repo=Forked Coin/Forked Coin" width="100%" />
  </a>
</div>


