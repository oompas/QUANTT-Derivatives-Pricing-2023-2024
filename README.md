# QUANTT Derivatives Pricing — 2023/2024

Welcome to the official repository for the QUANTT 2023-2024 derivatives pricing team. This repository is a comprehensive collection of models, research findings, and insights developed by our team over the course of the year. Our goal is to provide details of our approaches to derivatives pricing, data sources, challenges, and the accuracy and predictions of our models.

![Banner](https://drive.google.com/uc?export=download&id=1cd3UN4nEdmHOHb9JQiZUovlGWXSpP8kg)

## Overview
![Jupyter Notebook](https://img.shields.io/badge/jupyter-%23FA0F00.svg?style=for-the-badge&logo=jupyter&logoColor=white)
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

Our project aims to explore and develop traditional models for pricing derivatives. We strive to enhance our understanding of financial models applied in real-world scenarios through rigorous research and practical implementation. Our team went into this with an interest in math and learning more about an unfamiliar topic, and QUANTT seemed like an excellent opportunity to gain some experience! Especially as a more theoretical and fundamental project like this aligns more with the kind of work we'd want to do in the Quant Finance industry.

## Models

#### Black-Scholes-Merton Model

- The Black-Scholes-Merton model is a cornerstone of modern financial theory, providing a mathematical framework for pricing European options. It was the simplest model to implement as the parameters were readily available, except for volatility, which was calculated by taking the standard deviation of AMD's daily percent returns.
- We found that it was analytically simple, making it computationally fast, but it's overly simplistic, and the key assumption that volatility is constant limits its accuracy compared to real market conditions. Overall, it's a good foundational model, which is why it's been subject to various extensions to address its limitations, such as our third model.

#### Monte-Carlo Simulation Model

- The Monte-Carlo simulation model leverages random sampling and statistical modelling to estimate mathematical functions and mimic the operations of complex systems. It's incredibly well-documented since it's a common model in more domains than just option pricing, which made it easy to examine similar implementations for Python libraries to potentially utilize.
- We found that it's fairly accurate and excels with minimal historical data (a luxury for this project). However, it also makes too many assumptions, such as constant volatility and assuming AMD's movement follows random sampling from a normal distribution. These assumptions make it poor at forecasting overlying market conditions or reacting to short-term market changes.
- Here's a sample of some of the random walks in our Monte-Carlo simulation:
![Monte-Carlo Visualization](https://drive.google.com/uc?export=download&id=1DrCui1GXrxlipLYhX7_f5W-FG9-lSJK7)

#### Hybrid Model
- We tried a hybrid Monte-Carlo model, as volatility shouldn't be a constant. It differed from the Monte-Carlo by calculating volatility and skew at each time step. Nonetheless, it was more conservative in pricing than the original Monte-Carlo model. We believe this is due to our historical data with low volatility creating a feedback loop since any new prices generated would be *close* to the original historical data, so when added to that data over and over again, the overall volatility of the data gets closer to it's mean. Thus, implementing non-constant volatility in this way was flawed.

#### Heston Model

- The Heston is an extension of the Black-Scholes-Merton by modelling variable rather than constant volatility. This made the Heston much more complex to implement than our other models, as it involved finding a way to calculate AMD's mean reversion rate, volatility of volatility, and numerous other model parameters.
- We found that its pricing was still generally conservative. It tended to predict better than the Black-Scholes-Merton but worse than our original Monte-Carlo or our hybrid Monte-Carlo. However, there is a higher chance of an error in our implementation with this model, as estimating specific parameters was difficult and likely requires a higher level of knowledge in math than our team possesses. Overall, if we believe our findings from the models to be accurate, we found that treating volatility as a random variable didn't improve our predictions

## Data Sources

Initially, Yahoo Finance was our primary data source. However, after identifying innumerable discrepancies between it and market data from other verifiable sources, we transitioned to exclusively using data from Interactive Broker's API. This decision was driven by our commitment to precision, acknowledging that even minimal inaccuracies can significantly impact model performance.

## Research

Our research concentrated on several key areas, including, but not limited to, the volatility smile & skew and the impact of news, like macroeconomic announcements and geopolitical events, on market volatility, which affects option pricing.

### Volatility Smile & Skew

- The volatility smile is a phenomenon where implied volatilities varying across strike prices are graphed and look like a smile. The primary cause for the changes in implied volatility is supply and demand, as further ITM or OTM options have less demand, and thus, the market's supply for them is much lower. The risk of providing liquidity for these options is higher, hence the higher IV.

![Investopedia Volatility Smile Diagram](https://drive.google.com/uc?export=download&id=1g-bZOdsjm6SCU3CHJWwAY1wzacNYyx72)

- The volatility skew describes AMD's changing IV with respect to strike price. A stock's skew can be used to understand its risk assessment, option pricing, and hedging strategies.

![Optionistics AMD Volatility Skew Graph](https://drive.google.com/uc?export=download&id=12a-nPvAN7oUTHamoLxTyVsCr6csldHoE)

### Impact of News on Implied Volatility

- Macroeconomic announcements and geopolitical events can significantly affect market volatility and, thus, options pricing.
- Our first example was the most recent FOMC meeting (March 20, 2024), where Jerome Powell, the United States Federal Reserve Chair, announced that they were still expecting to have three interest rate cuts this year. The uncertainty around the outcome of the meeting led market IV to jump significantly. For instance, on SPX shorter-term option chains, IV jumped to as high as ~28%.
- Our second example is that when the Russia-Ukraine war started, the S&P 500 dropped over 2.5% overnight due to this unexpected event. As a result, the pricing of options the next morning on this drop had changed significantly as IV jumped. Nonetheless, the market recovered quickly, and the options pricing balance was restored within a couple of days. 

## Accuracy & Predictions

#### First, let's take a look at what happened in the market over the school year:
![SPY movement over the school year](https://drive.google.com/uc?export=download&id=1SmiKEj3xBmcm1TEViLejUePPtysanXHr)

#### Now, let's contrast that with AMD's movement over the same timeframe:
![AMD movement over the school year](https://drive.google.com/uc?export=download&id=1tIgSDw-gCPtYiDWGzoXCrxeh1V_H_pWq)

- Both have been on an absolute tear overall, but AMD has faltered recently, topping out around $230 before falling back to ~$180. Why is that? Well, AMD, being a part of the semiconductor stock group, is naturally influenced by other semiconductor stocks. Recently, NVDA topped out near $1,000, and SMCI topped out near $1,250; these two stocks lost momentum, causing the entire stock group to lose momentum, which also transferred into AMD's movement.

### Our Findings & Results
- Given our limited access to historical option data, our models follow their standard — albeit basic — implementation. Hence, none of the models were remarkably accurate when tested against real-market data. Nonetheless, our Monte-Carlo model had the highest accuracy in forecasting AMD pricing; thus, it performed best when used to predict option prices on AMD.
- If we made predictions solely from our quantitative models, they *essentially* suggest buying most available put permutations on the AMD options chain as it forecasts AMD's stock price to be quite lower than its current price. Realistically, though, there are factors that they cannot consider in simplistic modelling. Firstly, Jerome Powell, the United States Federal Reserve chair, announced on March 20, 2024, that three interest rate cuts are still planned for this fiscal year. Further, AMD is *primarily* a semiconductor stock, part of a stock group with roughly cyclical movement. Lastly, AMD's Q4 events, where they announce new R&D, historically have caused the stock to rise. Hence, our actual chosen AMD option to *hypothetically* purchase would be AMD250117C00250000 (250C for January 17, 2025).

## Team
| ![Jordan Matus](https://drive.google.com/uc?export=download&id=1Ucw8tXeEeIzCuUz67dVtSeDBWi382w9z) | ![Ava Galassi](https://drive.google.com/uc?export=download&id=1yOYJOGfWNt-vSzfM9PRgjiXPIUz6CWT9) | ![Denis Khatnyuk](https://drive.google.com/uc?export=download&id=1zmHMrE9AEOIGz7zXIm16ksioyz2gER6q) | ![Owen Martens](https://drive.google.com/uc?export=download&id=1m_Ce3Da90u63LLQj6DOJEpoAM-dCAw0U) | ![Jack Switzer](https://drive.google.com/uc?export=download&id=1VTrLpG2Qcvc54Kdc3PSz2SmBCbZpcld4) | ![Daryan Fadavi](https://drive.google.com/uc?export=download&id=1RpmcVph-HPidpxulkwfeYPEt3d2B63Mr) |
|:-----------------------------:|:-----------------------:|:-------------------------:|:----------------------------:|:-------------------------:|:-------------------------:|
| [Jordan Matus](https://www.linkedin.com/in/jordanmatus/) <br> Project Manager | [Ava Galassi](https://www.linkedin.com/in/avagalassi/) <br> Team Lead | [Denis Khatnyuk](https://www.linkedin.com/in/dkhatnyuk/) <br> Team Member | [Owen Martens](https://www.linkedin.com/in/owen-martens-28239b261/) <br> Team Member | [Jack Switzer](https://www.linkedin.com/in/jack-switzer-ba102418a/) <br> Team Member | [Daryan Fadavi](https://www.linkedin.com/in/daryanfadavi/) <br> Team Member |

## Acknowledgements

We sincerely thank the QUANTT 2023-2024 executive team for their unwavering support, leadership, and organization. Their initiative to introduce the derivatives pricing team this year has been instrumental in providing us with invaluable learning opportunities. Here's hoping that next year will be an even greater success for the club!

We want to acknowledge Interactive Brokers as our intermediary in obtaining high-quality CBOE market data, which was essential for being able to test the accuracy of our models. Additionally, we want to acknowledge TradingView, Optionistics, and Investopedia for access to detailed diagrams used within our presentation and this readme.

We look forward to continuing our journey in the field of quantitative finance!

---

For more information or questions, please get in touch with us through LinkedIn.
