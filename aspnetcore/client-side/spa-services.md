---
title: ASP.NET Core içinde tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanın
author: scottaddie
description: ASP.NET Core tarafından desteklenen tek sayfalı uygulama (SPA) oluşturmak için JavaScript Hizmetleri kullanmanın avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: c0c73882afd579510ad9cdf5b485c1d6fbeadd1c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663781"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>ASP.NET Core içinde tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanın

[Scott Ade](https://github.com/scottaddie) ve [fiyaz hasan](https://fiyazhasan.me/) tarafından

Tek sayfa uygulama (SPA) web uygulamasının kendi devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türdür. ASP.NET Core gibi sunucu tarafı çerçeveleri ile, [angular](https://angular.io/) veya yanıt verme gibi ISTEMCI tarafı Spa [çerçevelerini veya kitaplıklarını](https://facebook.github.io/react/)tümleştirme zor olabilir. Tümleştirme sürecinde uçuşmayı azaltmak için JavaScript Hizmetleri geliştirilmiştir. Ancak, farklı istemci ve sunucu teknoloji yığınları arasında sorunsuz bir işlemi etkinleştirir.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Bu makalede açıklanan özellikler ASP.NET Core 3,0 itibariyle kullanımdan kalkmıştır. [Microsoft. AspNetCore. SpaServices. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) NuGet paketinde daha basıt bir spa çerçeveleri tümleştirme mekanizması mevcuttur. Daha fazla bilgi için bkz. [Microsoft. aspnetcore. spaservices ve Microsoft. AspNetCore. NodeServices üzerinde kullanımdan bulunan [Duyuru]](https://github.com/dotnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>JavaScript Hizmetleri nedir?

JavaScript Hizmetleri, ASP.NET Core yönelik istemci tarafı teknolojilerinin bir koleksiyonudur. ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için hedefi sağlamaktır.

JavaScript Hizmetleri iki ayrı NuGet paketi içerir:

* [Microsoft. AspNetCore. nodeservices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (nodeservices)
* [Microsoft. AspNetCore. spaservices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (spaservices)

Bu paketler, aşağıdaki senaryolarda faydalıdır:

* Sunucu üzerinde JavaScript çalıştırma
* SPA altyapı veya kitaplığı kullanın
* İstemci tarafı Web varlıklarla oluşturun

Bu makalenin odak çoğunu SpaServices paketini kullanarak yerleştirilir.

## <a name="what-is-spaservices"></a>SpaServices nedir

ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için SpaServices oluşturulur. Istenmeyen hizmetler ASP.NET Core ile maça geliştirmek için gerekli değildir ve geliştiricileri belirli bir istemci çerçevesinde kilitlemez.

SpaServices gibi kullanışlı bir altyapı sağlar:

* [Sunucu tarafı prerendering](#server-side-prerendering)
* [Web paketi geliştirme ara yazılımı](#webpack-dev-middleware)
* [Sık kullanılan modül değiştirme](#hot-module-replacement)
* [Yönlendirme Yardımcıları](#routing-helpers)

Toplu olarak, bu altyapı bileşenlerini, hem geliştirme iş akışını hem de çalışma zamanı deneyimi geliştirin. Bileşenleri ayrı ayrı önemsenmeksizin devralınabilir.

## <a name="prerequisites-for-using-spaservices"></a>SpaServices kullanmanın önkoşulları

SpaServices ile çalışmak için aşağıdakileri yükleyin:

* NPM ile [Node. js](https://nodejs.org/) (sürüm 6 veya üzeri)

  * Bu bileşenler yüklenir ve bulunabilir doğrulamak için komut satırından aşağıdaki komutu çalıştırın:

    ```console
    node -v && npm -v
    ```

  * Bir Azure Web sitesine dağıtım yapıyorsanız bir işlem gerekmez&mdash;Node. js yüklenir ve sunucu ortamlarında kullanılabilir.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Visual Studio 2017 kullanarak Windows 'da SDK, **.NET Core platformlar arası geliştirme** iş yükü seçilerek yüklenir.

* [Microsoft. AspNetCore. SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi

## <a name="server-side-prerendering"></a>Sunucu tarafı prerendering

Bir evrensel (isomorphic olarak da bilinir) uygulama özellikli olan hem çalışan sunucu ve istemci üzerindeki bir JavaScript uygulamasıdır. Angular, React ve diğer popüler çerçeveleri için bu uygulama geliştirme stili Evrensel bir platform sağlar. İlk Node.js aracılığıyla sunucuda framework bileşenleri oluşturma ve yürütme istemciye daha fazla temsilci olur.

SpaServices tarafından sunulan ASP.NET Core [Etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) , sunucuda JavaScript işlevlerini çağırarak sunucu tarafı prerendering 'in uygulanmasını basitleştirir.

### <a name="server-side-prerendering-prerequisites"></a>Sunucu tarafı prerendering önkoşulları

[ASPNET-prerendering](https://www.npmjs.com/package/aspnet-prerendering) NPM paketini yükler:

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Sunucu tarafı prerendering yapılandırması

Etiket Yardımcıları, projenin *_ViewImports. cshtml* dosyasında ad alanı kaydı aracılığıyla bulunabilir hale getirilir:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Bu etiket Yardımcıları hemen bir HTML benzeri sözdizimi Razor görünüm içinde yararlanarak alt düzey API'ler ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>ASP-PreRender-Module etiketi Yardımcısı

Yukarıdaki kod örneğinde kullanılan `asp-prerender-module` etiketi Yardımcısı, Node. js aracılığıyla sunucuda *clientapp/Dist/Main-Server. js* ' yi yürütür. Netme 'nin sake, *Main-Server. js* dosyası, [WebPack](https://webpack.github.io/) derleme sürecinde TypeScript-to-JavaScript transpilation görevinin yapıtı. WebPack `main-server`; bir giriş noktası diğer adını tanımlar. ve, bu diğer ad için bağımlılık grafiğinin çapraz geçişi *clientapp/Boot-Server. TS* dosyasında başlar:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Aşağıdaki angular örneğinde, *clientapp/Boot-Server. TS* dosyası, Node. js aracılığıyla sunucu işlemesini yapılandırmak için `aspnet-prerendering` NPM paketinin `createServerRenderer` işlevini ve `RenderResult` türünü kullanır. Sunucu tarafı işlemeye yönelik HTML biçimlendirmesi, kesin belirlenmiş bir JavaScript `Promise` nesnesine Sarmalanan bir Resolve işlev çağrısına geçirilir. `Promise` nesnesinin önemi, DOM 'ın yer tutucu öğesine ekleme için sayfaya HTML işaretlemesini zaman uyumsuz olarak sağlar.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-PreRender-veri etiketi Yardımcısı

`asp-prerender-module` Tag Yardımcısı ile birlikte kullanıldığında, Razor görünümündeki bağlama bilgilerini sunucu tarafı JavaScript 'e geçirmek için `asp-prerender-data` etiketi Yardımcısı kullanılabilir. Örneğin, aşağıdaki biçimlendirme Kullanıcı verilerini `main-server` modülüne geçirir:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Alınan `UserName` bağımsız değişkeni yerleşik JSON seri hale getirici kullanılarak serileştirilir ve `params.data` nesnesinde depolanır. Aşağıdaki angular örneğinde, veriler `h1` öğesi içinde kişiselleştirilmiş bir selamlama oluşturmak için kullanılır:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Etiket yardımcılarının geçirildiği Özellik adları **PascalCase** gösterimi ile temsil edilir. Aynı özellik adlarının **camelCase**ile temsil edildiği JavaScript 'e kontrast. Bu fark için varsayılan JSON seri hale getirme yapılandırması sorumludur.

Yukarıdaki kod örneğini genişletmek için, `resolve` işlevine sunulan `globals` özelliğini hibir şekilde görüntüleyerek, veriler sunucudan görünüme geçirilebilir:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`globals` nesnesinin içinde tanımlanan `postList` dizisi tarayıcının Global `window` nesnesine iliştirilir. Bu değişken hoisting genel kapsam için aynı verileri bir kez sunucuda ve istemcinin yeniden yüklenmesi için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.

![Pencere nesnesi bağlı genel postList değişkeni](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Web geliştirme ara yazılımı

[Web paketi geliştirme ara yazılımı](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) , Web paketinin kaynakları isteğe bağlı olarak oluşturmakta olan kolaylaştırılmış bir geliştirme iş akışı Ara yazılım, otomatik olarak derler ve bir sayfa tarayıcıya yeniden yüklendiğinde istemci-tarafı kaynaklar sunar. Başka bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Web projenin npm derleme betiği aracılığıyla el ile başlatmak için yaklaşımdır. *Package. JSON* dosyasındaki NPM derleme betiği aşağıdaki örnekte gösterilmiştir:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>WebPack dev ara yazılım önkoşulları

[ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) NPM paketini yükler:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>WebPack dev ara yazılım yapılandırması

Web paketi geliştirme ara yazılımı, *Startup.cs* dosyasının `Configure` yönteminde aşağıdaki kod aracılığıyla http istek ardışık düzenine kaydedilir:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

`UseStaticFiles` uzantısı yöntemiyle [statik dosya barındırma](xref:fundamentals/static-files) kaydedilmeden önce `UseWebpackDevMiddleware` uzantısı yönteminin çağrılması gerekir. Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.

*WebPack. config. js* dosyasının `output.publicPath` özelliği, ara yazılıma, değişiklikler için `dist` klasörünü izlemesini söyler:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Sık erişimli modülü değiştirme

WebPack [geliştirme ara yazılımı](#webpack-dev-middleware)'nın bir evrimi olarak Web paketi 'Nin [sık kullanılan modül değiştirme](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) özelliğini düşünün. HMR aynı avantajları sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır. Bu, geçerli bellek içi durumu ve hata ayıklama oturumu SPA ile neden tarayıcı yenileme ile karıştırmayın. Web geliştirme ara yazılım hizmeti ve değişiklikleri tarayıcıya gönderilmesini anlamına gelir tarayıcı arasında canlı bir bağlantı yoktur.

### <a name="hot-module-replacement-prerequisites"></a>Sık kullanılan modül değiştirme önkoşulları

[WebPack-Hot-ara yazılım](https://www.npmjs.com/package/webpack-hot-middleware) NPM paketini yükler:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Sık kullanılan modül değiştirme yapılandırması

HMR bileşeni, `Configure` yönteminde MVC 'nin HTTP isteği ardışık düzenine kayıtlı olmalıdır:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

[Web paketi geliştirme ara yazılımı](#webpack-dev-middleware)ile doğru olduğu için, `UseWebpackDevMiddleware` uzantısı yönteminin `UseStaticFiles` uzantı yönteminden önce çağrılması gerekir. Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.

*Web paketi. config. js* dosyası boş bırakılmış olsa bile bir `plugins` dizisi tanımlamalıdır:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Uygulamayı tarayıcıda yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:

![Sık erişimli modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Yönlendirme Yardımcıları

Çoğu ASP.NET Core tabanlı maça, istemci tarafı yönlendirme genellikle sunucu tarafı yönlendirmeye ek olarak istenir. SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir. Yoktur, ancak bir kenar büyük/küçük harf taşıyor sorunlarını: HTTP 404 yanıtları tanımlayan.

`/some/page` bir extensionın ' ın kullanıldığı senaryoyu göz önünde bulundurun. İstek deseni-sunucu tarafı rota match değil, ancak desenine bir istemci-tarafı rota ile eşleşmekte varsayılır. Artık, genellikle sunucusunda bir görüntü dosyası bulmayı bekleyen `/images/user-512.png`gelen isteği düşünün. İstenen kaynak yolu herhangi bir sunucu tarafı yolu veya statik dosya ile eşleşmiyorsa, istemci tarafı uygulamanın&mdash;bunu işleyemesi büyük olasılıkla 404 HTTP durum kodu döndürülüyor.

### <a name="routing-helpers-prerequisites"></a>Yönlendirme Yardımcıları önkoşulları

İstemci tarafı yönlendirme NPM paketini yükler. Angular örnek olarak kullanıp:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Yönlendirme Yardımcıları yapılandırması

`Configure` yönteminde `MapSpaFallbackRoute` adlı bir genişletme yöntemi kullanılır:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Yollar yapılandırıldıkları sırayla değerlendirilir. Sonuç olarak, önceki kod örneğinde `default` yolu, önce model eşleştirme için kullanılır.

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

JavaScript Hizmetleri önceden yapılandırılmış uygulama şablonları sağlar. Istenmeyen hizmetler, bu şablonlarda, angular, tepki ve Redux gibi farklı çerçeveler ve kitaplıklarla birlikte kullanılır.

Bu şablonlar, aşağıdaki komutu çalıştırarak, .NET Core CLI yüklenebilir:

```dotnetcli
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Kullanılabilir SPA şablonlar listesi görüntülenir:

| Şablonlar                                 | Kısa Ad | Dil | Etiketler        |
| ------------------------------------------| :--------: | :------: | :---------: |
| Angular ile ASP.NET Core MVC             | Angular    | [C#]     | MVC/Web/SPA |
| React.js ile ASP.NET Core MVC            | react      | [C#]     | MVC/Web/SPA |
| ASP.NET Core MVC React.js ve Redux  | açarken kilitlenmesi | [C#]     | MVC/Web/SPA |

SPA şablonlarından birini kullanarak yeni bir proje oluşturmak için, [DotNet New](/dotnet/core/tools/dotnet-new) komutuna şablonun **kısa adını** ekleyin. Aşağıdaki komut, sunucu tarafı için yapılandırılan ASP.NET Core MVC ile Angular bir uygulama oluşturur:

```dotnetcli
dotnet new angular
```

### <a name="set-the-runtime-configuration-mode"></a>Çalışma zamanı yapılandırma modunu ayarlayın

Birincil çalışma zamanı yapılandırma için iki mod vardır:

* **Geliştirme**:
  * Hata ayıklamayı kolaylaştırmak için kaynak eşlemeleri içerir.
  * İstemci tarafı kod performans için en iyi duruma değil.
* **Üretim**:
  * Kaynak eşlemeleri dışlar.
  * Paketleme ve küçültmeye göre istemci tarafı kodunu iyileştirir.

ASP.NET Core, yapılandırma modunu depolamak için `ASPNETCORE_ENVIRONMENT` adında bir ortam değişkeni kullanır. Daha fazla bilgi için bkz. [ortamı ayarlama](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>.NET Core CLI ile Çalıştır

Proje kök dizininde aşağıdaki komutu çalıştırarak gerekli NuGet ve npm paketlerini geri yükleyin:

```dotnetcli
dotnet restore && npm i
```

Derleme ve uygulamayı çalıştırın:

```dotnetcli
dotnet run
```

Uygulama, [çalışma zamanı yapılandırma moduna](#set-the-runtime-configuration-mode)göre localhost üzerinde başlatılır. Tarayıcıda `http://localhost:5000` gitme giriş sayfasını görüntüler.

### <a name="run-with-visual-studio-2017"></a>Visual Studio 2017 ile çalıştırma

[DotNet New](/dotnet/core/tools/dotnet-new) komutu tarafından oluşturulan *. csproj* dosyasını açın. Gerekli NuGet ve npm paketleri proje üzerinde otomatik olarak geri yüklenir. Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırmaya hazır bir uygulamadır. Yeşil çalışma düğmesine tıklayın veya `Ctrl + F5`tuşuna basın ve tarayıcı uygulamanın giriş sayfasında açılır. Uygulama, [çalışma zamanı yapılandırma moduna](#set-the-runtime-configuration-mode)göre localhost üzerinde çalışır.

## <a name="test-the-app"></a>Uygulamayı test edin

SpaServices şablonları, [karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/)kullanarak istemci tarafı testleri çalıştıracak şekilde önceden yapılandırılmıştır. Test Çalıştırıcısı bu testler için karma bilgileriyse jasmine bir popüler birim testi çerçevesi için JavaScript, ' dir. Karma, geliştiricilerin her değişiklik yapıldığında testi durdurmak ve çalıştırmak için gerekli olmaması gibi [WebPack dev ara yazılımı](#webpack-dev-middleware) ile birlikte çalışmak üzere yapılandırılmıştır. Test çalışması veya test çalışması karşı çalışan kod olup olmadığını test otomatik olarak çalıştırılır.

Angular uygulamasını örnek olarak kullanarak, *Counter. Component. spec. TS* dosyasında `CounterComponent` Için Iki Jasmine test çalışması zaten sunulmaktadır:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

*Clientapp* dizininde komut istemi ' ni açın. Şu komutu çalıştırın:

```console
npm test
```

Betik, *karma. conf. js* dosyasında tanımlanan ayarları okuyan karma Test Çalıştırıcısı 'nı başlatır. Diğer ayarların yanı sıra, *karma. conf. js* `files` dizisi aracılığıyla yürütülecek test dosyalarını tanımlar:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Azure 'da yayımlama hakkında daha fazla bilgi için [Bu GitHub sorununa](https://github.com/dotnet/AspNetCore.Docs/issues/12474) bakın.

Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştiren çok uğraşmayı gerektirebilir. Ktam, Spaservice, yayın sürecinin tamamını `RunWebpack`adlı özel bir MSBuild hedefi ile düzenler:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild hedefi aşağıdaki sorumluluklara sahiptir:

1. NPM paketlerini geri yükleyin.
1. Üçüncü taraf, istemci tarafı varlıkların üretim sınıfı derlemesini oluşturun.
1. Özel istemci tarafı varlıkların üretim sınıfı derlemesini oluşturma.
1. Web paketi tarafından oluşturulan varlıkları Yayımla klasörüne kopyalayın.

MSBuild hedefi çalıştırılırken çağrılır:

```dotnetcli
dotnet publish -c Release
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Angular belgeleri](https://angular.io/docs)
