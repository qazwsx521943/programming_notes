
In Xcode, we have to kind of automated test we can write.

XCTest Framework is the framework that helps us write **unit** / **ui** test

Test跟Refactor是相輔相成的，
如果可以在Refactor之前把Test Code寫好，那你就可以拿來驗證你Refactor後的Code是否也可以正常運作。

XCode中做單元測試的方法：

Testing Class必須繼承XCTestCase，且這個class中的function都必須以test prefix.

最常使用的斷言物件就是`XCTAssertEqual`去測試結果是否符合預期。