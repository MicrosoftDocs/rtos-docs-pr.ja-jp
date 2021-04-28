---
title: 第 1 章 - Azure RTOS NetX Duo DNS クライアントの概要
description: DNS とは、ドメイン名と物理 IP アドレスの間のマッピングを含む分散型データベースを提供するものです。
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 4c07d6e3183d421c637874dcdeff3767554fca78
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2021
ms.locfileid: "104810841"
---
# <a name="chapter-1---introduction-to-the-azure-rtos-netx-duo-dns-client"></a><span data-ttu-id="c2db8-103">第 1 章 - Azure RTOS NetX Duo DNS クライアントの概要</span><span class="sxs-lookup"><span data-stu-id="c2db8-103">Chapter 1 - Introduction to the Azure RTOS NetX Duo DNS Client</span></span>

<span data-ttu-id="c2db8-104">DNS とは、ドメイン名と物理 IP アドレスの間のマッピングを含む分散型データベースを提供するものです。</span><span class="sxs-lookup"><span data-stu-id="c2db8-104">The DNS provides a distributed database that contains mapping between domain names and physical IP addresses.</span></span> <span data-ttu-id="c2db8-105">インターネット上には完全なマッピングを含む単一のエンティティは存在しないため、このデータベースは "*分散型*" と呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-105">The database is referred to as *distributed* because there is no single entity on the Internet that contains the complete mapping.</span></span> <span data-ttu-id="c2db8-106">マッピングの一部を保持するエンティティは、DNS サーバーと呼ばれます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-106">An entity that maintains a portion of the mapping is called a DNS Server.</span></span> <span data-ttu-id="c2db8-107">インターネットは多数の DNS サーバーで構成されており、それぞれにデータベースのサブセットが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c2db8-107">The Internet is composed of numerous DNS Servers, each of which contains a subset of the database.</span></span> <span data-ttu-id="c2db8-108">また DNS サーバーは、要求されたマッピングがサーバーに存在する場合にのみ、ドメイン名マッピング情報に関する DNS クライアント要求に応答します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-108">DNS Servers also respond to DNS Client requests for domain name mapping information, only if the server has the requested mapping.</span></span>

<span data-ttu-id="c2db8-109">NetX Duo 用 DNS クライアント プロトコルを使用すると、アプリケーションに対し、1 つまたは複数の DNS サーバーにマッピング情報を要求するサービスが提供されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-109">The DNS Client protocol for NetX Duo provides the application with services to request mapping information from one or more DNS Servers.</span></span>

## <a name="dns-client-setup"></a><span data-ttu-id="c2db8-110">DNS クライアントのセットアップ</span><span class="sxs-lookup"><span data-stu-id="c2db8-110">DNS Client Setup</span></span>

<span data-ttu-id="c2db8-111">DNS クライアント パッケージを正常に機能させるには、事前に NetX Duo IP インスタンスが作成されている必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-111">In order to function properly, the DNS Client package requires that a NetX Duo IP instance has already been created.</span></span>

<span data-ttu-id="c2db8-112">DNS クライアントを作成した後は、アプリケーションで DNS クライアントが管理するサーバーの一覧に 1 つ以上の DNS サーバーを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-112">After creating the DNS Client, the application must add one or more DNS servers to the server list maintained by the DNS Client.</span></span> <span data-ttu-id="c2db8-113">DNS サーバーを追加するには、アプリケーションで *nxd_dns_server_add* サービスを使用します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-113">To add DNS servers, the application uses the *nxd_dns_server_add* service.</span></span> <span data-ttu-id="c2db8-114">NetX Duo DNS クライアント サービスである *nx_dns_server_add* を使用してサーバーを追加することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-114">The NetX Duo DNS Client service *nx_dns_server_add* can also be used to add servers.</span></span> <span data-ttu-id="c2db8-115">ただし、これは IPv4 アドレスのみを許容するため、開発者は代わりに *nxd_dns_server_add* サービスを使用することをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="c2db8-115">However it only accepts IPv4 addresses and it is recommended that developers use the *nxd_dns_server_add* service instead.</span></span>

