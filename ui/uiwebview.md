## UIWebView

```objc
 - (void)viewDidLoad { 
 [super viewDidLoad];

 [self.webView loadRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:self.url]]];

}

#pragma mark - 监听点击
- (IBAction)reload {
 [self.webView reload];
}

- (IBAction)back {
 [self.webView goBack];
}

- (IBAction)forward {
 [self.webView goForward];
}


#pragma mark - <UIWebViewDelegate>
- (void)webViewDidFinishLoad:(UIWebView *)webView
{
 self.backItem.enabled = webView.canGoBack;
 self.forwardItem.enabled = webView.canGoForward;
}
```



