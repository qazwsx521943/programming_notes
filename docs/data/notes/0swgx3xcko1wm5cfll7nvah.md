
## What did **Actor** solved?

高併發所產生的data race問題。

## How did it solved?

透過Actor宣告的類別，instance會有一個數據隔離的特性，所有想要存取到該instance的外部資源都需要排入一個serial queue，以這個方式確保不會有同時修改到instance資源的問題。

## Actor isolation

以實例為單位，將內部資源與外部資源隔離。

- 需要隔離(會動到`mutable`屬性)：
  - `var`宣告的stored property、computed property
  - methods

- 不需隔離(`immutable`屬性無法被更改)：
  - 以`let`宣告的stored property

只有潛在可能更改到內部資源的訪問會需要隔離。

## `nonisolated`

有的時候，我們很確定某些computed property、function並不會引起data race，這時候我們就可以透過`nonisolated`在屬性前面做標記，減少性能損耗。

當然，標記`nonisolated`的computed property跟function在內部都不能存取到actor實體的`mutable`屬性。

```swift
actor Profile {
    let birthDay: String

}


extension Profile {
    nonisolated func familyNameLength() ->  {

        let dateFormatter = DateFormatter()
        dateFormatter.dateFormat = "yyyy-MM-dd"
        guard let startDate = dateFormatter.date(from: birthDay) else {
            fatalError("Failed to convert start date string to date.")
        }

        let endDate = Date()
        let calendar = Calendar.current
        let components = calendar.dateComponents([.day], from: startDate, to: endDate)

        if let daysDifference = components.day {
            print("1998/06/30 到今天相隔 \(daysDifference) 天。")
        } else {
            print("無法計算日期差距。")
        }
    }
}
```