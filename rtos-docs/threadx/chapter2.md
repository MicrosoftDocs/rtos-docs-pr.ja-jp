---
title: 第 2 章 - Azure RTOS ThreadX のインストールと使用
description: この章では、Azure RTOS ThreadX の高パフォーマンス カーネルのインストール、設定、使用に関連するさまざまな問題について説明します。
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: e1bf85d363b07c81f226d494107eae9ba18114a7
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2021
ms.locfileid: "104810271"
---
# <a name="chapter-2---installation-and-use-of-azure-rtos-threadx"></a>第 2 章 - Azure RTOS ThreadX のインストールと使用

この章では、Azure RTOS ThreadX の高パフォーマンス カーネルのインストール、設定、使用に関連するさまざまな問題について説明します。

## <a name="host-considerations"></a>ホストに関する考慮事項

埋め込みソフトウェアは、通常は Windows または Linux (Unix) ホスト コンピューターで開発されています。 アプリケーションは、ホスト上でコンパイル、リンク、および配置された後、実行のためにターゲット ハードウェアにダウンロードされます。

通常、ターゲットへのダウンロードは、開発ツールのデバッガー内から実行されます。 ダウンロードの完了後、デバッガーは、メモリおよびプロセッサ レジスタへのアクセスだけでなく、ターゲットの実行制御 (移動、停止、ブレークポイントなど) を提供します。

開発ツール デバッガーのほとんどが、JTAG (IEEE 1149.1) やバックグラウンド デバッグ モード (BDM) などのオンチップ デバッグ (OCD) 接続を介してターゲット ハードウェアと通信します。 また、デバッガーは、インサーキット エミュレーション (ICE) 接続を使用してターゲット ハードウェアと通信することもあります。 OCD 接続と ICE 接続はどちらも、ターゲットの常駐ソフトウェアへの侵入を最小限に抑えた堅牢なソリューションを提供します。

ホスト上で使用されるリソースについては、ThreadX のソース コードは ASCII 形式で提供され、ホスト コンピューターのハード ディスク上に約 1 MB の空き領域が必要です。

## <a name="target-considerations"></a>ターゲットに関する考慮事項

ThreadX は、ターゲット上に 2 KB から 20 KB の読み取り専用メモリ (ROM) を必要とします。 また、ThreadX システム スタックおよびその他のグローバル データ構造用に、ターゲットのランダム アクセス メモリ (RAM) がさらに 1 KB から 2 KB 必要です。

サービス呼び出しタイムアウト、タイムスライス、アプリケーション タイマーなどのタイマー関連関数が機能するためには、基になるターゲット ハードウェアは定期的な割り込みソースを提供する必要があります。 プロセッサがこの機能を備えている場合は、ThreadX によって使用されます。 あるいは、ターゲット プロセッサに定期的な割り込みを生成する機能がない場合は、ユーザーのハードウェアがそれを提供する必要があります。 タイマー割り込みの設定と構成は、通常は ThreadX ディストリビューションの ***tx_initialize_low_level*** アセンブリ ファイルにあります。

> [!NOTE]
> *定期的なタイマー割り込みソースを使用できない場合でも、ThreadX は機能します。ただし、タイマー関連サービスはいずれも機能しません。*

## <a name="product-distribution"></a>製品の配布

Azure RTOS ThreadX は、<https://github.com/azure-rtos/threadx/> のパブリック ソース コード リポジトリから入手できます。

以下は、リポジトリ内のいくつかの重要なファイルの一覧です。

| ファイル名 | 説明 |
|------------------- | ----------- |
| **tx_api.h**                      | すべてのシステム等価物、データ構造、およびサービス プロトタイプが含まれる C ヘッダーファイル。                                                             |
| **tx_port.h**                     | すべての開発ツール、およびターゲット固有のデータの定義と構造が含まれる C ヘッダー ファイル。                                                 |
| **demo_threadx.c**                | 小規模なデモ アプリケーションが含まれる C ファイル。                                                                                                       |
| **tx.a (または tx.lib)**              | "*標準*" パッケージと共に配布される ThreadX C ライブラリのバイナリ バージョン。                                                          |
|                                   |                                                                                                                                                   |

>[!NOTE]
>*ファイル名はすべて小文字で表記します。この名前付け規則を使用することで、コマンドを Linux (Unix) 開発プラットフォームに簡単に変換できます。*

## <a name="threadx-installation"></a>ThreadX のインストール

ThreadX は、GitHub リポジトリをローカル コンピューターに複製することによってインストールされます。 お使いの PC 上に ThreadX リポジトリの複製を作成するための一般的な構文を次に示します。

```c
    git clone https://github.com/azure-rtos/threadx
```

