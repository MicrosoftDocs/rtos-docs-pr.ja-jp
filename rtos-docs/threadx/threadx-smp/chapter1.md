---
title: 第 1 章 - Azure RTOS ThreadX SMP の概要
description: この章では、製品の概要と、そのアプリケーションおよび利点について説明します。
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 67bdb9076272fa3671ec9321baec609b291c04b8
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2021
ms.locfileid: "104811387"
---
# <a name="chapter-1-introduction-to-azure-rtos-threadx-smp"></a>第 1 章: Azure RTOS ThreadX SMP の概要

Azure RTOS ThreadX SMP は、埋め込みアプリケーション専用に設計された高パフォーマンス リアルタイム SMP カーネルです。 この章では、製品の概要と、そのアプリケーションおよび利点について説明します。

## <a name="threadx-smp-unique-features"></a>ThreadX SMP の固有の機能

ThreadX SMP によって、埋め込みアプリケーションに対称型マルチプロセッシング （SMP） テクノロジがもたらされます。 実行 「準備完了」 状態の ThreadX SMP のアプリケーション スレッド （優先度はさまざま） が、スケジューリング時に、使用可能なプロセッサ コアに動的に割り当てられます。 これにより、使用可能なすべてのプロセッサ コアに対してアプリケーション スレッドの実行を自動的に負荷分散するなど、真の SMP 処理が実現します。

他のリアルタイム カーネルとは異なり、ThreadX SMP は汎用性を重視して設計され、小型マイクロコントローラーをベースとしたアプリケーションから、強力な CISC、RISC、および DSP プロセッサ搭載のアプリケーションに容易にスケーリングできます。

ThreadX SMP は、基になるアーキテクチャに従ってスケーラブルに動作します。 ThreadX SMP サービスは C ライブラリとして実装されているため、アプリケーションによって実際に使用されるサービスのみが実行時イメージに取り込まれます。 したがって、ThreadX SMP の実際のサイズは、アプリケーションによって完全に決まります。 ThreadX SMP の命令イメージのサイズは、ほとんどのアプリケーションで 5 KB から 20 KB の範囲になります。

### <a name="picokernel-architecture"></a>picokernel™ アーキテクチャ 
ThreadX SMP サービスは、従来の 「*マイクロカーネル*」 アーキテクチャのようにカーネル関数を重ねていくのではなく、そのコアに直接接続します。 これにより、コンテキスト切り替えとサービス呼び出しのパフォーマンスが最速になります。 このような非レイヤー設計を *picokernel* アーキテクチャと呼んでいます。

### <a name="ansi-c-source-code"></a>ANSI C ソース コード 
ThreadX SMP は主に ANSI C で記述されており、カーネルを基になるターゲット プロセッサに合わせて調整するために、少量のアセンブリ言語が必要です。 この設計により、ThreadX SMP を、非常に短い時間で新しいプロセッサ ファミリーに移植できます （通常は数週間かかります）。

### <a name="advanced-technology"></a>高度なテクノロジ 
次に ThreadX SMP の高度なテクノロジの概要を示します。

  - シンプルな *picokernel* アーキテクチャ
  - 自動負荷分散
  - スレッドごとのプロセッサの除外
  - 自動スケーリング （占有領域が小さい）
  - 決定論的処理
  - 高速なリアルタイムのパフォーマンス
  - プリエンプティブ スケジューリングと協調スケジューリング
  - 柔軟なスレッド優先順位のサポート （32-1024）
  - システム オブジェクトの動的生成
  - システム オブジェクト数に制限がない
  - 最適化された割り込み処理
  - 優先しきい値™
  - 優先度の継承
  - イベントチェーン™
  - 高速ソフトウェアタイマー
  - 実行時メモリの管理
  - 実行時パフォーマンスの監視
  - 実行時スタック分析
  - 組み込みシステム トレース
  - 大規模なプロセッサのサポート
  - 大規模な開発ツールのサポート
  - 完全なエンディアン ニュートラル

### <a name="not-a-black-box"></a>ブラック ボックスではない
ThreadX SMP のほとんどのディストリビューションに、完全な C ソースコードと、プロセッサ固有のアセンブリ言語が含まれています。 これにより、多くの商用カーネルで発生する 「ブラックボックス」 の問題が解消されます。 ThreadX SMP を使用すると、アプリケーション開発者が、カーネルの動作を正確に知ることができます。謎はありません。

また、ソース コードによって、アプリケーション固有の変更を行うこともできます。 推奨はされていませんが、どうしても必要な場合にカーネルを変更できれば、確かに便利です。

これらの機能は、独自の 「*社内カーネル*」 の操作に慣れている開発者にとっては、特に心強いものです。 期待されているのは、ソースコードと、カーネルを変更する機能です。 ThreadX SMP は、このような開発者のための究極のカーネルです。

