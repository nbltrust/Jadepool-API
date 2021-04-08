---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - Example

toc_footers:
  - <a href='https://github.com/nbltrust/jadepool-doc'>Jadepool Documentation</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Jadepool API! 

# General Structure

## POST请求

> "Request withdrawal" example:

```json
{  
   "sig":"e365e9f0d43e0dd2b2c0d6ff0fa90c473791bdc532c67fe290f60229633f6c4a4839fb10245c5d29b82a5330788ffd504aa81149a66484dac48e5b9ac8f8881c",
   "data":{  
      "sequence":32132,
      "to":"mg2bfYdfii2GG13HK94jXBYPPCSWRmSiAS",
      "type":"BTC",
      "value":"0.01"
   },
   "encode":"hex",
   "appid":"testa",
   "hash":"sha256",
   "timestamp":1551907163440
}
```

This is the general request structure for POST requests.

*Request Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data | yes | object | data block that main parameters should be wrapped in
appid | yes | string | application ID, required since 0.11.22 version
timestamp | yes | number | the timestamp client used for signing
sig | yes | string | client's ECC signature
encode | no | string | signature encoding format, support hex and base64, default is base64
hash | no | string | support sha3 and sha256 and md5, default value is sha3
lang | no | string | the language of errors and messages in response. support en, cn, ja, default is cn

## GET Request

> "Fetch Wallet Status" example:

```
http://jadepool.example/api/v1/wallet/btc/status?appid=testa&timestamp=1548406480765&encode=hex&hash=sha256
&sig=355776c4b127a8db996032b2b0b3e5618ac83f1b2d394bada3af6b27fa9631f036dd3b46067b274d0e985453bac0f6783da262a41f7f920d84d0514b47b58937&lang=en
```

This is the general request structure for GET requests.

*Query Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
appid | yes | string | application ID, required since 0.11.22 version
timestamp | yes | number | the timestamp client used for signing
sig | yes | string | client's ECC signature
encode | no | string | signature encoding format, support hex and base64, default is base64
hash | no | string | support sha3 and sha256 and md5, default value is sha3
lang | no | string | the language of errors and messages in response. support en, cn, ja, default is cn

## Response 

> Successful Response Example:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1557144244145,
    "sig": {
        "r": "a6pPfsRpp7NVnjtCcoqDyqp5nOS971LG34uECKDBsj4=",
        "s": "csbcZjx+LycfCZoYPa5ba324TUnFIEmy0TzX4vNUPqI=",
        "v": 27
    },
    "result": {
        "address": "0x352c3d587080ae186c6037ccf786ea1c71ed87be",
        "valid": true,
        "namespace": "ETH",
        "sid": "HcEYvElHYswB47SDAAAC"
    }
}
```

> Failed Response Example:

```json
{
    "code": 20000,
    "status": 20000,
    "category": "invalid-parameter",
    "message": "unsupported coin type",
    "result": {
        "info": "field(type) cointype - wrong"
    }
}
```

These are the regular fields in response body.

*Response Fields*

Field | Type | Description
--------- | ------- | --------- 
code | number | 0 if succeeds. Otherwise it shows corresponding error code in [Errors](#errors)
status | number | 0 if succeeds. Otherwise it shows corresponding error code in [Errors](#errors)
message | string | "OK" if succeeds. Otherwise it shows corresponding error message in [Errors](#errors)
crypto | string | Only "ecc" is supported for now
timestamp | number | the timestamp Jadepool used for signing
sig | object | Jadepool's ECC signature
result | object | response contents

# API

<aside class="notice">
All timestamps are in millisecond.
</aside>

<aside class="notice">
All main parameters in POST API should be wraped in "data" object
</aside>

## Address

### Create Multiple Addresses

> Request "Create Multiple Addresses" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556009381747,
    "sig": {
        "r": "zSKIe50E0PEQ0QEP2ktr+rVTSwzHFLHXCbDcZLwbEYE=",
        "s": "O2V+rrLev9PHloM0HXknbn43qc6yGAQ+CYZGTVihON8=",
        "v": 28
    },
    "result": [
        {
            "id": 284,
            "appid": "pri",
            "type": "ETH",
            "address": "0x352c3d587080ae186c6037ccf786ea1c71ed87be",
            "state": "used",
            "mode": "deposit",
            "create_at": 1556009381632,
            "update_at": 1556009381688,
            "namespace": "ETH"
        },
        {
            "id": 285,
            "appid": "pri",
            "type": "ETH",
            "address": "0xe1f3247967f9e51a0cc733f8191166f5d6a8b30d",
            "state": "used",
            "mode": "deposit",
            "create_at": 1556009381730,
            "update_at": 1556009381744,
            "namespace": "ETH"
        }
    ]
}
```

This endpoint enables you to request multiple new addresses at once.

*HTTP Request*

`POST http://jadepool.example/api/v2/address/:coinName/batchnew`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.amount | yes | number | number of addresses, range from 1 to 500.
data.mode | yes | string | address mode: 'auto', 'deposit', 'deposit_memo', 'normal'

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | number | unique ID in Jadepool's database
appid | string | application ID, configurable on admin
type | string | token type of the new address
address | string | the new address
state | string | this value does not mean anything for now
mode | string | address mode

### Create Single Address

> Request "Create Single Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556009180055,
    "sig": {
        "r": "nIYVH6S5qz2gIHtxjWzp+muM4CVaqU0et0NCbCamjKs=",
        "s": "WR/OEB8RMPRXDQopWfpwc2Npki+b82ASsewONMpIR5I=",
        "v": 27
    },
    "result": {
        "id": 283,
        "appid": "pri",
        "type": "ETH",
        "address": "0xf7d862f2b43d3bff3444e6c3dd9aceef837f817d",
        "state": "used",
        "mode": "deposit",
        "create_at": 1556009180037,
        "update_at": 1556009180052,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to request single new address.

*HTTP Request*

`POST http://jadepool.example/api/v2/address/:coinName/new`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.mode | yes | string | address mode: 'auto', 'deposit', 'deposit_memo', 'normal'

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | number | unique ID in Jadepool's database
appid | string | application ID, configurable on admin
type | string | token type of the new address
address | string | the new address
state | string | this value does not mean anything for now
mode | string | address mode

### Validate Address

> Request "Validate Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556009541845,
    "sig": {
        "r": "RGw+vmBSAFIknSp4sYPLLlKAv0XNFt+3Xz5gTnttIFA=",
        "s": "V+zMC77GTyh2igr8eMbl0JtpM1hu9ZisJOgYGhmW5hQ=",
        "v": 28
    },
    "result": {
        "address": "0x352c3d587080ae186c6037ccf786ea1c71ed87be",
        "valid": true,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to verify if an address is valid for specified token.

*HTTP Request*

`POST http://jadepool.example/api/v2/address/:coinName/verify`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.address | yes | string | address to validate

*Response Result*

Value | Type | Description
--------- | ------- | ---------
address | string | the address to validate
valid | boolean | the address is valid or not

### Force Free

> Request "Force Free" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556010383578,
    "sig": {
        "r": "wBs2UnJLy16lww2mrp8oi71W/Qi1101ElkIDFKGjq9c=",
        "s": "WJYTn/vjf0y/Z3DdkJ/7XAiqvylKI/DhHP0mlMLDg7I=",
        "v": 27
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "_id": "5cbed58f8d1607c10ddddf93",
        "id": "6515",
        "coinName": "ETH",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "SWEEP_INTERNAL",
        "type": "ETH",
        "coinType": "ETH",
        "from": "0xa268f4ae690ac1421775568d783439c7c1151749",
        "to": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "value": "0.001",
        "sequence": 1,
        "confirmations": 0,
        "create_at": 1556010383570,
        "update_at": 1556010383571,
        "actionArgs": [],
        "actionResults": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to transfer tokens from a deposit address to the main hot wallet address.

*HTTP Request*

`POST http://jadepool.example/api/v2/address/:coinName/free`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again

### Force Obtain

> Request "Force Obtain" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556010383578,
    "sig": {
        "r": "wBs2UnJLy16lww2mrp8oi71W/Qi1101ElkIDFKGjq9c=",
        "s": "WJYTn/vjf0y/Z3DdkJ/7XAiqvylKI/DhHP0mlMLDg7I=",
        "v": 27
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "_id": "5cbed58f8d1607c10ddddf93",
        "id": "6515",
        "coinName": "ETH",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "SWEEP_INTERNAL",
        "type": "ETH",
        "coinType": "ETH",
        "from": "0xa268f4ae690ac1421775568d783439c7c1151749",
        "to": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "value": "0.001",
        "sequence": 1,
        "confirmations": 0,
        "create_at": 1556010383570,
        "update_at": 1556010383571,
        "actionArgs": [],
        "actionResults": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to transfer tokens from the main hot wallet address to a deposit address.

*HTTP Request*

`POST http://jadepool.example/api/v2/address/:coinName/obtain`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.address | yes | string | address to validate
data.sequence | yes | number | unique request sequence
data.value | yes | string | force obtain value

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again

### Fetch Incomplete Orders

> Request "Fetch Incomplete Orders" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556017079643,
    "sig": {
        "r": "EWxRFPkVClfS6Y8hz4kyarkhEbvOQLgcDY6GR+D8zdQ=",
        "s": "TavLCYToUrn3778oNslgPXuMu4j/Jsd+8hNbzN3dtZI=",
        "v": 27
    },
    "result": [
        {
            "_id": "5cbeefb353ee1205fdd12284",
            "id": "6515",
            "coinName": "LTC",
            "txid": null,
            "meta": null,
            "state": "init",
            "bizType": "WITHDRAW",
            "type": "LTC",
            "coinType": "LTC",
            "from": "mtRMmr6opfJMakQDxL2Dzv1KeQRt9VLRU8",
            "to": "mrsUG77kvGMXxDwntoeREUmgmPheX2o1F8",
            "value": "0.001",
            "sequence": 2,
            "confirmations": 0,
            "create_at": 1556017075658,
            "update_at": 1556017075660,
            "actionArgs": [],
            "actionResults": [],
            "n": 0,
            "fee": "0",
            "fees": [],
            "data": {
                "timestampBegin": 0,
                "timestampFinish": 0
            },
            "hash": "",
            "block": -1,
            "extraData": "",
            "memo": "",
            "sendAgain": false
        }
    ]
}
```

This endpoint enables you to fetch all incomplete orders that are of state in: 'init', 'holding', 'online', 'pending'.

*HTTP Request*

`GET http://jadepool.example/api/v2/address/:coinName/:address/orders[/:state]`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name
address | related address
state | optional, can be one of the incomplete order states: 'init', 'holding', 'online', 'pending'

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again

### Fetch Address Balance

> Request "Fetch Address Balance" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556016915285,
    "sig": {
        "r": "RpVC2Tm8m9LJ77mJcoIBwNLtbb/WQyOl2jqA4hBbwN0=",
        "s": "fUetmUE8uAIDRnXeTSBeM90sIj4pkqABdurQQb6/jxU=",
        "v": 28
    },
    "result": {
        "balance": "0",
        "namespace": "LTC",
        "sid": "viqU_GCM4-wg3P1kAAAD"
    }
}
```

This endpoint enables you to fetch balance of a specified address.

*HTTP Request*

`GET http://jadepool.example/api/v2/address/:coinName/:address/balance`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name
address | related address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
balance | string | the address' balance on blockchain

### Fetch Address Info

> Request "Fetch Address Info" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556016629310,
    "sig": {
        "r": "LPVc8+D2jDINOpLrUepizbhNU+CnKjIb1eH/yowP6nc=",
        "s": "ZW/mtl5bSIgejcXVSNVRa+/5IG7vOk7J6v0pYvbApzA=",
        "v": 28
    },
    "result": {
        "id": 288,
        "appid": "pri",
        "type": "BTC",
        "address": "moUQrRi7cKNjskr3pnKqjXbZZ5cH2dWuiG",
        "state": "used",
        "mode": "deposit",
        "create_at": 1556016613697,
        "update_at": 1556016613733,
        "namespace": "BTC",
        "sid": "XDd9EeHVqN08oGVOAAAC"
    }
}
```

This endpoint enables you to fetch details of a specified address.

*HTTP Request*

`GET http://jadepool.example/api/v2/address/:coinName/:address`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name
address | related address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | number | unique ID in Jadepool's database
appid | string | application ID, configurable on admin
type | string | token type of the new address
address | string | the new address
state | string | this value does not mean anything for now
mode | string | address mode

## Audit

### Fetch Audit Order

> Request "Fetch Audit Order" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1583808224613,
    "sig": {
        "r": "t90UoLKHNXTBtmw5yZdO7FJ/gcS60U4JKMgpYnCCo0o=",
        "s": "K4NcSIXRdJph2Yw8mVgSmFnp5689zfc6SSO65Rg8T+8=",
        "v": 28
    },
    "result": {
        "wallet": "default",
        "blocknumber": 214,
        "calculated": true,
        "deposit_total": "0.01",
        "deposit_num": 1,
        "withdraw_total": "0.2",
        "withdraw_num": 1,
        "revert_total": "0",
        "revert_num": 0,
        "refund_total": "0",
        "refund_num": 0,
        "system_call_total": "0",
        "system_call_num": 0,
        "delegate_total": "0",
        "delegate_num": 0,
        "undelegate_total": "0",
        "undelegate_num": 0,
        "principal_fund_total": "0",
        "principal_fund_num": 0,
        "interest_fund_total": "0",
        "interest_fund_num": 0,
        "sweep_total": "0",
        "sweep_num": 0,
        "sweep_internal_total": "0",
        "sweep_internal_num": 0,
        "airdrop_total": "0",
        "airdrop_num": 0,
        "recharge_total": "0",
        "recharge_num": 0,
        "recharge_internal_total": "0",
        "recharge_internal_num": 0,
        "recharge_unexpected_total": "0.601343293996",
        "recharge_unexpected_num": 3,
        "recharge_special_total": "0",
        "recharge_special_num": 0,
        "failed_withdraw_num": 0,
        "failed_sweep_num": 0,
        "failed_sweep_internal_num": 4,
        "failed_refund_num": 0,
        "failed_system_call_num": 0,
        "failed_delegate_num": 0,
        "failed_undelegate_num": 0,
        "fees": [
            {
                "withdraw_fee": "0.0101",
                "refund_fee": "0",
                "sweep_fee": "0",
                "sweep_internal_fee": "0",
                "system_call_fee": "0",
                "delegate_fee": "0",
                "undelegate_fee": "0",
                "failed_withdraw_fee": "0",
                "failed_refund_fee": "0",
                "failed_sweep_fee": "0",
                "failed_sweep_internal_fee": "0.004330699234",
                "failed_system_call_fee": "0",
                "failed_delegate_fee": "0",
                "failed_undelegate_fee": "0",
                "_id": "5e66fee06b50acfa7ec50fec",
                "fee_type": "KSM"
            }
        ],
        "chainKey": "Kusama",
        "type": "KSM",
        "timestamp": 1583808224613,
        "appid": "pri",
        "create_at": "2020-03-10T02:43:44.733Z",
        "update_at": "2020-03-10T02:43:44.833Z",
        "__v": 1,
        "calc_order_num": 13,
        "id": "5e66fee041db0dfa7f558411"
    }
}
```

This endpoint enables you to fetch audit order.

*HTTP Request*

`GET http://jadepool.example/api/v2/audits/:id`

*Query Parameters*

Parameter | Description
--------- | -------
id | audit order ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
calculated | boolean | if the auditing is finished
deposit_total | string | total deposit token value
deposit_num | number | total deposit times
withdraw_total | string | total withdraw token value
withdraw_num | number | total withdraw times
sweep_total | string | total hot-to-cold token value
sweep_num | number | total hot-to-cold times 
sweep_internal_total | string | total internal-out token value
sweep_internal_num | number | total internal-out times
airdrop_total | string | total airdrop token value
airdrop_num | number | total airdrop times
recharge_total | string | total cold-to-hot token value
recharge_num | number | total cold-to-hot times
recharge_internal_total | string | total internal-in token value
recharge_internal_num | number | total internal-in times
recharge_unexpected_total | string | total unexpected-in token value
recharge_unexpected_num | number | total unexpected-in times
recharge_special_total | string | total special-in token value
recharge_special_num | number | total special-in times
failed_withdraw_num | number | total failed withdrawal times
failed_sweep_num | number | total failed hot-to-cold times
failed_sweep_internal_num | number | total failed internal-out times
fees | array | all type of fees burned
withdraw_fee | string | fees burned for withdrawal
refund_fee | string | fees burned for refund
sweep_fee | string | fees burned for hot-to-cold
sweep_internal_fee | string | fees burned for internal-out
system_call_fee | string | fees burned for all system_call txs
delegate_fee | string | fees burned for staking
undelegate_fee | string | fees burned for unstaking
failed_withdraw_fee | string | fees burned for failed withdrawal
failed_refund_fee | string | fees burned for failed refund
failed_sweep_fee | string | fees burned for refund hot-to-cold
failed_sweep_internal_fee | string | fees burned for refund internal-out
failed_system_call_fee | string | fees burned for all failed system_call txs
failed_delegate_fee | string | fees burned for failed delegation
failed_undelegate_fee | string | fees burned for failed undelegation
type | string | the audited token type
timestamp | number | the audited timestamp
blocknumber | number | corresponding block near the audited timestamp
appid | string | application ID
calc_order_num | number | audited order number
last | string | audit order ID of the previous audit
id | string | audit order ID of the current audit

### Audited Orders

