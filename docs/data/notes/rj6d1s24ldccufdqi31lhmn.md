
## QRCode 掃條碼

### 產生QRCode

透過CoreImage的`CIFilter`物件來處理輸入的image data

(CIImage本身不是可以display的view物件，需要先轉成UIImage)

```swift
import CoreImage.CIFilterBuiltins

func generateQRCode(from string: String) -> UIImage {
  let context = CIContext()
  let filter = CIFilter.qrCodeGenerator()

  filter.message = Data(string.utf8)

  if let outputImage = filter.outputImage {
    if let cgImage = context.createCGImage(outputImage, from: outputImage.extent) {
      return UIImage(cgImage: cgImage)
    }
  }

  return UIImage(systemName: "xmark.circle") ?? UIImage()
}
```