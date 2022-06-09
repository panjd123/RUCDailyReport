# RUCDailyReport

为了帮助广大经常忘填疫情防控通的RUCer，selenium 实现的 RUC 疫情防控通电脑端自动登记脚本出现在了 github 上（雾

## 所用库

1. `selenium`
2. `win10toast`
3. `pandas`

## 常规使用指南

1. 下载 Edge 驱动，并将对应的 exe 文件添加到系统变量 `path` 中，详细步骤可百度 "selenium Edge"
2. 在 `setting.json` 中输入账号密码信息用于登录
3. 运行 `main.pyw`
4. 建议把 `main.pyw` 设为开机自启，并在测试可以使用后在 `setting.json` 中将 `silent` 设为 `true`
5. 提示：如果出现不明的 `cmd` 黑框，可以百度 "selenium 黑框"，它是 `selenium` 的控制台，可以通过改包关掉

登记失败会报错提醒，可能的原因有：账号密码错误，昨天忘记登记，导致部分信息缺失；网速太慢；网站改版

## setting.json 设置说明
`student_ID`: 微人大学号

`password`: 微人大密码

`silent`: 是否隐藏浏览器窗口，bool
> 最新版的实现方法是把窗口移动到 -2000 的位置达到隐藏界面

`alert`: 通知量，bool
> false 表示低通知，即只在失败时通知；true 表示高通知，即登记失败，今日已登记，本次登记成功都通知

`alert_time`: 边栏通知停留在屏幕上的时间，单位：秒

`timeout`: 判断超时的时间，单位：秒。
> 由于网络必然有延迟，所以需要经常轮询一些元素显示出来了没有，但是有可能因为填报错误，或者断网，元素永远都不可能出来，所以轮询需要一个超时时间。当你认为你的网络速度高时，这个可以调低，以更快地显示填报失败，如果你发现误判了填报失败，那可以把这个值调高。

`url`: 可以在微信打开防控通后，选择左上角在浏览器中打开获取。

`delay_time`: 打开脚本后延迟一段时间再执行，单位：秒
> 使用场景如：校园网连接需要手动操作一下，或者连接需要时间，脚本直接运行就会因没网报错。

## 注意

1. 脚本没有模拟定位的功能，只是代替点击而已。

2. 翻墙可能导致定位偏移，但如果定位和上次不同，防控通会报错，所以不用担心会上传错误的数据，只会填报失败。

3. 用 Edge 是因为经过测试，只有 Edge 可以在不翻墙的前提下正常获取到定位信息。

   > 理论上更好的实现方案在我的环境下失败了，如果你想尝试，这里提供改动代码的说明
   > 
   > `Registrant` 的 构造函数中有个参数 `driver_kind='Edge'`，可以换成 Firefo` 和 Chrome
   > 
   > 此时 `silent` 是实现方式是无头浏览器，即 `self.options.headless = True`。
   > 
   > 但是我的实测表明 Firefox 和 Chrome 没法正常定位。
   > 
   > 另外你也可以尝试给 Edge 加上无头标识，或者最小化来达到隐藏，但是我发现这都会导致无法正常获取位置。
   > 
   > 只有正常显示时才能正常运行。所以出此移动 `-2000` 的下策。

4. 为了解决有人昨天没填今天再填的时候部分信息要重填，现在的版本会直接无脑地勾选大众选项。代码就是 `# Information` 下的，不需要可以删除。

5. `logging.log` 是日志文件，事实上的格式为csv，列标签为日期，时间，运行信息，退出代码 (0表示成功，1表示失败)。运行时会先通过日志文件判断今天是不是已经填报过了。
