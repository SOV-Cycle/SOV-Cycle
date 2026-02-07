# Pump.fun Fee Claim → Buyback → Burn Bot Setup Guide

## Quick Start

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Set Up Environment Variables

Create a `.env` file in the same directory with:

```env
# PumpPortal API Key (get from https://pumpportal.fun)
PUMPPORTAL_API_KEY=your_api_key_here

# Sol-Incinerator API Key (get from https://sol-incinerator.com)
SOL_INCINERATOR_API_KEY=your_sol_incinerator_api_key

# Your wallet private key (Base58 encoded)
WALLET_PRIVATE_KEY=your_private_key_here

# Your token's mint address
TOKEN_MINT=your_token_mint_address

# Optional: Custom RPC endpoint (default is public Solana RPC)
# SOLANA_RPC=https://your-custom-rpc.com

```

### 3. Run the Bot
```bash
python pumpfun_buyback_burn_bot.py
```

## How It Works

The bot performs three actions in sequence:

1. **Claim Creator Fees**: Withdraws all accumulated creator fees from pump.fun
2. **Buyback Tokens**: Uses the claimed SOL to purchase your token from the bonding curve
3. **Burn Tokens**: Removes the purchased tokens from circulation (reduces supply)

## Configuration

### Environment Variables

- `PUMPPORTAL_API_KEY`: Your PumpPortal API key
- `SOL_INCINERATOR_API_KEY`: Your Sol-Incinerator API key
- `WALLET_PRIVATE_KEY`: Your Solana wallet's private key (Base58 format)
- `TOKEN_MINT`: The mint address of your token
- `SOLANA_RPC`: (Optional) Custom RPC endpoint

### In-Script Settings

Edit these in the script if needed:

```python
PRIORITY_FEE = 0.00001  # SOL - increase for faster transactions
SLIPPAGE_BPS = 500      # 5% slippage (500 basis points)
```

## Usage Modes

### Mode 1: Run Once
Execute a single cycle manually

### Mode 2: Every 5 Minutes (Recommended)
Automatically claims fees, buybacks, and burns every 5 minutes

### Mode 3: Custom Interval
Set your own interval in minutes (e.g., 10, 15, 30, etc.)

## Important Notes

⚠️ **Security**:
- Never share your private key
- Use environment variables or .env file (don't hardcode keys)
- Consider using a dedicated wallet for this bot

⚠️ **Burn Mechanism**:
The bot uses Sol-Incinerator's API (https://sol-incinerator.com) to burn tokens. Sol-Incinerator permanently destroys the tokens AND reclaims the rent deposit (SOL) back to your wallet. This is a win-win - tokens are burned forever and you get back the storage fees!

⚠️ **Gas & Fees**:
- Each transaction requires SOL for gas fees
- Ensure your wallet has enough SOL for operations
- Priority fees can be adjusted based on network congestion

⚠️ **Pool Type**:
If your token has migrated to Meteora, change `pool: "pump"` to `pool: "meteora-dbc"` in the script.

## Getting Your API Keys

### PumpPortal API Key
1. Visit https://pumpportal.fun
2. Sign up or log in
3. Navigate to API section
4. Generate a new API key
5. Copy it to your .env file

### Sol-Incinerator API Key
1. Visit https://sol-incinerator.com or their docs at https://docs.sol-incinerator.com
2. Sign up for API access
3. Generate your API key
4. Copy it to your .env file

## Getting Your Private Key

⚠️ **CAUTION**: Handle with extreme care!

If using Phantom wallet:
1. Settings → Show Secret Recovery Phrase
2. Use a tool to convert your seed phrase to Base58 private key
3. Alternatively, export the private key directly if your wallet supports it

Better approach:
- Create a new dedicated wallet for this bot
- Fund it with only what you need
- Export that wallet's private key

## Troubleshooting

### "No fees claimed"
- Wait a bit - fees may be accumulating but not yet claimable
- Check if you actually have creator fees to claim

### "Buyback failed"
- Check your token mint address is correct
- Ensure there's liquidity on the bonding curve
- Try increasing slippage if the market is volatile

### "Transaction failed"
- Increase PRIORITY_FEE for faster confirmation
- Check you have enough SOL for gas
- Verify RPC endpoint is working

## Example Workflow

1. Token launches on pump.fun ✅
2. Trading generates creator fees 💰
3. Bot claims fees every 5 minutes 🤖
4. Uses 100% of fees to buyback your token 📈
5. Burns bought tokens → reduces supply 🔥
6. Repeat every 5 minutes → creates constant buy pressure & deflationary effect 📊

This creates a continuous cycle that automatically applies buy pressure and burns supply throughout the day!

## Support

For issues with:
- PumpPortal API: https://pumpportal.fun/docs
- Solana Python SDK: https://github.com/michaelhly/solana-py
- This bot: Check the code comments or customize as needed

---

**Disclaimer**: This bot is provided as-is. Always test with small amounts first. Crypto trading involves risk. Never invest more than you can afford to lose.
