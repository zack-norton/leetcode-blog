---
title: LeetCode 121. Best Time to Buy and Sell Stock
date: "2022-08-09T11:59:03.284Z"
description: "Solution to LeetCode 121. Best Time to Buy and Sell Stock"
---

[Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

> You are given an array ```prices``` where ```prices[i]``` is the price of a given stock on the $i^{th}$ day.
>
> You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.
>
> Return the *maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return ```0```.

### One Pass

In this problem, the maximum profit can be defined as the difference between the minimum price and the maximum price after it. Knowing this, we can simply loop through the array once, checking each price. If the current price is less than our current minimum price, we set the current minimum price to the current price and continue. Otherwise, we calculate the difference between the current price and our current minimum price and compare it to the max.

```
for price in prices
    if price < minPrice
        minPrice = price
    else
        maxProfit = max(maxProfit, price - minPrice))
```

We can implement the above algorithm into a working function.

#### C#
```csharp
public int MaxProfit(int[] prices) {
    int maxProfit = 0;
    int minPrice = int.MaxValue;
    
    for(int i = 0; i < prices.Length; i++){
        if(prices[i] < minPrice){
            minPrice = prices[i];
        }
        else{
            maxProfit = Math.Max(maxProfit, prices[i] - minPrice);
        }
    }
    
    return maxProfit;
}
```

#### Rust
```rust
pub fn max_profit(prices: Vec<i32>) -> i32 {
    use std::cmp;
    
    let mut maxProfit = 0;
    let mut minPrice = i32::MAX;
    
    for price in prices.iter() {
        if price < &minPrice {
            minPrice = *price;
        }
        else {
            maxProfit = cmp::max(maxProfit, *price - minPrice);
        }
    }
    
    return maxProfit;
}
```

The time complexity is $O(n)$ since we just look at every value in the array once. The space complexity is $O(1)$.

