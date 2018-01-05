---
title: 'Autolayout下设置UITableView的tableHeaderView'
date: 2015-11-05 22:40:42
tags: 
- Autolayout
---

这两天在做项目的过程中，设置 `tableView的tableHeaderView` 之后，总是出现各种各样的问题。

比如：

* iOS7闪退，iOS8和9显示正常。
* iOS7显示正常，iOS8和9居然有约束冲突的提示。（对于有强迫症我来说绝对不可忍受）
* iOS7显示直接错乱掉，iOS8和9显示不正常。（高度不对）

## 猜测
由于采用自动布局自定义了一个 `fundHeaderView`，但是对于`tablevHeaderView`是需要设置`frame`的，如果直接使用 `self.tableView.tableHeaderView = self.funHeaderView` 会出现各种各样的错乱。（个人猜测此处属于系统bug)

不多说了，看下面代码

```objc
- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view.

    UIView *headerView = [UIView new];
    headerView.frame = self.fundHeaderView.bounds;
    [headerView addSubview:self.fundHeaderView];
    self.fundTableView.tableHeaderView = headerView;
}
- (THFundHeaderView *)fundHeaderView{
    if (!_fundHeaderView) {
        _fundHeaderView = [[[NSBundle mainBundle] loadNibNamed:@"THFundHeaderView"
                                                        owner:self
                                                      options:nil] lastObject];
        _fundHeaderView.frame = CGRectMake(0, 0, UIScreenWidth, 161.f);
    }
    return _fundHeaderView;
}
```