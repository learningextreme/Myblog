---
title:  "IS-IS Route Leak"
date:   2025-03-19
layout: single
author_profile: false
category: wire
comments: false
toc: true
toc_label: "IS-IS"
toc_sticky: true
---

To configure a route leaking on an IS-IS network:

Configure the route leak on the L1/L2 routers:

An L1/L2 router, when connected to an L1 router, transmits solely L1 updates. Configure route leaking between the L1 and L2 routers using policies and export functions to allow the L1 router to learn specific routes from the L2 router.
{: .text-justify}

<img src="/assets/images/ISIS Route Leak.png" alt="ISIS topology" style="border: 2px solid black; box-shadow: 5px 5px 5px rgba(0, 0, 0, 0.5);"> 

[ ![](sassets/images/ISIS Route Leak.png) ](/assets/images/ISIS Route Leak.png)

```
set policy-options policy-statement RTLeak term 1 from protocols isis
set policy-options policy-statement RTLeak term 1 from level 2
set policy-options policy-statement RTLeak term 1 from route-filter 6.6.6.6/32 exact
set policy-options policy-statement RTLeak term 1 to protocol isis
set policy-options policy-statement RTLeak term 1 to level 1
set policy-options policy-statement RTLeak term 1 then accept
set protocols isis export RTLeak
```

In our topology:
- R1, R2, and R3 are in area 49.0001.0000.0000.0000.00 (circled in <span style="color:green">Green</span>.)
- R4, R5, and R6 are in area 49.0002.0000.0000.0000.00 (circled in <span style="color:Red">Red</span>.)
- R2, R3, R4, and R5 are L1/L2 routers.
- R1 and R6 are configured as L1 routers.

R1’s routing table will not include the 6.6.6.6 route. To enable R1 to learn route to 6.6.6.6 network specifically, configure the route leak on R2 using the aforementioned commands.
{: .text-justify}