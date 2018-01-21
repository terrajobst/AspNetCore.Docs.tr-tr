---
title: "ASP.NET Core bağımlılık ekleme"
author: ardalis
description: "ASP.NET Core bağımlılık ekleme nasıl uyguladığını ve nasıl kullanılacağını öğrenin."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1da3d557c48921747634b08cedb518184fb5f963
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>ASP.NET Core bağımlılık ekleme giriş

<a name="fundamentals-dependency-injection"></a>

Tarafından [Steve Smith](https://ardalis.com/) ve [Scott Addie](https://scottaddie.com)

ASP.NET Core sıfırdan yukarı desteklemek ve bağımlılık ekleme yararlanmak için tasarlanmıştır. ASP.NET Core uygulamaları başlangıç sınıfı yöntemleri içine eklenen sağlayarak yerleşik bir çerçeve Hizmetleri yararlanabilir ve uygulama hizmetleri de ekleme işlemi için yapılandırılabilir. En az bir özellik ayarlama ve diğer kapsayıcılar değiştirmek üzere tasarlanmamıştır ASP.NET Core tarafından sağlanan varsayılan Hizmetleri kapsayıcı sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>Bağımlılık ekleme nedir?

Bağımlılık ekleme (dı), nesneleri ve ortak çalışanlar veya bağımlılıkları arasındaki bu sıkı bağ elde etmek için kullanılan bir tekniktir. Ortak Çalışanlar doğrudan başlatmasını veya statik başvuruları kullanma yerine eylemlerini gerçekleştirmek için bir sınıf gereken nesneleri şekilde sınıfında sağlanmaktadır. Sınıfları bağımlılıklarını izlemek vermeden kendi Oluşturucusu aracılığıyla çoğunlukla tanımlayacağınız [açık bağımlılıkları ilkesine](http://deviq.com/explicit-dependencies-principle/). Bu yaklaşım "Oluşturucu ekleme" bilinir.

Sınıflar ile DI göz önünde tasarlanmıştır, kendi ortak çalışanlar üzerinde doğrudan, sabit kodlanmış bağımlılıkları yoktur çünkü bunlar daha geniş bağlı değildir. Bunu izleyen [bağımlılık ters çevirmeyi ilkesine](http://deviq.com/dependency-inversion-principle/), hangi durumları *"yüksek düzey modüller düşük düzey modülleri değil bağımlı; her ikisi de soyutlamalar üzerinde bağımlı olmalıdır."* Belirli uygulamaları başvuran yerine sınıfları isteği soyutlamalar (genellikle `interfaces`) sağlanan onlara sınıfı oluşturulduğunda. Bağımlılıklar arabirimleri içine ayıklanması ve parametre olarak bu arabirimleri uygulamalarını sağlayarak örneğidir de [stratejisi tasarım deseni](http://deviq.com/strategy-design-pattern/).

Bir sistem dı kullanmak üzere tasarlanmış, bağımlılıklarını kendi Oluşturucusu (veya Özellikler) aracılığıyla isteyen çok sayıda sınıflarla, bunların ilişkili bağımlılıkları olan bu sınıfları oluşturmak için adanmış bir sınıf yararlıdır. Bu sınıfların denir *kapsayıcıları*, ya da daha açık belirtmek gerekirse [denetimi tersine çevirme (IOC)](http://deviq.com/inversion-of-control/) kapsayıcıları veya bağımlılık ekleme (dı) kapsayıcı. Buradan istenen türlerin örnekleri sağlamaktan sorumludur Fabrika aslında bir kapsayıcıdır. Belirli bir türde bağımlılıklara sahiptir ve kapsayıcı bağımlılık türleri sağlayacak şekilde yapılandırılmış belirtmiş, bağımlılıklar istenen örneği oluşturma bir parçası olarak oluşturur. Bu şekilde, tüm sabit kodlanmış nesne oluşturması gerek kalmadan sınıflarına karmaşık bağımlılık grafikleri sağlanabilir. Kendi bağımlılıkları olan nesneler oluşturmaya ek olarak, kapsayıcı nesne yaşam süresi uygulama içinde genellikle yönetin.

ASP.NET Core içeren basit bir yerleşik kapsayıcı (tarafından temsil edilen `IServiceProvider` arabirimi) varsayılan oluşturucu ekleme destekleyen ve ASP.NET belirli hizmetleri dı aracılığıyla kullanılabilir yapar. ASP. NET'in kapsayıcı yönetir olarak türleri başvurduğu *Hizmetleri*. Bu makalenin kalanında *Hizmetleri* ASP.NET Core'nın IOC kapsayıcı tarafından yönetilen türlerine başvurur. Yerleşik kapsayıcının Hizmetleri'nde yapılandırma `ConfigureServices` uygulamanızın yönteminde `Startup` sınıfı.

> [!NOTE]
> Martin Fowler yazma kapsamlı bir makale üzerinde [denetimi tersine çevirme kapsayıcıları ve bağımlılık ekleme düzeni](https://www.martinfowler.com/articles/injection.html). Microsoft Patterns and Practices Ayrıca, harika bir açıklamaya sahip [bağımlılık ekleme](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Bu makalede, tüm ASP.NET uygulamaları için geçerli olduğundan bağımlılık ekleme yer almaktadır. Bağımlılık ekleme MVC denetleyicileri içinde kapsanan [bağımlılık ekleme ve denetleyiciler](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Oluşturucusu ekleme işlemi davranışı

Oluşturucu ekleme, söz konusu Oluşturucusu olmasını gerektirir *ortak*. Aksi takdirde, uygulamanızı özel durum oluşturacak bir `InvalidOperationException`:

> 'YourType' türü için uygun bir oluşturucu bulunamadı. Türü somut ve Hizmetleri bir public oluşturucuya tüm parametreler için kayıtlı emin olun.


Bu yalnızca bir geçerli Oluşturucusu mevcut Oluşturucu ekleme gerektirir. Oluşturucu aşırı desteklenir, ancak yalnızca bir aşırı yüklemesi olan bağımsız değişkenler tüm tarafından bağımlılık ekleme yerine getirmenin bulunabilir. Birden fazla varsa, uygulamanızı özel durum oluşturacak bir `InvalidOperationException`:

> Tüm belirtilen bağımsız değişken türleri kabul birden çok oluşturucular türü 'YourType' bulundu. Yalnızca geçerli bir oluşturucusu olmalıdır.

Oluşturucular bağımlılık ekleme tarafından sağlanmayan bağımsız değişkenleri kabul edebilir, ancak bunlar varsayılan değerlerini desteklemesi gerekir. Örneğin:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Framework tarafından sağlanan hizmetleri kullanma

`ConfigureServices` Yönteminde `Startup` sınıftır uygulama kullanır, Entity Framework Core ve ASP.NET Core MVC gibi platform özellikleri dahil olmak üzere hizmetleri tanımlamak için sorumlu. Başlangıçta, `IServiceCollection` için sağlanan `ConfigureServices` tanımlı aşağıdaki hizmetleri sahiptir (bağlı olarak [konak nasıl yapılandırılan](xref:fundamentals/hosting)):

| Hizmet Türü | Ömür |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transient |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transient |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Transient |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Transient |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | singleton |

Genişletme yöntemleri gibi çok sayıda kullanarak kapsayıcı ek hizmetler eklemek nasıl bir örneği aşağıdadır `AddDbContext`, `AddIdentity`, ve `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

ASP.NET MVC gibi tarafından sağlanan ara yazılımı ve özellikler tek Ekle kullanmanın bir kuralı izleyin*ServiceName* tüm bu özellik tarafından gerekli hizmetleri kaydetmek için genişletme yöntemi.

>[!TIP]
> Belirli framework tarafından sağlanan hizmetlerin içindeki istek `Startup` parametresi listelerine - yöntemlerle bkz [uygulama başlangıç](startup.md) daha fazla ayrıntı için.

## <a name="registering-your-own-services"></a>Kendi Hizmetleri kaydediliyor

Kendi uygulama hizmetleri şu şekilde kaydedebilirsiniz. İlk genel tür kapsayıcıdan istenen türü (genellikle bir arabirim) temsil eder. İkinci genel türü tarafından kapsayıcı örneği ve bu tür isteklerini yerine getirmek için kullanılan somut türünü temsil eder.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Her `services.Add<ServiceName>` genişletme yöntemi ekler (ve büyük olasılıkla yapılandırır) Hizmetleri. Örneğin, `services.AddMvc()` MVC gerektirir Hizmetleri ekler. Genişletme yöntemleri yerleştirme bu kuralı izlemeniz önerilir `Microsoft.Extensions.DependencyInjection` hizmet kayıtlar grupları kapsüllemek için ad alanı.

`AddTransient` Yöntemi soyut türler bunu gerektiren her nesne için ayrı ayrı örneği somut Hizmetleri eşlemek için kullanılır. Bu hizmetin bilinir *ömrü*, ve ek ömrü seçenekler aşağıda açıklanmıştır. Kaydettiğiniz hizmetlerinin her biri için uygun bir yaşam süresi seçmek önemlidir. Hizmetinin yeni bir örneğini istediği her sınıf sağlanmalıdır? Bir örnek verilen web isteği kullanılsın mı? Veya tek bir örnek uygulama ömrü boyunca kullanılmalıdır?

Bu makalede örnek adlı karakter adları görüntüleyen basit bir denetleyici yok `CharactersController`. Kendi `Index` yöntemi uygulamada depolanan karakterler geçerli listesini görüntüler ve hiçbiri yoksa koleksiyon sayıda karakter ile başlatır. Bu uygulama Entity Framework Çekirdek kullansa unutmayın ve `ApplicationDbContext` sınıfı, hiçbiri denetleyicide görünen kendi kalıcılığı için. Bunun yerine, belirli veri erişim mekanizması bir arabirim soyutlanır `ICharacterRepository`, hangi aşağıdaki [havuz deseni](http://deviq.com/repository-pattern/). Örneği `ICharacterRepository` oluşturucu aracılığıyla istenir ve ardından gerekirse karakterleri erişmek için kullanılan bir özel alan atanmış.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

`ICharacterRepository` Denetleyicisinin gereksinim duyduğu çalışmak için iki yöntem tanımlar `Character` örnekleri.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Bu arabirim somut bir türde tarafından sırayla uygulanır `CharacterRepository`, yani çalışma zamanında kullanılır.

> [!NOTE]
> DI yolu ile kullanılan `CharacterRepository` tüm "depoları" ya da veri erişimi sınıfları, yalnızca uygulama hizmetlerinizi izleyin genel bir model bir sınıftır.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Unutmayın `CharacterRepository` isteklerini bir `ApplicationDbContext` kendi oluşturucusuna. Bunun gibi zincirleme bir şekilde sırayla kendi bağımlılıkları isteyen her istenen bağımlılık ile kullanılacak bağımlılık ekleme ait değil. Kapsayıcı, tüm bağımlılıklarıyla grafikte çözme ve tam olarak çözümlenmiş hizmeti döndürüyor sorumludur.

> [!NOTE]
> İstenen nesne ve tüm gerektirdiği nesneleri ve tüm bu gerektiren nesneleri oluşturma bazen olarak adlandırılır bir *Nesne grafiği*. Benzer şekilde, çözümlenmelidir bağımlılıkları toplu kümesini tipik olarak adlandırılır bir *bağımlılığı ağacı* veya *bağımlılık grafiğinin*.

Bu durumda, her ikisi de `ICharacterRepository` ve dolayısıyla `ApplicationDbContext` hizmetler kapsayıcısının ile kayıtlı olması gerekir `ConfigureServices` içinde `Startup`. `ApplicationDbContext`genişletme yöntemi çağrısı ile yapılandırılmış `AddDbContext<T>`. Aşağıdaki kod kaydını gösterir `CharacterRepository` türü.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Entity Framework bağlamları Hizmetleri kullanarak kapsayıcı eklenmesi `Scoped` yaşam süresi. Bu otomatik olarak yukarıda gösterildiği gibi yardımcı yöntemler kullanırsanız dikkate. Entity Framework'ü kullanın yapacak depoları aynı yaşam süresini kullanmanız gerekir.

>[!WARNING]
> Çok dikkatli olmak ana tehlike çözümlenirken bir `Scoped` tek gelen hizmet. Hizmet hatalı durum istekler işlenirken gerekir çalışması olasıdır.

Bağımlılıkları olan hizmetleri bunları kapsayıcısında kaydetmeniz. Bir hizmetin yapıcı, ilkel gibi gerektiriyorsa bir `string`, bunu kullanarak yerleştirilebilir [yapılandırma](xref:fundamentals/configuration/index) ve [seçenekleri düzeni](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Hizmet yaşam süresi ve kayıt seçenekleri

ASP.NET Hizmetleri ile aşağıdaki ömürleri yapılandırılabilir:

**Geçici**

Geçici ömrü hizmetleri talep edilen her zaman oluşturulur. Bu ömrü basit, durum bilgisi olmayan hizmetler için en iyi şekilde çalışır.

**Kapsamlı**

Kapsamlı ömrü Hizmetleri istek başına bir kez oluşturulur.

**Singleton**

Singleton ömrü Hizmetleri istenen ilk kez oluşturulur (veya ne zaman `ConfigureServices` bir örneği var. belirtirseniz çalıştırın) ve ardından sonraki her istek aynı örneğini kullanır. Uygulamanızı singleton davranışı gerektiriyorsa, hizmetin ömrünü yönetmek hizmet kapsayıcı izin vererek singleton tasarım deseni uygulama ve sınıf, nesnenin yaşam süresi kendiniz yönetmek yerine önerilir.

Hizmetleri, çeşitli şekillerde kapsayıcısı ile kaydedilebilir. Biz, zaten bir hizmet uygulaması ile belirli bir türde kullanmak için somut türde belirterek nasıl gördünüz. Ayrıca, bir Fabrika, ardından isteğe bağlı olarak örnek oluşturmak için kullanılacak olan belirtilebilir. Üçüncü yaklaşım, kapsayıcı durumu hiçbir zaman bir örnek oluşturmaya çalışacak (ya da örneği dispose) kullanmak için türün örneğini doğrudan belirtmektir.

Bu yaşam süresi ve kaydını seçenekleri arasındaki farkı göstermek için bir veya daha çok görev olarak temsil eden basit bir arabirim göz önünde bulundurun. bir *işlemi* benzersiz bir tanımlayıcı `OperationId`. Nasıl Biz bu hizmet için kullanım ömrünü nasıl yapılandırdığınıza bağlı olarak, kapsayıcı ya da aynı veya farklı örneklerini isteyen sınıfına hizmet sağlar. Hangi ömrü istenen temizleyin yapmak için bir türü ömür seçeneği başına oluşturacağız:

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Biz bu arabirimleri kullanarak tek bir sınıf uygulama `Operation`, kabul eden bir `Guid` kendi Oluşturucusu ya da yeni bir kullanır `Guid` hiçbiri sağlanmazsa.

Ardından `ConfigureServices`, her tür adlandırılmış yaşam göre kapsayıcı eklenir:

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Unutmayın `IOperationSingletonInstance` hizmet bilinen bir kimliği ile belirli bir örneği kullanarak `Guid.Empty` bu türü (GUID'sine tümüyle sıfırlardan olacaktır) kullanımda olduğunda Temizle olur. Biz de kayıtlı bir `OperationService` her, diğer bağımlıdır `Operation` türleri temizleyin isteği içinde bu hizmet denetleyicisi ya da her işlem türü için yeni bir tane olarak aynı örneği olup olmadığını alma olmayacaktır. Bu hizmeti yapar şey görünümünde görüntülenen şekilde bağımlılıklarını özellikleri olarak kullanıma sunar.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Nesne yaşam süresi içinde ve uygulama için ayrı ayrı istekler arasında göstermek için örnek içeren bir `OperationsController` her tür isteklerine `IOperation` türü yanı sıra bir `OperationService`. `Index` Sonra eylemi görüntüler tüm denetleyicinin ve hizmetin `OperationId` değerleri.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Şimdi iki ayrı Bu denetleyici eylemi için yapılan isteklerinin:

![Microsoft Edge geçici, kapsamındaki, tek ve örnek denetleyicisi ve işlem hizmeti işlemleri için ilk istek işlemi kimliği değerleri (GUID'ın) gösteren çalışan bağımlılık ekleme örnek web uygulaması işlemleri görünümü.](dependency-injection/_static/lifetimes_request1.png)

![İkinci bir istek için işlem kimliği değerleri gösteren işlemlerini görüntüleyin.](dependency-injection/_static/lifetimes_request2.png)

Hangi gözlemlemek `OperationId` değerleri, bir istek içinde ve istekler arasında değişir.

* *Geçici* nesneleri farklı her zaman; her denetleyici ve her hizmet için yeni bir örnek sağlanır.

* *Kapsamlı* nesneleri aynıdır ancak farklı istekler arasında farklı bir istek içinde

* *Singleton* nesneleridir aynı her nesne ve her istek için (örneği içinde olup olmadığını sağlanan bağımsız olarak `ConfigureServices`)

## <a name="request-services"></a>İstek Hizmetleri

Bir ASP.NET içinde kullanılabilir hizmetler isteği `HttpContext` aracılığıyla sunulan `RequestServices` koleksiyonu.

![İstek Hizmetleri alır veya ayarlar isteğin hizmet kapsayıcısı erişim sağlayan IServiceProvider belirten HttpContext isteği Hizmetleri IntelliSense bağlamsal iletişim kutusu.](dependency-injection/_static/request-services.png)

İstek hizmetleri yapılandırmak ve uygulamanızı bir parçası olarak istek Hizmetleri temsil eder. Nesnelerinizi bağımlılıklarını belirttiğinizde, bunlar bulunan tür tarafından karşılanır `RequestServices`değil `ApplicationServices`.

Genellikle, bunun yerine sınıfınızın oluşturucu aracılığıyla gerektiren sınıflarınızı istek türleri tercih ederek ve bu bağımlılıklar Ekle framework izin vererek, doğrudan bu özellikleri kullanmamalısınız. Bu test etmek daha kolay olan sınıfları verir (bkz [test](../testing/index.md)) ve daha geniş bağlı değildir.

> [!NOTE]
> Erişim için Oluşturucusu parametre olarak bağımlılıkları isteyen tercih `RequestServices` koleksiyonu.

## <a name="designing-your-services-for-dependency-injection"></a>Hizmetlerinizin bağımlılık ekleme için tasarlama

Bağımlılık ekleme kendi ortak çalışanlar almak için kullanılacak hizmetlerinizi tasarlamanız gerekir. Bu durum bilgisi olan statik yöntem çağrılarını kullanımını önleme anlamına gelir (hangi neden olarak bilinen bir kod kokusu [statik cling](http://deviq.com/static-cling/)) ve hizmetlerinizi içinde bağımlı sınıfların doğrudan örnek oluşturma. Tümcecik anımsaması yardımcı olabilecek [Birleştirici yenilikleri](https://ardalis.com/new-is-glue), bir türü örneği mi, yoksa bağımlılık ekleme istemek için seçerken. İzleyerek [DÜZ ilkeler, nesne yönelimli tasarımı](http://deviq.com/solid/), sınıflarınızı doğal olarak küçük, iyi faktörlere göre ve kolayca sınanan olma eğilimindedir.

Ne sınıflarınızı şekilde eklenmiş olan çok sayıda bağımlılıklara sahip olma eğilimi gösterir bulduğunuz? Bu, genellikle sınıfınız çok işlemini yapmaya çalışan ve büyük olasılıkla SRP - ihlal eden bir oturum [tek sorumluluk ilkesine](http://deviq.com/single-responsibility-principle/). Yeni bir sınıf bazı sorumlulukları taşıyarak sınıfı yeniden varsa bkz. Unutmayın, `Controller` sınıfları odaklanmış, UI sorunlarını iş kuralları ve veri erişim uygulama ayrıntılarını bunları uygun sınıflardaki tutulması gerektiği şekilde [ayrı sorunları](http://deviq.com/separation-of-concerns/).

Veri erişimi göre özellikle ekleyemezsiniz `DbContext` denetleyicilerinizi içine (varsayılarak hizmetler kapsayıcısının EF ekledik `ConfigureServices`). Bazı geliştiriciler veritabanına bir depo arabirimi kullanmayı tercih ederseniz injecting yerine `DbContext` doğrudan. Veri kapsülleyen bir arabirimini kullanarak tek bir yerde erişim mantığı veritabanınızı değiştiğinde değiştirmeniz gerekecektir basamak sayısını en aza indirebilirsiniz.

### <a name="disposing-of-services"></a>Hizmetlerin atma

Kapsayıcı çağıracak `Dispose` için `IDisposable` türleri oluşturur. Bir örneği kapsayıcıya kendiniz eklerseniz, ancak bunu silinip.

Örnek:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> Sürüm 1.0, kapsayıcı adlı dispose üzerinde *tüm* `IDisposable` nesneleri, onu oluşturmadı dahil olmak üzere.

## <a name="replacing-the-default-services-container"></a>Varsayılan hizmet kapsayıcı değiştirme

Yerleşik hizmet kapsayıcı framework üzerinde oluşturulan çoğu tüketici uygulamalar ve temel gereksinimlerini karşılamak için tasarlanmıştır. Ancak, geliştiricilerin kendi tercih edilen kapsayıcı ile yerleşik kapsayıcı değiştirebilirsiniz. `ConfigureServices` Yöntemi genellikle döndürür `void`, ancak imzası döndürülecek değiştirilirse `IServiceProvider`, farklı bir kapsayıcı yapılandırılmış ve döndürdü. .NET için birçok IOC kapsayıcıları vardır. Bu örnekte, [Autofac](https://autofac.org/) paketi kullanılır.

Önce uygun bir kapsayıcı paketler yükleyin:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Ardından, kapsayıcısında yapılandırın `ConfigureServices` ve dönüş bir `IServiceProvider`:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> Bir üçüncü taraf dı kapsayıcı kullanırken değiştirmelisiniz `ConfigureServices` döndürdüğü böylece `IServiceProvider` yerine `void`.

Son olarak, normal olarak Autofac yapılandırma `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Çalışma zamanında Autofac türleri çözümlemek ve bağımlılıkları ekleme için kullanılır. [Autofac ve ASP.NET Core kullanma hakkında daha fazla bilgi](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>İş parçacığı güvenliği

Singleton Hizmetleri iş parçacığı içinde korumalı olması gerekir. Tek bir hizmet üzerinde geçici bir hizmet bağımlılık varsa, geçici hizmet iş parçacığı tarafından singleton nasıl kullanıldığı bağlı olarak güvenli olacak şekilde de gerekebilir.

## <a name="recommendations"></a>Önerileri

Bağımlılık ekleme ile çalışırken, aşağıdaki önerileri göz önünde bulundurun:

* DI, karmaşık bağımlılıklara sahip nesneleri için durumdadır. Denetleyicileri, hizmetleri, bağdaştırıcılar ve depoları için dı eklenebilecek nesneler için tüm örnektir.

* Verileri ve yapılandırma doğrudan dı içinde saklamaktan kaçının. Örneğin, bir kullanıcının alışveriş sepeti Hizmetleri kapsayıcıya genellikle eklenmesi gerekir. Yapılandırma kullanması gereken [seçenekleri düzeni](xref:fundamentals/configuration/options). Benzer şekilde, yalnızca başka bir nesne erişmesine izin vermek için mevcut "veri sahibi" nesneleri kaçının. İstek dı mümkünse gereken gerçek öğesi daha iyidir.

* Statik hizmetlerine erişim kaçının.

* Hizmet konumu, uygulama kodunuzda kaçının.

* Statik erişimi önlemek `HttpContext`.

> [!NOTE]
> Gibi tüm ayarlar önerilerin bir yoksayılıyor gerekli olduğu durumlar karşılaşabilirsiniz. Nadir--istisnaları framework çoğunlukla çok özel durumlarını bulduk.

Bağımlılık ekleme olduğunu unutmayın bir *alternatif* statik/genel nesne erişim desenler için. Statik nesne erişimi ile karıştırmak istiyorsanız dı faydaları hayata geçirmek mümkün olmaz.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Uygulama Başlatma](startup.md)

* [Test etme](../testing/index.md)

* [Bağımlılık ekleme (MSDN) ile ASP.NET Core temiz kod yazma](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [Kapsayıcı yönetilen uygulama tasarımı, Prelude: Burada kapsayıcı ait mu?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [Açık bağımlılıkları İlkesi](http://deviq.com/explicit-dependencies-principle/)

* [Denetimi kapsayıcıları ve bağımlılık ekleme düzeni ters çevirmeyi](https://www.martinfowler.com/articles/injection.html) (Fowler)
