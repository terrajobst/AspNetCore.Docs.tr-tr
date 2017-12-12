---
title: "Veri Koruma API'ları ile çalışmaya başlama"
author: rick-anderson
description: "Bu belge koruma ve uygulama veri koruması kaldırıldığında ASP.NET Core veri koruma API kullanımı açıklanmaktadır."
keywords: ASP.NET Core, veri koruma
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 39b7a73c-29d4-4137-b311-49529adcf3cb
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 535bfaf2077cda91c27e7d0d68b9804e8596070e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-the-data-protection-apis"></a><span data-ttu-id="73242-104">Veri Koruma API'ları ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="73242-104">Getting Started with the Data Protection APIs</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="73242-105">Basit ve koruma verilerini aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="73242-105">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="73242-106">Veri koruyucu bir veri koruma sağlayıcıdan oluşturun.</span><span class="sxs-lookup"><span data-stu-id="73242-106">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="73242-107">Çağrı `Protect` yöntemi ile korumak istediğiniz verileri.</span><span class="sxs-lookup"><span data-stu-id="73242-107">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="73242-108">Çağrı `Unprotect` yöntemi verilerle istediğiniz geri düz metne dönüştürecek.</span><span class="sxs-lookup"><span data-stu-id="73242-108">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="73242-109">Çoğu çerçeveler ve ASP.NET veya SignalR, gibi uygulama modelleri zaten veri koruma sisteminde yapılandırmak ve bağımlılık ekleme erişim bir hizmet kapsayıcısı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="73242-109">Most frameworks and app models, such as ASP.NET or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="73242-110">Aşağıdaki örnek, bir hizmet kapsayıcısı bağımlılık ekleme için yapılandırma ve veri koruma yığını kaydetme, veri koruma sağlayıcısı dı aracılığıyla alma, koruyucusu ve koruma sonra kaldırmayı veri oluşturma gösterir</span><span class="sxs-lookup"><span data-stu-id="73242-110">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data</span></span>

[!code-csharp[Main](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="73242-111">Bir koruyucu oluşturduğunuzda bir veya daha fazla sağlamalısınız [amacı dizeleri](consumer-apis/purpose-strings.md).</span><span class="sxs-lookup"><span data-stu-id="73242-111">When you create a protector you must provide one or more [Purpose Strings](consumer-apis/purpose-strings.md).</span></span> <span data-ttu-id="73242-112">Bir amaç dize tüketicileri arasında yalıtım sağlar.</span><span class="sxs-lookup"><span data-stu-id="73242-112">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="73242-113">Örneğin, "Yeşil" amacı dizesi ile oluşturulan bir koruyucu "mor" bir amacı koruyucusu tarafından sağlanan verilerin korumasını mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="73242-113">For example, a protector created with a purpose string of "green" would not be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="73242-114">Örneklerini `IDataProtectionProvider` ve `IDataProtector` olan birden çok çağıranlar için iş parçacığı.</span><span class="sxs-lookup"><span data-stu-id="73242-114">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="73242-115">Bu bileşen bir başvuru edinir sonra hedeflenen bir `IDataProtector` çağrısıyla `CreateProtector`, birden çok çağrılar için bu başvuru kullanacağı `Protect` ve `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="73242-115">It is intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="73242-116">Çağrı `Unprotect` korumalı yükü doğrulandı veya yararlanılarak CryptographicException özel durum oluşturacak.</span><span class="sxs-lookup"><span data-stu-id="73242-116">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="73242-117">Bazı bileşenler hatalarını yok saymak isteyebilirsiniz sırasında işlemleri; korumayı Kaldır kimlik doğrulaması tanımlama bilgileri okuyan bir bileşeni bu hatayı işleyebilir ve isteği, tanımlama bilgisi hiç sahipmiş gibi ele alın yerine depolayabileceği isteği başarısız.</span><span class="sxs-lookup"><span data-stu-id="73242-117">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="73242-118">Bu davranış istediğiniz bileşenleri tüm özel durumları swallowing yerine CryptographicException özellikle catch.</span><span class="sxs-lookup"><span data-stu-id="73242-118">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
