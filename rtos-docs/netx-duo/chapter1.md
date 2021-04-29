---
title: チャプター 1 - Azure RTOS NetX Duo の概要
description: このチャプターでは、Azure RTOS NetX Duo の概要を述べ、NetX Duo のアプリケーションと利点について説明します。
author: philmea
ms.author: philmea
ms.date: 05/19/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 91dfd0e62cb565f677faa7d52fe22abc1f0e19a1
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2021
ms.locfileid: "104811039"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-duo"></a>チャプター 1 - Azure RTOS NetX Duo の概要

Azure RTOS NetX Duo は TCP/IP 規格のハイ パフォーマンスなリアルタイム実装であり、Azure RTOS ThreadX を使用する組み込みアプリケーション専用に設計されています。 このチャプターでは、NetX Duo の概要を述べ、NetX Duo のアプリケーションと利点について説明します。 

## <a name="netx-duo-unique-features"></a>NetX Duo 独自の機能

他の TCP/IP 実装と異なり、NetX Duo は汎用性の高い実装として設計されています。つまり、マイクロコントローラーを使用する小型のアプリケーションから、高性能な RISC や DSP のプロセッサを使用するアプリケーションまで、用途に合わせて簡単にスケールできます。 これは、本来ワークステーション環境用でありながら無理に組み込み用に設計し直したパブリック ドメインの実装や他の商用実装と大きく異なります。

### <a name="piconettrade-architecture"></a>Piconet&trade; アーキテクチャ

NetX Duo の優れたスケーラビリティとパフォーマンスを生み出しているのは、組み込みシステム向けに設計されたソフトウェア アーキテクチャの <em>Piconet</em> です。 Piconet アーキテクチャでは、NetX Duo のサービスを C ライブラリとして実装することにより、スケーラビリティを最大まで高めています。 この方法により、アプリケーションで使用するサービスだけを最終的なランタイム イメージに取り込みます。 そのため、NetX Duo の実際のサイズはアプリケーションによって決まります。 ほとんどのアプリケーションに対して推奨される NetX Duo のイメージの要件は 5 - 30 KB です。 IPv6 と ICMPv6 を有効にして IPv6 アドレス構成と近隣探索プロトコルを使用できるようにする場合、NetX Duo のサイズは 30 - 45 KB になります。

NetX Duo では、必要な場合にだけ内部コンポーネント関数の呼び出しを階層化することで、優れたネットワーク パフォーマンスを実現します。 さらに、NetX では処理の多くを直接インラインで行うため、過去に組み込みで使用されていたワークステーションのネットワーク ソフトウェアよりも、パフォーマンスが大幅に優れています。

### <a name="zero-copy-implementation"></a>ゼロコピーの実装

NetX Duo では、TCP/IP でのゼロコピーをパケット ベースで実装しています。 ゼロコピーは、アプリケーションのパケット バッファーのデータを NetX Duo 内にコピーしないことを意味します。 この仕組みによりパフォーマンスを大幅に高め、貴重なプロセッサのサイクルをアプリケーションでの処理のために節約できます。これは、組み込みアプリケーションにとって重要なことです。

### <a name="udp-fast-pathtrade-technology"></a>UDP Fast Path&trade; テクノロジー

NetX Duo では、<em>UDP Fast Path</em> により最速の UDP 処理を実現しています。 送信側では、UDP 処理 (オプションの UDP チェックサムを含む) は <em>**nx_udp_socket_send**</em> サービスで行います。 パケットが NetX Duo の IP 送信ルーチンによって送信できる状態になるまで、他の関数は呼び出しません。 また、このルーチンはフラットなので (つまり、関数呼び出しのネストが最小限なので)、パケットをアプリケーションのネットワーク ドライバーに渡すのも速いです。 NetX Duo では、受信した UDP パケットは、適切な UDP ソケットの受信キューに直接入れるか、UDP ソケットの受信キューからパケットを受け取るのを待って一時停止している最初のスレッドに渡します。 ThreadX ではそれ以外にコンテキスト スイッチは必要ありません。

### <a name="ansi-c-source-cod"></a>ANSI C ソース コード

NetX Duo は ANSI C だけで書かれており、ANSI C コンパイラと ThreadX サポートが存在するほとんどすべてのプロセッサ アーキテクチャにそのまま移植できます。 

