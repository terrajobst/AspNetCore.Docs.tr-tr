---
title: ASP.NET Core hataları işlemek
author: ardalis
description: ASP.NET Core uygulamaları hataların nasıl işleneceğini bulur.
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
uid: fundamentals/error-handling
ms.openlocfilehash: 2fe46ecc32d61a7fafb2ad6e2a35456476608251
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273715"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET Core hataları işlemek

Tarafından [Steve Smith](https://ardalis.com/) ve [zel Dykstra](https://github.com/tdykstra/)

Bu makalede, ASP.NET Core uygulamaları hataları işlemek için ortak appoaches yer almaktadır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Geliştirici özel durumu sayfası

Özel durumlar hakkında ayrıntılı bilgiler gösterilmektedir bir sayfasını görüntülemek için uygulamayı yapılandırmak için yükleme `Microsoft.AspNetCore.Diagnostics` NuGet paketini ve bir satırı ekleyin [başlangıç sınıfında yöntemini yapılandırma](xref:fundamentals/startup):

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

PUT `UseDeveloperExceptionPage` Ara yazılımların, istediğiniz gibi özel durumları yakalamak için önce `app.UseMvc`.

>[!WARNING]
> Geliştirici özel durum sayfasını etkinleştir **yalnızca uygulama geliştirme ortamında çalışırken**. Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık şekilde paylaşma istemezsiniz. [Ortamlarını yapılandırma hakkında daha fazla bilgi](xref:fundamentals/environments).

Geliştirici özel durum sayfasını görmek için ayarlanan ortam ile örnek uygulamayı çalıştırın `Development`ve ekleme `?throw=true` uygulamanın temel URL. Sayfa özel durumu ve istek hakkında bilgi içeren birkaç sekme içerir. İlk sekme yığın izlemesi içerir. 

![Yığın izleme](error-handling/_static/developer-exception-page.png)

Sonraki sekme sorgu dizesi parametreleri varsa gösterir.

![Sorgu dizesi parametreleri](error-handling/_static/developer-exception-page-query.png)

Bu istek tanımlama bilgilerine sahip oldu, ancak olsaydı, üzerinde görünecekleri **tanımlama bilgilerini** sekmesi. Son sekmede geçirilmiş üstbilgileri görebilirsiniz.

![Üstbilgileri](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Sayfa işleme özel bir durum yapılandırma

Uygulama çalışmadığında kullanmak için bir özel durum işleyici sayfasını yapılandırmak `Development` ortamı.

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

Bir Razor sayfalarının uygulamasında [dotnet yeni](/dotnet/core/tools/dotnet-new) Razor sayfalarının şablonu bir hata sayfası sağlar ve `ErrorModel` sayfasında modeli sınıfında *sayfaları* klasör.

Bir MVC uygulamasında HTTP yöntemi öznitelikleriyle hata işleyici eylem yöntemi gibi tasarlamanız yok `HttpGet`. Açık fiiller bazı istekleri yöntemi erişmesini engelleyin. Kimliği doğrulanmamış kullanıcıların hata görünümünün alabilir; böylece yöntemi anonim erişime izin verin.

Örneğin, aşağıdaki hata işleyici yöntemi tarafından sağlanan [dotnet yeni](/dotnet/core/tools/dotnet-new) MVC şablonu ve giriş denetleyicide görünür:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>Yapılandırma durumu kod sayfaları

Varsayılan olarak, bir uygulama zengin durum kod sayfası için HTTP durum kodları, gibi sağlamaz *404 Bulunamadı*. Kod sayfaları durumu sağlamak için bir satıra ekleyerek durum kodu sayfaları ara yazılımı yapılandırmanız `Startup.Configure` yöntemi:

```csharp
app.UseStatusCodePages();
```

Varsayılan olarak, durum kodu sayfaları ara yazılımı 404 gibi ortak durum kodları için basit, yalnızca metin işleyiciler ekler:

![404 sayfası](error-handling/_static/default-404-status-code.png)

Ara yazılım birkaç uzantı yöntemleri destekler. Lambda ifadesi bir yöntemi alır:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

Başka bir yöntem içerik türü ve biçim dizesini alır:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Ayrıca yönlendirmek ve genişletme yöntemleri yeniden çalıştırın. Yeniden yönlendirme yöntemi 302 durum kodunu istemciye gönderir:

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Yeniden çalıştırma yöntemi istemciye özgün durum kodunu döndüren ancak işleyicinin yeniden yönlendirme URL'si de yürütür:

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Bir Razor sayfalarının işleyici yöntemi veya MVC denetleyicisi belirli istekleri için durum kod sayfaları devre dışı bırakılabilir. Durum kod sayfaları devre dışı bırakmak için alma denemesi [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) gelen isteğin [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) koleksiyonu ve kullanılabilir durumdaysa özelliği devre dışı bırakın:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Kullanılıyorsa bir `UseStatusCodePages*` uygulama içinde uç noktalarına oluşturduğunuz bir MVC görünümü veya Razor sayfasını uç nokta için aşırı yükleme. Örneğin, [dotnet yeni](/dotnet/core/tools/dotnet-new) şablonu için bir Razor sayfalarının uygulaması aşağıdaki sayfasını ve sayfa model sınıfı üretir:

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Özel durum işleme kodu

Özel durum işleme sayfaları kodda istisnalar atabilirsiniz. Genellikle, yalnızca statik içeriği oluşması üretim hata sayfaları için iyi bir fikir olabilir.

Ayrıca, yanıt üstbilgileri gönderildikten sonra yanıtın durum kodu değişiklik yapamaz veya herhangi bir özel durum sayfaları veya işleyicileri çalıştırabilirsiniz dikkat edin. Yanıt doldurulmalıdır veya bağlantı durduruldu.

## <a name="server-exception-handling"></a>Sunucu özel durum işleme

Özel durum işleme mantığı, uygulamanızda yanı sıra [server](xref:fundamentals/servers/index) uygulamanızı barındırma bazı özel durum işleme gerçekleştirir. Üst bilgileri gönderilmeden önce sunucunun bir özel durum yakalar, sunucunun gönderir. bir *500 İç sunucu hatası* hiçbir gövdesi olan yanıt. Üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır. Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir. Sunucunun bir özel durum nedeniyle oluşan herhangi bir özel durum işlenmiş işleme. Herhangi bir özel hata sayfaları yapılandırılabilir veya özel durum işleme ara yazılımı veya filtreler bu davranışını etkilemez.

## <a name="startup-exception-handling"></a>Başlangıç özel durum işleme

Yalnızca barındırma katmanı uygulama başlatma sırasında gerçekleşmesi özel durumlar işleyebilir. Kullanarak [Web ana bilgisayarı](xref:fundamentals/host/web-host), yapabilecekleriniz [nasıl konak hatalarına yanıt olarak başlatma sırasında davranacağını yapılandırmak](xref:fundamentals/host/web-host#detailed-errors) ile `captureStartupErrors` ve `detailedErrors` anahtarları.

Ana bilgisayar adresini/bağlantı noktası sonra bağlama hatası oluşursa Barındırma yakalanan başlatma hatası için bir hata sayfası yalnızca gösterebilir. Hiçbir bağlama herhangi bir nedenle başarısız olursa, barındırma katman bir kritik özel durumu, dotnet işlem çökmesi günlüğe kaydeder ve uygulama çalışırken bir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel) sunucu.

Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:fundamentals/servers/aspnet-core-module) işlemi olamazsa başladı. Sorun giderme tavsiyesini takip [IIS üzerinde ASP.NET Core sorun giderme](xref:host-and-deploy/iis/troubleshoot) konu.

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC hata işleme

[MVC](xref:mvc/overview) uygulamalara sahip özel durum filtreleri yapılandırma ve model doğrulama gerçekleştirme gibi hataları işlemek için bazı ek seçenekleri.

### <a name="exception-filters"></a>Özel durum filtreleri

Özel durum filtreleri, genel olarak veya bir MVC uygulamasında her denetleyici veya eylem başına temelinde yapılandırılabilir. Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durum işleme ve aksi durumda adlı değil. Özel durum filtreleri hakkında daha fazla bilgi [filtreleri](xref:mvc/controllers/filters).

> [!TIP]
> Özel durum filtreleri içinde MVC Eylemler oluşan özel durumlarını yakalama için iyi, ancak bunlar hata ara yazılım işleme kadar esnek değildir. Ara yazılımı genel örneği için tercih ettiğiniz ve hata işleme yapmak için yalnızca ihtiyaç duyacağınız filtreleri kullanın *farklı* MVC eylemi seçildi tabanlı.

### <a name="handling-model-state-errors"></a>İşleme Model durumu hataları

[Model doğrulama](xref:mvc/models/validation) her denetleyici eylemi çağırma öncesi oluşur ve incelemek için eylem yönteminin sorumluluğu olan `ModelState.IsValid` ve uygun şekilde tepki.

Model doğrulama hataları, ilgilenmek için standart bir kural, bu durumda izlemeniz gereken bazı uygulamalar seçecektir bir [filtre](xref:mvc/controllers/filters) böyle bir ilke uygulamak için uygun bir yerdir olabilir. Geçersiz model durumlarıyla eylemlerinizi nasıl davranacağını test etmeniz gerekir. Daha fazla bilgi edinin [Test denetleyicisi mantığı](xref:mvc/controllers/testing).
