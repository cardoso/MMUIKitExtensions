# MMUIKitExtensions
>Useful UIKit extensions

## How to Use
>This is intended to be a collection of snippets rather than a monolithic package, so just copy and paste the extension you need in a file somewhere in your project

## UILabel with Ghost Animation

### Preview

![UILabelGhost](https://github.com/matheusmcardoso/MMUIKitExtensions/raw/master/media/label-00-ghost.gif)


### Usage

```swift
    @IBAction func pressedUp(_ sender: UIButton) {
        currentValue += 100
        numberLabel.doGhostAnimation(text: "+100", color: UIColor.green, tX: numberLabel.center.x/3, tY: -50, sX: 0.5, sY: 0.5)
    }

    @IBAction func pressedDown(_ sender: UIButton) {
        currentValue -= 100
        numberLabel.doGhostAnimation(text: "-100", color: UIColor.red, tX: numberLabel.center.x/3, tY: 100, sX: 0.5, sY: 0.5)
    }
```

### Extension

```swift
// UILabel+Ghost.swift
// UILabel with Ghost Animation
// https://github.com/matheusmcardoso/MMUIKitExtensions

import UIKit

extension UILabel {

    func doGhostAnimation(text: String? = nil, color: UIColor = UIColor.green,
                          tX: CGFloat = 0, tY: CGFloat = -50, sX: CGFloat = 0.5, sY: CGFloat = 0.5,
                          duration: TimeInterval = 0.5, completion: ((Bool) -> Void)? = nil) {

        let oldSize = self.frame.size
        var size = self.frame.size

        let oldColor = self.textColor
        self.textColor = color

        let oldText = self.text
        if let text = text {
            self.text = text
            size = self.text!.size(attributes: [NSFontAttributeName: self.font!])
            self.frame.size = size
        }
        self.superview!.layoutIfNeeded()

        guard let view = self.snapshotView(afterScreenUpdates: true) else { return }

        self.textColor = oldColor
        self.text = oldText
        self.frame.size = oldSize

        self.addSubview(view)

        UIView.animate(withDuration: duration, animations: {
            view.alpha = 0
            view.frame = view.frame.applying(CGAffineTransform(translationX: tX, y: tY)).applying(CGAffineTransform(scaleX: sX, y: sY))
        }, completion: {
            completed in
            view.removeFromSuperview()
            completion?(completed)
        })
    }
}

```
