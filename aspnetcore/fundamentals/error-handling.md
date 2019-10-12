---
title: ASP.NET Core hataları işleme
author: rick-anderson
description: ASP.NET Core uygulamalarında hataların nasıl işleneceğini öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a610c42d75864259b609e11b8bf0776c5ab8e507
ms.sourcegitcommit: 020c3760492efed71b19e476f25392dda5dd7388
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2019
ms.locfileid: "72288855"
---
# <a name="handle-errors-in-aspnet-core"></a>ASP.NET Core hataları işleme

[Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex)ve [Steve Smith](https://ardalis.com/) tarafından

Bu makalede ASP.NET Core uygulamalardaki hataları işlemeye yönelik yaygın yaklaşımlar ele alınmaktadır.

[Örnek kodu görüntüleyin veya indirin](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples). ([Nasıl indirilir](xref:index#how-to-download-a-sample).) Makale, farklı senaryoları etkinleştirmek için örnek uygulamada Önişlemci yönergelerinin (`#if`, `#endif`, `#define`) nasıl ayarlanacağı hakkında yönergeler içerir.

## <a name="developer-exception-page"></a>Geliştirici özel durum sayfası

*Geliştirici özel durum sayfası* , istek özel durumları hakkında ayrıntılı bilgileri görüntüler. Sayfa, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içindeki [Microsoft. Aspnetcore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) paketi tarafından kullanılabilir hale getirilir. Uygulama geliştirme [ortamında](xref:fundamentals/environments)çalışırken sayfayı etkinleştirmek için `Startup.Configure` yöntemine kod ekleyin:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

Özel durumları yakalamak istediğiniz herhangi bir ara yazılımın önüne <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> ' a çağrı koyun.

> [!WARNING]
> Geliştirici özel durum sayfasını **yalnızca uygulama geliştirme ortamında çalışırken**etkinleştirin. Uygulama üretimde çalıştırıldığında ayrıntılı özel durum bilgilerini herkese açık bir şekilde paylaşmak istemezsiniz. Ortamları yapılandırma hakkında daha fazla bilgi için bkz. <xref:fundamentals/environments>.

Sayfa, özel durum ve istek hakkında şu bilgileri içerir:

* Yığın izleme
* Sorgu dizesi parametreleri (varsa)
* Tanımlama bilgileri (varsa)
* Üst bilgiler

[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)geliştirici özel durum sayfasını görmek için `DevEnvironment` ön işlemci yönergesini kullanın ve giriş sayfasında **özel durum Tetikle** ' yi seçin.

## <a name="exception-handler-page"></a>Özel durum işleyici sayfası

Üretim ortamı için özel bir hata işleme sayfası yapılandırmak için özel durum Işleme ara yazılımını kullanın. Ara yazılım:

* Özel durumları yakalar ve günlüğe kaydeder.
* İsteği, belirtilen sayfa veya denetleyici için alternatif bir ardışık düzende yeniden yürütür. Yanıt başlatılmışsa istek yeniden yürütülmez.

Aşağıdaki örnekte, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>, geliştirme dışı ortamlarda özel durum Işleme ara yazılımını ekler:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

Razor Pages App Template, *Sayfalar* klasöründe bir hata sayfası ( *. cshtml*) ve <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> sınıfı (`ErrorModel`) sağlar. MVC uygulaması için, proje şablonu bir hata eylemi yöntemi ve bir hata görünümü içerir. Eylem yöntemi aşağıda verilmiştir:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

@No__t-0 gibi HTTP Yöntem öznitelikleriyle hata işleyicisi eylem yöntemini süslememe. Açık fiiller bazı isteklerin yönteme ulaşmasını önler. Kimliği doğrulanmamış kullanıcıların hata görünümünü alabilmesi için metoda anonim erişime izin verin.

### <a name="access-the-exception"></a>Özel duruma erişin

Bir hata işleyicisi denetleyicisi veya sayfasındaki özel duruma ve özgün istek yoluna erişmek için <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> kullanın:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> İstemcilere hassas hata bilgileri sunma. Hatalara hizmet vermek bir güvenlik riskidir.

[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)özel durum işleme sayfasını görmek için `ProdEnvironment` ve `ErrorHandlerPage` ön işlemci yönergelerini kullanın ve giriş sayfasında **bir özel durum Tetikle** ' yi seçin.

## <a name="exception-handler-lambda"></a>Özel durum işleyici lambda

Özel bir özel [durum işleyici sayfasına](#exception-handler-page) bir alternatif, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> ' e bir lambda sağlamaktır. Lambda kullanılması, yanıtı döndürmeden önce hataya erişim sağlar.

Özel durum işleme için lambda kullanmanın bir örneği aşağıda verilmiştir:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> @No__t -1 veya <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> ' den istemcilere hassas hata bilgileri sunma. Hatalara hizmet vermek bir güvenlik riskidir.

[Örnek uygulamada](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)lambda işlemenin özel durum işleme sonucunu görmek için `ProdEnvironment` ve `ErrorHandlerLambda` ön işlemci yönergelerini kullanın ve giriş sayfasında **bir özel durum Tetikle** ' yi seçin.

## <a name="usestatuscodepages"></a>UseStatusCodePages

Varsayılan olarak, bir ASP.NET Core uygulama HTTP durum kodları için *404-bulunamadı*gibi bir durum kodu sayfası sağlamaz. Uygulama bir durum kodu ve boş bir yanıt gövdesi döndürür. Durum kodu sayfaları sağlamak için durum kodu sayfaları ara yazılımını kullanın.

Ara yazılım, [Microsoft. aspnetcore. app metapackage](xref:fundamentals/metapackage-app)içindeki [Microsoft. Aspnetcore. Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) paketi tarafından kullanılabilir hale gelir.

Ortak hata durum kodları için varsayılan salt metin işleyicilerini etkinleştirmek üzere `Startup.Configure` yönteminde <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> ' ı çağırın:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

İstek işleme ara yazılımı (örneğin, statik dosya ara yazılımı ve MVC ara yazılımı) öncesinde `UseStatusCodePages` ' ı çağırın.

Varsayılan işleyiciler tarafından görüntülenbir metin örneği aşağıda verilmiştir:

```
Status Code: 404; Not Found
```

[Örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)çeşitli durum kodu sayfası biçimlerinden birini görmek için, `StatusCodePages` ile başlayan Önişlemci yönergelerinden birini kullanın ve giriş sayfasında **404 Tetikle** ' i seçin.

## <a name="usestatuscodepages-with-format-string"></a>Biçim dizesiyle UseStatusCodePages

Yanıt içerik türünü ve metnini özelleştirmek için, içerik türü ve biçim dizesi alan <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> ' nın aşırı yüklemesini kullanın:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a>Lambda ile UseStatusCodePages

Özel hata işleme ve yanıt yazma kodu belirtmek için, lambda ifadesi alan <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> ' nın aşırı yüklemesini kullanın:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirects"></a>Usestatuscodepageswithyönlendirmeler

@No__t-0 genişletme yöntemi:

* İstemciye *302 tarafından bulunan* bir durum kodu gönderir.
* İstemciyi, URL şablonunda belirtilen konuma yönlendirir.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

URL şablonu, örnekte gösterildiği gibi durum kodu için `{0}` yer tutucusu içerebilir. URL şablonu bir tilde (~) ile başlıyorsa, tilde uygulamanın `PathBase` ' dır. Uygulamanın içindeki bir uç noktayı işaret ederseniz, uç nokta için bir MVC görünümü veya Razor sayfası oluşturun. Razor Pages bir örnek için bkz. [örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) *Pages/StatusCode. cshtml* .

Bu yöntem genellikle uygulama şu şekilde kullanılır:

* , Genellikle farklı bir uygulamanın hatayı işlediği durumlarda istemciyi farklı bir uç noktaya yönlendirmelidir. Web Apps için, istemcinin tarayıcı adres çubuğu yeniden yönlendirilen uç noktayı yansıtır.
* İlk yeniden yönlendirme yanıtıyla birlikte özgün durum kodunu korumamalıdır ve döndürmemelidir.

## <a name="usestatuscodepageswithreexecute"></a>UseStatusCodePagesWithReExecute

@No__t-0 genişletme yöntemi:

* İstemciye özgün durum kodunu döndürür.
* Alternatif bir yol kullanarak istek ardışık düzenini yeniden yürüterek yanıt gövdesini oluşturur.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

Uygulamanın içindeki bir uç noktayı işaret ederseniz, uç nokta için bir MVC görünümü veya Razor sayfası oluşturun. Razor Pages bir örnek için bkz. [örnek uygulamadaki](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) *Pages/StatusCode. cshtml* .

Bu yöntem genellikle uygulama şunları yaparken kullanılır:

* Farklı bir uç noktaya yönlendirmeye gerek kalmadan isteği işleyin. Web Apps için, istemcinin tarayıcı adres çubuğu, ilk olarak istenen uç noktayı yansıtır.
* Yanıt ile orijinal durum kodunu koruyun ve geri döndürün.

URL ve sorgu dizesi şablonları durum kodu için bir yer tutucu (`{0}`) içerebilir. URL şablonu eğik çizgiyle (`/`) başlamalıdır. Yolda bir yer tutucu kullanırken, uç noktanın (sayfa veya denetleyicinin) yol kesimini işleyediğini doğrulayın. Örneğin, bir Razor sayfası, `@page` yönergesi ile isteğe bağlı yol segmenti değerini kabul etmelidir:

```cshtml
@page "{code?}"
```

Hatayı işleyen uç nokta, aşağıdaki örnekte gösterildiği gibi, hatayı oluşturan özgün URL 'YI alabilir:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a>Durum kodu sayfalarını devre dışı bırak

MVC denetleyicisi veya eylem yöntemi için durum kodu sayfalarını devre dışı bırakmak için [[Skipstatuscodepages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) özniteliğini kullanın.

Razor Pages işleyici yöntemindeki veya bir MVC denetleyicisindeki belirli istekler için durum kodu sayfalarını devre dışı bırakmak için <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> kullanın:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Özel durum işleme kodu

Özel durum işleme sayfalarındaki kod özel durumlar oluşturabilir. Üretim hatası sayfalarının tamamen statik içerikten oluşması genellikle iyi bir fikirdir.

### <a name="response-headers"></a>Yanıt üst bilgileri

Yanıt üstbilgileri gönderildikten sonra:

* Uygulama, yanıtın durum kodunu değiştiremiyor.
* Tüm özel durum sayfaları veya işleyicileri çalıştırılamaz. Yanıt tamamlanmalıdır veya bağlantı iptal edilmiş olmalıdır.

## <a name="server-exception-handling"></a>Sunucu özel durum işleme

Uygulamanızın özel durum işleme mantığına ek olarak, [http sunucusu uygulaması](xref:fundamentals/servers/index) bazı özel durumları işleyebilir. Sunucu yanıt üstbilgileri gönderilmeden önce bir özel durum yakalarsa, sunucu yanıt gövdesi olmadan *500-Iç sunucu hatası* yanıtı gönderir. Yanıt üstbilgileri gönderildikten sonra sunucu bir özel durum yakalar, sunucu bağlantıyı kapatır. Uygulamanız tarafından işlenmeyen istekler sunucu tarafından işlenir. Sunucu isteği gerçekleştirirken oluşan tüm özel durumlar sunucunun özel durum işleme tarafından işlenir. Uygulamanın özel hata sayfaları, özel durum işleme ara yazılımı ve filtreler bu davranışı etkilemez.

## <a name="startup-exception-handling"></a>Başlatma özel durum işleme

Uygulama başlangıcında gerçekleşen özel durumları yalnızca barındırma katmanı işleyebilir. Konak, [Başlangıç hatalarını yakalamak](xref:fundamentals/host/web-host#capture-startup-errors) ve [ayrıntılı hataları yakalamak](xref:fundamentals/host/web-host#detailed-errors)için yapılandırılabilir.

Barındırma katmanı, yalnızca hatanın ana bilgisayar adresi/bağlantı noktası bağlamalarından sonra oluşması durumunda yakalanan başlatma hatası için bir hata sayfası gösterebilir. Bağlama başarısız olursa:

* Barındırma katmanı, kritik bir özel durumu günlüğe kaydeder.
* DotNet işlemi kilitleniyor.
* HTTP sunucusu [Kestrel](xref:fundamentals/servers/kestrel)olduğunda hata sayfası gösterilmez.

[IIS](/iis) 'de (veya Azure App Service) veya [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)çalışırken, işlem başlatılamazsa [ASP.NET Core modülü](xref:host-and-deploy/aspnet-core-module) tarafından *502,5 işlem hatası* döndürülür. Daha fazla bilgi için bkz. <xref:test/troubleshoot-azure-iis>.

## <a name="database-error-page"></a>Veritabanı hata sayfası

Veritabanı hata sayfası ara yazılımı, Entity Framework geçişleri kullanılarak çözümleneyolabilecek veritabanıyla ilgili özel durumları yakalar. Bu özel durumlar gerçekleştiğinde, sorunu çözmeye yönelik olası eylemlerin ayrıntılarını içeren bir HTML yanıtı oluşturulur. Bu sayfa yalnızca geliştirme ortamında etkinleştirilmelidir. @No__t-0 ' a kod ekleyerek sayfayı etkinleştirin:

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

<!-- FUTURE UPDATE: On the next topic overhaul/release update, add API crosslink to this section for xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage* when available via the API docs. -->

## <a name="exception-filters"></a>Özel durum filtreleri

MVC uygulamalarında özel durum filtreleri, genel olarak veya denetleyicide veya eylem temelinde yapılandırılabilir. Razor Pages uygulamalarda, genel olarak veya sayfa modeli başına yapılandırılabilirler. Bu filtreler, bir denetleyici eyleminin veya başka bir filtrenin yürütülmesi sırasında oluşan işlenmemiş özel durumları işler. Daha fazla bilgi için bkz. <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> Özel durum filtreleri, MVC eylemleri içinde oluşan özel durumları yakalamak için yararlıdır, ancak özel durum Işleme ara yazılımı kadar esnek değildir. Ara yazılımı kullanmanızı öneririz. Yalnızca hangi MVC eyleminin seçilmesinden ayrı olarak hata işleme yapmanız gereken durumlarda filtreleri kullanın.

## <a name="model-state-errors"></a>Model durumu hataları

Model durumu hatalarını işleme hakkında daha fazla bilgi için bkz. [model bağlama](xref:mvc/models/model-binding) ve [model doğrulaması](xref:mvc/models/validation).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