<span data-ttu-id="c2db8-116">NX_DNS_IP_GATEWAY_SERVER オプションが有効になっていて、IP インスタンス ゲートウェイのアドレスがゼロ以外の場合は、IP インスタンス ゲートウェイがプライマリ DNS サーバーとして自動的に追加されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-116">If the NX_DNS_IP_GATEWAY_SERVER option is enabled, and the IP instance gateway address is non zero, the IP instance gateway is automatically added as the primary DNS server.</span></span> <span data-ttu-id="c2db8-117">DNS サーバー情報を静的に認識していない場合は、NetX Duo 用の動的ホスト構成プロトコル (DHCP) を通じて取得することも可能です。</span><span class="sxs-lookup"><span data-stu-id="c2db8-117">If DNS server information is not statically known, it may also be derived through the Dynamic Host Configuration Protocol (DHCP) for NetX Duo.</span></span> <span data-ttu-id="c2db8-118">詳細については、『NetX Duo DHCP ユーザー ガイド』を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c2db8-118">Please refer to the NetX Duo DHCP User Guide for more information.</span></span>

<span data-ttu-id="c2db8-119">DNS クライアントには、DNS メッセージを送信するためのパケット プールが必要です。</span><span class="sxs-lookup"><span data-stu-id="c2db8-119">The DNS Client requires a packet pool for transmitting DNS messages.</span></span> <span data-ttu-id="c2db8-120">既定ではこのパケット プールは、*nx_dns_create* サービスが呼び出されたときに DNS クライアントによって作成されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-120">By default, the DNS Client creates this packet pool when the *nx_dns_create* service is called.</span></span> <span data-ttu-id="c2db8-121">構成オプションの NX_DNS_PACKET_PAYLOAD_UNALIGNED と NX_DNS_PACKET_POOL_SIZE を使用すると、アプリケーションでこのパケット プールのパケット ペイロードとパケット プール サイズ (パケット数など) をそれぞれ指定できます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-121">The configuration options NX_DNS_PACKET_PAYLOAD_UNALIGNED and NX_DNS_PACKET_POOL_SIZE allow the application to determine the packet payload and packet pool size (e.g. number of packets) of this packet pool respectively.</span></span> <span data-ttu-id="c2db8-122">これらのオプションについては、第 2 章の「構成オプション」で説明しています。</span><span class="sxs-lookup"><span data-stu-id="c2db8-122">These options are described in section “Configuration Options” in Chapter Two.</span></span>

<span data-ttu-id="c2db8-123">DNS クライアントで独自のパケット プールを作成するのに代わる別の方法として、アプリケーションでパケット プールを作成し、*nx_dns_packet_pool_set* サービスを使用して DNS クライアントのパケット プールとして設定することもできます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-123">An alternative to the DNS Client creating its own packet pool is for the application to create the packet pool and set it as the DNS Client’s packet pool using the *nx_dns_packet_pool_set* service.</span></span> <span data-ttu-id="c2db8-124">これを行うには、NX_DNS_CLIENT_USER_CREATE_PACKET_POOL オプションを定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-124">To do so, the NX_DNS_CLIENT_USER_CREATE_PACKET_POOL option must be defined.</span></span> <span data-ttu-id="c2db8-125">このオプションでは、*nx_dns_packet_pool_set* へのパケット プール ポインター入力として、事前に *nx_packet_pool_create* を使用して作成したパケット プールも必要となります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-125">This option also requires a previously created packet pool using *nx_packet_pool_create* as the packet pool pointer input to *nx_dns_packet_pool_set* .</span></span> <span data-ttu-id="c2db8-126">NX_DNS_CLIENT_USER_CREATE_PACKET_POOL が有効になっている場合に DNS クライアント インスタンスが削除された場合、DNS クライアント パケット プールが不要であればアプリケーションがそれを削除する責任を持ちます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-126">When the DNS Client instance is deleted, the application is responsible for deleting the DNS Client packet pool if NX_DNS_CLIENT_USER_CREATE_PACKET_POOL is enabled if it is no longer needed.</span></span>

