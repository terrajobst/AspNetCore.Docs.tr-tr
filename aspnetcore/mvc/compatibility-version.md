---
title: ASP.NET Core MVC için uyumluluk sürümü
author: rick-anderson
description: ASP.NET Core başlangıç sınıfının Hizmetleri ve uygulamanın istek ardışık düzenini nasıl yapılandırdığını öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 9/25/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b29e2ee49aaf0f557f1acd0cf03e9e82d5ea0105
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667127"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a>ASP.NET Core MVC için uyumluluk sürümü

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> yöntemi, ASP.NET Core 3,0 uygulamaları için bir op değildir. Diğer bir deyişle, `SetCompatibilityVersion` herhangi bir <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion> değeri ile çağırmak uygulama üzerinde hiçbir etkiye sahip değildir.

* ASP.NET Core sonraki alt sürümü yeni bir `CompatibilityVersion` değeri verebilir.
* `Version_2_2` ile `Version_2_0` `CompatibilityVersion` değerler `[Obsolete(...)]`olarak işaretlenir.
* Bkz. [Antiforgery, CORS, Diagnostics, MVC ve yönlendirme IÇINDEKI API değişikliklerini bozma](https://github.com/aspnet/Announcements/issues/387). Bu liste, uyumluluk anahtarlarına yönelik son değişiklikleri içerir.

`SetCompatibilityVersion` ASP.NET Core 2. x uygulamalarıyla nasıl çalıştığını görmek için, [Bu makalenin ASP.NET Core 2,2 sürümünü](https://docs.microsoft.com/aspnet/core/mvc/compatibility-version?view=aspnetcore-2.2)seçin.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> yöntemi, bir ASP.NET Core 2. x uygulamasının, ASP.NET Core MVC 2,1 veya 2,2 ' de ortaya çıkan olası davranış değişikliklerinin kabul etmesine veya devre dışı olmasına izin verir. Bu olası davranışı ortadan kaldırma, MVC alt sisteminin davranışını ve **kodunuzun** çalışma zamanı tarafından nasıl çağrıldığını gösterir. ' De, en son davranışı ve ASP.NET Core uzun vadeli davranışını alırsınız.

Aşağıdaki kod uyumluluk modunu ASP.NET Core 2,2 olarak ayarlar:

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

Uygulamanızı en son sürümü (`CompatibilityVersion.Latest`) kullanarak test etmenizi öneririz. Çoğu uygulamanın en son sürümü kullanarak önemli davranış değişikliklerinin olmadığı tahmin ederiz.

`SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` çağıran uygulamalar, ASP.NET Core 2.1/2.2 MVC sürümlerinde tanıtılan büyük olasılıkla davranış değişikliklerinden korunur. Bu koruma:

* Tüm 2,1 ve sonraki değişikliklere uygulanmaz, MVC alt sisteminde çalışma zamanı davranış değişikliklerinin ASP.NET Core büyük olasılıkla bozmasına yöneliktir.
* ASP.NET Core 3,0 ' ye genişlemez.

`SetCompatibilityVersion` **çağırmayan** ASP.NET Core 2,1 ve 2,2 uygulamaları için varsayılan uyumluluk, 2,0 uyumluluğuna sahiptir. Diğer bir deyişle, `SetCompatibilityVersion` çağrılmayan `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`çağırma ile aynıdır.

Aşağıdaki kod, uyumluluk modunu aşağıdaki davranışlar dışında 2,2 ASP.NET Core olarak ayarlar:

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

Doğru uyumluluk anahtarlarını kullanarak, bölme davranışı ile ilgili olan uygulamalar için:

* En son sürümü kullanmanıza ve belirli bir bölme davranışı değişikliğini geri yüklemenize olanak tanır.
* , Uygulamanızı en son değişikliklerle çalışacak şekilde güncelleştirmeniz için size zaman kazandırır.

<xref:Microsoft.AspNetCore.Mvc.MvcOptions> belgelerinin ne değiştiği ve değişikliklerin çoğu kullanıcı için nasıl geliştirdiğinin iyi bir açıklaması vardır.

ASP.NET Core 3,0 ile, uyumluluk anahtarları tarafından desteklenen eski davranışlar kaldırılmıştır. Bunlar, neredeyse tüm kullanıcılar faydalanmasını olumlu değişiklikler olduğunu hissettik. Bu değişiklikleri 2,1 ve 2,2 ' de sunarak, çoğu uygulama yararlı olabilir, diğerleri ise güncelleştirilmeye zaman alabilir.
::: moniker-end
