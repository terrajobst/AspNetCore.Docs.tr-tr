---
title: ASP.NET Web API ASP.NET Core geçirme
author: ardalis
description: Bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek öğrenin.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 9385805d548bc87f4a50b87f2c06aa74abdaf8af
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272536"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>ASP.NET Web API ASP.NET Core geçirme

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)

Web API'leri istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetleridir. ASP.NET Core MVC web uygulamaları oluşturma tek, tutarlı bir yol sağlayarak Web API'ları oluşturmak için destek içerir. Bu makalede, bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API geçirmek için gereken adımları gösterir.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>Gözden geçirme ASP.NET Web API projesi

Bu makalede örnek proje kullanan *ProductsApp*, makalede oluşturulmuş [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) , başlangıç noktası olarak. Bu projede basit bir ASP.NET Web API projesi şu şekilde yapılandırılır.

İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` tanımlanan *App_Start*, ve yalnızca bir statik `Register` yöntemi:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


Bu sınıf yapılandırır [özniteliği yönlendirme](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), aslında projesinde kullanılan rağmen. Ayrıca, ASP.NET Web API tarafından kullanılan yönlendirme tablosu yapılandırır. Bu durumda, ASP.NET Web API biçim ile eşleşmesi için URL'leri beklediği */api/ {controller} / {id}*, ile *{id}* isteğe bağlı olması.

*ProductsApp* proje içeriyor devraldığı tek bir basit denetleyicisi `ApiController` ve iki yöntem sunar:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Son olarak, model, *ürün*, tarafından kullanılan *ProductsApp*, basit bir sınıf:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Biz basit bir proje başlayacağı sahip olduğunuza göre Biz bu Web API projesi ASP.NET Core MVC geçirmek nasıl ekleyebileceğiniz gösterilmektedir.

## <a name="create-the-destination-project"></a>Hedef projesi oluşturma

Visual Studio kullanarak, yeni, boş bir çözüm oluşturun ve adlandırın *WebAPIMigration*. Varolan eklemek *ProductsApp* ona proje sonra yeni bir ASP.NET çekirdek Web uygulama projesi ekleyin. Yeni proje adı *ProductsCore*.

![Yeni Proje iletişim kutusu açmak için Web Şablonları](webapi/_static/add-web-project.png)

Ardından, Web API projesi şablonunu seçin. Planlıyoruz *ProductsApp* bu yeni projeye içeriği.

![ASP.NET Core şablonları listesinde seçilen Web API projesi şablonuyla yeni Web uygulaması iletişim kutusu](webapi/_static/aspnet-5-webapi.png)

Silme `Project_Readme.html` yeni proje dosyasından. Çözümünüzü gibi görünmelidir:

![Uygulama çözüm dosyaları ve klasörleri ProductsApp ve ProductsCore projelerin gösteren Çözüm Gezgini'nde Aç](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Yapılandırmasını geçir

ASP.NET Core artık kullanan *Global.asax*, *web.config*, veya *App_Start* klasörler. Bunun yerine, tüm başlangıç görevleri yapılır *haline* proje kökündeki (bkz [uygulama başlangıç](../fundamentals/startup.md)). ASP.NET Core MVC'de öznitelik tabanlı yönlendirme artık varsayılan olarak dahil edilmiştir, `UseMvc()` olarak adlandırılır; ve bu Web API yolları yapılandırmak için önerilen yaklaşımdır (ve Web API başlangıç projesi yönlendirme nasıl işler).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

Öznitelik ileride projenizde yönlendirme kullanmak istediğiniz varsayıldığında, ek bir yapılandırma gerekmez. Yalnızca örnek gerçekleştirilir gibi denetleyicileri ve eylemleri gerektiği gibi öznitelikleri uygulanır `ValuesController` Web API starter projeye dahil sınıfı:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Varlığını Not *[denetleyicisi]* 8 satırındaki. Öznitelik tabanlı şimdi yönlendirmeyi destekleyen belirli belirteçleri gibi *[denetleyicisi]* ve *[eylem]*. Bu belirteçler çalışma zamanında denetleyici veya eylemin, adı ile sırasıyla öznitelik uygulanmış değiştirilir. Bu proje Sihirli dizelerde sayısını azaltmak için kullanılır ve otomatik yeniden adlandırma yapan yeniden düzenlemeler uygulandığında yolların kendi ilgili denetleyicileri ve eylemleri ile eşitlenmiş tutulacak sağlar.

Ürünler API denetleyicisi geçirmek için biz öncelikle kopyalamanız gerekir *ProductsController* yeni projeye. Ardından denetleyicisinde yol özniteliği yalnızca şunlardır:

```csharp
[Route("api/[controller]")]
```

Ayrıca eklemeniz gerekir `[HttpGet]` özniteliği için iki yöntem, her ikisi de HTTP Get çağrılmalıdır beri. Öznitelik için bir "id" parametresi beklentisi dahil `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

Bu noktada, yönlendirme doğru yapılandırılmış; Ancak, henüz bunu test edemiyor. Ek değişiklikler yapılan, önce *ProductsController* derlenir.