> [!NOTE] 
> <span data-ttu-id="c2db8-127">NX_DNS_CLIENT_USER_CREATE_PACKET_POOL オプションを使用して独自のパケット プールを指定するアプリケーションの場合、パケット サイズを、DNS の最大文字数 (512 bytes) に加えて、UDP ヘッダー、IPv4 または IPv6 ヘッダー、および MAC ヘッダー用のスペースを保持できるようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-127">For applications choosing to provide its own packet pool using the NX_DNS_CLIENT_USER_CREATE_PACKET_POOL option, the packet size needs to be able to hold the DNS maximum massage size (512 bytes) plus rooms for UDP header, IPv4 or IPv6 header, and the MAC header.</span></span>

## <a name="dns-messages"></a><span data-ttu-id="c2db8-128">DNS メッセージ</span><span class="sxs-lookup"><span data-stu-id="c2db8-128">DNS Messages</span></span>

<span data-ttu-id="c2db8-129">DNS では、ホスト名と IP アドレスの間のマッピングを取得するためのメカニズムは非常に簡単です。</span><span class="sxs-lookup"><span data-stu-id="c2db8-129">The DNS has a very simple mechanism for obtaining mapping between host names and IP addresses.</span></span> <span data-ttu-id="c2db8-130">マッピングを取得するために、DNS クライアントによって、解決する必要がある名前または IP アドレスを含む DNS クエリ メッセージが準備されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-130">To obtain a mapping, the DNS Client prepares a DNS query message containing the name or the IP address that needs to be resolved.</span></span> <span data-ttu-id="c2db8-131">次に、サーバーの一覧の最初の DNS サーバーにメッセージが送信されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-131">The message is then sent to the first DNS server in the server list.</span></span> <span data-ttu-id="c2db8-132">サーバーに要求されたマッピングが存在する場合、サーバーは要求されたマッピング情報を含む DNS 応答メッセージを使用して DNS クライアントに応答します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-132">If the server has such a mapping, it replies to the DNS Client using a DNS response message that contains the requested mapping information.</span></span> <span data-ttu-id="c2db8-133">サーバーが応答しない場合、DNS クライアントは一覧にある次のサーバーに対してクエリを行い、すべての DNS サーバーが照会されるまでこれを続けます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-133">If the server does not respond, the DNS Client queries the next server on its list until all its DNS servers have been queried.</span></span> <span data-ttu-id="c2db8-134">すべての DNS サーバーから応答がなかった場合、DNS クライアントには DNS メッセージを再送信する再試行ロジックがあります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-134">If no response from all its DNS servers is received, the DNS Client has retry logic to retransmit the DNS message.</span></span> <span data-ttu-id="c2db8-135">DNS クエリを再送信した場合、再送信タイムアウトが 2 倍になります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-135">On resending a DNS query, the retransmission timeout is doubled.</span></span> <span data-ttu-id="c2db8-136">このプロセスは、最大転送タイムアウト (*nxd_dns.h* で NX_DNS_MAX_RETRANS_TIMEOUT として定義されています) に達するか、サーバーから正常な応答を受信するまで続行されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-136">This process continues until the maximum transmission timeout (defined as NX_DNS_MAX_RETRANS_TIMEOUT in *nxd_dns.h*) is reached or until a successful response is received from that server is obtained.</span></span>

