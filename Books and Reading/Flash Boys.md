Flash Boys: A Wall Street Revolt
Michael Lewis

# Overview
A narrative story about the emergence of High Frequency Trading in the early 00's.  How HFTs leveraged differential access times to various exchanges and dark pools to take risk-free pennies per share of bulk trades and what was done to show and tackle the problem.

# Notes
* Lots of good background on the emergence of HFT.
  - Companies would measure the time between inbound orders to identify the bank looking to trade
  - HFTs would be able to see an order, place and execute another order to make the price move before other trades from a bank could come through
  - This was made worse in a dark pool, as the order types weren't known & were complex
  - This is "fastest takes all winnings" - if you were slightly behind the next HFT, you were too slow
- There are more exchanges than most people realize, and they don't all have public rules: https://www.nyse.com/article/market-focus.  Lewis points out that a broker is required to give you best execution, but that the definition of "best" leaves a timeframe ambiguous.  What's best can change a thousand times in a second.
* 18 bullet points on how complex systems fail https://how.complexsystems.fail/
* Lots of narrative of how RBC found the HFT folks, figured out what they were doing, and designed a system to minimize it.  Various career paths, and people getting arrested for stealing source code.

### Follow-on reading
https://how.complexsystems.fail/

### Rabbit holes
- This all requires an arms race in speed, and networks are slow.  Enter stupidly low-latency network equipment.  https://www.cisco.com/site/us/en/products/networking/cloud-networking-switches/nexus-3550-portfolio/index.html#tabs-9cfa4a460b-item-b8ba101fed-tab.  
- A deep dive into the ASICs of fast switches https://www.nextplatform.com/2018/06/20/a-deep-dive-into-ciscos-use-of-merchant-switch-chips/.  Apparently some of the cisco switches are FPGA powered.
- Normally, switching equipment has a latency of 50-125 uS of latency, fast switches get down to 5.  The HFT targeted switches are ~200 nanoseconds.  
- This requires a completely different way of approaching computers, with "context-switch-free" coding techniques.  See https://www.reddit.com/r/cpp/comments/10quwfs/what_the_heck_is_contextswitchfree_code/
- The cables get pretty expensive: https://www.reddit.com/r/homelab/comments/pav8iw/whats_the_difference_between_infiniband_vs_sfp/
- But worth it? https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_infiniband_and_rdma_networks/understanding-infiniband-and-rdma_configuring-infiniband-and-rdma-networks
  "RDMA provides access between the main memory of two computers without involving an operating system, cache, or storage. Using RDMA, data transfers with high-throughput, low-latency, and low CPU utilization."
