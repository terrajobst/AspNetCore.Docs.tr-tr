---
title: ASP.NET Core Razor sayfaları için filtre yöntemleri
author: Rick-Anderson
description: ASP.NET Core Razor sayfaları için filtre yöntemleri oluşturmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/05/2018
uid: razor-pages/filter
ms.openlocfilehash: 2480e67d251de8f8aecb6c484999c90d0220dd19
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64900893"
---
# <a name="filter-methods-for-razor-pages-in-aspnet-core"></a>ASP.NET Core Razor sayfaları için filtre yöntemleri

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Razor sayfası filtreler [IPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter?view=aspnetcore-2.0) ve [IAsyncPageFilter](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter?view=aspnetcore-2.0) bir Razor sayfası işleyici çalıştırılmadan önce ve sonra kodu çalıştırmak Razor sayfaları olanak verir. Razor sayfa filtreleri benzer [ASP.NET Core MVC eylem filtrelerini](xref:mvc/controllers/filters#action-filters)dışında tekil işleyici yöntemleri için uygulanamaz. 

Razor sayfası filtreler:

* Kodu bir işleyici yöntemi seçtikten sonra ancak model bağlama gerçekleşmeden önce çalıştırın.
* Model bağlama işlemi tamamlandıktan sonra işleyicisi yöntemi yürütülmeden önce kodu çalıştırın.
* İşleyici yöntemi yürütüldükten sonra kodu çalıştırın.
* Bir sayfadaki veya genel olarak uygulanabilir.
* Belirli bir sayfaya işleyici yöntemleri için uygulanamaz.

Kod sayfası Oluşturucusu veya ara yazılımlar kullanarak bir işleyici yöntemini yürütür, ancak Razor sayfası filtreler yalnızca erişimleri önce çalıştırılabilir [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_HttpContext). Filtreye sahip bir [FilterContext](/dotnet/api/microsoft.aspnetcore.mvc.filters.filtercontext?view=aspnetcore-2.0) erişim sağlayan bir parametre türetilmiş `HttpContext`. Örneğin, [uygulayan bir filtre özniteliğini](#ifa) örnek yanıt oluşturucular veya ara yazılımlar ile yapılamaz bir şey için bir başlık ekler.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/filter/sample/PageFilter) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Razor sayfa filtreleri, genel olarak veya sayfa düzeyinde uygulanabilir aşağıdaki yöntemleri sağlar:

* Zaman uyumlu metotları:

  * [OnPageHandlerSelected](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerselected?view=aspnetcore-2.0) : İşleyicisi yöntemi seçildi, ancak önce model bağlama gerçekleşir sonra çağrılır.
  * [OnPageHandlerExecuting](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuting?view=aspnetcore-2.0) : Model bağlama işlemi tamamlandıktan sonra işleyicisi yöntemi yürütülmeden önce çağrılır.
  * [OnPageHandlerExecuted](/dotnet/api/microsoft.aspnetcore.mvc.filters.ipagefilter.onpagehandlerexecuted?view=aspnetcore-2.0) : İşleyicisi yöntemi, önce eylem sonucu yürütüldükten sonra çağrılır.

* Zaman uyumsuz yöntemler:

  * [OnPageHandlerSelectionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerselectionasync?view=aspnetcore-2.0) : Zaman uyumsuz olarak işleyicisi yöntemi seçtikten sonra ancak model bağlama gerçekleşmeden önce çağrılır.
  * [OnPageHandlerExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncpagefilter.onpagehandlerexecutionasync?view=aspnetcore-2.0) : Zaman uyumsuz model bağlama işlemi tamamlandıktan sonra işleyicisi yöntemi çağırılmadan önce çağrılır.

> [!NOTE]
> Uygulama **ya da** zaman uyumlu veya zaman uyumsuz sürümü filtre arabirimi, her ikisini birden değil. Framework ilk filtre zaman uyumsuz arabirimini uygulayan ve çağrı yaptığı bu durumda olup olmadığını denetler. Aksi durumda, zaman uyumlu arabirim yöntemleri çağırır. Her iki arabirimde uygulanırsa, yalnızca zaman uyumsuz yöntemler olan çağrılabilir. Geçersiz kılmalar sayfalarında aynı kuralın uygulanacağı, zaman uyumlu veya zaman uyumsuz sürümü geçersiz kılma, ikisini birden uygular.

## <a name="implement-razor-page-filters-globally"></a>Genel olarak Filtreleri Uygula Razor sayfası

Aşağıdaki kod uygulayan `IAsyncPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SampleAsyncPageFilter.cs?name=snippet1)]

Önceki kodda, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger?view=aspnetcore-2.0) gerekli değildir. Aşağıdaki örnekte, uygulama için izleme bilgisi sağlamak üzere kullanılır.

Aşağıdaki kod etkinleştirir `SampleAsyncPageFilter` içinde `Startup` sınıfı:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet2&highlight=11)]

Aşağıdaki kod tam gösterir `Startup` sınıfı:

[!code-csharp[Main](filter/sample/PageFilter/Startup.cs?name=snippet1)]

Aşağıdaki kod çağrıları `AddFolderApplicationModelConvention` uygulanacak `SampleAsyncPageFilter` yalnızca sayfalarına */subFolder*:

[!code-csharp[Main](filter/sample/PageFilter/Startup2.cs?name=snippet2)]

Aşağıdaki kod, zaman uyumlu uygulayan `IPageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/Filters/SamplePageFilter.cs?name=snippet1)]

