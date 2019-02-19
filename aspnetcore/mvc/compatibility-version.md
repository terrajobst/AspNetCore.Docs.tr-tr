---
title: ASP.NET Core MVC için sürüm uyumluluğu
author: rick-anderson
description: ASP.NET core'da başlangıç sınıfı, hizmet ve uygulamaların istek işlem hattı nasıl yapılandırdığını keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2019
uid: mvc/compatibility-version
ms.openlocfilehash: 7c4189db435088e0803b35add82fa0eb9372e664
ms.sourcegitcommit: d75d8eb26c2cce19876c8d5b65ac8a4b21f625ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56410151"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>ASP.NET Core MVC için sürüm uyumluluğu

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> Yöntemi kabul etme veya potansiyel olarak davranışı sunulan içinde ASP.NET Core MVC 2.1 veya üzeri bozucu değişiklikler çevirme için bir uygulama sağlar. Potansiyel olarak yeni davranış değişiklikleri genel olarak, MVC alt sistemi davranışını nasıl ve ne kadar bunlar **kodunuzu** çalışma zamanı tarafından çağrılır. Seçim tarafından en son davranışı ve ASP.NET Core uzun vadeli davranışını alın.

Aşağıdaki kod, ASP.NET Core 2.2 için Uyumluluk modu ayarlar:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

En son sürümünü kullanarak uygulamanızı test öneririz (`CompatibilityVersion.Version_2_2`). Biz, çoğu uygulama en son sürümünü kullanarak davranışı değişiklikler olmaz beklenir.

Çağıran uygulamalar `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` potansiyel olarak ASP.NET Core 2.1 MVC ve sonraki 2.x sürümlerinde sunulan davranış değişiklikleri kesilmesini korunur. Bu koruma:

* Uygulanmaz 2.1 ve üzeri yapılan tüm değişiklikler, potansiyel olarak ASP.NET Core çalışma zamanı davranışı MVC alt sistemde değişiklikler için görünür duruma yöneliktir.
* Bir sonraki ana sürümüne genişletilmez.

ASP.NET Core 2.1 ve yapmak sonraki 2.x uygulamaları için varsayılan uyumluluk **değil** çağrı `SetCompatibilityVersion` 2.0 dönük uyumluluğa yöneliktir. Diğer bir deyişle, değil çağırma `SetCompatibilityVersion` arama aynı `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.

Aşağıdaki kod, ASP.NET Core 2.2 için Uyumluluk modu dışında aşağıdaki davranışları ayarlar:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Uygulamalar için uygun uyumluluk anahtarları kullanarak en son davranış değişiklikleri karşılaşırsınız:

* En son sürümünü kullanın ve belirli bozucu davranış değişiklikleri dışında iyileştirilmiş olanak sağlar.
* En son değişikliklerle birlikte çalışacak şekilde uygulamanızı güncelleştirmek için zaman verir.

[MvcOptions](https://github.com/aspnet/AspNetCore/blob/release/2.2/src/Mvc/Mvc.Core/src/MvcOptions.cs) sınıf kaynak yorumlar bulunan, değişiklikler ve çoğu kullanıcı için bir geliştirme değişiklikler neden iyi bir açıklama.

Bazı tarihte olacaktır bir [ASP.NET Core 3.0 sürümü](https://github.com/aspnet/Home/wiki/Roadmap). Uyumluluk anahtarları tarafından desteklenen eski davranışları 3.0 sürümünde kaldırılacak. Biz, neredeyse tüm kullanıcılar teknolojisinden yararlanan pozitif değişiklikler bunlar gönderebilirsiniz. Bu değişiklikleri şimdi Tanıtımı, çoğu uygulama artık yararlanabilir ve diğerlerinin uygulamalarını güncelleştirme zamanı gerekir.
