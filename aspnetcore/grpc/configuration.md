---
title: gRPC ASP.NET Core yapılandırma
author: jamesnk
description: GRPC ASP.NET Core uygulamaları için yapılandırmayı öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 05/30/2019
uid: grpc/configuration
ms.openlocfilehash: e269d701f45c0b852a9006107f0162cc5af2c38a
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67814921"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="650a3-103">gRPC ASP.NET Core yapılandırma</span><span class="sxs-lookup"><span data-stu-id="650a3-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="650a3-104">Hizmetleri seçeneklerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="650a3-104">Configure services options</span></span>

<span data-ttu-id="650a3-105">Aşağıdaki tabloda, gRPC hizmetleri yapılandırmak için seçenekleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="650a3-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="650a3-106">Seçenek</span><span class="sxs-lookup"><span data-stu-id="650a3-106">Option</span></span> | <span data-ttu-id="650a3-107">Varsayılan Değer</span><span class="sxs-lookup"><span data-stu-id="650a3-107">Default Value</span></span> | <span data-ttu-id="650a3-108">Açıklama</span><span class="sxs-lookup"><span data-stu-id="650a3-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="650a3-109">Sunucudan gönderilen bayt cinsinden en büyük ileti boyutu.</span><span class="sxs-lookup"><span data-stu-id="650a3-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="650a3-110">Bir özel durum yapılandırılan en büyük ileti boyutu sonuçlarında aşan bir ileti gönderilmeye çalışılıyor.</span><span class="sxs-lookup"><span data-stu-id="650a3-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="650a3-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="650a3-111">4 MB</span></span> | <span data-ttu-id="650a3-112">Sunucu tarafından alınan bayt cinsinden en büyük ileti boyutu.</span><span class="sxs-lookup"><span data-stu-id="650a3-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="650a3-113">Sunucu bu sınırı aşan bir ileti alır bir özel durum oluşturur.</span><span class="sxs-lookup"><span data-stu-id="650a3-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="650a3-114">Bu değeri artırmak, daha büyük iletileri almasına olanak sağlar, ancak bellek tüketimi olumsuz yönde etkileyebilir.</span><span class="sxs-lookup"><span data-stu-id="650a3-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="650a3-115">Varsa `true`, ayrıntılı bir hizmet yöntemi bir özel durum oluştuğunda, özel durum iletileri istemcilere döndürülür.</span><span class="sxs-lookup"><span data-stu-id="650a3-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="650a3-116">Varsayılan, `false` değeridir.</span><span class="sxs-lookup"><span data-stu-id="650a3-116">The default is `false`.</span></span> <span data-ttu-id="650a3-117">Ayarı `EnableDetailedErrors` için `true` hassas bilgileri sızabilir.</span><span class="sxs-lookup"><span data-stu-id="650a3-117">Setting `EnableDetailedErrors` to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="650a3-118">Gzip</span><span class="sxs-lookup"><span data-stu-id="650a3-118">gzip</span></span> | <span data-ttu-id="650a3-119">Sıkıştırmak ve iletileri genişletmek için kullanılan sıkıştırmasını sağlayıcıları koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="650a3-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="650a3-120">Özel sıkıştırmasını sağlayıcıları, oluşturulur ve koleksiyona eklenir.</span><span class="sxs-lookup"><span data-stu-id="650a3-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="650a3-121">Varsayılan sağlayıcı destekler yapılandırılmış **gzip** sıkıştırma.</span><span class="sxs-lookup"><span data-stu-id="650a3-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="650a3-122">Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma algoritması.</span><span class="sxs-lookup"><span data-stu-id="650a3-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="650a3-123">Algoritma sıkıştırma sağlayıcı eşleşmelidir `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="650a3-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="650a3-124">Algoritma yanıt sıkıştırmak istemci göndererek algoritmanın desteklediği belirtmelidir **grpc-kabul-encoding** başlığı.</span><span class="sxs-lookup"><span data-stu-id="650a3-124">For the algorithm to compress a response, the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="650a3-125">Sunucudan gönderilen iletileri sıkıştırmak için kullanılan sıkıştırma düzeyi.</span><span class="sxs-lookup"><span data-stu-id="650a3-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="650a3-126">Seçenekler yapılandırılabilir tüm hizmetler için bir seçenekler temsilciye sağlayarak `AddGrpc` Çağır `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="650a3-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

<span data-ttu-id="650a3-127">Tek bir hizmet seçeneklerini geçersiz kılmak, sağlanan genel seçenekleri `AddGrpc` ve kullanılarak yapılandırılabilir `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="650a3-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a><span data-ttu-id="650a3-128">İstemci seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="650a3-128">Configure client options</span></span>

<span data-ttu-id="650a3-129">Aşağıdaki kod, ayarlar istemci en büyük gönderme ve alma ileti boyutu:</span><span class="sxs-lookup"><span data-stu-id="650a3-129">The following code sets the client maximum send and receive message size:</span></span>

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a><span data-ttu-id="650a3-130">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="650a3-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
