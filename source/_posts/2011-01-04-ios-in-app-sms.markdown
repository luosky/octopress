---
layout: post
title: "iOS app内发送短信"
date: 2011-01-04 00:36
comments: true
external-url: 
categories: [programming, iOS]

---

iOS4.0新加入了`MFMessageComposeViewController`和`MFMessageComposeViewControllerDelegate`,提供了发送短信的接口,可以像发送邮件那样不用跳出程序来发送短信. 介绍可参阅Message UI Framework Reference

一些笔记:

###    MFMessageComposeViewController

  * 提供了操作界面
  * 使用前必须检查canSendText方法,若返回NO则不应将这个controller展现出来,而应该提示用户不支持发送短信功能.
  * 界面不能自行定制
  * 要发送的短信的内容(body)和收件人(recipients)在展现这个controller前需初始化好,展现了之后短信内容不能通过程序来进行修改.不过用户仍然可以手工修改短信内容和选择收件人
  * 用户点了发送或者取消,或者发送失败时,MFMessageComposeViewControllerDelegate 的- messageComposeViewController:didFinishWithResult:方法都能得到通知,在这里进行相应的处理

若在iOS3.0上运行的话,会提示`dyld: Symbol not found: _OBJC_CLASS_$_MFMessageComposeViewController` 解决方案:


  1. MessageUI.framework的引入类型应选择weak(在target -> Get Info -> General -> Linked Libraries -> MessageUI.framework -> Type 里修改)
  2. 不要在.h文件里直接import MessageUI/MFMessageComposeViewController.h,改为import <MessageUI/MessageUI.h>

具体的发送代码:

``` objective-c 
    
    #pragma mark -
    #pragma mark SMS
    
    -(IBAction)showSMSPicker:(id)sender {
    	//	The MFMessageComposeViewController class is only available in iPhone OS 4.0 or later.
    	//	So, we must verify the existence of the above class and log an error message for devices
    	//		running earlier versions of the iPhone OS. Set feedbackMsg if device doesn't support
    	//		MFMessageComposeViewController API.
    	Class messageClass = (NSClassFromString(@"MFMessageComposeViewController"));
    
    	if (messageClass != nil) {
    		// Check whether the current device is configured for sending SMS messages
    		if ([messageClass canSendText]) {
    			[self displaySMSComposerSheet];
    		}
    		else {
    			[UIAlertView quickAlertWithTitle:@"设备没有短信功能" messageTitle:nil dismissTitle:@"关闭"];
    		}
    	}
    	else {
    		[UIAlertView quickAlertWithTitle:@"iOS版本过低,iOS4.0以上才支持程序内发送短信" messageTitle:nil dismissTitle:@"关闭"];
    	}
    }
    
    -(void)displaySMSComposerSheet
    {
    	MFMessageComposeViewController *picker = [[MFMessageComposeViewController alloc] init];
    	picker.messageComposeDelegate = self;
    
    	NSMutableString* absUrl = [[NSMutableString alloc] initWithString:web.request.URL.absoluteString];
    	[absUrl replaceOccurrencesOfString:@"http://i.aizheke.com" withString:@"http://m.aizheke.com" options:NSCaseInsensitiveSearch range:NSMakeRange(0, [absUrl length])];
    
    	picker.body=[NSString stringWithFormat:@"我在爱折客上看到：%@ 可能对你有用，推荐给你！link：%@"
    										,[web stringByEvaluatingJavaScriptFromString:@"document.title"]
    										,absUrl];
    	[absUrl release];
    	[self presentModalViewController:picker animated:YES];
    	[picker release];
    }
    
    - (void)messageComposeViewController:(MFMessageComposeViewController *)controller
    				 didFinishWithResult:(MessageComposeResult)result {
    
    	switch (result)
    	{
    		case MessageComposeResultCancelled:
    			LOG_EXPR(@"Result: SMS sending canceled");
    			break;
    		case MessageComposeResultSent:
    			LOG_EXPR(@"Result: SMS sent");
    			break;
    		case MessageComposeResultFailed:
    			[UIAlertView quickAlertWithTitle:@"短信发送失败" messageTitle:nil dismissTitle:@"关闭"];
    			break;
    		default:
    			LOG_EXPR(@"Result: SMS not sent");
    			break;
    	}
    	[self dismissModalViewControllerAnimated:YES];
    }
```    
