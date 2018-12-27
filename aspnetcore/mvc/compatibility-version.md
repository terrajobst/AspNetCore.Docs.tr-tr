---
title: ASP.NET Core MVC için sürüm uyumluluğu
author: rick-anderson
description: ASP.NET core'da başlangıç sınıfı, hizmet ve uygulamaların istek işlem hattı nasıl yapılandırdığını keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/10/2018
uid: mvc/compatibility-version
ms.openlocfilehash: 63243d99c7cb74a7e594cd309a808455c6611fc0
ms.sourcegitcommit: 68a3081dd175d6518d1bfa31b4712bd8a2dd3864
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2018
ms.locfileid: "53577857"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="58e5f-103">ASP.NET Core MVC için sürüm uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="58e5f-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="58e5f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="58e5f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="58e5f-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Yöntemi kabul etme veya potansiyel olarak davranışı sunulan içinde ASP.NET Core MVC 2.1 veya üzeri bozucu değişiklikler çevirme için bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="58e5f-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="58e5f-106">Potansiyel olarak yeni davranış değişiklikleri genel olarak, MVC alt sistemi davranışını nasıl ve ne kadar bunlar **kodunuzu** çalışma zamanı tarafından çağrılır.</span><span class="sxs-lookup"><span data-stu-id="58e5f-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="58e5f-107">Seçim tarafından en son davranışı ve ASP.NET Core uzun vadeli davranışını alın.</span><span class="sxs-lookup"><span data-stu-id="58e5f-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="58e5f-108">Aşağıdaki kod, ASP.NET Core 2.2 için Uyumluluk modu ayarlar:</span><span class="sxs-lookup"><span data-stu-id="58e5f-108">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="58e5f-109">En son sürümünü kullanarak uygulamanızı test öneririz (`CompatibilityVersion.Version_2_2`).</span><span class="sxs-lookup"><span data-stu-id="58e5f-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_2`).</span></span> <span data-ttu-id="58e5f-110">Biz, çoğu uygulama en son sürümünü kullanarak davranışı değişiklikler olmaz beklenir.</span><span class="sxs-lookup"><span data-stu-id="58e5f-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="58e5f-111">Çağıran uygulamalar `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` potansiyel olarak ASP.NET Core 2.1 MVC ve sonraki 2.x sürümlerinde sunulan davranış değişiklikleri kesilmesini korunur.</span><span class="sxs-lookup"><span data-stu-id="58e5f-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="58e5f-112">Bu koruma:</span><span class="sxs-lookup"><span data-stu-id="58e5f-112">This protection:</span></span>

* <span data-ttu-id="58e5f-113">Uygulanmaz 2.1 ve üzeri yapılan tüm değişiklikler, potansiyel olarak ASP.NET Core çalışma zamanı davranışı MVC alt sistemde değişiklikler için görünür duruma yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="58e5f-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="58e5f-114">Bir sonraki ana sürümüne genişletilmez.</span><span class="sxs-lookup"><span data-stu-id="58e5f-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="58e5f-115">ASP.NET Core 2.1 ve yapmak sonraki 2.x uygulamaları için varsayılan uyumluluk **değil** çağrı `SetCompatibilityVersion` 2.0 dönük uyumluluğa yöneliktir.</span><span class="sxs-lookup"><span data-stu-id="58e5f-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="58e5f-116">Diğer bir deyişle, değil çağırma `SetCompatibilityVersion` arama aynı `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span><span class="sxs-lookup"><span data-stu-id="58e5f-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="58e5f-117">Aşağıdaki kod, ASP.NET Core 2.2 için Uyumluluk modu dışında aşağıdaki davranışları ayarlar:</span><span class="sxs-lookup"><span data-stu-id="58e5f-117">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* [<span data-ttu-id="58e5f-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="58e5f-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="58e5f-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="58e5f-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="58e5f-120">Uygulamalar için uygun uyumluluk anahtarları kullanarak en son davranış değişiklikleri karşılaşırsınız:</span><span class="sxs-lookup"><span data-stu-id="58e5f-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="58e5f-121">En son sürümünü kullanın ve belirli bozucu davranış değişiklikleri dışında iyileştirilmiş olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="58e5f-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="58e5f-122">En son değişikliklerle birlikte çalışacak şekilde uygulamanızı güncelleştirmek için zaman verir.</span><span class="sxs-lookup"><span data-stu-id="58e5f-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="58e5f-123">[MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) sınıf kaynak yorumlar bulunan, değişiklikler ve çoğu kullanıcı için bir geliştirme değişiklikler neden iyi bir açıklama.</span><span class="sxs-lookup"><span data-stu-id="58e5f-123">The [MvcOptions](https://github.com/aspnet/AspNetCore/blob/master/src/Mvc/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="58e5f-124">Bazı tarihte olacaktır bir [ASP.NET Core 3.0 sürümü](https://github.com/aspnet/Home/wiki/Roadmap).</span><span class="sxs-lookup"><span data-stu-id="58e5f-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="58e5f-125">Uyumluluk anahtarları tarafından desteklenen eski davranışları 3.0 sürümünde kaldırılacak.</span><span class="sxs-lookup"><span data-stu-id="58e5f-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="58e5f-126">Biz, neredeyse tüm kullanıcılar teknolojisinden yararlanan pozitif değişiklikler bunlar gönderebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="58e5f-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="58e5f-127">Bu değişiklikleri şimdi Tanıtımı, çoğu uygulama artık yararlanabilir ve diğerlerinin uygulamalarını güncelleştirme zamanı gerekir.</span><span class="sxs-lookup"><span data-stu-id="58e5f-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
