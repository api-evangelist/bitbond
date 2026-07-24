---
name: Deploy and manage a compliant ERC-20 token with Bitbond Token Tool
description: >-
  Operating instructions for using the official Bitbond Token Tool MCP server to
  estimate cost, deploy a compliant ERC-20 token, and run its lifecycle and
  compliance controls from an AI agent.
api: mcp/bitbond-mcp.yml
transport: stdio (npx -y token-tool-mcp)
operations:
- list_chains
- estimate_cost
- deploy_token
- get_token_info
- mint_tokens
- transfer_tokens
- add_to_whitelist
- get_compliance_status
---

# Deploy and manage a compliant ERC-20 token (Bitbond Token Tool MCP)

The official Bitbond Token Tool MCP server (`npx -y token-tool-mcp`, stdio, no
network listener) lets an agent deploy and manage CertiK-audited token contracts.
Every step below maps to a real, published tool name in `mcp/bitbond-mcp.yml`.

## Prerequisites
- The MCP server runs locally over stdio; it signs transactions with the wallet
  it is configured to use. Fund that wallet on a **testnet first** (see
  `sandbox/bitbond-sandbox.yml` for faucets: Ethereum Sepolia, BSC testnet, Base
  Sepolia, Solana Devnet).

## Steps
1. **Pick a chain.** Call `list_chains` to see supported mainnets and testnets.
   Start on a testnet.
2. **Estimate cost.** Call `estimate_cost` for the target chain before deploying;
   deployment incurs a network fee.
3. **Deploy.** Call `deploy_token` with the token parameters (name, symbol,
   supply, standard). This deploys via the Token Tool factory.
4. **Confirm.** Call `get_token_info` (and `list_deployed_tokens`) to verify the
   contract address and on-chain state.
5. **Distribute / manage supply.** Use `mint_tokens`, `transfer_tokens`, and
   `burn_tokens` as needed.
6. **Apply compliance controls.** For restricted tokens use `add_to_whitelist` /
   `remove_from_whitelist` (or `add_to_blacklist` / `remove_from_blacklist`),
   then `get_compliance_status` to confirm transfer restrictions.

## Conventions & safety
- Actions are on-chain and irreversible once mined — always `estimate_cost` and
  deploy on a testnet before mainnet.
- There is no idempotency-key API; on-chain idempotency is enforced by wallet
  nonces. Do not resubmit a pending transaction blindly.
- See `conventions/bitbond-conventions.yml` and `authentication/bitbond-authentication.yml`
  for the wider Bitbond surface (the Offering Manager REST API is separate).
