# Sonic Whitepaper

> This is an early version of this whitepaper. We intend to expound on it more in the coming weeks.

The Solana blockchain is renowned for its high throughput capabilities and relatively low gas fees, and it has seen rapid growth in recent years. The number of wallet accounts grew from 100,000 in 2022 to 1 million in 2023, and is expected to reach 5 million in 2024, 10 million in 2025, 25 million in 2026, and 50 million in 2027.&#x20;

<figure><img src="../.gitbook/assets/chart (2).png" alt=""><figcaption><p>Solana Number of Wallet Accounts vs. Year</p></figcaption></figure>

Simultaneously, the growth of dAPPs and DeFi activity is even faster, with the number of transactions executed per month jumping from an average of 4,188,712 per day in January 2022 to 205,823,984 per day in January 2024. Even by the most conservative estimates, the number of daily transactions is expected to far exceed 4 billion by 2026, and more likely reach tens of billions.&#x20;

<figure><img src="../.gitbook/assets/chart (3).png" alt=""><figcaption><p>Daily Transactions (per day) vs. Year</p></figcaption></figure>

Clearly, under such demand pressure, the throughput capacity provided by the current architecture of Solana L1 will face significant challenges. Currently, under the condition of 2500-4000 TPS, the average ping time of the Solana Cluster fluctuates between 6s and 80s, with most of the time around 40s. Furthermore, historical data (from February to September 2023) shows that when TPS exceeded 4000, the success rate of Solana transactions only reached 70% to 85%. Beyond the physical delays and network fluctuations caused by network conditions, it's clear that the main reason is related to the increasing saturation of TPS. Based on the growth trend of Solana, it's predicted that in the coming years, TPS values will reach 1,000,000 to tens of millions, which will lead to more significant performance issues.



And the situation may be even more severe. With the continuous emergence of dAPPs, although Solana has more performance advantages compared to Ethereum, it faces severe challenges from a large number of small games or a single large game, especially Fully On-Chain Games (FOCG), where hundreds of thousands or even millions of users' on-chain interactions will severely impact the main chain performance of Solana L1, especially during certain special operational activities (such as server launches, holiday events, flash sales, etc.), where the instantaneous impact can be very large and frightening. This will not only affect the performance of the entire Solana L1 chain but also the playability and user experience of each game, including game response speed and data availability. It goes without saying that poor user experience is fatal for any game.

Sonic is designed to address such scenarios, being able to comfortably handle the large and high instantaneous transaction impacts of game-centric dAPPs.&#x20;

\
