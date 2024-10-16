# zkBob Web Console

The simple tool to test and demonstrate zkBob solution possibilities 

# Running locally

Make sure you are using node js version higher or equal than `14.0.0`. The repo has been tested with `node v16.14.1` and `npm v8.5.0`

1. Clone repository and install dependencies

```bash
git clone https://github.com/zkBob/zkbob-console.git
cd zkbob-console
yarn install
```

2. Set appropriated settings in the `.env` file

3. Put circuit parameters and keys in `asset` folder. The same files should be located on relayer node

4. Run local bundle
```
yarn dev
```
5. Open your browser and enter [http://localhost:3000/](http://localhost:3000/) in the address line

It's recommended to clear your browser's history and cookies in case of you was used previous version of console

# Creating Docker container

Suppose you already done local running and set appropriated parameters and settings

1. Fix your docker ID in [this line](https://github.com/zkBob/zkbob-console/blob/0053ca2a63d00fd4be4e9bd646c05ffbdc2ecf3e/scripts/publish-docker#L4)

2. Build the prouction pack and push your container to the Docker Hub: `./scripts/publish-docker`


# Using console

## Account maintenance

`get-seed <password>` print the seed phrase for the current account (current password needed)

`get-sk <password>` get zkBob account spending key (current password needed,

`pools` list of the available pools

`switch-pool <pool_alias> <password>` switch to the another pool with the current spending key
  
## L1-level commands

`get-addres` get the linked account address. This address derived from account mnemonic phrase

`get-balance` get the linked account balance (native coins)

`get-token-balance` get the linked account balance (tokens)

`get-token-allowance <spender>` check token allowance to spend by provided address

`testnet-mint <amount>` mint some unshielded tokens (available on testnets only)

`transfer <to> <amount>` transfer native coins to the destination L1 address

`transfer-token <to> <amount>` transfer tokens to the destination L1 address

`approve-token <spender> <amount>` approve allowance to spend your token for the specified spender

## L2-level commands

`gen-shielded-address [number]` generate a new zkBob shielded address for the current pool (or several addressed)

`gen-shielded-address-generic [number]` generate a new zkBob universal shielded address (or several addressed)

`get-shielded-balance` get calculated private balance (with optimistic balance)

`deposit-shielded <amount> [times]` deposit some tokens into zk-account (depends on deposit scheme, you may need a few native coins). Specify `times` numeric value to repeat the operation several times

`deposit-shielded-ephemeral <amount> <index>` deposit some tokens from the internal ephemeral address (permit scheme)

`direct-deposit <amount> [times]` send tokens to the pool directly to receive it on own account. Additional approve transaction will be initiated if needed. Specify `times` numeric value to repeat the operation several times

`direct-deposit-native <amount> [times]` send native coins to the pool directly to wrap and receive it on own account. Specify `times` numeric value to repeat the operation several times

`transfer-shielded <shielded address> <amount> [times | +]` move shielded tokens to the another zkBob account (inside a pool). You can specify `times` numeric value to repeat the operation several times ***OR*** enter the multitransfer mode with adding `+` sign at the end of the command

`withdraw-shielded <amount> [address] [times]` withdraw shielded tokens from the zk accouint to the native address (to the your account if address is ommited). Specify `times` numeric value to repeat the operation several times

`history` print all transactions related to your account

`compliance-report` generate compliance report: history + evidence of transactions ownership

`pending-dd` print pending direct deposits for the account

## Transactions configuration

`max-transfer` calculate maximum available token amount for outcoming transaction (transfer or withdrawal)

`tx-amounts <amount> [amount2 amount3 ...]`, get transfer or withdrawal (will requested additionally) configuration for specified amount and fee per transaction. Type `+` sign at the end of the command to enter the multitransfer mode

`fee-estimate-deposit <amount>` estimate fee for depositing requested amount of tokens

`fee-estimate-transfer <amount> [amount2 amount3 ...]` estimate fee for transfering requested amount of tokens. Add additional token amounts (the only space accepted as a separator) to estimate fee for multitransfer mode

`fee-estimate-withdraw <amount>` estimate fee for withdrawing requested amount of tokens

`limits [address]` get pool contract limits for the specified address (linked account address will be used by default)

## Service commands

`shielded-address-info <shielded address>` get all available info for the shielded address

`internal-state` print your account and incoming notes (internal representation, for debug purposes only)

`root [index]` print local and remote Merkle tree root or retrieve just local one at the specified index (local and remote roots should be tha same for same indicies)

`siblings <index>` get left siblings at specified index (partial tree support). The index should be multiple of 128 and less than current index
 
`rollback <index>` rollback the user's state to the specified index (the index should be multiple of 128 and less than current index)

`sync` force synchronize user's state
    
`sync-stat` print available state synchronization statistics from the library

`get-ephemeral-address [index]` get the concrete ephemeral address or show first unused one

`get-ephemeral-used` show all used ephemeral addresses

`get-ephemeral-address-privkey <index>` get private key for concrete ephemeral address

`wipe-state` wipe internal state and history

`set-prover-mode <mode>` set prover mode (possible modes: Local, Delegated, DelegatedWithFallback)

`prover-info` print info about the used prover

`clear` clear the terminal

`reset` log out from the current account

`account-id` get the client account id (indexed DB name)

`support-id` print current support id (changed on each library initialization)

`version` print console and external services versions

`environment` print environment variables

`help` display CLI commands list

`tutorial` print usage example

## Gift cards commands

`gift-card-generate <balance> [quantity]` generates a bunch of gift cards, transfer funds. allows to download redemption links or copy from output.

`gift-card-generate-cloud <alias> <quantity> <balance> <token>` generates `<quantity>` cloud accounts with `<alias>_index` description using `<token>` to access admin API; generate QR codes to login into each account; makes shielded transfer of `<balance>` shBob to each created account; generates a report with account keys, redemptions urls and cloud Ids

`gift-card-balance <code_or_redemption_url> [code_or_redemption_url2 code_or_redemption_url3 ...]` retrieves gift cards balances

`gift-card-redeem <code_or_redemption_url>` redeems gift card to the current account
