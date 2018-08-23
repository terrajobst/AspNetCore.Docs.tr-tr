---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API bağlama parametresi | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/11/2013
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d29f087cd658faf1fadb0d9a85e9f32c03a2b3f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757368"
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API bağlama parametresi
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

Web API denetleyicisi üzerinde bir yöntemi çağırdığında, adlı bir işlem parametreleri için değer ayarlamalısınız *bağlama*. Bu makalede, Web API'si parametreleri nasıl bağlar ve bağlama işlemi nasıl özelleştirebileceğiniz açıklanmaktadır.

Varsayılan olarak, Web API'si parametleri bağlamak için aşağıdaki kuralları kullanır:

- "Basit" bir tür parametresi olduğundan, Web API'si URI'SİNDEN değerini almak çalışır. Basit türler .NET [ilkel türler](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **çift**, vb.), artı **TimeSpan**, **DateTime**, **GUID**, **ondalık**, ve **dize**, *artı* herhangi bir dizeden dönüştürebilirsiniz bir tür dönüştürücüsü yazın. (Hakkında daha fazla daha sonra tür dönüştürücüleri.)
- Karmaşık türler, Web API'si çalıştığı için ileti gövdesi okunamadı kullanarak bir [medya türü biçimlendiricisi](media-formatters.md).

Örneğin, tipik bir Web API denetleyicisi yöntemi şu şekildedir:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Kimliği* parametresi bir &quot;basit&quot; yazın, böylece Web API'si istek URI'SİNDEN değeri almaya çalışır. *Öğesi* parametre değeri gövdeden okunacak medya türü biçimlendiricisi Web API'si kullanan karmaşık bir tür olduğundan.

Web API URI'SİNDEN bir değer almak için rota verilerini ve URI sorgu dizesi içinde arar. Yönlendirme sistem URI ayrıştırırken bir rota için eşleşen bir rota verilerini doldurulur. Daha fazla bilgi için [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md).

Bu makalenin kalanında miyim model bağlama işleminden nasıl özelleştirebileceğiniz göstereceğiz. Karmaşık türler için ancak, mümkün olduğunda medya türü biçimlendiricileri kullanmayı düşünün. Bir anahtar HTTP kaynakları kaynağın gösterimini belirtmek için içerik anlaşması kullanılarak ileti gövdesinde gönderildiğinden emin ilkesidir. Medya türü biçimlendiricileri tam olarak bu amaç için tasarlanmıştır.

## <a name="using-fromuri"></a>[FromUri] kullanma

Web API'si, bir karmaşık tür URI'SİNDEN okumaya zorlamak için ekleme **[FromUri]** özniteliği için parametre. Aşağıdaki örnekte tanımlayan bir `GeoPoint` birlikte alır bir denetleyici yöntemi türü `GeoPoint` URI.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

İstemci enlem ve boylam değerleri sorgu dizesinde yerleştirebilir ve Web API bunları oluşturmak için kullanacağı bir `GeoPoint`. Örneğin:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>[FromBody] kullanma

Web API'si, bir basit türü gövdeden okunacak zorlamak için ekleme **[FromBody]** özniteliği için parametre:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

Bu örnekte, değerini okumak için bir medya türü biçimlendiricisi Web API kullanacağı *adı* istek gövdesinden. İşte bir örnek istemci isteği.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Web API'si Content-Type üstbilgisi [FromBody] bir parametreye sahip olduğunda, bir biçimlendirici seçmek için kullanır. Bu örnekte, içerik türü olduğundan &quot;application/json&quot; ve istek gövdesi ham bir JSON dizesi (JSON nesnesi değil).

En fazla bir parametre, ileti gövdeden okuma izin verilmez. Bu nedenle bu çalışmaz:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Bu kural için istek gövdesi yalnızca bir kez okunabilir olmayan arabelleğe alınan bir akış içinde depolanabilir nedenidir.

## <a name="type-converters"></a>Tür dönüştürücüleri

