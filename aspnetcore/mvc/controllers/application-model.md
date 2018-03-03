---
title: "Uygulama modeli ile çalışma"
author: ardalis
description: 
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/controllers/application-model
ms.openlocfilehash: 89a7af0ff95754f036b027aeafb8e25e49f397e2
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
---
# <a name="working-with-the-application-model"></a>Uygulama modeli ile çalışma

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC tanımlayan bir *uygulama modeli* bir MVC uygulamanın bileşenleri temsil eden. Okuma ve MVC öğeleri nasıl davranacağını değiştirmek için Bu modelle. Varsayılan olarak, MVC parametreleri ve yönlendirme nasıl davranacağını hangi sınıfların denetleyicileri olduğu kabul edilir ve bu sınıfların hangi yöntemlere eylemlerdir belirlemek için belirli kuralları izler. Bu davranış, kendi kuralları oluşturarak ve bunları genel olarak veya öznitelik olarak uygulayarak, uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz.

## <a name="models-and-providers"></a>Modelleri ve sağlayıcıları

ASP.NET Core MVC uygulama modeli soyut arabirimleri ve bir MVC uygulaması açıklamak somut uygulama sınıfları içerir. Bu model uygulamanın denetleyicileri, Eylemler, eylem parametrelerini, yolları ve varsayılan kuralları göre filtreler keşfetme MVC sonucudur. Uygulama modeli çalışarak, uygulamanızın farklı kuralarına varsayılan MVC davranışı değiştirebilirsiniz. Parametreler, adları, yollar ve filtreleri tüm eylemlerin ve denetleyicilerin için yapılandırma verileri olarak kullanılır.

ASP.NET Core MVC uygulama modeli aşağıdaki yapıya sahiptir:

* ApplicationModel
    * Denetleyicileri (ControllerModel)
        * Eylemler (ActionModel)
            * Parametreler (ParameterModel)

Ortak bir modelin her düzeyi erişimi `Properties` koleksiyonu ve alt düzey erişmek ve hiyerarşideki üst düzeyler tarafından ayarlanan özellik değerlerini üzerine yazın. Özellikler için kalıcı `ActionDescriptor.Properties` Eylemler ne zaman oluşturulur. Bir istek işlenirken, ardından herhangi bir özellik eklenemez veya değiştirilemez bir kuralı aracılığıyla erişilebilir `ActionContext.ActionDescriptor.Properties`. Özellikleri kullanarak, filtreleri, model bağlayıcıları, vb. bir eylem başına temelinde yapılandırmak için harika bir yoludur.

> [!NOTE]
> `ActionDescriptor.Properties` Toplama, uygulama başlatma tamamlandıktan sonra iş parçacığı (yazar için) güvenli değil. Kuralları, güvenli bir şekilde veri bu koleksiyona eklemek için en iyi yoludur.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC tarafından tanımlanan bir sağlayıcı desenini kullanarak uygulama modelini yükler [IApplicationModelProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) arabirimi. Bu bölümde bazı nasıl iç uygulama ayrıntılarını kapsayan bu sağlayıcı işlevleri. Bu ileri düzeyde bir konudur - uygulama modeli yararlanan çoğu uygulamaları kuralları ile çalışarak bunu yapmanız gerekir.

Uygulamaları `IApplicationModelProvider` arabirimi "kaydırma" birbirine, her uygulama arama ile `OnProvidersExecuting` göre artan kendi `Order` özelliği. `OnProvidersExecuted` Yöntemi sonra çağrılır ters sırada. Framework birkaç sağlayıcı tanımlar:

İlk (`Order=-1000`):

