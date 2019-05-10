---
title: ASP.NET Core uygulama modeli ile çalışma
author: ardalis
description: Okuma ve nasıl ASP.NET Core MVC öğeleri davranacağını değiştirmek için uygulama modeli ile düzenleme hakkında bilgi edinin.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/application-model
ms.openlocfilehash: 6b0591a877c0d82e0ee6ab002eb6a6650753677b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903179"
---
# <a name="work-with-the-application-model-in-aspnet-core"></a>ASP.NET Core uygulama modeli ile çalışma

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core MVC tanımlayan bir *uygulama modeli* bir MVC uygulaması bileşenleri temsil eden. Okuma ve MVC öğeleri nasıl davranacağını değiştirmek için Bu modelle. Varsayılan olarak, MVC denetleyicileri için hangi sınıflar olarak kabul edilir, bu sınıflar hangi yöntemlerin eylemlerdir ve parametreleri ve yönlendirme nasıl davranacağını belirlemek için belirli kuralları izler. Bu davranış, kendi kuralları oluşturarak ve bunları genel olarak veya öznitelikleri uygulayarak, uygulamanızın ihtiyaçlarına uyacak şekilde özelleştirebilirsiniz.

## <a name="models-and-providers"></a>Modelleri ve sağlayıcıları

ASP.NET Core MVC uygulama modeli, hem soyut arabirimleri hem de bir MVC uygulaması açıklayan somut uygulama sınıfları içerir. Bu model, MVC uygulamanın denetleyicileri, Eylemler, eylem parametrelerini, yollar ve varsayılan kuralları göre filtreler keşfetme sonucudur. Uygulama modeli ile birlikte çalışarak, varsayılan MVC davranış farklı kurallarına uygun şekilde değiştirebilirsiniz. Parametreler, adları, yollar ve filtreler tüm eylemlerin ve denetleyicilerin için yapılandırma verilerini kullanılır.

ASP.NET Core MVC uygulama modeli aşağıdaki yapıya sahiptir:

* ApplicationModel
  * Denetleyicileri (ControllerModel)
    * Eylemler (ActionModel)
      * Parametreler (ParameterModel)

Modelin her düzeyi bir ortak erişimi `Properties` koleksiyonu ve alt düzey erişebilir ve hiyerarşideki üst düzey tarafından ayarlanan özellik değerlerini üzerine yazın. Özellikler için kalıcı `ActionDescriptor.Properties` eylemleri oluşturduğunuzda. İstek işlenirken, ardından herhangi bir özellik eklendiğinde veya değiştirildiğinde bir kural aracılığıyla erişilebilir `ActionContext.ActionDescriptor.Properties`. Özellikleri kullanarak, bir eylem başına temelinde filtreleri, model bağlayıcıları vb. yapılandırmak için harika bir yoludur.

> [!NOTE]
> `ActionDescriptor.Properties` Koleksiyonu uygulama başlatma tamamlandıktan sonra iş parçacığı (yazma) güvenli değildir. Kuralları, güvenli bir şekilde veri bu koleksiyona eklemek için en iyi yoludur.

### <a name="iapplicationmodelprovider"></a>IApplicationModelProvider

ASP.NET Core MVC tarafından tanımlanan bir sağlayıcı desenini kullanarak uygulama modelini yükler [IApplicationModelProvider](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelprovider) arabirimi. Bu bölümde bazı nasıl iç uygulama ayrıntılarını kapsayan bu sağlayıcı işlevleri. Bu gelişmiş bir konudur - uygulama modeli yararlanan uygulamaların çoğu kuralları ile birlikte çalışarak bunu yapmanız gerekir.

Uygulamaları `IApplicationModelProvider` arabirimi "kaydırma", her uygulama arama ile `OnProvidersExecuting` göre artan kendi `Order` özelliği. `OnProvidersExecuted` Yöntemi ardından çağrılır ters sırada. Framework birkaç sağlayıcısı tanımlar:

İlk (`Order=-1000`):

* [`DefaultApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.defaultapplicationmodelprovider)

Ardından (`Order=-990`):

* [`AuthorizationApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.internal.authorizationapplicationmodelprovider)
* [`CorsApplicationModelProvider`](/dotnet/api/microsoft.aspnetcore.mvc.cors.internal.corsapplicationmodelprovider)

