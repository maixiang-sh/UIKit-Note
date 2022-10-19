# FileManager
文件系统内容的便捷接口，以及与之交互的主要方式。
```swift
class FileManager : NSObject
```
> `FileManager` 属于 `Foundation` 框架
Swift 中的文件目录使用 `URL` 表示，可以使用 `FileManager` 类实例获取文档目录 `URL`

## FileManager.default
`default` 是 `FileManager` 类的一个类属性（`class var`），类似于静态属性（`static var`），但是 `class var` 允许子类覆盖父类的实现。
这里的 `default` 通过类属性实现了一个 `FileManager` 单例：
```swift
class var `default`: FileManager { get }
```
默认使用 `default` 的原因是：使用多个线程可共享的单例，可以保证线程安全。

### 定位系统目录的方式
```swift
//在域中查找并选择性地创建指定的公共目录。
func url(for: FileManager.SearchPathDirectory, in: FileManager.SearchPathDomainMask, appropriateFor: URL?, create: Bool) -> URL
```

```swift
//返回请求域中指定公共目录的 URL 数组。
func urls(for: FileManager.SearchPathDirectory, in: FileManager.SearchPathDomainMask) -> [URL]

```


### 获取用户文档目录（ Documents ）的方式
```swift
FileManager.default.urls(for: .documentDirectory, in: .userDomainMask)
// 返回一个 URL 数组，由于 Documents 目录仅有一个，所以使用下标[0]拿到 URL
```

**`for directory`** 参数表示 搜索路径目录，是一个枚举类型，具体 case 例如：
>  `applicationDirectory`  应用程序目录（ /Applications）
>  
>  `libraryDirectory` 支持和配置文件目录（ /Library ）
>  
>  `userDirectory`  用户主目录（ /Users）
>  
>   **`documentDirectory`** **文档目录（适合保存App用户数据的目录）**
>   
>  `desktopDirectory` 用户桌面目录
>  
>  `downloadsDirectory` 用户下载目录
>  
>  `moviesDirectory` 用户的电影目录（ /Movies）
>  
>  `musicDirectory` 用户的音乐目录 （ /Music）
>  
>  `picturesDirectory`  用户的图片目录（ /Pictures）

**`in domainMask`** 参数表示 要搜索的文件系统域。是一个结构体类型，有几个静态属性
>  `userDomainMask`  用户的主目录——安装用户个人项目的地方
>  
>  `localDomainMask` 在这台机器上安装可供所有人使用的项目的地方
>  
>  `networkDomainMask` 安装网络上可用项目的位置 （ /Network）
>  
>  `systemDomainMask` Apple 提供的系统文件目录 （ /System）
>  
>  `allDomainsMask` 所有域。

### 重构获取 Documents 目录便于使用
方法
```swift
import Foundation

extension FileManager {
    static var documentDirectoryURL: URL {
        Self.default.urls(for: .documentDirectory, in: .userDomainMask)[0]
    }
}
```
使用
```swift
// 获取 Documents 目录的 URL
FileManager.documentDirectoryURL
// 使用`path()`方法将 URL 转为 String
FileManager.documentDirectoryURL.path()
```

### 在  Documents 目录基础上创建新 URL 的方法（添加子目录）
方法一：
使用 URL 的初始化方法， 在一个 URL 的基础上附加一个 URL
```swift
let reminderDataURL = URL(filePath: "Reminders", relativeTo: FileManager.documentDirectoryURL)
```
方法二：
使用 URL 的 `appending` 和 `appendingPathExtension` 方法，添加子目录和子目录扩展
```swift
let stringURL = FileManager.documentDirectoryURL
	.appending(path: "String")
	.appendingPathExtension("txt")

// stringURL.paht() = "/Users/maixiang/Library/Developer/XCPGDevices/XXXXXXXX-8888-8888-8888-XXXXXXXXXXXX/data/Containers/Data/Application/XXXXXXXX-8888-8888-8888-XXXXXXXXXXXX/Documents/String.txt"
```

> **Challenge**: 构造自定义URL
> ```swift
> var challengeString: String = "学习资料"
> 
> var challengeURL: URL = URL(filePath: challengeString).appendingPathExtension("avi")
> 
> challengeURL.lastPathComponent
> 
> // 使用 `URL` 的 `.lastPathComponent` 方法，可以获取 URL 最后一个路径组件
> // challengeURL.lastPathComponent == "学习资料.avi"
> ```





## 保存和加载数据
示例数据：
```swift
let favoriteBytes: [UInt8] = [
  240,          159,          152,          184,
  240,          159,          152,          185,
  0b1111_0000,  0b1001_1111,  0b1001_1000,  186,
  0xF0,         0x9F,         0x98,         187
]

```

写入方法：
```swift
// 从 favoriteBytes 创建 Data
let favoriteBytesData = Data(favoriteBytes)

// 创建保存数据的目录
let favoriteBytesURl = URL(filePath: "favoriteBytes",relativeTo: FileManager.documentsDirectoryURL)

/// 使用 Data.write(to:) 方法，将数据缓冲区的内容写入某个位置。write 方法可能会抛出错误，所以需要 try 关键词
try favoriteBytesData.write(to: favoriteBytesURl)
```

从 URL 加载数据的方法：

```swift
let favoriteBytesData = Data(favoriteBytes)
let favoriteBytesURl = URL(filePath: "favoriteBytes",relativeTo: FileManager.documentsDirectoryURL)


/// 序列化，将数据缓冲区的内容写入某个位置。
try favoriteBytesData.write(to: favoriteBytesURl)

/// 反序列化，使用 Data 的初始化方法 Data(contentsOf:URL) 从 URL 初始化 Data 实例。也需要 try
let savedfavoriteBytesData = try Data(contentsOf: favoriteBytesURl)
/// 从 Data 转换为 Array
let savedfavoriteBytes = Array<UInt8>(savedfavoriteBytesData)

favoriteBytes == savedfavoriteBytes  // true

```

