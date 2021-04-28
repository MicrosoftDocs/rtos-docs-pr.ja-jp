---
title: 第 1 章 - Azure RTOS NetX DHCP クライアントの概要
description: NetX では、アプリケーションの IP アドレスは、nx_ip_create サービス呼び出しに対して指定されるパラメーターの 1 つです。
author: philmea
ms.author: philmea
ms.date: 06/04/2020
ms.topic: article
ms.service: rtos
ms.openlocfilehash: 44eb764c84a15a1bc96cf94bcbc8f81be7b41eef
ms.sourcegitcommit: e3d42e1f2920ec9cb002634b542bc20754f9544e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/22/2021
ms.locfileid: "104811621"
---
# <a name="chapter-1---introduction-to-azure-rtos-netx-dhcp-client"></a><span data-ttu-id="2860b-103">第 1 章 - Azure RTOS NetX DHCP クライアントの概要</span><span class="sxs-lookup"><span data-stu-id="2860b-103">Chapter 1 - Introduction to Azure RTOS NetX DHCP Client</span></span>

<span data-ttu-id="2860b-104">NetX では、アプリケーションの IP アドレスは、*nx_ip_create* サービス呼び出しに対して指定されるパラメーターの 1 つです。</span><span class="sxs-lookup"><span data-stu-id="2860b-104">In NetX, the application’s IP address is one of the supplied parameters to the *nx_ip_create* service call.</span></span> <span data-ttu-id="2860b-105">アプリケーションで IP アドレスが静的に、またはユーザーの構成によって認識されている場合、IP アドレスを指定しても問題はありません。</span><span class="sxs-lookup"><span data-stu-id="2860b-105">Supplying the IP address poses no problem if the IP address is known to the application, either statically or through user configuration.</span></span> <span data-ttu-id="2860b-106">ただし、アプリケーションで IP アドレスを認識していない場合、または関知しない場合もあります。</span><span class="sxs-lookup"><span data-stu-id="2860b-106">However, there are some instances where the application doesn’t know or care what its IP address is.</span></span> <span data-ttu-id="2860b-107">そのような場合は、*nx_ip_create* 関数にゼロの IP アドレスを指定し、Azure RTOS DHCP クライアント プロトコルを使用して IP アドレスを動的に取得する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2860b-107">In such situations, a zero IP address should be supplied to the *nx_ip_create* function and the Azure RTOS DHCP Client protocol should be used to dynamically obtain an IP address.</span></span>

## <a name="dynamic-ip-address-assignment"></a><span data-ttu-id="2860b-108">動的な IP アドレスの割り当て</span><span class="sxs-lookup"><span data-stu-id="2860b-108">Dynamic IP Address Assignment</span></span>

<span data-ttu-id="2860b-109">ネットワークから動的な IP アドレスを取得するために使用される基本サービスは、逆アドレス解決プロトコル (RARP) です。</span><span class="sxs-lookup"><span data-stu-id="2860b-109">The basic service used to obtain a dynamic IP address from the network is the Reverse Address Resolution Protocol (RARP).</span></span> <span data-ttu-id="2860b-110">このプロトコルは ARP に似ていますが、別のネットワーク ノードの MAC アドレスを見つけるのではなく、自身の IP アドレスを取得するように設計されています。</span><span class="sxs-lookup"><span data-stu-id="2860b-110">This protocol is similar to ARP, except it is designed to obtain an IP address for itself instead of finding the MAC address for another network node.</span></span> <span data-ttu-id="2860b-111">低レベルの RARP メッセージはローカル ネットワークでブロードキャストされます。ネットワーク上のサーバーが、RARP 応答 (動的に割り当てられた IP アドレスを含む) に応答する役割を担います。</span><span class="sxs-lookup"><span data-stu-id="2860b-111">The low-level RARP message is broadcast on the local network and it is the responsibility of a server on the network to respond with an RARP response, which contains a dynamically allocated IP address.</span></span>

