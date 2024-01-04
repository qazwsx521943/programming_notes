
Flavor Flash 開發筆記

## Food App

- TA:

  1. 喜歡被動探索美食的人
  2. 有選擇障礙 / 不想花時間想要吃什麼的人
  3. 喜歡紀錄食物口感的人
  4. google評論有時候可以被刷出來、不如朋友圈一起建立可信的美食地圖

- Feature:

  1. 隨機選食物 -> 找附近的餐廳
  2. 左滑右滑收藏餐廳 (餐廳抽卡,UI要炫砲)
  3. 美食日記
     1. 飢餓狀態
     2. 上傳圖片，
        1. 後鏡頭拍食物分析食物的標籤(分析不準也可帶入自定義標籤)
        2. 前鏡頭拍人像分析人臉（喜怒哀樂）作為評價
     3. 寫下心得
  4. 可以查看自己曾到訪的餐廳，針對吃的次數給不同顏色圖釘、註解
  5. 好友的頁面可以查看他的美食日記、以及他的美食地圖(曾到訪的餐廳)
  6. 聊天系統
     1. 1對1
     2. 群組
     3. 傳jpg、png、gif
     4. 分享餐廳資訊
     5. (optional)影片
     6. (optional)投票要吃的餐廳
  7. 成就勳章：（月結、週結、小任務）
     1. 很常吃飯 => 飯桶
     2. 常常不同的餐廳 => 發現新大陸
  8. 週飲食習慣、月飲食習慣分析(3D呈現)
  9. APP顏色要可切換
  10. 日曆簽到系統，當天有發美食日記的話就算簽到成功
  11. 動態島、鎖屏提示(Activity Kit)

- 預計需要學的東西:
  1. MapKit
     1. Marker & Annotation
     2. selection
     3. contentBuilder
     4. mapStyle
  2. SpriteKit
  3. AVFoundation
  4. CoreML
     1. Vision
  5. Firebase APIs


- reference:
  1. https://github.com/PCChuang/Subminder
  2. https://github.com/TaiHsin/Travel
  3. https://github.com/pisck888/Hahabbit
  4. https://github.com/thisisalliet/FocusFoodie


![](/assets/images/ifoodie-webconfig.png)

我這次個人專案開發的是一款探索及紀錄美食的app，那會想要開發這款app的原因是因為我每天都會遇到午餐晚餐要吃什麼的問題，再加上女朋友有紀錄以及分享美食的習慣，於是我就決定以這兩個需求去開發這款美食App。

首先，首頁放的是我自己最常使用的功能，那我的使用情境就是在每天中午不知道要吃什麼時候就可以打開來點一下，並且透過調整地圖參數去調整你想看到的餐廳搜尋結果。

那進到地圖搜尋的頁面時，使用者可以透過點擊圖釘以及下方的餐廳卡片去移動到該餐廳的位置，點擊下方卡片也可以看到餐廳的基本資料。

如果我在瀏覽餐廳的時候看到了幾間還不錯的餐廳，我也可以餐廳資訊這邊先加到我的待吃清單，等到下一餐又不知道要吃什麼的時候，也可以從待吃的清單去做選擇。

那如果在地圖上有看到這餐想吃的餐廳時，你就可以點擊該餐廳的圖釘然後點旁邊的地圖icon去開啟apple map的導航。

那回到首頁點擊收藏的ICON就可以看到你待吃名單中的餐廳有哪些，如果吃了就可以把該餐廳移除待吃名單。

那這是探索附近餐廳的功能，接下來要帶過的是記錄美食的功能，使用者可以點擊這個相機的icon開啟前後鏡頭，開啟後使用者可以對鏡頭的顯示做一些調整，查看有沒有死角。

拍好之後也可以再次確認照片沒有問題，做一些簡單的編輯，那如果沒有問題就可以進一步寫下心得。

發文美食評論之後就可在美食足跡的社群中，看到自己跟朋友的發文，你可以對貼文留言&按讚。那如果有一些私底下想詢問的美食問題，也可以去聊天室建立群組聊天。

那除了在這個社群頁面可以看到其他朋友的發文，使用者也可以在個人頁面的foodprint中在地圖上查看他過往吃過的餐廳，也可以跟朋友做足跡的疊加。

## 專案遇到過的問題

### 要怎麼開啟不同硬體適合的鏡頭？ 為何不用 `UIImagePickerController`、`PHPPhotoPicker`？

   ```swift
      UIDevice.current.model

      UIDevice.current.systemName
   ```

   那我個人專案中並沒有使用到這個偵測型號的方法，
   我是透過設計一個camera物件，讓那個物件在初始化的時候透過AVFoundation的api去偵測該裝置可以開啟的鏡頭裝置。

   Camera初始化會做什麼？

- Step1

   AVCaptureDevice.DiscoverySession 拿到用戶裝置所有可用的鏡頭配置。

- Step2

   透過computed property去把可用的前、後鏡頭 filter 出來。

- Step3

   最後再從分類好的裝置中，綁定用戶該session要使用的前、後鏡頭配置，就完成相機初始化的部分。

### 要怎麼將相機preview的畫面順利接到畫面上?

先去暸解AVFoundation是如何建立一個capture session，那要建立一個capture session，我們會需要先拿到一個input device，然後建立一個output

![capture session](/assets/images/programming.side%20projects.capture-session.png)

- Step1

   我們想要達到的功能是一個前、後鏡頭，必須先要建立一個`AVCaptureMultiCamSession`實例。（在camera物件中初始化時建立）

   相機view出現在畫面上的時候，會去調用viewModel中camera物件的`start()`，開始串接Session的input、output。

- Step2

   因為camera是一個物件，我們要避免他重複串接Session的input跟output，我們額外添加一個`isMultiCaptureSessionConfigured`的property去紀錄。

   如果還沒串接過

   觸發camera物件的`start()`會開始
   讓camera class去conform一些AVFoundation定好 `AVCapturePhotoCaptureDelegate`

   在viewModel初始化的時候，會初始化camera物件，並透過closure的方式去請camera物件的回傳捕捉到的影像。那每次我們從camera物件中拿到新的image會更改viewModel中的properties，就會自動觸發view的rerender。
   那假設我這個部分沒有透過MVVM的架構而是透過MVC去實作，我如果相機的view有想要抽換，我可能就需要連帶

1. 要怎麼去分析圖片中的食物分類？
