---
title: ASP.NET core'da gRPC giriş
author: juntaoluo
description: GRPC hizmetleriyle Kestrel sunucusu ve ASP.NET Core yığını hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142623"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="2bb7e-103">ASP.NET core'da gRPC giriş</span><span class="sxs-lookup"><span data-stu-id="2bb7e-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="2bb7e-104">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="2bb7e-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="2bb7e-105">[gRPC](https://grpc.io/docs/guides/) dil belirsiz, yüksek performanslı uzak yordam çağrısı (RPC) çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="2bb7e-106">GRPC temelleri hakkında daha fazla bilgi için bkz. [gRPC belgeleri sayfasını](https://grpc.io/docs/).</span><span class="sxs-lookup"><span data-stu-id="2bb7e-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="2bb7e-107">GRPC başlıca avantajları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="2bb7e-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="2bb7e-108">Modern yüksek performanslı basit RPC çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="2bb7e-109">Önce anlaşma API geliştirme, varsayılan dil belirsiz uygulamalar için izin, protokol arabellekleri kullanarak.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="2bb7e-110">Kesin tür belirtilmiş sunucular ve istemciler oluşturmak birçok dili için kullanılabilen araçları.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="2bb7e-111">İstemci, sunucu ve iki yönlü akış çağrılarını destekler.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="2bb7e-112">Daha düşük ağ kullanım Protobuf ikili serileştirme ile.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="2bb7e-113">Bu avantajlar gRPC için ideal bir çözüm oluşturur:</span><span class="sxs-lookup"><span data-stu-id="2bb7e-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="2bb7e-114">Verimliliği kritik olduğu basit bir mikro hizmetler.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="2bb7e-115">Birden çok dilde geliştirme için gerekli olduğu polyglot sistemler.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="2bb7e-116">Akış istekler veya yanıtlar işlemeniz gereken noktadan noktaya gerçek zamanlı Hizmetleri.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="2bb7e-117">Ancak bir C# uygulama şu anda üzerinde resmi [gRPC sayfa](https://grpc.io/docs/quickstart/csharp.html), geçerli yerel kitaplık C'de yazılmış kullanır (gRPC [C-çekirdek](https://grpc.io/blog/grpc-stacks)).</span><span class="sxs-lookup"><span data-stu-id="2bb7e-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="2bb7e-118">İş şu anda Kestrel HTTP sunucusu ve tam olarak yönetilen bir ASP.NET Core yığın göre yeni bir uygulama sağlamak için devam ediyor.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="2bb7e-119">Aşağıdaki belgeler, gRPC hizmetler bu yeni bir uygulama oluşturmaya giriş bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2bb7e-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2bb7e-120">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2bb7e-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>