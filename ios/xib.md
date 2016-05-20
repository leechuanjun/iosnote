# Xib的加载
- 方法1

```objc
NSArray *views = [[NSBundle mainBundle] loadNibNamed:@"xib文件名" owner:nil options:nil]
```

- 方法2

```objc
UINib *nib = [UINib nibWithNibName:@"xib文件名" bundle:nil];
NSArray *views = [nib instantiateWithOwner:nil options:nil];
```
