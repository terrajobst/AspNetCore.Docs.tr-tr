---
title: ASP.NET Web API ASP.NET Core için geçirme
author: ardalis
description: Bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API'si geçirmeyi öğrenin.
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894198"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>ASP.NET Web API ASP.NET Core için geçirme

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)

Web istemcileri, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP Hizmetleri apı'lerdir. ASP.NET Core MVC web uygulamaları oluşturmanın tek, tutarlı bir yol sağlayan Web API'leri oluşturmaya yönelik destek içerir. Bu makalede, bir Web API uygulaması için ASP.NET Core MVC ASP.NET Web API'si geçirmek için gereken adımları size gösterir.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>ASP.NET Web API projesi gözden geçirme

Bu makalede örnek proje kullanan *ProductsApp*, makaledeki oluşturulmuş [ASP.NET Web API 2 ile çalışmaya başlama](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) , başlangıç noktası olarak. Projede, basit bir ASP.NET Web API projesi şu şekilde yapılandırılmıştır.

İçinde *Global.asax.cs*, için bir çağrı yapılır `WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` tanımlanan *App_Start*, ve yalnızca bir statik `Register` yöntemi:

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

Bu sınıf yapılandırır [öznitelik yönlendirme](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), ancak aslında projede kullanılıyor. Ayrıca, ASP.NET Web API'si tarafından kullanılan yönlendirme tablosunun yapılandırır. Bu durumda, ASP.NET Web API biçim ile eşleşmesi için URL'leri beklediği */api/ {denetleyici} / {id}*, ile *{id}* isteğe bağlı olan.

*ProductsApp* Proje öğesinden devralan tek bir basit denetleyicisi içeren `ApiController` ve iki yöntem sunar:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

Son olarak, model *ürün*tarafından kullanılan *ProductsApp*, basit bir sınıf:

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

Basit bir proje başlayacağı sahibiz, biz bu Web API projesi için ASP.NET Core MVC geçirme gösterebilirsiniz.

## <a name="create-the-destination-project"></a>Hedef proje oluşturma

Visual Studio kullanarak, yeni, boş bir çözüm oluşturun ve adlandırın *WebAPIMigration*. Mevcut yapıtı Ekle *ProductsApp* proje kendisine ve ardından, yeni bir ASP.NET Core Web uygulaması projesi çözüme ekleyin. Yeni proje adını *ProductsCore*.

![Yeni Proje iletişim kutusu açmak için Web Şablonları](webapi/_static/add-web-project.png)

Ardından, Web API proje şablonunu seçin. Taşımayı planlıyoruz *ProductsApp* içerikleri bu yeni proje için.

![ASP.NET Core şablonları listesinde seçilen Web API proje şablonuyla yeni Web uygulaması iletişim kutusu](webapi/_static/aspnet-5-webapi.png)

Silme `Project_Readme.html` dosyasından yeni bir proje. Çözümünüz şöyle görünmelidir:

