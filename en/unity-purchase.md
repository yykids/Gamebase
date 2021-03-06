## Game > Gamebase > Unity Developer's Guide > Purchase

This page describes how to set In-App Purchase (IAP).
Gamebase provides an integrated purchase API to easily link IAP of many stores in a game.

### Settings

For Android and iOS IAP setting, refer to the below documents.<br/>

* [Android Purchase Settings](aos-purchase#settings)<br/>
* [iOS Purchase Settings](ios-purchase#settings)

###  Purchase Flow

Item purchases should be implemented in the following order.<br/>

![purchase flow](http://static.toastoven.net/prod_gamebase/DevelopersGuide/purchase_flow_001_1.5.0.png)


1. Call **RequestPurchase** of Gamebase SDK to purchase in a game client.
2. After a successful purchase, call **RequestItemListOfNotConsumed** to check list of non-consumed purchases.
3. If the value is on the returned list, the game client sends a request to the game server to consume purchased items.
4. The game server request for Consume API to the Gamebase server via API. [API Guide](/Game/Gamebase/en/api-guide/#wrapping-api)
5. If the IAP server has successfully called Consume API, the game server provides the items to the game client.

A purchase at store may be successful but cannot be closed normally due to error. It is recommended to call each of the two APIs after login is completed, to initialize a reprocessing logic.<br/>

1. Request list of items that are not consumed
    * When a login is successful, call **RequestItemListOfNotConsumed** to check list of non-consumed purchases.
    * If the value is on the returned list, the game client sends a request to the game server to consume, so that items can be provided.
2. Request to retry transaction
    * When a login is successful, call **RequestRetryTransaction** to try to automatically reprocess the unprocessed.
    * If there is a value on the returned successList, the game client sends a request to the game server to consume, so that items can be provided.
    * If there is a value on the returned failList, send the value to the game server or Log &amp; Crash to collect logs. Also send inquiry to  [**Customer Center**](https://toast.com/support/inquiry) for the cause of reprocessing failure. 

### Purchase Item

Call following API of an item to purchase by using itemSeq to send a purchase request.
When a game user cancels purchasing, the **PURCHASE_USER_CANCELED** error will be returned.


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

To retrieve the list of items, call the following API. 
Information of each item is included in the array of callback return.

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

Request for a list of non-consumed items, which have not been normally consumed (delivered, or provided) after purchase.
In case of non-purchased items, ask the game server (item server) to proceed with item delivery (supply).

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

### Reprocess Failed Purchase Transaction

In case a purchase is not normally completed after a successful purchase at a store due to failure of authentication of TOAST IAP server, try to reprocess by using API.
Based on the latest success of purchase, reprocessing is required by calling an API for item delivery (supply).

**API**

Supported Platforms
<span style="color:#1D76DB; font-size: 10pt">■</span> UNITY_IOS
<span style="color:#0E8A16; font-size: 10pt">■</span> UNITY_ANDROID

```cs
static void RequestRetryTransaction(GamebaseCallback.GamebaseDelegate<GamebaseResponse.Purchase.PurchasableRetryTransactionResult> callback)
```

**Example**
```cs
public void RequestRetryTransaction()
{
    Gamebase.Purchase.RequestRetryTransaction((purchasableRetryTransactionResult, error) =>
    {
        if (Gamebase.IsSuccess(error))
        {
            Debug.Log("RequestRetryTransaction succeeded.");

            // Should Deal With This Retry Transaction Result.
            // You may send result to your gameserver and add item to user.
        }
        else
        {
            Debug.Log(string.Format("RequestRetryTransaction failed. error is {0}", error));
        }
    });
}
```

### AppStore Promotion IAP

It provides the function to purchase items in the App Store apps.
After a successful purchase of items, the items can be provided using the handler registered below.

The promotion IAP can be displayed only after additional settings are made in App Store Connect.

> <font color="red">[Caution]</font><br/>
>
> It is available for iOS version 11 or later.
> It must be built with Xcode 9.0 or later.
> It is supported by Gamebase 1.13.0 or later(It is applicable to TOAST IAP SDK 1.6.0 or later).


> <font color="red">[Caution]</font><br/>
>
> It can be called only after a successful login.
> After a successful login, it must be executed ahead of any other payment APIs.

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

> `Caution`
> You can test your app after uploading it to the App Store Connect and installing the app with TestFlight.
>

1. Install the app with TestFlight.
2. Call the following URL scheme (scheme) to proceed the test.

| URL Components | keyname | value |
| --- | --- | --- |
| scheme | itms-services | Fixed value |
| host &amp; path | None | None |
| queries | action | purchaseIntent |
|		  | bundleId | bundled identifier of the app |
|		  | productIdentifier | product identifier of the purchased item |

e.g.) `itms-services://?action=purchaseIntent&bundleId=com.bundleid.testest&productIdentifier=productid.001`


### Error Handling

| Error                                    | Error Code | Description                              |
| ---------------------------------------- | ---------- | ---------------------------------------- |
| PURCHASE_NOT_INITIALIZED | 4001 | The purchase module is not initialized.<br>Check if the gamebase-adapter-purchase-IAP module has been added to project. |
| PURCHASE_USER_CANCELED | 4002 | Purchase is cancelled. |
| PURCHASE_NOT_FINISHED\_PREVIOUS\_PURCHASING | 4003 | API has been called when a purchase logic is not completed. |
| PURCHASE_NOT_ENOUGH_CASH | 4004 | Cannot purchase due to shortage of cash of the store. |
| PURCHASE_NOT_SUPPORTED_MARKET | 4010 | The store is not supported.<br>You can choose either GG (Google), TS (ONE Store), or TEST. |
| PURCHASE_EXTERNAL_LIBRARY_ERROR | 4201 | Error in IAP library.<br>Check DetailCode. |
| PURCHASE_UNKNOWN_ERROR | 4999 | Unknown error in purchase.<br>Please upload the entire logs to the [Customer Center](https://toast.com/support/inquiry) and we'll respond ASAP. |

* Refer to the following document for the entire error code.
    * [Entire Error Codes](./error-code/#client-sdk)

**PURCHASE_EXTERNAL_LIBRARY_ERROR**

* Occurs at an IAP module.
* Check the error code as below:

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

* For IAP error codes, refer to the document below.
    * [Mobile Service > IAP > Error Code > Client API Error Typ](/Mobile%20Service/IAP/en/error-code/#client-api#client-api-errors)