> Request "Audited Orders" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1583897683128,
  "sig": {
    "r": "oQJOtaonOe3NvycDQXNXrjsfS3pIdgRo1imh+9o5Tv8=",
    "s": "eMrUrwaNs4MUFb3d+Y1ASYTN4ec46pHsgc4cvA5PnS8=",
    "v": 28
  },
  "result": [
    {
      "_id": "5e65b0c90c34df59da78c15c",
      "id": "6317",
      "coinName": "KSM",
      "txid": "0xea7876c8121bc7db0d7fd4ed005d267702a0e0fe6df3b9bb0533b0fc28faaae8",
      "meta": "13",
      "appid": "test",
      "wallet": "default",
      "state": "done",
      "bizType": "WITHDRAW",
      "type": "KSM",
      "coinType": "KSM",
      "from": "EkFskcVG8xm7mbASU29VmiJTuFuSBodNrs9xjbUKNHKqmHP",
      "to": "FVCSCiM5TricGwJ2TZ84mRGLgEcNAKfXUCd2fgB2MFJkCxd",
      "value": "0.2",
      "sequence": 1583722697437,
      "confirmations": 3750,
      "create_at": 1583722697549,
      "update_at": 1583897670562,
      "action": "withdraw-from-hotwallet",
      "actionArgs": [
        
      ],
      "actionResults": [
        
      ],
      "postHandlers": [
        
      ],
      "n": 3,
      "fee": "0.02",
      "fees": [
        {
          "_id": "5e660a42667364a489bdae6a",
          "amount": "0.02",
          "coinName": "KSM",
          "nativeAmount": "0",
          "nativeName": ""
        }
      ],
      "data": {
        "timestampBegin": 1583722702743,
        "timestampFinish": 1583745602955,
        "timestampHandle": 1583722702283,
        "failedBlockNumber": null,
        "failedTimestamp": null,
        "goodBlockNumber": 1373448,
        "goodTimestamp": 1583745597170
      },
      "hash": "0xea7876c8121bc7db0d7fd4ed005d267702a0e0fe6df3b9bb0533b0fc28faaae8",
      "txid_rawtx": "",
      "block": 1369700,
      "blockHash": "0x226b6eb54e524ad1ba2f1d11a4b9e7d0171a42dcb5b0ffa184f35e3b8448a959",
      "blockTimestamp": 1583722620000,
      "extraData": "",
      "memo": "",
      "sendAgain": false,
      "relatedOrder": "",
      "relatedIssueRecords": [
        
      ]
    },
    {
      "_id": "5e65b0f490acb159d97b45c5",
      "id": "6318",
      "coinName": "KSM",
      "txid": "0xea7876c8121bc7db0d7fd4ed005d267702a0e0fe6df3b9bb0533b0fc28faaae8",
      "meta": null,
      "appid": "pri",
      "wallet": "default",
      "state": "done",
      "bizType": "DEPOSIT",
      "type": "KSM",
      "coinType": "KSM",
      "from": "EkFskcVG8xm7mbASU29VmiJTuFuSBodNrs9xjbUKNHKqmHP",
      "to": "FVCSCiM5TricGwJ2TZ84mRGLgEcNAKfXUCd2fgB2MFJkCxd",
      "value": "0.2",
      "sequence": 15837227406189476,
      "confirmations": 3749,
      "create_at": 1583722740617,
      "update_at": 1583897670562,
      "actionArgs": [
        
      ],
      "actionResults": [
        
      ],
      "postHandlers": [
        
      ],
      "n": 3,
      "fee": "0.02",
      "fees": [
        {
          "_id": "5e660a3d667364a489bdae63",
          "amount": "0.02",
          "coinName": "KSM",
          "nativeAmount": "0",
          "nativeName": ""
        }
      ],
      "data": {
        "timestampBegin": 1583722740618,
        "timestampFinish": 1583745597398,
        "timestampHandle": 1583722740618
      },
      "hash": "0xea7876c8121bc7db0d7fd4ed005d267702a0e0fe6df3b9bb0533b0fc28faaae8",
      "txid_rawtx": "",
      "block": 1369700,
      "blockHash": "0x226b6eb54e524ad1ba2f1d11a4b9e7d0171a42dcb5b0ffa184f35e3b8448a959",
      "blockTimestamp": 1583722620000,
      "extraData": "",
      "memo": "",
      "sendAgain": false,
      "relatedOrder": "",
      "relatedIssueRecords": [
        
      ]
    }
  ]
}
```

This endpoint enables you to fetch orders in specified audit.

*HTTP Request*

`GET http://jadepool.example/api/v2/audits/:id/orders`

*Query Parameters*

Parameter | Description
--------- | ------- 
id | audit order ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

## Order

### Fetch Order

> Request "fetch Order" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556009989295,
    "sig": {
        "r": "b4uHdTjVe72esYVTLhIami5xz/oNcdPKQLRVorNGoAk=",
        "s": "dzMrG9qeLixaq7B+0qY5FhTWR62vbv3nH3ey0mg03yM=",
        "v": 27
    },
    "result": {
        "_id": "5cbece338d1607c10ddddbcd",
        "id": "6513",
        "coinName": "ETH",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "WITHDRAW",
        "type": "ETH",
        "coinType": "ETH",
        "from": "0xa268f4ae690ac1421775568d783439c7c1151749",
        "to": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "value": "0.001",
        "confirmations": 0,
        "create_at": 1556008499740,
        "update_at": 1556008499742,
        "actionArgs": [],
        "actionResults": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false
    }
}
```

This endpoint enables you to fetch any order details from Jadepool database.

*HTTP Request*

`GET http://jadepool.example/api/v2/orders/:id`

*Query Parameters*

Parameter | Description
--------- | ------- 
id | order ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

## Wallet

### Fetch Wallet Status

> Request "Fetch wallet Status" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556010241947,
    "sig": {
        "r": "FzPkBS6BgMRJu/8OLd5jq+JKTBXHEr6rXo5vx/MJZd8=",
        "s": "SXiyJ+eBkOrIF206dJ24IKrZlRgDqe37kOrozaalNCA=",
        "v": 28
    },
    "result": {
        "balance": "0.01",
        "balanceAvailable": "0",
        "balanceUnavailable": "0.01",
        "update_at": 1556010241943,
        "addressesWithBalance": 1,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to fetch balance details of any enabled token.

*HTTP Request*

`GET http://jadepool.example/api/v2/wallet/:coinName/status`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name

*Response Result*

Value | Type | Description
--------- | ------- | ---------
balance | string | total balance
balanceAvailable | string | balance available for outgoing transfer
balanceUnavailable | string | balance unavailable for outgoing transfer
update_at | number | the balance update time
addressesWithBalance | number | number of addresses that have balance

### Request Withdrawal

> Request "Request Withdrawal" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556010383578,
    "sig": {
        "r": "wBs2UnJLy16lww2mrp8oi71W/Qi1101ElkIDFKGjq9c=",
        "s": "WJYTn/vjf0y/Z3DdkJ/7XAiqvylKI/DhHP0mlMLDg7I=",
        "v": 27
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "_id": "5cbed58f8d1607c10ddddf93",
        "id": "6514",
        "coinName": "ETH",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "WITHDRAW",
        "type": "ETH",
        "coinType": "ETH",
        "from": "0xa268f4ae690ac1421775568d783439c7c1151749",
        "to": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "value": "0.001",
        "sequence": 1,
        "confirmations": 0,
        "create_at": 1556010383570,
        "update_at": 1556010383571,
        "actionArgs": [],
        "actionResults": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to request withdrawal.

*HTTP Request*

`POST http://jadepool.example/api/v2/wallet/:coinName/withdraw`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.to | yes | string | withdrawal address
data.value | yes | string | value to withdraw
data.sequence | yes | number | unique API nonce for each app ID
data.auth | no | string | authentication for coin, only for HSM
data.extraData | no | string | extra data

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

### Deposit to ETH2.0 Validator

> Request "Deposit to ETH2.0 Validator" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1601364624962,
    "sig": {
        "r": "mdEk9OjsiVzNI2Dk7AWlA90yj1zPrVJCdBBBDDs05DE=",
        "s": "fW8jnR8zx+ceZOBuiNl2XKy/3YFRuo9l7hBa8CuDpU0=",
        "v": 28
    },
    "result": {
        "_id": "5f72e2900a38036c1488cd70",
        "id": "131689",
        "coinName": "ETH",
        "txid": null,
        "meta": null,
        "appid": "app_4",
        "wallet": "wallet_2",
        "sendMode": "normal",
        "state": "init",
        "bizType": "SYSTEM_CALL",
        "type": "ETH",
        "coinType": "ETH",
        "from": "0x3E0A276B9d9085508979149A05Cd7e213EC5DA0E",
        "froms": [
            "0x3E0A276B9d9085508979149A05Cd7e213EC5DA0E"
        ],
        "to": "0x07b39F4fDE4A38bACe212b546dAc87C58DfE3fDC",
        "value": "1",
        "sequence": 1601364625,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1601364624953,
        "update_at": 1601364624955,
        "action": "validator_deposit",
        "actionArgs": [
            "98114c41a81aec827c4fec58f92ae86e3d69e1dd5f9b9f16653d746d46cee1465e55a604a801d4f40b3153c032cc7905",
            "00b025640e57e3bf6f3b8c06d737730b8444c034ed3a38173179f35ec4f85e31",
            "81526ac4a1da286d6aa73656a53120d3364510c40d2c24e7419e796104a492fdde59c4eeed9b6c1a93e980891faaca0c125ed0bc5eb032d8b8ffb7bfd957acef4e54462e39eb4d1e07b7964f4db1419e8850e615dbe48d8c7321fe683da6e963",
            "3c60988198779505a2379037bf258037df8fa970af9bde863052e18d26db886b",
            "0xa81b4143ef7be1015d22bedf3ded93dc965e94edbcf28164c84ad3831ee064e355fdc6c92e37e7a2fb1a30e0875c4164"
        ],
        "actionResults": [],
        "postHandlers": [],
        "bizFlows": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "options": {},
        "pendingTags": [],
        "sendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "9aYBClnuVaTicQ50AAAs"
    }
}
```

This endpoint enables you to deposit to ETH2.0 validator.

*HTTP Request*

`POST http://jadepool.example/api/v2/wallet/:token/validator_deposit`

*Query Parameters*

Parameter | Description 
--------- | --------- 
token | should pass "ETH"

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.sequence | yes | number | unique API nonce for each app ID
data.withdrawal_pubkey | yes | string | validator's withdrawal public key
data.withdrawal_credentials | yes | string | withdrawal credentials obtained from "signing key server"
data.signing_pubkey | yes | string | validator's signing public key
data.signature | yes | string | signature obtained from "signing key server"
data.deposit_data_root | yes | string | deposit_data_root obtained from "signing key server"
data.amount | yes | string | The amount to deposit to the validator

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

### Audit For Specified Coin

> Request "Audit For Specified Coin" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556010708546,
    "sig": {
        "r": "8h5Ut8zNxXNY9ZNDNtcZfRdp/pGOBbieXSRPj3d1adI=",
        "s": "AUlde6nijZ9R3+pY4MxYdKFocViQYzkq0ar9X9lP/WI=",
        "v": 28
    },
    "result": {
        "type": "ETH",
        "id": "5cbecdb38d1607c10ddddb49",
        "blocknumber": 9921056,
        "timestamp": 1546425032000,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to request audit with timestamp of your choice.

*HTTP Request*

`POST http://jadepool.example/api/v2/wallet/:coinName/audit`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | coin name

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.audittime | yes | number | audit timestamp

*Response Result*

Value | Type | Description
--------- | ------- | ---------
type | string | the audited token type
id | string | audit order ID of the current/last audit
timestamp | number | the audited timestamp
blocknumber | number | corresponding block near the audited timestamp

## Wallets

### Audit For All Coins

> Request "Audit For All Coins" API returns JSON structured like this:

```json
{
    "code": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556011203974,
    "sig": {
        "r": "r6Epdg/bkbDxZ5Enq6HBs06ZQGikLyij3VJ/SVJL0Ho=",
        "s": "X30CD6m/3kUBFkc2OCNz+939q029R7H/dnyusCyGo3U=",
        "v": 27
    },
    "result": [
        {
            "type": "ETH",
            "namespace": "ETH",
            "id": "5cbecdb38d1607c10ddddb49",
            "blocknumber": 9921056,
            "timestamp": 1546425032000,
            "sid": "j5D28yAyhO02C-_6AAAA"
        }
    ]
}
```

This endpoint enables you to request audit of all tokens.

**HTTP Request**

`POST http://jadepool.example/api/v2/wallets/audit`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.audittime | yes | number | audit timestamp

*Response Result*

Value | Type | Description
--------- | ------- | ---------
type | string | the audited token type
id | string | audit order ID of the current/last audit
timestamp | number | the audited timestamp
blocknumber | number | corresponding block near the audited timestamp

## Delegation

## Delegation

<aside class="warning">
Query parameter "coinName" in every delegation API must be the main token that can be delegated to validators. Otherwise
request will be rejected.
</aside>

### Fetch All Validators

> Request "Fetch All Validators" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556009186429,
    "sig": {
        "r": "kfHfp+ql5OGMYKhLORY1ri7mzP3uv1FGNL5TtK8XYLw=",
        "s": "WBhcqM6Jn4BJFHxQicO9uR9T7t6aFYCnGVzPqHfB5dw=",
        "v": 27
    },
    "result": [
        {
            "name": "node0",
            "address": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "pubkey": "cosmosvalconspub1zcjduepqmp75ajy7jm0mqttc2d23ks54dn83yfshxd3038r4ejldcjcd0atsnwxnyr",
            "jailed": false,
            "status": 2,
            "shares": "101010000.000000000000000000",
            "delegationAmount": "101010000",
            "minSelfDelegation": "1",
            "commission": {
                "rate": 0.1,
                "maxRate": 0.2,
                "maxChangeRate": 0.01,
                "updateAt": "2019-04-23T04:41:34.202Z"
            }
        },
        {
            "name": "node1",
            "address": "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
            "pubkey": "cosmosvalconspub1zcjduepqrjklsgkc8jeusfkjmrltlvz0s9cxe7vxr7uajpx2v0rfn0zvgtxqw5mwhj",
            "jailed": true,
            "status": 0,
            "shares": "2524242.424242424242424241",
            "delegationAmount": "2499000",
            "minSelfDelegation": "100000",
            "commission": {
                "rate": 0.1,
                "maxRate": 0.2,
                "maxChangeRate": 0.01,
                "updateAt": "2019-04-23T04:41:44.782Z"
            }
        }
    ]
}
```

This endpoint enables you to fetch all validators of specified chain.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/validators`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name

*Response Result*

Value | Type | Description
--------- | ------- | ---------
name | string | validator name
address | string | validator's address
pubkey | string | validator's public key
jailed | boolean | if the validator is jailed
status | number | validator's status
shares | string | validator's shares
delegationAmount | string | amount delegated to validator
minSelfDelegation | string | amount delegated to validator by itself
commission | object | validator's commission
rate | float | validator's commission rate
maxRate | float | validator's commission maxRate
maxChangeRate | float | validator's commission maxChangeRate
updateAt | string | validator info update time

### Fetch Specified Validator

> Request "Fetch Specified Validator" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1554801539663,
    "sig": {
        "r": "9H9TTNLQhYwre7yKsfd6dmt8iLAzqvVRxlt1O7++dZE=",
        "s": "Qcn6m5ZwqKnuBLHyRtFuB+UNhfJAz+/Ny+kA+8SzVKQ=",
        "v": 28
    },
    "result": {
        "name": "node1",
        "address": "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
        "pubkey": "cosmosvalconspub1zcjduepqfr9zx5yrnfg7y8zwhqvm8yywtvhuzg0qn7auwygkkp4zlfd36pusx4xgn3",
        "jailed": false,
        "status": 2,
        "shares": "1040000.000000000000000000",
        "delegationAmount": "1040000",
        "minSelfDelegation": "100000",
        "commission": {
            "rate": 0.1,
            "maxRate": 0.2,
            "maxChangeRate": 0.01,
            "updateAt": "2019-04-03T08:19:29.921Z"
        },
        "namespace": "Cosmos",
        "sid": "h-02Y6BZGFDuZyWzAAAF"
    }
}
```

This endpoint enables you to fetch details of a specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/validators/:validator`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
validator | validator name

*Response Result*

Value | Type | Description
--------- | ------- | ---------
name | string | validator name
address | string | validator's address
pubkey | string | validator's public key
jailed | boolean | if the validator is jailed
status | number | validator's status
shares | string | validator's shares
delegationAmount | string | amount delegated to validator
minSelfDelegation | string | amount delegated to validator by itself
commission | object | validator's commission
rate | float | validator's commission rate
maxRate | float | validator's commission maxRate
maxChangeRate | float | validator's commission maxChangeRate
updateAt | string | validator info update time

### Outstanding Reward At Validator

> Request "Fetch Reward With Validator" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1554796799490,
    "sig": {
        "r": "AD40uPwf5iJkuTXuhuKfRnt3+/imo8PNBgryOHxYhHE=",
        "s": "KN2Dk9BHCyucq291X8wFeFE9xYcnL3ziYBjdbeTfKaY=",
        "v": 28
    },
    "result": [
        {
            "validator": "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
            "nativeName": "stake",
            "nativeAmount": "40863.901112837984093534",
            "coinName": "ATOM",
            "amount": "0.04086390111283798409"
        }
    ]
}
```

This endpoint enables you to fetch everyone's total reward amount at a specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/validators/:validator/outstanding_rewards`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
validator | validator name

*Response Result*

Value | Type | Description
--------- | ------- | ---------
validator | string | validator name
nativeName | string | token
nativeAmount | string | total reward amount in token unit
coinName | string | main token
amount | string | total reward amount in main token unit

### Request Delegation

