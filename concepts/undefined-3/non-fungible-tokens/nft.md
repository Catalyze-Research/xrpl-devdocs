# 다른 계정에게 NFT 발행 권한 부여

각 계정에는 0명 또는 1명의 승인된 발행자가 대신 NFT를 채굴할 수 있습니다. 발행자를 승인함으로써 크리에이터는 다른 계정이 대신 NFT를 채굴하도록 허용할 수 있으며, 이를 통해 더 많은 NFT를 만드는 데 집중할 수 있습니다.

## 승인된 발행자

지정하기 승인된 발행자는 <mark style="background-color:yellow;">AccountSet</mark> 트랜잭션을 사용하여 설정합니다.

```json
tx_json = {
  "TransactionType": "AccountSet",    
  "Account": "rrE5EgHN4DfjXhR9USecudHm7UyhTYq6m",
  "NFTokenMinter": "r3riWB2TDWRmwmT7FRKdRHjqm6efYu4s9C",
  "SetFlag": xrpl.AccountSetAsfFlags.asfAuthorizedNFTokenMinter
}
```

<mark style="background-color:yellow;">NFTokenMinter</mark>는 동일한 XRP Ledger 인스턴스의 계정 ID입니다. <mark style="background-color:yellow;">asfAuthorizedNFTokenMinter</mark> 플래그는 <mark style="background-color:yellow;">NFTokenMinter</mark> 계정이 해당 <mark style="background-color:yellow;">계정</mark>을 대신하여 NFT를 발행할 수 있는 권한을 부여합니다.

{% hint style="info" %}
_Note_: <mark style="background-color:yellow;">asfAuthorizedNFTokenMinter</mark> 플래그는 <mark style="background-color:yellow;">AccountSet</mark> 트랜잭션에서만 사용됩니다. 이 플래그는 트랜잭션이 계정 루트의 NFTokenMinter 필드의 존재 또는 값을 영향을 주는지 여부를 나타냅니다. 구체적으로 AccountRoot에는 해당하는 플래그가 없습니다.
{% endhint %}

## 승인된 발행자 해제하기&#x20;

승인된 발행자를 제거하려면 <mark style="background-color:yellow;">AccountSet</mark> 트랜잭션을 사용하여 <mark style="background-color:yellow;">asfAuthorizedNFTokenMinter</mark> 플래그를 지웁니다.

```json
tx_json = {
  "TransactionType": "AccountSet",
  "Account": "rrE5EgHN4DfjXhR9USecudHm7UyhTYq6m",
  "ClearFlag": xrpl.AccountSetAsfFlags.asfAuthorizedNFTokenMinter
}
```

## 다른 계정을 위해 NFT 발행하기&#x20;

표준 <mark style="background-color:yellow;">NFTokenMint</mark> 트랜잭션을 사용하여 다른 계정에 대한 토큰을 만들 수 있습니다. 다른 계정을 위해 NFT를 발행할 때는 <mark style="background-color:yellow;">발행자</mark> 필드에 해당하는 계정의 계정 ID를 포함해야 합니다.

```javascript
const transactionBlob = {
  "TransactionType": "NFTokenMint",
  "Account": "r3riWB2TDWRmwmT7FRKdRHjqm6efYu4s9C",
  "URI": xrpl.convertStringToHex("ipfs://bafybeigdyrzt5sfp7udm7hu76uh7y26nf4dfuylqabf3oclgtqy55fbzdi"),
  "Flags": 8,
  "TransferFee": 5000,
  "NFTokenTaxon": 0,
  "Issuer": "rrE5EgHN4DfjXhR9USecudHm7UyhTYq6m", // Needed when minting for another
                                                 // account.
}
```

NFT를 판매할 때, TransferFee(거래 금액의 일정 비율)는 발행 계정에 입금됩니다.
