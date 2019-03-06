---
title: ASP.NET core'da hatalarını işleme
author: tdykstra
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345511"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET core'da hatalarını işleme

Tarafından [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), ve [Steve Smith](https://ardalis.com/)

Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Geliştirici özel durumu sayfası

Özel durumları hakkında ayrıntılı bilgi gösteren bir sayfasını görüntülemek için bir uygulamayı yapılandırmak için kullanın *Geliştirici özel durum sayfasında*. Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Bir satırı `Startup.Configure` yöntemi:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

Çağrı yapmak <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> önüne özel durumları yakalamak istediğiniz herhangi bir ara yazılım.

> [!WARNING]
> Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**. Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz. Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.

Geliştirici özel durumu sayfasını görmek için ortamı ayarlamak örnek uygulamayı çalıştırma `Development` ve ekleme `?throw=true` uygulamasının temel URL'si. Sayfasında, özel durum ve isteği hakkında aşağıdaki bilgileri içerir:

* Yığın izlemesi
* Dize parametreleri (varsa) sorgulama
* Tanımlama bilgileri (varsa)
* Üst bilgileri

## <a name="configure-a-custom-exception-handling-page"></a>Sayfa işleme özel bir özel durum yapılandırın

Uygulama geliştirme ortamında çalışmadığı aramanızı <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> özel durum işleme ara yazılım eklemek için genişletme yöntemi. Ara yazılım:

* Özel durumu yakalar.
* Özel durumları günlüğe kaydeder.
* Belirtilen denetleyici ve sayfa için alternatif bir işlem hattı istekte yeniden yürütür. İstek, yanıt başladıysa, yeniden çalıştırılır değildir.

Aşağıdaki örnekte, örnek uygulamadan <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> geliştirme olmayan ortamlarda özel durum işleme ara yazılım ekler. Bir hata sayfası veya denetleyicisinde genişletme yöntemi belirler `/Error` özel durum yakalandı ve günlüğe sonra yeniden yürütülen istekler için uç nokta:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

Bir hata sayfası Razor sayfaları uygulaması şablonunu sunar (*.cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) sayfalar klasöründe.

Aşağıdaki hata işleyicisi yöntemi bir MVC uygulamasında MVC uygulama şablonuna dahil edilir ve giriş denetleyicisine içinde görünür:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`. Açık fiilleri yöntemi bazı istekleri engellenir. Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.

## <a name="configure-status-code-pages"></a>Durum kod sayfaları yapılandırın

Varsayılan olarak, ASP.NET Core uygulaması durumu kod sayfası için HTTP durum kodları, gibi sağlamaz *404 - Bulunamadı*. Uygulama, durum kodu ve boş yanıt gövdesi döndürür. Kod sayfaları durumu sağlamak için durum kodu sayfa ara yazılımı kullanın.

Ara yazılım tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) kullanılabilir paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

Bir satırı `Startup.Configure` yöntemi:

```csharp
app.UseStatusCodePages();
```

Çağrı <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> önce ara (örneğin, statik dosya ara yazılımlarını ve MVC ara yazılım) işleme istek yöntemi.

Gibi yaygın durum kodları için salt metin işleyiciler varsayılan olarak, durum kodu sayfa ara yazılımı ekler *404 - Bulunamadı*:

```
Status Code: 404; Not Found
```

Ara yazılım davranışını özelleştirmenizi birkaç genişletme yöntemleri destekler.

Bir aşırı yüklemesini <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> özel hata işleme mantığı işlemek ve el ile yanıt yazmak için kullanabileceğiniz bir lambda ifadesini alır:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Bir aşırı yüklemesini <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> içerik türü ve yanıt metni özelleştirmek için kullanabileceğiniz bir içerik türü ve biçim dizesini alır:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>Yeniden yönlendirme uzantısı yöntemleri ve yeniden yürütme

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Gönderen bir *302 bulundu -* istemciye durum kodu.
* İstemci URL'si şablonda verilen konuma yönlendirir.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> yaygın olarak kullanıldığında uygulama:

* Genellikle, farklı bir uygulama hatası burada işler durumlarda farklı bir uç noktası için istemci yönlendirmelidir. Web apps için yeniden yönlendirilen uç nokta istemcinin tarayıcınızın adres çubuğuna yansıtır.
* Olmamalıdır korumak ve özgün ilk yönlendirme yanıt durum koduyla döndürür.

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Özgün durum kodunu istemciye döndürür.
* Yanıt gövdesi, alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek oluşturur.

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> Uygulama sırasında yaygın olarak kullanılır:

* İstek farklı bir uç noktasına yönlendirme olmadan işlem. Web apps için başlangıçta istenen uç noktası istemcinin tarayıcınızın adres çubuğuna yansıtır.
* Korumak ve özgün durum koduyla bir yanıt döndürür.

Şablonları, bir yer tutucu içerebilir (`{0}`) durum kodu. Şablonu bir eğik çizgi ile başlamalıdır (`/`). Bir yer tutucu kullanırken, uç noktayı (sayfa veya denetleyicisi) yol kesimi işleyebilir onaylayın. Örneğin, hatalar için bir Razor sayfası ile isteğe bağlı yol kesimi değerini kabul etmelidir `@page` yönergesi:

```cshtml
@page "{code?}"
```

Durum kod sayfaları için Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli isteklere devre dışı bırakılabilir. Durum kod sayfaları devre dışı bırakmak için alma denemesi <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> gelen isteğin [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) toplama ve varsa bu özelliği devre dışı bırak:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Kullanılacak bir <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> noktaları bir uç noktaya uygulama içinde oluşturduğunuz bir MVC görünümü ya da bir Razor sayfası uç nokta için aşırı yükleme. Razor sayfaları uygulama şablonu, örneğin, aşağıdaki sayfasını ve sayfa modeli sınıfı üretir:

*Error.cshtml*:

::: moniker range=">= aspnetcore-2.2"

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
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*:

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

::: moniker-end

## <a name="exception-handling-code"></a>Özel durum işleme kodu

Özel durum işleme sayfaları kodda özel durumlar. Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.

Ayrıca, dikkat edin, yanıt üst bilgileri gönderdikten sonra:

* Uygulama, yanıtın durum kodu değiştiremezsiniz.
* Herhangi bir özel durum sayfaları veya işleyicileri çalıştıramazsınız. Yanıt tamamlanması gereken veya bağlantı kesildi.

## <a name="server-exception-handling"></a>Sunucu özel durum işleme

Özel durum işleme mantığı, uygulamanıza ek olarak [sunucusu uygulaması](xref:fundamentals/servers/index) bazı özel durumları işleyebilir. Yanıt üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 - İç sunucu hatası* yanıt gövdesi olmadan yanıt. Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır. Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir. Oluşan özel sunucu özel durum tarafından işlenir işleme. Herhangi bir özel hata sayfaları yapılandırılmış veya özel durum işleme ara yazılım veya filtreleri bu davranışı etkilemez.

## <a name="startup-exception-handling"></a>Başlangıç özel durum işleme

Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir. Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalara yanıt başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.

Ana bilgisayar adresi/bağlantı noktası sonra bağlama bir hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma yalnızca gösterebilirsiniz. Bağlama için herhangi bir nedenle başarısız olursa:

* Barındırma katman kritik bir özel durumu günlüğe kaydeder.
* Dotnet işlem kilitleniyor.
* Uygulama çalışırken, hiçbir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucusu.

Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemi başlatılamazsa . Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot>. Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>ASP.NET Core MVC hata işleme

[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekler.

### <a name="exception-filters"></a>Özel durum filtreleri

Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir. Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle. Bu filtreler, aksi takdirde çağrılır değil. Daha fazla bilgi için bkz: <xref:mvc/controllers/filters>.

> [!TIP]
> Özel durum filtreleri MVC Eylemler içinde oluşan özel durumları yakalama için kullanışlıdır ancak bunlar ara yazılım işleme hata olarak kadar esnek değildir. Ara yazılım kullanılmasını öneririz. Hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtrelerini kullanma *farklı* göre MVC eylemi seçilir.

### <a name="handle-model-state-errors"></a>Model durumu hataları işleme

[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesinde gerçekleşir ve incelemek için eylem yönteminin sorumluluğu olan [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) ve uygun şekilde tepki verin.

Başa çıkmak için standart bir kural izlemek bazı uygulamalar seçin [model doğrulama](xref:mvc/models/validation) durumda hataları bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yere olabilir. Eylemlerinizi ile geçersiz model durumlarının nasıl davranacağını test etmeniz gerekir. Daha fazla bilgi için bkz. <xref:mvc/controllers/testing>.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
