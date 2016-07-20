# 父子控制器

```objc
@interface ViewController ()

@property (weak, nonatomic) IBOutlet UIButton *firstButton;

@property (weak, nonatomic) IBOutlet UIButton *secondButton;

@property (weak, nonatomic) IBOutlet UIButton *thirdButton;

@property (weak, nonatomic) IBOutlet UIView *contentView;

//当前显示的子控制器
@property (strong, nonatomic) UIViewController *showingController;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.

//  将子控制器添加到父控制器中
    [self addChildViewController:[[FirstViewController alloc] init]];
    [self addChildViewController:[[SecondViewController alloc] init]];
    [self addChildViewController:[[ThirdViewController alloc] init]];

//    [self click:self.firstButton];
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

#pragma mark - "click"
- (IBAction)click:(UIButton *)button {

    //将当前控制器的View 从父控制器的View中移除，好处是每次父控制中的View只有一个子控制器的View，不用绘制多余的View,节省内存
    [self.showingController.view removeFromSuperview];

    //记录当前标签的index
    NSInteger index = [button.superview.subviews indexOfObject:button];

    //取出需要显示的控制器
    self.showingController =  self.childViewControllers[index];

    //设置控制器的
    self.showingController.view.frame = self.contentView.bounds;

    //把子控制器的View添加到父控制器的View
    [self.contentView addSubview:self.showingController.view];
}

@end
```

####NavigationController父子控制器注意点
```objc
- (void)viewDidLoad {
    [super viewDidLoad];

    GreenViewController *green = [[GreenViewController alloc] init];
    green.view.frame = CGRectMake(50, 64, 100, 100);

    [self.view addSubview:green.view];

    self.green = green;

    [self addChildViewController:self.green];

```
- 注意：当添加一个控制器的view时，如果想实现跳转，那么要成为navigationController的子控制器

- #### 当前控制器已经被添加到某个父控制器上或者从某个父控制器上移除时就会调用这个方法

```objc
- (void)didMoveToParentViewController:(UIViewController *)parent
{
    [super didMoveToParentViewController:parent];

    NSLog(@"didMoveToParentViewController - %@", parent);
}
```

```objc
    //调用这个方法，会调用子控件的didMoveToParentViewController
    [first didMoveToParentViewController:self];

    //调用这个方法把当前控制器从某个父控制器上移除，会调用子控件的didMoveToParentViewController
    [self.childViewControllers[0] removeFromParentViewController];
```

```objc
//当parent 有值时，就是当前控制器已经被添加到某个父控制器
FirstViewController----->didMoveToParentViewController,parent is <ViewController: 0x7fc8d963ab80>
//当parent 为空时，就是当前控制器从某个父控制器上移除
FirstViewController----->didMoveToParentViewController,parent is (null)

```