### <a name="not-a-black-box"></a>ブラック ボックス化していない

NetX Duo のほとんどの配布パッケージには、C 言語の完全なソース コードが含まれています。 これにより、多くの商用ネットワーク スタックで発生する “ブラック ボックス化” の問題を解消しています。 NetX Duo を使用すれば、アプリケーション開発者は、ネットワーク スタックで何が起こっているかを正確に把握できます。すべては明々白々です。

ソース コードがあることで、アプリケーション独自の変更を施すこともできます。 推奨はしませんが、ネットワーク スタックを改変できる仕組みが必要な場合もあります。

自社用またはパブリック ドメインのネットワーク スタックを使用することに慣れている開発者には特に、これらの機能が歓迎されるでしょう。 そうした開発者にとっては、ソースコードがあってそれを改変できることが重要です。 NetX Duo はそのような開発者のための究極のネットワークソフトウェアです。

### <a name="bsd-compatible-socket-api"></a>BSD 互換ソケット API

古いアプリケーションに対しては、NetX Duo は BSD 互換ソケットのインターフェイスを提供しています。このインターフェースで実際に呼び出すのは、ハイ パフォーマンスな NetX Duo API です。 このインターフェイスにより、既存のネットワーク アプリケーションのコードを NetX Duo に移植しやくすなっています。

## <a name="rfcs-supported-by-netx-duo"></a>NetX Duo でサポートしている RFC

基本的なネットワーク プロトロルを定めた RFC のうち NetX Duo でサポートしているものの一部を次に挙げます。 NetX Duo では、小さなメモリ フットプリントと効率的な処理により、リアルタイム オペレーティング システムに使用するという制約の中で、一般的な推奨事項と基本的な要件をすべて満たしています。

| **RFC**  | **説明**                                        |
| -------- | ------------------------------------------------------ |
|RFC 1112 | IP マルチキャストのホスト拡張 (IGMPv1)           |
|RFC 1122 | インターネット ホストの要件 - 通信レイヤー |
|RFC 2236 | インターネット グループ管理プロトコル、バージョン 2          |
|RFC 768  | ユーザー データグラム プロトコル (UDP)                           |
|RFC 791  | インターネット プロトコル (IP)●いんたーねっとぷろとこる(ip)○                                 |
|RFC 792  | インターネット制御メッセージ プロトコル (ICMP)               |
|RFC 793  | 伝送制御プロトコル (TCP)                    |
|RFC 826  | イーサネット アドレス解決プロトコル (ARP) |
|RFC 903  | 逆アドレス解決プロトコル (RARP) |
|RFC 5681 | TCP 輻輳制御                     |

NetX Duo でサポートしている IPv6 関連 RFC を以下に挙げます。

|**RFC** |**説明**|
-------- | ------------------------------------------ |
|RFC 1981 |インターネット プロトコル バージョン 6 (IPv6) のパス MTU 探索|
|RFC 2460 | インターネットプロトコル バージョン 6 (IPv6) 仕様|
|RFC 2464 |イーサネット ネットワークでの IPv6 パケットの送信|
|RFC 4291 |インターネット プロトコル バージョン 6 (IPv6) アドレス アーキテクチャ|
|RFC 4443 |インターネット プロトコル バージョン 6 (IPv6) 仕様のインターネット制御メッセージ プロトコル (ICMPv6)|
|RFC 4861 |IPv6 の近隣探索|
|RFC 4862 |IPv6 ステートレス アドレス自動設定|

## <a name="embedded-network-applications"></a>組み込みネットワーク アプリケーション

組み込みネットワーク アプリケーションは、携帯電話、通信機器、自動車エンジン、レーザー プリンター、医療機器などの製品内部のマイクロプロセッサで実行する、ネットワークへのアクセスが必要なアプリケーションです。 これらのアプリケーションはほとんどの場合メモリとパフォーマンスの制約があります。 組み込みネットワーク アプリケーションのもう 1 つの特徴は、ソフトウェアとハードウェアに専用の用途があることです。