> Request "Request Delegation" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1557309326894,
    "sig": {
        "r": "kGvfjb52iroMPZP/UH7Gq0ngzzRwc/sNrzvvxXF0Ro0=",
        "s": "JMAThiBR+c4lBmu/nC0xNFLiMRemA9wCyZ82bTqnhKQ=",
        "v": 28
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "_id": "5cd2a78ee7fa5b57ab9ddaec",
        "id": "2428",
        "coinName": "ATOM",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "DELEGATE",
        "type": "ATOM",
        "coinType": "ATOM",
        "from": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "to": "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
        "value": "0.1",
        "sequence": 1557309326848,
        "confirmations": 0,
        "create_at": 1557309326881,
        "update_at": 1557309326884,
        "action": "delegate",
        "actionArgs": [
            "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
            "0.1"
        ],
        "actionResults": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "Cosmos",
        "sid": "V0-ORASYpnLqodqtAAAG"
    }
}
```

This endpoint enables you to request delegation.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/delegate`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.amount | yes | string | delegate value
data.validator | yes | string | validator to delegate
data.sequence | yes | number | unique sequence

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

### Request Re-delegation

> Request "Request Re-delegation" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008441494,
    "sig": {
        "r": "bPJroemxN8K+y1eDoAv/L37FjmzUfdjoUdjl9NyNSbs=",
        "s": "bIVR73xizsqwCANBfl+ZoJ6QeLm9Qc/AlAvMTg0YK7c=",
        "v": 28
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "id": "23",
        "coinName": "ATOM",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "SYSTEM_CALL",
        "type": "ATOM",
        "coinType": "ATOM",
        "to": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
        "value": "0",
        "sequence": 1556008441401,
        "confirmations": 0,
        "create_at": 1556008441486,
        "update_at": 1556008441488,
        "action": "re-delegate",
        "actionArgs": [
            "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
            "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "0.001"
        ],
        "actionResults": [],
        "from": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "Cosmos",
        "sid": "RPamVnMkXe2aIgElAAAB"
    }
}
```

This endpoint enables you to request delegation switch from one validator to another.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/redelegate`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.amount | yes | string | delegate value
data.sequence | yes | number | unique sequence
data.src_validator | yes | string | original validator
data.dst_validator | yes | string | validator to re-delegate

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

### Request Undelegation

> Request "Request Undelegation" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556018549519,
    "sig": {
        "r": "vxHm68qJIL6I3rl8E55Z219QZiv00GMBwZtvTrOaEyA=",
        "s": "HUN+YVLlQH/WLhywVllNbJ0RcXHsi9yImhRwZb0QaBc=",
        "v": 28
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "id": "38",
        "coinName": "ATOM",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "UNDELEGATE",
        "type": "ATOM",
        "coinType": "ATOM",
        "to": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
        "value": "0.001",
        "sequence": 1556018549426,
        "confirmations": 0,
        "create_at": 1556018549489,
        "update_at": 1556018549490,
        "action": "un-delegate",
        "actionArgs": [
            "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "0.001"
        ],
        "actionResults": [],
        "from": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "Cosmos",
        "sid": "h2112M6bFQZImpdUAAAB"
    }
}
```

This endpoint enables you to request undelegation.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/undelegate`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | address requests undelegation

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.amount | yes | string | undelegate value
data.sequence | yes | number | unique sequence
data.validator | yes | string | validator undelegate from

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

### Overall Delegation Status

> Request "Overall Delegation Status" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1554884132672,
    "sig": {
        "r": "cgQV3td0/uPse5oMyBjcfbXJ6lw9hO3hJAovdxwW2DQ=",
        "s": "RorA0lqY4t0kCNyezW46SlDflGpc19k35KBhhfoq7K4=",
        "v": 27
    },
    "result": [
        {
            "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "validator": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "amount": "0.03",
            "nativeAmount": "30000.000000000000000000"
        },
        {
            "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "validator": "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
            "amount": "0.011",
            "nativeAmount": "11000.000000000000000000"
        }
    ]
}
```

This endpoint enables you to fetch overall delegation status of a specified address.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/delegations`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
delegator | string | the address that performs delegation
validator | string | validator to delegate to
amount | string | delegated amount in main token unit
nativeAmount | string | delegated amount in token unit

### Delegation Status At Validator

> Request "Delegation Status At Validator" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1554796803335,
    "sig": {
        "r": "/v1N1WplY/d4Mses22jsHrb2MKkYuCEjFI82OumUyYE=",
        "s": "db9UeNag67gkF4RRgIcx9SZBkOwsyzQd6vnL9pIrg/Y=",
        "v": 27
    },
    "result": {
        "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "validator": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
        "amount": "0.02",
        "nativeAmount": "20000.000000000000000000",
        "namespace": "Cosmos",
        "sid": "V2569qy2CYUbK6PHAAAH"
    }
}
```

This endpoint enables you to fetch delegation status of a specified address at a specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/delegations/:validator`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address
validator | validator

*Response Result*

Value | Type | Description
--------- | ------- | ---------
delegator | string | the address that performs delegation
validator | string | validator to delegate to
amount | string | delegated amount in main token unit
nativeAmount | string | delegated amount in token unit

### Overall Undelegation Status

> Request "Overall Undelegation Status" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556018572656,
    "sig": {
        "r": "WWOEITPtz0GLLeVk0fvJd57kFdnTvmma1FSY/FVo13M=",
        "s": "Ka3Kv8vwpMRYjDJaQDki39fGGy1+9vy677hpWOygLTc=",
        "v": 28
    },
    "result": [
        {
            "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "validator": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "entries": [
                {
                    "creationHeight": 4779,
                    "completionTime": 1556018619654,
                    "initialNativeAmount": "1000",
                    "nativeAmount": "1000",
                    "initialAmount": "0.001",
                    "amount": "0.001"
                }
            ]
        }
    ]
}
```

This endpoint enables you to fetch overall unstaking status of a specified address.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/unstaking_delegations`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
delegator | string | the address that performs delegation
validator | string | validator delegated to
creationHeight | string | the block height while undelegation requested
completionTime | string | undelegation completion time 
initialNativeAmount | string | reuqested amount to undelegate in token unit
nativeAmount | string | real-time actual amount that will be undelegated in token unit
initialAmount | string | reuqested amount to undelegate in main token unit
amount | string | real-time actual amount that will be undelegated in main token unit

### Undelegation At Validator

> Request "Undelegation At Validator" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556018590623,
    "sig": {
        "r": "Oaww0WvkGyus5IFxbYdCW76STqsk5Phb7+2rbSTTbP0=",
        "s": "Q128OllQ+BPB1qbhz8a2DzNh+zd8THHmtDtcnqydV6c=",
        "v": 28
    },
    "result": {
        "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "validator": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
        "entries": [
            {
                "creationHeight": 4779,
                "completionTime": 1556018619654,
                "initialNativeAmount": "1000",
                "nativeAmount": "1000",
                "initialAmount": "0.001",
                "amount": "0.001"
            }
        ],
        "namespace": "Cosmos",
        "sid": "h2112M6bFQZImpdUAAAB"
    }
}
```

This endpoint enables you to fetch unstaking status of a specified address at a specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/unstaking_delegations/:validator`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address
validator | validator

*Response Result*

Value | Type | Description
--------- | ------- | ---------
delegator | string | the address that performs delegation
validator | string | validator delegated to
creationHeight | string | the block height while undelegation requested
completionTime | string | undelegation completion time
initialNativeAmount | string | reuqested amount to undelegate in token unit
nativeAmount | string | real-time actual amount that will be undelegated in token unit
initialAmount | string | reuqested amount to undelegate in main token unit
amount | string | real-time actual amount that will be undelegated in main token unit

### Fetch Address Validators

> Request "Fetch Address Validators" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556018616498,
    "sig": {
        "r": "xtLflM34H+li5bMTktfKGXMLUXdNlboDAaYc/D4Oafg=",
        "s": "SKIwC0dGZihYvY56jJPCz1BIItF9ClVSxAU9XQmE/t0=",
        "v": 27
    },
    "result": [
        {
            "name": "node0",
            "address": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "pubkey": "cosmosvalconspub1zcjduepqmp75ajy7jm0mqttc2d23ks54dn83yfshxd3038r4ejldcjcd0atsnwxnyr",
            "jailed": false,
            "status": 2,
            "shares": "102009000.000000000000000000",
            "delegationAmount": "102009000",
            "minSelfDelegation": "1",
            "commission": {
                "rate": 0.1,
                "maxRate": 0.2,
                "maxChangeRate": 0.01,
                "updateAt": "2019-04-23T04:41:34.202Z"
            }
        },
        {
            "name": "node1",
            "address": "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu",
            "pubkey": "cosmosvalconspub1zcjduepqrjklsgkc8jeusfkjmrltlvz0s9cxe7vxr7uajpx2v0rfn0zvgtxqw5mwhj",
            "jailed": true,
            "status": 0,
            "shares": "2726262.626262626262626261",
            "delegationAmount": "2699000",
            "minSelfDelegation": "100000",
            "commission": {
                "rate": 0.1,
                "maxRate": 0.2,
                "maxChangeRate": 0.01,
                "updateAt": "2019-04-23T04:41:44.782Z"
            }
        }
    ]
}
```

This endpoint enables you to fetch validators an address delegated to.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/validators`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
name | string | validator name
address | string | validator's address
pubkey | string | validator's public key
jailed | boolean | if the validator is jailed
status | number | validator's status
shares | string | validator's shares
delegationAmount | string | amount delegated to validator
minSelfDelegation | string | amount delegated to validator by itself
commission | object | validator's commission
rate | float | validator's commission rate
maxRate | float | validator's commission maxRate
maxChangeRate | float | validator's commission maxChangeRate
updateAt | string | validator info update time

### Set Reward Address

> Request "Set Reward Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008386558,
    "sig": {
        "r": "4VxS8mGcX5Wml0B25qwT8B1vCUOZG1411KcKGOAiKYE=",
        "s": "fPkv8gI9HwqXecFeZaDvluaDF/TA+pN7At6cd9GBreI=",
        "v": 28
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "id": "21",
        "coinName": "ATOM",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "SYSTEM_CALL",
        "type": "ATOM",
        "coinType": "ATOM",
        "to": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "value": "0",
        "confirmations": 0,
        "create_at": 1556008386549,
        "update_at": 1556008386551,
        "action": "set-reward-address",
        "actionArgs": [
            "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u"
        ],
        "actionResults": [],
        "from": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "Cosmos",
        "sid": "RPamVnMkXe2aIgElAAAB"
    }
}
```

This endpoint enables you to set address to claim reward to.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/address`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.sequence | no | number | unique sequence
data.address | yes | string | reward address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again

### Fetch Reward Address

> Request "Fetch Reward Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556018632885,
    "sig": {
        "r": "2fEarBGRKLM9KnEyjZX12yRJxPNScszopIxrL/IraQk=",
        "s": "QH/oBrjOoDHqL3TyR36lVRgcz6L8PmM5ToeezrxJOeQ=",
        "v": 27
    },
    "result": {
        "address": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "namespace": "Cosmos",
        "sid": "h2112M6bFQZImpdUAAAB"
    }
}
```

This endpoint enables you to fetch reward address.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/address`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
address | string | the address that receives award

### Claim Reward

> Request "Claim Reward" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1554801579264,
    "sig": {
        "r": "1q87ktmqvcZrB8ID1nGSI3JlM0Zv5m12ldsiCNa5tVg=",
        "s": "XlxYTIf7xYV6J/SrrReVtJIY8Nip1lCsxSVTZSfhGUo=",
        "v": 28
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "id": "1969",
        "coinName": "ATOM",
        "txid": null,
        "meta": null,
        "state": "init",
        "bizType": "SYSTEM_CALL",
        "type": "ATOM",
        "coinType": "ATOM",
        "to": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "value": "0",
        "confirmations": 0,
        "create_at": 1554801579255,
        "update_at": 1554801579257,
        "action": "claim-reward",
        "actionArgs": [
            "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "cosmosvaloper1wpw2nrq850mwgrhteh3jwc2g7w9led2hervrfu"
        ],
        "actionResults": [],
        "from": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
        "n": 0,
        "fee": "0",
        "fees": [],
        "hash": "",
        "block": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "namespace": "Cosmos",
        "sid": "h-02Y6BZGFDuZyWzAAAF"
    }
}
```

This endpoint enables you to claim reward at all or specified validators.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/claim`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.sequence | no | number | unique sequence
data.validator | no | string | specified validator to claim reward from, claim reward from all if not passed

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again

### Fetch Overall Reward

> Request "Fetch Overall Reward" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556018660578,
    "sig": {
        "r": "KpNI22ydps6qHjkWlKTr+OntLzXPV8uM7ayjALtI6ls=",
        "s": "H4r6Hcm+qmN7wX1mdtzMGxIjdFBrOQgrjX4wjxRvQPg=",
        "v": 28
    },
    "result": [
        {
            "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "nativeName": "stake",
            "nativeAmount": "17.526742169806709000",
            "coinName": "ATOM",
            "amount": "0.00001752674216980671"
        }
    ]
}
```

This endpoint enables you to fetch reward status at all validators.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/profits`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Response Result*

Value | Type | Description
--------- | ------- | ---------
delegator | string | address that performs delegation
nativeName | string | token
nativeAmount | string | reward amount in token unit
coinName | string | main token
amount | string | reward amount in main token unit

### Reward At Validator

> Request "Reward At Validator" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1554191851801,
    "sig": {
        "r": "myLCMqne33PLyA6PKqHRS/PrAbhFWwuEOt/+RxjQIyk=",
        "s": "Tb3p62GM1aA1x986whPjFjh0K4PC01xNI+aBIElB8E4=",
        "v": 27
    },
    "result": [
        {
            "delegator": "cosmos1hrnl5pugag20gnu8zy3604jetqrjaf60czkl8u",
            "validator": "cosmosvaloper1z7jw2deueg37nd6v3flmn4wy2v3nhz55tsx4y9",
            "nativeName": "stake",
            "nativeAmount": "16.877883495144000000",
            "coinName": "ATOM",
            "amount": "0.000016877883495144"
        }
    ]
}
```

This endpoint enables you to fetch reward at specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/profits/:validator`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address
validator | validator

*Response Result*

Value | Type | Description
--------- | ------- | ---------
delegator | string | address that performs delegation
validator | string | validator delegated to
nativeName | string | token
nativeAmount | string | reward amount in token unit
coinName | string | main token
amount | string | reward amount in main token unit

# Sudo API

<aside class="notice">
All timestamps are in millisecond.
</aside>

<aside class="notice">
All main parameters in POST API should be wraped in "data" object
</aside>

<aside class="notice">
以下所有 API 均为全新的sudo级接口，必须使用配置在 Jadepool 后台的 sudoKey(公钥)所对应的私钥进行签名
</aside>

<aside class="notice">
以下所有 API 调用时使用的 appid 为 'sudo'，注：若已经存在名为 'sudo' 是应用，该应用将被强制无效化
</aside>

## Wallet

### 创建钱包

> Request "创建钱包" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596165678554,
    "sig": {
        "r": "+XC2zWsOeZeV6HNxMnxlhJrjvkUnsYiDvWWINtoDqW0=",
        "s": "IjXGQ7ItE3lO9+0TkOCIIhpSnCT5ZKEhednUBxewLmg=",
        "v": 27
    },
    "result": {
        "ok": true,
        "wallet": {
            "mode": "manual",
            "mainIndex": 80,
            "addrIndex": 0,
            "_id": "5f238e2eb195db852aa89c79",
            "name": "munual_3",
            "desc": "冷签",
            "alias": "manual_3",
            "meta": {
                "test": "test"
            },
            "create_at": "2020-07-31T03:21:18.513Z",
            "update_at": "2020-07-31T03:21:18.513Z",
            "__v": 0
        },
        "walletCreated": true
    }
}
```

创建钱包接口

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.name | yes | string | 钱包ID，必须全局唯一
data.mode | no | string | 模式，默认是manual，支持normal、safe、manual
data.alias | no | string | 别名
data.desc | no | string | 描述
data.meta | no | any | 额外信息，选填

*Response Result*

Value | Type | Description
--------- | ------- | ---------
mode | string | 钱包模式
mainIndex | number | 钱包主衍生路径
addrIndex | number | 充值地址衍生路径的index位置
chains | list | 钱包中链的配置信息
_id | string | 数据库对象_id
name | string | 钱包ID
alias | string | 别名
meta | any | 创建钱包时传入的meta信息
desc | string | 描述
create_at | string | 创建时间
update_at | string | 上次更新时间

### 查询钱包列表

> Request "查询钱包列表" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596167247056,
    "sig": {
        "r": "dI3ppAgcxDN1frCDhZjbfdi1nYfQvFiNeB+L9HFgT7Q=",
        "s": "Hc40Oa9SWY3/+CZVnlkevYx+nj8mE5igcOiTniLFgWc=",
        "v": 28
    },
    "result": {
        "list": [
            {
                "mode": "normal",
                "mainIndex": 6,
                "addrIndex": 10273,
                "_id": "5cfe1b5ab205e1f50df6dd3d",
                "name": "wallet_1",
                "desc": "desc",
                "create_at": "2019-06-10T08:56:58.958Z",
                "update_at": "2020-07-31T03:46:55.339Z",
                "version": "0.1.0",
                "alias": "wallet_1"
            }
        ],
        "pagination": {
            "page": 0,
            "limit": 1,
            "total": 11
        }
    }
}
```