<span data-ttu-id="c2db8-137">NetX Duo DNS クライアントでは、*nxd_dns_host_by_name_get* の呼び出しで IP アドレスのバージョンを指定することによって、IPv6 アドレス検索 (タイプ AAAA) と IPv4 アドレス検索 (タイプ A) の両方を実行できます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-137">NetX Duo DNS Client can perform both IPv6 address lookups (type AAAA) and IPv4 address lookups (type A) by specifying the version of the IP address in the *nxd_dns_host_by_name_get* call.</span></span> <span data-ttu-id="c2db8-138">DNS クライアントは *nxd_dns_host_by_address_get* を使用して、IP アドレスの逆引き参照 (タイプ PTR のクエリ) を実行して Web ホスト名を取得できます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-138">The DNS Client can perform reverse lookups of IP addresses (type PTR queries) to obtain web host names using *nxd_dns_host_by_address_get*.</span></span> <span data-ttu-id="c2db8-139">NetX Duo DNS クライアントでは依然として *nx_dns_host_by_name_get* と *nx_dns_host_by_address_get* がサポートされています。これは同等のサービスですが、IPv4 ネットワーク通信に限定されています。</span><span class="sxs-lookup"><span data-stu-id="c2db8-139">The NetX Duo DNS Client still supports the *nx_dns_host_by_name_get* and *nx_dns_host_by_address_get* which are the equivalent services but which are limited to IPv4 network communication.</span></span> <span data-ttu-id="c2db8-140">ただし開発者は、既存の DNS クライアント アプリケーションを *nxd_dns_host_by_name_get* および *nxd_dns_host_by_address_get* サービスに移植することが推奨されています。</span><span class="sxs-lookup"><span data-stu-id="c2db8-140">However, developers are encouraged to port existing DNS Client applications to the *nxd_dns_host_by_name_get* and *nxd_dns_host_by_address_get* services.</span></span>

<span data-ttu-id="c2db8-141">DNS メッセージでは、UDP プロトコルを利用して要求とフィールドの応答を送信します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-141">DNS messaging utilizes the UDP protocol to send requests and field responses.</span></span> <span data-ttu-id="c2db8-142">DNS サーバーは、クライアントからのクエリをポート番号 53 でリッスンします。</span><span class="sxs-lookup"><span data-stu-id="c2db8-142">A DNS Server listens on port number 53 for queries from clients.</span></span> <span data-ttu-id="c2db8-143">そのため、事前に作成した IP インスタンス (_\*_nx_ip_create_\*\*) で \***nx_udp_enable** _ サービスを使用して、NETX Duo で UDP サービスを有効にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-143">Therefore UDP services must be enabled in NetX Duo using the ***nx_udp_enable** _ service on a previously created IP instance (_*_nx_ip_create_\*\*).</span></span>

<span data-ttu-id="c2db8-144">この時点で、DNS クライアントがアプリケーションからの要求を受け入れて、DNS クエリを送信する準備ができました。</span><span class="sxs-lookup"><span data-stu-id="c2db8-144">At this point, the DNS Client is ready to accept requests from the application and send out DNS queries.</span></span>

## <a name="extended-dns-resource-record-types"></a><span data-ttu-id="c2db8-145">拡張 DNS リソース レコード タイプ</span><span class="sxs-lookup"><span data-stu-id="c2db8-145">Extended DNS Resource Record Types</span></span>

<span data-ttu-id="c2db8-146">NX_DNS_ENABLE_EXTENDED_RR_TYPES が有効になっている場合、NetX Duo DNS クライアントでは次のレコード タイプのクエリもサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-146">If NX_DNS_ENABLE_EXTENDED_RR_TYPES is enabled, NetX Duo DNS Client also supports the following record type queries:</span></span>

- <span data-ttu-id="c2db8-147">CNAME: エイリアスの正規名が含まれています</span><span class="sxs-lookup"><span data-stu-id="c2db8-147">CNAME: contains the canonical name for an alias</span></span>
- <span data-ttu-id="c2db8-148">TXT: テキスト文字列が含まれています</span><span class="sxs-lookup"><span data-stu-id="c2db8-148">TXT: contains a text string</span></span>
- <span data-ttu-id="c2db8-149">NS: 権威ネーム サーバーが含まれています</span><span class="sxs-lookup"><span data-stu-id="c2db8-149">NS: contains an authoritative name server</span></span>
- <span data-ttu-id="c2db8-150">SOA: 権威のゾーンの開始が含まれています</span><span class="sxs-lookup"><span data-stu-id="c2db8-150">SOA: contains the start of a zone of authority</span></span>
- <span data-ttu-id="c2db8-151">MX: メール交換で使用されます</span><span class="sxs-lookup"><span data-stu-id="c2db8-151">MX: used for mail exchange</span></span>
- <span data-ttu-id="c2db8-152">SRV: ドメインによって提供されるサービスに関する情報が含まれています</span><span class="sxs-lookup"><span data-stu-id="c2db8-152">SRV: contains information on the service offered by the domain</span></span>