厳密な時間の範囲内に処理を実行する必要があるネットワーク ソフトウェアを、*リアルタイム* *ネットワーク* ソフトウェアといいます。また、時間的制約があるネットワーク アプリケーションは、リアルタイム アプリケーションに分類されます。 組み込みネットワーク アプリケーションは必然的に外部とやり取りを行うため、ほとんどの場合リアルタイムです。

## <a name="netx-duo-benefits"></a>NetX Duo の利点

組み込みアプリケーションに NetX Duo を使用する主な利点は、インターネット接続の通信が速いことと、必要なメモリが少ないことです。 NetX Duo は、ハイ パフォーマンスでマルチタスクの ThreadX リアルタイム オペレーティング システムとも統合されています。

### <a name="improved-responsiveness"></a>応答性の向上

ハイ パフォーマンスな NetX Duo のプロトコル スタックを使用すると、組み込みネットワーク アプリケーションの応答性がそれまでよりも向上します。 大量のネットワーク トラフィックを扱う、またはパケット 1 つ 1 つの処理に厳しい要件がある組み込みアプリケーションでは、特にこれが重要です。

### <a name="software-maintenance"></a>ソフトウェアのメンテナンス

NetX Duo を使用すると、開発者は、組み込みアプリケーションのネットワークに関連する各領域を簡単に分けることができます。 そうすることで、開発プロセス全体が簡単になり、ソフトウェアの将来のメンテナンスが大幅にやりやすくなります。

### <a name="increased-throughput"></a>スループットの向上

NetX Duo では、パケット処理の負荷を最小化することにより最高のパフォーマンスのネットワーク機能を提供します。 これによりスループットも向上します。

### <a name="processor-isolation"></a>プロセッサの分離

NetX Duo では、アプリケーションと、処理を行うプロセッサおよびネットワークの間に、プロセッサから独立した堅牢なインターフェイスを提供します。 これにより開発者は、ネットワークに直接影響を及ぼすハードウェアの問題への対処に余計な時間をかけることなく、アプリケーションのネットワークに集中できます。

### <a name="ease-of-use"></a>使いやすさ

NetX Duo はアプリケーション開発者を念頭に置いて設計されています。 NetX Duo のアーキテクチャとサービス呼び出しインターフェイスは分かりやすく設計されています。 そのため、NetX Duo を使用する開発者は高度な機能をすぐに利用できます。

### <a name="improve-time-to-market"></a>市場投入時間の短縮

NetX Duo の優れた機能でソフトウェア開発のプロセスが速くなります。 NetX Duo では、プロセッサとネットワークに関するハードウェアの問題のほとんどを抽象化しているため、アプリケーションのネットワーク関連領域の大部分ではそうした問題が起こりません。 この仕組みと、使いやすさ、高度な機能により市場投入時間を短縮します。

### <a name="protecting-the-software-investment"></a>ソフトウェア投資の保護

NetX Duo は ANSI C だけで書かれており、ThreadX リアルタイム オペレーティング システムと完全に統合されています。 つまり、NetX Duo のアプリケーションは、ThreadX をサポートしているすべてのプロセッサにそのまま移植できます。 その上、新たなプロセッサ アーキテクチャが市場に現れたときも、ThreadX によって数週間でサポートできるようになります。 そのため、NetX Duo を使用するとアプリケーションの移植手段を確保でき、最初の開発投資が無駄になりません。

## <a name="ipv6-ready-logo-certification"></a>IPv6 Reacy Logo 認定

