---
title: ASP.NET core'da veri koruma API'lerini kullanmaya başlama
author: rick-anderson
description: Koruma ve uygulama veri koruması kaldırıldıktan için ASP.NET Core veri koruma API'lerini kullanmayı öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/using-data-protection
ms.openlocfilehash: 25bf099a3d9edd7e6e0872725cbc3707750314e6
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757067"
---
# <a name="get-started-with-the-data-protection-apis-in-aspnet-core"></a><span data-ttu-id="bc797-103">ASP.NET core'da veri koruma API'lerini kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="bc797-103">Get started with the Data Protection APIs in ASP.NET Core</span></span>

<a name="security-data-protection-getting-started"></a>

<span data-ttu-id="bc797-104">Basit ve koruma verilerini aşağıdaki adımlardan oluşur:</span><span class="sxs-lookup"><span data-stu-id="bc797-104">At its simplest, protecting data consists of the following steps:</span></span>

1. <span data-ttu-id="bc797-105">Veri koruyucu bir veri koruma sağlayıcısı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="bc797-105">Create a data protector from a data protection provider.</span></span>

2. <span data-ttu-id="bc797-106">Çağrı `Protect` yöntemi ile korumak istediğiniz verileri.</span><span class="sxs-lookup"><span data-stu-id="bc797-106">Call the `Protect` method with the data you want to protect.</span></span>

3. <span data-ttu-id="bc797-107">Çağrı `Unprotect` düz metin yeniden etkinleştirmek istediğiniz veri yöntemi.</span><span class="sxs-lookup"><span data-stu-id="bc797-107">Call the `Unprotect` method with the data you want to turn back into plain text.</span></span>

<span data-ttu-id="bc797-108">Çoğu çerçeveleri ve ASP.NET Core veya SignalR, gibi uygulama modelleri zaten veri koruma sisteminde yapılandırın ve bağımlılık ekleme erişim hizmet kapsayıcı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bc797-108">Most frameworks and app models, such as ASP.NET Core or SignalR, already configure the data protection system and add it to a service container you access via dependency injection.</span></span> <span data-ttu-id="bc797-109">Aşağıdaki örnek, bir hizmet kapsayıcısı için bağımlılık ekleme ve veri koruma yığın kaydetme, DI aracılığıyla veri koruma sağlayıcısı alma, koruyucusu ve veri koruma sonra koruması kaldırılıyor oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="bc797-109">The following sample demonstrates configuring a service container for dependency injection and registering the data protection stack, receiving the data protection provider via DI, creating a protector and protecting then unprotecting data.</span></span>

[!code-csharp[](../../security/data-protection/using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="bc797-110">Bir koruyucu oluşturduğunuzda, bir veya daha fazla sağlamalısınız [amaç dizeleri](xref:security/data-protection/consumer-apis/purpose-strings).</span><span class="sxs-lookup"><span data-stu-id="bc797-110">When you create a protector you must provide one or more [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings).</span></span> <span data-ttu-id="bc797-111">Amaç dize tüketicileri arasında yalıtım sağlar.</span><span class="sxs-lookup"><span data-stu-id="bc797-111">A purpose string provides isolation between consumers.</span></span> <span data-ttu-id="bc797-112">Örneğin, "green" amaçlı dizisi ile oluşturulan bir koruyucu "mor" bir amacı bir koruyucu ile sağlanan verilerin korumasını saptayamazdınız.</span><span class="sxs-lookup"><span data-stu-id="bc797-112">For example, a protector created with a purpose string of "green" wouldn't be able to unprotect data provided by a protector with a purpose of "purple".</span></span>

>[!TIP]
> <span data-ttu-id="bc797-113">Örneklerini `IDataProtectionProvider` ve `IDataProtector` olan birden çok arayanlar için iş parçacığı açısından güvenli.</span><span class="sxs-lookup"><span data-stu-id="bc797-113">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="bc797-114">Bir bileşen için bir başvuru alır. sonra istemiş bir `IDataProtector` çağrısıyla `CreateProtector`, birden çok çağrı için bu başvuru kullanacağı `Protect` ve `Unprotect`.</span><span class="sxs-lookup"><span data-stu-id="bc797-114">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span>
>
><span data-ttu-id="bc797-115">Bir çağrı `Unprotect` korumalı yükü doğrulandı veya yararlanılarak CryptographicException oluşturmaz.</span><span class="sxs-lookup"><span data-stu-id="bc797-115">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="bc797-116">Bazı bileşenler hataları yoksayma isteyebilirsiniz sırasında işlemleri; Korumasını Kaldır kimlik doğrulaması tanımlama bilgileri okuyan bir bileşen bu hatayı işlemek ve isteği, tanımlama bilgisi hiç varmış gibi davran yerine yükseltebilir istek başarısız.</span><span class="sxs-lookup"><span data-stu-id="bc797-116">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="bc797-117">Bu davranış istediğiniz bileşenleri, tüm özel durumları swallowing yerine CryptographicException özel olarak yakalamalısınız.</span><span class="sxs-lookup"><span data-stu-id="bc797-117">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
