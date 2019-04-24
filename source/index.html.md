---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - JSON

toc_footers:
  - <a href='https://github.com/nbltrust/jadepool-doc'>Jadepool Documentation</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Jadepool API! 

We provide two versions of API: V1 and V2. We recommend you to use V2 which includes significant improvements comparing to V1. However, there will still be small changes
to V2 APIs along our development cycle. Check out our [releases](https://github.com/nbltrust/jadepool-doc/releases) for details.

Feel free to check out our [NodeJs SDK](https://github.com/nbltrust/jadepool-sdk-nodejs) and [Java SDK](https://github.com/nbltrust/jadepool-sdk-java). SDK only supports
V1 for now.

# V1

## Address

### Create Multiple Addresses

> Request "Create Multiple Addresses" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556006943412,
    "sig": {
        "r": "1fdgXdwlQDNoC8qJdJubZXLT5jrtwm3ttX/q2qIsIus=",
        "s": "HBC5Z8QmQDp0twfGE4k9ywK4p1VuTfIPNC7hFTXOut0=",
        "v": 28
    },
    "result": [
        {
            "id": 277,
            "appid": "pri",
            "coinName": "ETH",
            "address": "0xfeb3cd94cd4e1ca90514d589cf7b344e9e8cb4b1",
            "state": "used",
            "mode": "deposit",
            "create_at": 1556006943104,
            "update_at": 1556006943346,
            "namespace": "ETH"
        },
        {
            "id": 278,
            "appid": "pri",
            "type": "ETH",
            "address": "0x18cee689f40be2b1539ed875c0be57a768e078c5",
            "state": "used",
            "mode": "deposit",
            "create_at": 1556006943381,
            "update_at": 1556006943408,
            "namespace": "ETH"
        }
    ]
}
```

This endpoint enables you to request multiple new addresses at once.

*HTTP Request*

`POST http://jadepool.example/api/v1/addresses/batchnew`

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.type | yes | string | coin name
data.amount | yes | number | number of addresses, range from 1 to 500.
data.mode | no | string | address mode: 'auto', 'deposit', 'deposit_memo', 'normal'
data.callback | no | string | callback url

### Create Single Address

> Request "Create Single Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556007795024,
    "sig": {
        "r": "NgoqsdWEbqED8nfFR8arTE5psEbIPDRn1qCN6YOTaTI=",
        "s": "QVoftyErHN+VPlnCn1hoLqi8S6UAe2zJrjemWNQB/NM=",
        "v": 27
    },
    "result": {
        "id": 281,
        "appid": "pri",
        "type": "ETH",
        "address": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "state": "used",
        "mode": "deposit",
        "create_at": 1556007795000,
        "update_at": 1556007795020,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to request single new address.

*HTTP Request*

`POST http://jadepool.example/api/v1/addresses/new`

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.type | yes | string | coin name
data.mode | no | string | address mode: 'auto', 'deposit', 'deposit_memo', 'normal'
data.callback | no | string | callback url

### Validate Address

> Request "Validate address" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008291521,
    "sig": {
        "r": "/9lfiJms7SiZZDFpjVc4+phMIl1G3WN3fxKuwAE0xjo=",
        "s": "JZSHP/h0Pf1C7SsZinvL3lN1uO1BvaWvD+pHWQMVGuo=",
        "v": 27
    },
    "result": {
        "address": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "valid": true,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to verify if an address is valid for specified token.

*HTTP Request*

`POST http://jadepool.example/api/v1/addresses/verify`

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.type | yes | string | coin name
data.address | yes | string | address to validate

## Audit

### Fetch Audit Order

> Request "Fetch Audit Order" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1546425032000,
    "sig": {
        "r": "1S3Uk1gTOwnbshQLVsG9KHnAvQbmRm2Z0C/WcYN1z7w=",
        "s": "Z9I/bjAiica0RQJHHWEow7GzJi61MjoQnA4tT/VlDic=",
        "v": 28
    },
    "result": {
        "calculated": true,
        "deposit_total": "0",
        "deposit_num": 0,
        "withdraw_total": "0",
        "withdraw_num": 0,
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
        "recharge_unknown_total": "0",
        "recharge_unknown_num": 0,
        "recharge_special_total": "0",
        "recharge_special_num": 0,
        "failed_withdraw_num": 0,
        "failed_sweep_num": 0,
        "failed_sweep_internal_num": 0,
        "fees": [],
        "type": "ETH",
        "timestamp": 1546425032000,
        "blocknumber": 9921056,
        "appid": "test",
        "create_at": "2019-04-23T08:32:51.859Z",
        "update_at": "2019-04-23T08:32:58.666Z",
        "__v": 0,
        "calc_order_num": 0,
        "last": "5cbecdba8d1607c10ddddb4b",
        "id": "5cbecdb38d1607c10ddddb49"
    }
}
```

This endpoint enables you to fetch audit order.

*HTTP Request*

`GET http://jadepool.example/api/v1/audits/:id`

*Query Parameters*

Parameter | Description
--------- | ------- 
id | audit order ID

### Request audit

> Request "Request audit" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008371920,
    "sig": {
        "r": "tCWtyZLol3gTEjWsRThfltjxe7rWB0m1c5oiIeFCaPE=",
        "s": "UXCLuRMQsSDbU0xfo6dVfcADzIjAnVL5bJS5M/XqFuU=",
        "v": 28
    },
    "result": {
        "type": "ETH",
        "current": {
            "id": "5cbecdb38d1607c10ddddb49",
            "type": "ETH",
            "blocknumber": 9921056,
            "timestamp": 1546425032000
        },
        "last": null,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to request audit with timestamp of your choice.

*HTTP Request*

`POST http://jadepool.example/api/v1/audits`

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.type | yes | string | coin name for auditing
data.audittime | yes | number | audit timestamp
data.callback | no | string | callback url

## Transaction

### Request Withdrawal

> Request "Request Withdrawal" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008499840,
    "sig": {
        "r": "MDQ7ytbVqn2csvSRIlCK169YP+6Fw4npwCZrOyg7kR8=",
        "s": "Z2Qf7nB45dcH6TfjpDT1hH+eaef2ep2ARYJKF0t7iss=",
        "v": 28
    },
    "result": {
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0
        },
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

`POST http://jadepool.example/api/v1/transactions`

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.type | yes | string | coin name for auditing
data.to | yes | string | withdrawal address
data.value | yes | string | value to withdraw
data.sequence | no | number | unique API nonce for each app ID
data.auth | no | string | authentication for coin, only for HSM 
data.extraData | no | string | some data to save in Jadepool database and will be returned the same in response
data.callback | no | string | callback url

### Fetch Order

> Request "fetch Order" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008528904,
    "sig": {
        "r": "cJyoM1mnhdSrKECuXmWGGx5gD1ZVrEn0Fjb5xqmcJrM=",
        "s": "O9n6pLATkYJ5TA2vU/nPwCXsthtni41b1JYr/B9L1sw=",
        "v": 28
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
        "from": "",
        "to": "0xbf0aa791e34d3af906050fcb713305c1d92880af",
        "value": "0.001",
        "create_at": 1556008499740,
        "update_at": 1556008499742,
        "actionArgs": [],
        "actionResults": [],
        "n": 0,
        "fee": "0",
        "fees": [],
        "data": {
            "timestampBegin": 0,
            "timestampFinish": 0,
            "type": "Ethereum",
            "hash": "",
            "blockNumber": -1,
            "blockHash": "",
            "from": [],
            "to": [],
            "state": "init"
        },
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

This endpoint enables you to fetch any order details from Jadepool database.

*HTTP Request*

`GET http://jadepool.example/api/v1/transactions/:id`

*Query Parameters*

Parameter | Description
--------- | ------- 
id | order ID


## Wallet

### Fetch Wallet Status

> Request "Fetch Wallet Status" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1556008547809,
    "sig": {
        "r": "9Eld2EfJfsIxTv7GRbg41TsXos2ntvmOmcutXHh5yKg=",
        "s": "DgGgelZ0UWc01iDOfYixCG3P0rv59xKPUqgImKrAd4w=",
        "v": 28
    },
    "result": {
        "balance": "0.01",
        "balanceAvailable": "0",
        "balanceUnavailable": "0.01",
        "update_at": 1556008547806,
        "addressesWithBalance": 1,
        "namespace": "ETH",
        "sid": "j5D28yAyhO02C-_6AAAA"
    }
}
```

This endpoint enables you to fetch balance details of any enabled token.

*HTTP Request*

`GET http://jadepool.example/api/v1/wallet/:coinName/status`

*Query Parameters*

Parameter | Description
--------- | -------
coinName | coin name

## Delegation

<aside class="notice">
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

### Fetch Reward With Validator

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

This endpoint enables you to fetch reward amount at a specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/validators/:validator/outstanding_rewards`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
validator | validator name

### Request Delegation

> Request "Request Delegation" API returns JSON structured like this:

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

This endpoint enables you to request delegation.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/delegators/:delegator/delegate`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.amount | yes | string | delegate value
data.validator | yes | string | validator to delegate
data.sequence | yes | number | unique sequence

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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.amount | yes | string | delegate value
data.sequence | yes | number | unique sequence
data.src_validator | yes | string | original validator
data.dst_validator | yes | string | validator to re-delegate

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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.amount | yes | string | undelegate value
data.sequence | yes | number | unique sequence
data.validator | yes | string | validator undelegate from

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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.sequence | no | number | unique sequence, required in V2.
data.address | yes | string | reward address

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

This endpoint enables you to claim reward.

*HTTP Request*

`POST http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/claim`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.sequence | no | number | unique sequence, required in V2.
data.validator | yes | string | validator to claim reward from

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

This endpoint enables you to fetch reward status from all validators.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/profits`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address

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

This endpoint enables you to fetch reward status at specified validator.

*HTTP Request*

`GET http://jadepool.example/api/v1/stake/:coinName/rewards/:delegator/claim/:validator`

*Query Parameters*

Parameter | Description
--------- | ------- 
coinName | coin name
delegator | delegate from address
validator | validator

# V2

## Address

### Create Multiple Addresses

> Request "Create Multiple Addresses" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.amount | yes | number | number of addresses, range from 1 to 500.
data.mode | yes | string | address mode: 'auto', 'deposit', 'deposit_memo', 'normal'

### Create Single Address

> Request "Create Single Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.mode | yes | string | address mode: 'auto', 'deposit', 'deposit_memo', 'normal'

### Validate Address

> Request "Validate Address" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.address | yes | string | address to validate

### Force Free

> Request "Force Free" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.address | yes | string | address to validate
data.sequence | yes | number | unique request sequence

### Force Obtain

> Request "Force Obtain" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.address | yes | string | address to validate
data.sequence | yes | number | unique request sequence
data.value | yes | string | force obtain value

### Fetch Incomplete Orders

> Request "Fetch Incomplete Orders" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

### Fetch Address Balance

> Request "Fetch Address Balance" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

### Fetch Address Info

> Request "Fetch Address Info" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

## Audit

### Fetch Audit Order

> Request "Fetch Audit Order" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
    "message": "OK",
    "crypto": "ecc",
    "timestamp": 1546425032000,
    "sig": {
        "r": "1S3Uk1gTOwnbshQLVsG9KHnAvQbmRm2Z0C/WcYN1z7w=",
        "s": "Z9I/bjAiica0RQJHHWEow7GzJi61MjoQnA4tT/VlDic=",
        "v": 28
    },
    "result": {
        "calculated": true,
        "deposit_total": "0",
        "deposit_num": 0,
        "withdraw_total": "0",
        "withdraw_num": 0,
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
        "recharge_unknown_total": "0",
        "recharge_unknown_num": 0,
        "recharge_special_total": "0",
        "recharge_special_num": 0,
        "failed_withdraw_num": 0,
        "failed_sweep_num": 0,
        "failed_sweep_internal_num": 0,
        "fees": [],
        "type": "ETH",
        "timestamp": 1546425032000,
        "blocknumber": 9921056,
        "appid": "test",
        "create_at": "2019-04-23T08:32:51.859Z",
        "update_at": "2019-04-23T08:32:58.666Z",
        "__v": 0,
        "calc_order_num": 0,
        "last": "5cbecdba8d1607c10ddddb4b",
        "id": "5cbecdb38d1607c10ddddb49"
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

## Order

### Fetch Order

> Request "fetch Order" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

## Wallet

### Fetch wallet Status

> Request "Fetch wallet Status" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

### Request Withdrawal

> Request "Request Withdrawal" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.to | yes | string | withdrawal address
data.value | yes | string | value to withdraw
data.sequence | yes | number | unique API nonce for each app ID
data.auth | no | string | authentication for coin, only for HSM
data.extraData | no | string | callback url

### Audit For Specified Coin

> Request "Audit For Specified Coin" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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
        "current": {
            "id": "5cbecdb38d1607c10ddddb49",
            "type": "ETH",
            "blocknumber": 9921056,
            "timestamp": 1546425032000
        },
        "last": null,
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

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.audittime | yes | number | audit timestamp

## Wallets

### Audit For All Coins

> Request "Audit For All Coins" API returns JSON structured like this:

```json
{
    "code": 0,
    "status": 0,
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
            "current": {
                "id": "5cbecdb38d1607c10ddddb49",
                "type": "ETH",
                "blocknumber": 9921056,
                "timestamp": 1546425032000
            },
            "last": null,
            "sid": "j5D28yAyhO02C-_6AAAA"
        }
    ]
}
```

This endpoint enables you to request audit of all tokens.

**HTTP Request**

`POST http://jadepool.example/api/v2/wallets/audit`

*Request Body Parameters*

Parameter | Required | type | Description
--------- | ------- | --------- | -----------
data | yes | json | request data
data.audittime | yes | number | audit timestamp

## Delegation

Same as [V1] (#delegation), except that sequence is required in every API.
