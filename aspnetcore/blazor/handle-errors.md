---
title: ASP.NET Core Blazor uygulamalarında hataları işleme
author: guardrex
description: Blazor 'in işlenmemiş özel durumları nasıl yönettiğini ve hataları algılayan ve işleyen uygulamalar geliştirmeyi nasıl ASP.NET Core öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/06/2019
uid: blazor/handle-errors
ms.openlocfilehash: 52f55af99881b09c84d9cf88f5845efcb1ea76a1
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913861"
---
# <a name="handle-errors-in-aspnet-core-blazor-apps"></a>ASP.NET Core Blazor uygulamalarında hataları işleme

[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından

Bu makalede, Blazor işlenmemiş özel durumları nasıl yönettiği ve hataları algılayan ve işleyen uygulamalar geliştirileceği açıklanır.

## <a name="how-the-blazor-framework-reacts-to-unhandled-exceptions"></a>Blazor Framework işlenmemiş özel durumlara nasıl yeniden davranır

Blazor sunucu tarafı durum bilgisi olan bir çerçevedir. Kullanıcılar bir uygulamayla etkileşim kurarken, *devre*olarak bilinen sunucuya bir bağlantı sağlar. Devre, etkin bileşen örneklerini ve diğer birçok durum düzeyini barındırır; örneğin:

* Bileşenlerin en son işlenmiş çıktısı.
* İstemci tarafı olayları tarafından tetiklenebilecek geçerli olay işleme temsilcileri kümesi.

Bir Kullanıcı uygulamayı birden çok tarayıcı sekmelerinde açarsa, birden çok bağımsız devreler vardır.

Blazor, işlenmemiş özel durumların çoğunu gerçekleştiği devreyi önemli olarak değerlendirir. İşlenmeyen bir özel durum nedeniyle devre sonlandırılırsa, Kullanıcı yalnızca yeni bir bağlantı oluşturmak için sayfayı yeniden yükleyerek uygulamayla etkileşime geçerek devam edebilir. Diğer kullanıcılar veya diğer tarayıcı sekmeleri için devre dışı bırakılmış olan Sonlandırıcı dışındaki devreleri etkilenmez. Bu senaryo, kilitlenen&mdash;uygulamayı engelleyen bir masaüstü uygulamasına benzer, ancak diğer uygulamalar etkilenmemelidir.

Aşağıdaki nedenlerden dolayı işlenmeyen bir özel durum oluştuğunda devre sonlandırılır:

* İşlenmeyen bir özel durum, genellikle devre dışı bir durumda bırakır.
* İşlenmemiş bir özel durumdan sonra uygulamanın normal işlemi garanti edilemez.
* Devre devam ederse uygulamada güvenlik açıkları görünebilir.

## <a name="manage-unhandled-exceptions-in-developer-code"></a>Geliştirici kodundaki işlenmemiş özel durumları yönetme

Bir uygulamanın bir hatadan sonra devam edebilmesi için, uygulamanın hata işleme mantığı olması gerekir. Bu makalenin sonraki bölümlerinde işlenmemiş özel durumların olası kaynakları açıklanır.

Üretimde, çerçeve özel durum iletilerini veya yığın izlemelerini Kullanıcı arabiriminde işlemez. Özel durum iletilerini veya yığın izlemelerini işleme şu şekilde olabilir:

* Hassas bilgileri son kullanıcılara açıklayın.
* Kötü niyetli bir kullanıcının uygulama, sunucu veya ağ güvenliğini tehlikeye atabilecek bir uygulamada zayıf yanları bulmasına yardımcı olun.

## <a name="log-errors-with-a-persistent-provider"></a>Kalıcı bir sağlayıcıyla hataları günlüğe kaydet

İşlenmeyen bir özel durum oluşursa, özel durum hizmet kapsayıcısında yapılandırılan <xref:Microsoft.Extensions.Logging.ILogger> örneklere kaydedilir. Varsayılan olarak, Blazor uygulamaları konsol günlüğü sağlayıcısı ile konsol çıktısına kaydedilir. Günlük boyutunu ve günlük döndürmesini yöneten bir sağlayıcı ile daha kalıcı bir konuma oturum açmayı düşünün. Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

Geliştirme sırasında, Blazor genellikle hata ayıklamaya yardımcı olmak için tarayıcının konsoluna özel durumların tam ayrıntılarını gönderir. Üretimde, tarayıcı konsolundaki ayrıntılı hatalar varsayılan olarak devre dışıdır. Bu, hataların istemcilere gönderilmediği, ancak özel durumun tam ayrıntılarının hala sunucu tarafında günlüğe kaydedildiği anlamına gelir. Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.

Hangi olayların günlüğe kaydedileceğini ve günlüğe kaydedilen olayların önem düzeyi düzeyini karar vermelisiniz. Saldırgan kullanıcılar hataları kasıtlı olarak tetikleyebiliyor olabilir. Örneğin, ürün ayrıntılarını görüntüleyen bir bileşenin URL 'sinde, bilinmeyen `ProductId` bir hatadan olay günlüğe kaydetme. Tüm hatalar günlüğe kaydetme için yüksek önem derecesine sahip olaylar olarak değerlendirilmemelidir.

## <a name="places-where-errors-may-occur"></a>Hataların gerçekleşebileceği yerleri

Çerçeve ve uygulama kodu aşağıdaki konumlardan herhangi birinde işlenmeyen özel durumları tetikleyebiliriz:

* [Bileşen örneği oluşturma](#component-instantiation)
* [Yaşam döngüsü yöntemleri](#lifecycle-methods)
* [İşleme mantığı](#rendering-logic)
* [Olay işleyicileri](#event-handlers)
* [Bileşen elden çıkarma](#component-disposal)
* [JavaScript birlikte çalışma](#javascript-interop)
* [Devre işleyicileri](#circuit-handlers)
* [Devre elden çıkarma](#circuit-disposal)
* [Prerendering](#prerendering)

Önceki işlenmemiş özel durumlar, bu makalenin aşağıdaki bölümlerinde açıklanmıştır.

### <a name="component-instantiation"></a>Bileşen örneği oluşturma

Blazor bir bileşenin örneğini oluşturduğunda:

* Bileşenin Oluşturucusu çağrılır.
* Bileşen Oluşturucusu veya [@inject](xref:blazor/dependency-injection#request-a-service-in-a-component) [[Ekleme]](xref:blazor/dependency-injection#request-a-service-in-a-component) özniteliği aracılığıyla bileşen oluşturucusuna sağlanan tek başına bir veya daha fazla olmayan hizmetin oluşturucuları çağrılır. 

Herhangi `[Inject]` bir özellik için yürütülen herhangi bir Oluşturucu veya ayarlayıcı işlenmeyen bir özel durum oluşturduğunda devre dışı olur. Framework bileşeni örneklemediğinden özel durum önemlidir. Oluşturucu mantığı özel durumlar oluşturmayabilir, uygulama hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesini kullanarak özel durumları yakalemelidir.

### <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

Bir bileşenin kullanım ömrü boyunca Blazor, yaşam döngüsü yöntemlerini çağırır:

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Herhangi bir yaşam döngüsü yöntemi, zaman uyumlu veya zaman uyumsuz olarak bir özel durum oluşturursa, özel durum devre dışı olur. Bileşenler için yaşam döngüsü yöntemlerinde hatalarla başa çıkmak için hata işleme mantığı ekleyin.

Aşağıdaki örnekte `OnParametersSetAsync` , bir ürünü elde etmek için bir yöntemi çağırır:

* `ProductRepository.GetProductByIdAsync` Yönteminde oluşan bir özel durum, bir `try-catch` ifadesiyle işlenir.
* `catch` Blok yürütüldüğünde:
  * `loadFailed`, kullanıcıya bir `true`hata iletisi göstermek için kullanılan olarak ayarlanır.
  * Hata günlüğe kaydedilir.

[!code-cshtml[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>İşleme mantığı

Bir `.razor` bileşen dosyasındaki bildirim temelli biçimlendirme adlı C# `BuildRenderTree`bir yönteme derlenir. Bir bileşen oluşturduğunda, `BuildRenderTree` oluşturulan bileşenin öğelerini, metnini ve alt bileşenlerini açıklayan bir veri yapısını yürütür ve oluşturur.

İşleme mantığı bir özel durum oluşturabilir. Bu senaryonun bir örneği değerlendirilir, ancak `@someObject.PropertyName` `@someObject` ise `null`oluşur. İşleme mantığı tarafından oluşturulan işlenmeyen bir özel durum, devre için önemli bir durumdur.

Oluşturma mantığındaki null başvuru özel durumunu engellemek için, üyelerine erişmeden önce `null` bir nesne denetleyin. Aşağıdaki örnekte `person.Address` , `person.Address` şu `null`ise Özellikler erişilmez:

[!code-cshtml[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

Önceki kod olmadığını `person` `null`varsayar. Genellikle, kodun yapısı, bileşenin işlendiği sırada bir nesnenin var olmasını garanti eder. Bu durumlarda, işleme mantığını denetlemek `null` gerekli değildir. Önceki örnekte, bileşen örneklendiği `person` `person` zaman oluşturulduğu için, mevcut olması garantisini alabilir.

### <a name="event-handlers"></a>Olay işleyicileri

İstemci tarafı kod, kullanarak olay işleyicileri C# oluşturulduğunda kodun çağırmaları tetikler:

* `@onclick`
* `@onchange`
* Diğer `@on...` öznitelikler
* `@bind`

Olay işleyici kodu, bu senaryolarda işlenmeyen bir özel durum oluşturabilir.

Bir olay işleyicisi işlenmeyen bir özel durum oluşturursa (örneğin, bir veritabanı sorgusu başarısız olursa), özel durum devre dışı olarak önemli olur. Uygulama, dış nedenlerle başarısız olabilecek kodu çağırırsa, hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesini kullanarak özel durumlar yakalayın.

Kullanıcı kodu yakalanmazsa ve özel durumu işlemezse çerçeve özel durumu günlüğe kaydeder ve devre sonlandırır.

### <a name="component-disposal"></a>Bileşen elden çıkarma

Örneğin, Kullanıcı başka bir sayfaya gezindiği için, bir bileşen kullanıcı arabiriminden kaldırılabilir. Uygulayan <xref:System.IDisposable?displayProperty=fullName> bir bileşen kullanıcı arabiriminden kaldırıldığında, çerçeve <xref:System.IDisposable.Dispose*> bileşenin yöntemini çağırır. 

Bileşenin `Dispose` yöntemi işlenmeyen bir özel durum oluşturursa, özel durum devre dışı olarak önemli olur. Çıkarma mantığı özel durumlar oluşturmayabilir, uygulama hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesini kullanarak özel durumları yakalemelidir.

Bileşen elden çıkarma hakkında daha fazla bilgi için <xref:blazor/components#component-disposal-with-idisposable>bkz.

### <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

`IJSRuntime.InvokeAsync<T>`.NET kodunun Kullanıcı tarayıcısında JavaScript çalışma zamanına zaman uyumsuz çağrılar yapmasına izin verir.

Aşağıdaki koşullar ile `InvokeAsync<T>`hata işleme için geçerlidir:

* Bir çağrı `InvokeAsync<T>` zaman uyumlu başarısız olursa, .NET özel durumu oluşur. Sağlanan bağımsız değişkenler `InvokeAsync<T>` seri hale getirilemediğinden, başarısız olan bir çağrı başarısız oldu. Geliştirici kodu özel durumu yakalamalı. Bir olay işleyicisindeki veya bileşen yaşam döngüsü yöntemindeki uygulama kodu bir özel durumu işlemezse, ortaya çıkan özel durum devre dışı olur.
* Bir çağrı `InvokeAsync<T>` zaman uyumsuz olarak başarısız olursa, .net <xref:System.Threading.Tasks.Task> başarısız olur. Örneğin, JavaScript `InvokeAsync<T>` tarafı kodu bir özel durum oluşturduğundan veya olarak `rejected`tamamlanan bir `Promise` döndürürse, ' a çağrı başarısız olabilir. Geliştirici kodu özel durumu yakalamalı. [Await](/dotnet/csharp/language-reference/keywords/await) işleci kullanılıyorsa, yöntem çağrısını hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesinde sarmalamalı olarak düşünün. Aksi takdirde, başarısız kod, devre için önemli olan işlenmemiş bir özel durumla sonuçlanır.
* Varsayılan olarak, `InvokeAsync<T>` çağrısı belirli bir süre içinde tamamlanmalıdır veya çağrı zaman aşımına uğrar. Varsayılan zaman aşımı süresi bir dakikadır. Zaman aşımı, kodu ağ bağlantısında veya hiçbir zaman bir tamamlanma iletisi göndermeme JavaScript kodundaki bir kaybına karşı korur. Çağrı zaman aşımına uğrarsa sonuç `Task` bir <xref:System.OperationCanceledException>ile başarısız olur. Günlüğe kaydetme ile özel durumu yakalar ve işleyin.

Benzer şekilde, JavaScript kodu [[Jsinvokable] özniteliği](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions)tarafından belirtilen .net yöntemlerine çağrı başlatabilir. Bu .NET yöntemleri işlenmeyen bir özel durum oluşturur:

* Özel durum devre için önemli olarak değerlendirilmez.
* JavaScript tarafı `Promise` reddedilir.

.NET tarafında ya da yöntem çağrısının JavaScript tarafında hata işleme kodu kullanma seçeneğiniz vardır.

Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.

### <a name="circuit-handlers"></a>Devre işleyicileri

Blazor, kodun bir Kullanıcı devresi durumu değiştiğinde bildirimleri alan bir *devre işleyicisi*tanımlamasına olanak tanır. Aşağıdaki durumlar kullanılır:

* `initialized`
* `connected`
* `disconnected`
* `disposed`

Bildirimler, `CircuitHandler` soyut temel sınıftan devralan bir dı hizmeti kaydedilerek yönetilir.

Özel bir devre işleyicisindeki Yöntemler işlenmeyen bir özel durum oluşturuyorsam, özel durum devre için önemli olur. Bir işleyicinin kodundaki veya yöntemleri çağrılan özel durumlara tolerans sağlamak için kodu bir veya daha fazla [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) deyiminde hata işleme ve günlüğe kaydetme ile sarın.

### <a name="circuit-disposal"></a>Devre elden çıkarma

Bir kullanıcının bağlantısı kesilmediği ve Framework devre durumunu temizlemede bir devre dışı bırakıldığında, çerçeve devre dışı bırakıldı. Kapsamı elden atılırken, uygulayan <xref:System.IDisposable?displayProperty=fullName>HIÇBIR devre kapsamlı dı hizmeti yok. Herhangi bir DI hizmeti, elden çıkarma sırasında işlenmeyen bir özel durum oluşturursa, çerçeve özel durumu günlüğe kaydeder.

### <a name="prerendering"></a>Prerendering

Blazor bileşenleri, işlenmiş HTML işaretlemesi kullanıcının ilk `Html.RenderComponentAsync` http isteğinin bir parçası olarak döndürülmek üzere kullanılarak önceden alınabilir. Bu şu şekilde geçerlidir:

* Aynı sayfanın parçası olan tüm ön işlenmiş bileşenleri içeren yeni bir devre oluşturma.
* İlk HTML oluşturuluyor.
* Kullanıcı tarayıcısı, devre `disconnected` üzerinde etkileşim sağlamak için aynı sunucuya doğru bir SignalR bağlantısı kurana kadar, devreyi kabul etmek.

Herhangi bir bileşen prerendering sırasında, örneğin bir yaşam döngüsü yöntemi veya işleme mantığı sırasında işlenmeyen bir özel durum oluşturursa:

* Bu, devre için önemli bir durumdur.
* Özel durum, çağrı yığınını `Html.RenderComponentAsync` çağrıdan oluşur. Bu nedenle, özel durum geliştirici kodu tarafından açıkça yakalanmadığı takdirde tüm HTTP isteği başarısız olur.

Normal koşullarda, prerendering başarısız olduğunda bileşeni oluşturma ve işleme devam etmek, çalışan bir bileşen işlenemediği için mantıklı değildir.

Prerendering sırasında oluşabilecek hatalara tolerans sağlamak için hata işleme mantığı özel durum oluşturabilecek bir bileşenin içine yerleştirilmelidir. [Try-catch](/dotnet/csharp/language-reference/keywords/try-catch) deyimlerini hata işleme ve günlüğe kaydetme ile kullanın. Çağrısını `RenderComponentAsync` `RenderComponentAsync`bir `try-catch` deyime sarmalama yerine, tarafından işlenen bileşene hata işleme mantığını koyun.

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

### <a name="recursive-rendering"></a>Özyinelemeli işleme

Bileşenler yinelemeli olarak iç içe olabilir. Bu, özyinelemeli veri yapılarını temsil etmek için kullanışlıdır. Örneğin, bir `TreeNode` bileşen düğüm alt öğelerinin her `TreeNode` biri için daha fazla bileşen işleyebilir.

Yinelemeli olarak işlenirken, sonsuz özyineleme sonucu veren kodlama desenlerinden kaçının:

* Bir döngüyü içeren bir veri yapısını yinelemeli olarak işlemez. Örneğin, alt öğeleri içeren bir ağaç düğümünü işlemez.
* Bir döngüyü içeren bir düzen zinciri oluşturmayın. Örneğin, düzeni kendisi olan bir düzen oluşturmayın.
* Son kullanıcının, kötü amaçlı veri girişi veya JavaScript birlikte çalışabilirlik çağrıları aracılığıyla özyineleme/varyantları (kurallar) ihlal etmemenizi izin verme.

Oluşturma sırasında sonsuz döngüler:

* İşleme işleminin süresiz olarak devam etmesine neden olur.
* Sonlandırılmamış bir döngü oluşturmaya eşdeğerdir.

Bu senaryolarda, etkilenen devre askıda kalır ve iş parçacığı genellikle şunları yapmayı dener:

* Süresiz olarak işletim sisteminin izin verdiği CPU süresini çok fazla kullanın.
* Sınırsız miktarda sunucu belleği tükettin. Sınırsız bellek tüketme, Sonlandırılmamış bir döngünün her yinelemede bir koleksiyona giriş eklediği senaryoya eşdeğerdir.

Sonsuz özyineleme desenlerinin önüne geçmek için, özyinelemeli işleme kodunun uygun durdurma koşullarını içerdiğinden emin olun.

### <a name="custom-render-tree-logic"></a>Özel işleme ağacı mantığı

Çoğu Blazor bileşeni *. Razor* dosyaları olarak uygulanır ve çıktısını işlemek için bir `RenderTreeBuilder` üzerinde çalışan Logic üretmek için derlenir. Geliştirici, yordamsal C# kodu kullanarak `RenderTreeBuilder` mantığı el ile uygulayabilir. Daha fazla bilgi için bkz. <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> El ile işleme ağacı Oluşturucu mantığının kullanımı, genel bileşen geliştirme için önerilmeyen gelişmiş ve güvenli olmayan bir senaryo olarak değerlendirilir.

`RenderTreeBuilder` Kod yazılmışsa, geliştirici kodun doğruluğunu garanti etmelidir. Örneğin, geliştirici şunları sağlamalıdır:

* `OpenElement` Ve`CloseElement` çağrıları doğru şekilde dağıtılır.
* Öznitelikler yalnızca doğru yerlere eklenir.

Hatalı elle işleme ağacı Oluşturucu mantığı kilitlenmeler, sunucu askıda kalma ve güvenlik açıkları dahil rastgele tanımsız davranışa neden olabilir.

El ile işleme ağacı Oluşturucu mantığını aynı karmaşıklık düzeyinde ve aynı *tehlike* düzeyiyle, el ile derleme kodu veya MSIL yönergeleri yazarak değerlendirin.