查询已创建的钱包

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets?appid=sudo&page=0&limit=1`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
page | no | number | 页数，默认0
limit | no | number | 每页查询数量，默认20

*Response Result*

Value | Type | Description
--------- | ------- | ---------
list | list | 钱包列表，钱包具体信息参照“查询钱包具体信息”接口返回
pagination | object | 分页的结果

### 查询钱包具体信息

> Request "查询钱包具体信息" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596168440773,
    "sig": {
        "r": "0w2ic9KI+p/8XttsophO3VYwiblrJUTSLeHaNlOLpac=",
        "s": "LgmSYyWIc6EansiF/YpyygpcbfVUZXvmH3Z8MjARj5s=",
        "v": 28
    },
    "result": {
        "mode": "normal",
        "mainIndex": 0,
        "addrIndex": 21679,
        "chains": [
            {
                "source": {
                    "user": "seed",
                    "hot": "seed",
                    "cold": "seed"
                },
                "status": {
                    "enabled": true,
                    "coinsEnabled": [
                        "ETH",
                        "BCOIN"
                    ],
                    "coinsUsed": [
                        "ETH",
                        "BNB",
                        "P_USDC",
                        "P_USDTERC20"
                    ],
                    "coins": [
                        "ETH",
                        "BNB",
                        "P_USDC",
                        "P_USDTERC20"
                    ]
                },
                "inherit": false,
                "coins": [
                    "5d1b2de5924c17fa471177ba",
                    "5d3a5cf2924c17fa4791bf30",
                    "5d49289b6c065cddb991f9a6",
                    "5dd5f93e4c5456258d57d7ad",
                    "5e8c1ddb51eb57d7fc64c381",
                    "5f111b4a7a3a6f770152b1b4"
                ],
                "_id": "5d1b2de3924c17fa471176f3",
                "chainKey": "ETH",
                "wallet": "5d1b2de38e08804c9dbe57f4",
                "__v": 0,
                "data": {
                    "seedKey": "ETH",
                    "defaultHotDerivePath": "m/44'/0'/0'/2/0",
                    "hsmKey": "secp256k1-test",
                    "hotAddress": "0x0fe96bfffcd668a5c17ccf691aa63eddf2ef567f",
                    "hotAddressCachedAt": "2020-07-31T04:02:15.309Z",
                    "coldAddress": "0x3aaCBDa920eE8F2785A7fC1f08d1fC48B792e717",
                    "coldAddressCachedAt": "2020-07-31T04:03:15.269Z",
                    "generalOptions": {
                        "GasCoefficient": 0.5,
                        "GasPriceMaxNoDecimal": 200000000000
                    }
                },
                "id": "5d1b2de3924c17fa471176f3",
                "isFromTemplate": false,
                "config": {
                    "disabled": false,
                    "Chain": "Ethereum",
                    "CoreType": "ETH",
                    "ChainIndex": 60,
                    "WalletDefaults": {
                        "data": {
                            "seedKey": "ETH",
                            "defaultHotDerivePath": "m/44'/0'/0'/2/0"
                        },
                        "coinsEnabled": [
                            "ETH"
                        ]
                    },
                    "ledgerMode": "local",
                    "ledgerOptions": {
                        "file": "ethereum.js"
                    },
                    "walletModes": [
                        "normal",
                        "safe",
                        "manual"
                    ],
                    "addressMode": "multi",
                    "addressOnline": false,
                    "addressBizModes": [
                        "deposit",
                        "normal"
                    ],
                    "addressSources": [
                        "seed",
                        "offline_signer"
                    ],
                    "generalOptions": {
                        "disableWalletSimple": false,
                        "RescanMode": "block",
                        "waitingSendOrdersOnline": true,
                        "GasPriceMaxNoDecimal": 200000000000,
                        "GasCoefficient": 1,
                        "AffirmativeConfirmation": 3,
                        "FailedAffirmativeConfirmation": 6
                    },
                    "tokenExtendsEnabled": true,
                    "tokenTypes": [
                        "ETH",
                        "ERC20",
                        "DSToken"
                    ],
                    "tokenTemplatesDynamic": [
                        {
                            "type": "DSToken",
                            "path": "coin.GasLimit",
                            "rule": {
                                "type": "int",
                                "required": false,
                                "min": 50000,
                                "default": 100000
                            }
                        },
                        {
                            "type": "ERC20",
                            "path": "coin.GasLimit",
                            "rule": {
                                "type": "int",
                                "required": false,
                                "min": 50000,
                                "default": 100000
                            }
                        }
                    ],
                    "tokenTemplates": [
                        {
                            "path": "coin.address",
                            "rule": {
                                "type": "string",
                                "required": false,
                                "nocore": true,
                                "condRequired": true
                            }
                        },
                        "coin.Type",
                        "coin.Rate",
                        "coin.GasLimit",
                        "jadepool.HighWaterLevel",
                        "jadepool.SweepTo",
                        "jadepool.LowWaterLevel",
                        "jadepool.SweepToHotCap",
                        "jadepool.SendAgainCap",
                        "jadepool.BatchCount",
                        "jadepool.Airdrop.enabled",
                        "jadepool.Airdrop.Address"
                    ],
                    "contractWhiteList": [
                        "0x1522900b6dafac587d499a862861c0869be6e428",
                        "0x131a99859a8bfa3251d899f0675607766736ffae",
                        "0xd4f5bf184bebfd53ac276ec6e091d051d0ed459e",
                        "0x22941e38116f21923b7d82ed003375dd7fca9d45",
                        "0xd6a062cae6123c158768a5c444ca0896cc60d6b1",
                        "0x74d4344bf8cead0779e2041f19d2985985c67d7b",
                        "0xe5d7ccc5fc3b3216c4dff3a59442f1d83038468c",
                        "0xe7c2a3404f8dd0783719df1e3a27b814caca2009",
                        "0x08daf71efa7915807bc5675ad402989d7820cf6f",
                        "0x2daf5060a0ce14e2dc28011b9e3d89e964030837",
                        "0x969d96825eebc717e8019f0b8c481fb34f231f8d",
                        "0xc3a35b47e79967aab1e75881c2739fb0affb0558",
                        "0x26d37f2d8d9703255cd79a383c3f80b528b2342c"
                    ],
                    "closer": {
                        "softForkIgnoreCap": 10,
                        "previousBlocks": 10,
                        "scanBlockTaskCap": 500,
                        "scanAddressTaskCap": 10,
                        "doubleScanGap": 30
                    },
                    "explorers": [
                        "https://kovan.etherscan.io/tx/{txid}"
                    ],
                    "endpoints": [
                        "http://34.92.187.246:8547"
                    ],
                    "chainConfigTemplates": {
                        "endpoints": {
                            "mode": "stringMode",
                            "stringParams": {
                                "placeholder": "http://eth-testnet.cybex.io",
                                "rule": {
                                    "type": "string",
                                    "required": true
                                }
                            }
                        }
                    },
                    "node": [
                        {
                            "name": "ETH",
                            "MainNet": [
                                "http://47.52.191.84:8545/",
                                "http://47.92.89.120:8545/"
                            ],
                            "TestNet": [
                                "http://47.100.16.186:8547"
                            ]
                        }
                    ],
                    "explorer": {
                        "ETH": "https://kovan.etherscan.io/tx/{txid}"
                    },
                    "id": "5ca58972f2b8471f2a6e609c",
                    "key": "ETH"
                },
                "coinsEnabled": [
                    {
                        "name": "ETH",
                        "data": {
                            "seedKey": "ETH",
                            "defaultHotDerivePath": "m/44'/0'/0'/2/0",
                            "hsmKey": "secp256k1-test",
                            "hotAddress": "0x0fe96bfffcd668a5c17ccf691aa63eddf2ef567f",
                            "hotAddressCachedAt": "2020-07-31T04:02:15.309Z",
                            "coldAddress": "0x3aaCBDa920eE8F2785A7fC1f08d1fC48B792e717",
                            "coldAddressCachedAt": "2020-07-31T04:03:15.269Z",
                            "generalOptions": {
                                "GasCoefficient": 0.5,
                                "GasPriceMaxNoDecimal": 200000000000
                            }
                        },
                        "status": {
                            "depositDisabled": false,
                            "withdrawDisabled": false
                        },
                        "config": {
                            "disabled": false,
                            "coin": {
                                "Type": "ETH",
                                "Rate": 1000000000000000000,
                                "GasLimit": 21000
                            },
                            "jadepool": {
                                "HighWaterLevel": 10000,
                                "SweepTo": 10000,
                                "LowWaterLevel": 0,
                                "SweepToHotCap": 0,
                                "SendAgainCap": 5,
                                "BatchCount": 1,
                                "Sources": {
                                    "hot": "hsm_deep",
                                    "cold": "hsm_deep"
                                },
                                "RescanMode": "block,address",
                                "Airdrop": {
                                    "enabled": false,
                                    "Address": ""
                                }
                            },
                            "name": "ETH",
                            "id": "5ca5afe2c52a961a3b1ecb5e",
                            "key": "ETH"
                        },
                        "isFromTemplate": false,
                        "shortcut": {
                            "chainKey": "ETH",
                            "chain": "Ethereum",
                            "coreType": "ETH",
                            "tokenEnabled": true,
                            "depositWithdrawEnabled": true,
                            "type": "ETH",
                            "rate": 1000000000000000000
                        }
                    }
                ]
            }
        ],
        "_id": "5d1b2de38e08804c9dbe57f4",
        "name": "default",
        "desc": "Jadepool default wallet",
        "create_at": "2019-07-02T10:11:47.669Z",
        "update_at": "2020-07-31T04:03:15.270Z",
        "__v": 25,
        "version": "0.1.0"
    }
}
```

