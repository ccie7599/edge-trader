# Edge Trader - an Edge-Native, Distributed Options Trading Game running on NATS.io

I built a realtime, distributed Options Trading game to highlight benefits, value, and best practices when building an Edge Native application.

The game displays a realtime price chart, and lets players buy or sell Call or Put Options to earn profits.

The game logic runs in a nodejs container, and uses a NATS.io cluster to maintain distributed state across the game, including functions for Leaderboard, Price Broadcast, Chat, Announcement, Order Processing, Cash and Positions Management, and Options pricing.

The game is built to be delivered through Akamai Content Delivery, Security, and Load Balancing systems. Sample HTML pages for the game can be found in examples. 

A running demo of the game can be accessed here - https://workshop.connected-cloud.io/scoreboard/testscore.html?origin=bapley

## Game Deployment Architecture
![image](https://github.com/user-attachments/assets/4a2f0dce-b4e5-4e93-a9d5-79b26237a7ea)

## Running a Distributed Edge-Native Game or Application 

Whether the intent is to build a multiplayer game, a realtime scoreboard, or enhance an existing API with better performance and more efficient delivery, this game demo highlights values of an Edge-Native, distributed architecture. 

* The stock price is published to a single NATS.io node, which then distributes it across the NATS.io cluster. Clients can connect to the scoreboard via HTTPS, and using Server-Sent Events, the latest price is published globally through Akamai's CDN and Security network. This offers faster performance vs. a centralized architecture, and offloads the origin source-of-truth by 100% (as requests never have to return to origin).

* Orders are placed through a Websockets channel that is also embedded in HTML and also transmitted through Akamai's CDN and Security network. The NATS.io system keeps orders in a strongly consistent datastream, ensuring ordering and strict acknowledgement of delivery. The stream is configured as a work queue, so that orders will be processed exactly-once, while ensuring that multiple order processors are available for redudancy.

* Positions and Cash Management is processed by each game node in the cluster, read from a durable stream, and stored locally for fast query response. No matter what node a player connects to, they will receive a consistent view of their cash and positions.

* All of the game components are open-source, cloud-native platforms, making the game easy to setup, modify, administer, and scale up or down.
