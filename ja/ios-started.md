## Game > Gamebase > iOS SDK ご利用ガイド > はじめる

### Environments


> [INFO]
>
> 要件：iOS9以上、または一部IDPサポート時、iOS10以上<br/>
> iOS、iOS Simulator
> Xcode10以上でビルド可能
>


### Installation

Gamebaseは、次のような方法で設定できます。

#### Download

* [Download Gamebase iOS SDK](/Download/#game-gamebase)

Gamebase.framework.zip及び必要なadapterをダウンロードします。<br/>
また、各IdPの認証をするためのSDKファイルをダウンロードする必要があります。該当するIdPのログインを使用するときにだけ含めれば問題ありません。<br/>
ダウンロードした後、該当するSDKファイルをプロジェクトのtargetに含めなければなりません。

**3rd Party SDK Download**

| Gamebase | Gamebase.framework, Gamebase.bundle | ToastSDK 0.19.3 | GamebaseのInterfaceおよび核心ロジックを含む | Gamebase内に含まれる | iOS9 or later
| Gamebase Auth Adapters | GamebaseAuthFacebookAdapter.framework | FacebookSDK v5.6.0 | Facebookログインをサポート | [LINK \[Go to Download\]](https://developers.facebook.com/docs/ios/downloads) | iOS9 or later |
|  | GamebaseAuthPaycoAdapter.framework | PaycoID Login 3rd SDK v1.3.2 | Paycoログインをサポート | [LINK \[Go to Download\]](https://developers.payco.com/guide/sdk/download) | iOS9 or later |
|  | GamebaseAuthNaverAdapter.framework | naveridlogin-sdk-ios-4.0.10 | Naverログインをサポート | [LINK \[Go to Download\]](https://developers.naver.com/docs/login/sdks/) | iOS9 or later |
|  | GamebaseAuthGamecenterAdapter.framework | GameKit.framework | Gamecenterログインをサポート |  | iOS9 or later |
|  | GamebaseAuthGoogleAdapter.framework | | Googleログインをサポート | | iOS9 or later |
|  | GamebaseAuthTwitterAdapter.framework | | Twitterログインをサポート | | iOS9 or later |
|  | GamebaseAuthLineAdapter.framework | LineSDK v5.0.1 | LINEログインをサポート | [LINK \[Go to Download\]](https://github.com/line/line-sdk-starter-ios-v2) | iOS10 or later |
|  | GamebaseAuthAppleidAdapter.framework |  | Sign In with Apple | AuthenticationServices.frameworkをOptionalに設定 | iOS13 or later |
| Gamebase IAP | GamebasePurchaseIAPAdapter.framework | StoreKit.framework, ToastIAP 0.19.8, ToastGamebaseIAP 0.9.7 | ゲーム内決済をサポート | Gamebase IAP内に含まれる | iOS9 or later |
| Gamebase Push | GamebasePushAdapter.framework | ToastPush 0.19.3 | Pushをサポート | Gamebase Push内に含まれる | iOS9 or later |


> <font color="red">[注意]</font><br/>
>
> Sign In with Appleに必要なAuthenticationServices.frameworkを追加する場合は、必ずOptionalに設定する必要があります。
> Requiredに設定した場合は、iOS 11以下の端末では実行直後にクラッシュが発生します。
> 
<br/>


> <font color="red">[注意]</font><br/>
>
> Gamebase Frameworkのファイルのうち、名前に**Adapter**が含まれるファイルはプロジェクト内で使用するかどうかを任意で決定することができ、該当Adapter Frameworkを使用するには上記の表に記載された外部SDKが必要な場合があります。
> 一部認証Adpaterの場合は、上の表にあるSupport iOS Versionに注意してください。
> (サポートバージョンがiOS10以上のAuth Adpaterをビルドに含めた時、iOS9以下ではruntime Crashが発生します)

<br/>


> [INFO]
> 
> 各IdPが提供する外部SDKに対する設定は、各IdPのガイドドキュメントをご参考ください。
>

#### Xcode Settings

解凍すると、次のようにGamebase.frameworkなどのSDKを確認することができます。

![unzip gamebase](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-002_1.0.0.png)


* 1) FrameworkファイルをProjectのProject Navigatorにドラッグ・アンド・ドロップでimportします。このときに追加されたFrameworkファイルは、プロジェクトtargetに追加されなければなりません。
* 2) **Gamebase.bundle**ファイルも**Copy Bundle Resources**に追加します。
![Gamebase.bundle Bundle Resources](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-003_1.0.0.png)
* 3) Gamebaseを使用するためには、Gamebaseのframework以外にGamebaseで使用している外部SDKの機能を含めるため、複数のframeworkとlibraryファイルをlinkerで参照できるように追加しなければなりません。次の項目を追加してください。
    * libicucore.tbd (Gamebase SDK v1.1.5以上で追加)
    * libz.tbd
    * libsqlite3.tbd
    * libc++.tbd
    * AdSupport.framework
    * ImageIO.framework
    * GameKit.framework
    * StoreKit.framework
    * AuthenicationServices.framework (Optional)
![Link Binary With Libraries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-005_1.0.0.png)
* 4) **Target > Build Settings > Linking > Other Linker Flags**に**-ObjC**を追加する必要があります。
![Other Linker Flags](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-006_1.0.0.png)
* 5) **Target > Build Settings > Enable Bitcode**を**No**に設定します。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)
* 6) NaverAuthAdapterを使用する場合は、NaverSDKで提供する**NaverThirdPartyLogin.framework**ファイルを**Target > General > Embedded Binaries**に追加する必要があります。
 ![Naver Embeded Binaries](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-started-001_1.7.0.png)

