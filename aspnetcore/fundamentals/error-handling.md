---
title: ASP.NET core'da hatalarını işleme
author: ardalis
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: 6aded9525a0abd31dec8441c7fba60d8845c7d93
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938247"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET core'da hatalarını işleme

Tarafından [Steve Smith](https://ardalis.com/) ve [Tom Dykstra](https://github.com/tdykstra/)

Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Geliştirici özel durumu sayfası

::: moniker range=">= aspnetcore-2.1"

Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*. Tarafından kullanıma sunulan sayfayı [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Bir satırı `Startup.Configure` yöntemi:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*. Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage). Bir satırı `Startup.Configure` yöntemi:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*. Sayfa için bir paket başvurusu ekleme tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) proje dosyasındaki paket. Bir satırı `Startup.Configure` yöntemi:

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Çağrı yapmak [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önüne `app.UseMvc`.

>[!WARNING]
> Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**. Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz. [Ortamları yapılandırma hakkında daha fazla bilgi edinin](xref:fundamentals/environments).

Geliştirici özel durumu sayfasını görmek için ortamı ayarlamak örnek uygulamayı çalıştırma `Development`ve ekleme `?throw=true` uygulamasının temel URL'si. Sayfanın birkaç sekme özel durum ve isteği hakkında bilgi içerir. Bir yığın izlemesi ilk sekme içerir:

![Yığın izlemesi](error-handling/_static/developer-exception-page.png)

Sonraki sekme varsa sorgu dizesi parametreleri gösterir:

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

İstek tanımlama bilgisi varsa, göründükleri **tanımlama bilgilerini** sekmesi. Üst son sekmede görülür:

![Üst bilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Sayfa işleme bir özel durum yapılandırma

Uygulama çalışmadığında kullanmak için bir özel durum işleyicisi sayfası yapılandırma `Development` ortam:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Bir Razor sayfaları uygulamasında [yeni dotnet](/dotnet/core/tools/dotnet-new) Razor sayfaları şablonu, bir hata sayfası sağlar ve `ErrorModel` sayfa modeli sınıfında *sayfaları* klasör.

Bir MVC uygulamasında HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`. Açık fiilleri yöntemi bazı istekleri engellenir. Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.

Örneğin, aşağıdaki hata işleyicisi yöntemi tarafından sağlanan [yeni dotnet](/dotnet/core/tools/dotnet-new) MVC şablonu ve giriş denetleyicisine içinde görünür:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Yapılandırma durumu kod sayfaları

Varsayılan olarak, bir uygulama durumu zengin kod sayfası için HTTP durum kodları, gibi sağlamaz *404 Bulunamadı*. Kod sayfaları durumu sağlamak için bir satıra ekleyerek durumu kod sayfaları ara yazılım yapılandırma `Startup.Configure` yöntemi:

```csharp
app.UseStatusCodePages();
```

Varsayılan olarak, yalnızca metin işleyicileri 404 gibi yaygın durum kodları için durum kod sayfaları ara yazılım ekler:

![404 sayfa](error-handling/_static/default-404-status-code.png)

Ara yazılım birkaç uzantı yöntemleri destekler. Bir yöntem, lambda ifadesi alır:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Başka bir yöntemi, bir içerik türü ve biçim dizesini alır:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Var. Ayrıca yeniden yönlendirme ve genişletme yöntemlerini yeniden çalıştırın. Yeniden yönlendirme yöntemini gönderen bir *302 bulundu* istemciye durum kodu:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Yeniden çalıştırma yöntemi özgün durum kodunu istemciye döndürür ancak işleyicinin yeniden yönlendirme URL'sini de yürütür:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Durum kod sayfaları için Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli isteklere devre dışı bırakılabilir. Durum kod sayfaları devre dışı bırakmak için alma denemesi [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) gelen isteğin [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) toplama ve varsa bu özelliği devre dışı bırak:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Kullanılıyorsa bir `UseStatusCodePages*` noktaları bir uç noktaya uygulama içinde oluşturduğunuz bir MVC görünümü ya da bir Razor sayfası uç nokta için aşırı yükleme. Örneğin, [yeni dotnet](/dotnet/core/tools/dotnet-new) aşağıdaki sayfasını ve sayfa modeli sınıfı için bir Razor sayfaları uygulaması şablonu üretir:

*Error.cshtml*:

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Özel durum işleme kodu

Özel durum işleme sayfaları kodda özel durumlar. Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.

Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin. Yanıt tamamlanması gereken veya bağlantı kesildi.

## <a name="server-exception-handling"></a>Sunucu özel durum işleme

Özel durum işleme mantığı, uygulamanıza ek olarak [sunucu](xref:fundamentals/servers/index) uygulamanızı barındıran bazı özel durum işleme gerçekleştirir. Üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 İç sunucu hatası* hiçbir gövdesi olan yanıt. Üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır. Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir. Oluşan özel sunucu özel durum tarafından işlenir işleme. Herhangi bir özel hata sayfaları yapılandırılmış veya özel durum işleme ara yazılım veya filtreleri bu davranışı etkilemez.

## <a name="startup-exception-handling"></a>Başlangıç özel durum işleme

Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir. Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalara yanıt başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.

Ana bilgisayar adresi/bağlantı noktası sonra bağlama bir hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma yalnızca gösterebilirsiniz. Bağlama için herhangi bir nedenle başarısız olursa, barındırma katman kritik özel durum, dotnet işlem çökmesi günlüğe kaydeder ve uygulamayı çalıştıran herhangi bir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.

Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlem yoksa başlatıldı. IIS ile barındırırken başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/iis/troubleshoot>. Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC hata işleme

[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekler.

### <a name="exception-filters"></a>Özel durum filtreleri

Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir. Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle. Bu filtreler, aksi takdirde çağrılır değil. Daha fazla bilgi için bkz. [filtreleri](xref:mvc/controllers/filters).

> [!TIP]
> Özel durum filtreleri, MVC Eylemler içinde oluşan özel durumları yakalama için uygundur, ancak bunlar ara yazılım işleme hata olarak kadar esnek değildir. Genel olarak ara yazılım kullanımını tercih ve hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtrelerini kullanma *farklı* göre MVC eylemi seçilir.

### <a name="handling-model-state-errors"></a>Model durumu hataları işleme

[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki verin.

Model doğrulama hataları, başa çıkmak için standart bir kural, bu durumda izlemek bazı uygulamalar seçin bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yere olabilir. Eylemlerinizi ile geçersiz model durumlarının nasıl davranacağını test etmeniz gerekir. Daha fazla bilgi [Test denetleyicisi mantığı](xref:mvc/controllers/testing).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