> [!NOTE]
> Hangi iki sağlayıcıları için aynı değeri ile sırayla `Order` adlandırılır tanımlı değildir ve bu nedenle üzerine dayanan olmamalıdır.

> [!NOTE]
> `IApplicationModelProvider` genişletmek framework yazarları için Gelişmiş bir kavramdır. Genel olarak, uygulamaların kurallarını kullanması gerekir ve çerçeveleri sağlayıcıları kullanmanız gerekir. Anahtar ayrımı sağlayıcıları her zaman önce kuralları çalışmasıdır.

`DefaultApplicationModelProvider` ASP.NET Core MVC tarafından kullanılan varsayılan davranışlar birçoğu oluşturur. Sorumlulukları şunlardır:

* Genel filtrelerin bağlamına ekleme
* Denetleyicileri için içerik ekleme
* Eylem olarak genel denetleyicisi yöntemler ekleme
* Eylem yöntemi parametrelerine bağlamına ekleme
* Yol ve diğer özniteliklerini uygulama

Bazı yerleşik davranışları tarafından uygulanan `DefaultApplicationModelProvider`. Bu sağlayıcı oluşturmaktan sorumlu [ `ControllerModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.controllermodel), sırayla başvuran [ `ActionModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.actionmodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ActionModel), [ `PropertyModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.propertymodel), ve [ `ParameterModel` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.parametermodel#Microsoft_AspNetCore_Mvc_ApplicationModels_ParameterModel) örnekleri. `DefaultApplicationModelProvider` Sınıfı, bir iç uygulama ayrıntısı ve gelecekte değişecektir. 

`AuthorizationApplicationModelProvider` İle ilişkili davranışı uygulamak için sorumlu `AuthorizeFilter` ve `AllowAnonymousFilter` öznitelikleri. [Bu öznitelikler hakkında daha fazla bilgi](xref:security/authorization/simple).

`CorsApplicationModelProvider` İlişkili davranışı uyguladığında `IEnableCorsAttribute` ve `IDisableCorsAttribute`ve `DisableCorsAuthorizationFilter`. [CORS hakkında daha fazla bilgi](xref:security/cors).

## <a name="conventions"></a>Kurallar

Uygulama modeli modelin tamamı veya sağlayıcı geçersiz kılma daha modelleri davranışını özelleştirmek için daha basit bir yol sağlayan kuralı soyutlama tanımlar. Bu soyutlama, uygulamanızın davranışını değiştirmek için önerilen yoldur. Kuralları özelleştirmeleri dinamik olarak geçerli kod yazmak bir yol sağlar. Sırada [filtreleri](xref:mvc/controllers/filters) framework'ün davranışı değiştirmenin bir yol sağlar, özelleştirmeler nasıl tüm uygulama birlikte kablolu denetlemenize olanak tanır.

Aşağıdaki yöntemleri kullanılabilir:

* [`IApplicationModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iapplicationmodelconvention)
* [`IControllerModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.icontrollermodelconvention)
* [`IActionModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iactionmodelconvention)
* [`IParameterModelConvention`](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.iparametermodelconvention)

MVC seçenekler ekleyerek veya uygulama kuralları uygulanır `Attribute`s ve denetleyicileri, Eylemler veya eylem parametrelerini uygulamadan (benzer şekilde [ `Filters` ](xref:mvc/controllers/filters)). Uygulama başlıyor, her isteğin bir parçası olarak değil, filtreleri yalnızca kuralları çalıştırılır.

### <a name="sample-modifying-the-applicationmodel"></a>Örnek: ApplicationModel değiştirme

Aşağıdaki kural, uygulama modeli için bir özellik eklemek için kullanılır. 

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ApplicationDescription.cs)]

Uygulama modeli kuralları, seçenek olarak uygulanır, MVC eklendiğinde `ConfigureServices` içinde `Startup`.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=5)]

Özellikleri erişilebilir `ActionDescriptor` özellikler koleksiyonu içinde denetleyici eylemleri:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/AppModelController.cs?name=AppModelController)]

### <a name="sample-modifying-the-controllermodel-description"></a>Örnek: ControllerModel açıklamasını değiştirme

Önceki örnekte olduğu gibi denetleyicisi modeli ayrıca özel özellikler eklemek için değiştirilebilir. Bu uygulama modeli belirtilen aynı ada sahip mevcut özellikleri kılar. Aşağıdaki kural öznitelik denetleyici düzeyinde bir açıklama ekler:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ControllerDescriptionAttribute.cs)]