GitHub メイン ページ上のダウンロード ボタンを使用して、リポジトリのコピーをダウンロードすることもできます。

また、オンライン リポジトリのフロント ページ上で ThreadX ライブラリを構築する手順も紹介します。

> [!NOTE]
> *アプリケーション ソフトウェアが ThreadX SMP ライブラリ ファイル (通常は **tx.a** または **tx.lib**) と、C インクルード ファイル **_tx_api.h_* _ および _*_tx_port.h_*_ にアクセスする必要があります。 そのためには、開発ツールで適切なパスを設定するか、アプリケーション開発を行う領域にこれらのファイルをコピーします。_

## <a name="using-threadx"></a>ThreadX を使用する

ThreadX を使用するには、アプリケーション コードには ***tx_api.h** _ をコンパイル中に含めて、ThreadX SMP ランタイム ライブラリ _*_tx.a_*_ (または _*_tx.lib)_ ** とリンクする必要があります。

ThreadX アプリケーションをビルドするには、次の 4 つの手順が必要です。

1. ***tx_api.h*** ファイルを、ThreadX のサービスまたはデータ構造を使用するすべてのアプリケーション ファイル内に追加します。

1. 標準の C ***main** _ 関数を作成します。 この関数が最終的に _ *_tx_kernel_enter_** を呼び出して THREADX を開始する必要があります。 ThreadX に関係しないアプリケーション固有の初期化が、カーネルに入る前に追加される場合があります。

      > [!IMPORTANT]
      > *ThreadX のエントリ関数 ***tx_kernel_enter** _ は戻りません。 したがって、その後で一切の処理または関数の呼び出しを行わないようにしてください。_

1. ***tx_application_define*** 関数を作成します。 ここに、最初のシステム リソースが作成されます。 システム リソースの例としては、スレッド、キュー、メモリ プール、イベント フラグ グループ、ミューテックス、セマフォなどがあります。

1. アプリケーション ソースをコンパイルして、ThreadX ランタイム ライブラリ ***tx.lib*** にリンクします。 結果のイメージはターゲットにダウンロードして、実行できます。

## <a name="small-example-system"></a>小規模のシステム例

図 1 の小規模のシステム例は、優先度が 3 のシングル スレッドの作成を示しています。 スレッドは実行されて、カウンターが増え、時計の 1 目盛り分の間スリープ状態になります。
このプロセスは永久に続きます。

```c
#include "tx_api.h"
unsigned long my_thread_counter = 0;
TX_THREAD my_thread;
main( )
{
    /* Enter the ThreadX kernel. */
    tx_kernel_enter( );
}
void tx_application_define(void *first_unused_memory)
{
    /* Create my_thread! */
    tx_thread_create(&my_thread, "My Thread",
    my_thread_entry, 0x1234, first_unused_memory, 1024,
    3, 3, TX_NO_TIME_SLICE, TX_AUTO_START);
}
void my_thread_entry(ULONG thread_input)
{
    /* Enter into a forever loop. */
    while(1)
    {
        /* Increment thread counter. */
        my_thread_counter++;
        /* Sleep for 1 tick. */
        tx_thread_sleep(1);
    }
}
```

**図 1. アプリケーション開発のためのテンプレート**

これは簡単な例ですが、実際のアプリケーション開発に適したテンプレートとなります。

## <a name="troubleshooting"></a>トラブルシューティング

各 ThreadX ポートには、デモンストレーション アプリケーションが付属しています。 実際のターゲット ハードウェアまたはシミュレートされた環境で、最初にデモンストレーション システムを稼働させることをお勧めします。

デモンストレーション システムが正常に実行されない場合のトラブルシューティングのヒントを、以下に示します。

1. 実行中のデモンストレーションの量を確認します。
1. スタック サイズを増やします (これは、デモンストレーションよりも実際のアプリケーション コードでより重要です)。
1. TX_ENABLE_STACK_CHECKING を定義して ThreadX ライブラリをリビルドします。 これにより、組み込みの ThreadX スタック チェックが有効になります。
1. 最近の変更を一時的にバイパスして、問題が解消または変更するかどうかを確認します。 このような情報は、サポート エンジニアにとって有益です。

