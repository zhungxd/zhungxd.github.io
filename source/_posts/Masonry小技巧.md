title: Masonry小技巧
date: 2016-05-19 17:12:03
tags: ios
---

当你写了一个错误的constanints，例如:

```
[self.statusLabel mas_makeConstraints:^(MASConstraintMaker *make) {
make.top.equalTo(self.contentView).offset(15.0f);
make.left.equalTo(self.contentView).offset(10.f);
make.height.equalTo(10);
make.bottom.equalTo(self.contentView);
}];
```

然后在你会看到如下的debug log:
```
Probably at least one of the constraints in the following list is one you don't want. Try this: 
(1) look at each constraint and try to figure out which you don't expect; 
(2) find the code that added the unwanted constraint or constraints and fix it. 
(Note: If you're seeing NSAutoresizingMaskLayoutConstraints that you don't understand, 
refer to the documentation for the UIView property translatesAutoresizingMaskIntoConstraints)
(
"<MASLayoutConstraint:0x1704a9b40 UILabel:0x12fde1c10.top == UITableViewCellContentView:0x12fdde840.top + 15>",
"<MASLayoutConstraint:0x1704a9c00 UILabel:0x12fde1c10.height == 10>",
"<MASLayoutConstraint:0x1704a9c60 UILabel:0x12fde1c10.bottom == UITableViewCellContentView:0x12fdde840.bottom>",
"<NSLayoutConstraint:0x17429fae0 UITableViewCellContentView:0x12fdde840.height == 177.5>"
)
Will attempt to recover by breaking constraint
<MASLayoutConstraint:0x1704a9c00 UILabel:0x12fde1c10.height == 10>
Make a symbolic breakpoint at UIViewAlertForUnsatisfiableConstraints to catch this in the debugger.
The methods in the UIConstraintBasedLayoutDebugging category on UIView listed in <UIKit/UIView.h> may also be helpful.
```

是不是会眼花，如果有很多label你是不是需要找半天。
现在你只需要加上一句      
```
self.statusLabel = [UILabel new];
[self.contentView addSubview:self.statusLabel];
MASAttachKeys(self.statusLabel);
```
只要加一句，不需要￥998

然后log会变成这样
```
(
"<MASLayoutConstraint:0x1704a4980 UILabel:self.statusLabel.top == UITableViewCellContentView:0x147f046f0.top + 15>",
"<MASLayoutConstraint:0x1704a4a40 UILabel:self.statusLabel.height == 10>",
"<MASLayoutConstraint:0x1704a4aa0 UILabel:self.statusLabel.bottom == UITableViewCellContentView:0x147f046f0.bottom>",
"<NSLayoutConstraint:0x170496080 UITableViewCellContentView:0x147f046f0.height == 177.5>"
)
Will attempt to recover by breaking constraint
<MASLayoutConstraint:0x1704a4a40 UILabel:self.statusLabel.height == 10>
```
是不是很清楚，一看就知道错在哪里。

同理，如果你没有用Masonry，而只是用系统的autolayout API，可以试试NSLayoutConstraint的identifier来让你的debug log更易读。