Bu kural, bir denetleyici özniteliği olarak uygulanır.

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=ControllerDescription&highlight=1)]

"Description" özelliği, önceki örneklerde olduğu gibi aynı şekilde erişilir.

### <a name="sample-modifying-the-actionmodel-description"></a>Örnek: ActionModel açıklamasını değiştirme

Ayrı özniteliği kural, uygulama veya denetleyici düzeyinde uygulanmış davranışı geçersiz kılma bireysel işlemlere uygulanabilir.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/ActionDescriptionAttribute.cs)]

Bu eylem önceki örnekteki denetleyici içinde uygulanan nasıl denetleyici düzeyinde kuralı geçersiz kılmaları gösterir:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/DescriptionAttributesController.cs?name=DescriptionAttributesController&highlight=9)]

### <a name="sample-modifying-the-parametermodel"></a>Örnek: ParameterModel değiştirme

Eylem parametreleri değiştirmek için aşağıdaki kuralı uygulanabilir kendi `BindingInfo`. Aşağıdaki kuralı parametresi bir rota parametresini olmasını gerektirir; diğer olası bağlama kaynakları (örneğin, sorgu dizesi değerlerini) göz ardı edilir.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/MustBeInRouteParameterModelConvention.cs)]

Öznitelik, herhangi bir eylem parametre uygulanabilir:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/ParameterModelController.cs?name=ParameterModelController&highlight=5)]

### <a name="sample-modifying-the-actionmodel-name"></a>Örnek: ActionModel adı değiştirme

Aşağıdaki kural değiştirir `ActionModel` güncelleştirilecek *adı* , uygulandığı eylem. Yeni bir ad, öznitelik bir parametre olarak sağlanır. Bu eylem yöntemine erişmek için kullanılan rota etkileyecek şekilde yönlendirerek, bu yeni adı kullanılır.

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/CustomActionNameAttribute.cs)]

Bu öznitelik bir eylem yöntemine uygulanan `HomeController`:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/HomeController.cs?name=ActionModelConvention&highlight=2)]

Yöntem adı olsa bile `SomeName`, özniteliğin yöntem adını kullanarak MVC kuralını geçersiz kılar ve eylem adıyla değiştirir `MyCoolAction`. Bu nedenle, bu eylem erişmek için kullanılan rota olan `/Home/MyCoolAction`.

> [!NOTE]
> Bu örnekte temelde yerleşik kullanarak aynıdır [ActionName](/dotnet/api/microsoft.aspnetcore.mvc.actionnameattribute) özniteliği.

### <a name="sample-custom-routing-convention"></a>Örnek: Özel rota kuralları örneği

Kullanabileceğiniz bir `IApplicationModelConvention` yönlendirmenin nasıl çalıştığını özelleştirmek için. Örneğin, aşağıdaki kuralı denetleyicileri ad alanları değiştirerek kendi yolları birleştirecektir `.` ad ile `/` rotadaki:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/NamespaceRoutingConvention.cs)]

Kuralı, bir başlatma seçeneği olarak eklenir.

[!code-csharp[](./application-model/sample/src/AppModelSample/Startup.cs?name=ConfigureServices&highlight=6)]

> [!TIP]
> Kurallarına ekleyebilirsiniz, [ara yazılım](xref:fundamentals/middleware/index) erişerek `MvcOptions` kullanma `services.Configure<MvcOptions>(c => c.Conventions.Add(YOURCONVENTION));`

Bu örnek özniteliği denetleyiciye "Namespace" adına sahip olduğu yönlendirmeyi kullanmayan yolları Bu kuralı uygular. Bu kural aşağıdaki denetleyicisi gösterir:

[!code-csharp[](./application-model/sample/src/AppModelSample/Controllers/NamespaceRoutingController.cs?highlight=7-8)]

## <a name="application-model-usage-in-webapicompatshim"></a>Uygulama modeli WebApiCompatShim kullanımı

ASP.NET Core MVC, ASP.NET Web API 2'den farklı kuralları kümesi kullanır. Özel kuralları kullanarak, bir Web API'sini uygulama ile tutarlı olacak şekilde bir ASP.NET Core MVC uygulamanın davranışını değiştirebilirsiniz. Microsoft verilir [WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim/) özellikle bu amaç için.