<span data-ttu-id="c2db8-153">CNAME および TXT レコード タイプを除いて、アプリケーションでは DNS データ レコードを受信するために、4-byte のアラインされたバッファーを提供する必要があります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-153">With the exception of CNAME and TXT record types, the application must supply a 4-byte aligned buffer to receive the DNS data record.</span></span>

<span data-ttu-id="c2db8-154">NetX Duo DNS クライアントでは、レコード データはバッファー領域を最も効率的に使用できるように格納されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-154">In NetX Duo DNS Client, record data is stored in such a way to make most efficient use of buffer space.</span></span>

<span data-ttu-id="c2db8-155">固定長のレコード バッファー (タイプ AAAA レコード) の例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-155">An example of a record buffer of fixed length (type AAAA record) is shown below:</span></span>

![固定長のレコード バッファー](media/image2.png)

<span data-ttu-id="c2db8-157">ホスト名が可変長である NS レコードのような、レコード タイプが可変のデータ長を持つクエリの場合、NetX Duo DNS クライアントは次のようにデータを保存します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-157">For those queries whose record types have variable data length, such as NS records whose host names are of variable length, NetX Duo DNS Client saves the data as follows.</span></span> <span data-ttu-id="c2db8-158">DNS クライアント クエリで指定されたバッファーは、固定長データの領域と非構造化メモリの領域に編成されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-158">The buffer supplied in the DNS Client query is organized into an area of fixed length data and an area of unstructured memory.</span></span> <span data-ttu-id="c2db8-159">メモリ バッファーの先頭は、4-byte のアラインされたレコード エントリに編成されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-159">The top of the memory buffer is organized into 4-byte aligned record entries.</span></span> <span data-ttu-id="c2db8-160">各レコード エントリには IP アドレスと、その IP アドレスの可変長データへのポインターが含まれます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-160">Each record entry contains the IP address and a pointer to the variable length data for that IP address.</span></span> <span data-ttu-id="c2db8-161">各 IP アドレスの可変長データは、メモリ バッファーの末尾から始まる非構造化領域のメモリに格納されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-161">The variable length data for each IP address are stored in the unstructured area memory starting at the end of the memory buffer.</span></span> <span data-ttu-id="c2db8-162">連続する各レコード エントリの可変長データは、前のレコード エントリの変数データに隣接する次の領域のメモリに保存されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-162">The variable length data for each successive record entry is saved in the next area memory adjacent to the previous record entries variable data.</span></span> <span data-ttu-id="c2db8-163">そのため、新しいレコード エントリと可変長データを格納するためのメモリが不足するまで、レコード エントリを格納する構造化されたメモリ領域に向かって変数データが "増加" していきます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-163">Hence, the variable data ‘grows’ towards the structured area of memory containing the record entries until there is insufficient memory to store another record entry and variable data.</span></span>

<span data-ttu-id="c2db8-164">これは次の図で示されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-164">This is shown in the figure below:</span></span>

![図 2](media/image3.png)

<span data-ttu-id="c2db8-166">DNS ドメイン名 (NS) データ ストレージの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-166">The example of the DNS domain name (NS) data storage is shown above.</span></span>

<span data-ttu-id="c2db8-167">レコード ストレージ形式を使用する NetX Duo DNS クライアントのクエリは、レコード バッファーに保存されたレコードの数を返します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-167">NetX Duo DNS Client queries using the record storage format return the number of records saved to the record buffer.</span></span> <span data-ttu-id="c2db8-168">この情報を使用して、アプリケーションでレコード バッファーから NS レコードを抽出できます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-168">This information enables the application to extract NS records from the record buffer.</span></span>

