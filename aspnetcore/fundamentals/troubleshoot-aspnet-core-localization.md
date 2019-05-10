---
title: ASP.NET Core yerelleştirme sorunlarını giderme
author: hishamco
description: ASP.NET Core uygulamalarında Yerelleştirme sorunları tanılamayı öğrenin.
ms.author: riande
ms.date: 01/24/2019
uid: fundamentals/troubleshoot-aspnet-core-localization
ms.openlocfilehash: c76732c1a0389818f8f9efae8fe384ca0f9ca308
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087361"
---
# <a name="troubleshoot-aspnet-core-localization"></a><span data-ttu-id="e7925-103">ASP.NET Core yerelleştirme sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="e7925-103">Troubleshoot ASP.NET Core Localization</span></span>

<span data-ttu-id="e7925-104">Tarafından [Hisham Bin Ateya](https://github.com/hishamco)</span><span class="sxs-lookup"><span data-stu-id="e7925-104">By [Hisham Bin Ateya](https://github.com/hishamco)</span></span>

<span data-ttu-id="e7925-105">Bu makalede, ASP.NET Core uygulamasını yerelleştirme sorunları tanılama hakkında yönergeler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e7925-105">This article provides instructions on how to diagnose ASP.NET Core app localization issues.</span></span>

## <a name="localization-configuration-issues"></a><span data-ttu-id="e7925-106">Yerelleştirme yapılandırma sorunları</span><span class="sxs-lookup"><span data-stu-id="e7925-106">Localization configuration issues</span></span>

<span data-ttu-id="e7925-107">**Yerelleştirme ara yazılım sırası**</span><span class="sxs-lookup"><span data-stu-id="e7925-107">**Localization middleware order**</span></span>  
<span data-ttu-id="e7925-108">Yerelleştirme ara yazılım, beklendiği gibi sıralı değildir çünkü uygulamayı yerelleştirmek değil.</span><span class="sxs-lookup"><span data-stu-id="e7925-108">The app may not localize because the localization middleware isn't ordered as expected.</span></span>

<span data-ttu-id="e7925-109">Bu sorunu çözmek için önce MVC ara yazılım, yerelleştirme ara yazılım kayıtlı emin olun.</span><span class="sxs-lookup"><span data-stu-id="e7925-109">To resolve this issue, ensure that localization middleware is registered before MVC middleware.</span></span> <span data-ttu-id="e7925-110">Aksi takdirde, yerelleştirme ara yazılım uygulanmaz.</span><span class="sxs-lookup"><span data-stu-id="e7925-110">Otherwise, the localization middleware isn't applied.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

<span data-ttu-id="e7925-111">**Yerelleştirme kaynakları yol bulunamadı**</span><span class="sxs-lookup"><span data-stu-id="e7925-111">**Localization resources path not found**</span></span>

<span data-ttu-id="e7925-112">**Desteklenen kültürler RequestCultureProvider içinde ile bir kez kayıtlı eşleşmiyor**</span><span class="sxs-lookup"><span data-stu-id="e7925-112">**Supported Cultures in RequestCultureProvider don't match with registered once**</span></span>  

## <a name="resource-file-naming-issues"></a><span data-ttu-id="e7925-113">Kaynak dosya adlandırma sorunları</span><span class="sxs-lookup"><span data-stu-id="e7925-113">Resource file naming issues</span></span>

<span data-ttu-id="e7925-114">ASP.NET Core'nın önceden tanımlı kuralları ve ayrıntılı olarak açıklanan yerelleştirme kaynaklarını dosya adlandırma, yönergeleri [burada](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span><span class="sxs-lookup"><span data-stu-id="e7925-114">ASP.NET Core has predefined rules and guidelines for localization resources file naming, which are described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).</span></span>

## <a name="missing-resources"></a><span data-ttu-id="e7925-115">Eksik kaynakları</span><span class="sxs-lookup"><span data-stu-id="e7925-115">Missing resources</span></span>

<span data-ttu-id="e7925-116">Kaynak bulunamadığından yaygın nedenler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e7925-116">Common causes of resources not being found include:</span></span>

- <span data-ttu-id="e7925-117">Kaynak adları ya da yanlış `resx` dosya ya da yerelleştiriciye isteği.</span><span class="sxs-lookup"><span data-stu-id="e7925-117">Resource names are misspelled in either the `resx` file or the localizer request.</span></span>
- <span data-ttu-id="e7925-118">Kaynak eksik `resx` bazı dillerde, ancak diğer bulunmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e7925-118">The resource is missing from the `resx` for some languages, but exists in others.</span></span>
- <span data-ttu-id="e7925-119">Hala sorun yaşıyorsanız, yerelleştirme günlük iletilerini denetleyin (olduğu anda `Debug` günlük düzeyi) eksik kaynakları hakkında daha fazla ayrıntı için.</span><span class="sxs-lookup"><span data-stu-id="e7925-119">If you're still having trouble, check the localization log messages (which are at `Debug` log level) for more details about the missing resources.</span></span>

<span data-ttu-id="e7925-120">_**İpucu:** Kullanırken `CookieRequestCultureProvider`, ile kültürlere yerelleştirme tanımlama bilgisi değeri içinde tek tırnak kullanılmaz doğrulayın. Örneğin, `c='en-UK'|uic='en-US'` bir geçersiz tanımlama bilgisi değeri sırada `c=en-UK|uic=en-US` geçerli bir addır._</span><span class="sxs-lookup"><span data-stu-id="e7925-120">_**Hint:** When using `CookieRequestCultureProvider`, verify single quotes are not used with the cultures inside the localization cookie value. For example, `c='en-UK'|uic='en-US'` is an invalid cookie value, while `c=en-UK|uic=en-US` is a valid._</span></span>

## <a name="resources--class-libraries-issues"></a><span data-ttu-id="e7925-121">Kaynaklar ve sınıf kitaplıkları sorunları</span><span class="sxs-lookup"><span data-stu-id="e7925-121">Resources & Class Libraries issues</span></span>

<span data-ttu-id="e7925-122">Varsayılan olarak ASP.NET Core sınıf kitaplıkları aracılığıyla kaynak dosyalarını bulmak izin vermek için bir yol sağlar [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span><span class="sxs-lookup"><span data-stu-id="e7925-122">ASP.NET Core by default provides a way to allow the class libraries to find their resource files via [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).</span></span>

<span data-ttu-id="e7925-123">Sınıf kitaplıkları ile ilgili yaygın sorunları şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e7925-123">Common issues with class libraries include:</span></span>
- <span data-ttu-id="e7925-124">Eksik `ResourceLocationAttribute` bir sınıf kitaplığı engelleyecek `ResourceManagerStringLocalizerFactory` kaynakları bulmasını.</span><span class="sxs-lookup"><span data-stu-id="e7925-124">Missing the `ResourceLocationAttribute` in a class library will prevent `ResourceManagerStringLocalizerFactory` from discovering the resources.</span></span>
- <span data-ttu-id="e7925-125">Kaynak dosya adlandırma.</span><span class="sxs-lookup"><span data-stu-id="e7925-125">Resource file naming.</span></span> <span data-ttu-id="e7925-126">Daha fazla bilgi için [kaynak dosyası adlandırma sorunları](#resource-file-naming-issues) bölümü.</span><span class="sxs-lookup"><span data-stu-id="e7925-126">For more information, see [Resource file naming issues](#resource-file-naming-issues) section.</span></span>
- <span data-ttu-id="e7925-127">Kök ad alanı sınıf kitaplığı değiştiriliyor.</span><span class="sxs-lookup"><span data-stu-id="e7925-127">Changing the root namespace of the class library.</span></span> <span data-ttu-id="e7925-128">Daha fazla bilgi için [kök Namespace sorunları](#root-namespace-issues) bölümü.</span><span class="sxs-lookup"><span data-stu-id="e7925-128">For more information, see [Root Namespace issues](#root-namespace-issues) section.</span></span>

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a><span data-ttu-id="e7925-129">CustomRequestCultureProvider beklendiği gibi çalışmıyor</span><span class="sxs-lookup"><span data-stu-id="e7925-129">CustomRequestCultureProvider doesn't work as expected</span></span>

<span data-ttu-id="e7925-130">`RequestLocalizationOptions` Sınıfında üç varsayılan sağlayıcı:</span><span class="sxs-lookup"><span data-stu-id="e7925-130">The `RequestLocalizationOptions` class has three default providers:</span></span>

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

<span data-ttu-id="e7925-131">[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) yerelleştirme kültürü uygulamanızda nasıl sağlandığını özelleştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e7925-131">The [CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) allows you to customize how the localization culture is provided in your app.</span></span> <span data-ttu-id="e7925-132">`CustomRequestCultureProvider` Varsayılan sağlayıcıları gereksinimlerinizi karşılayıp kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e7925-132">The `CustomRequestCultureProvider` is used when the default providers don't meet your requirements.</span></span>

- <span data-ttu-id="e7925-133">İlk sağlayıcısında değil özel sağlayıcı düzgün çalışmıyor yaygın bir nedeni olduğundan `RequestCultureProviders` listesi.</span><span class="sxs-lookup"><span data-stu-id="e7925-133">A common reason custom provider don't work properly is that it isn't the first provider in the `RequestCultureProviders` list.</span></span> <span data-ttu-id="e7925-134">Bu sorunu çözmek için:</span><span class="sxs-lookup"><span data-stu-id="e7925-134">To resolve this issue:</span></span>

- <span data-ttu-id="e7925-135">Özel sağlayıcı konumu 0 eklemek `RequestCultureProviders` listesi aşağıdaki gibi:</span><span class="sxs-lookup"><span data-stu-id="e7925-135">Insert the custom provider at the position 0 in the `RequestCultureProviders` list as the following:</span></span>

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- <span data-ttu-id="e7925-136">Kullanım `AddInitialRequestCultureProvider` özel sağlayıcı ilk sağlayıcısı olarak ayarlamak için genişletme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e7925-136">Use `AddInitialRequestCultureProvider` extension method to set the custom provider as initial provider.</span></span>

## <a name="root-namespace-issues"></a><span data-ttu-id="e7925-137">Kök Namespace sorunları</span><span class="sxs-lookup"><span data-stu-id="e7925-137">Root Namespace issues</span></span>

<span data-ttu-id="e7925-138">Kök ad alanı bir derlemenin derleme adından farklı olduğunda, yerelleştirme, varsayılan olarak çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="e7925-138">When the root namespace of an assembly is different than the assembly name, localization doesn't work by default.</span></span> <span data-ttu-id="e7925-139">Bu sorunu kullanımı önlemek için [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), ayrıntılı olarak açıklanan [burada](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span><span class="sxs-lookup"><span data-stu-id="e7925-139">To avoid this issue use [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), which is described in detail [here](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)</span></span>

## <a name="resources--build-action"></a><span data-ttu-id="e7925-140">Kaynaklar ve derleme eylemi</span><span class="sxs-lookup"><span data-stu-id="e7925-140">Resources & Build Action</span></span>

<span data-ttu-id="e7925-141">Yerelleştirme için kaynak dosyaları kullanıyorsanız, bunlar uygun yapı eylemi olması önemlidir.</span><span class="sxs-lookup"><span data-stu-id="e7925-141">If you use resource files for localization, it's important that they have an appropriate build action.</span></span> <span data-ttu-id="e7925-142">Olmaları gerektiği **gömülü kaynak**, aksi halde `ResourceStringLocalizer` bu kaynakları bulmak mümkün değildir.</span><span class="sxs-lookup"><span data-stu-id="e7925-142">They should be **Embedded Resource**, otherwise the `ResourceStringLocalizer` is not able to find these resources.</span></span>
