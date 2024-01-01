---
id: ycav7y1plpd72ulknzdaw6x
title: Alamofire
desc: ''
updated: 1704005644980
created: 1703998468835
---

## What is it built for?

Provide elegant and composable interface to HTTP network requests.

## How it was built

Built on top of Apple's `URL Loading System` provided by `Foundation` framework

Wrapping up `URLSession` and the `URLSessionTask` subclasses.

### Where does `AF` come from?

```swift
// Shared singleton instance used by all `AF.request` APIs. Cannot be modified.
// Session可以建立、管理Request實例。
public let AF = Session.default
```

#### `Session`職責

建立Request

#### `URLConvertible`、`URLRequestConvertible`protocol

```swift
public protocol URLConvertible {
    /// Returns a `URL` from the conforming instance or throws.
    ///
    /// - Returns: The `URL` created from the instance.
    /// - Throws:  Any error thrown while creating the `URL`.
    func asURL() throws -> URL
}

public protocol URLRequestConvertible {
    /// Returns a `URLRequest` or throws if an `Error` was encountered.
    ///
    /// - Returns: A `URLRequest`.
    /// - Throws:  Any error thrown while constructing the `URLRequest`.
    func asURLRequest() throws -> URLRequest
}
```



### `SessionDelegate` 職責

> Class which implements the various `URLSessionDelegate` methods to connect various Alamofire features.

- URLSessionDelegate
- URLSessionTaskDelegate
- URLSessionDataDelegate
- URLSessionWebSocketDelegate
- URLSessionDownloadDelegate

## Usage

### requests