<span data-ttu-id="c2db8-169">このレコード ストレージ形式を使用して可変長の DNS データを格納する DNS クライアント クエリの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-169">An example of a DNS Client query that stores variable length DNS data using this record storage format is shown below:</span></span>

```C
UINT  _nx_dns_domain_name_server_get(NX_DNS *dns_ptr, 
                    UCHAR *host_name, VOID *record_buffer, 
                    UINT buffer_size, UINT *record_count, 
                    ULONG wait_option);
```


<span data-ttu-id="c2db8-170">詳細については、第 3 章の「DNS クライアント サービスの説明」を参照してください。</span><span class="sxs-lookup"><span data-stu-id="c2db8-170">More details are available in Chapter 3, “Description of DNS Client Services”.</span></span>

## <a name="dns-cache"></a><span data-ttu-id="c2db8-171">DNS キャッシュ</span><span class="sxs-lookup"><span data-stu-id="c2db8-171">DNS Cache</span></span>

<span data-ttu-id="c2db8-172">NX_DNS_CACHE_ENABLE が有効になっている場合、NetX Duo DNS クライアントでは DNS キャッシュ機能がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-172">If NX_DNS_CACHE_ENABLE is enabled, NetX Duo DNS Client supports the DNS Cache feature.</span></span> <span data-ttu-id="c2db8-173">DNS クライアントを作成した後、アプリケーションでは API *nx_dns_cache_initialize()* を呼び出して、特別な DNS キャッシュを設定できます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-173">After creating the DNS Client, the application can call the API *nx_dns_cache_initialize()* to set the special DNS Cache.</span></span> <span data-ttu-id="c2db8-174">DNS キャッシュ機能を有効にすると、DNS クライアントは DNS クエリの送信を開始する前に、DNS キャッシュから使用可能な応答を検索します。使用可能な応答が見つかった場合は、その応答を直接アプリケーションに返します。見つからなかった場合、DNS クライアントは DNS サーバーにクエリ メッセージを送信し、応答を待ちます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-174">If enable DNS Cache feature, DNS Client will find the available answer from DNS Cache before starts to send DNS query, if find the available answer, directly return the answer to application, otherwise DNS Client sends out query message to DNS server and waits for the reply.</span></span> <span data-ttu-id="c2db8-175">DNS クライアントが応答メッセージを取得したときに利用可能な空きキャッシュがある場合、DNS クライアントはアプリケーションに応答を返し、応答をリソース レコードとして DNS キャッシュに追加します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-175">When DNS Client gets the response message and there is free cache available, DNS Client returns the answer to the application and also adds the answer as resource record into DNS cache.</span></span>

<span data-ttu-id="c2db8-176">それぞれの応答は、キャッシュ内の *NX_DNS_RR* データ構造体 (リソース レコード) に格納されます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-176">Each answer a data structure *NX_DNS_RR* (Resource Record) in the cache.</span></span> <span data-ttu-id="c2db8-177">レコード内の文字列 (リソース レコード名とデータ) は可変長であるため、NX_DNS_RR 構造体には格納されません。</span><span class="sxs-lookup"><span data-stu-id="c2db8-177">Strings (resource record name and data) in Records are variable length, therefore are not stored in the NX_DNS_RR structure.</span></span> <span data-ttu-id="c2db8-178">レコードには、文字列が格納されている実際のメモリ位置へのポインターが含まれています。</span><span class="sxs-lookup"><span data-stu-id="c2db8-178">The Record contains pointers to the actual memory location where the strings are stored.</span></span> <span data-ttu-id="c2db8-179">文字列テーブルとレコードはキャッシュを共有します。</span><span class="sxs-lookup"><span data-stu-id="c2db8-179">The string table and the Records share the cache.</span></span> <span data-ttu-id="c2db8-180">レコードはキャッシュの先頭から格納され、キャッシュの末尾に向かって増加していきます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-180">Records are stored from the beginning of the cache, and grow towards the end of the cache.</span></span> <span data-ttu-id="c2db8-181">文字列テーブルはキャッシュの末尾から開始し、キャッシュの先頭に向かって増加していきます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-181">The string table starts from the end of the cache and grows towards the beginning of the cache.</span></span> <span data-ttu-id="c2db8-182">文字列テーブル内の各文字列には、長さフィールドとカウンター フィールドがあります。</span><span class="sxs-lookup"><span data-stu-id="c2db8-182">Each string in the string table has a length field and a counter field.</span></span> <span data-ttu-id="c2db8-183">文字列が文字列テーブルに追加されるときに、同じ文字列がテーブルに既に存在する場合は、カウンター値が増分され、その文字列にはメモリが割り当てられません。</span><span class="sxs-lookup"><span data-stu-id="c2db8-183">When a string is added to the string table, if the same string is already present in the table, the counter value is incremented and no memory is allocated for the string.</span></span> <span data-ttu-id="c2db8-184">リソース レコードまたは新しい文字列をこれ以上キャッシュに追加できない場合、キャッシュはいっぱいであると見なされます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-184">The cache is considered full if no more resource records or new strings can be added to the cache.</span></span>

