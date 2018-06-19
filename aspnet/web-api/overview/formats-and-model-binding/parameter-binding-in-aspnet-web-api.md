---
uid: web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
title: ASP.NET Web API bağlama parametresi | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2013
ms.topic: article
ms.assetid: e42c8388-04ed-4341-9fdb-41b1b4c06320
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/parameter-binding-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5aa532137436922519c86246ebfa834910ac0d86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042436"
---
<a name="parameter-binding-in-aspnet-web-api"></a>ASP.NET Web API bağlama parametresi
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Web API denetleyicisi üzerinde bir yöntem çağırdığında adlı bir işlem parametre değerlerini ayarlamalısınız *bağlama*. Bu makalede, Web API parametreleri nasıl bağlar ve bağlama işlemi nasıl özelleştirebileceğiniz açıklanmaktadır.

Varsayılan olarak, Web API parametreleri bağlamak için aşağıdaki kuralları kullanır:

- Parametre "Basit" bir tür ise, Web API URI'den değeri almaya çalışır. Basit türler .NET [ilkel türler](https://msdn.microsoft.com/library/system.type.isprimitive.aspx) (**int**, **bool**, **çift**, vb.), artı **TimeSpan**, **DateTime**, **GUID**, **ondalık**, ve **dize**, *artı* herhangi bir dizeden dönüştürebilirsiniz türü dönüştürücü ile yazın. (Hakkında daha fazla daha sonra tür dönüştürücüleri.)
- Karmaşık türler, Web API çalışır için ileti gövdesinden değerini okumaya kullanarak bir [medya türü biçimlendiricisi](media-formatters.md).

Örneğin, tipik bir Web API denetleyicisi yöntemi şöyledir:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample1.cs)]

*Kimliği* parametresi bir &quot;basit&quot; Web API isteğin URI değeri almaya çalıştığında şekilde yazın. *Öğesi* parametredir karmaşık bir tür böylece Web API isteği gövdesinden değerini okumak için bir medya türü biçimlendiricisi kullanır.

URI'den bir değer almak için Web API rota verilerini ve URI sorgu dizesi arar. Yönlendirme sistem URI ayrıştırırken bir rotayla eşleşen rota verilerini doldurulur. Daha fazla bilgi için bkz: [Yönlendirme ve eylem seçimi](../web-api-routing-and-actions/routing-and-action-selection.md).

Bu makalede kalan ı model bağlama işleminden nasıl özelleştirebileceğiniz göstereceğiz. Karmaşık türler için Bununla birlikte, medya türü biçimlendiricilerini mümkün olduğunca kullanmayı düşünün. Bir anahtar HTTP kaynakları kaynak gösterimini belirtmek için içerik anlaşması kullanarak ileti gövdesini gönderilir ilkesidir. Medya türü biçimlendiricilerini tam olarak bu amaç için tasarlanmıştır.

## <a name="using-fromuri"></a>[FromUri] kullanma

Karmaşık bir tür URI'den okumak için Web API zorlamak için add **[FromUri]** özniteliği parametresi. Aşağıdaki örnek tanımlayan bir `GeoPoint` alır denetleyici yöntemi birlikte türü `GeoPoint` uri'den.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample2.cs)]

İstemci sorgu dizesinde enlem ve boylam değerlerini koyabilirsiniz ve Web API kullanılacağını bunları oluşturmak için bir `GeoPoint`. Örneğin:

`http://localhost/api/values/?Latitude=47.678558&Longitude=-122.130989`

## <a name="using-frombody"></a>[FromBody] kullanma

Web API isteği gövdesinden basit bir tür okumaya zorlamak için add **[FromBody]** özniteliği için parametre:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample3.cs)]

Bu örnekte, değerini okumak için bir medya türü biçimlendiricisi Web API kullanacağı *adı* isteği gövdesinden. Burada, örnek istemci isteği verilmiştir.

[!code-console[Main](parameter-binding-in-aspnet-web-api/samples/sample4.cmd)]

