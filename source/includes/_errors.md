# Errors

Jadepool API uses the following error codes:

Error Code | Category | CN | EN | JA
---------- | ------- | ---------- | ---------- | ----------
401 | general-error | 401 用户认证失败或已过期 | 401 authentication failed or expired | 401 認証が期限切れ
403 | general-error | 403 权限不足拒绝访问 | 403 invalid authorization | 403 権限不足
404 | general-error | 404 该API不存在 | API not found | このAPIは存在しない
405 | general-error | 405 该方法不可用 | 405 Method Not Allowed | 405 Method Not Allowed
500 | general-error | 500 请求失败({code}) | 500 request failed({code}) | 500 リクエスト失敗({code})
10001 | general-error | 系统错误 | system error | システムエラー
10002 | general-error | 非法参数 | invalid parameter | 不正パラメータ
10003 | general-error | 请求内容需为参数data | request data should be in 'data' field | 依頼内容をパラメータ必要'data'
10004 | general-error | 无法删除已关联的对象 | cannot delete associated object | 関連する対象を削除できません
10005 | general-error | 未找到指定的钱包 | No specified wallet was found | 指定されたウォレットが見つかりませんでした
10010 | general-error | 暂不支持该模块 | This module is not supported for now | このモジュールはサポートされていません
10098 | general-error | 系统尚未初始化 | System not initialized | システムはまだ初期化されていません
10099 | general-error | 系统已完成了初始化流程 | system is already initialized | システムは初期化の流れを完了した
10200 | general-error | 系统连接慢雾网络时出错 | Occur error when connecting to slowmist | Occur error when connecting to slowmist
20000 | invalid-parameter | 不支持该币种类型 | unsupported coin type | 貨幣種のタイプを支持しない
20001 | invalid-parameter | 地址错误-首字母不对 | address wrong - first charactor | アドレスエラー-頭文字が間違っていません
20002 | invalid-parameter | 地址错误-长度不对 | address wrong - length | アドレスエラー-長さが違います
20003 | invalid-parameter | 地址与类型不匹配 | The address is miss-match with requested type | アドレスとタイプが一致しない
20004 | invalid-parameter | 提币金额高于高水位 | The withdrawal amount is higher then high water level | 貨幣の金額は高い水位より高い
20005 | invalid-parameter | 提币MEMO长度非法 | length of the withdrawal memo is invalid | 引き下げメモの長さが無効です
20006 | invalid-parameter | 提现序号已使用 | The withdrawal sequence was used | 番号が使用済み
20007 | invalid-parameter | 不支持该业务模式 | This business model is not supported | このビジネスモデルはサポートされていません
20008 | invalid-parameter | 该地址余额与参数不符 | The address balance does not match the parameters | このアドレス残高はパラメータと一致しません。
20009 | invalid-parameter | 该地址已存在相关业务 | The address already has relevant business | すでに関連業務があります。
20010 | invalid-parameter | 必选参数不能为空 | Necessary parameters cannot be empty | 必選パラメータは空けてはならない
20011 | invalid-parameter | 该币种类型目前无法提现 | The type of currency cannot be withdrawn. | この貨幣の種類のタイプは現在現われることができない
20012 | invalid-parameter | 该币种无法用于Stake相关操作 | This currency cannot be used for Stake-related operations | この貨幣はステーションに関連する操作ができません
20013 | invalid-parameter | 该地址尚未在目标上进行过抵押代理 | The address has not been delegated on the target validator | ターゲットバリデータにアドレスが委任されていません
20014 | invalid-parameter | 该地址在目标上的抵押金额不足 | The address has insufficient delegated amount on the target validator | ターゲット上のアドレス、担保額
20015 | invalid-parameter | 该地址在目标上不存在取消中的抵押 | The address has no undelegations on the target validator | このアドレスは、ターゲットには、キャンセル中の担保が存在しません。
20016 | invalid-parameter | 转抵押地址不可相同 | The address of re-delegations should be not same | 担保のアドレスは同じではない
20017 | invalid-parameter | 该地址无法用于Stake相关操作 | This address cannot be used for Stake-related operations | このアドレスに関連した活動のために使われる賭け
20018 | invalid-parameter | 该地址在目标上的取消抵押数已达到上限 | The amount of un-delegations on the target address has reached the upper limit. | 目標アドレスに関する国連代表団の量が上限に達している。
20021 | invalid-parameter | 扫描地址需为内部地址 | Scanning address should be internal address | スキャンアドレスは内部アドレス
20022 | invalid-parameter | 提现地址不可为系统地址 | The withdrawal address is not a systematic address. | 現金引き出しアドレスと係統のアドレス
20023 | invalid-parameter | 交易发起地址需为内部地址 | Source address should be an internal address | アドレスを内部アドレスでなければなりません
20024 | invalid-parameter | 该地址并不属于指定钱包 | The address does not belong to the specified wallet. | この住所は指定されたウォレットではありません
20025 | invalid-parameter | 该订单并不属于指定钱包 | The order does not belong to the specified wallet. | この注文は指定された財布ではありません
20026 | invalid-parameter | 该内容并不属于指定钱包 | The entity does not belong to the specified wallet. | この内容は指定されたウォレットではありません
20028 | invalid-parameter | 已开启自动审计，该API已禁用 | Auto Audit has been enabled and the API is disabled | 自動監査が開始されました。このAPIは無効になりました。
20029 | invalid-parameter | 未找到符合时间戳的区块号 | No block number matching the timestamp was found | 時間のスタンプに合ったブロック番号が見つかりません
20030 | invalid-parameter | 起始时间需小于结束时间 | Start time should be less than end time | 開始時間は終了時間より小さい
20031 | invalid-parameter | 审计时间戳应小于当前时间 | Audit timestamp should be less than current time | 監査時間のスタンプは現在の時間より小さいです
20032 | invalid-parameter | 不支持地址重扫功能 | Rescan by Address is not supported | アドレスをサポートしていない
20033 | invalid-parameter | 不支持区块重扫功能 | Rescan by Block is not supported | カット機能をサポートしていない
20034 | invalid-parameter | 应用AppID必须为字母 | AppID must be alphabetic | 応用AppIDなければならない文字
20035 | invalid-parameter | 传入的公钥不符合指定的编码方式 | The incoming public key does not match the specified encoding | 伝えられた公鍵は指定されたコード方式に合致しない
20036 | invalid-parameter | 应用AppID不存在 | AppID does not exist | 応用AppIDは存在しない
20037 | invalid-parameter | 应用AppID已存在 | AppID already exists | 応用AppIDすでに存在する
20038 | invalid-parameter | 代币已存在 | Token already exists | トークンが既に存在している
20039 | invalid-parameter | 不存在此代币 | Token doesn't exist | このトークンは存在しない
20040 | invalid-parameter | 需要禁用此代币 | Token should be disabled | トークンを無効にする
20041 | invalid-parameter | 已使用的公钥 | Public key duplicated | 公鍵が既に存在している
20042 | invalid-parameter | 已存在该名称的钱包 | A wallet with that name already exists | この名前のウォレットは既に存在します
20043 | invalid-parameter | 此代币尚存在处理中的订单 | This token still has some order under processing | このトークンはまだ処理中の注文があります。
20044 | invalid-parameter | 不可删除钱包内的最后一个应用 | The last application in the wallet that cannot be deleted | ウォレットの最後のアプリケーションを削除してはいけません。
20045 | invalid-parameter | 该应用目前存在异常不可用 | The application is currently unavailable | このアプリケーションは現在異常があります。利用できません
20046 | invalid-parameter | 该币种无法使用此操作 | This currency cannot be used for this operation | この貨幣はこの操作を使用できない
20047 | invalid-parameter | 批量操作进程失败 | Batch operations of process failed | Batch operations of process failed
20048 | invalid-parameter | 该用户名已存在 | The username already exists | ユーザ名は既に存在します
20049 | invalid-parameter | 非法地址 | Invalid address | Invalid address
20050 | invalid-parameter | OrderBatch处于不支持此操作的状态 | OrderBatch处于不支持此操作的状态 | OrderBatch处于不支持此操作的状态
20051 | invalid-parameter | 签名文件格式异常 | 签名文件格式异常 | 签名文件格式异常
20052 | invalid-parameter | 存在处理中OrderBatch | 存在处理中OrderBatch | 存在处理中OrderBatch
20053 | invalid-parameter | 不能同时切换多个私钥，请等待订单完成后再试。 | Only changing one permission is allowed at a time. Please retry later. | Only changing one permission is allowed at a time. Please retry later.
20054 | invalid-parameter | 非法公钥 | Invalid public key | Invalid public key
20101 | chain-general-error | 热钱包余额不足 | Insufficient hot wallet balance | 熱い財布の残額が足りない
20102 | chain-general-error | 连接区块链节点失败 | Failed to connection blockchain node | ブロックのチェーンノードを接続して失敗します
20103 | chain-general-error | 手续费不足 | Insufficient fees | 手数料が足りない
20104 | chain-general-error | 目前系统不支持该区块链网络 | The current system does not support this block chain network | 現在システムはこのエリアのチェーンネットワークをサポートしていません
20105 | chain-general-error | 目前该区块链网络已被禁用 | The block chain network is currently disabled | 現在この区画チェーンネットワークはすでに使用禁止されている
20106 | chain-general-error | 该区块链网络目前正在运行中 | The block chain network is currently in operation. | 同チェーンブロックインターネットは現在進行中
20107 | chain-general-error | 该类型已存在激活中的运行进程，若需开启更多同类进程需JP_MULTI_WORKERS特性支持 | "This type of running process already exists in activation |  and JP_MULTI_WORKERS feature support is required to open more similar processes." | このタイプは存在の活性化のプロセスを実行、もし必要多く同類のプロセスJP_MULTI_WORKERS支持必要特性
20108 | chain-general-error | 该方法仅支持UTXO类代币调用 | This method only supports UTXO class token invocation | この方法はUTXO類トークンの呼び出しのみに対応しています。
20109 | chain-general-error | 该类型在部分钱包中尚处于启用状态 | The one is still active in some wallets | このタイプは一部のウォレットでまだ有効です
20110 | chain-general-error | 调用区块链方法的参数不正确 | Incorrect parameters for calling block chain methods | ブロックチェーンのパラメータを呼び出すのは正しくない
20111 | chain-general-error | 删除前请先禁用该代币 | Disable token first | 先ずはトークンを無効にしてください
20112 | chain-general-error | 请确保区块链至少启动过一次 | Please make sure the blockchain has been started at least once | ブロックチェーンが少なくとも一回起動されたことを確認してください
20113 | chain-general-error | 该区块链不支持安全钱包模式 |  | 
20114 | chain-general-error | 该操作仅在普通钱包中支持 |  | 
20115 | chain-general-error | 系统检测到订单状态不正确，申请补发通知失败 | Request failed due to incorrect order status | Request failed due to incorrect order status
20200 | missing-method | 无法找到list方法 | Failed to find 'list' method | 「list」方法を見つけることができない
20201 | missing-method | 无法找到fetch方法 | Failed to find 'fetch' method | 「fetch」方法を見つけることができない
20202 | missing-method | 无法找到create方法 | Failed to find 'create' method | 「create」方法を見つけることができない
20203 | missing-method | 无法找到patch方法 | Failed to find 'patch' method | 「patch」方法を見つけることができない
20204 | missing-method | 无法找到delete方法 | Failed to find 'delete' method | 「delete」方法を見つけることができない
20205 | missing-method | 该方法未实现 | This method is not implemented | この方法は実現していない
20301 | chain-bitcoin | 非法参数 | invalid parameter | 不正パラメータ
20302 | chain-bitcoin | 发送Bitcoin交易失败 | failed to send bitcoin transaction | Bitcoin取引に失敗しました
20304 | chain-bitcoin | UTXO不足 | utxo not enough | UTXO不足
20305 | chain-bitcoin | 交易拼接后UTXO不足 | utxo not enough after tx compose | トランザクションの後にUTXOが不足しています
20322 | chain-cybex | 发送Cybex交易失败 | failed to send cybex transaction | Cybex取引失敗送信
20323 | chain-cybex | Cybex钱包余额不足 | cybex wallet without enough balance | Cybex財布の残高不足
20324 | chain-cybex | Cybex未找到合适的Keys | missing cybex keys | Cybexに適切なKeysが見つかりませんでした
20325 | chain-cybex | 扫描区块尚未处于LIB以下 | Scanning block should be below LIB | 走査ブロックまだは「LIB」以下
20326 | chain-cybex | 未找到指定Cybex区块 | cybex block not found | 見つからないで指定Cybexブロック
20327 | chain-cybex | 未找到指定Cybex账号 | cybex account not found | 見つからないで指定Cybexアカウント
20331 | chain-neo | Neo地址被冻结 | Neo address is frozen | Neoのアドレスが凍結された
20332 | chain-neo | Neo交易发送失败 | Failed to send Neo transaction | Neo取引に失敗しました
20340 | chain-vechain | VeChain交易发送失败 | Failed to send VeChain transaction | VeChain取引に失敗しました
21000 | rpc-failed | 该服务未找到相关信息 | The service did not find relevant information. | このサービスに関する情報は見つかりません
21001 | rpc-failed | 该服务尚未连接或名字空间错误 | The service has not been connected or has a namespace error | このサービスはまだ接続されていません。
21002 | rpc-failed | 该服务未找到合适的工作进程 请开启相关进程后重试 | The service did not find the right work process.Please launch the work process | このサービスのプロセスが見つかりません
21003 | rpc-failed | 该操作未找到目标进行执行 | The operation did not find the target for execution. | この操作は目標を発見していない
21004 | rpc-failed | JSONRPC服务未找到 | JSONRPC service not found | JSONRPCサービスは見つかりません
21005 | rpc-failed | 请求超时 | Request timeout | 超過を請求する
21006 | rpc-failed | 返回类型错误 | Return type error | 戻るタイプのエラー
21007 | rpc-failed | 未在数据库找到该链的标尺 | Specified ruler not found | この操作は目標を発見していない
22010 | job-flow-error | 订单处于该状态不能进行交易构建 | Unable to build raw tx due to improper status | Unable to build raw tx due to wrong status
22011 | job-flow-error | 手续费不足 | Insufficient fee | Insufficient fee
22012 | job-flow-error | 订单处于该状态不能进行交易发送 | Unable to send tx due to improper status | Unable to send tx due to wrong status
22013 | job-flow-error | 订单可能已被处理过 | Order might has already been processed due to improper status | Order might has already been processed due to improper status
22014 | job-flow-error | 无法找到交易原始数据 | Tx raw data missing | Tx raw data missing
22015 | job-flow-error | 缺少签名或签名验证失败 | Signature missing or verification failed | Signature missing or verification failed
22016 | job-flow-error | 余额不足 | Insufficient balance | Insufficient balance
22017 | job-flow-error | 无法获取余额 | Failed to get balance | Failed to get balance
22018 | job-flow-error | 构建交易过程异常 | Exception occurs while building tx | Exception occurs while building tx
22019 | job-flow-error | 订单未能参与构建 | Order was missed while building tx | Order was missed while building tx
22020 | job-flow-error | 无法在节点找到交易且不能断定交易一定失败 | Unable to find tx on node and cannot conclude that tx definitely failed | Unable to find tx on node and cannot conclude that tx definitely failed
22021 | job-flow-error |  |  | 
29001 | warning | 热钱包余额低于低水位 | Hot wallet balance below low water level | 熱財布の残高が低い水位を下回る
29002 | warning | 订单执行失败 | Failure of order execution | 注文に失敗した
29003 | warning | 智能合约发生故障 | Failure of Intelligent Contract | インテリジェント契約の故障
29004 | warning | 订单无法被处理 |  | 
29005 | warning | 订单处理被冻结 |  | 
29006 | warning | 订单处理时间过长 |  | 
29007 | warning | 第一次出现 FailBlockNumber（通用） |  | 
29008 | warning | FailedBlackNumber满之前（通用） |  | 
29009 | warning | FailedBlackNumber满 -》 nonce占用（特定） |  | 
29010 | warning | 出现 Expired 时（通用） |  | 
29011 | warning | failed bn => good bn 时 |  | 
29012 | warning | good bn => failed bn 时 |  | 
29013 | warning | 订单处理 holding => init 时 |  | 
29014 | warning | 订单处理 holding => online 时 |  | 
29015 | warning | 订单处理 holding => failed 时 |  | 
29016 | warning | 订单处理变为 cancelled 时 |  | 
40100 | authorization-failed | 用户认证失败 | User authentication failed | ユーザ認証失敗
40101 | authorization-failed | 缺少验证参数sig | Lack of validation parameter sig | 検証パラメータsig不足
40102 | authorization-failed | 缺少验证参数timestamp | Lack of validation parameter timestamp | 検証パラメータtimes tamp不足
40103 | authorization-failed | 缺少验证参数crypto | Lack of validation parameter crypto | 検証パラメータcrypto不足
40104 | authorization-failed | 未找到验证所需的公钥 | Failed to find required public key for validation | 検証に必要な公鍵が見つからない
40110 | authorization-failed | 该API仅限内部调用 | The API is internal only | このAPIは内部の呼び出しだけで
40300 | permission-failed | 无访问权限 | No access rights | アクセス権限
40301 | permission-failed | 未完成认证 | Uncompleted certification | 未完成の認証
40302 | permission-failed | 该操作被锁定 | The operation is locked | この操作ロックされて
40400 | resouce-not-found | 未找到指定订单号 | Failed to find the specified order id | 見つからない指定の注文番号
40401 | resouce-not-found | 未找到指定审计信息 | Failed to find the specified audit information | 見つからない指定の監査情報
40410 | resouce-not-found | 未找到指定对象 | Failed to find the specified object | 見つからない指定のオブジェクト
40411 | resouce-not-found | 未找到指定地址 | Failed to find the specified address | 見つからない指定のアドレス
50000 | service-failed | 未能找到Redis服务 | Failed to connect redis server | Redis服务接続失敗
50001 | service-failed | 未能找到Consul服务 | Failed to connect consul server | Consul服务接続失敗
50003 | service-failed | Redis查询错误 | Faild to fetch data from redis server | Redis検索失敗

