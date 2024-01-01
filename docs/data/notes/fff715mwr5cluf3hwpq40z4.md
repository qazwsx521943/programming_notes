
## Factory Pattern 如何隱藏複雜性？

將特定物件複雜的初始化程序，封裝到一個factory class，當我們需要特定物件就從這個factory class取得。

假設將建立彈跳視窗物件的code封裝到一個class(factory)，當我們app有view需要用到彈窗時，僅需要透過這個factory製造出我們想要的彈窗。

## How to implement Factory Pattern?

以彈跳視窗為例：

1. 建立一個interface(protocol)，每個彈窗都需要可以
   1. 點擊關閉
   2. 點擊確認

2. 建立Factory class