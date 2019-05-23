# YHDragContainer
参考了git上其他关于滑牌的开源库，比如`CCDraggableCard`。
`CCDraggableCard`只是对卡片的宽做了缩放，没有对高做缩放，另外，其还可以滑动和点击没有显示在最顶部的卡片，而且其没有提供当前滑动索引的方法，并且其属性在框架内部写死了，不能灵活配置。之前在使用的时候，由于这些原因导致我改动了大量的源代码，因此本人决定自己写一个扩展性好，可以灵活配置各种属性的滑牌库。

## 效果预览
<img src="YHDragContainer/GIF/test.gif" width="350">

## 安装

### 手动
Clone代码，把`DragCard`文件夹拖入项目，#import "YHDragCardContainer.h"，就可以使用了

### CocoaPods
```
pod 'YHDragContainer'
```
如果提示未找到，先执行`pod repo update`，再执行`pod install`

### 使用

1、初始化
```
self.dragContainer = [[YHDragCardContainer alloc] initWithFrame:CGRectMake(30, 100, [UIScreen mainScreen].bounds.size.width - 30.0 * 2, 400) config:[[YHDragCardConfig alloc] init]];
self.dragContainer.dataSource = self; // 设置数据源
self.dragContainer.delegate = self; // 设置代理
[self.view addSubview:self.dragContainer];
```

2、刷新<br>
初始化完成之后，请根据自己的实际项目需要，在合适的时机执行刷新
```
[self.dragContainer reloadData];
```

3、实现数据源协议（必须实现）
```
- (int)numberOfCardWithCardContainer:(YHDragCardContainer *)cardContainer{
    // 滑牌总数量
}
- (UIView *)cardContainer:(YHDragCardContainer *)cardContainer viewForIndex:(int)index{
    // 每个索引所对应的View
}
```

4、实现代理协议（非必须）
```
- (void)cardContainer:(YHDragCardContainer *)cardContainer didScrollToIndex:(int)index{
    // 当前滑动到某一个索引的回调
    // 在这里你可以实现分页，显示当前滑动到了第几个
}

- (void)cardContainerDidFinishDragLastCard:(YHDragCardContainer *)cardContainer{
    // 最后一个卡片滑完了的回调
    // 在这儿你可以做诸如请求下一页数据等操作
}

- (void)cardContainer:(YHDragCardContainer *)cardContainer dragDirection:(YHDragCardDirection)dragDirection widthRatio:(CGFloat)widthRatio heightRatio:(CGFloat)heightRatio{
	// 滑动过程中的回调
	// 提供了横向和竖向的滑动比例
	// widthRatio:  >0 右滑      <0 左滑
	// heightRatio: >0 下滑      <0 上滑
	// 在这里你可以实现一些动画效果，比如摊摊，在滑动过程中，按钮会随着滑动放大或者缩小
	// 注意：框架内部已实现了动画效果，因此在此处无需再实现动画效果
}

- (void)cardContainer:(YHDragCardContainer *)cardContainer didSelectedIndex:(int)index{
    // 点击某个卡片的回调
}

```

### 补充
该仓库会不断进行优化，在使用过程中，有任何建议或问题，欢迎提issue，或者通过邮箱1035841713@qq.com联系我
喜欢就star❤️一下吧