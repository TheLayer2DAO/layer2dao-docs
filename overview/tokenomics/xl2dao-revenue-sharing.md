# 🚀 xL2DAO Revenue Sharing

### Description

Layer2DAO is a DAO that seeks to highlight and bring to the masses high impact Ethereum L2 protocols and projects. The DAO treasury receives revenue through LP provisioning, .L2 domain sales, NFT royalties, and partner projects.

25% of fees generated from anything other than L2DAO tokens will be converted to ETH (if received in a different form of asset) and sent to the vault for stakers to claim. The amount of each staker’s claim is pro-rata to all other stakers in the xL2DAO system.&#x20;

### Locking up L2DAO

Your total amount of xL2DAO depends on 2 inputs: the amount of L2DAO you stake, and how long you lock up your L2DAO tokens. The longer you lock up your tokens, the higher your proportional share of xL2DAO and the larger your percentage claim and voting power. The most you can lock your tokens up for is 24 months. If you lock up 1 L2DAO for 24 months, you receive 1 xL2DAO. If you lock up 1 L2DAO for 12 months, you receive 0.5 xL2DAO, and so on. Once your L2DAO is locked up, there is no way to unlock it until the lock period has expired. Be certain that you don’t need access to your L2DAO before you lock it up!

### Specifications&#x20;

* You can lock up your L2DAO from 2 weeks (= 0.02 xL2DAO / L2DAO) up to 2 years (= 1 xL2DAO / L2DAO).
* You can only lock until Thursdays, if you select any other day, it will lock it for the previous Thursday (e.g if you choose to lock until Friday, June 25th it will lock it until Thursday June 24th)
* As you get closer to your unlocking date, **your balance of xL2DAO decreases linearly**.
* xL2DAO is non-transferable&#x20;
* **WARNING :** You can only have **one locking schedule**. It means you cannot lock a share of your L2DAO for 1 year and another for 4 months.&#x20;
* You can extend your locking schedule at any time, but you cannot lock it for more than 2 years since you first locked some L2DAO.

### Contracts

Our contracts are forks of Curve’s veCRV contracts, modified slightly by Ribbon. These contracts have been audited extensively and are some of the most used contracts in DeFi. All contracts can also be [viewed on our GitHub](https://github.com/Layer2DAO/Layer2DAO/tree/main/contracts).

Vote-escrowed L2DAO (xL2DAO) - Arbitrum: [0xA7AF63b5154eB5d6Fb50a6d70d5C229e5f030AB2 ](https://arbiscan.io/token/0xa7af63b5154eb5d6fb50a6d70d5c229e5f030ab2)

Revenue Share Fee Distributor - Arbitrum: [0xC15DDD98341346A2d2C9bf0187f56666247dF4C6](https://arbiscan.io/address/0xc15ddd98341346a2d2c9bf0187f56666247df4c6)

### Implementation Details

User voting power $$𝑤𝑖$$ is linearly decreasing since the moment of lock. So does the total voting power 𝑊. In order to avoid periodic check-ins, every time the user deposits, or withdraws, or changes the locktime, we record user’s slope and bias for the linear function $$𝑤𝑖(𝑡)$$ in the public mapping `user_point_history`. We also change slope and bias for the total voting power $$𝑊(𝑡)$$ and record it in `point_history`. In addition, when a user’s lock is scheduled to end, we schedule change of slopes of $$𝑊(𝑡)$$ in the future in `slope_changes`. Every change involves increasing the `epoch` by 1.

This way we don’t have to iterate over all users to figure out, how much should $$𝑊(𝑡)$$ change by, neither we require users to check in periodically. However, we limit the end of user locks to times rounded off by whole weeks.

Slopes and biases change both when a user deposits and locks governance tokens, and when the locktime expires. All the possible expiration times are rounded to whole weeks to make number of reads from blockchain proportional to number of missed weeks at most, not number of users (which is potentially large).

For more details, please visit [curve docs](https://curve.readthedocs.io/dao-vecrv.html#querying-balances-locks-and-supply).