查询单个钱包的具体信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet?appid=sudo`

*Query Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
wallet | yes | string | 钱包ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
mode | string | 模式
mainIndex | number | 钱包主衍生路径
addrIndex | number | 充值地址衍生路径的index位置
chains | list | 钱包中链的配置信息
_id | string | 数据库对象ID
name | string | 钱包ID
alias | string | 别名
meta | any | 创建钱包时传入的meta信息
desc | string | 描述
create_at | string | 创建时间
update_at | string | 上次更新时间

### 创建充值地址

> Request "创建充值地址" API returns JSON structured like this:
```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596169526673,
    "sig": {
        "r": "cTmJcJT46Qr88I7qrchNa90+84UNsIelnFdBu1zwMCA=",
        "s": "bKpz+Rokh+vCTQtKZ9AMPktJ7xo/aMDcem05ln/optE=",
        "v": 28
    },
    "result": {
        "id": 2,
        "appid": "sudo",
        "wallet": "manual_1",
        "type": "ETH",
        "address": "0xBa4618A68f5fD24d97D37870b310F3728FBDed2a",
        "state": "used",
        "mode": "safe_deposit",
        "incoming": false,
        "incomings": [],
        "create_at": 1596169526625,
        "update_at": 1596169526661,
        "derivedPath": "m/44'/60/7700/0/2",
        "awaitPlans": [],
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

创建单个充值地址

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/address/:coinName/new`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.mode | yes | string | 地址模式：auto、deposit、deposit_memo、normal、safe_deposit
data.algorithm | no | string | 生成地址所用的算法：secp256k1、ed25519、sr25519
data.algorithmHash | no | string | 生成地址用的hash方法，默认default: default、legacy

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | number | 衍生路径中index标识位
appid | string | 应用appid
wallet | string | 钱包ID
type | string | 主币种类型
address | string | 生成的充值地址
state | string | 启用或弃用标识位
mode | string | 地址模式
derivedPath | string | 衍生路径

### 批量创建充值地址

> Request "批量创建充值地址" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596178700331,
    "sig": {
        "r": "3AyU17LcAhEynYVjvsOI2VXc0bLaX58VGtcisS7iVaU=",
        "s": "Cocxfd1vc7D9UdFYShvOhUoxUVduf6EtCfh292SksYo=",
        "v": 28
    },
    "result": [
        {
            "id": 37,
            "appid": "sudo",
            "wallet": "safe",
            "type": "ETH",
            "address": "0xe6Ec148ABC0d06D69f8Fd1Cd5dF8A4b87461cb09",
            "state": "used",
            "mode": "safe_deposit",
            "incoming": false,
            "incomings": [],
            "create_at": 1596178700307,
            "update_at": 1596178700317,
            "derivedPath": "m/44'/60'/3500'/0/37",
            "awaitPlans": [],
            "namespace": "ETH",
            "sid": "AViqqexi-V0lvr4BAAAI"
        },
        {
            "id": 38,
            "appid": "sudo",
            "wallet": "safe",
            "type": "ETH",
            "address": "0x5a1afE94E951eF98D2762A92eb3bce1D01F2d0b5",
            "state": "used",
            "mode": "safe_deposit",
            "incoming": false,
            "incomings": [],
            "create_at": 1596178700319,
            "update_at": 1596178700327,
            "derivedPath": "m/44'/60'/3500'/0/38",
            "awaitPlans": [],
            "namespace": "ETH",
            "sid": "AViqqexi-V0lvr4BAAAI"
        }
    ]
}
```

一次创建多个充值地址

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/address/:coinName/batch_new`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.amount | yes | number | 创建数量
data.mode | yes | string | 地址模式：auto、deposit、deposit_memo、normal、safe_deposit
data.algorithm | no | string | 生成地址所用的算法：secp256k1、ed25519、sr25519
data.algorithmHash | no | string | 生成地址用的hash方法，默认default: default、legacy

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | number | 衍生路径中index标识位
appid | string | 应用appid
wallet | string | 钱包ID
type | string | 主币种类型
address | string | 生成的充值地址
state | string | 启用或弃用标识位
mode | string | 地址模式
derivedPath | string | 衍生路径

### 查询充值地址信息

> Request "查询充值地址信息" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596179114768,
    "sig": {
        "r": "J+hB2N0f2qHvWPX0kgXGLrTrNXMPBXjD3sphE+lWKT8=",
        "s": "L7wCJoXibRqFRCbohnRF4euLzkhTigrwgolyGefJCnI=",
        "v": 28
    },
    "result": {
        "id": 28,
        "appid": "sudo",
        "wallet": "safe",
        "type": "ETH",
        "address": "0xAbb6a5747aCB1a6D92251Dc646FE542869Bca733",
        "state": "used",
        "mode": "safe_deposit",
        "incoming": false,
        "incomings": [],
        "create_at": 1592382772682,
        "update_at": 1592382772690,
        "derivedPath": "m/44'/60'/3500'/0/28",
        "awaitPlans": [],
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

查询充值地址的详细信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/address/:coinName/:address?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称
address | 充值地址

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | number | 衍生路径中index标识位
appid | string | 应用appid
wallet | string | 钱包ID
type | string | 主币种类型
address | string | 生成的充值地址
state | string | 启用或弃用标识位
mode | string | 地址模式
incoming | boolean | 地址中是否存在可用余额标识
incomings | list | 地址中存在可用余额的币种列表
awaitPlans | list | 授权订单列表
derivedPath | string | 衍生路径


### 查询充值地址余额

> Request "查询充值地址余额" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596179558954,
    "sig": {
        "r": "4bcfJuNrjqERahrdAj8a1w2zIu0rPGBeNXRvImrSloY=",
        "s": "GzEiyIkenTI9RlO/lUA1GmC+/e6AkVnO8aE4PNxDHHU=",
        "v": 27
    },
    "result": {
        "balance": "0.004074",
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

查询充值地址指定币种的余额信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/address/:coinName/:address/balance?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称
address | 充值地址

*Response Result*

Value | Type | Description
--------- | ------- | ---------
balance | string | 指定币种余额

### 查询订单详情

> Request "查询订单详情" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596179821894,
    "sig": {
        "r": "mjimL2h/rSLqpskdS75mAD+8ojSHnkIjCEzEf+Cwqsw=",
        "s": "OfzN3jj62bOg6NIVS+NoHQxnEqaoHgpeX5w5jrGYWcU=",
        "v": 27
    },
    "result": {
        "_id": "5f2153327eac44853d2798e8",
        "id": "130975",
        "coinName": "BCOIN",
        "txid": "0x040406b8feae2069907eb27da6f3bbe75be6bbfe85b8f79dc607131f5a197ea5",
        "meta": "1271",
        "appid": "sudo",
        "wallet": "manual_1",
        "sendMode": "manual_sign",
        "state": "done",
        "bizType": "WITHDRAW",
        "type": "ETH",
        "subType": "BCOIN",
        "coinType": "BCOIN",
        "from": "0x2884af7c92f24d8c753eaef6b8d11bed550db4a6",
        "froms": [
            "0x2884af7c92f24d8c753eaef6b8d11bed550db4a6"
        ],
        "to": "0x3aaCBDa920eE8F2785A7fC1f08d1fC48B792e717",
        "value": "0.001",
        "sequence": 1596019506,
        "confirmations": 3,
        "expectedConfirmations": 3,
        "create_at": 1596019506108,
        "update_at": 1596019759784,
        "action": "withdraw-from-hotwallet",
        "actionArgs": [],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0.000366894",
        "fees": [
            {
                "_id": "5f21542f60d4c3853c90853f",
                "amount": "0.000366894",
                "coinName": "ETH",
                "nativeAmount": "0",
                "nativeName": ""
            }
        ],
        "data": {
            "timestampBegin": 1596019614934,
            "timestampFinish": 1596019759783,
            "timestampHandle": 1596019612219
        },
        "pendingTags": [],
        "events": [],
        "hash": "0x040406b8feae2069907eb27da6f3bbe75be6bbfe85b8f79dc607131f5a197ea5",
        "txid_rawtx": "0x040406b8feae2069907eb27da6f3bbe75be6bbfe85b8f79dc607131f5a197ea5",
        "block": 19899336,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": []
    }
}
```

查询一条订单的具体信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/orders/:orderId?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
orderId | 订单号

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录 

### 查询订单附加流程

> Request "查询订单附加流程" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1599120766743,
    "sig": {
        "r": "2DwcfGNW+DFQ0PhyqZ/pgUj2IP8vuyYPMvaDlixcEWA=",
        "s": "G7YPgpzm1HzSsNtM/6prRoBHkgpnkErOegkjTyPL5jg=",
        "v": 28
    },
    "result": {
        "wallet": "user1",
        "category": "order-safe-deposit",
        "currentFlow": "aml-success",
        "lastFlow": "attempt-failed",
        "_id": "5f509ad332406319967afaef",
        "chainKey": "ETH",
        "coinName": "ETH",
        "related": "5f509acf79e8081994d37cfe",
        "data": {
            "targets": [
                "0xc5e33b3ced9af1c58875759ce1a179b9ee06761d"
            ],
            "passed": true,
            "retry": true
        },
        "order": {
            "_id": "5f50a57832406319967afb3c",
            "id": "1517",
            "coinName": "ETH",
            "txid": null,
            "meta": null,
            "appid": "",
            "wallet": "user1",
            "sendMode": "normal",
            "state": "init",
            "bizType": "SWEEP_INTERNAL",
            "type": "ETH",
            "coinType": "ETH",
            "from": "0x8b71f8d59fc673044e6e6283abd33600e827c31f",
            "froms": [
                "0x8b71f8d59fc673044e6e6283abd33600e827c31f"
            ],
            "to": "0x8767996c12dd811111313d3458a18e275fb63142",
            "value": "0.1",
            "sequence": 15991207605972806,
            "confirmations": 0,
            "expectedConfirmations": 0,
            "create_at": 1599120760597,
            "update_at": 1599120760599,
            "action": "sweep-to-hot",
            "actionArgs": [],
            "actionResults": [],
            "postHandlers": [],
            "n": 0,
            "fee": "0",
            "fees": [],
            "data": {
                "timestampBegin": 0,
                "timestampFinish": 0
            },
            "pendingTags": [],
            "events": [],
            "hash": "",
            "txid_rawtx": "",
            "block": -1,
            "blockHash": "",
            "blockTimestamp": -1,
            "extraData": "",
            "memo": "",
            "sendAgain": false,
            "relatedOrder": "5f509acf79e8081994d37cfe",
            "relatedIssueRecords": []
        },
        "flows": [
            {
                "dataAccepts": [
                    {
                        "type": "boolean",
                        "_id": "5f509ad332406319967afaf1",
                        "field": "passed",
                        "required": true
                    },
                    {
                        "type": "string",
                        "_id": "5f509ad332406319967afaf2",
                        "field": "refundAddress",
                        "requiredCond": {
                            "passed": false
                        }
                    }
                ],
                "transitions": [
                    {
                        "_id": "5f509ad332406319967afaf3",
                        "path": "data.passed",
                        "value": true,
                        "next": "aml-success"
                    },
                    {
                        "_id": "5f509ad332406319967afaf4",
                        "path": "data.passed",
                        "value": false,
                        "next": "aml-failure"
                    }
                ],
                "_id": "5f509ad332406319967afaf0",
                "key": "start"
            },
            {
                "dataAccepts": [],
                "transitions": [
                    {
                        "_id": "5f509ad332406319967afaf6",
                        "path": "order.state",
                        "value": "done",
                        "next": "end"
                    },
                    {
                        "_id": "5f509ad332406319967afaf7",
                        "path": "order.state",
                        "value": "failed",
                        "next": "attempt-failed"
                    }
                ],
                "_id": "5f509ad332406319967afaf5",
                "key": "aml-success"
            },
            {
                "dataAccepts": [],
                "transitions": [
                    {
                        "_id": "5f509ad332406319967afaf9",
                        "path": "order.state",
                        "value": "done",
                        "next": "end"
                    },
                    {
                        "_id": "5f509ad332406319967afafa",
                        "path": "order.state",
                        "value": "failed",
                        "next": "attempt-failed"
                    }
                ],
                "_id": "5f509ad332406319967afaf8",
                "key": "aml-failure"
            },
            {
                "dataAccepts": [
                    {
                        "type": "boolean",
                        "_id": "5f509ad332406319967afafc",
                        "field": "retry",
                        "required": true
                    }
                ],
                "transitions": [
                    {
                        "_id": "5f509ad332406319967afafd",
                        "path": "data.retry",
                        "value": true,
                        "next": "back"
                    },
                    {
                        "_id": "5f509ad332406319967afafe",
                        "path": "data.retry",
                        "value": false,
                        "next": "end"
                    }
                ],
                "_id": "5f509ad332406319967afafb",
                "key": "attempt-failed"
            },
            {
                "dataAccepts": [],
                "transitions": [],
                "_id": "5f509ad332406319967afaff",
                "key": "end"
            }
        ],
        "attempts": [
            {
                "_id": "5f50a55f32406319967afb3b",
                "flow": "aml-success",
                "order": "5f50a50532406319967afb38"
            }
        ],
        "createdAt": "2020-09-03T07:27:15.253Z",
        "updatedAt": "2020-09-03T08:12:40.606Z",
        "__v": 1,
        "finished": false,
        "id": "5f509ad332406319967afaef"
    }
}
```

查询safe或manual类型钱包的充值订单bizFlow流程的具体信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/orders/:orderId/bizflow?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
orderId | 订单号


*Response Result*

Value | Type | Description
--------- | ------- | ---------
wallet | string | 钱包ID
category | string | 类别
currentFlow | string | 当前bizFlow的位置
lastFlow | string | 上次bizFlow的位置
_id | string | 数据库_id
chainKey | string | 区块链的主key
coinName | string | 币种名
related | string | 充值订单对应数据库的_id
data | list | 审核数据记录
order | string | 审核后，汇总或退款的订单
flows | list | 整个flow流程
attempts | list | 订单失败后重新创建的记录
finished | boolean |  flow结束标识位


### 处理订单附加流程

> Request "处理订单附加流程" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596181686010,
    "sig": {
        "r": "z9TDQ24FAlrZo98a/yZYCKHo62mgRyZVPk+gT2sutHw=",
        "s": "OjBb+3DJXW2HWPu8voa13C9g7ekx4i1mEEeZUw4NsRM=",
        "v": 28
    },
    "result": {
        "wallet": "safe",
        "category": "order-safe-deposit",
        "currentFlow": "start",
        "lastFlow": null,
        "_id": "5f23cbf160d4c3853c908542",
        "chainKey": "ETH",
        "coinName": "ETH",
        "related": "5f23cbe8d22d60853a07213d",
        "data": {
            "targets": [
                "0x3aacbda920ee8f2785a7fc1f08d1fc48b792e717"
            ],
            "passed": true
        },
        "order": null,
        "flows": [
            {
                "dataAccepts": [
                    {
                        "type": "boolean",
                        "_id": "5f23cbf160d4c3853c908544",
                        "field": "passed",
                        "required": true
                    },
                    {
                        "type": "string",
                        "_id": "5f23cbf160d4c3853c908545",
                        "field": "refundAddress",
                        "requiredCond": {
                            "passed": false
                        }
                    }
                ],
                "transitions": [
                    {
                        "_id": "5f23cbf160d4c3853c908546",
                        "path": "data.passed",
                        "value": true,
                        "next": "aml-success"
                    },
                    {
                        "_id": "5f23cbf160d4c3853c908547",
                        "path": "data.passed",
                        "value": false,
                        "next": "aml-failure"
                    }
                ],
                "_id": "5f23cbf160d4c3853c908543",
                "key": "start"
            },
            {
                "dataAccepts": [],
                "transitions": [
                    {
                        "_id": "5f23cbf160d4c3853c908549",
                        "path": "order.state",
                        "value": "done",
                        "next": "end"
                    },
                    {
                        "_id": "5f23cbf160d4c3853c90854a",
                        "path": "order.state",
                        "value": "failed",
                        "next": "attempt-failed"
                    }
                ],
                "_id": "5f23cbf160d4c3853c908548",
                "key": "aml-success"
            },
            {
                "dataAccepts": [],
                "transitions": [
                    {
                        "_id": "5f23cbf160d4c3853c90854c",
                        "path": "order.state",
                        "value": "done",
                        "next": "end"
                    },
                    {
                        "_id": "5f23cbf160d4c3853c90854d",
                        "path": "order.state",
                        "value": "failed",
                        "next": "attempt-failed"
                    }
                ],
                "_id": "5f23cbf160d4c3853c90854b",
                "key": "aml-failure"
            },
            {
                "dataAccepts": [
                    {
                        "type": "boolean",
                        "_id": "5f23cbf160d4c3853c90854f",
                        "field": "retry",
                        "required": true
                    }
                ],
                "transitions": [
                    {
                        "_id": "5f23cbf160d4c3853c908550",
                        "path": "data.retry",
                        "value": true,
                        "next": "back"
                    },
                    {
                        "_id": "5f23cbf160d4c3853c908551",
                        "path": "data.retry",
                        "value": false,
                        "next": "end"
                    }
                ],
                "_id": "5f23cbf160d4c3853c90854e",
                "key": "attempt-failed"
            },
            {
                "dataAccepts": [],
                "transitions": [],
                "_id": "5f23cbf160d4c3853c908552",
                "key": "end"
            }
        ],
        "attempts": [],
        "createdAt": "2020-07-31T07:44:49.189Z",
        "updatedAt": "2020-07-31T07:48:06.004Z",
        "__v": 0,
        "finished": false,
        "id": "5f23cbf160d4c3853c908542"
    }
}
```

审核safe或manual类型钱包的充值订单

*HTTP Request*

`PATCH http://jadepool.example/api/v2/s/wallets/:wallet/orders/:orderId/bizflow?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
orderId | 订单号

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.passed | yes | boolean | 订单bizFlow在start状态时传入
data.refundAddress | no | string | 订单bizFlow在start状态时传入，passed=false时必传
data.retry | no | boolean | 订单bizFlow在attempt-failed状态时传入

*Response Result*

Value | Type | Description
--------- | ------- | ---------
wallet | string | 钱包ID
category | string | 类别
currentFlow | string | 当前bizFlow的位置
lastFlow | string | 上次bizFlow的位置
_id | string | 数据库_id
chainKey | string | 区块链的主key
coinName | string | 币种名
related | string | 充值订单对应数据库的_id
data | list | 审核数据记录
order | string | 审核后，汇总或退款的订单
flows | list | 整个flow流程
attempts | list | 订单失败后重新创建的记录
finished | boolean |  flow结束标识位

### 查询钱包余额

> Request "查询钱包余额" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596182155555,
    "sig": {
        "r": "rIob/hW9bMAeaHju4YU10rbqG6FTLsn2TsAEy10BRuY=",
        "s": "XLwZjWRnAMgtsBU6CUPBvlznSte2lW0/9rOwEu/fHP0=",
        "v": 28
    },
    "result": {
        "balance": "129.9851",
        "balanceAvailable": "121.6308",
        "balanceUnavailable": "8.3543",
        "addressAmount": 14,
        "hotAddress": "0x0fe96bfffcd668a5c17ccf691aa63eddf2ef567f",
        "coldAddress": "0x3aaCBDa920eE8F2785A7fC1f08d1fC48B792e717",
        "balanceCold": "344.17496134",
        "update_at": 1596182155549,
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

查询指定钱包某个币种的余额信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/tokens/:coinName/status?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名

*Response Result*

Value | Type | Description
--------- | ------- | ---------
balance | string | 钱包中地址总余额
balanceAvailable | string | 可用余额
balanceUnavailable | string | 不可用余额
addressAmount | number | 充值地址数量
hotAddress | string | 热主地址
coldAddress | string | 冷钱包地址
balanceCold | string | 冷钱包地址余额
update_at | number| 上次更新时间

### 查询钱包地址

> Request "查询钱包地址" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596182474014,
    "sig": {
        "r": "bKxjI68jj/NKkIikNrgFInJrz7pT3HUZgb1uxopi1Gg=",
        "s": "HUmbWe9KAVVNJecmyY2gxPB/1ZITmvuh4CMs7TFuLko=",
        "v": 28
    },
    "result": {
        "wallet": "manual",
        "chainKey": "ETH",
        "hot": [
            "0x0299bbe86e2a44a78ed7d4053a30a7e74378eae5"
        ],
        "cold": [
            "0x0299bbe86e2a44a78ed7d4053a30a7e74378eae5"
        ],
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

查询指定钱包的热主、冷地址等信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/tokens/:coinName/address?appid=sudo`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名

*Response Result*

Value | Type | Description
--------- | ------- | ---------
wallet | string | 钱包ID
chainKey | string | 区块链的基础key
hot | list | 热主地址
cold | list | 冷钱包地址

### 提现

