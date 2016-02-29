
---
layout: post
title: "KeyRemap4MacBook"
date: 2013-08-13 19:21
comments: true
external-url: 
categories: productivity
---


[KeyRemap4MacBook](https://pqrs.org/macosx/KeyRemap4MacBook/) 是个可以改变Mac按键映射的工具，用到最多的就是[将Caps Lock键改成Hyper键](http://brettterpstra.com/2012/12/08/a-useful-caps-lock-key)。也强烈建议开发人员把 `Caps Lock` 改成 `Hyper`（Shift + Control + Command + Option），这样你就可以有一整套一般不会被程序占用的快捷键可供使用了。

今天看了下他的具体语法，发现还可以有别的玩法：

## 一键切换到指定的输入法

切换中英文输入法我一直觉得是浪费时间的一件事情，每次都要先试错法判断下当前处于什么输入法。特别是如果你有时候还会用到中文输入法的英文输入的话，这时候你要切换到中文的话恐怕得将视线移到标题栏里确认当前是处于英文输入法还是处于中文输入法的英文输入，然后才好切换到中文输入（当然避免这种情况最好的办法还是把中文输入法的英文输入禁掉）。

用KeyRemap4MacBook，你可以为你的每个输入法都设置一个快捷键。这里我用`Cmd + Space`来切换到英文输入法，用`Shift + Cmd + Space`来切换到中文输入法。我的中文输入法是百度拼音，用别的输入法的同学可以用KeyRemap4MacBook的Event Viewer来查看自己所用的输入法的input source id,然后将下面的`inputsourceid_equal`值换成对应的 source id 就可以了。

``` 
	<item>
	    <name>fast input source switch</name>
	    <appendix>Change input source to US by command + space</appendix>
	    <appendix>Change input source to Baidu by command + alt + space</appendix>
	    <identifier>luosky.change_input_source_to_us</identifier>
	    <vkchangeinputsourcedef>
	    	<name>KeyCode::VK_CHANGE_INPUTSOURCE_BAIDU</name>
	    	<inputsourceid_equal>com.baidu.inputmethod.BaiduIM.pinyin</inputsourceid_equal>
 		</vkchangeinputsourcedef>
	    <autogen>
			__KeyToKey__
			KeyCode::SPACE, ModifierFlag::COMMAND_L | ModifierFlag::NONE,
			KeyCode::VK_CHANGE_INPUTSOURCE_US
	    </autogen>
	    <autogen>
			__KeyToKey__
			KeyCode::SPACE, ModifierFlag::COMMAND_L | ModifierFlag::OPTION_L | ModifierFlag::NONE,
			KeyCode::VK_CHANGE_INPUTSOURCE_BAIDU
	    </autogen>
	</item>
```


## 单独设置 HHKB 以贴合普通键盘的习惯


最近入了神器键盘 [HHKB](http://en.wikipedia.org/wiki/Happy_Hacking_Keyboard)，由于它天生就把`Caps Lock`去掉了，重度依赖的`Hyper`键一下子没了很不习惯，于是我做了以下修改，以便在保留  HHKB 键位优点的基础上，尽可能的贴合普通键盘的习惯(这些设置都只在 HHKB 上才生效)：

* 长按`Tab` => `Hyper`
* 右边的`Option` => `Hyper`，搭配之前设置的`Hyper` + `J/K/H/L` 输出 `下/上/左/右`，这样只要用手掌按住右`Option`就可以直接单手用 `J/K/H/L`这些 vi 键来做方向键了。
* 左边的`Shift` + `Delete` => `Forward Delete`


``` 
	<item> 
		<name>HHKB adopt Hyper settings</name>
		<identifier>luosky.hhkb_hyper</identifier>
		<appendix>(Option_R / TAB to Hyper (ctrl+shift+cmd+opt), press only once, send escape)</appendix>

		<devicevendordef>
			<vendorname>TOPRE</vendorname>
			<vendorid>0x0853</vendorid>
		</devicevendordef>

		<deviceproductdef>
			<productname>HHKB_PRO2_TYPES</productname>
			<productid>0x0100</productid>
		</deviceproductdef>
		<device_only>DeviceVendor::TOPRE, DeviceProduct::HHKB_PRO2_TYPES</device_only>
		<autogen>
			--KeyOverlaidModifier--
			KeyCode::TAB,
			KeyCode::COMMAND_L,
			ModifierFlag::OPTION_L | ModifierFlag::SHIFT_L | ModifierFlag::CONTROL_L,
			KeyCode::TAB
		</autogen>
		<autogen>
			--KeyOverlaidModifier--
			KeyCode::OPTION_R,
			KeyCode::COMMAND_L,
			ModifierFlag::OPTION_L | ModifierFlag::SHIFT_L | ModifierFlag::CONTROL_L,
			KeyCode::ESCAPE
		</autogen>
		<autogen>
			--KeyToKey--
			KeyCode::DELETE, ModifierFlag::SHIFT_L | ModifierFlag::NONE,
			KeyCode::FORWARD_DELETE
		</autogen>
	</item>
```

update: HHKB 的 Fn 键 不能被 KeyRemap4MacBook 捕获，所以不支持 Fn 相关的修改。只有 Fn 键在 EventView 里能看到的键盘，才可以修改。


## 一键启动/切换程序，在该程序里按的话则隐藏该程序的窗口

经常有些应用，比如 QQ 或者邮件，你只需要很快的瞄一眼， 就可以切换回原来正在进行的工作中。用 `Command + Tab`切换的话得一个个切换过去实在太慢。如果可以为这个应用指定一个快捷键，按一下就切换过去，再按一下，切换回之前的应用，那可以节约不少时间。

这个用 KeyRemap4MacBook 也能做到，实现的思路是当当前应用不是 QQ 时，这个快捷键打开 QQ，当当前应用是 QQ 时，这个快捷键隐藏当前应用的所有窗口。


```	

  <vkopenurldef>
    <name>KeyCode::VK_OPEN_URL_APP_QQ</name>
    <url>file:///Applications/QQ.app</url>
  </vkopenurldef>
  <appdef>
    <appname>APP_QQ</appname>
    <equal>com.tencent.qq</equal>
  </appdef>

  <item>
    <name>Change hyper + q to open QQ</name>
    <identifier>luosky.hyper_q_qq</identifier>
    <not>APP_QQ</not>
    <autogen>
      __KeyToKey__
      KeyCode::Q, ModifierFlag::OPTION_L | ModifierFlag::SHIFT_L | ModifierFlag::CONTROL_L | ModifierFlag::COMMAND_L,
      KeyCode::VK_OPEN_URL_APP_QQ
    </autogen>
  </item>
  <item>
    <name>Change hyper + q to hide QQ in QQ</name>
    <identifier>luosky.hyper_q_hide_qq</identifier>
    <only>APP_QQ</only>
    <autogen>
      __KeyToKey__
      KeyCode::Q, ModifierFlag::OPTION_L | ModifierFlag::SHIFT_L | ModifierFlag::CONTROL_L | ModifierFlag::COMMAND_L,
      KeyCode::H, ModifierFlag::COMMAND_L
    </autogen>
  </item>

```
**updated**:后来想想，这个功能用 [betterTouchTool](http://www.boastr.de/) 来实现更方便些，这里就当留个备份吧
##其他

除了上面几个以外我还用到了下面这些：


* `Hyper` + `Space` 输出 `Enter`。 适合需要按 `Enter`而右手在鼠标上时用，最常见的场景就是跳出对话框让你选确定还是取消，而你想直接按`Enter`键选择确定的时候。
* `Shift_L/R ` + `Space` 输出 `( ` / `) `
* 自带的
	* ~~~单击 `Option_L` 进入浏览模式~~~
	* vi mode中的`Command` +`h/j/k/l`和`Fn`+`h/j/k/l` 输出 上下左右 ，和`Command` + `f/b`输出 `PageDown`/`PageUp`。
	* `Command_L` + `Command_R` 进入 complete vi mode
	

完整的 [private.xml ](https://www.dropbox.com/s/i3fy89qdt4txeo9/private.xml)


