---
title: 第 1 章 - Azure RTOS NetX Duo SMTP クライアントの概要
description: 簡易メール転送プロトコル (SMTP) は、ネットワークとインターネットを介してメールを転送するためのプロトコルです。
author: philmea
ms.author: philmea
ms.date: 07/09/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: df8c6b6920577ebfc18ed9252761401c30822c034e30d7ae95b25778707f53d5
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2021
ms.locfileid: "116797824"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-duo-smtp-client"></a>第 1 章 - Azure RTOS NetX Duo SMTP クライアントの概要

簡易メール転送プロトコル (SMTP) は、ネットワークとインターネットを介してメールを転送するためのプロトコルです。 このプロトコルでは、信頼性の高い伝送制御プロトコル (TCP) サービスを利用して、コンテンツ転送機能を実行します。

## <a name="netx-duo-smtp-client-requirements"></a>NetX Duo SMTP クライアントの要件

NetX Duo SMTP クライアントでは、NetX Duo IP インスタンスと NetX Duo パケット プールを作成する必要があります。 SMTP クライアントは、TCP ソケットを使用して、*既知のポート 25 で SMTP サーバーに接続します。そのため*、事前に作成した IP インスタンスで *nx_tcp_enable* サービスを呼び出して、まず TCP を有効にする必要があります。

SMTP クライアントの作成呼び出し (nxd_smtp_client_create) では、サーバーに SMTP コマンドを送信してさらに実際のメール メッセージを送信するために、事前に作成したパケット プールが必要です。 パケット ペイロードは、メール コンテンツの予想されるサイズに依存し、TCP、IP ヘッダー、および MAC ヘッダーを許可する必要があります (IPv6 ヘッダーは 40 バイトで、IPv4 ヘッダーは 20 バイトであることに注意してください)。

メール メッセージ全体が 1 つのパケットに収まりきらない場合、SMTP クライアントによって、メッセージの残りの部分を格納するための追加のパケットが割り当てられます。

## <a name="netx-duo-smtp-client-constraints"></a>NetX Duo SMTP クライアントの制約

NetX Duo SMTP プロトコルでは RFC 2821 と 2554 標準が実装されますが、次のようないくつかの制約があります。

1. NetX Duo SMTP クライアントは、LOGIN と PLAIN 認証のみをサポートし、CRAM MD5 ダイジェスト認証はサポートしていません。
2. NetX Duo SMTP クライアント メッセージは、メール アイテムごとに 1 つの受信者と、SMTP サーバーとの TCP 接続ごとに 1 つのメール メッセージのみに制限されます。
3. VRFY、SEND、SOML、EXPN、SAML、ETRN、TURN、SIZE SMTP オプションはサポートされていません。
4. SMTP クライアントは、通常メール メッセージの作成に使用されるメール ブラウザー ("メール ユーザー エージェント") ではありません。 これは "メール転送エージェント" です。 これは、RFC 2821 で指定されている SMTP 転送のメール メッセージ本文の必要な処理を提供します。 たとえば、受信者と逆方向の経路など、コンテンツの構文が正しいかどうかはチェックしません。 MIME データやクリア テキスト メッセージなど、メール バッファーの内容には、制限はありません。 RFC 2822 で指定されている、ヘッダーとメッセージ本文を含めるためのメール メッセージ形式は、SMTP クライアント API の範囲を超えています。

## <a name="commands-supported-by-netx-duo-smtp-client"></a>NetX Duo SMTP クライアントでサポートされているコマンド

NetX Duo SMTP クライアントでは、SMTP サーバーとのメール セッション中に次のコマンドが使用されます。

- **EHLO** クライアントから、SMTP サーバーから利用可能な一部または全部の拡張プロトコル SMTP サービスを含むセッションの開始が求められています。 既定値です。
- **HELO** クライアントから、基本的な SMTP サービスに限定されたセッションの開始が求められています。
- **MAIL** クライアントから、サーバーでのクライアント メールの受信が求められています。
- **AUTH** クライアントから、サーバーによる認証の開始が求められています。
- **RCPT** クライアントから、メールの配信先として使用する別のホストのメールボックスの送信が求められています。
- **DATA** クライアントから、サーバーへのメール メッセージ データの送信開始が求められています。
- **QUIT** クライアントによって、セッションの終了が求められています。

## <a name="getting-started"></a>作業の開始

SMTP クライアント アプリケーションによって IP インスタンスが作成され、その IP インスタンスで TCP が有効になります。 その後、次のサービスを使用して SMTP クライアントが作成されます。

```C
UINT nxd_smtp_client_create(NX_SMTP_CLIENT *client_ptr,
    NX_IP *ip_ptr, NX_PACKET_POOL
    *client_packet_pool_ptr,
    CHAR *username, CHAR *password,
    CHAR *from_address,
    CHAR *client_domain,
    UINT authentication_type,
    NXD_ADDRESS *server_address, UINT port);
```

*Client_packet_pool_ptr* は、SMTP クライアントが SMTP サーバーにメッセージを送信するために使用する、以前に作成されたパケット プールへのポインターです。

アプリケーションは、ローカル デバイスの *from_address* とサーバーの IP アドレスを提供する必要があることに注意してください。 すべてのアドレスは完全修飾ドメイン名である必要があります。 完全修飾ドメイン名には、"@" 文字で区切られた、ローカル部分とドメイン名が含まれます。 SMTP クライアントは、次の nx_smtp_mail_send サービスで *from_address* または *recipient_address* の構文をチェックしないことに注意してください。

SMTP クライアントが作成されると、SMTP クライアント アプリケーションによって適切な形式の SMTP メール メッセージを含むメール アイテムが作成され、次の API を使用して、SMTP クライアントにメール アイテムの送信が要求されます。

