---
title: ASP.NET Core Blazor uygulamalarda hataları işleme
author: guardrex
description: Blazor işlenmemiş özel durumları nasıl yönettiğini ve hataları algılayan ve işleyen uygulamalar geliştirme Blazor nasıl ASP.NET Core olduğunu öğrenin.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2020
no-loc:
- Blazor
- SignalR
uid: blazor/handle-errors
ms.openlocfilehash: 7b5602d5ae5e58d1678762fe1cd2adec1f31c969
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809009"
---
# <a name="handle-errors-in-aspnet-core-opno-locblazor-apps"></a>ASP.NET Core Blazor uygulamalarda hataları işleme

[Steve Sanderson](https://github.com/SteveSandersonMS) tarafından

Bu makalede, işlenmemiş özel durumları Blazor nasıl yönettiği ve hataları algılayan ve işleyen uygulamalar geliştirileceği açıklanır.

## <a name="detailed-errors-during-development"></a>Geliştirme sırasında ayrıntılı hatalar

Geliştirme sırasında Blazor bir uygulama düzgün bir şekilde çalışmadığı zaman, uygulamanın ayrıntılı hata bilgilerinin alınması sorunu gidermeye ve sorunun giderilmesine yardımcı olur. Bir hata oluştuğunda, Blazor uygulamalar ekranın alt kısmında altın bir çubuk görüntüler:

* Geliştirme sırasında altın çubuk, özel durumu görebileceğiniz tarayıcı konsoluna yönlendirir.
* Üretimde, altın çubuk kullanıcıya bir hata oluştuğunu bildirir ve tarayıcıyı yenilemeyi önerir.

Bu hata işleme deneyimi için Kullanıcı arabirimi Blazor projesi şablonlarının bir parçasıdır. Blazor WebAssembly uygulamasında, *Wwwroot/index.html* dosyasındaki deneyimi özelleştirin:

```html
<div id="blazor-error-ui">
    An unhandled error has occurred.
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

Blazor sunucu uygulamasında, *Pages/_Host. cshtml* dosyasındaki deneyimi özelleştirin:

```cshtml
<div id="blazor-error-ui">
    <environment include="Staging,Production">
        An error has occurred. This application may no longer respond until reloaded.
    </environment>
    <environment include="Development">
        An unhandled exception has occurred. See browser dev tools for details.
    </environment>
    <a href="" class="reload">Reload</a>
    <a class="dismiss">🗙</a>
</div>
```

`blazor-error-ui` öğesi Blazor şablonlarına eklenen stillerle gizlenir ve bir hata oluştuğunda gösterilir.

## <a name="how-the-opno-locblazor-framework-reacts-to-unhandled-exceptions"></a>Blazor Framework işlenmemiş özel durumlara nasıl yeniden davranır

Blazor sunucusu durum bilgisi olan bir çerçevedir. Kullanıcılar bir uygulamayla etkileşim kurarken, *devre*olarak bilinen sunucuya bir bağlantı sağlar. Devre, etkin bileşen örneklerini ve diğer birçok durum düzeyini barındırır; örneğin:

* Bileşenlerin en son işlenmiş çıktısı.
* İstemci tarafı olayları tarafından tetiklenebilecek geçerli olay işleme temsilcileri kümesi.

Bir Kullanıcı uygulamayı birden çok tarayıcı sekmelerinde açarsa, birden çok bağımsız devreler vardır.

Blazor, işlenmemiş özel durumların çoğunu gerçekleştiği devreyi önemli olarak değerlendirir. İşlenmeyen bir özel durum nedeniyle devre sonlandırılırsa, Kullanıcı yalnızca yeni bir bağlantı oluşturmak için sayfayı yeniden yükleyerek uygulamayla etkileşime geçerek devam edebilir. Diğer kullanıcılar veya diğer tarayıcı sekmeleri için devre dışı bırakılmış olan Sonlandırıcı dışındaki devreleri etkilenmez. Bu senaryo, kilitlenen uygulamanın yeniden başlatılması gereken&mdash;, ancak diğer uygulamaların etkilenmemesi gerektiğini engelleyen bir masaüstü uygulamasına benzerdir.

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

İşlenmeyen bir özel durum oluşursa, özel durum hizmet kapsayıcısında yapılandırılmış <xref:Microsoft.Extensions.Logging.ILogger> örneklerine kaydedilir. Varsayılan olarak, Blazor uygulamalar konsol günlüğü sağlayıcısı ile konsol çıktısına kaydedilir. Günlük boyutunu ve günlük döndürmesini yöneten bir sağlayıcı ile daha kalıcı bir konuma oturum açmayı düşünün. Daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

Geliştirme sırasında, Blazor hata ayıklamaya yardımcı olması için genellikle özel durumların tüm ayrıntılarını tarayıcı konsoluna gönderir. Üretimde, tarayıcı konsolundaki ayrıntılı hatalar varsayılan olarak devre dışıdır. Bu, hataların istemcilere gönderilmediği, ancak özel durumun tam ayrıntılarının hala sunucu tarafında günlüğe kaydedildiği anlamına gelir. Daha fazla bilgi için bkz. <xref:fundamentals/error-handling>.

Hangi olayların günlüğe kaydedileceğini ve günlüğe kaydedilen olayların önem düzeyi düzeyini karar vermelisiniz. Saldırgan kullanıcılar hataları kasıtlı olarak tetikleyebiliyor olabilir. Örneğin, ürün ayrıntılarını görüntüleyen bir bileşenin URL 'sinde bilinmeyen bir `ProductId` sağlandığı bir hatadan olay günlüğe kaydetme. Tüm hatalar günlüğe kaydetme için yüksek önem derecesine sahip olaylar olarak değerlendirilmemelidir.

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
* [`@inject`](xref:blazor/dependency-injection#request-a-service-in-a-component) yönergesi veya [`[Inject]`](xref:blazor/dependency-injection#request-a-service-in-a-component) özniteliği aracılığıyla bileşen oluşturucusuna sağlanan Singleton olmayan her türlü hizmeti Oluşturucu.

Herhangi bir `[Inject]` özelliği için yürütülen bir Oluşturucu veya ayarlayıcı işlenmeyen bir özel durum oluşturduğunda devre dışı olur. Framework bileşeni örneklemediğinden özel durum önemlidir. Oluşturucu mantığı özel durumlar oluşturmayabilir, uygulama hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesini kullanarak özel durumları yakalemelidir.

### <a name="lifecycle-methods"></a>Yaşam döngüsü yöntemleri

Bir bileşenin kullanım ömrü boyunca Blazor aşağıdaki [yaşam döngüsü yöntemlerini](xref:blazor/lifecycle)çağırır:

* `OnInitialized` / `OnInitializedAsync`
* `OnParametersSet` / `OnParametersSetAsync`
* `ShouldRender` / `ShouldRenderAsync`
* `OnAfterRender` / `OnAfterRenderAsync`

Herhangi bir yaşam döngüsü yöntemi, zaman uyumlu veya zaman uyumsuz olarak bir özel durum oluşturursa, özel durum devre dışı olur. Bileşenler için yaşam döngüsü yöntemlerinde hatalarla başa çıkmak için hata işleme mantığı ekleyin.

Aşağıdaki örnekte `OnParametersSetAsync` bir ürünü almak için bir yöntemi çağırır:

* `ProductRepository.GetProductByIdAsync` yönteminde oluşan bir özel durum `try-catch` ifadesiyle işlenir.
* `catch` bloğu yürütüldüğünde:
  * `loadFailed`, kullanıcıya bir hata iletisi göstermek için kullanılan `true`olarak ayarlanır.
  * Hata günlüğe kaydedilir.

[!code-razor[](handle-errors/samples_snapshot/3.x/product-details.razor?highlight=11,27-39)]

### <a name="rendering-logic"></a>İşleme mantığı

`.razor` bileşen dosyasındaki bildirim temelli biçimlendirme `BuildRenderTree`adlı bir C# yönteme derlenir. Bir bileşen işlendiğinde `BuildRenderTree` yürütülür ve işlenmiş bileşenin öğelerini, metnini ve alt bileşenlerini açıklayan bir veri yapısı oluşturur.

İşleme mantığı bir özel durum oluşturabilir. `@someObject.PropertyName` değerlendiriliyorsa ancak `@someObject` `null`bu senaryoya bir örnek oluşur. İşleme mantığı tarafından oluşturulan işlenmeyen bir özel durum, devre için önemli bir durumdur.

Oluşturma mantığındaki null başvuru özel durumunu engellemek için, üyelerine erişmeden önce bir `null` nesnesini denetleyin. Aşağıdaki örnekte, `person.Address` `null``person.Address` özelliklere erişilmez:

[!code-razor[](handle-errors/samples_snapshot/3.x/person-example.razor?highlight=1)]

Yukarıdaki kod, `person` `null`olmadığını varsayar. Genellikle, kodun yapısı, bileşenin işlendiği sırada bir nesnenin var olmasını garanti eder. Bu durumlarda, işleme mantığındaki `null` denetlemek gerekli değildir. Önceki örnekte, bileşen örneği oluşturulduğunda `person` oluşturulduğu için `person` olması garanti edilebilir.

### <a name="event-handlers"></a>Olay işleyicileri

İstemci tarafı kod, kullanarak olay işleyicileri C# oluşturulduğunda kodun çağırmaları tetikler:

* `@onclick`
* `@onchange`
* Diğer `@on...` öznitelikleri
* `@bind`

Olay işleyici kodu, bu senaryolarda işlenmeyen bir özel durum oluşturabilir.

Bir olay işleyicisi işlenmeyen bir özel durum oluşturursa (örneğin, bir veritabanı sorgusu başarısız olursa), özel durum devre dışı olarak önemli olur. Uygulama, dış nedenlerle başarısız olabilecek kodu çağırırsa, hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesini kullanarak özel durumlar yakalayın.

Kullanıcı kodu yakalanmazsa ve özel durumu işlemezse çerçeve özel durumu günlüğe kaydeder ve devre sonlandırır.

### <a name="component-disposal"></a>Bileşen elden çıkarma

Örneğin, Kullanıcı başka bir sayfaya gezindiği için, bir bileşen kullanıcı arabiriminden kaldırılabilir. <xref:System.IDisposable?displayProperty=fullName> uygulayan bir bileşen kullanıcı arabiriminden kaldırıldığında, çerçeve bileşenin <xref:System.IDisposable.Dispose*> yöntemini çağırır.

Bileşenin `Dispose` yöntemi işlenmeyen bir özel durum oluşturursa, bu özel durum devre dışı olarak önemli olur. Çıkarma mantığı özel durumlar oluşturmayabilir, uygulama hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesini kullanarak özel durumları yakalemelidir.

Bileşen aktiften çıkarma hakkında daha fazla bilgi için bkz. <xref:blazor/lifecycle#component-disposal-with-idisposable>.

### <a name="javascript-interop"></a>JavaScript ile birlikte çalışma

`IJSRuntime.InvokeAsync<T>`, .NET kodunun Kullanıcı tarayıcısındaki JavaScript çalışma zamanına zaman uyumsuz çağrılar yapmasına izin verir.

`InvokeAsync<T>`ile ilgili hata işleme için aşağıdaki koşullar geçerlidir:

* `InvokeAsync<T>` çağrısı eşzamanlı olarak başarısız olursa, .NET özel durumu oluşur. `InvokeAsync<T>` çağrısı başarısız olabilir, örneğin, sağlanan bağımsız değişkenler serileştirilemiyor. Geliştirici kodu özel durumu yakalamalı. Bir olay işleyicisindeki veya bileşen yaşam döngüsü yöntemindeki uygulama kodu bir özel durumu işlemezse, ortaya çıkan özel durum devre dışı olur.
* `InvokeAsync<T>` çağrısı zaman uyumsuz olarak başarısız olursa, .NET <xref:System.Threading.Tasks.Task> başarısız olur. Örneğin, JavaScript tarafı kodu bir özel durum oluşturduğundan veya `rejected`olarak tamamlanan bir `Promise` döndürdüğünden `InvokeAsync<T>` çağrısı başarısız olabilir. Geliştirici kodu özel durumu yakalamalı. [Await](/dotnet/csharp/language-reference/keywords/await) işleci kullanılıyorsa, yöntem çağrısını hata işleme ve günlüğe kaydetme ile [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) ifadesinde sarmalamalı olarak düşünün. Aksi takdirde, başarısız kod, devre için önemli olan işlenmemiş bir özel durumla sonuçlanır.
* Varsayılan olarak, `InvokeAsync<T>` çağrıları belirli bir süre içinde tamamlanmalıdır veya çağrı zaman aşımına uğrar. Varsayılan zaman aşımı süresi bir dakikadır. Zaman aşımı, kodu ağ bağlantısında veya hiçbir zaman bir tamamlanma iletisi göndermeme JavaScript kodundaki bir kaybına karşı korur. Çağrı zaman aşımına uğrarsa, elde edilen `Task` bir <xref:System.OperationCanceledException>başarısız olur. Günlüğe kaydetme ile özel durumu yakalar ve işleyin.

Benzer şekilde, JavaScript kodu [`[JSInvokable]`](xref:blazor/javascript-interop#invoke-net-methods-from-javascript-functions) özniteliği tarafından belirtilen .net yöntemlerine çağrı başlatabilir. Bu .NET yöntemleri işlenmeyen bir özel durum oluşturur:

* Özel durum devre için önemli olarak değerlendirilmez.
* JavaScript tarafı `Promise` reddedilir.

.NET tarafında ya da yöntem çağrısının JavaScript tarafında hata işleme kodu kullanma seçeneğiniz vardır.

Daha fazla bilgi için bkz. <xref:blazor/javascript-interop>.

### <a name="circuit-handlers"></a>Devre işleyicileri

Blazor sunucusu, kodun bir Kullanıcı devresi durumunda değişiklikler üzerinde kod çalıştırmaya izin veren bir *devhandler*tanımlamasına olanak tanır. Devre işleyicisi, `CircuitHandler` türeterek ve sınıfı uygulamanın hizmet kapsayıcısına kaydettirilerek uygulanır. Bir devre işleyicinin aşağıdaki örneği açık SignalR bağlantılarını izler:

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> _circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        _circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => _circuits.Count;
}
```

Devre işleyicileri DI kullanılarak kaydedilir. Kapsamlı örnekler, bir devrenin örneği başına oluşturulur. Yukarıdaki örnekte `TrackingCircuitHandler` kullanarak, tüm devrelerin durumu izlenmelidir çünkü bir tek hizmet oluşturulur:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

Özel bir devre işleyicisindeki Yöntemler işlenmeyen bir özel durum oluşturuyorsam, özel durum Blazor sunucusu devresi için önemli olur. Bir işleyicinin kodundaki veya yöntemleri çağrılan özel durumlara tolerans sağlamak için kodu bir veya daha fazla [try-catch](/dotnet/csharp/language-reference/keywords/try-catch) deyiminde hata işleme ve günlüğe kaydetme ile sarın.

### <a name="circuit-disposal"></a>Devre elden çıkarma

Bir kullanıcının bağlantısı kesilmediği ve Framework devre durumunu temizlemede bir devre dışı bırakıldığında, çerçeve devre dışı bırakıldı. Kapsamı elden atılırken, <xref:System.IDisposable?displayProperty=fullName>uygulayan hiçbir devre kapsamlı DI hizmeti yok. Herhangi bir DI hizmeti, elden çıkarma sırasında işlenmeyen bir özel durum oluşturursa, çerçeve özel durumu günlüğe kaydeder.

### <a name="prerendering"></a>Prerendering

Blazor bileşenleri, `Component` etiketi Yardımcısı kullanılarak, işlenmiş HTML biçimlendirmesinin kullanıcının ilk HTTP isteğinin bir parçası olarak döndürülmesi için önceden kullanılabilir. Bu şu şekilde geçerlidir:

* Aynı sayfanın parçası olan tüm ön işlenmiş bileşenler için yeni bir devre oluşturma.
* İlk HTML oluşturuluyor.
* Kullanıcı tarayıcısının aynı sunucuya geri SignalR bir bağlantı kuruncaya kadar devreyi `disconnected` olarak kabul edin. Bağlantı oluşturulduğunda, devre üzerindeki etkileşim sürdürülür ve bileşenlerin HTML işaretlemesi güncelleştirilir.

Herhangi bir bileşen prerendering sırasında, örneğin bir yaşam döngüsü yöntemi veya işleme mantığı sırasında işlenmeyen bir özel durum oluşturursa:

* Bu, devre için önemli bir durumdur.
* Özel durum, `Component` Tag Yardımcısı 'ndan çağrı yığınını ortaya atılır. Bu nedenle, özel durum geliştirici kodu tarafından açıkça yakalanmadığı takdirde tüm HTTP isteği başarısız olur.

Normal koşullarda, prerendering başarısız olduğunda bileşeni oluşturma ve işleme devam etmek, çalışan bir bileşen işlenemediği için mantıklı değildir.

Prerendering sırasında oluşabilecek hatalara tolerans sağlamak için hata işleme mantığı özel durum oluşturabilecek bir bileşenin içine yerleştirilmelidir. [Try-catch](/dotnet/csharp/language-reference/keywords/try-catch) deyimlerini hata işleme ve günlüğe kaydetme ile kullanın. Bir `try-catch` bildiriminde `Component` etiketi yardımcısını sarmalama yerine, `Component` etiketi Yardımcısı tarafından işlenen bileşene hata işleme mantığını koyun.

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

### <a name="recursive-rendering"></a>Özyinelemeli işleme

Bileşenler yinelemeli olarak iç içe olabilir. Bu, özyinelemeli veri yapılarını temsil etmek için kullanışlıdır. Örneğin, bir `TreeNode` bileşeni, düğüm alt öğelerinin her biri için daha fazla `TreeNode` bileşeni işleyebilir.

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

Çoğu Blazor bileşen *. Razor* dosyaları olarak uygulanır ve çıktılarını oluşturmak için `RenderTreeBuilder` üzerinde çalışan Logic üretmek için derlenir. Bir geliştirici, yordamsal C# kodu kullanarak `RenderTreeBuilder` mantığını el ile uygulayabilir. Daha fazla bilgi için bkz. <xref:blazor/components#manual-rendertreebuilder-logic>.

> [!WARNING]
> El ile işleme ağacı Oluşturucu mantığının kullanımı, genel bileşen geliştirme için önerilmeyen gelişmiş ve güvenli olmayan bir senaryo olarak değerlendirilir.

`RenderTreeBuilder` kod yazılmışsa, geliştirici kodun doğruluğunu garanti etmelidir. Örneğin, geliştirici şunları sağlamalıdır:

* `OpenElement` ve `CloseElement` çağrıları doğru dengelenir.
* Öznitelikler yalnızca doğru yerlere eklenir.

Hatalı elle işleme ağacı Oluşturucu mantığı kilitlenmeler, sunucu askıda kalma ve güvenlik açıkları dahil rastgele tanımsız davranışa neden olabilir.

El ile işleme ağacı Oluşturucu mantığını aynı karmaşıklık düzeyinde ve aynı *tehlike* düzeyiyle, el ile derleme kodu veya MSIL yönergeleri yazarak değerlendirin.
