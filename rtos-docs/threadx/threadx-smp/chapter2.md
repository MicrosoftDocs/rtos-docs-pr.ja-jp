---
title: 第 2 章 - Azure RTOS ThreadX SMP のインストールと使用
description: この章では、Azure RTOS ThreadX SMP の高パフォーマンス カーネルのインストール、設定、および使用に関連するさまざまな問題について説明します。
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: d0a63f3798adbc634a43cdda7e9d44941de655d9333f9ae0fb4181f1a6c0566e
ms.sourcegitcommit: 93d716cf7e3d735b18246d659ec9ec7f82c336de
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2021
ms.locfileid: "116801905"
---
# <a name="chapter-2---installation--use-of-azure-rtos-threadx-smp"></a>第 2 章 - Azure RTOS ThreadX SMP のインストールと使用

この章では、Azure RTOS ThreadX SMP の高パフォーマンス カーネルのインストール、設定、および使用に関連するさまざまな問題について説明します。

## <a name="host-considerations"></a>ホストに関する考慮事項

埋め込みソフトウェアは、通常は Windows または Linux (Unix) ホスト コンピューターで開発されています。 アプリケーションは、ホスト上でコンパイル、リンク、および配置された後、実行のためにターゲット ハードウェアにダウンロードされます。

通常、ターゲットのダウンロードは、開発ツールのデバッガー内から実行されます。 ダウンロード後、デバッガーは、メモリおよびプロセッサ レジスタへのアクセスだけでなく、ターゲットの実行制御 (移動、停止、ブレークポイントなど) を提供します。

ほとんどの開発ツールのデバッガーでは、JTAG (IEEE 1149.1) やバックグラウンド デバッグ モード (BDM) などのオンチップ デバッグ (OCD) 接続でターゲット ハードウェアと通信します。 また、デバッガーは、インサーキット エミュレーション (ICE) 接続を使用してターゲット ハードウェアと通信します。 OCD 接続と ICE 接続はどちらも、ターゲットの常駐ソフトウェアへの侵入を最小限に抑えた堅牢なソリューションを提供します。

ホスト上で使用されるリソースについては、ThreadX SMP のソースコードは ASCII 形式で提供され、ホスト コンピューターのハード ディスク上に約 1 MB の空き領域が必要です。

> [!IMPORTANT]
> ホスト システムに関する他の考慮事項とオプションについては、付属の **readme_threadx.txt** ファイルを参照してください。

## <a name="target-considerations"></a>ターゲットに関する考慮事項

ThreadX SMP は、ターゲット上に 2 KB から 20 KB の読み取り専用メモリ (ROM) を必要とします。 また、ThreadX SMP システム スタックおよびその他のグローバル データ構造用に、ターゲットのランダム アクセス メモリ (RAM) がさらに 1 KB から 2 KB 必要です。

サービス呼び出しタイムアウト、タイムスライス、アプリケーション タイマーなどのタイマー関連関数が機能するためには、基になるターゲット ハードウェアは定期的な割り込みソースを提供する必要があります。 プロセッサがこの機能を備えている場合は、ThreadX SMP によって使用されます。 あるいは、ターゲット プロセッサに定期的な割り込みを生成する機能がない場合は、ユーザーのハードウェアがそれを提供する必要があります。 タイマー割り込みのセットアップと構成は、通常は ThreadX SMP ディストリビューションの ***tx_initialize_low_level*** アセンブリ ファイルにあります。

> [!IMPORTANT]
> ThreadX SMP は、定期的なタイマー割り込みソースがない場合でも機能します。 ただし、タイマー関連サービスは機能しません。 ホスト システムに関する他の考慮事項やオプションについては、付属の **readme_threadx.txt** ファイルを参照してください。

## <a name="product-distribution"></a>製品の配布

ディストリビューション ディスクの詳しい内容は、ターゲット プロセッサ、開発ツール、購入された ThreadX SMP パッケージによって異なります。 ほとんどの製品ディストリビューションに共通するいくつかの重要ファイルの一覧を次に示します。

### <a name="threadx_express_startuppdf"></a>ThreadX_Express_Startup.pdf

この PDF では、ThreadX SMP を特定のターゲット プロセッサ/ボードおよび特定の開発ツールで動作させるための簡単な 4 段階の手順を説明しています。

