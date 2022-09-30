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
            1.continuous  连续的，过度会自然一些
            2.circular  圆形的，圆角和直边衔接处看上去会有一些棱角
        */
        cardView.layer.cornerCurve = .continuous
        
        view.addSubview(cardView)
    
        self.view = view
    }
}

// Present the view controller in the Live View window
PlaygroundPage.current.liveView = MyViewController()

```
