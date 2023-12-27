---
id: fb3cv62eok3am0c3pgwwtfp
title: WebRTC signalling
desc: ''
updated: 1703591324237
created: 1703589664596
---

## 如何用Firebase Collection建立一個signalling server?

### 建立一個直播間

1. 建立一個Stream Collection，紀錄存在的直播間
2. 當使用者按下直播時，我們會透過`WebRTCClient`的`peerConnection`產生一個`RTCSessionDescription`(後續簡稱`sdp`，代表發起直播者提供的`offer`)，他產生的`sdp`我們會將它設為local description。接下來，我們會在Stream Collection建立一個新Document，並將這個`sdp`的資訊存在這個Document中，並給一個`offer`的key。

    ```javascript

    offer: {
      type: offer.type,
      sdp: offer.sdp
    }

    ```

3. 我們開始監聽這個直播間是否有人進入，如果有的話，將這個進來的人的`sdp`設為發起直播者的remote description

### 加入一個直播間

1. 使用者選擇一個想看的直播間，並按下加入，我們會將使用者的remote description設定成開啟直播者的`sdp`，並請`WebRTCClient`的`peerConnection`產生一個`answer`的`sdp`，並將其設為local description。

2. 將產生的answer `sdp`寫入該直播間的document field

    ```javascript
    answer: {
      type: answer.type,
      sdp: answer.sdp
    }
    ```

3. 這時候開啟直播的人就會監聽到stream doc的變動，並將加入者的`sdp`設為其remote description，並完成了發起直播與加入直播之間的`sdp`交換

### Collect ICE candidates