## <a name="dns-client-limitations"></a><span data-ttu-id="c2db8-185">DNS クライアントの制限事項</span><span class="sxs-lookup"><span data-stu-id="c2db8-185">DNS Client Limitations</span></span>

<span data-ttu-id="c2db8-186">DNS クライアントでは、一度に 1 つの DNS 要求がサポートされます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-186">The DNS Client supports one DNS request at a time.</span></span> <span data-ttu-id="c2db8-187">別の DNS 要求を実行しようとしているスレッドは、前の DNS 要求が完了するまで一時的にブロックされます。</span><span class="sxs-lookup"><span data-stu-id="c2db8-187">Threads attempting to make another DNS request are temporarily blocked until the previous DNS request is complete.</span></span>

<span data-ttu-id="c2db8-188">NetX Duo DNS クライアントは、権威のある応答からのデータを使用して、他の DNS サーバーに追加の DNS クエリを転送しません。</span><span class="sxs-lookup"><span data-stu-id="c2db8-188">The NetX Duo DNS Client does not use data from authoritative answers to forward additional DNS queries to other DNS servers.</span></span>

## <a name="dns-rfcs"></a><span data-ttu-id="c2db8-189">DNS RFC</span><span class="sxs-lookup"><span data-stu-id="c2db8-189">DNS RFCs</span></span>

<span data-ttu-id="c2db8-190">NetX Duo DNS は、次の RFC に準拠しています。</span><span class="sxs-lookup"><span data-stu-id="c2db8-190">NetX Duo DNS is compliant with the following RFCs:</span></span>

- <span data-ttu-id="c2db8-191">RFC1034 ドメイン名 - 概念と機能</span><span class="sxs-lookup"><span data-stu-id="c2db8-191">RFC1034 DOMAIN NAMES - CONCEPTS AND FACILITIES</span></span>
- <span data-ttu-id="c2db8-192">RFC1035 ドメイン名 - 実装と仕様</span><span class="sxs-lookup"><span data-stu-id="c2db8-192">RFC1035 DOMAIN NAMES - IMPLEMENTATION AND SPECIFICATION</span></span>
- <span data-ttu-id="c2db8-193">RFC1480 US ドメイン</span><span class="sxs-lookup"><span data-stu-id="c2db8-193">RFC1480 The US Domain</span></span>
- <span data-ttu-id="c2db8-194">RFC 2782 サービスの場所を指定するための DNS RR (DNS SRV)</span><span class="sxs-lookup"><span data-stu-id="c2db8-194">RFC 2782 A DNS RR for specifying the location of services (DNS SRV)</span></span>
- <span data-ttu-id="c2db8-195">RFC 3596 IP バージョン 6 をサポートするための DNS 拡張</span><span class="sxs-lookup"><span data-stu-id="c2db8-195">RFC 3596 DNS Extensions to Support IP Version 6</span></span>