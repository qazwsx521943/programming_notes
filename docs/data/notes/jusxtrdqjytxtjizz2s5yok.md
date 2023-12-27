
## 前情提要

今天的需求是，前端的資料流以及畫面渲然只需要部分的資料，但是後端訂出的API格式多給了很多暫時不需要的資料。

## 資料處理步驟

### Step1 先釐清前端所需要的資料結構

於是我們考量了`GroceryStore`跟`Product`之間的關聯性，我們定義出了下方的類:

```swift
// 每間商店包含 商店名、商品陣列
struct GroceryStore {
    var name: String
    var products: [Product]

    struct Product: Codable {
        var name: String
        var points: Int
        var description: String?
    }
}
```

### Step2 建立一個中繼的類去Decode我們拿回來的資料

但是今天API定出來的資料格式如下：

```json
[
    {
        "name": "Home Town Market",
        "aisles": [
            {
                "name": "Produce",
                "shelves": [
                    {
                        "name": "Discount Produce",
                        "product": {
                            "name": "Banana",
                            "points": 200,
                            "description": "A banana that's perfectly ripe."
                        }
                    }
                ]
            }
        ]
    },
    {
        "name": "Big City Market",
        "aisles": [
            {
                "name": "Sale Aisle",
                "shelves": [
                    {
                        "name": "Seasonal Sale",
                        "product": {
                            "name": "Chestnuts",
                            "points": 700,
                            "description": "Chestnuts that were roasted over an open fire."
                        }
                    },
                    {
                        "name": "Last Season's Clearance",
                        "product": {
                            "name": "Pumpkin Seeds",
                            "points": 400,
                            "description": "Seeds harvested from a pumpkin."
                        }
                    }
                ]
            }
        ]
    }
]
```

於是我們建立了一個中繼的類，去接收傳過來的資料。

```swift
struct GroceryStoreService {
    let name: String
    let aisles: [Aisles]

    struct Aisles: Decodable {
        let name: String
        let shelves: [Shelves]

        struct Shelves: Decodable {
            let name: String
            let product: GroceryStore.Product
        }
    }
}
```

### Step3 把Step1建立的類透過`extension`讓我們可以透過中繼類初始化我們真正需要的資料

```swift
extension GroceryStore {
    init(from service: GroceryDataService) {
        name = service.name

        products = []
        for aisle in service.aisles {
            for shelf in service.shelves {
                product.append(shelf.product)
            }
        }
    }
}
```