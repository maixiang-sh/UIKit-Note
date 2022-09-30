``` swift
import UIKit
import PlaygroundSupport

class MyViewController : UIViewController {
    override func loadView() {
        let view = UIView()
        view.backgroundColor = .white

        let cardView = UIView()
        cardView.frame = CGRect(x: 20, y: 20, width: 300, height: 250)
        cardView.backgroundColor = .blue
        cardView.layer.cornerRadius = 40
        
        /*  cornerCurve 有两种系统提供的值：
            1.continuous  连续的，过度会自然一些，显得比较圆润
            2.circular  圆形的，圆角和直边衔接处看上去会有一些棱角，显的比较方正
        */
        cardView.layer.cornerCurve = .continuous
        
        
        /// 阴影颜色
        cardView.layer.shadowColor = UIColor.black.cgColor
        /// 阴影不透明度
        cardView.layer.shadowOpacity = 0.5
        /// 阴影偏移
        cardView.layer.shadowOffset = CGSize(width: 0, height: 5)
        /// 阴影半径
        cardView.layer.shadowRadius = 10
        
        view.addSubview(cardView)
    
        self.view = view
    }
}

// Present the view controller in the Live View window
PlaygroundPage.current.liveView = MyViewController()

```

<table>
    <tr>
        <th>
            .continuous
        </th>
        <th>
            .circular
        </th>
    </tr>
    <tr>
        <td>
            <img width="300" alt="continuous" src="https://user-images.githubusercontent.com/47806196/193244052-b2cd97e0-b0b3-4d9f-b28e-2cb6ca1436b9.png">
        </td>
            <td><img width="300" alt="circular" src="https://user-images.githubusercontent.com/47806196/193244243-344d2aff-ea97-4c4f-aa84-cfb59dc92cbb.png">
        </td>
    </tr>
</table>