### <a name="readme_threadxtxt"></a>readme_threadx.txt

ThreadX SMP ポートに関する特定の情報と、ターゲット プロセッサおよび開発ツールに関する情報が記載されたテキスト ファイルです。

| ツール | 説明 |
| -------------- | ------------------------------------------------------------------------------------------------- |
| **tx_api.h**  | すべてのシステム等価物、データ構造、およびサービス プロトタイプが含まれる C ヘッダーファイル。             |
| **tx_port.h** | すべての開発ツール、およびターゲット固有のデータの定義と構造が含まれる C ヘッダー ファイル。 |
|**demo_threadx.c**| 小規模なデモ アプリケーションが含まれる C ファイル。|
|**tx.a (または tx.lib)**| "*標準*" パッケージと共に配布される ThreadX SMP C ライブラリのバイナリ バージョン。|

> [!IMPORTANT]
> ファイル名はすべて小文字で表記します。 この名前付け規則を使用することで、コマンドを Linux (Unix) 開発プラットフォームに簡単に変換できます。

## <a name="threadx-smp-installation"></a>ThreadX SMP のインストール

ThreadX SMP のインストールは簡単です。 ***ThreadX_Express_Startup.pdf** _ ファイルと _ *_readme_threadx.txt_** ファイルで、ThreadX SMP を特定の環境でインストールする場合の具体的な情報について参照してください。

> [!IMPORTANT]
> かならず ThreadX SMP ディストリビューション ディスクのバックアップを取って、安全な場所に保管してください。

> [!IMPORTANT]
> アプリケーション ソフトウェアが ThreadX SMP ライブラリ ファイル (通常は **tx.a** または **tx.lib**) と、C インクルード ファイル **tx_api.h** および **tx_port.h** にアクセスする必要があります。 これを行うには、開発ツールの適切なパスを設定するか、これらのファイルをアプリケーション開発領域にコピーします。

## <a name="using-threadx-smp"></a>ThreadX SMP を使用する

ThreadX SMP は簡単に使用できます。 基本的に、アプリケーション コードには ***tx_api.h** _ をコンパイル中に含めて、ThreadX SMP ランタイム ライブラリ _*_tx.a_*_ (または _*_tx.lib)_ ** とリンクする必要があります。

ThreadX SMP アプリケーションをビルドするには、次の 4 つの手順が必要です。

***tx_api.h*** ファイルを、ThreadX SMP のサービスまたはデータ構造を使用するすべてのアプリケーション ファイル内にインクルードします。

標準の C ***main** _ 関数を作成します。 この関数が最終的に _ *_tx_kernel_enter_** を呼び出して THREADX SMP を開始する必要があります。 ThreadX SMP に関係しないアプリケーション固有の初期化が、カーネルに入る前に追加される場合があります。

> [!IMPORTANT]
> ThreadX SMP のエントリ関数 **tx_kernel_enter** は戻りません。 したがって、その後でいっさいの処理または関数の呼び出しを行わないようにしてください。

***tx_application_define*** 関数を作成します。 ここに、最初のシステム リソースが作成されます。 システム リソースの例としては、スレッド、キュー、メモリ プール、イベント フラグ グループ、ミューテックス、セマフォなどがあります。

アプリケーション ソースをコンパイルして、ThreadX SMP ランタイム ライブラリ ***tx.lib*** にリンクします。 結果のイメージはターゲットにダウンロードして、実行できます。

## <a name="small-example-system"></a>小規模システムの例

28 ページの図 1 の小規模システムの例は、優先度が 3 のシングル スレッドの作成を示しています。 スレッドは実行されて、カウンターが増え、1 クロック ティック間スリープ状態になります。 このプロセスは永久に続きます。

