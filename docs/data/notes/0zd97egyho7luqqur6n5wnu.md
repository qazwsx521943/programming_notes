
## SPM can't find module

```git
rm -rf ~/Library/Developer/Xcode/DerivedData
```

## Fastest Way removing Derived Data

```terminal
rm -rf ~/Library/Developer/Xcode/DerivedData
```

## Multiple command produce

https://stackoverflow.com/questions/50718018/xcode-10-error-multiple-commands-produce

## Check Thread issue

![](/assets/images/project%20brainstorming.flavor%20flash%20journal.xcode%20issues_check_thread.png)

## XCode project file merge 'group'

要注意合併時，檔案夾之間是否有 source group結尾包住，否則會有啟動不了的問題

```
E7FFB38B2B1CA2B90065A213 /* FoodPrintHistoryViewModel.swift */ = {isa = PBXFileReference; lastKnownFileType = sourcecode.swift; path = FoodPrintHistoryViewModel.swift; sourceTree = "<group>"; };
```

## 如何查看模擬器的UserDefaults plist?

https://pradnya-nikam.medium.com/view-and-change-user-defaults-on-ios-simulator-d2c3fc3b82b1

1. 先找到模擬器被存在哪：
    ```
    /Users/zhongzhexuan/Library/Developer/CoreSimulator/Devices/BE1B25CF-FBF9-4850-BBCC-58807BA3D19B
    ```
2. 進到模擬器的資料夾：
    ```
    find . -name ios22-jason.SwiftJunior.plist
    ```
3. 如果有UserDefaults檔案，就會搜尋的到
    ```
    ./data/Containers/Data/Application/521CEC9B-FB38-4A6B-8329-02CC51188A20/Library/Preferences/ios22-jason.SwiftJunior.plist
    ```