## Game > Gamebase > Unity SDK ご利用ガイド > 決済

ここではUnityでアプリ内決済機能を使用するために必要な設定方法についてご案内いたします。
Gamebaseは、一つの統合された決済APIを提供することで、ゲームで簡単に各ストアのアプリ内決済を連携することができるようサポートします。

### Settings

AndroidやiOSでアプリ内決済機能を設定する方法は、次のドキュメントをご参考ください。<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

Unity Standaloneで決済を行うには、IapAdapterとWebViewAdapterを追加する必要があります。
![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-settingtool_iap_2.4.0.png)


###  Purchase Flow

アイテムの購入は次のような手順で設計してください。<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)

1. ゲームクライアントでは、Gamebase SDKの**RequestPurchase**を呼び出して決済を試みます。
2. 決済が成功した場合、**RequestItemListOfNotConsumed**を呼び出して未消費決済の内訳を確認します。
3. 返された未消費決済内訳リストに値がある場合、ゲームクライアントがゲームサーバーに決済アイテムに対するconsume(消費)をリクエストします。
4. ゲームサーバーは、GamebaseのサーバーにAPI経由でconsume(消費)APIをリクエストします。[APIガイド](/Game/Gamebase/ja/api-guide/#wrapping-api)
5. IAPサーバーからconsume(消費)APIの呼び出しに成功すると、ゲームサーバーがゲームクライアントにアイテムを配布します。

* ストア決済は成功しましたが、エラーが発生し正常に終了できない場合があります。ログイン完了後、未消費決済履歴を確認してください。<br/>
	* ログインに成功すると、**RequestItemListOfNotConsumed**を呼び出して未消費決済履歴を確認します。
	* 返された未消費決済履歴リストに値が存在する場合は、ゲームクライアントがゲームサーバーにconsume(消費)をリクエストしてアイテムを支給します。

### Purchase Item

購入したいアイテムのitemSeqを利用して次のAPIを呼び出し、購入をリクエストします。
ゲームユーザーが購入をキャンセルする場合、**PURCHASE_USER_CANCELED**エラーが返されます。


**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestPurchase(long itemSeq, GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**
```cs
public void RequestPurchase(long itemSeq)
{
    Gamebase.Purchase.RequestPurchase(itemSeq, (purchasableReceipt, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
        	if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}
```

### Get a List of Purchasable Items

アイテムリストを照会したい場合、次のAPIを呼び出します。
コールバックで返されるリストの中にはそれぞれ各アイテムの情報が含まれています。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListPurchasable(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableItem>> callback)
```

**Example**
```cs
public void RequestItemListPurchasable()
{
    Gamebase.Purchase.RequestItemListPurchasable((purchasableItemList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```



### Get a List of Non-Consumed Items

アイテムを購入したものの、正常にアイテムが消費(送信、配布)されていない未消費決済の内訳をリクエストします。
未決済の内訳がある場合は、ゲームサーバー(アイテムサーバー)にリクエストを出してアイテムを送信(配布)するように処理する必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestItemListOfNotConsumed(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestItemListOfNotConsumed()
{
    Gamebase.Purchase.RequestItemListOfNotConsumed((purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Get list succeeded.");

            // Should Deal With This non-consumed Items.
            // Send this item list to the game(item) server for consuming item.
        }
        else
        {
            Debug.Log(string.Format("Get list failed. error is {0}", error));
        }
    });
}
```

### Get the List of Actived Subscriptions

現在のユーザーIDで有効になっている定期購入リストを照会します。
決済が完了した定期購入商品(自動更新型定期購入、自動更新型消費性定期購入商品)は、期間が終了するまで照会できます。 
ユーザーIDが同じならAndroidとiOSで購入した定期購入商品が全て照会されます。

> <font color="red">[注意]</font><br/>
>
> 現在、定期購入商品は、Androidの場合Google Playストアのみサポートします。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestActivatedPurchases(GamebaseCallback.GamebaseDelegate<List<GamebaseResponse.Purchase.PurchasableReceipt>> callback)
```

**Example**
```cs
public void RequestActivatedPurchasesSample()
{
    Gamebase.Purchase.RequestActivatedPurchases((purchasableReceiptList, error) =>
    {
        if (Gamebase.IsSuccess(error) == true)
        {
            Debug.Log("RequestItemListPurchasable succeeded");

            foreach (GamebaseResponse.Purchase.PurchasableReceipt purchasableReceipt in purchasableReceiptList)
            {
                var message = new StringBuilder();
                message.AppendLine(string.Format("itemSeq:{0}", purchasableReceipt.itemSeq));
                message.AppendLine(string.Format("price:{0}", purchasableReceipt.price));
                message.AppendLine(string.Format("currency:{0}", purchasableReceipt.currency));
                
                // You will need paymentSeq and purchaseToken when calling the Consume API.
                // Refer to the following document for the Consume API.
                // https://docs.toast.com/en/Game/Gamebase/en/api-guide/#purchaseiap
                message.AppendLine(string.Format("paymentSeq:{0}", purchasableReceipt.paymentSeq));
                message.AppendLine(string.Format("purchaseToken:{0}", purchasableReceipt.purchaseToken));
                message.AppendLine(string.Format("marketItemId:{0}", purchasableReceipt.marketItemId));
                Debug.Log(message);
            }
        }
        else
        {
            // Check the error code and handle the error appropriately.
            Debug.Log(string.Format("RequestItemListPurchasable failed. error is {0}", error));
        }
    });
}
```

### App Store Promotion IAP

App Storeアプリでアイテムを購入できる機能を提供します。
アイテム購入成功後、登録しておいた下記のハンドラを利用してアイテムを支給できます。

プロモーションIAPは、App Store Connectで別途設定すると表示できます。

> <font color="red">[注意]</font><br/>
>
> iOS 11以上でのみ使用できます。
> Xcode 9.0以上でビルドする必要があります。
> Gamebase 1.13.0以上でサポートします(TOAST IAP SDK 1.6.0以上適用)。


> <font color="red">[注意]</font><br/>
>
> ログイン成功後にのみ呼び出すことができます。
> ログイン成功後、他の決済APIより先に実行する必要があります。

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS

```cs
static void SetPromotionIAPHandler(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableReceipt> callback)
```

**Example**
```cs
public void SetPromotionIAPHandler()
{
    Gamebase.Purchase.SetPromotionIAPHandler((purchasableReceipt, error) => 
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("Purchase succeeded.");
        }
        else
        {
            if (error.code == (int)GamebaseErrorCode.PURCHASE_USER_CANCELED)
            {
                Debug.Log("User canceled purchase.");
            }
            else
            {
            	Debug.Log(string.Format("Purchase failed. error is {0}", error));
            }
        }
    });
}
```
**Overview**

* Apple Developer Overview : https://developer.apple.com/app-store/promoting-in-app-purchases/
* Apple Developer Reference : https://help.apple.com/app-store-connect/#/deve3105860f

**How to Test AppStore Promotion IAP**

> `注意`
> App Store Connectにアプリをアップロードし、TestFlightでアプリをインストールした後、テストできます。
> 

1. TestFlightでアプリをインストールします。
2. 下記のようなURLスキーム(scheme)を呼び出し、テストを進行します。

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 固定値 |
| host &amp; path | なし | なし |
| queries | action | purchaseIntent |
|		  | bundleId | アプリのbundeld identifier |
|		  | productIdentifier | 購入アイテムのproduct identifier |

例) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchaseモジュールが初期化されていません。<br>gamebase-adapter-purchase-IAPモジュールをプロジェクトに追加したか確認してください。|
| PURCHASE_USER_CANCELED                   | 4002       | ゲームユーザーがアイテムの購入をキャンセルしました。                 |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 購入ロジックが完了していない状態でAPIが呼び出されました。|
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 該当するストアのcashが足りないため決済することができません。            |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | このストアには対応しておりません。<br>選択可能なストアは、GG(Google)、TS(ONE store)、TESTです。|
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAPライブラリーエラーです。<br>DetailCodeを確認してください。  |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 定義されていない購入エラーです。<br>ログ全体を[カスタマーセンター](https://toast.com/support/inquiry)にアップロードしてください。なるべく早くお答えいたします。|

* 全体のエラーコードは、次のドキュメントをご参考ください。
    * [エラーコード](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* このエラーは、IAPモジュールで発生したエラーです。
* エラーコードは次のように確認できます。

```cs
GamebaseError gamebaseError = error; // GamebaseError object via callback

if (Gamebase.IsSuccess(gamebaseError))
{
    // succeeded
}
else
{
    Debug.Log(string.Format("code:{0}, message:{1}", gamebaseError.code, gamebaseError.message));

    GamebaseError moduleError = gamebaseError.error; // GamebaseError.error object from external module
    if (null != moduleError)
    {
        int moduleErrorCode = moduleError.code;
        string moduleErrorMessage = moduleError.message;

        Debug.Log(string.Format("moduleErrorCode:{0}, moduleErrorMessage:{1}", moduleErrorCode, moduleErrorMessage));
    }
}
```

* IAPのエラーコードは、次のドキュメントをご参考ください。
    * [Mobile Service > IAP > エラーコード > Client APIエラータイプ](/Mobile%20Service/IAP/ja/error-code/#client-api)





