# 分页
- 只要将UIScrollView的pageEnabled属性设置为YES，
UIScrollView会被分割成多个独立页面，
里面的内容就能进行分页展示

- 一般会配合UIPageControl增强分页效果，UIPageControl常用属性如下

```objc
一共有多少页
@property(nonatomic) NSInteger numberOfPages;
//当前显示的页码
@property(nonatomic) NSInteger currentPage;
//只有一页时，是否需要隐藏页码指示器
@property(nonatomic) BOOL hidesForSinglePage;
//其他页码指示器的颜色
@property(nonatomic,retain) UIColor *pageIndicatorTintColor;
//当前页码指示器的颜色
@property(nonatomic,retain) UIColor *currentPageIndicatorTintColor;
```
