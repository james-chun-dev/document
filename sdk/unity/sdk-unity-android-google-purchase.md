구글 인앱결제 설정 및 API 에 대해 설명합니다.



## InAppPurchase



결제함수

```
/**
 * @param pid : 스토어에 등록된 상품ID
 * @param action : 결과처리
 */
GamePubSDK.Instance.InAppPurchase("gamepub_1000", result =>
{
    result.Match(
        value => {
            //value.IntDate
            //value.PlayerId
            //value.ProductId
            //value.ServerId
            //value.Status
        },
        error => {
            //error.Code
            //error.Message
        });
});
```



Purchase Response Status Code

| Error Code | Description |
| ---------- | ----------- |
| 0          | default     |
| 1          | Request     |
| 100        | Result      |
| 200        | SendItem OK |

