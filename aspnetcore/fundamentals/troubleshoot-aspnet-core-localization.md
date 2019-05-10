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
# <a name="troubleshoot-aspnet-core-localization"></a>ASP.NET Core yerelleştirme sorunlarını giderme

Tarafından [Hisham Bin Ateya](https://github.com/hishamco)

Bu makalede, ASP.NET Core uygulamasını yerelleştirme sorunları tanılama hakkında yönergeler sağlar.

## <a name="localization-configuration-issues"></a>Yerelleştirme yapılandırma sorunları

**Yerelleştirme ara yazılım sırası**  
Yerelleştirme ara yazılım, beklendiği gibi sıralı değildir çünkü uygulamayı yerelleştirmek değil.

Bu sorunu çözmek için önce MVC ara yazılım, yerelleştirme ara yazılım kayıtlı emin olun. Aksi takdirde, yerelleştirme ara yazılım uygulanmaz.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddLocalization(options => options.ResourcesPath = "Resources");

    services.AddMvc();
}
```

**Yerelleştirme kaynakları yol bulunamadı**

**Desteklenen kültürler RequestCultureProvider içinde ile bir kez kayıtlı eşleşmiyor**  

## <a name="resource-file-naming-issues"></a>Kaynak dosya adlandırma sorunları

ASP.NET Core'nın önceden tanımlı kuralları ve ayrıntılı olarak açıklanan yerelleştirme kaynaklarını dosya adlandırma, yönergeleri [burada](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming).

## <a name="missing-resources"></a>Eksik kaynakları

Kaynak bulunamadığından yaygın nedenler şunlardır:

- Kaynak adları ya da yanlış `resx` dosya ya da yerelleştiriciye isteği.
- Kaynak eksik `resx` bazı dillerde, ancak diğer bulunmaktadır.
- Hala sorun yaşıyorsanız, yerelleştirme günlük iletilerini denetleyin (olduğu anda `Debug` günlük düzeyi) eksik kaynakları hakkında daha fazla ayrıntı için.

_**İpucu:** Kullanırken `CookieRequestCultureProvider`, ile kültürlere yerelleştirme tanımlama bilgisi değeri içinde tek tırnak kullanılmaz doğrulayın. Örneğin, `c='en-UK'|uic='en-US'` bir geçersiz tanımlama bilgisi değeri sırada `c=en-UK|uic=en-US` geçerli bir addır._

## <a name="resources--class-libraries-issues"></a>Kaynaklar ve sınıf kitaplıkları sorunları

Varsayılan olarak ASP.NET Core sınıf kitaplıkları aracılığıyla kaynak dosyalarını bulmak izin vermek için bir yol sağlar [ResourceLocationAttribute](/dotnet/api/microsoft.extensions.localization.resourcelocationattribute?view=aspnetcore-2.1).

Sınıf kitaplıkları ile ilgili yaygın sorunları şunlardır:
- Eksik `ResourceLocationAttribute` bir sınıf kitaplığı engelleyecek `ResourceManagerStringLocalizerFactory` kaynakları bulmasını.
- Kaynak dosya adlandırma. Daha fazla bilgi için [kaynak dosyası adlandırma sorunları](#resource-file-naming-issues) bölümü.
- Kök ad alanı sınıf kitaplığı değiştiriliyor. Daha fazla bilgi için [kök Namespace sorunları](#root-namespace-issues) bölümü.

## <a name="customrequestcultureprovider-doesnt-work-as-expected"></a>CustomRequestCultureProvider beklendiği gibi çalışmıyor

`RequestLocalizationOptions` Sınıfında üç varsayılan sağlayıcı:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

[CustomRequestCultureProvider](/dotnet/api/microsoft.aspnetcore.localization.customrequestcultureprovider?view=aspnetcore-2.1) yerelleştirme kültürü uygulamanızda nasıl sağlandığını özelleştirmenizi sağlar. `CustomRequestCultureProvider` Varsayılan sağlayıcıları gereksinimlerinizi karşılayıp kullanılır.

- İlk sağlayıcısında değil özel sağlayıcı düzgün çalışmıyor yaygın bir nedeni olduğundan `RequestCultureProviders` listesi. Bu sorunu çözmek için:

- Özel sağlayıcı konumu 0 eklemek `RequestCultureProviders` listesi aşağıdaki gibi:

```csharp
options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
```

- Kullanım `AddInitialRequestCultureProvider` özel sağlayıcı ilk sağlayıcısı olarak ayarlamak için genişletme yöntemi.

## <a name="root-namespace-issues"></a>Kök Namespace sorunları

Kök ad alanı bir derlemenin derleme adından farklı olduğunda, yerelleştirme, varsayılan olarak çalışmaz. Bu sorunu kullanımı önlemek için [RootNamespace](/dotnet/api/microsoft.extensions.localization.rootnamespaceattribute?view=aspnetcore-2.1), ayrıntılı olarak açıklanan [burada](xref:fundamentals/localization?view=aspnetcore-2.2#resource-file-naming)

## <a name="resources--build-action"></a>Kaynaklar ve derleme eylemi

Yerelleştirme için kaynak dosyaları kullanıyorsanız, bunlar uygun yapı eylemi olması önemlidir. Olmaları gerektiği **gömülü kaynak**, aksi halde `ResourceStringLocalizer` bu kaynakları bulmak mümkün değildir.