> Request "提现" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596182945981,
    "sig": {
        "r": "K63Ij5SUnUToO9PujvGoX7fb4GSNOoiMCIS8OnzOIHM=",
        "s": "YYKd3KYZneOVt83an/mybvsRZrqr7G+h+SGgV/KqHqY=",
        "v": 27
    },
    "result": {
        "_id": "5f23d1a17eac44853d2798f3",
        "id": "130979",
        "coinName": "BCOIN",
        "txid": null,
        "meta": null,
        "appid": "sudo",
        "wallet": "manual_1",
        "sendMode": "manual_sign",
        "state": "init",
        "bizType": "WITHDRAW",
        "type": "ETH",
        "subType": "BCOIN",
        "coinType": "BCOIN",
        "from": "0x2884Af7C92f24d8C753EAef6B8d11beD550DB4A6",
        "froms": [
            "0x2884Af7C92f24d8C753EAef6B8d11beD550DB4A6"
        ],
        "to": "0x3aaCBDa920eE8F2785A7fC1f08d1fC48B792e717",
        "value": "0.001",
        "sequence": 1596182946,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1596182945963,
        "update_at": 1596182945967,
        "action": "withdraw-from-hotwallet",
        "actionArgs": [],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "pendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

提现操作

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/tokens/:coinName/withdraw`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.sequence | yes | number | 序列号
data.to | yes | string | 目标地址
data.value | yes | boolean | 订单金额
data.priority | no | number | 手续费率倍数,大于等于1，小于等于10

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 给订单设置自定义标签

> Request "给订单设置自定义标签" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1605532469083,
    "sig": {
        "r": "2oKRvlpISt7BDr3fOvrzJ6JXqgILsvS5B1damohCKzU=",
        "s": "fnRX3/u5+ySC03ogTSo8kWnSMzUp2SHzPU78iUD4Mg4=",
        "v": 28
    },
    "result": {
        "n": 1,
        "nModified": 1,
        "ok": 1
    }
}

```

给订单设置自定义标签

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/orders/:orderId/tags`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
orderId | 需要打标签的订单

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.key | yes | string | 全局唯一，关联一组标签
data.locales | yes | array | 标签内容
data.locales[n].lang | yes | enum | 标签语言，'zh-cn'或'en'
data.locales[n].label | yes | string | 标签

*Response Result*

Value | Type | Description
--------- | ------- | ---------
n | number | 选中的对象数
nModified | number | 修改的对象数
ok | number | 修改结果，1是成功，0是失败

## DS
### 验证是否可转账

> Request "验证是否可转账" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596186366424,
    "sig": {
        "r": "D1HKEWL+wIbWi5xyUbztuAhC87YpgqZXoLvRgVoKR3o=",
        "s": "athmEUn8eykLT0DEhI4HpVlt9mRfZhJIUTt5b5CLlPo=",
        "v": 27
    },
    "result": {
        "code": 0,
        "reason": "Valid",
        "namespace": "ETH",
        "sid": "4D3nqsKl3MVqmWxiAAAJ"
    }
}
```

检验钱包是否可以做转账操作

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/transfer_check`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.to | yes | string | 目标地址
data.value | yes | string | 要转账的金额

*Response Result*

Value | Type | Description
--------- | ------- | ---------
code | number | 响应码
reason | string | 说明

### 将钱包注册为投资人

> Request "将钱包注册为投资人" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596186564542,
    "sig": {
        "r": "ofbTnjrF4G1CTrepSkIY72+H9u8dL2skTVx0oJeONX8=",
        "s": "AEAc6a6J8F4idYtW0vYeBVbxR9ChT3TbD8/M0Yl/E3M=",
        "v": 28
    },
    "result": {
        "_id": "5f23dfc48f49a57714beed21",
        "id": "999",
        "coinName": "DSTOKEN",
        "txid": null,
        "meta": null,
        "appid": "sudo",
        "wallet": "MANUAL_OP",
        "sendMode": "normal",
        "state": "init",
        "bizType": "DS_METHOD",
        "type": "ETH",
        "subType": "DSTOKEN",
        "coinType": "DSTOKEN",
        "from": "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f",
        "froms": [
            "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f"
        ],
        "to": "0x8549A6843Bf411d811C6bBD0Ac50024f29F64672",
        "value": "0",
        "sequence": 1596186563994,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1596186564512,
        "update_at": 1596186564525,
        "action": "register_investor",
        "actionArgs": [
            "user20",
            "user20",
            "0x5a5dd27083a25cfec157d86f6a14453933cf0770"
        ],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "pendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "4D3nqsKl3MVqmWxiAAAJ"
    }
}
```

把钱包注册为投资人

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/register`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.investorID | yes | string | 投资人id
data.sequence | yes | number | 唯一序列号

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 添加到当前投资人

> Request "添加到当前投资人" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596186667086,
    "sig": {
        "r": "vpzV0lJ6xce/ixSsxHX5iCQcOTEL+fdGjwrPzaAkuQM=",
        "s": "A4A/w15q7Odq2n7XpGzXvX5M5sYiju/Lpn7kAq5FMKI=",
        "v": 28
    },
    "result": {
        "_id": "5f23e02b8f49a57714beed24",
        "id": "1001",
        "coinName": "DSTOKEN",
        "txid": null,
        "meta": null,
        "appid": "sudo",
        "wallet": "MANUAL_OP",
        "sendMode": "normal",
        "state": "init",
        "bizType": "DS_METHOD",
        "type": "ETH",
        "subType": "DSTOKEN",
        "coinType": "DSTOKEN",
        "from": "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f",
        "froms": [
            "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f"
        ],
        "to": "0x8549A6843Bf411d811C6bBD0Ac50024f29F64672",
        "value": "0",
        "sequence": 1596186666383,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1596186667077,
        "update_at": 1596186667078,
        "action": "add_address_to_investor",
        "actionArgs": [
            "0x98a7dDaaF2ebedd12569a081ec343ED7C059349e",
            "user1",
            "user1"
        ],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "pendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "4D3nqsKl3MVqmWxiAAAJ"
    }
}
```

把充值地址添加到当前投资人下

HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/add_address`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.address | yes | string | 充值地址
data.sequence | yes | number | 唯一序列号

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 从当前投资人移出

> Request "从当前投资人移出" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596186743827,
    "sig": {
        "r": "q9wOxqIlyrpGiknUdXMD7Xw8xFvPoKcqJjCf0G2v4oo=",
        "s": "T9b+N+GhXSFM7KuxvsNYDz+9VZsD/75MVaeiIzxd6W0=",
        "v": 28
    },
    "result": {
        "_id": "5f23e0778f49a57714beed25",
        "id": "1002",
        "coinName": "DSTOKEN",
        "txid": null,
        "meta": null,
        "appid": "sudo",
        "wallet": "MANUAL_OP",
        "sendMode": "normal",
        "state": "init",
        "bizType": "DS_METHOD",
        "type": "ETH",
        "subType": "DSTOKEN",
        "coinType": "DSTOKEN",
        "from": "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f",
        "froms": [
            "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f"
        ],
        "to": "0x8549A6843Bf411d811C6bBD0Ac50024f29F64672",
        "value": "0",
        "sequence": 1596186743475,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1596186743814,
        "update_at": 1596186743817,
        "action": "remove_address_from_investor",
        "actionArgs": [
            "0x6ce2a8920792a7b03c86c5c67a885971de808fa5",
            "user1",
            "user1"
        ],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "pendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "4D3nqsKl3MVqmWxiAAAJ"
    }
}
```

把充值地址从当前投资人下移出

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/remove_address`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.address | yes | string | 充值地址
data.sequence | yes | number | 唯一序列号

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 添加投资人国家

> Request "添加投资人国家" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596187009717,
    "sig": {
        "r": "+pVtZNTA+FBL8hN45G9woBA4MONjkjq+BSZHrzp7sM4=",
        "s": "XAeizGyp3FRCxrd6xZzUXXCIy9tc8ZGRnNbE9ZJqOnI=",
        "v": 28
    },
    "result": {
        "_id": "5f23e1818f49a57714beed26",
        "id": "1005",
        "coinName": "DSTOKEN",
        "txid": null,
        "meta": null,
        "appid": "sudo",
        "wallet": "MANUAL_OP",
        "sendMode": "normal",
        "state": "init",
        "bizType": "DS_METHOD",
        "type": "ETH",
        "subType": "DSTOKEN",
        "coinType": "DSTOKEN",
        "from": "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f",
        "froms": [
            "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f"
        ],
        "to": "0x8549A6843Bf411d811C6bBD0Ac50024f29F64672",
        "value": "0",
        "sequence": 1596187008892,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1596187009704,
        "update_at": 1596187009709,
        "action": "set_investor_country",
        "actionArgs": [
            "user20",
            "HK",
            "user20"
        ],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "pendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "4D3nqsKl3MVqmWxiAAAJ"
    }
}
```

给投资人添加国家属性

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/set_country`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.countryCode | yes | string | 国家简码
data.sequence | yes | number | 唯一序列号


*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 添加投资人属性

> Request "添加投资人属性" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596187035318,
    "sig": {
        "r": "7TOE6KAUhg3XeEYQ5L2ibXlioYeJHIPyja2nOonPK2c=",
        "s": "QKpwmP5tI18LV52uWde3c5I8uOgo8xzO6Qn246OvHsI=",
        "v": 28
    },
    "result": {
        "_id": "5f23e19b8f49a57714beed27",
        "id": "1006",
        "coinName": "DSTOKEN",
        "txid": null,
        "meta": null,
        "appid": "sudo",
        "wallet": "MANUAL_OP",
        "sendMode": "normal",
        "state": "init",
        "bizType": "DS_METHOD",
        "type": "ETH",
        "subType": "DSTOKEN",
        "coinType": "DSTOKEN",
        "from": "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f",
        "froms": [
            "0xfa5e799713546b2b9DeBF2A611d4B695B628a20f"
        ],
        "to": "0x8549A6843Bf411d811C6bBD0Ac50024f29F64672",
        "value": "0",
        "sequence": 1596187034769,
        "confirmations": 0,
        "expectedConfirmations": 0,
        "create_at": 1596187035305,
        "update_at": 1596187035309,
        "action": "set_investor_attribute",
        "actionArgs": [
            "user20",
            "1",
            "1",
            "1596187034769",
            "0xC5C22849CEB17E3518de508254504Ba34dCB6E0",
            "user20"
        ],
        "actionResults": [],
        "postHandlers": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
        "pendingTags": [],
        "events": [],
        "hash": "",
        "txid_rawtx": "",
        "block": -1,
        "blockHash": "",
        "blockTimestamp": -1,
        "extraData": "",
        "memo": "",
        "sendAgain": false,
        "relatedOrder": "",
        "relatedIssueRecords": [],
        "namespace": "ETH",
        "sid": "4D3nqsKl3MVqmWxiAAAJ"
    }
}
```

给投资人添加属性

*HTTP Request*

`POST http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/set_attribute`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.attributeID | yes | number | 
data.value | yes | string | 
data.expiry | yes | string | 
data.proofHash | yes | string | 
data.sequence | yes | number | 唯一序列号


*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list | 
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 获取钱包的注册信息

> Request "获取钱包的注册信息" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1597386711337,
    "sig": {
        "r": "oyw9l4iBl7V6mBXyGgfKYXBVUunH97ERiWs4XknvYrE=",
        "s": "f4DZHBpdQfoC9s8QNBnTy2JlWN5G5vCkkMSUJad5ofk=",
        "v": 28
    },
    "result": {
        "investorID": "user20",
        "countryCode": "HK",
        "attributes": {
            "0": {
                "expiry": "1596426046699",
                "proofHash": "0xC5C22849CEB17E3518de508254504Ba34dCB6E2",
                "value": "2"
            },
            "1": {
                "expiry": "1596424378444",
                "proofHash": "0xC5C22849CEB17E3518de508254504Ba34dCB6E0",
                "value": "1"
            },
            "2": {
                "expiry": "1597376046112",
                "proofHash": "0xC5C22849CEB17E3518de508254504Ba34dCB6E2",
                "value": "0"
            },
            "4": {
                "expiry": "1597376055762",
                "proofHash": "0xC5C22849CEB17E3518de508254504Ba34dCB6E2",
                "value": "2"
            },
            "5": {
                "expiry": "1597376076488",
                "proofHash": "0xC5C22849CEB17E3518de508254504Ba34dCB6E2",
                "value": "9"
            },
            "8": {
                "expiry": "1597376066647",
                "proofHash": "0xC5C22849CEB17E3518de508254504Ba34dCB6E2",
                "value": "2"
            }
        },
        "namespace": "ETH",
        "sid": "9rZHtghw7vHbnUjBAAAC"
    }
}
```

获取钱包的注册信息

*HTTP Request*

`GET http://jadepool.example/api/v2/s/wallets/:wallet/ds/:coinName/reg_info`

*Query Parameters*

Parameter | Description 
--------- | --------- 
wallet | 钱包ID
coinName | 币种名称

*Response Result*

Value | Type | Description
--------- | ------- | ---------
investorID | string | 投资人id

## Utilities

### 验证地址有效性

> Request "验证地址有效性" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596183478631,
    "sig": {
        "r": "0FM5+Dy7yNEAGWYYXWc6GXw1udDyOzpJtUglhWd7GqA=",
        "s": "ECE/FIQIh8cKuwycleE0DTImDnOf2DZ0VP1DlP0ByqU=",
        "v": 27
    },
    "result": {
        "address": "0x3aaCBDa920eE8F2785A7fC1f08d1fC48B792e717",
        "valid": true,
        "namespace": "ETH",
        "sid": "AViqqexi-V0lvr4BAAAI"
    }
}
```

验证地址是否合法

*HTTP Request*

`POST http://jadepool.example/api/v2/s/utilities/tokens/:coinName/address_verify`

*Query Parameters*

Parameter | Description 
--------- | --------- 
coinName | 币种名称

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.address | yes | string | 充值地址

*Response Result*

Value | Type | Description
--------- | ------- | ---------
address | string | 充值地址
valid | boolean | 是否合法

### 设置回调地址

> Request "设置回调地址" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1596183742084,
    "sig": {
        "r": "ozMYHAcy09g5oN7PV2Efi/ez3xbI+2NxtuhS7cU2SiA=",
        "s": "AMBlR8DAXnSjk0T+L5zmA0DEME9py8iB2UCzyQ5N2mg=",
        "v": 28
    },
    "result": {
        "isok": true
    }
}
```

设置全局回调地址

*HTTP Request*

`POST http://jadepool.example/api/v2/s/utilities/set_callback`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.audit | no | string | 审计回调地址
data.orderDefault | no | string | 默认回调地址
data.order.WITHDRAW | no | string | 充值订单回调地址

# MPC API

## 托管单元

### 注册公钥

> Request "注册公钥" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695319833,
  "sig": {
    "r": "gtXA4NL9etHtxg3f8nUar5jBmq3CNMNsBCwIoEK4xb8=",
    "s": "ADjm+yGuK0qAI+wsh9YL9Nt/pPu33/03XIqXHEzS0ag=",
    "v": 27
  },
  "result": {
    "algorithm": "secp256k1",
    "_id": "606c1257733901019076dcef",
    "addresses": [],
    "appid": "custom_app",
    "wallet": "custom",
    "publicKey": "033fdc284b5b980927c7a011ff0506aa7208ea327b3e905328f038bf259d2fbc75",
    "alias": "alice",
    "meta": {
      "a": 1
    },
    "create_at": "2021-04-06T07:48:39.828Z",
    "update_at": "2021-04-06T07:48:39.828Z",
    "__v": 0
  }
}
```

注册公钥

*HTTP Request*

`POST http:/{ip}/api/v2/units`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.pubKey | yes | string | 需要注册的公钥
data.alias | no | string | 选填，注册的别名
data.algorithm | no | string | 私钥到公钥的算法（secp256k1、ecdsa、ed25519、sr25519）

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | resource ID，唯一ID，可用以查询
pubKey | string | 需要注册的公钥
alias | string | 请求时传入值
addresses | array | 注册公钥已衍生的地址的ObjectID
wallet | string | 注册公钥的（MPC）钱包
algorithm | string | 传入的私钥到公钥的算法

### 查询注册公钥列表

> Request "查询注册公钥列表" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695346131,
  "sig": {
    "r": "RoZ8C6fe8oihxS8IS8YeN4tGCqR9Y43PDEJULWCAOBc=",
    "s": "SXYDRGi9ZJUsU/Vl5fKUdCgfVK3kGyK7VWSC/Klw5Xo=",
    "v": 27
  },
  "result": {
    "list": [
      {
        "algorithm": "secp256k1",
        "_id": "606c1257733901019076dcef",
        "addresses": [],
        "appid": "custom_app",
        "wallet": "custom",
        "publicKey": "033fdc284b5b980927c7a011ff0506aa7208ea327b3e905328f038bf259d2fbc75",
        "alias": "alice",
        "meta": {
          "a": 1
        },
        "create_at": "2021-04-06T07:48:39.828Z",
        "update_at": "2021-04-06T07:48:39.828Z"
      }
    ],
    "pagination": {
      "page": 0,
      "limit": 10,
      "total": 1
    }
  }
}
```

查询注册公钥列表

*HTTP Request*

`GET http://{ip}/api/v2/units?page={page}&limit={limit}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
page | no | number | 查询页数
limit | no | number | 每页查询数量

*Response Result*

Value | Type | Description
--------- | ------- | ---------
list[n]._id | string | 注册单元的唯一ID resource ID
list[n].pubKey | string | 需要注册的公钥
list[n].alias | string | 请求时传入值
list[n].addresses | array | 注册公钥已衍生的地址的ObjectID
list[n].wallet | string | 注册公钥的（MPC）钱包### 注册公钥
list[n].appid | string | 应用ID
list[n].algorithm | string | 私钥到公钥的算法
pagination.page | number | 查询所在页数
pagination.limit | number | 每页查询数量
pagination.total | number | 符合条件的条目总数

### 查询注册公钥信息

> Request "查询注册公钥信息" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695360816,
  "sig": {
    "r": "2ss/SK084+L0Czz5eg+KCTUcEv+d3jQJ4SRMa/O1+6o=",
    "s": "BwMJ0/VnDZrZvw7+bspxMs8AFCITZNO35BjdCvIZ/xM=",
    "v": 27
  },
  "result": {
    "algorithm": "secp256k1",
    "_id": "606c1257733901019076dcef",
    "addresses": [],
    "appid": "custom_app",
    "wallet": "custom",
    "publicKey": "033fdc284b5b980927c7a011ff0506aa7208ea327b3e905328f038bf259d2fbc75",
    "alias": "alice",
    "meta": {
      "a": 1
    },
    "create_at": "2021-04-06T07:48:39.828Z",
    "update_at": "2021-04-06T07:48:39.828Z",
    "__v": 0
  }
}
```

查询注册公钥信息

*HTTP Request*

`GET http:/{ip}/api/v2/units/{ResourceID}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 注册单元的唯一ID resource ID
pubKey | string | 需要注册的公钥
alias | string | 请求时传入值
addresses | array | 注册公钥已衍生的地址的ObjectID
algorithm | string | 私钥到公钥的算法
wallet | string | 注册公钥的（MPC）钱包
appid | string | 应用ID

### 修改注册公钥信息

> Request "修改注册公钥信息" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617696633108,
  "sig": {
    "r": "jObqwfW/o+CzaSqOO44Y4DT20qZmXedurFgvkjAKYn8=",
    "s": "FRZ7X04xjrfnEguXy+ZMtvv1PouqVlO75D/SJ/3mjio=",
    "v": 28
  },
  "result": {
    "algorithm": "secp256k1",
    "_id": "606c1257733901019076dcef",
    "addresses": [
      {
        "_id": "606c12930b207604bc55a029",
        "chainKey": "ETH",
        "instance": "606c12930b207604bc55a028"
      }
    ],
    "appid": "custom_app",
    "wallet": "custom",
    "publicKey": "033fdc284b5b980927c7a011ff0506aa7208ea327b3e905328f038bf259d2fbc75",
    "alias": "alice",
    "meta": {
      "a": 3
    },
    "create_at": "2021-04-06T07:48:39.828Z",
    "update_at": "2021-04-06T08:10:33.104Z",
    "__v": 0
  }
}
```

修改注册公钥信息

*HTTP Request*

`PATCH http:/{ip}/api/v2/units/{ResourceID}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
data.alias | yes | string | 目前只能修改别名

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 注册单元的唯一ID resource ID
pubKey | string | 需要注册的公钥
alias | string | 别名
addresses | array | 注册公钥已衍生的地址的ObjectID
algorithm | string | 私钥到公钥的算法
wallet | string | 注册公钥的（MPC）钱包
appid | string | 应用ID

### 创建地址

> Request "创建地址" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695379654,
  "sig": {
    "r": "FfDsD3F65s12UQ44x4gA0W6ZGMolMeqozkM+jtaGywY=",
    "s": "Huw9E+pyAvjfEkILfLLhGuYTVIGIizEOaIYy6I4hDmc=",
    "v": 28
  },
  "result": {
    "algorithmHash": "default",
    "wallet": "custom",
    "incoming": false,
    "incomings": [],
    "tags": [],
    "functional": false,
    "dusts": [],
    "state": "used",
    "awaitPlans": [],
    "assign_at": 1617695379640,
    "_id": "606c12930b207604bc55a028",
    "appid": "custom_app",
    "type": "ETH",
    "subType": "ETH",
    "mode": "normal",
    "algorithm": "secp256k1",
    "id": 1,
    "derivedPath": "",
    "address": "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9",
    "create_at": 1617695379642,
    "update_at": 1617695379646,
    "unit": "606c1257733901019076dcef",
    "__v": 0,
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

创建地址

*HTTP Request*

`POST http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 创建地址的token
data.algorithmHash | no | string | 选填，目前传default就可以

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 地址的唯一ObjectID
wallet | string | 注册公钥、生成地址的（MPC）钱包
appid | string | 应用ID
subType | string | token
address | string | 地址
unit | string | 注册单元的唯一ID resource ID

### 查询地址

