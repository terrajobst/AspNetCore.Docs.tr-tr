---
title: ASP.NET Core Razor Pages için filtre yöntemleri
author: Rick-Anderson
description: ASP.NET Core Razor Pages için filtre yöntemleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 1da46c61617a01698e3c4b1fe6bf9825db6643fd
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589946"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor Pages için filtre yöntemleri

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

Razor sayfa filtreleri [ıpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [ıasyncpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor Pages, bir Razor sayfa işleyicisi çalıştırılmadan önce ve sonra kodu çalıştırmasına izin verir. Razor sayfası filtreleri, tek sayfa işleyicisi yöntemlerine uygulanamadığından, [ASP.NET Core MVC eylem filtrelerine](xref:mvc/controllers/filters#action-filters)benzerdir. 

Razor sayfası filtreleri:

* Bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce kodu çalıştırın.
* Model bağlama işlemi tamamlandıktan sonra işleyici metodu yürütülmeden önce kodu çalıştırın.
* İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.
* , Bir sayfada veya genel olarak uygulanabilir.
* Belirli sayfa işleyici yöntemlerine uygulanamaz.

Bir işleyici yöntemi sayfa Oluşturucusu veya ara yazılım kullanılarak yürütülmeden önce kod çalıştırılabilir, ancak yalnızca Razor sayfası filtrelerinin [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext)'e erişimi vardır. Filtrelerin `HttpContext` erişim sağlayan bir [Filtercontext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) türetilmiş parametresi vardır. Örneğin, [bir filtre uygula özniteliği](#ifa) örneği yanıta, oluşturucular veya ara yazılım ile yapılamadığını belirten bir üst bilgi ekler.

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

Razor sayfası filtreleri, genel olarak veya sayfa düzeyinde uygulanabilecek aşağıdaki yöntemleri sağlar:

* Zaman uyumlu Yöntemler:

  * [Onpagehandlerselected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce çağırılır.
  * [Onpagehandlerexecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Işleyici Yöntemi yürütülmeden önce çağırılır, model bağlama işlemi tamamlandıktan sonra.
  * [Onpagehandleryürütüldü](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : işleyici yöntemi yürütüldükten sonra, eylem sonucundan önce çağırılır.

* Zaman uyumsuz yöntemler:

  * [Onpagehandlerselectionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Handler yöntemi seçildikten sonra zaman uyumsuz olarak çağırılır, ancak model bağlama gerçekleşmeden önce.
  * [Onpagehandlerexecutionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Handler yöntemi çağrılmadan önce zaman uyumsuz olarak çağrıldı, model bağlama işlemi tamamlandıktan sonra.

> [!NOTE]
> Her ikisini de değil, bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.** Çerçeve öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır. Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır. Her iki arabirim de uygulanmışsa yalnızca zaman uyumsuz yöntemler çağrılır. Aynı kural sayfalardaki geçersiz kılmalara uygulanır, her ikisine de değil, geçersiz kılmanın zaman uyumlu veya zaman uyumsuz sürümünü uygular.

## <a name="implement-razor-page-filters-globally"></a>Razor sayfası filtrelerini küresel olarak uygulama

Aşağıdaki kod `IAsyncPageFilter` uygular:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

Yukarıdaki kodda, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) gerekli değildir. Uygulama için izleme bilgilerini sağlamak üzere örnekte kullanılır.

Aşağıdaki kod `Startup` sınıfındaki `SampleAsyncPageFilter` sunar:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Aşağıdaki kod, tüm `Startup` sınıfını gösterir:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Aşağıdaki kod, `SampleAsyncPageFilter` yalnızca */alt klasöründeki*sayfalara uygulamak için `AddFolderApplicationModelConvention` çağırır:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Aşağıdaki kod, zaman uyumlu `IPageFilter` uygular:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Aşağıdaki kod `SamplePageFilter` etkinleştirilir:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama

Aşağıdaki kod, zaman uyumlu Razor sayfası filtrelerini geçersiz kılar:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Filtre özniteliği uygulama

Yerleşik öznitelik tabanlı filtre [Onresultexecutionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtresi, alt sınıflı olabilir. Aşağıdaki filtre yanıta bir üst bilgi ekler:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Aşağıdaki kod `AddHeader` özniteliğini uygular:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Sıralamayı geçersiz kılma yönergeleri için bkz. [varsayılan sırayı geçersiz kılma](xref:mvc/controllers/filters#overriding-the-default-order) .

Filtre işlem hattının bir filtreden kısa devre dışı olması için bkz. [iptal ve kısa](xref:mvc/controllers/filters#cancellation-and-short-circuiting) devre. 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Yetkilendir filtre özniteliği

[Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği bir `PageModel` uygulanabilir:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