<span data-ttu-id="2860b-112">RARP では IP アドレスを動的に割り当てるためのサービスが提供されますが、いくつかの短所があります。</span><span class="sxs-lookup"><span data-stu-id="2860b-112">Although RARP provides a service for dynamic allocation of IP addresses, it has several shortcomings.</span></span> <span data-ttu-id="2860b-113">最も明白な欠点は、RARP では IP アドレスの動的割り当てのみが提供されることです。</span><span class="sxs-lookup"><span data-stu-id="2860b-113">The most glaring deficiency is that RARP only provides dynamic allocation of the IP address.</span></span> <span data-ttu-id="2860b-114">ほとんどの場合、デバイスがネットワークに適切に参加するためには、さらに情報が必要となります。</span><span class="sxs-lookup"><span data-stu-id="2860b-114">In most situations, more information is necessary in order for a device to properly participate on a network.</span></span> <span data-ttu-id="2860b-115">IP アドレスに加えて、ほとんどのデバイスではネットワーク マスクとゲートウェイ IP アドレスが必要です。</span><span class="sxs-lookup"><span data-stu-id="2860b-115">In addition to an IP address, most devices need the network mask and the gateway IP address.</span></span> <span data-ttu-id="2860b-116">DNS サーバーの IP アドレスとその他のネットワーク情報が必要な場合もあります。</span><span class="sxs-lookup"><span data-stu-id="2860b-116">The IP address of a DNS server and other network information may also be needed.</span></span> <span data-ttu-id="2860b-117">RARP には、この情報を提供する機能がありません。</span><span class="sxs-lookup"><span data-stu-id="2860b-117">RARP does not have the ability to provide this information.</span></span>

## <a name="rarp-alternatives"></a><span data-ttu-id="2860b-118">RARP の代替</span><span class="sxs-lookup"><span data-stu-id="2860b-118">RARP Alternatives</span></span>

<span data-ttu-id="2860b-119">RARP の欠点を克服するために、研究者によって、ブートストラップ プロトコル (BOOTP) と呼ばれるより包括的な IP アドレス割り当てメカニズムが開発されました。</span><span class="sxs-lookup"><span data-stu-id="2860b-119">In order to overcome the deficiencies of RARP, researchers developed a more comprehensive IP address allocation mechanism called the Bootstrap Protocol (BOOTP).</span></span> <span data-ttu-id="2860b-120">このプロトコルでは、IP アドレスを動的に割り当てることができ、追加の重要なネットワーク情報も提供されます。</span><span class="sxs-lookup"><span data-stu-id="2860b-120">This protocol has the ability to dynamically allocate an IP address and also provide additional important network information.</span></span> <span data-ttu-id="2860b-121">ただし、BOOTP には、静的なネットワーク構成用に設計されているという欠点があります。</span><span class="sxs-lookup"><span data-stu-id="2860b-121">However, BOOTP has the drawback of being designed for static network configurations.</span></span> <span data-ttu-id="2860b-122">素早いアドレス割り当てや自動アドレス割り当ては許可されません。</span><span class="sxs-lookup"><span data-stu-id="2860b-122">It does not allow for quick or automated address assignment.</span></span>

<span data-ttu-id="2860b-123">これが動的ホスト構成プロトコル (DHCP) が非常に便利である点です。</span><span class="sxs-lookup"><span data-stu-id="2860b-123">This is where the Dynamic Host Configuration Protocol (DHCP) is extremely useful.</span></span> <span data-ttu-id="2860b-124">DHCP は、BOOTP の基本的な機能を拡張するように設計されており、完全に自動化された IP サーバーの割り当てと、指定した期間にわたって IP アドレスをクライアントに "リース" することによる完全に動的な IP アドレスの割り当てが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2860b-124">DHCP is designed to extend the basic functionality of BOOTP to include completely automated IP server allocation and completely dynamic IP address allocation through “leasing” an IP address to a client for a specified period of time.</span></span> <span data-ttu-id="2860b-125">DHCP は、BOOTP のように静的な方法で IP アドレスを割り当てるように構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="2860b-125">DHCP can also be configured to allocate IP addresses in a static manner like BOOTP.</span></span>

