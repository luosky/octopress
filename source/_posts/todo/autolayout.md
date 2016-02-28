
* AutoLayout的语法不是很友好，可以考虑用 [KeepLayout](https://github.com/iMartinKiss/KeepLayout) 或者 [Masonry](https://github.com/cloudkite/Masonry) 来简化。KeepLayout 的语法看着比较舒服点，不过Masonry 用的人更多也相对比较活跃的样子。

* 用NSDictionaryOfVariableBindings时，里面的view 不能直接用 self.priceLabel 这种，比如

```
[NSLayoutConstraint constraintsWithVisualFormat:@"|-10-[foodNameLabel][priceLabel]"
                                             options:Nil
                                             metrics:Nil
                                               views:NSDictionaryOfVariableBindings(self.foodNameLabel,self.priceLabel)]
```
就会直接崩溃。可用_priceLabel 来代替。

* 添加 constraint 的几点心得：
	* constraint 在 view 的awakeFromNib 写，不要在 IB 里设置，以免随便移动就被改了。
	* 只针对会有变化的 view 写 constraint
	* constraint 涉及到的 view 记得设置 translatesAutoresizingMaskIntoConstraints = NO
	* 界面在 nib 里初始化设置，nib 里将 Use AutoLayout 取消勾选
      