Web API Content-Type üstbilgisi [FromBody] bir parametreye sahip olduğunda, bir biçimlendirici seçmek için kullanır. Bu örnekte, içerik türü olduğundan &quot;uygulama/json&quot; ve istek gövdesi ham JSON dizesi (JSON nesnesi değil).

En fazla bir parametre ileti gövdeden okuma izni yok. Bu nedenle bu çalışmaz:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample5.cs)]

Bu kural için istek gövdesini yalnızca bir kez okunabilir olmayan arabelleğe alınan bir akışta depolanabilir nedenidir.

## <a name="type-converters"></a>Tür dönüştürücüleri

Web API (Web API URI'den bağlamak deneyecek böylece) bir sınıfı basit bir tür olarak davranmak oluşturarak yapabileceğiniz bir **Typeconverter'a** ve dize dönüştürme sağlama.

Aşağıdaki kodda gösterildiği bir `GeoPoint` coğrafi noktasını temsil eden sınıf artı bir **Typeconverter'a** dönüştüren için dizelerden `GeoPoint` örnekleri. `GeoPoint` Sınıfı donatılmış ile bir **[Typeconverter'a]** öznitelik türü dönüştürücü belirtin. (Bu örnek tarafından CAN kabini'nın blog gönderisi yaratıcı [MVC/Webapı eylem imzalarında özel nesneleri bağlamak nasıl](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx).)

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample6.cs)]

Web API işler artık `GeoPoint` basit bir tür olarak bağlamak çalışır yani `GeoPoint` URI parametrelerinden. Eklenecek gerekmeyen **[FromUri]** parametresi üzerinde.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample7.cs)]

İstemci şöyle bir URI ile yöntemi çağırabilirsiniz:

`http://localhost/api/values/?location=47.678558,-122.130989`

## <a name="model-binders"></a>Model Binders

Özel bir model bağlayıcısını oluşturmak için tür dönüştürücüsünü olandan daha esnek bir seçenek. Bir model bağlayıcı ile HTTP isteği, eylem açıklaması ve ham değerler gibi rota verilerinden erişebilirsiniz.

Bir model bağlayıcısı oluşturmak için uygulaması **IModelBinder** arabirimi. Bu arabirim tek bir yöntemi tanımlayan **BindModel**:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample8.cs)]

İçin bir model bağlayıcı işte `GeoPoint` nesneleri.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample9.cs)]

Ham giriş değerleri bir model bağlayıcı alır bir *değer sağlayıcı*. Bu tasarım, iki farklı işlevler ayırır:

- Değer sağlayıcı HTTP isteğini alır ve anahtar-değer çiftleri sözlüğü doldurur.
- Model bağlayıcı Bu sözlük model doldurmak için kullanır.

Web API varsayılan değer sağlayıcısında rota verilerini ve sorgu dizesi değerlerini alır. Örneğin, URI ise `http://localhost/api/values/1?location=48,-122`, aşağıdaki anahtar-değer çiftleri değer sağlayıcısı oluşturur:

- id = &quot;1&quot;
- konumu = &quot;48,122&quot;

(Olan varsayılan rota şablonu varsayılarak &quot;API / {controller} / {id}&quot;.)

Bağlanacak parametre adını depolanan **ModelBindingContext.ModelName** özelliği. Model Bağlayıcısı sözlüğünde bu değer bir anahtarla arar. Değeri varsa ve metne dönüştürülecek bir `GeoPoint`, model bağlayıcı bağımlı değere atar **ModelBindingContext.Model** özelliği.

Model bağlayıcı için bir basit tür dönüştürme sınırlı olmadığına dikkat edin. Bu örnekte, model bağlayıcı ilk bilinen konumları tablosunda arar ve başarısız olursa, tür dönüştürme kullanır.

**Model bağlayıcı ayarlama**

Bir model bağlayıcısını ayarlamak için birkaç yolu vardır. İlk olarak, ekleyebileceğiniz bir **[çoğaltan ModelBinder]** özniteliği parametresi.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample10.cs)]

