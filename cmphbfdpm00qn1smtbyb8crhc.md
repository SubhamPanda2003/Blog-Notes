---
title: "Geohash Explained: How Facebook and Uber Find Nearby Friends and Cabs

"
datePublished: 2026-05-22T19:31:26.892Z
cuid: cmphbfdpm00qn1smtbyb8crhc
slug: geohash-explained-how-facebook-and-uber-find-nearby-friends-and-cabs

---

* * *

How Facebook finds your nearby friends or how uber gets you the nearby cab? The answer is geo hash and quad tree. There is also something called Google S2.

How does a geo hash works? Our problem statement is can we get a hash code for two region and tell their approximate distance or whether they are neighbors of each other or not. The first thing that comes to our mind is an evenly divided map. Just divide the map into evenly divided grids.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/78da67c7-274f-4abe-bf17-1dd5d8cee8a6.png align="center")

This works good when our nearby friend/cab radius is fixed and static. If its predefined to be 100km we can divide the map and store the grid info for further use. But the issue occurs when we change 100km to 150km. All the stored data becomes irrelevant.

Taking the above process a bit deeper . Can we create a hashcode for each region. The hashcode should be generated in a way that just by looking at the code we can say how far they are.

If we divide the map into two vertical parts and give one part code 0 and the other as 1.

We divided the 5000km x 5000km into two parts. Doing that again horizontally. Now we have the map broke into four parts and we have four codes assigned to them. each box becomes 2500km x 2500km. Each of the small grid boxes can be again broken to smaller cells just by following the above process. Repeating the process again and gain we can increase the granularity to an extent that we need.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/1d36d4e7-24ea-46f3-a1dc-a11c2171a240.png align="center")

We divided the map into 4 parts and the radius becomes r/2. Break it into 16 parts each small part would be r/4. so its basically r/(sqr root n).

All the above numbers are approximate. As we are dividing a round object into square grids so there will be a bit of difference.

If we calculate the length of the hashcode:  
For dividing it into 2 parts we need 1  
For dividing it into 4 parts we need 2.

For a hashcode of size n we can break the map into 2^n blocks.

Below table shows what length of our hash and the cell size.

| GeoHash Length | Cell Size (Width × Height) | Approx Radius Coverage |
| --- | --- | --- |
| 1 | 5000 km × 5000 km | ~2500 km |
| 2 | 1250 km × 625 km | ~630 km |
| 3 | 156 km × 156 km | ~78 km |
| 4 | 39 km × 19.5 km | ~20 km |
| 5 | 4.9 km × 4.9 km | ~2.4 km |
| 6 | 1.2 km × 0.61 km | ~0.61 km |
| 7 | 153 m × 153 m | ~76 m |
| 8 | 38 m × 19 m | ~19 m |
| 9 | 4.8 m × 4.8 m | ~2.4 m |
| 10 | 1.2 m × 0.6 m | ~0.6 m |
| 11 | 15 cm × 15 cm | ~7 cm |
| 12 | 3.7 cm × 1.9 cm | ~2 cm |

A neighboring cell would have a difference in their most insignificant digit. 1011 and 1010 will be neighbors. In this way if we configure our hashcode to 76m, we can configure it in a way to get 2.4km radius just by selecting all the grids whose hash value differs by last 4 digit. The first five hash character should be same.

Now we know how we know how a geo hash works. But there is an issue. If the cell is on the edge of the map. As earth is a spherical object the far ends of the Map are actually neighbors on earth. But they would have their complete hash different.

![](https://cdn.hashnode.com/uploads/covers/6a06127cbaf09db7a628dd57/913a5cd8-c214-4da8-b557-64f347220358.png align="center")

Grid 1 and 2 are neighbors but would have completely different hash.  
We will discuss more on the above problem and how can we tackle in the above scenario.

We will discuss more on the above on upcoming articles.

* * *