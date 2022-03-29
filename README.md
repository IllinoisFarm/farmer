# Illinois Farmer

Member: Jongwon Park (jwp6)

## Introduction
Decentralized Finance (DeFi) is booming at an unparalleled speed with ambitious dreams to overthrow the traditional finance in the coming decades. As DeFi introduces novel concepts and technical innovations nearly daily, its exponential growth has entailed a sizeable amount of inefficiencies due to the lack of unifying protocols. Hence, the field presents enormous opportunities to leverage those inefficiencies, including arbitrages, bridges, flash loans, MEV, LP & yield farming, and liqudity mining.

The system initially focuses on Ethereum's DeFi ecosystem as it's the most established with an invigorating developer community. It mainly explores untapped arbitrage opportunities between decentralized exchanges (DEX) by calculating possible swap paths and the minimal extractable profit (MEP). If MEP exceeds the (estimated) maximal cost of arbitrage, the transactions are submitted to the mempool. Additional mechanisms are implemented to combat against MEV attacks, though the protection techniques aren't rigorous enough for realistic usages. As Rust is safe and extremely efficient, we explore different ways to utilize the upsides of the language.

## System Overview
This is a very simplified overview that is subjected to changes.
1. System opens multithreaded connections to multiple DEX smart contracts.
    1. Get liquidity pool information, grabbing max N tokens based on some pre-fixed criteria.
    2. Grab prices of those tokens using the smart contract.
    3. When a thread grabs all information on a DEX, it instantly returns the data, spawns a new thread to repeat the task. Old thread is destructed.
2. With constant inflow of data, system runs path calculations every X seconds using the best data available.
    1. The exact path calculation still has to be sorted out.
3. With each path, system calculates MEP.
4. System calculates the current gas fee for priority transaction, and sniff the mempool to see if any similar transactions exist.
5. For any paths exceeding MEP, system submits the whole path transactions onto the mempool.
    1. This process is another pain-point, as there are other arb bots sniffing the mempool and responding to any profitable transactions by either frontrunning, backrunning, or sandwiching (both).
    2. Gas fee prediction and mining time prediction will be critical to the successful execution of the transactions.


## Possible Challenges
1. Fetching data from multiple DEXs concurrently (multithreading) and ensuring that the system calculates opportunities on the latest data possible.
2. Ensuring that best path & arb calculations don't slow down the program. For instance, three layers of n tokens each between two pairs can produce `3^n` paths, and k layers produce `k^n` paths (which can very easily explode).
3. Rigorous testing of the software with as many edge cases as possible.
4. Deploying the system on testnet vs. mainnet. The liquidity and actions between testnet and mainnet is immeasurably different, hence the testnet deployment for testing can only do so much.

## Future Explorations
Beyond just DEX arbs, we will investigate yield farming & LP arbs, exploring permutations of countless stablecoins and altcoin paris.
