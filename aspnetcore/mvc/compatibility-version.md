---
title: ASP.NET Core MVC için sürüm uyumluluğu
author: rick-anderson
description: ASP.NET core'da başlangıç sınıfı, hizmet ve uygulamaların istek işlem hattı nasıl yapılandırdığını keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b360da105799a1dccb1902e167e50e78864b76a9
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647934"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="29354-103">ASP.NET Core MVC için sürüm uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="29354-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="29354-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="29354-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="29354-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Yöntemi kabul etme veya potansiyel olarak davranışı sunulan içinde ASP.NET Core MVC 2.1 veya üzeri bozucu değişiklikler çevirme için bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="29354-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="29354-106">Potansiyel olarak yeni davranış değişiklikleri genel olarak, MVC alt sistemi davranışını nasıl ve ne kadar bunlar **kodunuzu** çalışma zamanı tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="29354-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="29354-107">Seçim tarafından en son davranışı ve ASP.NET Core uzun vadeli davranışını alın.</span><span class="sxs-lookup"><span data-stu-id="29354-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="29354-108">Aşağıdaki kod, ASP.NET Core 2.2 için Uyumluluk modu ayarlar:</span><span class="sxs-lookup"><span data-stu-id="29354-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="29354-109">En son sürümünü kullanarak uygulamanızı test öneririz (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="29354-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="29354-110">Biz, çoğu uygulama en son sürümünü kullanarak davranışı değişiklikler olmaz beklenir.</span><span class="sxs-lookup"><span data-stu-id="29354-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="29354-111">Çağıran uygulamalar `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` potansiyel olarak ASP.NET Core 2.1 MVC ve sonraki 2.x sürümlerinde sunulan davranış değişiklikleri kesilmesini korunur.</span><span class="sxs-lookup"><span data-stu-id="29354-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="29354-112">Bu koruma:</span><span class="sxs-lookup"><span data-stu-id="29354-112">This protection:</span></span>

* <span data-ttu-id="29354-113">Uygulanmaz 2.1 ve üzeri yapılan tüm değişiklikler, potansiyel olarak ASP.NET Core çalışma zamanı davranışı MVC alt sistemde değişiklikler için görünür duruma yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="29354-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="29354-114">Bir sonraki ana sürümüne genişletilmez.</span><span class="sxs-lookup"><span data-stu-id="29354-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="29354-115">ASP.NET Core 2.1 ve yapmak sonraki 2.x uygulamaları için varsayılan uyumluluk **değil** çağrı `SetCompatibilityVersion` 2.0 dönük uyumluluğa yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="29354-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="29354-116">Diğer bir deyişle, değil çağırma `SetCompatibilityVersion` arama aynı `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="29354-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="29354-117">Aşağıdaki kod, ASP.NET Core 2.2 için Uyumluluk modu dışında aşağıdaki davranışları ayarlar:</span><span class="sxs-lookup"><span data-stu-id="29354-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="29354-118">Uygulamalar için uygun uyumluluk anahtarları kullanarak en son davranış değişiklikleri karşılaşırsınız:</span><span class="sxs-lookup"><span data-stu-id="29354-118">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="29354-119">En son sürümünü kullanın ve belirli bozucu davranış değişiklikleri dışında iyileştirilmiş olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="29354-119">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="29354-120">En son değişikliklerle birlikte çalışacak şekilde uygulamanızı güncelleştirmek için zaman verir.</span><span class="sxs-lookup"><span data-stu-id="29354-120">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="29354-121"><xref:Microsoft.AspNetCore.Mvc.MvcOptions> Belgelerde değişiklikler ve çoğu kullanıcı için bir geliştirme değişiklikler neden iyi bir açıklama bulunur.</span><span class="sxs-lookup"><span data-stu-id="29354-121">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions> documentation has a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="29354-122">Bazı tarihte olacaktır bir [ASP.NET Core 3.0 sürümü](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="29354-122">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="29354-123">Uyumluluk anahtarları tarafından desteklenen eski davranışları 3.0 sürümünde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="29354-123">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="29354-124">Biz, neredeyse tüm kullanıcılar teknolojisinden yararlanan pozitif değişiklikler bunlar gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="29354-124">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="29354-125">Bu değişiklikleri şimdi Tanıtımı, çoğu uygulama artık yararlanabilir ve diğerlerinin uygulamalarını güncelleştirme zamanı gerekir.</span><span class="sxs-lookup"><span data-stu-id="29354-125">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
