---
title: ASP.NET Core Razor sayfalarında için filtre yöntemleri
author: Rick-Anderson
description: ASP.NET Core Razor sayfalar için filtre yöntemleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: ff2f4b4c2556e31d0261bc2c2ff47a4a6c7a1335
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291599"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor sayfalarında için filtre yöntemleri

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor sayfa filtreleri [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) Razor kodu önce ve Razor sayfasını işleyici çalıştırıldıktan sonra çalıştırmak sayfaları olanak verir. Razor sayfa filtreleri benzer [ASP.NET Core MVC eylem filtrelerini](xref:mvc/controllers/filters#action-filters), tek tek sayfa işleyici yöntemlerine uygulanamaz dışında. 

Razor sayfa filtreleri:

* Kod bir işleyici yöntemi seçtikten sonra ancak model bağlama gerçekleşmeden önce çalıştırın.
* Model bağlama tamamlandıktan sonra işleyici yöntemi yürütülmeden önce kodu çalıştırın.
* İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.
* Bir sayfa veya genel olarak uygulanabilir.
* Belirli bir sayfaya işleyici yöntemlerine uygulanamaz.

Kod sayfası Oluşturucusu veya ara yazılımı kullanarak bir işleyici yöntemi çalışır, ancak yalnızca Razor sayfa filtreleri erişiminiz önce çalıştırılabilir [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Filtreleri bulunan bir [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) erişim sağlayan parametre türetilmiş `HttpContext`. Örneğin, [bir filtre özniteliğini uygulamak](#ifa) örnek yanıt oluşturucular veya ara yazılımı ile yapılamaz bir şey için bir başlık ekler.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Razor sayfa filtreleri genel olarak veya sayfa düzeyinde uygulanabilir aşağıdaki yöntemleri sağlar:

* Zaman uyumlu yöntemleri:

    * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : bir işleyici yöntemi seçildiyse, ancak önce model bağlama oluşur sonra çağrılır.
    * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : model bağlama tamamlandıktan sonra işleyici yöntemi yürütülmeden önce çağrılır.
    * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : işleyici yöntemi, önce eylem sonucu yürütüldükten sonra çağrılır.

* Zaman uyumsuz yöntemleri:

    * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : zaman uyumsuz olarak sonra adlı işleyici yöntemi seçilmedi, ancak önce model bağlama oluşur.
    * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : model bağlama tamamlandıktan sonra işleyici yöntemi çağrılmadan önce zaman uyumsuz olarak çağrılır.

> [!NOTE]
> Uygulama **ya da** zaman uyumlu veya zaman uyumsuz bir filtre arabiriminin sürümünü, her ikisine değil. Framework ilk filtreyi zaman uyumsuz arabirimini uygulayan ve Öyleyse, çağıran bakar. Aksi durumda, zaman uyumlu arabiriminin yöntemleri çağırır. Her iki arabirimde uygulanırsa, yalnızca zaman uyumsuz yöntemleri olan çağrılabilir. Geçersiz kılmaları sayfalarında aynı kuralın uygulanacağı, zaman uyumlu veya zaman uyumsuz sürümü geçersiz kılar, ikisini uygulayabilirsiniz.

## <a name="implement-razor-page-filters-globally"></a>Razor sayfa filtreleri genel uygulama

Aşağıdaki kod uygulayan `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

Önceki kod [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) gerekli değildir. Aşağıdaki örnekte, uygulama için izleme bilgilerini sağlamak için kullanılır.

Aşağıdaki kod etkinleştirir `SampleAsyncPageFilter` içinde `Startup` sınıfı:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Aşağıdaki kod tam gösterir `Startup` sınıfı:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Aşağıdaki kod çağrıları `AddFolderApplicationModelConvention` uygulamak için `SampleAsyncPageFilter` yalnızca sayfalarına */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Aşağıdaki kod zaman uyumlu uygular `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Aşağıdaki kod etkinleştirir `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"
## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Filtre yöntemi geçersiz kılarak uygulama Razor sayfa filtreleri

Aşağıdaki kod, zaman uyumlu Razor sayfa filtrelerini geçersiz kılan:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>
## <a name="implement-a-filter-attribute"></a>Bir filtre özniteliğini uygulayın

Yerleşik öznitelik tabanlı filtre [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtre sınıflandırma. Aşağıdaki filtre üstbilgi yanıta ekler:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Aşağıdaki kod geçerlidir `AddHeader` özniteliği:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Bkz: [varsayılan sırası geçersiz kılma](xref:mvc/controllers/filters#overriding-the-default-order) sırası geçersiz kılma hakkında yönergeler için.

Bkz: [iptali ve kestirmeler](xref:mvc/controllers/filters#cancellation-and-short-circuiting) yönergeler bir filtre filtre ardışık düzen kısa devre oluşturur. 

<a name="auth"></a>
## <a name="authorize-filter-attribute"></a>Filtre özniteliği yetkilendirmek

[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği uygulanabilir bir `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