Ayrıca ekleyebileceğiniz bir **[çoğaltan ModelBinder]** öznitelik türü. Web API, bu türdeki tüm parametreler için belirtilen model bağlayıcı kullanır.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample11.cs)]

Son olarak, bir model bağlayıcı sağlayıcısı ekleyebilirsiniz **HttpConfiguration**. Bir model bağlayıcı sağlayıcısı bir model bağlayıcı oluşturan yalnızca bir Fabrika sınıftır. Türetilen bir sağlayıcı oluşturabilirsiniz [ModelBinderProvider](https://msdn.microsoft.com/library/system.web.http.modelbinding.modelbinderprovider.aspx) sınıfı. Tek bir tür, model bağlayıcı işler, ancak bu yerleşik kullanmak daha kolay olur **SimpleModelBinderProvider**, bu amaç için tasarlanmıştır. Aşağıdaki kod bunun nasıl yapılacağı gösterilmektedir.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample12.cs)]

Bir model bağlama sağlayıcıyla hala eklemek istediğiniz **[çoğaltan ModelBinder]** parametresi, bir model bağlayıcı ve medya türü biçimlendiricisi kullanmalıdır Web API bildirmek için öznitelik. Ancak şimdi özniteliğinde model bağlayıcı türünü belirtmenize gerek yoktur:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample13.cs)]

## <a name="value-providers"></a>Değer sağlayıcıları

I bir model bağlayıcı değerleri sağlayıcıdan bir değer alır belirtiliyor. Bir özel değer sağlayıcı yazmak için uygulama **IValueProvider** arabirimi. İstekte tanımlama bilgisi değerleri çeken örnek aşağıda verilmiştir:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample14.cs)]

Ayrıca bir değer sağlayıcı üreteci türetme tarafından oluşturmanıza gerek **ValueProviderFactory** sınıfı.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample15.cs)]

Değer sağlayıcı üreteci eklemek **HttpConfiguration** gibi.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample16.cs)]

Web API oluşturur tüm değer sağlayıcıları, böylece bir model bağlayıcı çağırdığında **ValueProvider.GetValue**, onu oluşturabildiği ilk değer sağlayıcıdan değeri model bağlayıcı alır.

Alternatif olarak, değer sağlayıcı üreteci parametre düzeyinde kullanarak ayarlayabileceğiniz **valueprovider'ın** özniteliğini aşağıdaki gibi:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample17.cs)]

Bu Web API ile belirtilen değer sağlayıcı üreteci model bağlama kullanın ve herhangi bir kayıtlı değer sağlayıcıları kullanmayacak şekilde söyler.

## <a name="httpparameterbinding"></a>HttpParameterBinding

Model bağlayıcıları daha genel bir mekanizması belirli bir örneği var. Bakarsanız **[çoğaltan ModelBinder]** özniteliği, Özet türetilen görürsünüz **ParameterBindingAttribute** sınıfı. Tek bir yöntem bu sınıfı tanımlayan **GetBinding**, döndüren bir **HttpParameterBinding** nesnesi:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample18.cs)]

Bir **HttpParameterBinding** bir değere bir parametre bağlama için sorumludur. Durumunda **[çoğaltan ModelBinder]**, öznitelik döndüren bir **HttpParameterBinding** kullanan uygulaması bir **IModelBinder** gerçek bağlama gerçekleştirmek için. Ayrıca kendi uygulayabileceğiniz **HttpParameterBinding**.

Örneğin, gelen Etag'ler almak istediğinizi varsayalım `if-match` ve `if-none-match` istekteki üstbilgi. Etag'ler temsil etmek için bir sınıf tanımlayarak başlayacağız.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample19.cs)]

Biz de Etag'den alma atlanmayacağını belirtmek için bir numaralandırma tanımlarsınız `if-match` üstbilgisi veya `if-none-match` üstbilgi.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample20.cs)]

Burada bir **HttpParameterBinding** , istenen başlığından ETag alır ve ETag türünde bir parametre için bağlar:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample21.cs)]