> Request "查询地址" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695411724,
  "sig": {
    "r": "Lrv/KIVbLJROLjOvpWhVzTzj0E9NzvC4gRVgqQz/0ew=",
    "s": "Okh/d/pnHmUZJjEuyD9xAzxxkDPCDcn01Jd39HZvhaM=",
    "v": 28
  },
  "result": {
    "algorithmHash": "default",
    "wallet": "custom",
    "incoming": false,
    "incomings": [],
    "tags": [],
    "functional": false,
    "dusts": [],
    "state": "used",
    "awaitPlans": [],
    "assign_at": 1617695379640,
    "_id": "606c12930b207604bc55a028",
    "appid": "custom_app",
    "type": "ETH",
    "subType": "ETH",
    "mode": "normal",
    "algorithm": "secp256k1",
    "id": 1,
    "derivedPath": "",
    "address": "0x829a4d2a502ab39cc7641d7752ee88c727b41cb9",
    "create_at": 1617695379642,
    "update_at": 1617695379646,
    "unit": "606c1257733901019076dcef",
    "__v": 0,
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

查询地址

*HTTP Request*

`GET http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 查询地址的token

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 地址的唯一ObjectID
wallet | string | 注册公钥、生成地址的（MPC）钱包
appid | string | 应用ID
subType | string | token
address | string | 地址
unit | string | 注册单元的唯一ID resource ID

### 查询地址相关订单

> Request "查询地址相关订单" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695595541,
  "sig": {
    "r": "U7P1fDVTntuQ0Dpl7E0PdXOxoUEcfJYSwsQ57VzxEg0=",
    "s": "HMsBS92+n1cDfIJ8iwUXtSWRVleEKkxKBJ5qG4rNco4=",
    "v": 27
  },
  "result": [
    {
      "_id": "606c1352b10f4804ae3167f1",
      "id": "54",
      "coinName": "ETH",
      "txid": "0x1b10841c6a16c85b3748695bd890908e3998d71b809914fe2ab1f8c826754e66",
      "meta": null,
      "appid": "custom_app",
      "wallet": "custom",
      "sendMode": "normal",
      "state": "pending",
      "bizType": "DEPOSIT",
      "type": "ETH",
      "coinType": "ETH",
      "from": "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f",
      "froms": [
        "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f"
      ],
      "to": "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9",
      "value": "0.1",
      "sequence": 16176955705364982,
      "confirmations": 14,
      "expectedConfirmations": 20,
      "create_at": 1617695570536,
      "update_at": 1617695595377,
      "actionArgs": [],
      "actionResults": [],
      "postHandlers": [],
      "bizFlows": [],
      "tags": [],
      "n": 0,
      "fee": "0.000315",
      "fees": [
        {
          "_id": "606c136bacf9f804af38890c",
          "amount": "0.000315",
          "coinName": "ETH",
          "nativeAmount": "0",
          "nativeName": ""
        }
      ],
      "data": {
        "timestampBegin": 1617695570537,
        "timestampFinish": 0,
        "timestampHandle": 1617695570537
      },
      "options": {},
      "pendingTags": [],
      "sendingTags": [],
      "stateTags": [],
      "validationResults": [],
      "events": [],
      "inputs": [],
      "hash": "0x1b10841c6a16c85b3748695bd890908e3998d71b809914fe2ab1f8c826754e66",
      "txid_rawtx": "",
      "block": 24172701,
      "blockHash": "",
      "blockTimestamp": -1,
      "extraData": "",
      "memo": "",
      "sendAgain": false,
      "relatedOrder": "",
      "relatedIssueRecords": [],
      "namespace": "ETH",
      "sid": "-1IenchlwJXm5e2eAAAE"
    },
    {
      "_id": "606c13460b207604bc55a02a",
      "id": "53",
      "coinName": "ETH",
      "txid": null,
      "meta": null,
      "appid": "custom_app",
      "wallet": "custom",
      "sendMode": "custom_sign",
      "state": "init",
      "bizType": "WITHDRAW",
      "type": "ETH",
      "coinType": "ETH",
      "from": "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9",
      "froms": [
        "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9"
      ],
      "to": "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f",
      "value": "0.002",
      "sequence": 1617695558,
      "confirmations": 0,
      "expectedConfirmations": 0,
      "create_at": 1617695558556,
      "update_at": 1617695559236,
      "action": "withdraw-from-addr",
      "actionArgs": [],
      "actionResults": [],
      "postHandlers": [],
      "bizFlows": [
        "606c13460b207604bc55a02b"
      ],
      "tags": [],
      "n": 0,
      "fee": "0",
      "fees": [],
      "data": {
        "timestampBegin": 0,
        "timestampFinish": 0
      },
      "options": {
        "unitId": "606c1257733901019076dcef"
      },
      "pendingTags": [],
      "sendingTags": [],
      "stateTags": [],
      "validationResults": [],
      "events": [],
      "inputs": [],
      "hash": "",
      "txid_rawtx": "",
      "block": -1,
      "blockHash": "",
      "blockTimestamp": -1,
      "extraData": "",
      "memo": "",
      "sendAgain": false,
      "relatedOrder": "",
      "relatedIssueRecords": [],
      "namespace": "ETH",
      "sid": "-1IenchlwJXm5e2eAAAE"
    }
  ]
}
```

查询地址相关订单

*HTTP Request*

`GET http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}/orders/{state}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 查询地址的token
state | no | string | 订单状态，选填。如果只需查询某一状态的订单，传入状态（init,holding,online,pending,done,failed,cancelled）

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list |
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 查询地址余额

> Request "查询地址余额" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695511824,
  "sig": {
    "r": "F9NToE/+JamPmC4nBqm+13xC7K5CIYO8fxUDcbQirwQ=",
    "s": "LZ86aDC+astqztaAUsCz6Ei0N9yJHUn6v+JoOIQ/maA=",
    "v": 28
  },
  "result": {
    "balance": "0.1",
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

查询地址余额

*HTTP Request*

`GET http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}/balance`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 查询地址的token

*Response Result*

Value | Type | Description
--------- | ------- | ---------
balance | string | 地址余额

### 强制更新地址余额

> Request "强制更新地址余额" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695496440,
  "sig": {
    "r": "CvvYeJneY00UbEqtJS/6cHTzD0ggNHtGCWCqafkkWJU=",
    "s": "NGkUB4aseN2TrQbqdg+cl+mk3wLkFXEji7Shp70FAl0=",
    "v": 27
  },
  "result": {
    "ok": true,
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

强制更新地址余额

*HTTP Request*

`POST http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}/active`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 地址token
data | yes | object | 传空对象即可

*Response Result*

Value | Type | Description
--------- | ------- | ---------
ok | boolean | true表示请求成功

### 发起提现

> Request "发起提现" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617696974795,
  "sig": {
    "r": "cg7b80V6KPyJPW6IQm5I/Ga6A6bc6xdU1+tcEoEJ1eQ=",
    "s": "MIH+w9ak4hDa8/xZkBkRilJATWM8pZoEWhGRofMZyuQ=",
    "v": 27
  },
  "result": {
    "_id": "606c18ce11806dc32f818b04",
    "id": "1207",
    "coinName": "ETH",
    "txid": null,
    "meta": null,
    "appid": "custom_app",
    "wallet": "mpc",
    "sendMode": "custom_sign",
    "state": "init",
    "bizType": "WITHDRAW",
    "type": "ETH",
    "coinType": "ETH",
    "from": "0x3B207951C2c906ed2F246A875275A9C70a22f2A4",
    "froms": [
      "0x3B207951C2c906ed2F246A875275A9C70a22f2A4"
    ],
    "to": "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f",
    "value": "0.002",
    "sequence": 1617696974,
    "confirmations": 0,
    "expectedConfirmations": 0,
    "create_at": 1617696974676,
    "update_at": 1617696974689,
    "action": "withdraw-from-addr",
    "actionArgs": [],
    "actionResults": [],
    "postHandlers": [],
    "bizFlows": [],
    "tags": [],
    "n": 0,
    "fee": "0",
    "fees": [],
    "data": {
      "timestampBegin": 0,
      "timestampFinish": 0
    },
    "options": {
      "feePrice": 10000000000,
      "gasLimit": 21000,
      "buildingTimeout": 15000,
      "unitId": "602356b0f7b2d26fe4e8bdb1"
    },
    "pendingTags": [],
    "sendingTags": [],
    "stateTags": [],
    "validationResults": [],
    "events": [],
    "inputs": [],
    "hash": "",
    "txid_rawtx": "",
    "block": -1,
    "blockHash": "",
    "blockTimestamp": -1,
    "extraData": "",
    "memo": "",
    "sendAgain": false,
    "relatedOrder": "",
    "relatedIssueRecords": [],
    "namespace": "ETH",
    "sid": "0yRVwFz4Fbn6JvNnAAAE"
  }
}
```

发起提现

*HTTP Request*

`POST http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}/withdraw`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 地址token
data.sequence | yes | number | 请求的唯一序列号
data.to | yes | number | 提现接收地址
data.value | yes | number | 提现金额
data.advanced.feePrice | no | number | 交易手续费，必须是（最小单位的）整数
data.advanced.gasLimit | no | number | 以太坊系交易设置
data.advanced.buildingTimeout | no | number | 如果超过该时间段构建交易没有完成就取消订单
data.advanced.priority | no | number | 手续费倍率，和feePrice、gasLimit，只能二选一传入

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list |
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 发起合约交易

