---
title: ASP.NET Web API ASP.NET Core için geçirme
author: ardalis
description: Bir web API uygulaması için ASP.NET Core MVC ASP.NET 4.x Web API'si geçirmeyi öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: 9806c502f8f5244740f9f9614657a40cfaa03314
ms.sourcegitcommit: 1872d2e6f299093c78a6795a486929ffb0bbffff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53216839"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>ASP.NET Web API ASP.NET Core için geçirme

Tarafından [Scott Addie](https://twitter.com/scott_addie) ve [Steve Smith](https://ardalis.com/)

ASP.NET 4.x Web API'si, istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaştığında bir HTTP hizmetidir. ASP.NET Core, ASP.NET 4.x'ın MVC birleştirir ve ASP.NET Core MVC bilinen basit bir programlama modeli içinde Web API uygulaması modeller. Bu makalede, ASP.NET 4.x Web API'si için ASP.NET Core MVC geçirmek için gereken adımları gösterilmektedir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>ASP.NET 4.x Web API projesi gözden geçirin

Bu makalede bir başlangıç noktası olarak kullandığı *ProductsApp* oluşturulan proje [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api). Projede, basit bir ASP.NET 4.x Web API projesi şu şekilde yapılandırılmıştır.

İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` Sınıfı içinde bulunan *App_Start* klasör ve bir statik `Register` yöntemi:

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

Bu sınıf yapılandırır [öznitelik yönlendirme](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), ancak aslında projede kullanılıyor. Ayrıca, ASP.NET Web API'si tarafından kullanılan yönlendirme tablosunun yapılandırır. Bu durumda, biçim ile eşleşmesi için URL ASP.NET 4.x Web API bekliyor `/api/{controller}/{id}`, ile `{id}` isteğe bağlı olan.

*ProductsApp* proje bir denetleyici içerir. Denetleyici devraldığı `ApiController` ve iki eylemleri içerir:

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

`Product` Modeli tarafından kullanılan `ProductsController` basit sınıfı:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Aşağıdaki bölümlerde ASP.NET Core MVC Web API projesi geçişini gösterir.

## <a name="create-destination-project"></a>Hedef proje oluşturma

Visual Studio'da aşağıdaki adımları tamamlayın:

* Git **dosya** > **yeni** > **proje** > **diğer proje türleri**  >  **Visual Studio çözümleri**. Seçin **boş çözüm**ve çözüm adı *WebAPIMigration*. Tıklayın **Tamam** düğmesi.
* Mevcut yapıtı Ekle *ProductsApp* çözüme bir proje.
* Yeni bir **ASP.NET Core Web uygulaması** çözüme bir proje. Seçin **.NET Core** hedef framework'açılır ve seçin **API** proje şablonu. Projeyi adlandırın *ProductsCore*, tıklatıp **Tamam** düğmesi.

Çözüm, artık iki proje içermektedir. Aşağıdaki bölümlerde, geçiş açıklanmaktadır *ProductsApp* projenin içeriğini *ProductsCore* proje.

## <a name="migrate-configuration"></a>Yapılandırma geçişi

ASP.NET Core kullanmaz *App_Start* klasör veya *Global.asax* dosyasını ve *web.config* dosya eklendiğinde yayımlama zamanı. *Startup.cs* ardılı olan *Global.asax* ve proje kök dizininde bulunur. `Startup` Sınıfı, tüm uygulama başlatma görevlerini işler. Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

ASP.NET Core MVC öznitelik yönlendirme varsayılan olarak dahil edilir olduğunda <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> çağrılma yeri `Startup.Configure`. Aşağıdaki `UseMvc` çağrı değiştirir *ProductsApp* projenin *App_Start/WebApiConfig.cs* dosyası:

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>Modelleri ve denetleyicileri geçirme

Kopyalayabilirsiniz *ProductApp* projenin denetleyici ve modeli kullanır. Aşağıdaki adımları uygulayın:

1. Kopyalama *Controllers/ProductsController.cs* özgün proje için yeni bir tane.
1. Tamamını *modelleri* özgün proje klasörüne yeni bir tane.
1. Yeni Proje adıyla eşleşmesi için kopyalanan dosyalar ad alanlarını değiştirmek (*ProductsCore*). Ayarlama `using ProductsApp.Models;` deyiminde *ProductsController.cs* çok.

Bu noktada, derleme hataları çeşitli uygulama sonuçları oluşturma. Aşağıdaki bileşenler de ASP.NET Core mevcut olmaması nedeniyle hatalar oluşur:

* `ApiController` Sınıfı
* `System.Web.Http` Namespace
* `IHttpActionResult` Arabirimi

Şu şekilde hataları düzeltin:

1. Değişiklik `ApiController` için <xref:Microsoft.AspNetCore.Mvc.ControllerBase>. Ekleme `using Microsoft.AspNetCore.Mvc;` çözümlenecek `ControllerBase` başvuru.
1. `using System.Web.Http;` klasörünü silin.
1. Değişiklik `GetProduct` eylemin dönüş türünden `IHttpActionResult` için `ActionResult<Product>`.

Basitleştirmek `GetProduct` eylemin `return` aşağıdaki deyimi:

```csharp
return product;
```

## <a name="configure-routing"></a>Yönlendirmeyi Yapılandırma

Yönlendirmeyi şu şekilde yapılandırın:

1. Süslemek `ProductsController` aşağıdaki özniteliklerle sınıfı:

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    Önceki [[yol]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) özniteliği, denetleyicinin öznitelik yönlendirme deseni yapılandırır. [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) özniteliği öznitelik bu denetleyicide tüm eylemler için bir gereksinim yönlendirme sağlar.

    Öznitelik yönlendirme belirteçleri destekler, gibi `[controller]` ve `[action]`. Çalışma zamanında her belirteç denetleyici veya eylemin, adıyla sırasıyla özniteliği uygulanmış değiştirilir. Belirteçler, projedeki Sihirli dize sayısını azaltın. Belirteçleri de yollar ilgili denetleyicileri ile eşitlenmiş olarak kalır ve otomatik yeniden adlandırdığınızda yeniden düzenlemeler eylemleri uygulanan emin olun.
1. Projenin uyumluluk modu için ASP.NET Core 2.2 ayarlayın:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    Yukarıdaki değişikliği:

    * Kullanmak için gerekli `[ApiController]` denetleyici düzeyinde özniteliği.
    * ASP.NET Core 2.2 içinde tanıtılan davranışları bozucu olabilecek için kabul eder.
1. HTTP Get isteklerini etkinleştirme `ProductController` eylemler:
    * Uygulama [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) özniteliğini `GetAllProducts` eylem.
    * Uygulama `[HttpGet("{id}")]` özniteliğini `GetProduct` eylem.

Önceki değişiklikler ve kullanılmayan kaldırılmasını sonra `using` deyimleri *ProductsController.cs* dosya şu şekilde görünür:

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

Geçirilen proje çalıştırın ve göz atın `/api/products`. Üç ürün tam bir listesi görüntülenir. konumuna gözatın `/api/products/1`. İlk ürün görünür.

## <a name="compatibility-shim"></a>Uyumluluk dolgusu

[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı, ASP.NET 4.x Web API projelerini ASP.NET Core taşımak için Uyumluluk dolgu sağlar. Uyumluluk dolgu, ASP.NET Core, ASP.NET 4.x Web API 2 kurallarından sayısını destekleyecek şekilde genişletir. Bu belgede daha önce unity'nin örnek uyumluluk dolgu gereksiz yeterince temel üyeliktir. Daha büyük projeler için Uyumluluk dolgu kullanarak ASP.NET Core ve ASP.NET 4.x Web API 2 arasındaki API açığını geçici olarak köprüleme yararlı olabilir.

Web API Uyumluluk dolgu geçirme büyük ASP.NET 4.x Web API projelerini ASP.NET core'a desteklemek için geçici bir önlem kullanılmak üzere tasarlanmıştır. Zaman içinde projeler üzerinde uyumluluğu dolgu güvenmek yerine, ASP.NET Core düzenlerinin kullanılacağı güncelleştirilmelidir.

Uyumluluk özellikleri dahil `Microsoft.AspNetCore.Mvc.WebApiCompatShim` içerir:

* Ekler bir `ApiController` denetleyicileri temel türleri güncelleştirilmesi gerekmeyen yazın.
* Web API stili model bağlama sağlar. ASP.NET Core MVC, model bağlama işlevlerine benzer şekilde, ASP.NET 4.x MVC 5, varsayılan olarak. Uyumluluk dolgu değişiklikleri model daha ASP.NET 4.x Web API 2 model bağlama kurallarına benzer şekilde bağlama. Örneğin, karmaşık türler gövdeden otomatik olarak bağlanır.
* Denetleyici eylemleri türünde parametre yararlanabilmeniz model bağlama genişletir `HttpRequestMessage`.
* İleti biçimlendiriciler eylemleri izin verme türü sonuçları döndürmek için ekler `HttpResponseMessage`.
* Web API 2 eylemleri yanıtlar verecek kullanmış olabilirsiniz ek yanıt yöntemleri ekler:
  * `HttpResponseMessage` oluşturucuları:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Eylem sonucu yöntemleri:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Bir örneğini ekler `IContentNegotiator` uygulamaya bağımlılık ekleme (dı) kapsayıcı kullanıcının ve içerik anlaşması ile ilgili türlerinden kullanılabilmesini [System.NET.http.Formatting](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/). Bu tür örnekleri, `DefaultContentNegotiator` ve `MediaTypeFormatter`.

Uyumluluk dolgu kullanmak üzere:

1. Yükleme [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketi.
1. Çağırarak uygulamanın DI kapsayıcı ile uyumluluk dolgu ait Hizmetleri kaydedin `services.AddMvc().AddWebApiConventions()` içinde `Startup.ConfigureServices`.
1. Web API özel yolları kullanarak tanımlama `MapWebApiRoute` üzerinde `IRouteBuilder` uygulamasının `IApplicationBuilder.UseMvc` çağırın.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