### <a name="the-rtos-standard"></a>RTOS 標準
ThreadX SMP は、その汎用性、高パフォーマンス *picokernel* アーキテクチャ、高度なテクノロジ、および実証済みの移植性により、現在 20 億台以上のデバイスに導入されています。 これにより、事実上、ThreadX SMP は、深く埋め込まれているアプリケーションの RTOS 標準となっています。

## <a name="safety-certifications"></a>安全性の認定

### <a name="tv-certification"></a>TÜV 認定  
ThreadX SMP は、IEC61508 および IEC-62304 に従って、安全性が重要なシステムで使用できるように SGS-TÜV Saar によって認定されています。 この認定により、「電気・電子・プログラマブル電子安全関連系の機能安全」 に関する国際電気標準会議 （IEC） 61508 および IEC 62304 の、安全性の最高レベルに対応する安全関連ソフトウェアの開発に、ThreadX SMP を使用できることが承認されます。ThreadX SMP can be used in the development of safety-related software for the highest safety integrity levels of the International Electrotechnical Commission ドイツの SGS-Group と TÜV Saarland の共同事業を通じて結成された SGS-TÜV Saar は、世界中の安全関連システムのための埋め込みソフトウェアのテスト、監査、検証、認定を行う、世界有数の公認された独立系企業になりました。 工業安全規格の IEC 61508 と、そこから派生したすべての標準 （IEC 62304 を含む） は、電気・電子・プログラマブル電子安全関連の医療デバイス、プロセス制御システム、工業機械、鉄道制御システムの機能安全を保証するために使用されます。

SGS-TÜV Saar は、ISO 26262 標準に従って、ThreadX SMP を、安全性が重要な自動車システムで使用できるものとして認定しました。 さらに、ThreadX SMP は、ISO 26262 認定の最高レベルを表す Automotive Safety Integrity Level （ASIL） D に認定されています。

また、SGS-TÜV Saar は、ThreadX SMP を、安全性が重要な鉄道アプリケーションで使用できるものとして認定しており、EN 50128 標準の最高レベル SW-SIL 4 を満たしています。

![TÜV 認定](media/image2.png)

IEC 61508 （最高レベル: SIL 4）  
IEC 62304 （最高レベル: SW 安全性クラス C）  
ISO 26262 ASIL D  
EN 50128 SW-SIL 4  