NetX Duo は、IPv6 Ready Organization が提供している IPv6 Core Protocol (Phase 2) Self Test パッケージを使用して、IPv6 Ready 認定を取得しました。 テストのプラットフォームおよび事例の詳しい情報は、次の IPv6-Ready プロジェクトのウェブサイトをご覧ください:[ **https://www.ipv6ready.org/** ](https://www.ipv6ready.org/)

Phase 2 IPv6 Core Protocol Self Test Suite では、広範なテストを実施することで、次の RFC で定める要件を IPv6 スタックが満たしていることを証明します:  
セクション 1: RFC 2460  
セクション 2: RFC 4861  
セクション 3: RFC 4862  
セクション 4: RFC 1981  
セクション 5: RFC 4443  

## <a name="ixanvl-test"></a>IxANVL テスト

NetX Duo は、イクシアの IxANVL でテストを行っています。 IxANVL は、自動ネットワークとプロトコルの検証の業界標準です。 IxANVL の詳しい情報は [ **https://www.ixiacom.com/products/ixanvl** ](https://www.ipv6ready.org/) で確認できます。

具体的には、次の NetX Duo モジュールをIxANVL でテストしています:

|**Module** | **Standard** |
| --------- | ------------ |
|IP | RFC791 </br> RFC1122 </br> RFC894|
|ICMP | RFC792 </br> RFC1122 </br> RFC1812|
|UDP | RFC768 </br> RFC1122|
|TCP-Core| RFC793 </br> RFC1122 </br> RFC2460|
|TCP-Advanced| RFC1981</br>RFC2001</br>RFC2385</br>RFC2463</br>RFC813</br>RFC896|
|TCP-Performance| RFC793</br>RFC1323</br>RFC2018|

## <a name="safety-certifications"></a>安全性の認定

### <a name="tv-certification"></a>TÜV 認定  

NetX Duo は、IEC 61508 と IEC 62304 に従って、セーフティクリティカル システムで使用するのに必要な認定を SGS-TÜV Saar により受けています。 NetX Duo を安全関連ソフトウェアの開発に使用することで、国際電気標準会議 (IEC) 61508 および 62304 で定める最高の安全度水準を満たし、“電気・電子・プログラマブル電子安全関連系の機能安全” を確保できることが、TÜV の証明書により保証されています。 ドイツの SGS グループ と TÜV Saarland のジョイント ベンチャーとして設立された SGS-TÜV Saar は、世界の組み込みソフトウェアの安全関連系のテスト、監査、証明、認定を行う公認独立企業の最大手です。 工業安全規格の IEC 61508 と、そこから派生したすべての規格 (IEC 62304 を含む) は、電気・電子・プログラマブル電子安全関連の医療機器、プロセス制御システム、産業機械、鉄道制御システムの機能安全を保証するための規格です。 

NetX Duo は、SGS-TÜV Saar により、ISO 26262 に従って、セーフティクリティカル自動車システムでの使用に必要な認定を受けています。 さらに、NetX Duo は自動車安全度水準 (ASIL) D の認定を受けています。これは ISO 26262 規格で認定される最高の水準です。 

また NetX Duo は、SGS-TÜV Saar により、セキュリティクリティカルな鉄道用アプリケーションでの使用に必要な認定を受けており、EN 50128 規格の SW-SIL 4 およびそれ以下の水準の満たしています。

![SGS-TÜV Saar 認定ロゴの図。](./media/user-guide/sgs-tuv-saar-logo.png)

IEC 61508 SIL 4 およびそれ以下の水準  
IEC 62304 SW 安全性クラス C およびそれ以下の水準 ISO 26262 ASIL D EN 50128 SW-SIL 4

> [!IMPORTANT]
> *どのバージョンの NetX Duo が TÜV の認定を受けているかという情報、テスト報告書、認定証、その他関連文書の公開に関する情報が必要な方は、Microsoft にお問い合わせください。*

### <a name="ul-certification"></a>UL 認定

NetX Duo は、UL によって、UL 60730-1 附属書 H、CSA E60730-1 附属書 H、IEC 60730-1 附属書 H、UL 60335-1 附属書 R、IEC 60335-1 附属書 R および UL 1998 が定める、プログラマブル部品のソフトウェアの安全規格への準拠を証明する認定を受けています。 IEC/UL 60730-1 附属書 H には “ソフトウェアを使用した制御” の要件があり、IEC 60335-1 附属書 R は “プログラマブル電子回路” の要件を記載しています。IEC 60730 附属書 H と IEC 60335-1 附属書 R には、洗濯機、食器洗い機、乾燥機、冷蔵庫、冷凍庫、オーブンなどの電気器具で使用する MCU ハードウェアおよびソフトウェアの安全に関する規程があります。

![UL 認定ロゴの図。](./media/user-guide/c-ru-us-logo.png)

*UL/IEC 60730、UL/IEC 60335、UL 1998*

> [!IMPORTANT]  
> *どのバージョンの NetX Duo が UL の認定を受けているかという情報、テスト報告書、認定証、その他の関連文書の公開に関する情報が必要な方は、Microsoft にお問い合わせください。*