* [`DefaultApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Sonra (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Hangi iki sağlayıcıları için aynı değeri ile sırayla `Order` denir tanımlanmamış ve sonra bu nedenle dayanıyordu gerekir.

> [!NOTE]
> `IApplicationModelProvider` framework yazarların genişletmek için Gelişmiş bir kavramdır. Genel olarak, uygulama kuralları kullanmanız gerekir ve çerçeveleri sağlayıcıları kullanmanız gerekir. Anahtar fark sağlayıcıları her zaman önce kuralları çalıştırılan.

`DefaultApplicationModelProvider` ASP.NET Core MVC tarafından kullanılan varsayılan davranışlar çoğunu oluşturur. Sorumlulukları içerir:

* Genel filtre bağlamına ekleme
* Bağlamına denetleyicileri ekleme
* Eylem olarak ortak denetleyicisi yöntemler ekleme
* Eylem yöntemi parametrelerine bağlamına ekleme
* Yol ve diğer öznitelikleri uygulama

Bazı yerleşik davranışları tarafından uygulanan `DefaultApplicationModelProvider`. Bu sağlayıcı oluşturmaktan sorumlu [ `ControllerModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), sırayla başvuran [ `ActionModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), ve [ `ParameterModel` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) örnekleri. `DefaultApplicationModelProvider` Ve gelecekte değiştirecek bir iç framework uygulaması ayrıntı bir sınıftır. 

`AuthorizationApplicationModelProvider` İlişkili davranışı uygulamak için sorumlu `AuthorizeFilter` ve `AllowAnonymousFilter` öznitelikleri. [Bu öznitelikler hakkında daha fazla bilgi](xref:security/authorization/simple).

`CorsApplicationModelProvider` İlişkili davranışı uygulayan `IEnableCorsAttribute` ve `IDisableCorsAttribute`ve `DisableCorsAuthorizationFilter`. [CORS hakkında daha fazla bilgi](xref:security/cors).

## <a name="conventions"></a>Kurallar

Uygulama modeli tüm model veya sağlayıcısı geçersiz kılma daha modelleri davranışını özelleştirmek için basit bir yol sağlamak kuralı soyutlamalar tanımlar. Bu soyutlama, uygulamanızın davranışını değiştirmek için önerilen yoldur. Kuralları özelleştirmeleri dinamik olarak uygulanacak kod yazmak bir yol sağlar. Sırada [filtreleri](xref:mvc/controllers/filters) framework'ün davranışını değiştirme sağladıkları, özelleştirmeleri nasıl tüm uygulama birlikte kablolu denetlemenize olanak tanır.

Aşağıdaki kuralları kullanılabilir:

* [`IApplicationModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

MVC seçenekler ekleyerek veya uygulama kuralları uygulanır `Attribute`s ve bunları denetleyicileri, Eylemler veya eylem parametrelerini uygulayarak (benzer şekilde [ `Filters` ](xref:mvc/controllers/filters)). Uygulama başlıyor, her isteğin bir parçası değil, filtreleri farklı olarak, yalnızca kuralları çalıştırılır.

### <a name="sample-modifying-the-applicationmodel"></a>Örnek: ApplicationModel değiştirme

Aşağıdaki kural, bir özellik için uygulama modeli eklemek için kullanılır. 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Uygulama modeli kuralları, seçeneği olarak uygulanır, MVC eklendiğinde `ConfigureServices` içinde `Startup`.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Özellikler erişilebilir `ActionDescriptor` özellikler koleksiyonu içinde denetleyici eylemleri:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Örnek: ControllerModel açıklama değiştirme

Önceki örnekte olduğu gibi denetleyici modeline ayrıca özel özellikleri eklemek için değiştirilebilir. Bunlar mevcut özelliklerini uygulama modelde belirtilen aynı adda geçersiz kılar. Aşağıdaki kural öznitelik denetleyici düzeyinde bir açıklama ekler:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Bu kural, bir denetleyici özniteliği olarak uygulanır.

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

Önceki örneklerde olduğu gibi aynı şekilde "Açıklama" özelliği erişilebilir.

### <a name="sample-modifying-the-actionmodel-description"></a>Örnek: ActionModel açıklama değiştirme

Ayrı özniteliği kuralı ayrı Eylemler, uygulama veya denetleyici düzeyinde uygulanmış davranışı geçersiz kılma uygulanabilir.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Bu eylem önceki örnekteki denetleyicisi içinde uygulanan nasıl denetleyici düzeyinde kuralı geçersiz kılmaları gösterir:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Örnek: ParameterModel değiştirme

Aşağıdaki kural değiştirmek için eylem parametrelerini uygulanabilir kendi `BindingInfo`. Aşağıdaki kural parametresi bir rota parametresini olmalıdır; diğer olası bağlama kaynakları (örneğin, sorgu dizesi değerlerini) göz ardı edilir.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Öznitelik için herhangi bir eylem parametre uygulanabilir:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Örnek: ActionModel adını değiştirme

Aşağıdaki kural değiştirir `ActionModel` güncelleştirmek için *adı* onu Uygulanacağı eylem. Yeni bir ad özniteliği için bir parametre olarak sağlanır. Bu eylem yöntemine erişmek için kullanılan rota etkileyecek şekilde bu yeni ad yönlendirme tarafından kullanılır.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Bu öznitelik, bir eylem yöntemine uygulanan `HomeController`:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Yöntem adı olsa bile `SomeName`, özniteliğin yöntem adını kullanarak MVC kuralı geçersiz kılar ve eylem adı ile değiştirir `MyCoolAction`. Bu nedenle, bu eylem erişmek için kullanılan rota olduğu `/Home/MyCoolAction`.

> [!NOTE]
> Bu örnekte temelde aynı yerleşik kullanmakla, [EylemAdı](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.actionnameattribute) özniteliği.

### <a name="sample-custom-routing-convention"></a>Örnek: Özel yönlendirme kuralı

Kullanabileceğiniz bir `IApplicationModelConvention` yönlendirmenin nasıl çalıştığını özelleştirmek için. Örneğin, aşağıdaki kural denetleyicileri ad alanları değiştirerek kendi yollar dahil `.` ile ad alanında `/` rotadaki:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Kuralı, bir başlatma seçeneği olarak eklenir.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Kurallarına ekleyebilirsiniz, [ara yazılımı](xref:fundamentals/middleware/index) erişerek `MvcOptions` kullanma `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

Bu örnek denetleyicisi "Namespace" adına sahip olduğu özniteliği yönlendirme kullanmıyorsanız yolları Bu kuralı uygular. Bu kural aşağıdaki denetleyicisiyle gösterir:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Uygulama modeli WebApiCompatShim kullanımda

ASP.NET Core MVC farklı kümesinden kuralları ASP.NET Web API 2 kullanır. Özel kuralları kullanarak, bir Web API uygulaması ile tutarlı olacak şekilde bir ASP.NET Core MVC uygulamanın davranışı değiştirebilirsiniz. Microsoft gelir [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) özellikle bu amaç için.

> [!NOTE]
> Daha fazla bilgi edinmek [ASP.NET Web API öğesinden geçiş](xref:migration/webapi).

Web API uyumluluk dolgusu kullanmak için paket projenize ekleyin ve ardından kuralları için MVC çağırarak eklemeniz gerekir `AddWebApiConventions` içinde `Startup`:

```c#
services.AddMvc().AddWebApiConventions();
```

Dolgu tarafından sağlanan kuralları yalnızca uygulanmış belirli öznitelikler beklendiğinden uygulama bölümlerini uygulanır. Aşağıdaki dört öznitelikler denetleyicileri dolgu 's kuralları tarafından değiştirilmiş kendi kurallarına sahip denetlemek için kullanılır:

* [UseWebApiActionConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Eylem kuralları

`UseWebApiActionConventionsAttribute` Kendi adına göre eylemler için HTTP yöntemini eşlemek için kullanılır (örneğin, `Get` için eşleyen `HttpGet`). Yalnızca öznitelik yönlendirmeyi kullanmayan eylemleri için de geçerlidir.

### <a name="overloading"></a>Aşırı Yükleme

`UseWebApiOverloadingAttribute` Uygulamak için kullanılan `WebApiOverloadingApplicationModelConvention` kuralı. Bu kuralı ekler bir `OverloadActionConstraint` eylem seçimi işlemi için hangi sınırlar bu isteği karşılayan tüm isteğe bağlı olmayan parametreler için aday eylemler.

### <a name="parameter-conventions"></a>Parametre kuralları

`UseWebApiParameterConventionsAttribute` Uygulamak için kullanılan `WebApiParameterConventionsApplicationModelConvention` Eylem kuralı. Bu kural eylemi parametre olarak kullanılan basit türler karmaşık türler isteği gövdesinden bağlı sırasında varsayılan olarak, URI'den ilişkili belirtir.

### <a name="routes"></a>Yollar

`UseWebApiRoutesAttribute` Denetimleri olup olmadığını `WebApiApplicationModelConvention` denetleyicisi kuralı uygulanır. Etkinleştirildiğinde, bu kural için destek eklemek için kullanılan [alanları](xref:mvc/controllers/areas) rota.

Uyumluluk Paketi kuralları kümesi yanı sıra içerir bir `System.Web.Http.ApiController` temel Web API'si tarafından sağlanan yerini sınıfı. Web API ve devralma için yazılmış denetleyicilerinizi böylece gelen kendi `ApiController` bunlar, ASP.NET Core MVC çalıştırılırken tasarlanan şekilde çalışması için. Bu temel denetleyici sınıfı tüm ile donatılmış `UseWebApi*` yukarıda listelenen öznitelikleri. `ApiController` Özellikleri, yöntemleri ve Web API'si bulunanlar uyumlu sonuç türleri kullanıma sunar.

## <a name="using-apiexplorer-to-document-your-app"></a>Uygulamanızı belge için ApiExplorer kullanma

Uygulama modeli kullanıma sunan bir [ `ApiExplorer` ](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) her düzeyde uygulamanın yapısı geçiş yapmak için kullanılan özellik. Bunun için kullanılabilir [Swagger gibi araçları kullanarak, Web API'leri için Yardım sayfalarına üret](https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger). `ApiExplorer` Özelliği düzenlemenizi sağlayan bir `IsVisible` uygulamanızın modeli hangi kısımlarının açılmamalıdır belirtmek için ayarlanabilir özelliği. Bir kural kullanarak bu ayarı yapılandırabilirsiniz:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Bu yaklaşım (ve gerekirse ek kuralları) kullanarak etkinleştirin veya uygulamanızda herhangi bir düzeyde API görünürlük devre dışı bırakın. 