「[カスタマー サポート センター](about-this-guide.md#customer-support-center)」に記載されている手順に従って、トラブルシューティング手順で収集した情報を送信します。

## <a name="configuration-options"></a>構成オプション

ThreadX を使用して ThreadX ライブラリとアプリケーションをビルドする場合、構成オプションがいくつかあります。 以下のオプションは、アプリケーション ソース内、コマンド ライン上、または ***tx_user.h*** インクルード ファイル内で定義できます。

> [!IMPORTANT]
> ****tx_user.h** _ 内で定義されているオプションは、アプリケーションと ThreadX ライブラリが、_ *TX_INCLUDE_USER_DEFINE_FILE** を定義してビルドされている場合にのみ適用されます。*

### <a name="smallest-configuration"></a>最小の構成

コード サイズを最小にするには、次の ThreadX 構成オプションを考慮する必要があります (他のすべてのオプションが存在しない場合)。

```c
TX_DISABLE_ERROR_CHECKING
TX_DISABLE_PREEMPTION_THRESHOLD
TX_DISABLE_NOTIFY_CALLBACKS
TX_DISABLE_REDUNDANT_CLEARING
TX_DISABLE_STACK_FILLING
TX_NOT_INTERRUPTABLE
TX_TIMER_PROCESS_IN_ISR
```

### <a name="fastest-configuration"></a>最も速い構成

実行速度を最速にするには、以前の最小の構成で使用したものと同じ構成オプションを使用します。ただし、これらのオプションも考慮します。

```c
TX_REACTIVATE_INLINE
TX_INLINE_THREAD_RESUME_SUSPEND
```

[詳細な構成オプション](#detailed-configuration-options)が説明されています。

### <a name="global-time-source"></a>グローバル タイム ソース

他の Microsoft Azure RTOS 製品 (FileX、NetX、GUIX、USBX など) では、ThreadX で 1 秒を表す ThreadX タイマー刻みの数が定義されています。 この定数に基づいて各種の時間要件が導き出されます。 既定では、この値は 100 で、10 ミリ秒の定期的割り込みを想定しています。 この値をユーザーがオーバーライドするには、***tx_port.h*** または IDE あるいはコマンド ライン内で TX_TIMER_TICKS_PER_SECOND を必要な値で定義します。

### <a name="detailed-configuration-options"></a>詳細な構成オプション

**TX_BYTE_POOL_ENABLE_PERFORMANCE_INFO**

このオプションを定義すると、バイト プールのパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_BYTE_POOL_ENABLE_PERFORMANCE_INFO**

このオプションを定義すると、バイト プールのパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_DISABLE_ERROR_CHECKING**

基本的なサービス呼び出しエラー チェックをバイパスします。 アプリケーション ソースで定義されていると、すべての基本パラメーター エラー チェックが無効になります。 これによりパフォーマンスが最大 30% 向上する場合があり、イメージ サイズも小さくなる可能性があります。

> [!NOTE]
> *エラー チェックを無効にしても安全なのは、アプリケーションですべての入力パラメーターが、外部入力から派生した入力パラメーターも含めて、すべての状況で常に有効であると保証される場合だけです。無効な入力が API に提供され、エラー チェックが無効になっている場合、その結果の動作は定義されていないため、メモリが破損したりシステムがクラッシュする可能性があります。*

> [!NOTE]
> *エラー チェックを無効にすることの影響がない ThreadX API の戻り値を、第 4 章の各 API の説明の「戻り値」セクションに太字で示しています。太字でない戻り値は、TX_DISABLE_ERROR_CHECKING オプションを使用してエラー チェックを無効にしている場合は void になります。*

**TX_DISABLE_NOTIFY_CALLBACKS**

定義されている場合、さまざまな ThreadX オブジェクトの通知コールバックが無効になります。 このオプションを使用するとコード サイズが若干減り、パフォーマンスが向上します。 既定では、このオプションは定義されていません。

**TX_DISABLE_PREEMPTION_THRESHOLD**

定義されている場合、プリエンプションしきい値機能が無効になり、コード サイズが若干減って、パフォーマンスが向上します。 もちろん、プリエンプションしきい値機能は使用できなくなります。 既定では、このオプションは定義されていません。

**TX_DISABLE_REDUNDANT_CLEARING**

定義されている場合、ThreadX のグローバル C データ構造をゼロに初期化するためのロジックが削除されます。 コンパイラの初期化コードが初期化されていないすべての C グローバル データをゼロに設定する場合のみに使用してください。 このオプションを使用するとコード サイズが若干減り、初期化中のパフォーマンスが向上します。 既定では、このオプションは定義されていません。

**TX_DISABLE_STACK_FILLING**

定義されている場合、各スレッドのスタックの作成時に各バイトへの 0xEF 値の配置が無効になります。 既定では、このオプションは定義されていません。

**TX_ENABLE_EVENT_TRACE**

定義されている場合、ThreadX で TraceX トレース バッファーを作成するためのイベント収集コードが有効になります。

**TX_ENABLE_STACK_CHECKING**

定義されている場合、ThreadX ランタイム スタック チェックが有効になります。これには、スタック使用量の解析と、スタック領域の前後のデータ パターン "フェンス"の検査が含まれます。 スタック エラーが検出されると、登録されているアプリケーション スタック エラー ハンドラーが呼び出されます。 このオプションにより、オーバーヘッドとコード サイズが若干増加します。 詳細については、***tx_thread_stack_error_notify*** API 関数を参照してください。 既定では、このオプションは定義されていません。

**TX_EVENT_FLAGS_ENABLE_PERFORMANCE_INFO**

定義されている場合、イベント フラグ グループに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_INLINE_THREAD_RESUME_SUSPEND**

定義されている場合、ThreadX でインライン コードを介した ***tx_thread_resume** _ および _ *_tx_thread_suspend_** の API 呼び出しが向上します。 これによりコード サイズが増えますが、これら 2 つの API 呼び出しのパフォーマンスが向上します。

**TX_MAX_PRIORITIES**

ThreadX の優先度レベルを定義します。 有効な値は 32 から 1024 の範囲 (両端を含む) で、32 で均等に割り切れ "*なければなりません*"。 サポートされている優先度レベルの数を増やすと、優先度 を 32 上げるごとに RAM 使用量が 128 バイト増えます。 ただし、パフォーマンスへの影響はごくわずかです。 既定では、この値は 32 の優先度レベルに設定されます。

**TX_MINIMUM_STACK**

最小スタック サイズ (バイト単位) を定義します。 これは、スレッドが作成されるときのエラー チェックに使用されます。 既定値はポート固有であり、***tx_port.h*** にあります。

**TX_MISRA_ENABLE**

定義されている場合、ThreadX が MISRA C 準拠規則を使用します。 詳細については、***ThreadX_MISRA_Compliance.pdf*** を参照してください。

**TX_MUTEX_ENABLE_PERFORMANCE_INFO**

定義されている場合、ミューテックスに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_NO_TIMER**

定義されている場合、ThreadX タイマー ロジックが完全に無効になります。 これは、ThreadX のタイマー機能 (スレッド スリープ、API タイムアウト、タイムスライス、アプリケーション タイマー) を使用しない場合に便利です。 **TX_NO_TIMER** が指定されている場合は、オプション **TX_TIMER_PROCESS_IN_ISR** も定義する必要があります。

**TX_NOT_INTERRUPTABLE**

定義されている場合、ThreadX は割り込みロックアウト時間を最小に抑えようとしません。 この結果、実行速度が速くなりますが、割り込みロックアウト時間が若干長くなります。

**TX_QUEUE_ENABLE_PERFORMANCE_INFO**

定義されている場合、キューに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_REACTIVATE_INLINE**

定義されている場合、関数呼び出しを使用する代わりに、ThreadX タイマーの再アクティブ化がインラインで実行されます。 これによりパフォーマンスが向上しますが、コード サイズが若干増えます。 既定では、このオプションは定義されていません。

**TX_SEMAPHORE_ENABLE_PERFORMANCE_INFO**

定義されている場合、セマフォに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_THREAD_ENABLE_PERFORMANCE_INFO**

定義されている場合、スレッドに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_TIMER_ENABLE_PERFORMANCE_INFO**

定義されている場合、タイマーに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。

**TX_TIMER_PROCESS_IN_ISR**

定義されている場合、ThreadX の内部システム タイマー スレッドは除去されます。 この結果、タイマー スタックと制御ブロックが不要になるため、タイマー イベントのパフォーマンスが向上し、RAM の要件が小さくなります。 ただし、このオプションを使用すると、タイマー期限切れ処理がすべてタイマー ISR レベルに移動します。 既定では、このオプションは定義されていません。

> [!NOTE]
> *タイマーから許可されているサービスは ISR から許可されない可能性があるため、このオプションを使用していると許可されない場合があります。*

**TX_TIMER_THREAD_PRIORITY**

ThreadX の内部システム タイマー スレッドの優先度を定義します。 既定値は優先度 0 で、ThreadX で最も高い優先度です。 既定値は ***tx_port.h*** で定義されています。

**TX_TIMER_THREAD_STACK_SIZE**

ThreadX の内部システム タイマー スレッドのスタック サイズ (バイト単位) を定義します。 このスレッドは、すべてのスレッド スリープ要求だけでなく、すべてのサービス呼び出しタイムアウトも処理します。 さらに、すべてのアプリケーション タイマー コールバック ルーチンはこのコンテキストから呼び出されます。 既定値はポート固有であり、***tx_port.h*** にあります。

## <a name="threadx-version-id"></a>ThreadX のバージョン ID

プログラマーは ***tx_port.h** _ ファイルを調べることで ThreadX のバージョンを確認できます。 加えて、このファイルには、対応するポートのバージョン履歴も含まれています。 アプリケーション ソフトウェアは、グローバル文字列 _*_tx_version_id** を調べることで ThreadX のバージョンを取得できます。