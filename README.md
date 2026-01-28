# Compound V3 Comet Progressive Disclosure Skills

Progressive disclosure skills for Compound V3 Comet protocol development. These skills provide context-aware guidance for building on Compound V3, the monolithic money market optimized for single-asset borrowing.

## Installation

### Claude Code
```bash
claude mcp add-json cyotee-plugins '{"type":"stdio","command":"npx","args":["@anthropic-ai/claude-code-mcp-server"],"env":{"CLAUDE_PLUGINS_DIRECTORY":"/path/to/cyotee-claude-plugins/plugins"}}'
```

### OpenCode
Copy the `.opencode/skills/` directory to your project.

## Skills

| Skill | Trigger Keywords | Description |
|-------|-----------------|-------------|
| `comet-architecture` | Compound V3, Comet, architecture, monolithic | High-level protocol architecture |
| `comet-core` | supply, withdraw, borrow, repay, transfer | Core user operations |
| `comet-interest-rates` | interest rate, APR, utilization, kink | Kinked interest rate model |
| `comet-collateral` | collateral, collateral factor, assetsIn, supplyCap | Collateral system and factors |
| `comet-liquidation` | liquidation, absorb, buyCollateral, reserves | Absorb liquidation system |
| `comet-bulker` | Bulker, batch, invoke, native token | Batching multiple operations |
| `comet-rewards` | rewards, COMP, claim, tracking | COMP reward distribution |
| `comet-configurator` | Configurator, governance, governor, deploy | Governance and upgrades |

## Compound V3 Comet Key Concepts

### Single Borrowable Asset
Unlike V2, each Comet market has one borrowable base asset (e.g., USDC) with multiple collateral types.

### Principal-Based Accounting
```
User Balance = Principal × Interest Index

principal > 0  →  Supplying (earning interest)
principal < 0  →  Borrowing (paying interest)
```

### Collateral Factors
```
borrowCollateralFactor (82.5%) - Maximum borrowing power
liquidateCollateralFactor (85%) - Liquidation threshold
                          2.5%  - Safety buffer

liquidationFactor (93%) - Value applied at liquidation
                    7%  - Protocol fee
```

### Absorb Liquidation Model
```
1. Protocol absorbs underwater account (takes collateral + debt)
2. Anyone can buy collateral from reserves at discount
3. Discount = storeFrontPriceFactor × (1 - liquidationFactor)
```

### Kinked Interest Rates
```
Rate = Base + SlopeLow × min(Util, Kink) + SlopeHigh × max(0, Util - Kink)
```

## Repository Structure

```
compound-v3-comet/
├── contracts/
│   ├── Comet.sol              # Main protocol (supply, withdraw, etc.)
│   ├── CometExt.sol           # Extension (approve, allow, EIP-712)
│   ├── CometStorage.sol       # Storage layout
│   ├── CometConfiguration.sol # Config structs
│   ├── CometCore.sol          # Shared functions
│   ├── CometRewards.sol       # COMP distribution
│   ├── Configurator.sol       # Governance config
│   ├── CometFactory.sol       # Deploy implementations
│   ├── bulkers/
│   │   ├── BaseBulker.sol     # Batch operations
│   │   └── MainnetBulker.sol  # Mainnet extensions
│   └── liquidator/
│       └── OnChainLiquidator.sol  # Automated liquidation
├── test/                      # Foundry/Hardhat tests
└── deployments/               # Deployment scripts
```

## Deployed Addresses (Ethereum Mainnet)

| Contract | Address |
|----------|---------|
| cUSDCv3 | `0xc3d688B66703497DAA19211EEdff47f25384cdc3` |
| cWETHv3 | `0xA17581A9E3356d9A858b789D68B4d866e593aE94` |
| Configurator | `0x316f9708bB98af7dA9c68C1C3b5e79039cD336E3` |
| Rewards | `0x1B0e765F6224C21223AeA2af16c1C46E38885a40` |
| Bulker | `0x74a81F84268744a40FEbC48f8fB04a26e3F5bE9e` |

## License

BUSL-1.1 (matching Compound license)