Aşağıdaki kod etkinleştirir `SamplePageFilter`:

[!code-csharp[Main](filter/sample/PageFilter/StartupSync.cs?name=snippet2&highlight=11)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implement-razor-page-filters-by-overriding-filter-methods"></a>Filtre yöntemi geçersiz kılarak Filtreleri Uygula Razor sayfası

Aşağıdaki kod, zaman uyumlu bir Razor sayfası filtrelerini geçersiz kılan:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Index.cshtml.cs)]

::: moniker-end

<a name="ifa"></a>

## <a name="implement-a-filter-attribute"></a>Bir filtre özniteliğini uygulayın

Yerleşik öznitelik tabanlı filtre [OnResultExecutionAsync](/dotnet/api/microsoft.aspnetcore.mvc.filters.iasyncresultfilter.onresultexecutionasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Filters_IAsyncResultFilter_OnResultExecutionAsync_Microsoft_AspNetCore_Mvc_Filters_ResultExecutingContext_Microsoft_AspNetCore_Mvc_Filters_ResultExecutionDelegate_) filtre sınıflandırma. Aşağıdaki filtre yanıt olarak bir başlık ekler:

[!code-csharp[Main](filter/sample/PageFilter/Filters/AddHeaderAttribute.cs)]

Aşağıdaki kod geçerlidir `AddHeader` özniteliği:

[!code-csharp[Main](filter/sample/PageFilter/Pages/Contact.cshtml.cs?name=snippet1)]

Bkz: [varsayılan sırası geçersiz kılma](xref:mvc/controllers/filters#overriding-the-default-order) sırası geçersiz kılma hakkında yönergeler için.

Bkz: [iptal ve kestirmeler](xref:mvc/controllers/filters#cancellation-and-short-circuiting) filtre ardışık düzen bir filtre tarafından iki ilişkin yönergeler için. 

<a name="auth"></a>

## <a name="authorize-filter-attribute"></a>Filtre özniteliği Yetkilendir

[Authorize](/dotnet/api/microsoft.aspnetcore.authorization.authorizeattribute?view=aspnetcore-2.0) özniteliği uygulanabilir bir `PageModel`:

[!code-csharp[Main](filter/sample/PageFilter/Pages/ModelWithAuthFilter.cshtml.cs?highlight=7)]
