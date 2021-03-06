# IMUIMessageCollectionView
[中文文档](https://github.com/jpush/imui/blob/dev/docs/iOS/usage_iOS.md)

IMUIMessageCollectionView is a message list in chatting interface, use to display all kinds of messages, and it can be fully customize. If you don't define your style, IMUIMessageCollectionView will use default style.

## Install
#### [CocoaPods](https://cocoapods.org/)  (recommended)

````ruby
# For latest release in cocoapods
pod ‘AuroraIMUI'
````



#### Manual

Copy `IMUI` folder to your project. That's it.

- **Note:** Make sure that `Info.plist` include  camera , Microphone, Photo Library permission.

## Usage
To use IMUIMessageCollectionView only need three simple steps, or you can check out our [sample project](https://github.com/jpush/imui/tree/master/iOS/IMUIChat) to try it yourself.
- **Setp one:** drag a view to your UIViewController (storyboard or xib), and adjust class to `IMUIMessageCollectionView`.

- **Setp two:** implement `IMUIMessageMessageCollectionViewDelegate`.

```
  @IBOutlet weak var messageCollectionView: IMUIMessageCollectionView!
  
  override func viewDidLoad() {
    super.viewDidLoad()
    self.messageCollectionView.delegate = self
  }

// MARK - IMUIMessageMessageCollectionViewDelegate 
  func messageCollectionView(_: UICollectionView, forItemAt: IndexPath, model: IMUIMessageModelProtocol) {}
  
  
  func messageCollectionView(didTapMessageBubbleInCell: UICollectionViewCell, model: IMUIMessageModelProtocol) {}
  
  func messageCollectionView(didTapHeaderImageInCell: UICollectionViewCell, model: IMUIMessageModelProtocol)
  
  func messageCollectionView(_: UICollectionView, willDisplayMessageCell: UICollectionViewCell, forItemAt: IndexPath, model: IMUIMessageModelProtocol) {}
  
  func messageCollectionView(_: UICollectionView, didEndDisplaying: UICollectionViewCell, forItemAt: IndexPath, model: IMUIMessageModelProtocol) {}
  
  func messageCollectionView(_ willBeginDragging: UICollectionView){}
```
- **Setp three:** construct your model.

**1.** To be add messages, you need to implement `IMUIMessageModelProtocol` protocol into your existing mode.
```
  protocol IMUIMessageModelProtocol {
  @request
  var msgId: String { get }
  var fromUser: IMUIUserProtocol { get }
  var layout: IMUIMessageCellLayoutProtocal { get }
  // 消息为发出的消息
  var isOutGoing: Bool { get }
  
  @optional
  
  // return time lable string
  var timeString: String { get }
  
  // return text message's string
  func text() -> String

  // return media(image, voice, video ) file path
  func mediaFilePath() -> String

  // return duration of audio or video
  var duration: CGFloat { get }

  // the bubble background image
  // @warning the image must be resizable just like that: bubbleImg.resizableImage(withCapInsets: UIEdgeInsetsMake(24, 15, 9, 10), resizingMode: .tile)
  var resizeBubbleImage: UIImage { get }
```

**2.** Construct your user model , to implement `IMUIUserProtocol` protocal.

```
public protocol IMUIUserProtocol {
@request
  func userId() -> String 
  func displayName() -> String
  func Avatar() -> UIImage
}
```

## Data management
#### add new message
To add new message in message list is pretty easy, we support some way to add message to `IMUIMessageCollectionView`.
- append message to bottom:
```
messageCollectionView.appendMessage(with message: IMUIMessageModel)
```

- insert message cell to top:
```
messageCollectionViewinsertMessage(with message: IMUIMessageModel)
```
- insert messages cell to top:
```
  insertMessages(with messages:[IMUIMessageModel])
```

## Custom  Layout
Create `MessageModel` object need to specify layout infomation, if not, will use default layout `IMUIMessageCellLayout`. Base on default layout, here offers simple configuration to adjust elements in `MessageCell`:

```
//  head image size
IMUIMessageCellLayout.avatarSize 

//  head image offset to  message cell
IMUIMessageCellLayout.avatarOffsetToCell

//   name label frame
IMUIMessageCellLayout.nameLabelFrame

// message bubble offset to head image
IMUIMessageCellLayout.bubbleOffsetToAvatar

// message cell width
IMUIMessageCellLayout.cellWidth

// message cell content inset
IMUIMessageCellLayout.cellContentInset
```

If you simply adjust the default layout of the configuration items can not meet their own layout needs.  Just construct you layout object. Inherit from `IMUIMessageCellLayout ` and override `IMUIMessageCellLayoutProtocal` function.

```
class MyMessageCellLayout: IMUIMessageCellLayout {
  
  override init(isOutGoingMessage: Bool, isNeedShowTime: Bool, bubbleContentSize: CGSize) {
    
    super.init(isOutGoingMessage: isOutGoingMessage, isNeedShowTime: isNeedShowTime, bubbleContentSize: bubbleContentSize)
  }
  
  override var bubbleContentInset: UIEdgeInsets {
    
    if isOutGoingMessage {
      return UIEdgeInsets(top: 10, left: 10, bottom: 10, right: 15)
    } else {
      return UIEdgeInsets(top: 10, left: 15, bottom: 10, right: 10)
    }
  }
  
}
```

## Future plan
- update message cell status
- custom message 
- message animation
- React Native support