Web API (Web API URI'SİNDEN bağlama dener böylece) bir sınıfı basit bir tür olarak davranmak oluşturarak yapabileceğiniz bir **TypeConverter** ve dize dönüştürme sağlar.

Aşağıdaki kodda gösterildiği bir `GeoPoint` coğrafi bir noktayı temsil eden sınıf yanı sıra **TypeConverter** dönüştüren için dizelerden `GeoPoint` örnekleri. `GeoPoint` Sınıfı ile donatılmış bir **[TypeConverter]** tür dönüştürücüsünü belirtmek için özniteliği. (Bu örnekte Mike kabini'nın blog gönderisi tarafından ilham alan [MVC/Webapı eylem imzaları özel nesneler bağlama nasıl](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Web API değerlendirir artık `GeoPoint` basit bir tür bağlamak çalışır, yani `GeoPoint` URI parametreleri. Eklemeniz gerekmez **[FromUri]** parametresi üzerinde.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

İstemci, şöyle bir URI yöntemi çağırabilirsiniz:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Model bağlayıcıları

Özel bir model bağlayıcısını oluşturmak için bir tür dönüştürücüsü olandan daha esnek bir seçenek. Bir model Bağlayıcısı ile HTTP isteği, eylem açıklaması ve ham değerler gibi rota verileri erişebilirsiniz.

Bir model bağlayıcı oluşturmak için uygulaması **IModelBinder** arabirimi. Bu arabirim, tek bir yöntemi tanımlar **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

Bir model bağlayıcı için işte `GeoPoint` nesneleri.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Ham giriş değerlerini bir model bağlayıcı alır bir *değer sağlayıcısındaki*. Bu tasarım, iki ayrı işlevlerin ayırır:

- Değer sağlayıcı, HTTP isteği alır ve anahtar-değer çiftlerinin dictionary'si doldurur.
- Model bağlayıcı Bu sözlük model doldurmak için kullanır.

Varsayılan değer sağlayıcı Web API'si, rota verilerini ve sorgu dizesi değerlerini alır. Örneğin, URI ise `http://localhost/api/values/1?location=48,-122`, aşağıdaki anahtar-değer çiftleri değer sağlayıcı oluşturur:

- kimlik = &quot;1&quot;
- konumu = &quot;48,122&quot;

(Olan varsayılan rota şablonu varsayılarak &quot;API / {denetleyici} / {id}&quot;.)

Bağlanacak parametre adını depolanan **ModelBindingContext.ModelName** özelliği. Model Bağlayıcısı sözlüğündeki bu değere sahip bir anahtar arar. Değer varsa ve metne dönüştürülecek bir `GeoPoint`, model bağlayıcı ilişkili değeri atar **ModelBindingContext.Model** özelliği.

Model bağlayıcı için bir basit tür dönüştürme sınırlı olmadığına dikkat edin. Bu örnekte, model bağlayıcı önce bir tabloda bilinen konumları arar ve başarısız olursa, tür dönüştürme kullanır.

**Model bağlayıcı ayarlama**

Bir model bağlayıcısını ayarlamak için birkaç yolu vardır. İlk olarak ekleyebileceğiniz bir **[çoğaltan ModelBinder]** özniteliği için parametre.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Ayrıca bir **[çoğaltan ModelBinder]** öznitelik türü için. Web API'si, bu türdeki tüm parametreler için belirtilen bir model bağlayıcı kullanır.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Son olarak, bir model Bağlayıcısı sağlayıcısına ekleyebilirsiniz **HttpConfiguration**. Yalnızca bir model bağlayıcı oluşturan bir Üreteç sınıfını bir model bağlayıcı sağlayıcısıdır. Türetilen bir sağlayıcı oluşturabilirsiniz [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) sınıfı. Tek bir tür, model bağlayıcı işler, ancak, yerleşik kullanımı daha kolay olur **SimpleModelBinderProvider**, bu amaç için tasarlanmıştır. Aşağıdaki kod, bunun nasıl yapılacağını gösterir.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Bir model bağlama sağlayıcısı ile eklemek yine **[çoğaltan ModelBinder]** özniteliği için parametre Web API'si, bir model bağlayıcı ve medya türü biçimlendiricisi kullanmalısınız bildirmek için. Ancak artık öznitelik, model bağlayıcı türünü belirtmeniz gerekmez:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Değer sağlayıcıları

Bir model bağlayıcı değerlerini sağlayıcıdan bir değer alır bahsetmiştim. Özel değer sağlayıcı yazma için uygulayan **IValueProvider** arabirimi. İstekte tanımlama bilgisi değerleri çeken bir örnek aşağıda verilmiştir:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Ayrıca bir değer sağlayıcı üreteci türeterek oluşturmak için ihtiyacınız **ValueProviderFactory** sınıfı.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Değer sağlayıcı üreteci için ekleme **HttpConfiguration** gibi.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API ölçeklemesini tüm değer sağlayıcıları, bu nedenle bir model bağlayıcı çağırdığında **ValueProvider.GetValue**, model bağlayıcı bunu oluşturabildiği ilk değer sağlayıcısında değerini alır.

Parametre düzeyinde kullanarak değer sağlayıcı üreteci alternatif olarak, ayarlayabilirsiniz **valueprovider'ın** özniteliğini aşağıdaki gibi:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Bu, Web API ile belirtilen değer sağlayıcı üreteci model bağlama kullanın ve herhangi bir kayıtlı değer sağlayıcıları kullanmayacak şekilde bildirir.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Model bağlayıcıları daha genel bir mekanizması belirli bir örneğini ' dir. Bakarsanız **[çoğaltan ModelBinder]** özniteliği, Özet türetilen görürsünüz **ParameterBindingAttribute** sınıfı. Bu sınıf, tek bir yöntemi tanımlar **GetBinding**, döndüren bir **HttpParameterBinding** nesnesi:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Bir **HttpParameterBinding** bir değere bir parametre bağlama için sorumludur. Durumunda, **[çoğaltan ModelBinder]**, öznitelik döndürür bir **HttpParameterBinding** kullanan uygulaması bir **IModelBinder** gerçek bağlama gerçekleştirmek için. Ayrıca kendi uygulayabilirsiniz **HttpParameterBinding**.

Örneğin, gelen Etag'ler almak istediğiniz varsayalım `if-match` ve `if-none-match` istekteki üstbilgi. Etag'ler temsil etmek için bir sınıf tanımlayarak başlayacağız.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Biz de Etag'den almak gösteren numaralandırmaya tanımlarsınız `if-match` üst bilgi veya `if-none-match` başlığı.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

İşte bir **HttpParameterBinding** ETag istenen başlığından alır ve ETag türünde bir parametre için bağlar:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** yöntemi bağlama yapar. Bu yöntem içinde bağlı bir parametre değerine ekleyin **ActionArgument** sözlüğünde **HttpActionContext**.

> [!NOTE]
> Varsa, **ExecuteBindingAsync** yöntemi okur istek iletisinin gövdesi, geçersiz kılma **WillReadBody** özelliği true döndürür. İstek gövdesi, Web API'sini bir kural, en fazla bir bağlama uygular, böylece yalnızca bir kez okunabilir arabellekten çıkarılan bir akışı, ileti gövdesi okuyabilirsiniz olabilir.


Özel bir uygulamaya **HttpParameterBinding**, türetilen bir öznitelik tanımlayabilirsiniz **ParameterBindingAttribute**. İçin `ETagParameterBinding`, için iki öznitelik tanımlarız `if-match` üstbilgileri, diğeri de `if-none-match` üstbilgileri. Her ikisi de, soyut bir temel sınıfından türetilir.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

İşte kullanan bir denetleyici yöntem `[IfNoneMatch]` özniteliği.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Yanında **ParameterBindingAttribute**, özel bir eklemek için başka bir kanca yoktur **HttpParameterBinding**. Üzerinde **HttpConfiguration** nesnesi **ParameterBindingRules** özelliği, türü anomymous işlevleri koleksiyonudur (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Örneğin, herhangi bir ETag parametre GET yöntemini kullanan bir kural ekleyebilirsiniz `ETagParameterBinding` ile `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

İşlev döndürmelidir `null` parametrelerinin nereye bağlama uygulanabilir değildir.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Parametre bağlamasını sürecinin tamamını takılabilir bir hizmet tarafından denetlenir **IActionValueBinder**. Varsayılan uygulaması **IActionValueBinder** şunları yapar:

1. Aranan bir **ParameterBindingAttribute** parametresi üzerinde. Bu içerir **[FromBody]**, **[FromUri]**, ve **[çoğaltan ModelBinder]**, ya da özel öznitelikler.
2. Aksi halde, konum **HttpConfiguration.ParameterBindingRules** null olmayan bir döndüren bir işlev için **HttpParameterBinding**.
3. Aksi takdirde, daha önce açıklanan varsayılan kuralları kullanın. 

    - Parametre türü "Basit" veya bir tür dönüştürücüsü varsa URI'SİNDEN bağlayın. Bu almaya eşdeğerdir **[FromUri]** parametre özniteliği.
    - Aksi takdirde, iletinin gövdesinden parametresi okunacak deneyin. Bu almaya eşdeğerdir **[FromBody]** parametresi üzerinde.

İsterseniz, tüm yerini alabilir **IActionValueBinder** hizmeti ile özel bir uygulama.

## <a name="additional-resources"></a>Ek Kaynaklar

[Özel parametre bağlama örneği](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Mike kabini Web API'si parametre bağlama hakkında blog gönderilerini iyi bir dizi yazdı:

- [Parametre bağlama Web API'sini nasıl yaptığını](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API'si için MVC stili parametre bağlaması](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [Eylem imzalarında MVC/Web API'de özel nesnelere bağlama](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Web API'de özel değer sağlayıcı oluşturma](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Web API'de parametre bağlama başlık altında](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
