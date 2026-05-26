---
title: "Adaptive ETA for Google Maps: Hierarchical Tile Indexing for Faster Traffic Updates"
datePublished: 2026-05-26T19:48:08.625Z
cuid: cmpn1s9bl00e12elx1w034jcv
slug: adaptive-eta-for-google-maps-hierarchical-tile-indexing-for-faster-traffic-updates

---

Google maps breaks map into small tiles. Now for each zoom level we store different tiles. The tiles are precomputed at different levels and stored in a CDN for faster delivery.  
For better understanding, level 0 means the whole map at 256\*256 pixel. At level 1 the map is divided into 4 tiles with total combined resolution of 1024\*1024 pixel. Now for level 2 its 16 tiles and so on. You can easily create a genric expression for the above.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/805ce71f-a8d9-4036-8f53-41668bf4aff2.png align="center")

If there is a origin and destination that user needs to traverse. Below example shows the route from tile a to tile f.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/b9965488-829c-4fec-b769-4dd50fa1bd85.png align="center")

Now imagine there is a huge traffic in tile c. We need to go through the whole user database scanning through each path to change the ETA for reaching at the destination. If there are N user and M average routing path length, its going to be N\*M.  
Is there a faster way of doing that?  
What if we store the parent tile instead of each smaller tiles?

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/75358396-7162-49c1-9cdc-68971afbb712.png align="center")

The advantage of the above representation : Just check whether the effected tile(for us it was tile c) is present in the last tile or not as it is the super parent tile.  
Now our time complexity becomes N, that is number of users.

But Give this a though:

If all my information are present in the last tile what is the requirement of the previous tiles.