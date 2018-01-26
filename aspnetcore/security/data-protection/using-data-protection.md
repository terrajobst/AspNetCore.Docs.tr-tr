---
title: "Veri Koruma API'ları ile çalışmaya başlama"
author: rick-anderson
description: "Bu belge koruma ve uygulama veri koruması kaldırıldığında ASP.NET Core veri koruma API kullanımı açıklanmaktadır."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: c6a631b6dc4a7855b11031dfcef42b17906754b0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="96964-103">Veri Koruma API'ları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="96964-103">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="96964-104">Basit ve koruma verilerini aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="96964-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="96964-105">Veri koruyucu bir veri koruma sağlayıcıdan oluşturun.</span><span class="sxs-lookup"><span data-stu-id="96964-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="96964-106">Çağrı `Protect` yöntemi ile korumak istediğiniz verileri.</span><span class="sxs-lookup"><span data-stu-id="96964-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="96964-107">Çağrı `Unprotect` yöntemi verilerle istediğiniz geri düz metne dönüştürecek.</span><span class="sxs-lookup"><span data-stu-id="96964-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="96964-108">Çoğu çerçeveler ve ASP.NET veya SignalR, gibi uygulama modelleri zaten veri koruma sisteminde yapılandırmak ve bağımlılık ekleme erişim bir hizmet kapsayıcısı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="96964-108">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="96964-109">Aşağıdaki örnek, bir hizmet kapsayıcısı bağımlılık ekleme için yapılandırma ve veri koruma yığını kaydetme, veri koruma sağlayıcısı dı aracılığıyla alma, koruyucusu ve koruma sonra kaldırmayı veri oluşturma gösterir</span><span class="sxs-lookup"><span data-stu-id="96964-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="96964-110">Bir koruyucu oluşturduğunuzda bir veya daha fazla sağlamalısınız [amacı dizeleri](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="96964-110">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="96964-111">Bir amaç dize tüketicileri arasında yalıtım sağlar.</span><span class="sxs-lookup"><span data-stu-id="96964-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="96964-112">Örneğin, "Yeşil" amacı dizesi ile oluşturulan bir koruyucu "mor" bir amacı koruyucusu tarafından sağlanan verilerin korumasını yapamazlar.</span><span class="sxs-lookup"><span data-stu-id="96964-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="96964-113">Örneklerini `IDataProtectionProvider` ve `IDataProtector` olan birden çok çağıranlar için iş parçacığı.</span><span class="sxs-lookup"><span data-stu-id="96964-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="96964-114">Bir bileşen bir başvuru edinir sonra istemiş bir `IDataProtector` çağrısıyla `CreateProtector`, birden çok çağrılar için bu başvuru kullanacağı `Protect` ve `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="96964-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="96964-115">Çağrı `Unprotect` korumalı yükü doğrulandı veya yararlanılarak CryptographicException özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="96964-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="96964-116">Bazı bileşenler hatalarını yok saymak isteyebilirsiniz sırasında işlemleri; korumayı Kaldır kimlik doğrulaması tanımlama bilgileri okuyan bir bileşeni bu hatayı işleyebilir ve isteği, tanımlama bilgisi hiç sahipmiş gibi ele alın yerine depolayabileceği isteği başarısız.</span><span class="sxs-lookup"><span data-stu-id="96964-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="96964-117">Bu davranış istediğiniz bileşenleri tüm özel durumları swallowing yerine CryptographicException özellikle catch.</span><span class="sxs-lookup"><span data-stu-id="96964-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
