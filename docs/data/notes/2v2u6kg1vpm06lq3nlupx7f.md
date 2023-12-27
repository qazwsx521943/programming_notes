
## 前情提要

很多時候前端定義出的model跟資料來源有點出入，例如：

我們定義的Model:

```swift
struct Team: Codable {
    var members: [Member]
    let id: String

    struct Member: Codable {
      let name: String
      let id: String
      let age: Int
    }
}
```

預期進來的資料是 ⇩

```json
{
    "members": [
        {
            "name": "Jason",
            "id": "123",
            "age": 20
        },
        {
            "name": "Lyy",
            "id": "456",
            "age": 18
        }
    ]
}
```

但是實際資料是 ⇩，這時候我們就會需要針對key去做特別的處理。

```json
// 實際的 JSON Data
{
    "members": [
        "Jason": {
            "id": "123",
            "age": 20
        },
        "Lyy": {
            "id": "456",
            "age": 18
        }
    ]
}
```

這個時候我們不應該為了Decode而再建立一個新的類別，專門去迎合資料的結構，而是加工處理進來的資料，讓他符合我們App開發的商業邏輯。

## 資料處理步驟

```swift
extension Team {
    struct MemberKey: CodingKey {
        var stringValue: String

        init?(stringValue: String) {
          self.stringValue = stringValue
        }

        var intValue: Int?

        init?(intValue: Int) {
          return nil
        }

        static let id = MemberKey(stringValue: "id")!
        static let age = MemberKey(stringValue: "age")!
    }

    enum CodingKeys: CodingKey {
        case members
        case id
    }

    func encode(to encoder: Encoder) throws {
        var container = encoder.container(keyedBy: CodingKeys.self)

        try container.encode(id, forKey: .id)
        var memberContainer = container.nestedContainer(keyedBy: MemberKey.self, forKey: .members)

        for member in members {
            let nameKey = MemberKey(stringValue: member.name)!

            var nestedContainer = memberContainer.nestedContainer(keyedBy: MemberKey.self, forKey: nameKey)
            try nestedContainer.encode(member.id, forKey: .id)
            try nestedContainer.encode(member.age, forKey: .age)
        }
    }

    init(from decoder: Decoder) throws {
        let container = try decoder.container(keyedBy: CodingKeys.self)
        let memberContainer = try container.nestedContainer(keyedBy: MemberKey.self, forKey: .members)
        id = try container.decode(String.self, forKey: .id)

        members = []
        for key in memberContainer.allKeys {
            let memberContainer = try memberContainer.nestedContainer(keyedBy: MemberKey.self, forKey: key)
            let age = try memberContainer.decode(Int.self, forKey: .age)
            let id = try memberContainer.decode(String.self, forKey: .id)

            members.append(Member(name: key.stringValue, id: id, age: age))
      }
    }
}
```