**ExecuteBindingAsync** yöntemi bağlama yapar. İçinde bu yöntem, ilişkili parametre değerine eklemek **ActionArgument** sözlükte **HttpActionContext**.

> [!NOTE]
> Varsa, **ExecuteBindingAsync** yöntemi Okuma isteği ileti gövdesi, geçersiz kılma **WillReadBody** özelliği true döndürür. İstek gövdesini Web API'sini bir kural, en çok bir bağlama zorlar, böylece yalnızca bir kez okunabilir bir arabellekten çıkarılan akışı ileti gövdesi okuyabilir olabilir.


Özel bir uygulamak için **HttpParameterBinding**, türetilen bir öznitelik tanımlayabilirsiniz **ParameterBindingAttribute**. İçin `ETagParameterBinding`, için iki öznitelik tanımlama olasılığınız yüksektir `if-match` üstbilgileri, diğeri de `if-none-match` üstbilgileri. Her ikisi de soyut taban sınıfından türetilir.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample22.cs)]

Kullanan bir denetleyici yöntem işte `[IfNoneMatch]` özniteliği.

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample23.cs)]

Yanında **ParameterBindingAttribute**, özel bir eklemek için başka bir kanca yoktur **HttpParameterBinding**. Üzerinde **HttpConfiguration** nesnesi **ParameterBindingRules** özellik türünün anomymous işlevlerinin koleksiyonudur (**HttpParameterDescriptor**  - &gt; **HttpParameterBinding**). Örneğin, herhangi bir ETag parametre GET yöntemini kullanan bir kural ekleyebilirsiniz `ETagParameterBinding` ile `if-none-match`:

[!code-csharp[Main](parameter-binding-in-aspnet-web-api/samples/sample24.cs)]

İşlev döndürmelidir `null` bağlama olduğu geçerli parametreler için.

## <a name="iactionvaluebinder"></a>IActionValueBinder

Tüm parametre bağlama işlemi takılabilir bir hizmet tarafından denetlenen **IActionValueBinder**. Varsayılan uygulaması **IActionValueBinder** şunları yapar:

1. Aramak bir **ParameterBindingAttribute** parametresi üzerinde. Bu içerir **[FromBody]**, **[FromUri]**, ve **[çoğaltan ModelBinder]**, ya da özel öznitelikler.
2. Aksi takdirde, konum **HttpConfiguration.ParameterBindingRules** null olmayan bir döndüren bir işlev için **HttpParameterBinding**.
3. Aksi takdirde, daha önce açıklanan varsayılan kuralları kullanın. 

    - Parametre türü "Basit" veya bir tür dönüştürücüsünü varsa, URI'den bağlayın. Bu koymak eşdeğerdir **[FromUri]** parametre özniteliği.
    - Aksi takdirde, ileti gövdesinden parametresi okumaya çalışır. Bu koymak eşdeğerdir **[FromBody]** parametresi üzerinde.

İsterseniz, tüm koyabilirsiniz **IActionValueBinder** özel bir uygulama hizmet.

## <a name="additional-resources"></a>Ek Kaynaklar

[Özel parametre bağlama örneği](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/CustomParameterBinding/ReadMe.txt)

Can kabini blog gönderileri iyi bir dizi Web API parametre bağlama hakkında yazdı:

- [Parametre bağlama Web API'sini nasıl yapar](https://blogs.msdn.com/b/jmstall/archive/2012/04/16/how-webapi-does-parameter-binding.aspx)
- [Web API için MVC stili parametre bağlama](https://blogs.msdn.com/b/jmstall/archive/2012/04/18/mvc-style-parameter-binding-for-webapi.aspx)
- [MVC/Web API eylem imzalarında özel nesneleri bağlamak nasıl](https://blogs.msdn.com/b/jmstall/archive/2012/04/20/how-to-bind-to-custom-objects-in-action-signatures-in-mvc-webapi.aspx)
- [Bir özel değer sağlayıcı Web API'si oluşturma](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx)
- [Başlık altında Web API parametre bağlama](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx)