> [!NOTE]
> Daha fazla bilgi edinin [ASP.NET Web API geçişini](xref:migration/webapi).

Web API Uyumluluk dolgu kullanmak üzere, paketi projenize ekleyin ve ardından kuralları için MVC çağırarak eklemeniz gerekir `AddWebApiConventions` içinde `Startup`:

```csharp
services.AddMvc().AddWebApiConventions();
```

Dolgu tarafından sağlanan kuralları, yalnızca belirli öznitelikleri uygulanmış olan uygulama bölümlerine uygulanır. Aşağıdaki dört öznitelikleri denetleyicileri dolgu'nın kuralları tarafından değiştirilmiş kendi kurallarına sahip denetlemek için kullanılır:

* [UseWebApiActionConventionsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiactionconventionsattribute)
* [UseWebApiOverloadingAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapioverloadingattribute)
* [UseWebApiParameterConventionsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiparameterconventionsattribute)
* [UseWebApiRoutesAttribute](/dotnet/api/microsoft.aspnetcore.mvc.webapicompatshim.usewebapiroutesattribute)

### <a name="action-conventions"></a>Eylem kuralları

`UseWebApiActionConventionsAttribute` Eylemlerine kendi adına göre HTTP yöntemi eşleştirmek için kullanılır (örneğin, `Get` eşlemek `HttpGet`). Yalnızca öznitelik yönlendirme kullanmayan eylemleri için de geçerlidir.

### <a name="overloading"></a>Aşırı Yükleme

`UseWebApiOverloadingAttribute` Uygulamak için kullanılan `WebApiOverloadingApplicationModelConvention` kuralı. Bu kuralı ekler bir `OverloadActionConstraint` eylem seçim işlemi için bu isteği karşılayan tüm isteğe bağlı olmayan parametreler için aday eylemleri sınırlar.

### <a name="parameter-conventions"></a>Parametre kuralları

`UseWebApiParameterConventionsAttribute` Uygulamak için kullanılan `WebApiParameterConventionsApplicationModelConvention` eylem yöntemi. Bu kural, eylem parametre olarak kullanılan basit türler karmaşık türler gövdeden bağlı sırasında varsayılan olarak, URI'SİNDEN ilişkili belirtir.

### <a name="routes"></a>Yollar

`UseWebApiRoutesAttribute` Denetimleri olmadığını `WebApiApplicationModelConvention` denetleyicisi kuralı uygulanır. Etkin olduğunda, bu kuralı için destek eklemek için kullanılan [alanları](xref:mvc/controllers/areas) yönlendirmek için.

Birtakım kuralları ek olarak, Uyumluluk Paketi içeren bir `System.Web.Http.ApiController` temel Web API'si tarafından sağlanan yerini sınıfı. Web API ve devralma için yazılan denetleyicilerinizi böylece gelen kendi `ApiController` bunlar, ASP.NET Core MVC çalıştırılırken tasarlanmış şekilde çalışması için. Bu temel denetleyici sınıfı tüm ile donatılmış `UseWebApi*` yukarıda listelenen öznitelikleri. `ApiController` Özellikleri, yöntemleri ve Web API'SİNDE bulunanlar ile uyumlu olan sonuç türleri kullanıma sunar.

## <a name="using-apiexplorer-to-document-your-app"></a>Uygulamanızı belge ApiExplorer kullanma

Uygulama modeli kullanıma sunan bir [ `ApiExplorer` ](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.apiexplorermodel) her düzeyde uygulamanın yapısını geçirmek için kullanılan özellik. Bunu yapmak için kullanılabilir [Swagger gibi araçlar kullanarak Web API'leri için Yardım sayfaları oluşturun](xref:tutorials/web-api-help-pages-using-swagger). `ApiExplorer` Özelliği kullanıma sunan bir `IsVisible` uygulamanızın modeli hangi parçalarının açılmamalıdır belirtmek için ayarlanabilir özelliği. Bir kural kullanarak bu ayarı yapılandırabilirsiniz:

[!code-csharp[](./application-model/sample/src/AppModelSample/Conventions/EnableApiExplorerApplicationConvention.cs)]

Bu yaklaşım (ve gerekirse ek kuralları) kullanarak etkinleştirin veya uygulamanızda herhangi bir düzeyde API görünürlük devre dışı bırakın. 