## <a name="migrate-models-and-controllers"></a>Modelleri ve denetleyiciler geçirme

Bu basit Web API projesi için geçiş işlemi son adımda denetleyicileri ve kullandıkları modelleri kopyalamaktır. Bu durumda, yalnızca kopyalama *Controllers/ProductsController.cs* yeni bir için özgün projeden. Ardından, tüm modeller klasörü için yeni bir özgün projeden kopyalayın. Yeni proje adı ile eşleşmesi için ad alanları ayarlayın (*ProductsCore*).  Bu noktada, uygulama oluşturabilir ve derleme hatalarının sayısı bulacaksınız. Bunlar genellikle aşağıdaki kategorilere ayrılır:

* *ApiController* yok

* *System.Web.Http* ad alanı mevcut değil

* *Ihttpactionresult* yok

Neyse ki, bu düzeltmesi tümü çok kolay şunlardır:

* Değişiklik *ApiController* için *denetleyicisi* (eklemeniz gerekebilir *Microsoft.AspNetCore.Mvc kullanarak*)

* Silmek için başvuran deyimiyle *System.Web.Http*

* Tüm yöntemi döndürme değiştirme *Ihttpactionresult* döndürmek için bir *IActionResult*

Bu değişiklikler yapılmış ve kullanılmayan işlendikten sonra using deyimleri kaldırıldı, geçirilen *ProductsController* sınıfı şu şekilde görünür:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Şimdi geçirilen projeyi çalıştırın ve Gözat yapabiliyor olmanız gerekir */api/ürünleri*; ve 3 ürünlerinin tam listesini görmelisiniz. Gözat */api/products/1* ve ilk ürün görmeniz gerekir.

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a>Microsoft.AspNetCore.Mvc.WebApiCompatShim

ASP.NET Core geçirme ASP.NET Web API projeleri zaman yararlı bir araçtır [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı. Uyumluluk dolgusu kullanılmak üzere farklı Web API 2 kuralları sayısı izin vermek için ASP.NET Core genişletir. Bu belgede daha önce bağlantı noktalı uyumluluk dolgusu gerekli değildi yeterince temel örnektir. Büyük projeler için uyumluluk dolgusu kullanarak geçici olarak API boşluk ASP.NET Core ve ASP.NET Web API 2 arasında köprü oluşturma için yararlı olabilir.

Web API uyumluluk dolgusu geçici bir ölçü olarak ASP.NET Core büyük geçirme Web API projeleri kolaylaştırmak için kullanılmak üzere tasarlanmıştır. ASP.NET Core desenleri uyumluluk dolgusu güvenmek yerine kullanmak için zaman içinde projeleri güncelleştirilmesi gerekir. 

İçinde Microsoft.AspNetCore.Mvc.WebApiCompatShim dahil uyumluluk özellikleri içerir:

* Ekler bir `ApiController` denetleyicileri taban türleri güncelleştirilecek gerekmemesi yazın.
* Web API stili model bağlama sağlar. ASP.NET Core MVC model bağlama işlevleri varsayılan MVC 5 benzer şekilde çalışır. Uyumluluk dolgusu değişiklikleri Web API 2 model bağlama kurallarına daha benzer şekilde bağlama model. Örneğin, karmaşık türler isteği gövdesinden otomatik olarak bağlanır.
* Denetleyici eylemleri türünde parametre yararlanabilmeniz model bağlama genişletir `HttpRequestMessage`.
* Eylemler izin verme iletisi biçimlendiricileri'türünde sonuçlar döndürecek şekilde ekler `HttpResponseMessage`.
* Web API 2 Eylemler yanıtları sunmak için kullanılan başka yanıt yöntemleri ekler:
    * Bilgisayarın HttpResponseMessage oluşturucuları:
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * Eylem sonucu yöntemleri:
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* Bir örneğini ekler `IContentNegotiator` uygulamanın dı kapsayıcısı ve yapar içerik anlaşması ile ilgili türlerinden [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kullanılabilir. Bu gibi türlerini içerir `DefaultContentNegotiator`, `MediaTypeFormatter`vb.

Uyumluluk dolgusu kullanmak için aktarmanız gerekir:

* Başvuru [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketi.
* Çağırarak uygulamanın dı kapsayıcı ile uyumluluk dolgusu ait Hizmetleri kaydedin `services.AddWebApiConventions()` uygulamanın içinde `Startup.ConfigureServices` yöntemi.
* Web API özel yollar kullanılarak tanımlamak `MapWebApiRoute` üzerinde `IRouteBuilder` uygulamanın içinde `IApplicationBuilder.UseMvc` çağırın.

## <a name="summary"></a>Özet

Basit bir ASP.NET Web API projesi ASP.NET Core MVC geçirilmesi thanks ASP.NET Core MVC Web API'leri için yerleşik destek için oldukça kolay değildir. Her ASP.NET Web API projesi geçiş yapmanız gerekecektir ana parça rotalara, denetleyicilere ve modeller, güncelleştirmeleri denetleyicileri ve eylemleri tarafından kullanılan türleri ile birlikte ' dir.
