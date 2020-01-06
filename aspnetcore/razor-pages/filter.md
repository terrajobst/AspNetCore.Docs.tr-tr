---
title: ASP.NET Core Razor Pages için filtre yöntemleri
author: Rick-Anderson
description: ASP.NET Core Razor Pages için filtre yöntemleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 12/28/2019
uid: razor-pages/filter
ms.openlocfilehash: 02771219454556b236080c2668243f788693b2c1
ms.sourcegitcommit: 077b45eceae044475f04c1d7ef2d153d7c0515a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/29/2019
ms.locfileid: "75542712"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor Pages için filtre yöntemleri

::: moniker range=">= aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor sayfa filtreleri [ıpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [ıasyncpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor Pages, bir Razor sayfa işleyicisi çalıştırılmadan önce ve sonra kodu çalıştırmasına izin verir. Razor sayfası filtreleri, tek sayfa işleyicisi yöntemlerine uygulanamadığından, [ASP.NET Core MVC eylem filtrelerine](xref:mvc/controllers/filters#action-filters)benzerdir.

Razor sayfası filtreleri:

* Bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce kodu çalıştırın.
* Model bağlama işlemi tamamlandıktan sonra işleyici metodu yürütülmeden önce kodu çalıştırın.
* İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.
* , Bir sayfada veya genel olarak uygulanabilir.
* Belirli sayfa işleyici yöntemlerine uygulanamaz.
* , [Bağımlılık ekleme](xref:fundamentals/dependency-injection) (dı) tarafından doldurulmuş Oluşturucu bağımlılıkları olabilir. Daha fazla bilgi için bkz. [Servicefilterattribute](/aspnet/core/mvc/controllers/filters#servicefilterattribute) ve [typefilterattribute](/aspnet/core/mvc/controllers/filters#typefilterattribute).

Bir işleyici yöntemi sayfa Oluşturucusu veya ara yazılım kullanılarak yürütülmeden önce kod çalıştırılabilir, ancak yalnızca Razor sayfası filtrelerinin <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext>erişimi vardır. Filtreler, `HttpContext`erişim sağlayan <xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext> türetilmiş bir parametreye sahiptir. Örneğin, [bir filtre uygula özniteliği](#ifa) örneği yanıta, oluşturucular veya ara yazılım ile yapılamadığını belirten bir üst bilgi ekler.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/3.1sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Razor sayfası filtreleri, genel olarak veya sayfa düzeyinde uygulanabilecek aşağıdaki yöntemleri sağlar:

* Zaman uyumlu Yöntemler:

  * [Onpagehandlerselected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce çağırılır.
  * [Onpagehandlerexecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Işleyici Yöntemi yürütülmeden önce çağırılır, model bağlama işlemi tamamlandıktan sonra.
  * [Onpagehandleryürütüldü](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : işleyici yöntemi yürütüldükten sonra, eylem sonucundan önce çağırılır.

* Zaman uyumsuz yöntemler:

  * [Onpagehandlerselectionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Handler yöntemi seçildikten sonra zaman uyumsuz olarak çağırılır, ancak model bağlama gerçekleşmeden önce.
  * [Onpagehandlerexecutionasync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Handler yöntemi çağrılmadan önce zaman uyumsuz olarak çağrıldı, model bağlama işlemi tamamlandıktan sonra.

Her ikisini de **değil** , bir filtre arabiriminin zaman uyumlu veya zaman uyumsuz **sürümünü uygulayın.** Çerçeve öncelikle filtrenin zaman uyumsuz arabirimi uygulayıp uygulamadığını denetler ve bu durumda bunu çağırır. Aksi takdirde, zaman uyumlu arabirimin Yöntem (ler) i çağırır. Her iki arabirim de uygulanmışsa yalnızca zaman uyumsuz yöntemler çağrılır. Aynı kural sayfalardaki geçersiz kılmalara uygulanır, her ikisine de değil, geçersiz kılmanın zaman uyumlu veya zaman uyumsuz sürümünü uygular.

## <a name="implement-razor-page-filters-globally"></a>Razor sayfası filtrelerini küresel olarak uygulama

Aşağıdaki kod `IAsyncPageFilter`uygular:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

Yukarıdaki kodda `ProcessUserAgent.Write`, Kullanıcı aracı dizesiyle birlikte çalışarak Kullanıcı tarafından sağlanan koddur.

Aşağıdaki kod `Startup` sınıfındaki `SampleAsyncPageFilter` sunar:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup.cs?name=snippet2)]

Aşağıdaki kod, `SampleAsyncPageFilter` yalnızca */filmlerde*sayfalara uygulamak için <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection.AddFolderApplicationModelConvention*> çağırır:

[!code-csharp[Main](filter/3.1sample/PageFilter/Startup2.cs?name=snippet2)]

Aşağıdaki kod, zaman uyumlu `IPageFilter`uygular:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Aşağıdaki kod `SamplePageFilter`etkinleştirilir:

[!code-csharp[Main](filter/3.1sample/PageFilter/StartupSync.cs?name=snippet2)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama

Aşağıdaki kod, zaman uyumsuz Razor sayfası filtrelerini geçersiz kılar:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Index.cshtml.cs?name=snippet)]

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Filtre özniteliği uygulama

Yerleşik öznitelik tabanlı filtre <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncResultFilter.OnResultExecutionAsync*> filtresi, alt sınıflı olabilir. Aşağıdaki filtre yanıta bir üst bilgi ekler:

[!code-csharp[Main](filter/3.1sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Aşağıdaki kod `AddHeader` özniteliğini uygular:

[!code-csharp[Main](filter/3.1sample/PageFilter/Pages/Movies/Test.cshtml.cs)]

Üst bilgileri incelemek için tarayıcı geliştirici araçları gibi bir araç kullanın. **Yanıt üst bilgileri**altında `author: Rick` görüntülenir.

Sıralamayı geçersiz kılma yönergeleri için bkz. [varsayılan sırayı geçersiz kılma](xref:mvc/controllers/filters#overriding-the-default-order) .

Filtre işlem hattının bir filtreden kısa devre dışı olması için bkz. [iptal ve kısa](xref:mvc/controllers/filters#cancellation-and-short-circuiting) devre.

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Yetkilendir filtre özniteliği

[Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği bir `PageModel`uygulanabilir:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor sayfa filtreleri [ıpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [ıasyncpagefilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor Pages, bir Razor sayfa işleyicisi çalıştırılmadan önce ve sonra kodu çalıştırmasına izin verir. Razor sayfası filtreleri, tek sayfa işleyicisi yöntemlerine uygulanamadığından, [ASP.NET Core MVC eylem filtrelerine](xref:mvc/controllers/filters#action-filters)benzerdir.

Razor sayfası filtreleri:

* Bir işleyici yöntemi seçildikten sonra, ancak model bağlama gerçekleşmeden önce kodu çalıştırın.
* Model bağlama işlemi tamamlandıktan sonra işleyici metodu yürütülmeden önce kodu çalıştırın.
* İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.
* , Bir sayfada veya genel olarak uygulanabilir.
* Belirli sayfa işleyici yöntemlerine uygulanamaz.

Bir işleyici yöntemi sayfa Oluşturucusu veya ara yazılım kullanılarak yürütülmeden önce kod çalıştırılabilir, ancak yalnızca Razor sayfası filtrelerinin [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext)'e erişimi vardır. Filtrelerin `HttpContext`erişim sağlayan bir [Filtercontext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) türetilmiş parametresi vardır. Örneğin, [bir filtre uygula özniteliği](#ifa) örneği yanıta, oluşturucular veya ara yazılım ile yapılamadığını belirten bir üst bilgi ekler.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

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

Aşağıdaki kod `IAsyncPageFilter`uygular:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

Yukarıdaki kodda, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) gerekli değildir. Uygulama için izleme bilgilerini sağlamak üzere örnekte kullanılır.

Aşağıdaki kod `Startup` sınıfındaki `SampleAsyncPageFilter` sunar:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Aşağıdaki kod, tüm `Startup` sınıfını gösterir:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Aşağıdaki kod, `SampleAsyncPageFilter` yalnızca */alt klasöründeki*sayfalara uygulamak için `AddFolderApplicationModelConvention` çağırır:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Aşağıdaki kod, zaman uyumlu `IPageFilter`uygular:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Aşağıdaki kod `SamplePageFilter`etkinleştirilir:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Filtre yöntemlerini geçersiz kılarak Razor sayfası filtrelerini uygulama

Aşağıdaki kod, zaman uyumlu Razor sayfası filtrelerini geçersiz kılar:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

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

[Yetkilendir](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği bir `PageModel`uygulanabilir:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]

::: moniker-end