```c
#include              "tx_api.h"

unsigned long         my_thread_counter = 0;
TX_THREAD             my_thread;

main( )
{
      /* Enter the ThreadX SMP kernel. */
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

これは簡単な例ですが、実際のアプリケーション開発に適したテンプレートとなります。 もういちど、***readme_threadx.txt*** ファイルで詳細をご覧ください。

## <a name="troubleshooting"></a>トラブルシューティング

各 ThreadX SMP ポートには、デモンストレーション アプリケーションが付属しています。 最初にデモンストレーション システムを、実際のターゲット ハードウェアまたはシミュレートされた環境で稼働させることをお勧めします。

> [!IMPORTANT]
> ディストリビューションに付属している **readme_threadx.txt** ファイルで、デモンストレーション システムに関する詳細をご確認ください。

デモンストレーション システムが正常に実行されない場合のトラブルシューティングのヒントを、以下に示します。

  - デモンストレーションがどの程度実行されているかを調べます。

  - スタック サイズを増やします (これは、デモンストレーションよりも実際のアプリケーション コードでより重要です)。

  - TX_ENABLE_STACK_CHECKING を定義して ThreadX SMP ライブラリをリビルドします。 これにより、組み込みの ThreadX SMP スタック チェックが有効になります。

  - 最近の変更を一時的にバイパスして、問題が解消または変更するかどうかを確認します。 このような情報は、サポート エンジニアにとって有益です。

トラブルシューティング手順で収集した情報を送信するには、12 ページの「必要なもの」に記載されている手順に従ってください。

## <a name="configuration-options"></a>構成オプション

ThreadX SMP を使用して ThreadX SMP ライブラリとアプリケーションをビルドする場合、構成オプションがいくつかあります。 以下のオプションは、アプリケーション ソース内、コマンド ライン上、または ***tx_user.h*** インクルード ファイル内で定義できます。

> [!IMPORTANT]
> **tx_user.h** 内で定義されているオプションは、アプリケーションと ThreadX SMP ライブラリが、**TX_INCLUDE_USER_DEFINE_FILE** を定義してビルドされている場合にのみ適用されます。

### <a name="smallest-configuration"></a>最小の構成
コード サイズを最小にするには、次の ThreadX SMP 構成オプションを考慮する必要があります (他のすべてのオプションが存在しない場合)。

- TX_DISABLE_ERROR_CHECKING
- TX_DISABLE_PREEMPTION_THRESHOLD
- TX_DISABLE_NOTIFY_CALLBACKS 
- TX_DISABLE_REDUNDANT_CLEARING 
- TX_DISABLE_STACK_FILLING 
- TX_NOT_INTERRUPTABLE 
- TX_TIMER_PROCESS_IN_ISR

### <a name="fastest-configuration"></a>最も速い構成 
実行速度を最速にするには、前の最小の構成で使用したのと同じ構成オプションを使用します。ただし、このオプションも考慮します。

- TX_REACTIVATE_INLINE

***readme_threadx.txt*** ファイルで、特定バージョンの ThreadX SMP の追加オプションについて確認してください。 詳細な構成オプションについて、28 ページから説明しています。

### <a name="global-time-source"></a>グローバル タイム ソース  
他の Azure RTOS 製品 (FileX、NetX、GUIX、USBX など) では、ThreadX SMP で 1 秒を表す ThreadX SMP タイマー刻みの数が定義されています。 この定数に基づいて各種の時間要件が導き出されます。 既定では、この値は 100 で、10 ミリ秒の定期的割り込みを想定しています。 この値をユーザーがオーバーライドするには、***tx_port.h*** または IDE あるいはコマンド ライン内で TX_TIMER_TICKS_PER_SECOND を必要な値で定義します。

### <a name="detailed-configuration-options"></a>詳細な構成オプション

- **TX_BLOCK_POOL_ENABLE_PERFORMANCE_INFO**: 定義されている場合、ブロック プールに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_BYTE_POOL_ENABLE_PERFORMANCE_INFO**: 定義されている場合、バイト プールに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_DISABLE_ERROR_CHECKING**: 基本サービス呼び出しのエラー チェックをバイパスします。 アプリケーション ソースで定義されていると、すべての基本パラメーター エラー チェックが無効になります。 これによりパフォーマンスが最大 30% 向上する場合があり、イメージ サイズも小さくなる可能性があります。

> [!NOTE]
> *エラー チェックを無効にしても安全なのは、アプリケーションですべての入力パラメーターが、外部入力から派生した入力パラメーターも含めて、すべての状況で常に有効であると保証される場合だけです。無効な入力が API に提供され、エラー チェックが無効になっている場合、その結果の動作は定義されていないため、メモリが破損したりシステムがクラッシュする可能性があります。*

> [!NOTE]
> *エラー チェックを無効にすることの影響がない ThreadX SMP API の戻り値を、第 4 章の各 API の説明の「戻り値」セクションに太字で示しています。太字でない戻り値は、TX_DISABLE_ERROR_CHECKING オプションを使用してエラー チェックを無効にしている場合は void になります。*
- **TX_DISABLE_NOTIFY_CALLBACKS**: 定義されている場合、さまざまな ThreadX SMP オブジェクトの通知コールバックが無効になります。 このオプションを使用するとコード サイズが若干減り、パフォーマンスが向上します。 既定では、このオプションは定義されていません。
- **TX_DISABLE_PREEMPTION_THRESHOLD**: 定義されている場合、プリエンプションしきい値機能が無効になり、コード サイズが若干減って、パフォーマンスが向上します。 プリエンプションしきい値機能は使用できなくなります。 既定では、このオプションは定義されていません。
- **TX_DISABLE_REDUNDANT_CLEARING**: 定義されている場合、ThreadX SMP のグローバル C データ構造をゼロに初期化するためのロジックが削除されます。 これを使用するのは、コンパイラの初期化コードが初期化されていないすべての C グローバル データをゼロに設定する場合だけにしてください。 このオプションを使用するとコード サイズが若干減り、初期化中のパフォーマンスが向上します。 既定では、このオプションは定義されていません。
- **TX_DISABLE_STACK_FILLING**: 定義されている場合、各スレッドのスタックの作成時に各バイトへの 0xEF 値の配置が無効になります。 既定では、このオプションは定義されていません。
- **TX_ENABLE_EVENT_TRACE**: 定義されている場合、ThreadX SMP で TraceX トレース バッファーを作成するためのイベント収集コードが有効になります。 詳細については、TraceX ユーザー ガイドをご覧ください。
- **TX_ENABLE_STACK_CHECKING**: 定義されている場合、ThreadX SMP ランタイム スタック チェックが有効になります。これには、スタック使用量の解析と、スタック領域の前後のデータ パターン「フェンス」の検査が含まれます。 スタック エラーが検出されると、登録されているアプリケーション スタック エラー ハンドラーが呼び出されます。 このオプションにより、オーバーヘッドとコード サイズが若干増加します。 **_tx_- thread_stack_error_notify_** API で詳細を確認してください。 既定では、このオプションは定義されていません。
- **TX_EVENT_FLAGS_ENABLE_PERFORMANCE_INFO**: 定義されている場合、イベント フラグ グループに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_INLINE_THREAD_RESUME_SUSPEND**: 定義されている場合、ThreadX SMP でインライン コードを介した **_tx_thread_resume_ *_ および _* _tx_thread_suspend_** の API 呼び出しが向上します。 これによりコード サイズが増えますが、この 2 つの API 呼び出しのパフォーマンスが向上します。
- **TX_MAX_PRIORITIES**: ThreadX SMP の優先度レベルを定義します。 有効な値は 32 から 1024 の範囲 (両端を含む) で、32 で均等に割り切れなければなりません。 サポートされている優先度レベルの数を増やすと、優先度 を 32 上げるごとに RAM 使用量が 128 バイト増えます。 ただし、パフォーマンスへの影響はごくわずかです。 既定では、この値は 32 の優先度レベルに設定されます。
- **TX_MINIMUM_STACK**: 最小スタック サイズ (バイト単位) を定義します。 これは、スレッドが作成されるときのエラー チェックに使用されます。 既定値はポート固有であり、**_tx_port.h_** にあります。
- **TX_MISRA_ENABLE**: 定義されている場合、ThreadX SMP が MISRA C 準拠規則を使用します。 詳細については、**_ThreadX_MISRA_Compliance.pdf_** をご覧ください。
- **TX_MUTEX_ENABLE_PERFORMANCE_INFO**: 定義されている場合、ミューテックスに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_NO_TIMER**: 定義されている場合、ThreadX SMP タイマー ロジックが完全に無効になります。 これは、ThreadX SMP のタイマー機能 (スレッド スリープ、API タイムアウト、タイムスライス、アプリケーション タイマー) を使用しない場合に便利です。
- **TX_NOT_INTERRUPTABLE**: 定義されている場合、ThreadX SMP は割り込みロックアウト時間を最小に抑えようとしません。 この結果、実行速度が速くなりますが、割り込みロックアウト時間が若干長くなります。
- **TX_QUEUE_ENABLE_PERFORMANCE_INFO**: 定義されている場合、キューに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_REACTIVATE_INLINE**: 定義されている場合、関数呼び出しを使用する代わりに、ThreadX SMP タイマーの再アクティブ化がインラインで行なわれます。 これによりパフォーマンスが向上しますが、コード サイズが若干増えます。 既定では、このオプションは定義されていません。
- **TX_SEMAPHORE_ENABLE_PERFORMANCE_INFO**: 定義されている場合、セマフォに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_THREAD_ENABLE_PERFORMANCE_INFO**: 定義されている場合、スレッドに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_THREAD_SMP_CORE_MASK**: CORE 除外のビット マップ マスクを定義します。 たとえば 4 コア環境では、この定義の値が 0xF です。
- **TX_THREAD_SMP_DEBUG_ENABLE**: 定義されている場合、ThreadX SMP のデバッグ情報が循環バッファーに保存されます。
- **TX_THREAD_SMP_DYNAMIC_CORE_MAX**: 定義されている場合、実行時に調整できる動的最大コア数が有効になります。
- **TX_THREAD_SMP_EQUAL_PRIORITY**: 定義されている場合、ThreadX SMP は同じ優先度のスレッドのみを並列でスケジュールします。 これは、ThreadX SMP ライブラリをビルドする前に定義する必要があります。
- **TX_THREAD_SMP_INTER_CORE_INTERRUPT**: 定義されている場合、ThreadX SMP がコア間割り込みを生成します。
- **TX_THREAD_SMP_MAX_CORES**: 最大コア数を定義します。
- **TX_THREAD_SMP_ONLY_CORE_0_DEFAULT**: 定義されている場合、ThreadX SMP ですべてのスレッドとタイマーがコア 0 だけで実行されるよう既定で設定されます。 アプリケーションは、コア除外 API を呼び出すことによってこれをオーバーライドする場合があります。 これは、ThreadX SMP ライブラリをビルドする前に定義する必要があります。
- **TX_THREAD_SMP_WAKEUP_LOGIC**: 定義されている場合、コア "i" をウェイクアップするアプリケーション マクロが呼び出されます。 これは、**_tx_port.h_** をインクルードする前に定義する必要があります。
- **TX_THREAD_SMP_WAKEUP(i)** : コア "i" をウェイクアップするアプリケーション マクロを定義します。 これは、**_tx_port.h_** をインクルードする前に定義する必要があります。
- **TX_TIMER_ENABLE_PERFORMANCE_INFO**: 定義されている場合、タイマーに関するパフォーマンス情報の収集が有効になります。 既定では、このオプションは定義されていません。
- **TX_TIMER_PROCESS_IN_ISR**: 定義されている場合、ThreadX SMP の内部システム タイマー スレッドは除去されます。 この結果、タイマー スタックと制御ブロックが不要になるため、タイマー イベントのパフォーマンスが向上し、RAM の要件が小さくなります。 ただし、このオプションを使用すると、タイマー期限切れ処理がすべてタイマー ISR レベルに移動します。 既定では、このオプションは定義されていません。

    > [!NOTE]
    > タイマーから許可されているサービスが ISR から許可されなくなる可能性があり、その結果としてこのオプションを使用しているとき許可されなくなる場合があります。

- **TX_TIMER_THREAD_PRIORITY**: ThreadX SMP の内部システム タイマー スレッドの優先度を定義します。 既定値は優先度 0 で、ThreadX SMP で最も高い優先度です。 既定値は **_tx_port.h_** で定義されています。
- **TX_TIMER_THREAD_STACK_SIZE**: ThreadX SMP 内部システム タイマー スレッドのスタック サイズ (バイト単位) を定義します。 このスレッドは、すべてのスレッド スリープ要求だけでなく、すべてのサービス呼び出しタイムアウトも処理します。 さらに、すべてのアプリケーション タイマー コールバック ルーチンがこのコンテキストから呼び出されます。 既定値はポート固有であり、**_tx_port.h_** にあります。

## <a name="threadx-smp-version-id"></a>ThreadX SMP バージョン ID

ThreadX SMP バージョン ID は、***readme_threadx.txt** _ ファイルにあります。 このファイルには、対応するポートのバージョン履歴も含まれています。 アプリケーション ソフトウェアは、グローバル文字列 _ *_ _tx_version_id_** を調べることで ThreadX SMP バージョンを取得できます。