> Request "发起合约交易" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695725450,
  "sig": {
    "r": "1Fzg6L2pwr2hn5o4Bd5X8ZWxaQx6DTybeEGMksLSUos=",
    "s": "UZ9sOBHe3QrCtC880lFYw8wIReoPdJ9NoNVBgjJhBeM=",
    "v": 28
  },
  "result": {
    "_id": "606c13ed0b207604bc55a053",
    "id": "55",
    "coinName": "ETH",
    "txid": null,
    "meta": null,
    "appid": "custom_app",
    "wallet": "custom",
    "sendMode": "custom_sign",
    "state": "init",
    "bizType": "SYSTEM_CALL",
    "type": "ETH",
    "coinType": "ETH",
    "from": "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9",
    "froms": [
      "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9"
    ],
    "to": "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f",
    "value": "0",
    "sequence": 1617695725,
    "confirmations": 0,
    "expectedConfirmations": 0,
    "create_at": 1617695725403,
    "update_at": 1617695725406,
    "action": "general:general:UNISWAP swapETHForExactTokens",
    "actionArgs": [
      "hello"
    ],
    "actionResults": [],
    "postHandlers": [],
    "bizFlows": [],
    "tags": [],
    "n": 0,
    "fee": "0",
    "fees": [],
    "data": {
      "timestampBegin": 0,
      "timestampFinish": 0
    },
    "options": {
      "unitId": "606c1257733901019076dcef"
    },
    "pendingTags": [],
    "sendingTags": [],
    "stateTags": [],
    "validationResults": [],
    "events": [],
    "inputs": [],
    "hash": "",
    "txid_rawtx": "",
    "block": -1,
    "blockHash": "",
    "blockTimestamp": -1,
    "extraData": "",
    "memo": "",
    "sendAgain": false,
    "relatedOrder": "",
    "relatedIssueRecords": [],
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

发起合约交易

*HTTP Request*

`POST http:/{ip}/api/v2/units/{ResourceID}/addresses/{token}/invoke`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
ResourceID | yes | string | 注册公钥时返回的唯一ID
token | yes | string | 地址token
data.sequence | yes | number | 请求的唯一序列号
data.to | yes | number | 交易接收地址
data.value | yes | number | 交易金额
data.input | yes | number | 以太坊系交易的input data
data.action | yes | number | 该项仅用于后台管理系统显示，传入便于阅读的值即可

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list |
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

## 订单相关

### 查询订单

> Request "查询订单" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695936950,
  "sig": {
    "r": "J/g2BKbw8o6rLO4DUfcUWzUdtdKDRb9IeckomuLvyd0=",
    "s": "H/Hz0SwHmKSEh7BuzOZ/c3eD6P9liLsqaolQkc3EfOI=",
    "v": 28
  },
  "result": {
    "_id": "606c13460b207604bc55a02a",
    "id": "53",
    "coinName": "ETH",
    "txid": null,
    "meta": null,
    "appid": "custom_app",
    "wallet": "custom",
    "sendMode": "custom_sign",
    "state": "cancelled",
    "bizType": "WITHDRAW",
    "type": "ETH",
    "coinType": "ETH",
    "from": "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9",
    "froms": [
      "0x829A4D2a502ab39CC7641D7752eE88C727B41CB9"
    ],
    "to": "0x04e98D7A5ca93d3DdcF88DbB0f9Dde1E2910061f",
    "value": "0.002",
    "sequence": 1617695558,
    "confirmations": 0,
    "expectedConfirmations": 0,
    "create_at": 1617695558556,
    "update_at": 1617695713143,
    "action": "withdraw-from-addr",
    "actionArgs": [],
    "actionResults": [],
    "postHandlers": [],
    "bizFlows": [
      {
        "wallet": "custom",
        "category": "custom-withdraw",
        "currentFlow": "end",
        "lastFlow": "cancelled",
        "_id": "606c13460b207604bc55a02b",
        "chainKey": "ETH",
        "coinName": "ETH",
        "related": "606c13460b207604bc55a02a",
        "data": {
          "unitId": "606c1257733901019076dcef",
          "building_trying": false,
          "building_error": null
        },
        "attempts": [
          {
            "_id": "606c1347acf9f804af388907",
            "flow": "unbuilt",
            "transaction": "606c1347acf9f804af388905"
          }
        ],
        "logs": [
          {
            "_id": "606c13e2acf9f804af388916",
            "from": "cancelled",
            "to": "end",
            "data": "{\"unitId\":\"606c1257733901019076dcef\",\"building_trying\":false,\"building_error\":null}",
            "occuredAt": 1617695714241
          },
          {
            "_id": "606c13e1acf9f804af388914",
            "from": "rawtx_built",
            "to": "cancelled",
            "data": "{\"unitId\":\"606c1257733901019076dcef\",\"building_trying\":false,\"building_error\":null}",
            "occuredAt": 1617695713271
          },
          {
            "_id": "606c1348acf9f804af388908",
            "from": "unbuilt",
            "to": "rawtx_built",
            "data": "{\"unitId\":\"606c1257733901019076dcef\",\"building_trying\":false}",
            "occuredAt": 1617695560340
          }
        ],
        "createdAt": "2021-04-06T07:52:38.581Z",
        "updatedAt": "2021-04-06T07:55:14.244Z",
        "__v": 3,
        "transaction": "606c1347acf9f804af388905",
        "finished": true,
        "id": "606c13460b207604bc55a02b"
      }
    ],
    "tags": [],
    "n": 0,
    "fee": "0",
    "fees": [],
    "data": {
      "timestampBegin": 0,
      "timestampFinish": 0,
      "reason": "user request."
    },
    "options": {
      "unitId": "606c1257733901019076dcef"
    },
    "pendingTags": [],
    "sendingTags": [],
    "stateTags": [],
    "validationResults": [],
    "events": [
      {
        "topic": "state-issue",
        "_id": "606c13e10b207604bc55a052",
        "value": "cancelled",
        "code": 29016,
        "message": "user request.",
        "occuredAt": 1617695713142
      }
    ],
    "inputs": [],
    "hash": "",
    "txid_rawtx": "",
    "block": -1,
    "blockHash": "",
    "blockTimestamp": -1,
    "extraData": "",
    "memo": "",
    "sendAgain": false,
    "relatedOrder": "",
    "relatedIssueRecords": [],
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

查询订单

*HTTP Request*

`GET http:/{ip}/api/v2/orders/{orderID}`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
orderID | yes | string | 查询订单的ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
_id | string | 数据库中_id序列
id | string | 订单号
coinName | string | 币种名称
txid | string | 上链后的txid
meta | string | 存放nonce
appid | string | 应用id
wallet | string | 钱包ID
sendMode | string | 发送方式，normal、manual_sign
state | string | 订单状态
bizType | string | 订单类型
type | string | 主type
subType | string | 子type
coinType | string | 币种类型
from | string | 订单创建时的from
froms | string | 实际链上去重后的from
to | string | 到账地址
value | string | 订单金额
sequence | number | 唯一序列号
confirmations | number | 当前确认数
expectedConfirmations | number | 期望确认数
action | string | 订单动作
actionArgs | list | action的参数
actionResults | list | action结果
postHandlers | list |
n | number | 订单所在交易的位置
fee | string | 手续费
fees | list | 手续费具体信息
data | object | 订单处理过程中的数据记录
pendingTags | list | 订单异常时的标识位
events | list | 事件记录
hash | string | 交易上链的hash
txid_rawtx | string | 预先计算的hash
block | number | 订单所在区块
blockHash | string | 订单所在区块的hash
blockTimestamp | number | 订单所在区块的时间戳
extraData | string | 额外信息
memo | string | 提现时的memo
sendAgain | string | 重发标识
relatedOrder | string | 关联订单
relatedIssueRecords | list | holding订单记录

### 取消订单

> Request "取消订单" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695713151,
  "sig": {
    "r": "GaWb4TJ0ZHUHTa7RlwabSkuun9OFX/2x23ZKdzhPADU=",
    "s": "c9+Y8ZJ3CE5Y5R/hoQ4IxSkp2J4ypX3/bkIcG5AUdgw=",
    "v": 27
  },
  "result": {
    "ok": true,
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

取消订单

*HTTP Request*

`POST http:/{ip}/api/v2/orders/{orderID}/cancel`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
orderID | yes | string | 取消订单的ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
ok | boolean | true表示请求成功

### 上传签名

> Request "上传签名" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617698921829,
  "sig": {
    "r": "dNVXJvS7k0qGk2z7c6k/CaJpGhdNYNQ8Z2OK/tOx0LM=",
    "s": "CF5ogiGkkPvkoDfn0F6tYOctR+QEXVnX1iAdR2WM5Q0=",
    "v": 28
  },
  "result": {
    "ok": true,
    "namespace": "ETH",
    "sid": "b47BkqPk6RBd8EksAAAD"
  }
}
```

上传签名

*HTTP Request*

`POST http:/{ip}/api/v2/orders/{orderID}/import-sig`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
orderID | yes | string | 上传签名的订单ID
data.sigs | yes | array | 所有签名
data.sigs.signature | yes | string | 签名
data.sigs.recovery | yes | number | recovery ID

*Response Result*

Value | Type | Description
--------- | ------- | ---------
ok | boolean | true表示请求成功

## 工具

### 查询手续费

> Request "查询手续费" API returns JSON structured like this:

```json
{
  "code": 0,
  "status": 0,
  "message": "OK",
  "crypto": "ecc",
  "hash": "sha3",
  "sort": "key-alphabet",
  "encode": "base64",
  "timestamp": 1617695873746,
  "sig": {
    "r": "zfnD4y41JOJV00wncmnDPen4Qk31TVL6xalKdob6C4w=",
    "s": "bsQeFGQuadB84yUdUYDilQqmXUXOHdYg8HilbLV8vzo=",
    "v": 28
  },
  "result": {
    "priorityDefault": 1,
    "feeToken": "ETH",
    "feeDecimal": 18,
    "feePriceMin": 1500000000,
    "feePriceMax": 150000000000,
    "feePriceRealtime": 15000000000,
    "gasLimitDefault": 21000,
    "gasLimitMin": 21000,
    "gasLimitMax": 100000,
    "namespace": "ETH",
    "sid": "-1IenchlwJXm5e2eAAAE"
  }
}
```

查询手续费

*HTTP Request*

`GET http://{ip}/api/v2/utilities/tokens/{token}/fee-prices`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
token | yes | string | 查询手续费的token

*Response Result*

Value | Type | Description
--------- | ------- | ---------
priorityDefault | number | 默认手续费倍率
feeToken | string | 查询的token
feeDecimal | number | 单位小数位
feePriceMin | number | 范围最低，实时fee的一倍
feePriceMax | number | 范围最高，实时fee的十倍
feePriceRealtime | number | 实时fee价格
gasLimitMin | number | 最小gas limit
gasLimitMax | number | 最大gas limit

### 区块链调用

> Request "区块链调用" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "hash": "sha3",
    "sort": "key-alphabet",
    "encode": "base64",
    "timestamp": 1617695804871,
    "sig": {
        "r": "QqE3UwP5VcWuGcbxY1gGid9X7WdFGyGfXvZOkCsOf8c=",
        "s": "UyekOcKXgIySyA4Oo0DoXTHRZpE70FUOaSW3J+VIlLU=",
        "v": 28
    },
    "result": {
        "jsonrpc": "2.0",
        "result": "0x37e11d600",
        "id": "0x1232",
        "namespace": "ETH",
        "sid": "-1IenchlwJXm5e2eAAAE"
    }
}
```

区块链调用

*HTTP Request*

`POST http://{ip}/api/v2/utilities/tokens/{token}/call`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
token | yes | string | 调用区块链方法的token
data.id | no | string | 调用rpc方法的ID
data.method | no | string | 调用的rpc方法
data.params | no | array | 调用rpc方法传入的参数

*Response Result*

Value | Type | Description
--------- | ------- | ---------
id | string | 传入的调用rpc方法的ID
result | string | 调用rpc方法的返回结果


# BLS Key Server API

<aside class="notice">
端口是8901，默认只能本地访问。若需外部访问，请参考教程设置：https://nbltrust.github.io/jadepool-hub-tech-docs/zh-hans/seed-permission.html
</aside>

### Derive BLS Key Pair

> Request "Derive BLS Key Pair" API returns JSON structured like this:

```json
{
    "index": 10,
    "privatekey": "0x70821c9df11e4d0b0763c502d4bfafba35070c2d379f3ae2a6e3f5689c6b7745",
    "pubkey": "0xb74e693fc033ae0ba8eb1cf94c063c9e40050803cead9da516e9e544c96b7bd5ec23b099f17dfba1ac3f8d6649e1f1dd"
}
```

Derive BLS Key Pair

*HTTP Request*

`POST http://{api url of seed server}/api/derive_keypair`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
index | yes | number | unique ID to derive key from master seed

*Response Result*

Value | Type | Description
--------- | ------- | ---------
index | number | unique ID to derive key from master seed
privatekey | string | private key
pubkey | string | public key

### Generate Deposit Data

> Request "Generate Deposit Data" API returns JSON structured like this:

```json
{
    "signing_pubkey": "98114c41a81aec827c4fec58f92ae86e3d69e1dd5f9b9f16653d746d46cee1465e55a604a801d4f40b3153c032cc7905",
    "withdrawal_credentials": "00b025640e57e3bf6f3b8c06d737730b8444c034ed3a38173179f35ec4f85e31",
    "signature": "81526ac4a1da286d6aa73656a53120d3364510c40d2c24e7419e796104a492fdde59c4eeed9b6c1a93e980891faaca0c125ed0bc5eb032d8b8ffb7bfd957acef4e54462e39eb4d1e07b7964f4db1419e8850e615dbe48d8c7321fe683da6e963",
    "deposit_data_root": "3c60988198779505a2379037bf258037df8fa970af9bde863052e18d26db886b"
}
```

Generate Deposit Data

*HTTP Request*

`POST http://{api url of seed server}/api/generate_deposit_data`

*Main Parameters*

Parameter | Required | Type | Description
--------- | ------- | --------- | -----------
index | yes | number | unique ID to derive key from master seed
withdrawal_pubkey | yes | string | validator's withdrawal public key
amount | yes | string | amount of ETH to deposit to the validator

*Response Result*

Value | Type | Description
--------- | ------- | ---------
signing_pubkey | string | signing public key
withdrawal_credentials | string | withdrawal credentials
signature | string | signature
deposit_data_root | string | deposit data root

# Callback

<aside class="notice">
All timestamps are in millisecond.
</aside>

## Order

> Order notification sends JSON structured like this:

```json
{  
   "code":0,
   "message":"OK",
   "crypto":"ecc",
   "timestamp":1557286096399,
   "sig":{  
      "r":"WiTkCfTdBCGWHsMdd0sLiOZ536Zo013xnfUxHtFMkEk=",
      "s":"WvJHFQucDaOIZ8HkpW3xJiCBXh1eoIdlw4Hh/n0IXDE=",
      "v":28
   },
   "result":{  
      "_id":"5cd24caaa57c8d32ee301222",
      "id":"108",
      "coinName":"NASH",
      "txid":"0x662c73a8361acee47a267f580a55da9d7bbc720bb070bfd25baf765f79c2f019",
      "meta":null,
      "state":"pending",
      "bizType":"WITHDRAW",
      "type":"ETH",
      "subType":"NASH",
      "coinType":"NASH",
      "from":"0x9b878f3c613c77b50ccebd67faf6d7afd46de8ae",
      "to":"0x9cf16ecf95d707bd19a8e7f96e4fd93bd66e8c46",
      "value":"0.2",
      "sequence":11,
      "confirmations":19,
      "create_at":1557286058120,
      "update_at":1557286084437,
      "actionArgs":[  

      ],
      "actionResults":[  

      ],
      "n":0,
      "fee":"0.00040597",
      "fees":[  
         {  
            "_id":"5cd24ccea57c8d32ee301247",
            "amount":"0.00040597",
            "coinName":"ETH",
            "nativeAmount":"0",
            "nativeName":""
         }
      ],
      "data":{  
         "timestampBegin":1557286074480,
         "timestampFinish":0,
         "nonce":932,
         "type":"Ethereum",
         "hash":"0x662c73a8361acee47a267f580a55da9d7bbc720bb070bfd25baf765f79c2f019",
         "blockNumber":8262212,
         "blockHash":"0x0c946f37a4c18935b75f9c7eb714c4d970f1d6d7be47158a5a29131d7f8bf6df",
         "from":[  
            {  
               "address":"0x9b878f3c613c77b50ccebd67faf6d7afd46de8ae",
               "value":"0.2",
               "asset":"NASH"
            }
         ],
         "to":[  
            {  
               "address":"0x9cf16ecf95d707bd19a8e7f96e4fd93bd66e8c46",
               "value":"0.2",
               "asset":"NASH"
            }
         ],
         "fee":"0.00040597",
         "confirmations":19
      },
      "hash":"0x662c73a8361acee47a267f580a55da9d7bbc720bb070bfd25baf765f79c2f019",
      "block":8262212,
      "extraData":"",
      "memo":"",
      "sendAgain":false
   }
}
```

Data block enables you to get latest information about the transaction.

*Callbck Result*

Value | Type | Description
--------- | ------- | ---------
id | string | the order ID
coinName | string | unique token name
txid | string | transaction hash
meta | any | supplementary data
state | string | order state
bizType | string | order type
type | string | token type
from | string | transaction input
to | string | transaction output
value | string | transaction value
confirmations | number | number of transaction confirmations
fee | string | fee burnt for the transaction
fees | array | all types of fee burnt for the transaction
hash | string | transaction hash
block | number | the block transaction mined in
extraData | string | same as the extraData in request
memo | string | order note, editable on admin
sendAgain | boolean | if we suggest client to send the same request again
data | object | latest info of the transaction

## Audit

> Audit notification sends JSON structured like this:

```json
{
   "code":0,
   "status":0,
   "message":"OK",
   "crypto":"ecc",
   "hash":"sha3",
   "sort":"key-alphabet",
   "encode":"base64",
   "timestamp":1582285270762,
   "sig":{
      "r":"Fcn5eCj/2tpF4vLhhrLDNSSlP8smdVwWXcoufINclQc=",
      "s":"cUeE1d1HFQmzJbea/1+YjqNm+/cikJbyzMY8vqClM0I=",
      "v":27
   },
   "result":{
      "current":{
         "id":"5e4fc1d6548491723a096c23",
         "type":"ETH",
         "blocknumber":140215,
         "timestamp":1582285270762,
         "deposit_total":"0",
         "withdraw_total":"0.004",
         "fee_total":"0.000126",
         "internal_fee":"0",
         "internal_num":"0"
      },
      "wallet":"default",
      "blocknumber":140215,
      "calculated":true,
      "deposit_total":"0",
      "deposit_num":0,
      "withdraw_total":"0.004",
      "withdraw_num":1,
      "revert_total":"0",
      "revert_num":0,
      "refund_total":"0",
      "refund_num":0,
      "system_call_total":"0",
      "system_call_num":0,
      "delegate_total":"0",
      "delegate_num":0,
      "undelegate_total":"0",
      "undelegate_num":0,
      "principal_fund_total":"0",
      "principal_fund_num":0,
      "interest_fund_total":"0",
      "interest_fund_num":0,
      "sweep_total":"0",
      "sweep_num":0,
      "sweep_internal_total":"0",
      "sweep_internal_num":0,
      "airdrop_total":"0",
      "airdrop_num":0,
      "recharge_total":"0",
      "recharge_num":0,
      "recharge_internal_total":"0",
      "recharge_internal_num":0,
      "recharge_unexpected_total":"0",
      "recharge_unexpected_num":0,
      "recharge_special_total":"0",
      "recharge_special_num":0,
      "failed_withdraw_num":1,
      "failed_sweep_num":0,
      "failed_sweep_internal_num":0,
      "failed_refund_num":0,
      "failed_system_call_num":0,
      "failed_delegate_num":0,
      "failed_undelegate_num":0,
      "fees":[
         {
            "withdraw_fee":"0.000126",
            "refund_fee":"0",
            "sweep_fee":"0",
            "sweep_internal_fee":"0",
            "system_call_fee":"0",
            "delegate_fee":"0",
            "undelegate_fee":"0",
            "failed_withdraw_fee":"0",
            "failed_refund_fee":"0",
            "failed_sweep_fee":"0",
            "failed_sweep_internal_fee":"0",
            "failed_system_call_fee":"0",
            "failed_delegate_fee":"0",
            "failed_undelegate_fee":"0",
            "_id":"5e65bec73aef16290cb85510",
            "fee_type":"ETH"
         }
      ],
      "chainKey":"ETH",
      "type":"ETH",
      "timestamp":1582285270762,
      "appid":"test",
      "create_at":"2020-02-21T11:41:10.864Z",
      "update_at":"2020-03-09T03:57:59.884Z",
      "__v":6,
      "calc_order_num":2,
      "last":"5e4f8be476adea46c1cea568",
      "id":"5e4fc1d6548491723a096c23"
   }
}
```

Delegation related orders are not audited for now. 

*Callback Result*

Value | Type | Description
--------- | ------- | ---------
calculated | boolean | if the auditing is finished
deposit_total | string | total deposit token value
deposit_num | number | total deposit times
withdraw_total | string | total withdraw token value
withdraw_num | number | total withdraw times
sweep_total | string | total hot-to-cold token value
sweep_num | number | total hot-to-cold times 
sweep_internal_total | string | total internal-out token value
sweep_internal_num | number | total internal-out times
airdrop_total | string | total airdrop token value
airdrop_num | number | total airdrop times
recharge_total | string | total cold-to-hot token value
recharge_num | number | total cold-to-hot times
recharge_internal_total | string | total internal-in token value
recharge_internal_num | number | total internal-in times
recharge_unexpected_total | string | total unexpected-in token value
recharge_unexpected_num | number | total unexpected-in times
recharge_special_total | string | total special-in token value
recharge_special_num | number | total special-in times
failed_withdraw_num | number | total failed withdrawal times
failed_sweep_num | number | total failed hot-to-cold times
failed_sweep_internal_num | number | total failed internal-out times
fees | array | all type of fees burned
withdraw_fee | string | fees burned for withdrawal
refund_fee | string | fees burned for refund
sweep_fee | string | fees burned for hot-to-cold
sweep_internal_fee | string | fees burned for internal-out
system_call_fee | string | fees burned for all system_call txs
delegate_fee | string | fees burned for staking
undelegate_fee | string | fees burned for unstaking
failed_withdraw_fee | string | fees burned for failed withdrawal
failed_refund_fee | string | fees burned for failed refund
failed_sweep_fee | string | fees burned for refund hot-to-cold
failed_sweep_internal_fee | string | fees burned for refund internal-out
failed_system_call_fee | string | fees burned for all failed system_call txs
failed_delegate_fee | string | fees burned for failed delegation
failed_undelegate_fee | string | fees burned for failed undelegation
type | string | the audited token type
timestamp | number | the audited timestamp
blocknumber | number | corresponding block near the audited timestamp
appid | string | application ID
calc_order_num | number | audited order number
last | string | audit order ID of the previous audit
id | string | audit order ID of the current audit

# ECC Signature

## Release 0.13.x

>  Javascript code of building the message to be signed:

```js
function _buildMsg (obj, opts = {}) {
  let sortRule = opts.sort || 'key-alphabet'
  let arr = []
  if (_.isArray(obj)) {
    arr = obj.map((o, i) => ({
      k: (sortRule === 'kvpair' || sortRule === 'value') ? '' : i,
      v: _buildMsg(o, opts)
    }))
  } else if (_.isObject(obj)) {
    for (let k in obj) {
      if (obj[k] !== undefined) {
        arr.push({ k, v: _buildMsg(obj[k], opts) })
      }
    }
  } else if (obj === undefined || obj === null) {
    return ''
  } else {
    return obj.toString()
  }
  // Sort Array
  arr.sort((a, b) => {
    let aVal
    let bVal
    switch (sortRule) {
      case 'key':
        aVal = a.k
        bVal = b.k
        break
      case 'key-alphabet':
        aVal = a.k.toString()
        bVal = b.k.toString()
        break
      case 'value':
        aVal = a.v
        bVal = b.v
        break
      case 'kvpair':
      default:
        aVal = a.k.toString() + a.v
        bVal = b.k.toString() + b.v
        break
    }
    if (aVal < bVal) {
      return -1
    } else if (aVal === bVal) {
      return 0
    } else {
      return 1
    }
  })
  // Build message
  return arr.reduce((lastMsg, curr) => {
    return lastMsg + curr.k + curr.v
  }, '')
}
```

*Steps of building signature for GET requests::*

1. Get the current timestamp.
2. Form a String message that contains the timestamp above. The message looks like this:
</br>
```
timestamp1557913602438
```
3. Encrypt the message using sha3 or sha256 or md5.
4. Sign the encrypted message and get the signature.
5. Send request as the same format as described in [General Structure](#general-structure).

*Steps of building signature for POST requests::*

1. Get the current timestamp.
2. Form a String message that consists of all main parameters in "data" object and the timestamp, the keys 
must be sorted alphabetically. Use "request withdrawal" as an example, the message looks like this:
</br>
```
sequence0timestamp1557913602438tomg2bfYdfii2GG13HK94jXBYPPCSWRmSiAStypeBTCvalue0
```
3. Encrypt the message using sha3 or sha256 or md5.
4. Sign the encrypted message and get the signature.
5. Send request as the same format as described in [General Structure](#general-structure).

*Steps of verifying signature:*

1. Form a String message that consists of all fields in "result" object and the given timestamp of the response, the keys 
must be sorted alphabetically. Use "validate address" as an example, the message looks like this:
</br>
```
addressawesome1namespaceEosiosidJGEPU47lvG6qICvmAAABtimestamp1557912642626validtrue
```
2. Encrypt the message using sha3 or sha256 or md5.
3. Verify signature using the plain encrypted message and Jadepool's public key.

## Release 1.0

*Steps of building signature for GET requests:*

1. Get the current timestamp.
2. Form a String message that contains the timestamp above. The message looks like this:
</br>
```
timestamp1557913602438
```
3. Encrypt the message using sha3 or sha256 or md5.
4. Sign the encrypted message and get the signature.
5. Send request as the same format as described in [General Structure](#general-structure).

*Steps of building signature for POST requests:*

1. Get the current timestamp.
2. Form a String message that consists of all main parameters in "data" object, the keys 
must be sorted alphabetically. Use "request withdrawal" as an example, the message looks like this:
</br>
```
sequence0tomg2bfYdfii2GG13HK94jXBYPPCSWRmSiAStypeBTCvalue0
```
3. add timestamp at the end of the message, the message looks like this:
</br>
```
sequence0tomg2bfYdfii2GG13HK94jXBYPPCSWRmSiAStypeBTCvalue0timestamp1557913602438
```
4. Encrypt the message using sha3 or sha256 or md5.
5. Sign the encrypted message and get the signature.
6. Send request as the same format as described in [General Structure](#general-structure).

*Steps of verifying signature:*

1. Form a String message that consists of all fields in "result" object of the response, the keys 
must be sorted alphabetically. Use "validate address" as an example, the message looks like this:
</br>
```
addressawesome1namespaceEosiosidJGEPU47lvG6qICvmAAABvalidtrue
```
2. add the given timestamp at the end of the message, the message looks like this:
</br>
```
addressawesome1namespaceEosiosidJGEPU47lvG6qICvmAAABvalidtruetimestamp1557912642626
```
2. Encrypt the message using sha3 or sha256 or md5.
3. Verify signature using the plain encrypted message and Jadepool's public key.