## <a name="dhcp-messages"></a><span data-ttu-id="2860b-126">DHCP のメッセージ</span><span class="sxs-lookup"><span data-stu-id="2860b-126">DHCP Messages</span></span>

<span data-ttu-id="2860b-127">DHCP では BOOTP の機能が大幅に拡張されていますが、BOOTP と同じメッセージ形式が使用され、BOOTP と同じベンダー オプションがサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2860b-127">Although DHCP greatly enhances the functionality of BOOTP, DHCP uses the same message format as BOOTP and supports the same vendor options as BOOTP.</span></span> <span data-ttu-id="2860b-128">この機能を実行するために、DHCP では次の 7 つの新しい DHCP 固有のオプションが導入されています。</span><span class="sxs-lookup"><span data-stu-id="2860b-128">In order to perform its function, DHCP introduces seven new DHCP-specific options, as follows:</span></span>

- <span data-ttu-id="2860b-129">DISCOVER (1) (DHCP クライアントによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-129">DISCOVER (1) (sent by DHCP Client)</span></span>

- <span data-ttu-id="2860b-130">OFFER (2) (DHCP サーバーによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-130">OFFER (2) (sent by DHCP Server)</span></span>

- <span data-ttu-id="2860b-131">REQUEST (3) (DHCP クライアントによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-131">REQUEST (3) (sent by DHCP Client)</span></span>

- <span data-ttu-id="2860b-132">DECLINE (4) (DHCP クライアントによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-132">DECLINE (4) (sent by DHCP Client)</span></span>

- <span data-ttu-id="2860b-133">ACK (5) (DHCP サーバーによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-133">ACK (5) (sent by DHCP Server)</span></span>

- <span data-ttu-id="2860b-134">NACK (6) (DHCP サーバーによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-134">NACK (6) (sent by DHCP Server)</span></span>

- <span data-ttu-id="2860b-135">RELEASE (7) (DHCP クライアントによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-135">RELEASE (7) (sent by DHCP Client)</span></span>

- <span data-ttu-id="2860b-136">INFORM (8) (DHCP クライアントによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-136">INFORM (8) (sent by DHCP Client)</span></span>

- <span data-ttu-id="2860b-137">FORCERENEW (9) (DHCP クライアントによって送信されます)</span><span class="sxs-lookup"><span data-stu-id="2860b-137">FORCERENEW (9) (sent by DHCP Client)</span></span>

## <a name="dhcp-communication"></a><span data-ttu-id="2860b-138">DHCP による通信</span><span class="sxs-lookup"><span data-stu-id="2860b-138">DHCP Communication</span></span>

<span data-ttu-id="2860b-139">DHCP では UDP プロトコルを利用して、要求とフィールドの応答が送信されます。</span><span class="sxs-lookup"><span data-stu-id="2860b-139">DHCP utilizes the UDP protocol to send requests and field responses.</span></span> <span data-ttu-id="2860b-140">IP アドレスが提供される前に、DHCP 情報を運ぶ UDP メッセージが、IP ブロードキャスト アドレス 255.255.255.255 を利用して送受信されます。</span><span class="sxs-lookup"><span data-stu-id="2860b-140">Prior to having an IP address, UDP messages carrying the DHCP information are sent and received by utilizing the IP broadcast address of 255.255.255.255.</span></span>

## <a name="dhcp-client-state-machine"></a><span data-ttu-id="2860b-141">DHCP クライアントのステート マシン</span><span class="sxs-lookup"><span data-stu-id="2860b-141">DHCP Client State Machine</span></span>

<span data-ttu-id="2860b-142">DHCP クライアントは、ステート マシンとして実装されます。</span><span class="sxs-lookup"><span data-stu-id="2860b-142">The DHCP Client is implemented as a state machine.</span></span> <span data-ttu-id="2860b-143">このステート マシンは、*nx_dhcp_create* の処理中に作成される内部 DHCP スレッドによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="2860b-143">The state machine is processed by an internal DHCP thread that is created during *nx_dhcp_create* processing.</span></span> <span data-ttu-id="2860b-144">DHCP クライアントの主な状態は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2860b-144">The main states of DHCP Client are as follows:</span></span>


- <span data-ttu-id="2860b-145">**NX_DHCP_STATE_BOOT** 以前の IP アドレスから開始します</span><span class="sxs-lookup"><span data-stu-id="2860b-145">**NX_DHCP_STATE_BOOT** Starting with a previous IP address</span></span>

- <span data-ttu-id="2860b-146">**NX_DHCP_STATE_INIT** 以前の IP アドレス値を使用せずに開始します</span><span class="sxs-lookup"><span data-stu-id="2860b-146">**NX_DHCP_STATE_INIT** Starting with no previous   IP address value</span></span>

- <span data-ttu-id="2860b-147">**NX_DHCP_STATE_SELECTING** 任意の DHCP サーバーからの応答を待機しています</span><span class="sxs-lookup"><span data-stu-id="2860b-147">**NX_DHCP_STATE_SELECTING** Waiting for a response from any DHCP server</span></span>

- <span data-ttu-id="2860b-148">**NX_DHCP_STATE_REQUESTING** DHCP サーバーが特定され、IP アドレス要求を送信しました</span><span class="sxs-lookup"><span data-stu-id="2860b-148">**NX_DHCP_STATE_REQUESTING** DHCP Server identified, IP address request sent</span></span>

- <span data-ttu-id="2860b-149">**NX_DHCP_STATE_BOUND** DHCP による IP アドレスのリースが確立されました</span><span class="sxs-lookup"><span data-stu-id="2860b-149">**NX_DHCP_STATE_BOUND** DHCP IP Address lease established</span></span>

- <span data-ttu-id="2860b-150">**NX_DHCP_STATE_RENEWING** DHCP による IP アドレスのリースの更新時間が経過し、更新を要求されました</span><span class="sxs-lookup"><span data-stu-id="2860b-150">**NX_DHCP_STATE_RENEWING** DHCP IP Address lease renewal time elapsed, renewal requested</span></span>

- <span data-ttu-id="2860b-151">**NX_DHCP_STATE_REBINDING** DHCP による IP アドレスのリースの再バインドの時間が経過し、更新を要求されました</span><span class="sxs-lookup"><span data-stu-id="2860b-151">**NX_DHCP_STATE_REBINDING** DHCP IP Address lease rebind time elapsed, renewal requested</span></span>

- <span data-ttu-id="2860b-152">**NX_DHCP_STATE_FORCERENEW** DHCP による IP アドレスのリースが確立されました。サーバー (現在はサポートされていません) またはアプリケーションによって nx_dhcp_force_renew が呼び出されることによって更新が強制されます</span><span class="sxs-lookup"><span data-stu-id="2860b-152">**NX_DHCP_STATE_FORCERENEW** DHCP IP Address lease established, force renewal by server (currently not supported) or by the application calling nx_dhcp_force_renew</span></span>

- <span data-ttu-id="2860b-153">**NX_DHCP_STATE_ADDRESS_PROBING** DHCP による IP アドレスのプローブ。IP アドレスの競合を検出するために、ARP プローブが送信されます。</span><span class="sxs-lookup"><span data-stu-id="2860b-153">**NX_DHCP_STATE_ADDRESS_PROBING** DHCP IP Address probing, send the ARP probe to detect IP address conflict.</span></span>

## <a name="dhcp-client-multiple-interface-support"></a><span data-ttu-id="2860b-154">DHCP クライアントでの複数インターフェイスのサポート</span><span class="sxs-lookup"><span data-stu-id="2860b-154">DHCP Client Multiple Interface Support</span></span>

<span data-ttu-id="2860b-155">DHCP クライアントは、1 つのネットワーク インターフェイスでのみ実行するように以前は実装されていました。</span><span class="sxs-lookup"><span data-stu-id="2860b-155">The DHCP Client was previously implemented to run on only a single network interface.</span></span> <span data-ttu-id="2860b-156">既定の動作では、DHCP クライアントはプライマリ インターフェイスで実行されていました (現在でも)。</span><span class="sxs-lookup"><span data-stu-id="2860b-156">The default behavior was (and still is) for the DHCP Client to run on the primary interface.</span></span> <span data-ttu-id="2860b-157">*nx_dhcp_set_interface_index* を呼び出すことによって、アプリケーションでプライマリ インターフェイスではなく、セカンダリ ネットワーク インターフェイスで DHCP を実行することができました (現在でも)。</span><span class="sxs-lookup"><span data-stu-id="2860b-157">By calling *nx_dhcp_set_interface_index*, the application could (and still can) run DHCP on a secondary network interface instead of the primary interface.</span></span>

<span data-ttu-id="2860b-158">DHCP を複数のインターフェイスで並列的に実行することがサポートされるようになりました。</span><span class="sxs-lookup"><span data-stu-id="2860b-158">It now supports DHCP running on multiple interfaces in parallel.</span></span> <span data-ttu-id="2860b-159">複数の物理インターフェイスで DHCP クライアントを同時に実行する方法の詳細については、第 2 章の **複数のインターフェイスに同時に存在する DHCP クライアント** に関するページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="2860b-159">See **DHCP Client on Multiple Interfaces Simultaneously** in Chapter Two for specific details how to run DHCP Client on more than one physical interface simultaneously.</span></span>

## <a name="dhcp-user-request"></a><span data-ttu-id="2860b-160">DHCP でのユーザー要求</span><span class="sxs-lookup"><span data-stu-id="2860b-160">DHCP User Request</span></span>

<span data-ttu-id="2860b-161">DHCP サーバーによって IP アドレスが付与されたら、DHCP クライアントの処理では、*nx_dhcp_user_option_request* サービスを使用して、追加のパラメーターを要求できます (一度に 1 つずつ)。</span><span class="sxs-lookup"><span data-stu-id="2860b-161">Once the DHCP server grants an IP address, the DHCP client processing can request additional parameters — one at a time — by using the *nx_dhcp_user_option_request* service.</span></span>

## <a name="dhcp-client-socket-queue"></a><span data-ttu-id="2860b-162">DHCP クライアントのソケット キュー</span><span class="sxs-lookup"><span data-stu-id="2860b-162">DHCP Client Socket Queue</span></span> 

<span data-ttu-id="2860b-163">DHCP クライアントでは、サーバーが応答するのを待機している間、他の DHCP クライアントに向けた DHCP サーバーからのブロードキャスト パケットがそのソケット受信キューから自動的にクリアされます。</span><span class="sxs-lookup"><span data-stu-id="2860b-163">The DHCP Client automatically clears broadcast packets from DHCP Servers intended for other DHCP Clients from its socket receive queue while waiting for Server to respond to itself.</span></span> <span data-ttu-id="2860b-164">ビジー状態のネットワークでは、そうしないと、そのクライアントに向けられたパケットが破棄される可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2860b-164">In a busy network, not doing so could cause packets intended for the Client to be dropped.</span></span>

## <a name="dhcp-rfcs"></a><span data-ttu-id="2860b-165">DHCP の RFC</span><span class="sxs-lookup"><span data-stu-id="2860b-165">DHCP RFCs</span></span>

<span data-ttu-id="2860b-166">NetX DHCP は、RFC2132、RFC2131、関連する RFC に準拠しています。</span><span class="sxs-lookup"><span data-stu-id="2860b-166">NetX DHCP is compliant with RFC2132, RFC2131, and related RFCs.</span></span>
