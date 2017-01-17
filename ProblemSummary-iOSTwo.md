## 设置一个单个角的弧
```OC
UIBezierPath *maskPath = [UIBezierPath
    bezierPathWithRoundedRect:self.backgroundImageView.bounds byRoundingCorners:(UIRectCornerBottomLeft | UIRectCornerBottomRight)
    cornerRadii:CGSizeMake(20, 20)
];
CAShapeLayer *maskLayer = [CAShapeLayer layer];
maskLayer.frame = self.bounds;
maskLayer.path = maskPath.CGPath;
self.backgroundImageView.layer.mask = maskLayer;
```