```C
UINT nx_smtp_mail_send(NX_SMTP_CLIENT *client_ptr,
    CHAR *recipient_address, UINT priority,
    CHAR *subject, CHAR *mail_body,
    UINT mail_body_length);
```

IPv4 または IPv6 経由の SMTP クライアントを実行する場合、ユーザーの観点からは基本的に違いはありません。 2 つの IP プロトコルの違いは、基になる NetX Duo レイヤーで処理されます。

メールを送信するアプリケーションでは、*nx_smtp_client_mail* の呼び出しで受信者アドレスを指定する必要があることに注意してください。

認証の場合、ユーザー名には完全修飾ドメイン名を使用するか、ユーザー名を表示することができます。 これは、サーバーでの認証方法によって決まります。

このユーザー ガイドで後述する「小さい例」セクションのデモで、メッセージの書式設定方法を示します。 メール アイテムが正常に送信された場合のステータスは NX_SUCCESS になります。 エラーが発生した場合、それが内部エラー、TCP 接続の切断、またはサーバーの応答エラー コードの受信のいずれであっても、*nx_smtp_mail_send* によって 0 以外のエラー状態が返されます。

メール アイテムを送信するときに、NetX Duo SMTP クライアントによって SMTP サーバーとの新しい TCP 接続が作成され、SMTP セッションが開始されます。 このセッションでは、クライアントによって SMTP プロトコルの一部として SMTP サーバーに一連のコマンドが送信され、最終的に実際のメール メッセージが送信されます。 その後で、SMTP セッションの結果に関係なく、TCP 接続が終了します。

メールの送信後、成功または失敗に関係なく、SMTP クライアントは "初期" 状態に戻り、別のメール転送セッションに使用できます。

## <a name="netx-duo-smtp-authentication"></a>NetX Duo SMTP 認証

認証は、SMTP クライアントが SMTP サーバーに対して ID を証明し、信頼できるユーザーとしてクライアントのメールを配信させる方法です。 ほとんどの商用 SMTP サーバーでは、クライアントが認証される必要があります。

通常、認証データは、送信者のユーザー名とパスワードで構成されます。 認証チャレンジ中に、サーバーによってこの情報が要求され、クライアントからは要求されたデータをエンコードされた形式での送信という形で応答が行われます。 サーバーでデータがデコードされ、ユーザー データベース内で一致するデータの照合が試みられます。 見つかった場合、サーバーによって認証が成功したことが示されます。 SMTP 認証は [RFC 2554](http://www.ietf.org/rfc/rfc2554.txt) で定義されています。

認証には、"*基本*" と "*ダイジェスト*" の 2 種類があります。 ダイジェストは、現在の NetX Duo SMTP クライアントではサポートされていないので、ここでは説明しません。 基本認証は、前述の "*名前*" と "*パスワード*" の認証に相当します。 SMTP 基本認証では、名前とパスワードは base64 でエンコードされます。 基本認証の利点は、実装が簡単で、広く使用されていることです。 基本認証の主な欠点は、名前とパスワードが要求でオープンに送信されることです。

### <a name="plain-authentication"></a>プレーン認証

NetX Duo SMTP クライアントからは、PLAIN パラメーターを指定した AUTH コマンドが送信されます。 NetX Duo SMTP サーバーでこの種類の認証がサポートされている場合、334 応答コードで応答が行われます。 クライアントからは、1 つの base64 でエンコードされたユーザー名とパスワードのメッセージでサーバーに応答します。 サーバーでクライアント認証が成功したと判断された場合は、235 の成功コードで応答します。

### <a name="login-authentication"></a>ログイン認証

NetX Duo SMTP クライアントからは、LOGIN パラメーターを指定して AUTH コマンドが送信されます。 NetX Duo SMTP サーバーでこの種類の認証がサポートされている場合は、認証 "チャレンジ" の最初に 334 応答コードで応答します。 Base64 でエンコードされたプロンプトをクライアントに送り返します。これは通常 "Username" です。 クライアントによってプロンプトがデコードされ、base64 でエンコードされたユーザー名が返されます。 サーバーでクライアントのユーザー名が受け入れられると、クライアント パスワードを要求する base64 でエンコードされたプロンプトが送信されます。 クライアントからは base64 でエンコードされたパスワードが返されます。 サーバーでクライアント認証が成功したと判断された場合は、235 の成功コードで応答します。

### <a name="no-authentication"></a>[認証なし]

一部の SMTP サーバーは、認証なしで構成されています。 その場合、クライアントの EHLO メッセージに対する 250 の応答には、認証の種類のリストが表示されません。 ただし、どの認証の種類もリストに表示されないことが、必ずしもサーバーが認証を必要としない、またはサポートしていないことを意味するわけではありません。 このような状況でクライアントが PLAIN 認証または LOGIN 認証用に構成されている場合、NetX Duo クライアント スレッド タスクの既定値は PLAIN になります。 クライアントが NONE に構成されている場合、認証ステップはスキップされ、SMTP の状態は MAIL の状態に進みます。

クライアントが認証なしで構成されていて、SMTP サーバーで認証がサポートされている場合、クライアント認証の種類は PLAIN に切り替わります。

## <a name="rfcs-supported-by-netx-duo-smtp-client"></a>NetX Duo SMTP クライアントでサポートされている RFC

NetX Duo SMTP クライアント API は、RFC2821 "Simple Mail Transfer Protocol (簡易メール転送プロトコル)" および RFC 2554 の "SMTP Service Extension for Authentication (認証のための SMTP サービス拡張)" に準拠しています。 “