> [!IMPORTANT]
> TÜV によって認定されている ThreadX SMP のバージョンに関する詳細情報や、テスト レポート、証明書、および関連ドキュメントの入手については、[azure-rtos-support@microsoft.com](https://azure-rtos-support@microsoft.com) までお問い合わせください。

### <a name="misra-c-compliant"></a>MISRA C 準拠 
MISRA C は、C プログラミング言語を使用した重要なシステム用の一連のプログラミング ガイドラインです。 元の MISRA C ガイドラインは主に自動車アプリケーションを対象としていましたが、現在 MISRA C は安全性が重要なすべてのアプリケーションに適用できるものとして広く認識されています。 ThreadX SMP は、MISRA-C:2004 と MISRA C:2012 のすべての 「必要」 ルールと 「必須」 ルールに準拠しています。 また、ThreadX SMP は、3 つを除くすべての 「推奨」 ルールにも準拠しています。 詳細については、***ThreadX_MISRA_Compliance.pdf*** ドキュメントをご覧ください。

### <a name="ul-certification"></a>UL 認定 
ThreadX SMP は、プログラマブル コンポーネント内のソフトウェアに対する UL 60730-1 付属文書 H、CSA E60730-1 付属文書 H、IEC 60730-1 付属文書 H、UL 60335-1 付属文書 R、IEC 60335-1 付属文書 R、UL 1998 の各安全標準に準拠しているものとして UL によって認定されています。 IEC/UL 60730-1 の付属文書 H には 「ソフトウェアを使用した制御」 に対する要件が、そして IEC 60335-1 標準の付属文書 R には 「プログラマブル電子回路」 に対する要件が記載されています。また、IEC 60730 の 付属文書 R と IEC 60335-1 の 付属文書 R は、洗濯機、乾燥機、冷蔵庫、冷凍庫、オーブンなどのアプライアンスで使用される MCU のハードウェアとソフトウェアの安全性を取り上げています。

![UL 認定](media/image3.png) 

*UL/IEC 60730、UL/IEC 60335、UL 1998*

> [!IMPORTANT]
> TÜV によって認定されている ThreadX SMP のバージョンに関する詳細情報や、テスト レポート、証明書、および関連ドキュメントの入手については、[azure-rtos-support@microsoft.com](https://azure-rtos-support@microsoft.com) までお問い合わせください。

### <a name="certification-pack"></a>認定パック

ThreadX SMP Certification Pack™ は、すぐに使える 100% 完全な業界固有のスタンドアロン パッケージで、安全性が重要な航空、医療、産業システムに求められる最高レベルの信頼性と重要度レベルに対して、ThreadX SMP ベースの製品を認定または適切に提出するのに必要なすべての ThreadX SMP の証拠を提供します。 サポートされている認定には、DO-178B、ED-12B、DO-278、FDA510（k）、IEC-62304、IEC-60601、ISO-14971、UL-1998、IEC-61508、CENELEC EN50128、BS50128、および 49CFR236 があります。 認定パックの詳細については、sales@expresslogic.com にお問い合わせください。

## <a name="embedded-applications"></a>埋め込みアプリケーション

埋め込みアプリケーションは、ワイヤレス通信デバイス、自動車エンジン、レーザー プリンター、医療機器などの製品内に埋め込まれたマイクロプロセッサ上で実行されます。また、そのソフトウェアとハードウェアに専用の用途があることも、埋め込みアプリケーションの特徴の 1 つです。

### <a name="real-time-software"></a>リアルタイム ソフトウェア 
アプリケーション ソフトウェアに時間的な制約が課される場合、そのソフトウェアは 「*リアルタイム*」 ソフトウェアと呼ばれます。 基本的には、正確な時間内に処理を実行する必要があるソフトウェアが、「*リアルタイム*」 ソフトウェアと呼ばれます。 埋め込みアプリケーションは外部イベントとのやり取りが不可欠であるため、ほとんどの場合、リアルタイムです。

### <a name="multitasking"></a>マルチタスキング  
前述のように、埋め込みアプリケーションには専用の用途があります。 この目的を達成するために、ソフトウェアはさまざまな 「*タスク*」 を実行する必要があります。 タスクは、特定の職務を実行する、アプリケーションの半独立した部分です。 また、タスクによっては、他のタスクよりも重要になってくる場合もあります。 埋め込みアプリケーションの大きな課題の 1 つが、さまざまなアプリケーション タスク間でプロセッサを割り振ることです。 競合するタスク間で処理を割り振ることが、ThreadX SMP の主な目的です。

### <a name="tasks-vs-threads"></a>タスクとスレッド 
タスクについては、もう 1 つ特徴があります。 タスクという用語は、さまざまな意味で使われます。 あるときは、個別に読み込み可能なプログラムを意味します。 内部プログラム セグメントを指すこともあります。

最近のオペレーティング システムの話をする場合、このタスクという用語は、ほぼ 「*プロセス*」 と 「*スレッド* 」 という 2 つの用語に置き換えられます。 「*プロセス*」 は、独自のアドレス空間を持つ完全に独立したプログラムで、「*スレッド*」 は、プロセス内で実行される半独立したプログラム セグメントです。 スレッドは同じプロセス アドレス空間を共有します。 スレッド管理に伴うオーバーヘッドは最小限に抑えられます。

埋め込みアプリケーションのほとんどが、本格的なプロセス指向のオペレーティング システムに伴うオーバーヘッド （メモリとパフォーマンスの両方） を許容できません。 また、小規模なマイクロプロセッサには、真のプロセス指向オペレーティング システムをサポートするハードウェア アーキテクチャがありません。 このような理由から、ThreadX SMP が実装しているスレッド モデルは、リアルタイムの埋め込みアプリケーションのほとんどに対して非常に効率的かつ実用的です。

混乱を避けるため、「*タスク*」 という用語は、ThreadX SMP では使用されていません。 代わりに、よりわかりやすい最近の名称である 「*スレッド*」 が使用されます。

## <a name="threadx-smp-benefits"></a>ThreadX SMP の利点

ThreadX SMP を使用すると、埋め込みアプリケーションに多くのメリットがもたらされます。 もちろん、一番のメリットは、埋め込みアプリケーションのスレッドへの処理時間の割り当て方法です。

### <a name="automatic-load-balancing"></a>自動負荷分散  
ThreadX SMP は、自動負荷分散 （使用可能なコア間でのスレッドの実行） が可能であるため、マルチコア プロセッサの利用が可能な限り容易になります。 

### <a name="improved-responsiveness"></a>応答性の向上  
ThreadX SMP のようなリアルタイム カーネルが登場する前は、埋め込みアプリケーションのほとんどが、通常は C の *main* 関数内から、単純な制御ループを使用して処理時間を割り当てていました。 この方法は、非常に小さなアプリケーションや単純なアプリケーション内では今でも使用されています。 しかし、大きなアプリケーションまたは複雑なアプリケーションの場合、これは実用的ではありません。イベントへの応答時間が、制御ループを 1 回通過するのに要する最悪のケースの処理時間の関数になるからです。

さらに悪いことに、制御ループが変わるたびに、アプリケーションのタイミング特性が変化します。 これにより、アプリケーションが本質的に不安定になり、保守と改良が困難になります。

ThreadX SMP が重要な外部イベントに対して提供する応答時間は、高速かつ決定論的です。 ThreadX SMP は、優先順位に基づくプリエンプティブなスケジューリング アルゴリズムによってこれを実現するため、優先度の高いスレッドが、実行中の優先度の低いスレッドを横取りできます。 結果として、最悪のケースでの応答時間は、コンテキスト切り替えの実行に必要な時間に近くなります。 これは決定論的であるだけでなく、非常に高速でもあります。

### <a name="software-maintenance"></a>ソフトウェアのメンテナンス  
ThreadX SMP カーネルを使用すると、アプリケーション開発者がアプリケーション スレッドの特定の要件に専念できます。その際、そのアプリケーションの他の領域のタイミングが変わることを心配する必要はありません。 また、この機能によって、ThreadX SMP を利用するアプリケーションの修復や拡張がはるかに簡単になります。

### <a name="increased-throughput"></a>スループットの向上  
制御ループの応答時間に関する問題は、ポーリングを追加することで回避できることがあります。 これにより応答性は向上しますが、やはり最悪のケースの応答時間が一定になることは保証されず、アプリケーションの将来の変更を強化するものではありません。 また、追加のポーリングが原因で、プロセッサが実行する不要な処理が増えてしまいます。 このような不要な処理すべてが、システムの全体的なスループット低下につながります。

オーバーヘッドに関する興味深い点は、開発者の多くが、ThreadX SMP のようなマルチスレッド環境はオーバーヘッドを増やし、システム全体のスループットに悪影響を及ぼすと考えていることです。 しかし、マルチスレッド化により、制御ループ環境で発生する冗長なポーリングすべてが排除され、実際にオーバーヘッドを軽減できる場合もあります。 マルチスレッド カーネルに関連するオーバーヘッドは、通常、コンテキスト切り替えに必要な時間の関数です。 コンテキスト切り替え時間がポーリング プロセスよりも短い場合、ThreadX SMP は、オーバーヘッドを減らし、スループットを向上させる可能性のあるソリューションを提供します。 このため、ある程度の複雑さやサイズを持つアプリケーションに ThreadX SMP を選択するのは自然なことです。

### <a name="processor-isolation"></a>プロセッサの分離  
ThreadX SMP は、アプリケーションと基になるプロセッサの間に、プロセッサに依存しない堅牢なインターフェイスを提供します。 これにより、開発者はアプリケーションに専念することができます。ハードウェアの詳細の学習に膨大な時間をかける必要はありません。 

### <a name="dividing-the-application"></a>アプリケーションの分割  
制御ループをベースにしたアプリケーションの場合は、各開発者が、アプリケーション全体の実行時の動作と要件を熟知している必要があります。 これは、プロセッサの割り当てロジックがアプリケーション全体に分散しているためです。 アプリケーションのサイズや複雑さが増すほど、すべての開発者が、アプリケーション全体の正確な処理要件を覚えることは不可能になります。

ThreadX SMP は、プロセッサの割り当てに関する悩みから各開発者を解放し、埋め込みアプリケーションの特定の部分に集中できるようにします。 さらに、ThreadX SMP により、アプリケーションは、明確に定義されたスレッドに強制的に分割されます。 このようにアプリケーションをスレッドに分割すれば、開発が格段に簡単になります。

### <a name="ease-of-use"></a>使いやすさ  
ThreadX SMP は、アプリケーション開発者を念頭に置いて設計されています。 ThreadX SMP のアーキテクチャとサービス呼び出しインターフェイスは、簡単に理解できるように設計されています。 その結果、ThreadX SMP の開発者は、その高度な機能をすぐに使うことができます。  

### <a name="improve-time-to-market"></a>市場投入までの時間の短縮  
ThreadX SMP のすべての利点が、ソフトウェア開発プロセスを加速します。 ThreadX SMP は、プロセッサに関するほとんどの問題と、最も一般的な安全性の認定に対応しているため、これに伴う作業を開発スケジュールから取り除くことができます。 その結果、市場投入までの時間が短縮されます。  

### <a name="protecting-the-software-investment"></a>ソフトウェア投資の保護  
ThreadX SMP は、そのアーキテクチャのおかげで、新しいプロセッサや開発ツール環境に簡単に移植できます。 このことと、ThreadX SMP が、基になるプロセッサの詳細からアプリケーションを切り離しているという事実が相まって、ThreadX SMP アプリケーションも移植性が高くなっています。 その結果、アプリケーションの移行パスが保証され、当初の開発投資が保護されます。