---
title: Yerelleştirme genişletilebilirliği
author: hishamco
description: ASP.NET Core uygulamalarında yerelleştirme API 'Lerini genişletmeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/03/2019
uid: fundamentals/localization-extensibility
ms.openlocfilehash: dfa2efe78b2e1e118e6b3f09bfc41f3330e1d721
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288944"
---
# <a name="localization-extensibility"></a>Yerelleştirme genişletilebilirliği

, [Hisham bin Ateya](https://github.com/hishamco) tarafından

Bu makale:

* Yerelleştirme API 'Lerinde genişletilebilirlik noktalarını listeler.
* ASP.NET Core uygulama yerelleştirmesini genişletme hakkında yönergeler sağlar.

## <a name="extensible-points-in-localization-apis"></a>Yerelleştirme API 'Lerinde Genişletilebilir noktaları

ASP.NET Core yerelleştirme API 'Leri genişletilebilir olacak şekilde oluşturulmuştur. Genişletilebilirlik, geliştiricilerin gereksinimlerine göre yerelleştirmeyi özelleştirmesini sağlar. Örneğin, [Orchardcore](https://github.com/orchardCMS/OrchardCore/) bir `POStringLocalizer`sahiptir. `POStringLocalizer`, Yerelleştirme kaynaklarını depolamak üzere `PO` dosyalarını kullanmak için [Taşınabilir nesne yerelleştirmesini](xref:fundamentals/portable-object-localization) kullanma hakkında ayrıntılı bilgi sağlar.

Bu makalede, yerelleştirme API 'Lerinin sağladığı iki ana genişletilebilirlik noktası listelenmektedir: 

* <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider>
* <xref:Microsoft.Extensions.Localization.IStringLocalizer>

## <a name="localization-culture-providers"></a>Yerelleştirme kültür sağlayıcıları

ASP.NET Core yerelleştirme API 'Leri, yürütülen bir isteğin geçerli kültürünü belirleyebilmesi için dört varsayılan sağlayıcıya sahiptir:

* <xref:Microsoft.AspNetCore.Localization.QueryStringRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CookieRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.AcceptLanguageHeaderRequestCultureProvider>
* <xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider>

Önceki sağlayıcılar, [Yerelleştirme ara yazılımı](xref:fundamentals/localization) belgelerinde ayrıntılı olarak açıklanmıştır. Varsayılan sağlayıcılar gereksinimlerinizi karşılamıyorsa, aşağıdaki yaklaşımlardan birini kullanarak özel bir sağlayıcı oluşturun:

### <a name="use-customrequestcultureprovider"></a>CustomRequestCultureProvider kullanma

<xref:Microsoft.AspNetCore.Localization.CustomRequestCultureProvider>, geçerli yerelleştirme kültürünü belirlemede basit bir temsilci kullanan özel bir <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> sağlar:

::: moniker range="< aspnetcore-3.0"
```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
{
    var currentCulture = "en";
    var segments = context.Request.Path.Value.Split(new char[] { '/' }, 
        StringSplitOptions.RemoveEmptyEntries);

    if (segments.Length > 1 && segments[0].Length == 2)
    {
        currentCulture = segments[0];
    }

    var requestCulture = new ProviderCultureResult(culture);
    
    return Task.FromResult(requestCulture);
}));
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
```csharp
options.AddInitialRequestCultureProvider(new CustomRequestCultureProvider(async context =>
{
    var currentCulture = "en";
    var segments = context.Request.Path.Value.Split(new char[] { '/' }, 
        StringSplitOptions.RemoveEmptyEntries);

    if (segments.Length > 1 && segments[0].Length == 2)
    {
        currentCulture = segments[0];
    }

    var requestCulture = new ProviderCultureResult(culture);
    
    return Task.FromResult(requestCulture);
}));
```

::: moniker-end

### <a name="use-a-new-implemetation-of-requestcultureprovider"></a>RequestCultureProvider 'in yeni bir ımplemei kullanın

Özel bir kaynaktan gelen istek kültür bilgilerini belirleyen yeni bir <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> uygulanması oluşturulabilir. Örneğin, özel kaynak bir yapılandırma dosyası veya veritabanı olabilir.

Aşağıdaki örnekte, *appSettings. JSON*' dan gelen istek kültür bilgilerini belirleyen <xref:Microsoft.AspNetCore.Localization.RequestCultureProvider> genişleten `AppSettingsRequestCultureProvider`gösterilmektedir:

```csharp
public class AppSettingsRequestCultureProvider : RequestCultureProvider
{
    public string CultureKey { get; set; } = "culture";

    public string UICultureKey { get; set; } = "ui-culture";

    public override Task<ProviderCultureResult> DetermineProviderCultureResult(HttpContext httpContext)
    {
        if (httpContext == null)
        {
            throw new ArgumentNullException();
        }

        var configuration = httpContext.RequestServices.GetService<IConfigurationRoot>();
        var culture = configuration[CultureKey];
        var uiCulture = configuration[UICultureKey];

        if (culture == null && uiCulture == null)
        {
            return Task.FromResult((ProviderCultureResult)null);
        }

        if (culture != null && uiCulture == null)
        {
            uiCulture = culture;
        }

        if (culture == null && uiCulture != null)
        {
            culture = uiCulture;
        }
        
        var providerResultCulture = new ProviderCultureResult(culture, uiCulture);

        return Task.FromResult(providerResultCulture);
    }
}
```

## <a name="localization-resources"></a>Yerelleştirme kaynakları

ASP.NET Core yerelleştirme <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer>sağlar. <xref:Microsoft.Extensions.Localization.ResourceManagerStringLocalizer>, Yerelleştirme kaynaklarını depolamak için `resx` kullanan bir <xref:Microsoft.Extensions.Localization.IStringLocalizer> uygulamasıdır.

`resx` dosyaları kullanma sınırlı değilsiniz. `IStringLocalized`uygulayarak, herhangi bir veri kaynağı kullanılabilir.

Aşağıdaki örnek projeler <xref:Microsoft.Extensions.Localization.IStringLocalizer>uygular: 

* [EFStringLocalizer](https://github.com/aspnet/Entropy/tree/master/samples/Localization.EntityFramework)
* [JsonStringLocalizer](https://github.com/hishamco/My.Extensions.Localization.Json)
* [SqlLocalizer](https://github.com/damienbod/AspNetCoreLocalization)
