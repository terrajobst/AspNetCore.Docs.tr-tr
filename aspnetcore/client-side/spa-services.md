---
title: ASP.NET Core içinde tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanın
author: scottaddie
description: ASP.NET Core tarafından desteklenen tek sayfalı uygulama (SPA) oluşturmak için JavaScript Hizmetleri kullanmanın avantajları hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 09/06/2019
uid: client-side/spa-services
ms.openlocfilehash: 16c9eb1d79bca792062d292795763c54dd02bd37
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773410"
---
# <a name="use-javascript-services-to-create-single-page-applications-in-aspnet-core"></a>ASP.NET Core içinde tek sayfalı uygulamalar oluşturmak için JavaScript hizmetlerini kullanın

Tarafından [Scott Addie](https://github.com/scottaddie) ve [Fiyaz Hasan](https://fiyazhasan.me/)

Tek sayfa uygulama (SPA) web uygulamasının kendi devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türdür. ASP.NET Core gibi sunucu tarafı çerçeveleri ile, [angular](https://angular.io/) veya yanıt verme gibi ISTEMCI tarafı Spa [çerçevelerini veya kitaplıklarını](https://facebook.github.io/react/)tümleştirme zor olabilir. Tümleştirme sürecinde uçuşmayı azaltmak için JavaScript Hizmetleri geliştirilmiştir. Ancak, farklı istemci ve sunucu teknoloji yığınları arasında sorunsuz bir işlemi etkinleştirir.

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> Bu makalede açıklanan özellikler ASP.NET Core 3,0 itibariyle kullanımdan kalkmıştır. [Microsoft. AspNetCore. SpaServices. Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices.Extensions) NuGet paketinde daha basıt bir spa çerçeveleri tümleştirme mekanizması mevcuttur. Daha fazla bilgi için bkz. [Microsoft. aspnetcore. spaservices ve Microsoft. AspNetCore. NodeServices üzerinde kullanımdan bulunan [Duyuru]](https://github.com/aspnet/AspNetCore/issues/12890).

::: moniker-end

## <a name="what-is-javascript-services"></a>JavaScript Hizmetleri nedir?

JavaScript Hizmetleri, ASP.NET Core yönelik istemci tarafı teknolojilerinin bir koleksiyonudur. ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için hedefi sağlamaktır.

JavaScript Hizmetleri iki ayrı NuGet paketi içerir:

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)

Bu paketler, aşağıdaki senaryolarda faydalıdır:

* Sunucu üzerinde JavaScript çalıştırma
* SPA altyapı veya kitaplığı kullanın
* İstemci tarafı Web varlıklarla oluşturun

Bu makalenin odak çoğunu SpaServices paketini kullanarak yerleştirilir.

## <a name="what-is-spaservices"></a>SpaServices nedir

ASP.NET Core geliştiricilerinin Spa'lar oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak yerleştirmek için SpaServices oluşturulur. Istenmeyen hizmetler ASP.NET Core ile maça geliştirmek için gerekli değildir ve geliştiricileri belirli bir istemci çerçevesinde kilitlemez.

SpaServices gibi kullanışlı bir altyapı sağlar:

* [Sunucu tarafı prerendering](#server-side-prerendering)
* [Web geliştirme ara yazılımı](#webpack-dev-middleware)
* [Sık erişimli modülü değiştirme](#hot-module-replacement)
* [Yönlendirme Yardımcıları](#routing-helpers)

Toplu olarak, bu altyapı bileşenlerini, hem geliştirme iş akışını hem de çalışma zamanı deneyimi geliştirin. Bileşenleri ayrı ayrı önemsenmeksizin devralınabilir.

## <a name="prerequisites-for-using-spaservices"></a>SpaServices kullanmanın önkoşulları

SpaServices ile çalışmak için aşağıdakileri yükleyin:

* [Node.js](https://nodejs.org/) (sürüm 6 veya sonrası) npm ile

  * Bu bileşenler yüklenir ve bulunabilir doğrulamak için komut satırından aşağıdaki komutu çalıştırın:

    ```console
    node -v && npm -v
    ```

  * Bir Azure Web sitesine dağıtım yapıyorsanız hiçbir işlem gerekli&mdash;değildir. js, sunucu ortamlarında yüklenir ve kullanılabilir.

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Visual Studio 2017 kullanarak Windows 'da SDK, **.NET Core platformlar arası geliştirme** iş yükü seçilerek yüklenir.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi

## <a name="server-side-prerendering"></a>Sunucu tarafı prerendering

Bir evrensel (isomorphic olarak da bilinir) uygulama özellikli olan hem çalışan sunucu ve istemci üzerindeki bir JavaScript uygulamasıdır. Angular, React ve diğer popüler çerçeveleri için bu uygulama geliştirme stili Evrensel bir platform sağlar. İlk Node.js aracılığıyla sunucuda framework bileşenleri oluşturma ve yürütme istemciye daha fazla temsilci olur.

ASP.NET Core [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından sağlanan SpaServices server JavaScript işlevleri çağrılarak sunucu tarafı prerendering uygulamasını basitleştirin.

### <a name="server-side-prerendering-prerequisites"></a>Sunucu tarafı prerendering önkoşulları

[ASPNET-prerendering](https://www.npmjs.com/package/aspnet-prerendering) NPM paketini yükler:

```console
npm i -S aspnet-prerendering
```

### <a name="server-side-prerendering-configuration"></a>Sunucu tarafı prerendering yapılandırması

Etiket Yardımcıları projesinin ad kayıt bulunabilir yapılan *_viewımports.cshtml* dosyası:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Bu etiket Yardımcıları hemen bir HTML benzeri sözdizimi Razor görünüm içinde yararlanarak alt düzey API'ler ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="asp-prerender-module-tag-helper"></a>ASP-PreRender-Module etiketi Yardımcısı

`asp-prerender-module` Etiketi Yardımcısı, önceki kod örneğinde kullanılan yürütür *ClientApp/dist/main-server.js* Node.js aracılığıyla sunucuda. Açıklık'ın çok için *ana server.js* dosyasıdır TypeScript JavaScript transpilation görevin bir yapıt [Web](https://webpack.github.io/) derleme işlemi. Web tanımlayan bir giriş noktası diğer `main-server`; ve bu diğer adı için bağımlılık grafiği geçişini başlar *ClientApp/önyükleme-server.ts* dosyası:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Aşağıdaki Angular örnekte *ClientApp/önyükleme-server.ts* dosya kullanır `createServerRenderer` işlevi ve `RenderResult` tür `aspnet-prerendering` npm paketi, sunucu işleme Node.js aracılığıyla yapılandırılır. Kesin türü belirtilmiş bir JavaScript içinde sarmalanmış bir Çözümle işlev çağrısı için sunucu tarafı işleme geçirilen hedefleyen bir HTML biçimlendirmesi `Promise` nesne. `Promise` Nesnenin önemi olan, zaman uyumsuz olarak DOM'ın yer tutucu öğesi yerleştirmeye sayfasına HTML biçimlendirme sağlar.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="asp-prerender-data-tag-helper"></a>ASP-PreRender-veri etiketi Yardımcısı

İle birlikte zaman `asp-prerender-module` etiketi Yardımcısı `asp-prerender-data` etiketi Yardımcısı, bağlamsal bilgiler için sunucu tarafı JavaScript Razor görünümden geçirmek için kullanılabilir. Örneğin, kullanıcı verilerini aşağıdaki biçimlendirme geçirir `main-server` Modülü:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Alınan `UserName` bağımsız değişkeni yerleşik JSON serileştirici kullanılarak serileştirilmiş ve depolanan `params.data` nesne. Angular aşağıdaki örnekte veri içinde kişiselleştirilmiş bir karşılama oluşturmak için kullanılan bir `h1` öğesi:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Etiket yardımcılarının geçirildiği Özellik adları **PascalCase** gösterimi ile temsil edilir. Burada aynı özellik adları ile gösterilir, JavaScript, Karşıtlık **camelCase**. Bu fark için varsayılan JSON seri hale getirme yapılandırması sorumludur.

Yukarıdaki kod örneğinde genişletmek için verileri sunucudan görünüme hydrating tarafından geçirilebilir `globals` özelliği için sağlanan `resolve` işlevi:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` İçinde tanımlanan bir dizi `globals` nesne bağlı tarayıcının genel `window` nesne. Bu değişken hoisting genel kapsam için aynı verileri bir kez sunucuda ve istemcinin yeniden yüklenmesi için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.

![Pencere nesnesi bağlı genel postList değişkeni](spa-services/_static/global_variable.png)

## <a name="webpack-dev-middleware"></a>Web geliştirme ara yazılımı

[Web geliştirme ara yazılım](https://webpack.js.org/guides/development/#using-webpack-dev-middleware) yapabildiği Web yapılar kaynakları isteğe bağlı olarak Basitleştirilmiş geliştirme iş akışı sunar. Ara yazılım, otomatik olarak derler ve bir sayfa tarayıcıya yeniden yüklendiğinde istemci-tarafı kaynaklar sunar. Başka bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Web projenin npm derleme betiği aracılığıyla el ile başlatmak için yaklaşımdır. Bir npm derleme betiği *package.json* dosya, aşağıdaki örnekte gösterilmiştir:

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="webpack-dev-middleware-prerequisites"></a>WebPack dev ara yazılım önkoşulları

[ASPNET-WebPack](https://www.npmjs.com/package/aspnet-webpack) NPM paketini yükler:

```console
npm i -D aspnet-webpack
```

### <a name="webpack-dev-middleware-configuration"></a>WebPack dev ara yazılım yapılandırması

HTTP istek işlem hattı aşağıdaki kod aracılığıyla içine kayıtlı Web geliştirme ara yazılım *Startup.cs* dosyanın `Configure` yöntemi:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_WebpackMiddlewareRegistration&highlight=4)]

`UseWebpackDevMiddleware` Genişletme yöntemi çağrıldığında, önce [statik dosya barındırma kaydetme](xref:fundamentals/static-files) aracılığıyla `UseStaticFiles` genişletme yöntemi. Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.

*Webpack.config.js* dosyanın `output.publicPath` özelliği söyler izlemek için bir ara yazılım `dist` klasördeki değişiklikleri:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

## <a name="hot-module-replacement"></a>Sık erişimli modülü değiştirme

Web'ın düşünebilirsiniz [sık erişimli modülü değiştirme](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) özelliği'nın Gelişmiş hali olarak [Web geliştirme ara yazılım](#webpack-dev-middleware). HMR aynı avantajları sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır. Bu, geçerli bellek içi durumu ve hata ayıklama oturumu SPA ile neden tarayıcı yenileme ile karıştırmayın. Web geliştirme ara yazılım hizmeti ve değişiklikleri tarayıcıya gönderilmesini anlamına gelir tarayıcı arasında canlı bir bağlantı yoktur.

### <a name="hot-module-replacement-prerequisites"></a>Sık kullanılan modül değiştirme önkoşulları

[WebPack-Hot-ara yazılım](https://www.npmjs.com/package/webpack-hot-middleware) NPM paketini yükler:

```console
npm i -D webpack-hot-middleware
```

### <a name="hot-module-replacement-configuration"></a>Sık kullanılan modül değiştirme yapılandırması

HMR bileşen MVC'nin HTTP istek işlem hattı, kaydedilmesi gerekir `Configure` yöntemi:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

İle doğru olarak [Web geliştirme ara yazılım](#webpack-dev-middleware), `UseWebpackDevMiddleware` genişletme yöntemi çağrıldığında, önce `UseStaticFiles` genişletme yöntemi. Uygulama geliştirme modunda çalıştırıldığında güvenlik nedenleriyle, ara yazılım kaydedin.

*Webpack.config.js* dosya tanımlamalıdır bir `plugins` dizi bile boş bırakılır:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Uygulamayı tarayıcıda yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:

![Sık erişimli modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

## <a name="routing-helpers"></a>Yönlendirme Yardımcıları

Çoğu ASP.NET Core tabanlı maça, istemci tarafı yönlendirme genellikle sunucu tarafı yönlendirmeye ek olarak istenir. SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir. Yoktur, ancak bir kenar büyük/küçük harf taşıyor sorunlarını: HTTP 404 yanıtları tanımlayan.

Senaryoyu göz önünde bulundurun uzantısız bir yolu `/some/page` kullanılır. İstek deseni-sunucu tarafı rota match değil, ancak desenine bir istemci-tarafı rota ile eşleşmekte varsayılır. Şimdi gelen bir istek için göz önünde bulundurun `/images/user-512.png`, hangi genellikle bekliyor sunucudaki bir görüntü dosyası bulunamıyor. İstenen kaynak yolu herhangi bir sunucu tarafı yolu veya statik dosya ile eşleşmiyorsa, istemci tarafı uygulamanın bu&mdash;işlemi genellikle bir 404 HTTP durum kodu döndürerek işlemesi çok düşüktür.

### <a name="routing-helpers-prerequisites"></a>Yönlendirme Yardımcıları önkoşulları

İstemci tarafı yönlendirme NPM paketini yükler. Angular örnek olarak kullanıp:

```console
npm i -S @angular/router
```

### <a name="routing-helpers-configuration"></a>Yönlendirme Yardımcıları yapılandırması

Adlı bir genişletme yöntemi `MapSpaFallbackRoute` kullanılır `Configure` yöntemi:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=snippet_MvcRoutingTable&highlight=7-9)]

Yollar yapılandırıldıkları sırayla değerlendirilir. Sonuç olarak, `default` rota önceki kod örneğinde kullanılan ilk desen eşleştirmesi için.

## <a name="create-a-new-project"></a>Yeni bir proje oluşturma

JavaScript Hizmetleri önceden yapılandırılmış uygulama şablonları sağlar. Istenmeyen hizmetler, bu şablonlarda, angular, tepki ve Redux gibi farklı çerçeveler ve kitaplıklarla birlikte kullanılır.

Bu şablonlar, aşağıdaki komutu çalıştırarak, .NET Core CLI yüklenebilir:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Kullanılabilir SPA şablonlar listesi görüntülenir:

| Şablonlar                                 | Kısa Ad | Dil | Etiketler        |
| ------------------------------------------| :--------: | :------: | :---------: |
| Angular ile ASP.NET Core MVC             | Angular    | [C#]     | MVC/Web/SPA |
| React.js ile ASP.NET Core MVC            | react      | [C#]     | MVC/Web/SPA |
| ASP.NET Core MVC React.js ve Redux  | açarken kilitlenmesi | [C#]     | MVC/Web/SPA |

SPA şablonlardan birini kullanarak yeni bir proje oluşturmak için dahil **kısa ad** şablonda, [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu. Aşağıdaki komut, sunucu tarafı için yapılandırılan ASP.NET Core MVC ile Angular bir uygulama oluşturur:

```console
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

ASP.NET Core kullanan adlı bir ortam değişkeni `ASPNETCORE_ENVIRONMENT` yapılandırma modunu depolamak için. Daha fazla bilgi için bkz. [ortamı ayarlama](xref:fundamentals/environments#set-the-environment).

### <a name="run-with-net-core-cli"></a>.NET Core CLI ile Çalıştır

Proje kök dizininde aşağıdaki komutu çalıştırarak gerekli NuGet ve npm paketlerini geri yükleyin:

```console
dotnet restore && npm i
```

Derleme ve uygulamayı çalıştırın:

```console
dotnet run
```

Şunlara göre localhost üzerinde uygulama başladığında [çalışma zamanı yapılandırma modunu](#set-the-runtime-configuration-mode). Gezinme `http://localhost:5000` tarayıcısında giriş sayfası görüntüler.

### <a name="run-with-visual-studio-2017"></a>Visual Studio 2017 ile çalıştırma

Açık *.csproj* tarafından oluşturulan dosya [yeni dotnet](/dotnet/core/tools/dotnet-new) komutu. Gerekli NuGet ve npm paketleri proje üzerinde otomatik olarak geri yüklenir. Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırmaya hazır bir uygulamadır. Yeşil çalıştırma düğmesine veya tuşuna tıklayın `Ctrl + F5`, ve tarayıcıda uygulama giriş sayfası açılır. Uygulama ayarına göre localhost üzerinde çalışır [çalışma zamanı yapılandırma modunu](#set-the-runtime-configuration-mode).

## <a name="test-the-app"></a>Uygulamayı test etme

SpaServices şablonları kullanarak istemci tarafı testleri çalıştırmak için önceden yapılandırılmış [Karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/). Test Çalıştırıcısı bu testler için karma bilgileriyse jasmine bir popüler birim testi çerçevesi için JavaScript, ' dir. Karma çalışmak üzere yapılandırılmış [Web geliştirme ara yazılım](#webpack-dev-middleware) sağlayacak şekilde Geliştirici durdurmak ve değişiklikler her zaman test çalıştırması için gerekli değildir. Test çalışması veya test çalışması karşı çalışan kod olup olmadığını test otomatik olarak çalıştırılır.

Angular uygulamasını örnek olarak kullanıp, Jasmine iki test çalışmaları zaten için sağlanan `CounterComponent` içinde *counter.component.spec.ts* dosyası:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Komut istemi açın *ClientApp* dizin. Şu komutu çalıştırın:

```console
npm test
```

Komut dosyası içinde tanımlanan ayarlarını okur Karma test Çalıştırıcısı başlatır *karma.conf.js* dosya. Diğer ayarlar arasında *karma.conf.js* aracılığıyla yürütülecek test dosyalarını tanımlar, `files` dizi:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

## <a name="publish-the-app"></a>Uygulamayı yayımlama

Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştiren çok uğraşmayı gerektirebilir. Ne SpaServices adlı özel bir MSBuild hedefi ile tüm yayın işlem düzenler `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild hedefi aşağıdaki sorumluluklara sahiptir:

1. NPM paketlerini geri yükleyin.
1. Üçüncü taraf, istemci tarafı varlıkların üretim sınıfı derlemesini oluşturun.
1. Özel istemci tarafı varlıkların üretim sınıfı derlemesini oluşturma.
1. Web paketi tarafından oluşturulan varlıkları Yayımla klasörüne kopyalayın.

MSBuild hedefi çalıştırılırken çağrılır:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Angular belgeleri](https://angular.io/docs)
