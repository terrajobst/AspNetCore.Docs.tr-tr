---
title: ASP.NET core'da hatalarını işleme
author: tdykstra
description: ASP.NET Core uygulamaları hataları işlemek nasıl keşfedin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/07/2019
uid: fundamentals/error-handling
ms.openlocfilehash: cbb9462a3c6010e074dc391aa128ac2cbb901456
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59705584"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET core'da hatalarını işleme

Tarafından [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), ve [Steve Smith](https://ardalis.com/)

Bu makalede, ASP.NET Core uygulamalarında hata işleme için ortak bir yaklaşım ele alınmaktadır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples). ([Nasıl indirileceğini](xref:index#how-to-download-a-sample).) Makale önişlemci yönergeleri ayarlama hakkında yönergeler içerir (`#if`, `#endif`, `#define`) farklı senaryoları etkinleştirmek için örnek uygulama.

## <a name="developer-exception-page"></a>Geliştirici özel durumu sayfası

*Geliştirici özel durum sayfasında* istek özel durumları hakkında ayrıntılı bilgiler görüntüler. Sayfa tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app). Kodu `Startup.Configure` uygulama geliştirmesinde çalışırken sayfa etkinleştirme yöntemi [ortam](xref:fundamentals/environments):

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

Çağrı yapmak <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> önce özel durumları yakalamak istediğiniz herhangi bir ara yazılım.

> [!WARNING]
> Geliştirici özel durumu sayfası etkinleştirme **yalnızca geliştirme ortamında uygulama çalışırken**. Üretim ortamında uygulama çalıştığında ayrıntılı özel durum bilgileri herkese açık şekilde paylaşma istemezsiniz. Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.

Sayfasında, özel durum ve isteği hakkında aşağıdaki bilgileri içerir:

* Yığın izlemesi
* Dize parametreleri (varsa) sorgulama
* Tanımlama bilgileri (varsa)
* Üst bilgileri

Geliştirici özel durum sayfasında görmek için [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), kullanın `DevEnvironment` önişlemci yönergesi ve select **bir özel durum harekete** giriş sayfasında.

## <a name="exception-handler-page"></a>Özel durum işleyicisi sayfası

Bir özel hata sayfası üretim ortamı için işleme yapılandırmak için özel durum işleme ara yazılım kullanın. Ara yazılım:

* Yakalar ve özel durumları günlüğe kaydeder.
* Belirtilen denetleyici ve sayfa için alternatif bir işlem hattı istekte yeniden yürütür. İstek, yanıt başladıysa, yeniden çalıştırılır değildir.

Aşağıdaki örnekte, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> geliştirme olmayan ortamlarda özel durum işleme ara yazılım ekler:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

Bir hata sayfası Razor sayfaları uygulaması şablonunu sunar (*.cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) içinde *sayfaları* klasör. Bir MVC uygulaması için proje şablonu, bir hata eylem yöntemi ve bir hata görünümü içerir. Eylem yöntemi aşağıda verilmiştir:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

HTTP yöntemi öznitelikleriyle hata işleyicisi eylem yöntemi gibi süslemek yoksa `HttpGet`. Açık fiilleri yöntemi bazı istekleri engellenir. Kimliği doğrulanmamış kullanıcılar hata görünümünün alabilirsiniz, böylece yöntemi anonim erişime izin verin.

### <a name="access-the-exception"></a>Erişim özel durumu

Kullanım <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> özel durum ve hata işleyicisi denetleyici ya da sayfa özgün istek yolu erişmek için:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> Yapmak **değil** önemli hata bilgilerini istemcilere hizmet. Hataları hizmet veren bir güvenlik riski oluşturur.

Özel durum işleme sayfasında görmek için [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), kullanın `ProdEnvironment` ve `ErrorHandlerPage` önişlemci yönergeleri ve select **bir özel durum harekete** giriş sayfasında.

## <a name="exception-handler-lambda"></a>Özel durum işleyici lambda

Alternatif bir [özel durum işleyicisi sayfa](#exception-handler-page) lambda sağlamaktır <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>. Bir lambda kullanarak yanıt döndürmeden önce hata erişim izni verir.

Özel durum işleme için bir lambda kullanma örneği aşağıda verilmiştir:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> Yapmak **değil** önemli hata bilgileri hizmet <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> veya <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> istemcilere. Hataları hizmet veren bir güvenlik riski oluşturur.

Özel durum işleme lambda içinde sonucunu görmek için [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), kullanın `ProdEnvironment` ve `ErrorHandlerLambda` önişlemci yönergeleri ve select **bir özel durum harekete** giriş sayfasında.

## <a name="usestatuscodepages"></a>UseStatusCodePages

Varsayılan olarak, ASP.NET Core uygulaması durumu kod sayfası için HTTP durum kodları, gibi sağlamaz *404 - Bulunamadı*. Uygulama, durum kodu ve boş yanıt gövdesi döndürür. Kod sayfaları durumu sağlamak için durum kod sayfası ara yazılımını kullanın.

Ara yazılım tarafından kullanılabilir hale getirileceğini [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) bulunduğu paketini [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).

Genel hata durum kodları için varsayılan salt metin işleyicileri etkinleştirmek için çağrı <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> içinde `Startup.Configure` yöntemi:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Çağrı `UseStatusCodePages` istek işleme Ara (örneğin, statik dosya ara yazılımlarını ve MVC ara yazılım) önce.

Varsayılan işleyici tarafından görüntülenen metnin bir örnek aşağıda verilmiştir:

```
Status Code: 404; Not Found
```

Çeşitli durumu kod sayfası biçimlerinden birini görmek için [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), önişlemci yönergeleri ile başlayan birini kullanın `StatusCodePages`seçip **404 bir tetikleyici** giriş sayfasında.

## <a name="usestatuscodepages-with-format-string"></a>Biçim dizesi ile UseStatusCodePages

Yanıt içerik türü ve metin özelleştirmek için aşırı yüklemesini kullanın. <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> , bir içerik türü ve biçim dizesini alır:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a>Lambda ile UseStatusCodePages

Özel hata işleme ve yazma yanıtını belirtmek için kod, aşırı yüklemesini kullanın <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> , bir lambda ifadesini alır:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a>UseStatusCodePagesWithRedirect

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> Genişletme yöntemi:

* Gönderen bir *302 bulundu -* istemciye durum kodu.
* İstemci URL'si şablonda verilen konuma yönlendirir.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

URL şablonu içerebilir bir `{0}` için durum kodunu, örnekte gösterildiği gibi yer tutucu. URL şablonu bir tilde (~) ile başlıyorsa tilde uygulamanın tarafından değiştirilir `PathBase`. Uygulamadaki bir uç noktaya işaret ederseniz, bir MVC görünümü ya da uç noktası için Razor sayfası oluşturun. Razor sayfaları örnek için bkz: [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) içinde [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).

Bu yaygın bir yöntemdir kullanılabilir uygulama:

* Genellikle, farklı bir uygulama hatası burada işler durumlarda farklı bir uç noktası için istemci yönlendirmelidir. Web apps için yeniden yönlendirilen uç nokta istemcinin tarayıcınızın adres çubuğuna yansıtır.
* Olmamalıdır korumak ve özgün ilk yönlendirme yanıt durum koduyla döndürür.

## <a name="usestatuscodepageswithreexecute"></a>UseStatusCodePagesWithReExecute

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> Genişletme yöntemi:

* Özgün durum kodunu istemciye döndürür.
* Yanıt gövdesi, alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek oluşturur.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

Uygulamadaki bir uç noktaya işaret ederseniz, bir MVC görünümü ya da uç noktası için Razor sayfası oluşturun. Razor sayfaları örnek için bkz: [StatusCode.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/Pages/StatusCode.cshtml) içinde [örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).

Bu yöntem, uygulamayı gerektiğinde yaygın olarak kullanılır:

* İstek farklı bir uç noktasına yönlendirme olmadan işlem. Web apps için başlangıçta istenen uç noktası istemcinin tarayıcınızın adres çubuğuna yansıtır.
* Korumak ve özgün durum koduyla bir yanıt döndürür.

URL ve sorgu dize şablonları, bir yer tutucu içerebilir (`{0}`) durum kodu. URL şablonu bir eğik çizgiyle başlamalıdır (`/`). Bir yer tutucu yolunda kullanırken, uç noktayı (sayfa veya denetleyicisi) yol kesimi işleyebilir onaylayın. Örneğin, hatalar için bir Razor sayfası ile isteğe bağlı yol kesimi değerini kabul etmelidir `@page` yönergesi:

```cshtml
@page "{code?}"
```

Hata işleme uç noktasını, aşağıdaki örnekte gösterildiği gibi hatayı oluşturan özgün URL'yi alabilirsiniz:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a>Durum kod sayfaları devre dışı bırak

Durum kod sayfaları için Razor sayfaları işleyicisi yöntemi veya MVC denetleyicisi belirli isteklere devre dışı bırakılabilir. Durum kod sayfaları devre dışı bırakmak için <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Özel durum işleme kodu

Özel durum işleme sayfaları kodda özel durumlar. Genellikle, tamamen statik içeriği oluşmalıdır üretim hata sayfaları için iyi bir fikir olduğunu.

### <a name="response-headers"></a>Yanıt Üstbilgileri

Yanıt Üstbilgileri gönderdikten sonra:

* Uygulama, yanıtın durum kodu değiştiremezsiniz.
* Herhangi bir özel durum sayfaları veya işleyicileri çalıştıramazsınız. Yanıt tamamlanması gereken veya bağlantı kesildi.

## <a name="server-exception-handling"></a>Sunucu özel durum işleme

Özel durum işleme mantığı, uygulamanıza ek olarak [HTTP sunucusu uygulamasını](xref:fundamentals/servers/index) bazı özel durumları işleyebilir. Yanıt üst bilgileri gönderilmeden önce sunucunun bir özel durumu yakalar, sunucunun gönderdiği bir *500 - İç sunucu hatası* yanıt gövdesi olmadan yanıt. Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durumu yakalar, sunucu bağlantıyı kapatır. Uygulamanız tarafından işlenmeyen isteği sunucu tarafından işlenir. Sunucu isteği işlerken oluşan özel sunucu özel durum tarafından işlenir işleme. Uygulamanın özel hata sayfaları, özel durum işleme ara yazılım ve filtreler bu davranışı etkilemez.

## <a name="startup-exception-handling"></a>Başlangıç özel durum işleme

Yalnızca barındırma katmanı, uygulama başlatma sırasında gerçekleşmesi özel durumları işleyebilir. Konak için yapılandırılabilir [yakalama başlatma hataları](xref:fundamentals/host/web-host#capture-startup-errors) ve [ayrıntılı hataları yakalamaya](xref:fundamentals/host/web-host#detailed-errors).

Ana bilgisayar adresi/bağlantı noktası sonra bağlama yalnızca hata oluşursa bir hata sayfası için yakalanan başlatma hatası barındırma katman gösterebilirsiniz. Bağlama başarısız olursa:

* Barındırma katman kritik bir özel durumu günlüğe kaydeder.
* Dotnet işlem kilitleniyor.
* HTTP sunucusu olduğunda hiçbir hata sayfası görüntülenir [Kestrel](xref:fundamentals/servers/kestrel).

Çalışırken [IIS](/iis) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - işlem hatası* tarafından döndürülen [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemi başlatılamazsa . Daha fazla bilgi için bkz. <xref:host-and-deploy/iis/troubleshoot>. Azure App Service ile başlatma sorunlarını giderme hakkında daha fazla bilgi için bkz: <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="database-error-page"></a>Veritabanı hata sayfası

[Veritabanı hata sayfası](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) ara yazılım, Entity Framework geçişleri kullanarak çözülebilir, veritabanı ile ilgili özel durumları yakalar. Bu özel durumlar oluştuğunda, bu sorunu çözmek için olası Eylemler ilgili ayrıntıları içeren bir HTML yanıtını oluşturulur. Bu sayfa yalnızca geliştirme ortamında etkinleştirilmesi gerekir. Kod ekleyerek sayfası etkinleştirme `Startup.Configure`:

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a>Özel durum filtreleri

MVC uygulamalarında özel durum filtreleri genel olarak veya her denetleyici veya eylem başına temelinde yapılandırılabilir. Razor sayfaları uygulamalar, bunlar genel olarak veya sayfa modeli başına yapılandırılabilir. Bu filtreler bir denetleyici eylemi veya başka bir filtre yürütülmesi sırasında oluşan tüm işlenmeyen bir özel durumu işle. Daha fazla bilgi için bkz. <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> Özel durum filtreleri, MVC Eylemler içinde oluşan özel durumları yakalama için yararlıdır, ancak bunlar özel durum işleme ara yazılımı kadar esnek değildir. Ara yazılım kullanmanızı öneririz. MVC eylemi seçilen farklı tabanlı hata işleme gerçekleştirmek için yalnızca gerek duyduğunuz filtreleri kullanın.

## <a name="model-state-errors"></a>Model durumu hataları

Model durumu hatalarının nasıl işleneceğini hakkında daha fazla bilgi için bkz. [Model bağlama](xref:mvc/models/model-binding) ve [Model doğrulama](xref:mvc/models/validation).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
