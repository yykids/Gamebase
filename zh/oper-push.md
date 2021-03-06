## Game > Gamebase > 操控台使用指南> Push


您可以向应用用户发送推送消息。
Gamebase使用TOAST Push功能发送推送消息。

## Push

> <font color="red">[重要]</font><br/>
> 
> 正在服务的游戏应用**Gamebase SDK 2.6.0以上**版本时使用的菜单。

可确认发送推送的历史和创建的推送预约列表。

![gamebase_push_01_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_01_201910.png)

### Registered List

在预约列表中选择推送，可确认推送的预计发送时间及创建信息，单击**取消传输**按钮，可取消预约发送件，或单击**复制**按钮，可利用创建的推送的创建信息新建推送。

### Send History

在发送历史列表中选择推送，可查询传输的推送的详细明细。
单击**复制**按钮，可利用发送的推送创建信息轻松创建推送。

![gamebase_push_02_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_02_201910.png)
![gamebase_push_03_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_03_201910.png)

### Register Push

若欲创建新推送，单击**注册**按钮。
可以通过右侧的预览UI确认控制台中注册的值在实际终端机中如何显示。

![gamebase_push_04_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_04_201910.png)
![gamebase_push_05_201910](https://static.toastoven.net/prod_gamebase/gamebase_push_05_201910.png)

#### (1) 发送类型

选择发送周期。
- **立即发送**：注册时立即发送推送信息。
- **预约发送**：在预约的时间发送推送信息。欲以用户的终端机国家时间为准发送推送信息时，选择**以当地时间为准发送**。若选择**以当地时间为准发送**并设置为“2017-01-01 09:00”，使用韩国终端机的用户在韩国时间“2017-01-01 09:00”、使用英国终端机的用户在英国时间“2017-01-01 09:00”接收推送信息。
- **反复发送**：定期发送相同的推送。欲以用户的终端机国家时间为准发送推送信息时，选择**以当地时间为准发送**。选择欲发送的期间后，选择相应期间内想要发送的时间。然后选择的发送期间内发送的周期以**每天**、**每周**、**每月**为准输入信息。
	- 每天：每天发送推送。
	- 每周：选择欲发送的周几，仅在特定周几发送推送。周几可重复选择。
	- 每月：以一个月为准，在需要的日期发送推送。仅可输入日期(1 ~ 31)，可输入一个或一个以上日期。例如，若欲在1日、5日、10日发送，输入1、5、10，则在相应日期发送推送。

#### (2) 发送对象
选择要发送推送信息的对象。
- **全部发送**：可按照操作系统选择。使用所选操作系统的用户都将收到推送。
- **仅向特定会员发送**：欲仅向特定会员发送推送信息时选择。向以输入的用户ID注册推送令牌的终端机发送推送信息。对象用户利用5个终端机进行游戏时，向5个终端机发送推送。
- **组合发送**：将欲发送信息的用户列表注册为文件。组合发送1次最多可发送10,000人。
- **标签发送**：可利用标签发送推送。可利用在**推送 - 标签**菜单中注册的标签发送，还可注册多个标签。标签过滤器支持如下功能。
	- AND：仅向满足所有选中标签的用户发送推送。
	- OR：向即使只满足选中标签中一个条件的用户发送推送。

#### (3) 对象国家

选择要发送推送信息的国家。
- **全部国家**：向所有用户发送推送信息。
- **部分国家**：仅向所选国家的用户发送推送信息。
  输入欲添加的国家名，自动完成输入。若无欲输入的国家，请联系[客服中心](https://toast.com/support/inquiry)。

> [参考]
> 
> 国家判断标准
> 以用户的**USIM国家代码**为准进行判断，无USIM时，以为终端机设置的国家为准显示推送信息。

#### (4) 提示音（可选项目）

可设置终端机接收推送时播放的提示音。
单击**添加提示音**按钮，可设置提示音，若不设置，则播放默认提示音。
输入外部URL链接或应用程序中内置的提示音文件路径。


#### (5) 信息类型

> [参考]
> 
> 为遵守韩国的“信息通信网法”而提供的功能。
> 发送推送时，若不是信息性信息，应以“（广告）”开始信息，信息中应含有联系方式及取消接收方法。

- **宣传性**：在输入的信息中添加“（广告）”序言。并且，添加输入的联系方式及取消接收方法一同发送。对于全部发送等广告性推送，在韩国国内，为遵守信息通信网法，必须作为“宣传性”信息发送。
- **信息性**：仅输入的信息通过推送发送。对于非韩国终端机（USIM国家代码不是韩国）的用户，作为信息性信息发送即可。


> <font color="red">[重要]</font><br/>
> 
> 选择**宣传性**后在输入信息中输入“（广告）”语句时，“（广告）”语句会重复，因此请注意。 

#### (6) 发送信息

输入向用户显示的推送信息。

信息可利用多种语言输入，对于使用输入的语种以外语种的用户，以选择为默认语种的语种显示。若欲添加语种，单击下端的**添加信息**按钮。若欲添加列表中没有的语种，请联系[客服中心](https://toast.com/support/inquiry)。

> [参考]
> 
> 发送了推送信息，但终端机未收到信息时
> 大部分情况下是未注册用户的推送令牌。请确认是否已注册用户的推送令牌。 
> 在平台上注册推送令牌的方法请参考以下文件。
>
> - [Android > Register Push](./aos-push/#2-register-push) 
> - [iOS > Register Push](./ios-push/#2-register-push) 
> - [Unity > Register Push](./unity-push/#2-register-push) 

#### (7) 信息文字颜色(Only Android)

对于Android，可指定终端机中显示的推送信息的文字颜色。
可分别指定标题和内容的颜色，可在文字颜色选择窗口中选择颜色或直接输入RGB Hex值指定颜色。
选择的文字颜色可通过右侧的预览界面确认。

#### (8) 大图标(Only Android)

单击**添加大图标**按钮，可设置接收推送时右侧显示的图标的图像。
设置大图标图像并发送推送时，仅Android终端机显示图像，iOS中不显示。
选择外部，输入要显示的图标图像的外部URL，或选择内部，输入应用程序中内置的图标图像文件路径。
为外部图像时，可在右侧预览中提前确认相应图标。

#### (9) 按钮

接收推送时的信息下方显示的按钮最多可设置3个，一同发送。
有4种形态的按钮，各类型运行方式及设置的值不同，因此请参考如下指南设置。
- 打开应用程序：在推送信息中单击按钮，即可运行相应应用程序。仅可指定按钮名。
- 打开URL：在推送信息中单击按钮，即可移动至在控制台中输入的URL。可通过应用程序架构运行特定应用程序或连接网页。
- 响应：在推送信息中单击按钮，即可运行能够响应的窗口。对于iOS，可一同设置传送按钮名。
- 关闭，在推送信息中单击按钮，即可关闭推送。仅可指定按钮名。

#### (10) 媒体(iOS)

可添加接收iOS推送时动态执行的媒体。
可设置**IMAGE**、**GIF**、**VIDEO**、**AUDIO**项目，可输入欲连接的外部URL或内部文件路径。

#### (11) 媒体(Android)

可添加接收Android推送时执行的媒体。
目前Android中仅可设置**IMAGE**项目，可输入欲连接的外部URL或内部文件路径。
## Push(Old)
您可以通过Gamebase确认已发送的推送记录和预约发送列表。
![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_01_201812.png)
推送计划明细里的列表可以在**Push Details**中取消发送。

### Registered List

如果从推送计划列表中选择推送，则可以查看推送的预计发送时间和创建的信息。
目前，只能直接取消预约，预计将会提供编辑已创建信息的功能。

### Send History

您可以通过“推送记录”列表选择“推送”来查看已发推送的详细信息。
通过单击**复制**按钮，可以利用已发推送的信息轻松创建推送。
![gamebase_push_02_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_02_201812.png)

阿
### Register Push

要创建新的Push，请单击**创建**按钮。

![gamebase_push_03_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_03_201812.png)

#### (1) 消息类型
> [参考]
> 提供此功能是为了遵守韩国的“信息和通信网络法案”。
> 如果推送的不是信息类的消息，则消息必须以“（广告）”标题 ，并且消息必须包含联系方式以及取消订阅方法。

- **宣传性**：输入的消息将自动添加标题为“（广告）”。并将发送联系方式和取消订阅的方法。在韩国国内，全体发送广告类推送时，必须遵守通信网络法，将其作为“宣传性”消息来发送。
- **信息性**：只推送输入的信息。 使用非韩国手机的用户（USIM国家代码不是韩语），将发送信息性消息。

> <font color="red">[重要]</font>
>  选择**宣传性**后，在输入消息中输入“（广告）”字样，则会重复短语“（广告）”，敬请注意。

#### (2) 发送对象
选择发送Push信息的对象

- - **全体发送**：您可以通过操作系统进行选择。 使用所选操作系统的所有用户都将收到推送。
- **仅发送给特定用户**：仅在将推送消息发送给特定用户时进行选择，推送消息将发送到，以输入的用户ID登记推送令牌的设备。
- **群组发送**：发送消息的用户列表，可以使用包含用户列表的文件。群组发送一次最多可支持10,000人。

#### (3) 发送类型
选择发送周期。

- **立即发送**：创建后立即发出推送消息。
- **预约发送**：在预定时间发出推送消息。如想根据设备的用户所在国家时间发送推送消息，请选择**按当地时间发送**。例如: 将时间设定为'2017-01-01 09:00',使用韩国手机的用户在韩国时间'2017-01-01 09:00'收到推送消息，使用英国手机的用户在英国时间'2017-01-01 09:00'收到推送消息。

预计将会追加重复发送(每日，每周，每月)的功能。

#### (4)选择国家
选择要发出推送信息的国家。

- **所有国家**：向所有用户发出推送信息。
- **部分国家**：仅向所选国家的用户发出推送消息
  输入您要添加的国家名，它将自动完成。如果没有所需国家，请您可以通过 [用户中心](https://toast.com/support/inquiry)与我们联系

> [参考]
> 国家标准
>以用户的 **USIM国家代码** 判断，如果没有USIM，将根据 **Device**中设置的地区显示推送消息。

#### (5) 推送消息
输入要显示给用户的推送消息。
输入的消息可以使用多种语言，对于使用您输入语言以外的其他语言的用户，将选择默认语言作为显示语言. 单击右侧的 **+** 按钮可以添加语言。如果没有所需语言，您可以通过[用户中心](https://toast.com/support/inquiry)与我们联系，添加新语言。

> [参考]
> 发出了推送消息，但设备上没有收到信息时
> 在大多数情况下，是因为未登记用户的推送令牌。 请确认是否已登记用户的推送令牌。
> 平台上登记推送令牌方法，请参考以下文件。
>
> - [Android > Register Push](./aos-push/#2-register-push)
> - [iOS > Register Push](./ios-push/#2-register-push)
> - [Unity > Register Push](./unity-push/#2-register-push)



## Tag

提供可按照特定标准绑定用户进行传输的标签功能。

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_06_201910.png)

可注册TOAST Push发送推送时使用的标签名。



### Tag register

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_09_201910.png)



### Tag detail

可管理注册的标签及注册于相应标签的用户列表。

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_07_201911.png)

通过上方的删除/修改按钮，可修改并删除标签相关信息，通过下方的用户ID管理功能，可在标签注册或删除用户。

#### 注册用户

##### 单件注册

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_08_201910.png)

##### 文件注册

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_10_201910.png)

按注册按钮，弹出如上注册弹出窗口，可直接输入ID或通过文件注册输入。

欲利用文件注册进行注册时，一次最多可注册1000人。



#### 删除用户

![gamebase_push_01_201812](https://static.toastoven.net/prod_gamebase/gamebase_push_11_201910.png)

欲删除时，在用户列表中选择左侧的复选框后按删除按钮，可删除注册于标签的用户。