> [INFO]
>
> Linkerに**-ObjC**オプション設定は、Static LibraryにあるすべてのObjective-C classとcategoryを読み込みます。<br/>
> このオプションを設定しない場合、**selector not recognized**のようなエラーがRuntime上で発生することがあります。
>

#### CocoaPods Settings

Gamebase iOS SDKは、CocoaPodsを使用して設定できます。

1. Xcodeを実行し、プロジェクトを作成します。
2. Terminalを実行し、CocoaPodsを適用するプロジェクトのディレクトリに移動します。
3. **pod init**コマンドを実行し、**Podfile**を作成します。
4. 作成された**Podfile**をエディタで開き、次のような内容を作成します。

```ruby
platform :ios, '10.0'

target 'SampleApplication' do
    pod 'Gamebase'
    pod 'GamebaseAuthFacebookAdapter'
    pod 'GamebaseAuthGamecenterAdapter'
    pod 'GamebaseAuthPaycoAdapter'
    pod 'GamebaseAuthNaverAdapter'
    pod 'GamebaseAuthTwitterAdapter'
    pod 'GamebaseAuthGoogleAdapter'
    pod 'GamebaseAuthLineAdapter'
    pod 'GamebaseAuthAppleidAdapter'
    pod 'GamebasePushAdapter'
    pod 'GamebasePurchaseIAPAdapter'

end
```

> [参考]
>
> **target 'SampleApplication' do**部分には作成したプロジェクトのターゲット名を入力します。<br/>
> **pod 'Gamebase', '2.6.0'**のように入力して、特定バージョンを指定できます。それぞれのpodにバージョンを明示しない場合は最新バージョンが設定されます。<br/>
> 特定Adapterのみを任意で適用できます。
>



> <font color="red">[注意]</font><br/>
>
> Gamebase最新バージョンを使用しない場合、一部のAdapterを使用できないことがあります。
>

5. Podfileの作成が完了したら、**pod install**または**pod update**コマンドを実行してGamebaseをインストールします。
6. インストールが完了したら、**プロジェクト名.xcworkspace**ファイルが作成されます。以降は作成された**xcworkspace**ファイルを使用して開発を行います。
7. **Target > Build Settings > Enable Bitcode**を**No**に設定します。
![Enable Bitcode](http://static.toastoven.net/prod_gamebase/iOSDevelopersGuide/ios-developers-guide-installation-007_1.0.0.png)

> [参考]
>
> 詳細なCocoaPods使用方法は、[CocoaPods Guide](https://guides.cocoapods.org/)の[Using CocoaPods](https://guides.cocoapods.org/using/index.html)ページを参照してください。
>
>

## 3rd-Party Provider SDK Guide

* [Facebook for developers](https://developers.facebook.com/docs/ios)
* [Naver for developers](https://developers.naver.com/docs/login/ios/)
* [Twitter Developer's guide - Log in with Twitter](https://dev.twitter.com/web/sign-in/implementing)
* [Twitter Developer's guide - Authentication](https://developer.twitter.com/en/docs/basics/authentication/overview/oauth)
* [Line for developers](https://developers.line.me/en/docs/line-login/ios/integrate-line-login/)
* [PaycoID SDK for developers](https://developers.payco.com/guide/development/apply/ios)

## API Reference

SDKの中に含まれています。

## API Deprecate Governance

GamebaseでサポートしないAPIは、使用していないもの(deprecate)として処理します。
使用していない(deprecated) APIは、次の条件を満たす場合、事前告知を行わずに削除されることがあります。

* 5回以上のマイナーバージョンアップデート
	* Gamebase Version Format - XX.YY.ZZ
		* XX：Major
		* YY：Minor
		* ZZ：Hotfix

* 最低5ヶ月経過