![Dosya ve klasörleri ProductsApp ve ProductsCore projelerin gösteren Çözüm Gezgini içinde uygulama çözümü açın](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>Yapılandırma geçişi

ASP.NET Core bundan böyle *Global.asax*, *web.config*, veya *App_Start* klasörleri. Bunun yerine, tüm başlangıç görevleri gerçekleştirilir *Startup.cs* proje kökündeki (bkz [uygulama başlatma](../fundamentals/startup.md)). ASP.NET Core MVC, öznitelik tabanlı yönlendirme artık varsayılan olarak dahil edilir olduğunda `UseMvc()` çağrılır; ve bu Web API yolları yapılandırmak için önerilen bir yaklaşım (ve üretim Web API'si başlangıç projesini nasıl işleyeceğini).

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

İleriye dönük projenizde öznitelik yönlendirme kullanmak istediğiniz varsayılarak, ek bir yapılandırma gerekmez. Aşağıdaki örnekte olduğu gibi denetleyicileri ve eylemleri gerektiği gibi öznitelikleri yalnızca uygulanır `ValuesController` Web API'si başlangıç projesine dahil sınıfı:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

Varlığını Not *[controller]* 8. satırda. Öznitelik tabanlı artık yönlendirmeyi destekleyen bazı belirteçler gibi *[controller]* ve *[action]*. Bu belirteçler çalışma zamanında denetleyici veya eylemin, adıyla sırasıyla özniteliği uygulanmış değiştirilir. Bu projedeki Sihirli dize sayısını azaltmak için kullanılır ve otomatik yeniden adlandır yeniden düzenlemeler uygulandığında yolların kendi ilgili denetleyicileri ve eylemleri ile eşitlenmiş tutulacak sağlanır.

Ürünleri API denetleyicisi geçirmek için biz öncelikle kopyalamalısınız *ProductsController* yeni projeye. Ardından yalnızca denetleyici üzerinde rota özniteliğinin şunları içerir:

```csharp
[Route("api/[controller]")]
```

Ayrıca eklemenize gerek `[HttpGet]` her ikisi de HTTP Get çağrılmalıdır beri iki yöntem için özniteliği. Özniteliği için bir "ID" parametresi beklentisi dahil `GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

Bu noktada, yönlendirme düzgün yapılandırılıp yapılandırılmadığını; Ancak, henüz bunu test edilemez. Ek değişiklik yaptıysanız, önce *ProductsController* derlenir.

## <a name="migrate-models-and-controllers"></a>Modelleri ve denetleyicileri geçirme

Bu basit bir Web API projesi için geçiş işlemi son adımda denetleyicileri ve kullandıkları herhangi bir model kopyalamaktır. Bu durumda, sadece kopyalayın *Controllers/ProductsController.cs* özgün proje için yeni bir tane. Ardından, tüm modeller klasörü özgün projeden yeni bir tane kopyalayın. Yeni Proje adıyla eşleşecek şekilde ad alanlarını Ayarla (*ProductsCore*).  Bu noktada, uygulama oluşturabilir ve derleme hatalarının sayısını görebilirsiniz. Bunlar, genellikle aşağıdaki kategorilere ayrılır:

* *ApiController* yok

* *System.Web.Http* ad alanı mevcut değil

* *Ihttpactionresult* yok

Neyse ki, bunları düzeltmek tüm çok kolaydır:

* Değişiklik *ApiController* için *denetleyicisi* (eklemeniz gerekebilir *Microsoft.AspNetCore.Mvc kullanarak*)

* Silmek başvuran deyimini *System.Web.Http*

* Döndüren bir yöntemi değiştirme *Ihttpactionresult* döndürülecek bir *IActionResult*

Bu değişiklikler yapılmış ve kullanılmayan ulaştıktan sonra using deyimlerini kaldırıldıysa, geçirilen *ProductsController* sınıfı aşağıdaki gibi görünür:

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

Artık geçirilen projeyi çalıştırın ve göz atın olmalıdır */api/ürünleri*; ve 3 ürünlerin tam listesini görmeniz gerekir. Gözat */api/products/1* ve ilk ürün görmeniz gerekir.

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>ASP.NET 4.x Web API 2 uyumluluk dolgusu

Geçirme ASP.NET Web API ASP.NET Core projelerinde, kullanışlı bir araçtır [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) kitaplığı. ASP.NET Core, kullanılacak farklı bir Web API 2 kuralları sayısı izin vermek için Uyumluluk dolgu genişletir. Bu belgede daha önce unity'nin örnek uyumluluk dolgu gerekli olmadığını yeterince temel üyeliktir. Daha büyük projeler için Uyumluluk dolgu kullanarak geçici olarak ASP.NET Web API 2 ile ASP.NET Core arasındaki API açığını köprüleme yararlı olabilir.

Web API Uyumluluk dolgu geçirme büyük Web API projelerini ASP.NET core'a kolaylaştırmak için geçici bir önlem kullanılmak üzere tasarlanmıştır. Zaman içinde projeler üzerinde uyumluluğu dolgu güvenmek yerine, ASP.NET Core düzenlerinin kullanılacağı güncelleştirilmelidir.

Uyumluluk özellikleri dahil `Microsoft.AspNetCore.Mvc.WebApiCompatShim` içerir:

* Ekler bir `ApiController` denetleyicileri temel türleri güncelleştirilmesi gerekmeyen yazın.
* Web API stili model bağlama sağlar. ASP.NET Core MVC model bağlama işlevleri varsayılan olarak, MVC 5 benzer şekilde çalışır. Uyumluluk dolgu değişiklikleri model daha Web API 2 model bağlama kurallarına benzer şekilde bağlama. Örneğin, karmaşık türler gövdeden otomatik olarak bağlanır.
* Denetleyici eylemleri türünde parametre yararlanabilmeniz model bağlama genişletir `HttpRequestMessage`.
* İleti biçimlendiriciler eylemleri izin verme türü sonuçları döndürmek için ekler `HttpResponseMessage`.
* Web API 2 eylemleri yanıtlar verecek kullanmış olabilirsiniz ek yanıt yöntemleri ekler:
  * HttpResponseMessage oluşturucuları:
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * Eylem sonucu yöntemleri:
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* Bir örneğini ekler `IContentNegotiator` uygulamanın DI kapsayıcıya ve içerik anlaşması ile ilgili türlerinden yapar [System.NET.http.Formatting](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) kullanılabilir. Bu gibi türlerini içerir `DefaultContentNegotiator`, `MediaTypeFormatter`vb..

Uyumluluk dolgu kullanmak üzere için gerekir:

* Yükleme [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet paketi.
* Çağırarak uygulamanın DI kapsayıcı ile uyumluluk dolgu ait Hizmetleri kaydedin `services.AddMvc().AddWebApiConventions()` uygulamasının `Startup.ConfigureServices` yöntemi.
* Web API özel yollar kullanarak tanımlama `MapWebApiRoute` üzerinde `IRouteBuilder` uygulamasının `IApplicationBuilder.UseMvc` çağırın.

## <a name="summary"></a>Özet

ASP.NET Core MVC için basit bir ASP.NET Web API projesi geçirme yararlanabilirsiniz ASP.NET Core MVC Web API'leri için yerleşik destek oldukça basittir. Her bir ASP.NET Web API projesi geçiş yapmanız gerekecektir ana parçaları rotalara, denetleyicilere ve modeller, denetleyicilere ve eylemlere tarafından kullanılan türleri güncelleştirmeleri ile birlikte ' dir.
