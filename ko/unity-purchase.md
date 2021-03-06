## Game > Gamebase > Unity SDK 사용 가이드 > 결제

여기에서는 Unity에서 인앱 결제 기능을 사용하기 위해 필요한 설정 방법을 알아보겠습니다.
Gamebase는 하나의 통합된 결제 API를 제공해 게임에서 손쉽게 많은 스토어의 인앱 결제를 연동할 수 있도록 돕습니다.

### Settings

Android나 iOS에서 인앱 결제 기능을 설정하는 방법은 다음 문서를 참고하시기 바랍니다.<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

Unity Standalone에서 결제를 진행하기 위해 IapAdapter와 WebViewAdapter를 반드시 추가하여야 합니다.
![GamebaseUnitySDKSettins Inspector](http://static.toastoven.net/prod_gamebase/UnityDevelopersGuide/unity-developers-guide-settingtool_iap_2.4.0.png)


###  Purchase Flow

아이템 구매는 다음과 같은 순서로 구현하시기 바랍니다.<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_2.6.2.png)


1. 게임 클라이언트에서는 Gamebase SDK의 **RequestPurchase**를 호출하여 결제를 시도합니다.
2. 결제가 성공하였다면 **RequestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인합니다.
3. 반환된 미소비 결제 내역 목록에 값이 있으면 게임 클라이언트가 게임 서버에 결제 아이템에 대한 consume(소비)을 요청합니다.
	* UserID, itemSeq, paymentSeq, purchaseToken 을 전달합니다.
4. 게임 서버는 게임 DB 에 이미 동일한 paymentSeq, purchaseToken 으로 아이템을 지급한 이력이 있는지 확인합니다.
	* 4-1. 아직 아이템을 지급하지 않았다면 UserID 에 itemSeq 에 해당하는 아이템을 지급합니다.
    * 4-2. 아이템 지급 후 게임 DB 에 UserID, itemSeq, paymentSeq, purchaseToken 을 저장하여 이후에 중복 지급을 확인할 수 있도록 합니다.
5. 게임 서버는 Gamebase 서버에 API를 통해 consume(소비) API를 요청합니다.
	* [API 가이드](./api-guide/#consume)

<br/>

* 스토어 결제는 성공했으나 오류가 발생하여 정상 종료되지 못하는 경우가 있습니다. 로그인 완료 후 미소비 결제 내역을 확인하시기 바랍니다.
	* 로그인에 성공하면 **RequestItemListOfNotConsumed**를 호출하여 미소비 결제 내역을 확인합니다.
	* 반환된 미소비 결제 내역 목록에 값이 존재한다면 게임 클라이언트가 게임 서버에 consume(소비)를 요청하여 아이템을 지급합니다.

### Purchase Item

구매하고자 하는 아이템의 itemSeq를 이용해 다음의 API를 호출하여 구매를 요청합니다.
게임 유저가 구매를 취소하는 경우 **PURCHASE_USER_CANCELED** 오류가 반환됩니다.


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

아이템 목록을 조회하려면 다음 API를 호출합니다. 
콜백으로 반환되는 목록 안에는 각 아이템들에 대한 정보가 담겨 있습니다.

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

아이템을 구매했지만, 정상적으로 아이템이 소비(배송, 지급)되지 않은 미소비 결제 내역을 요청합니다.
미결제 내역이 있는 경우에는 게임 서버(아이템 서버)에 요청하여, 아이템을 배송(지급)하도록 처리해야 합니다.

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

현재 사용자 ID 기준으로 활성화된 구독 목록을 조회합니다.
결제가 완료된 구독 상품(자동 갱신형 구독, 자동 갱신형 소비성 구독 상품)은 만료되기 전까지 계속 조회할 수 있습니다.
사용자 ID가 같다면 Android와 iOS에서 구매한 구독 상품이 모두 조회됩니다.

> <font color="red">[주의]</font><br/>
>
> 현재 구독 상품은 Android의 경우 Google Play 스토어만 지원합니다.

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

App Store 앱 내에서 아이템을 구매할 수 있는 기능을 제공합니다.
아이템 구매 성공 후, 아래의 등록해놓은 핸들러를 통하여, 아이템지급을 진행할 수 있습니다.

프로모션 IAP는 App Store Connect 에서 별도의 설정이 되어야 노출이 가능합니다.

> <font color="red">[주의]</font><br/>
>
> iOS 11 이상에서만 사용할 수 있습니다.
> Xcode 9.0 이상에서 빌드가 필요합니다.
> Gamebase 1.13.0 이상에서 지원합니다. (TOAST IAP SDK 1.6.0 이상적용)


> <font color="red">[주의]</font><br/>
>
> 로그인 성공 이후에만 호출 할 수 있습니다.
> 로그인 성공 후, 다른 결제 API보다 먼저 실행되어야 합니다.

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

> `주의`
> App Store Connect에 앱을 업로드한 다음 TestFlight를 통하여 앱을 설치 후, 테스트를 진행할 수 있습니다.
> 

1. TestFlight로 App을 설치합니다.
2. 아래와 같은 URL Scheme을 호출하여, 테스트를 진행합니다.

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | 고정값 |
| host &amp; path | 없음 | 없음 |
| queries | action | purchaseIntent |
|		  | bundleId | 앱의 bundeld identifier |
|		  | productIdentifier | 구매 아이템의 product identifier |

예제) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`

### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED                 | 4001       | Purchase 모듈이 초기화되지 않았습니다.<br>gamebase-adapter-purchase-IAP 모듈을 프로젝트에 추가했는지 확인해 주세요. |
| PURCHASE_USER_CANCELED                   | 4002       | 게임 유저가 아이템 구매를 취소하였습니다.                  |
| PURCHASE_NOT_FINISHED_PREVIOUS_PURCHASING | 4003 | 구매 로직이 아직 완료되지 않은 상태에서 API가 호출되었습니다. |
| PURCHASE_NOT_ENOUGH_CASH                 | 4004       | 해당 스토어의 캐시가 부족해 결제할 수 없습니다.              |
| PURCHASE_NOT_SUPPORTED_MARKET            | 4010       | 지원하지 않는 스토어입니다.<br>선택 가능한 스토어는 GG(Google), ONESTORE 입니다. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR          | 4201       | IAP 라이브러리 오류입니다.<br>DetailCode를 확인하세요.   |
| PURCHASE_UNKNOWN_ERROR                   | 4999       | 정의되지 않은 구매 오류입니다.<br>전체 로그를 [고객 센터](https://toast.com/support/inquiry)에 올려 주시면 가능한 한 빠르게 답변 드리겠습니다. |

* 전체 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [오류 코드](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* 이 오류는 IAP 모듈에서 발생한 오류입니다.
* 오류 코드를 확인하는 방법은 다음과 같습니다.

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

* IAP 오류 코드는 다음 문서를 참고하시기 바랍니다.
    * [TOAST > TOAST SDK 사용 가이드 > TOAST IAP > Unity > 오류 코드](/TOAST/ko/toast-sdk/iap-unity/#_17)





