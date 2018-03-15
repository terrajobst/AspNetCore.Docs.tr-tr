---
title: "ASP.NET çekirdek tek sayfa uygulamaları oluşturmak için JavaScriptServices kullanın"
author: scottaddie
description: "Bir tek sayfa uygulaması (ASP.NET Core tarafından yedeklenen SPA) oluşturmak için JavaScriptServices kullanmanın avantajları hakkında bilgi edinin."
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c962fc160cf39ad1c69f4269616c993fde420035
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>ASP.NET çekirdek tek sayfa uygulamaları oluşturmak için JavaScriptServices kullanın

Tarafından [Scott Addie](https://github.com/scottaddie) ve [Fiyaz Hasan](http://fiyazhasan.me/)

Tek sayfa uygulama (SPA) web uygulaması, devralınan zengin kullanıcı deneyimi nedeniyle popüler bir türde değil. İstemci tarafı SPA çerçeveleri veya kitaplıklar gibi tümleştirme [Angular](https://angular.io/) veya [tepki](https://facebook.github.io/react/), ASP.NET Core zor olabilir gibi sunucu tarafı çerçeveleri ile. [JavaScriptServices](https://github.com/aspnet/JavaScriptServices) tümleştirme işleminde uyuşmazlık azaltmak için geliştirilmiştir. Farklı istemci ve sunucu teknolojisi yığınları arasında sorunsuz işlemi sağlar.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>JavaScriptServices nedir?

JavaScriptServices ASP.NET Core için istemci tarafı teknolojileri koleksiyonudur. ASP.NET Core geliştiricilerinin SPAs oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak konumlandırmak için kendi hedeftir.

JavaScriptServices üç ayrı NuGet paketlerini oluşur:
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

Bu paketleri yararlı varsa:
* JavaScript sunucusunda çalıştırın
* SPA framework veya kitaplık kullanın
* İstemci-tarafı varlıklar Webpack ile derleme

Bu makalede odak çoğunu SpaServices paketini kullanarak yerleştirilir.

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>SpaServices nedir?

SpaServices geliştiricilerinin SPAs oluşturmaya yönelik olarak tercih edilen sunucu tarafı platformu olarak ASP.NET Core konumlandırmak için oluşturuldu. SpaServices SPAs ASP.NET Core ile geliştirmek için gerekli değildir ve belirli bir istemci çerçeveye kilitleyin değil.

SpaServices gibi yararlı altyapısı sağlar:
* [Sunucu tarafı prerendering](#server-prerendering)
* [Webpack geliştirme Ara](#webpack-dev-middleware)
* [Sık kullanılan modülü değiştirme](#hot-module-replacement)
* [Yönlendirme Yardımcıları](#routing-helpers)

Toplu olarak, bu altyapı bileşenlerini geliştirme iş akışı ve çalışma zamanı deneyimini geliştirin. Bileşenleri tek tek benimsenen.

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>SpaServices kullanma önkoşulları

SpaServices ile çalışmak için aşağıdaki yükleyin:
* [Node.js](https://nodejs.org/) (sürüm 6 veya sonrası) npm ile
    * Bu bileşenlerin yüklü olduğundan ve bulunabilir doğrulamak için aşağıdaki komut satırından çalıştırın:

    ```console
    node -v && npm -v
    ```

Not: bir Azure web sitesine dağıtıyorsanız, burada bir şey yapmanız gerekmez &mdash; Node.js yüklü olduğundan ve sunucu ortamlarında kullanılabilir.

* [.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (veya üstü)
    * Windows üzerinde değilseniz, bu Visual Studio 2017'ın seçerek yüklenebilir **.NET Core platformlar arası geliştirme** iş yükü.

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet paketi

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>Sunucu tarafı prerendering

Sunucu ve istemci üzerinde çalışan hem özellikli bir JavaScript uygulaması, bir evrensel (isomorphic olarak da bilinir) uygulamasıdır. Açısal tepki vermek ve diğer popüler uygulamayı Evrensel bir platform için bu uygulama geliştirme stil sağlayın. İlk Node.js aracılığıyla sunucuda framework bileşenlerini oluşturmak ve daha fazla istemciye yürütme temsilci olur.

ASP.NET Core [etiket Yardımcıları](xref:mvc/views/tag-helpers/intro) tarafından sağlanan SpaServices sunucudaki JavaScript işlevlerini çağırarak sunucu tarafı prerendering uyarlamasını basitleştirin.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yükleyin:
* [ASPNET prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm paket:

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>Yapılandırma

Etiket Yardımcıları projesinin üzerinden ad kayıt bulunabilirlik yapılan *_viewımports.cshtml* dosyası:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

Bu etiket Yardımcıları hemen Razor görünüm içinde HTML benzeri bir sözdizimi yararlanarak alt düzey API'leri ile doğrudan iletişim ayrıntılı olarak incelenmektedir soyut:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module` Etiket Yardımcısı

`asp-prerender-module` Etiket Yardımcısı, önceki kod örneğinde kullanılan yürütür *ClientApp/dist/main-server.js* Node.js aracılığıyla sunucuda. Netlik'ın artırmak amacıyla için *ana server.js* dosyasıdır bir yapı içinde TypeScript JavaScript transpilation görevinin [Webpack](http://webpack.github.io/) derleme işlemi. Webpack tanımlayan bir giriş noktası diğer ad `main-server`; ve bu diğer adı için bağımlılık grafiğinin geçişi başlar *önyükleme/ClientApp-server.ts* dosyası:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

Aşağıdaki örnekte Açısal, *önyükleme/ClientApp-server.ts* dosya yararlanan `createServerRenderer` işlevi ve `RenderResult` türü `aspnet-prerendering` npm paket Node.js aracılığıyla sunucu işleme yapılandırmak için. Sunucu tarafı işleme kesin türü belirtilmiş bir JavaScript'te kaydırılan bir çözümleme işlev çağrısı için geçirilen hedefleyen HTML biçimlendirmesi `Promise` nesnesi. `Promise` Nesnenin anlamlı olduğundan, zaman uyumsuz olarak DOM'ın bir yer tutucu öğeyi yerleştirmeye sayfasına HTML biçimlendirmesi sağlar.

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data` Etiket Yardımcısı

İle birlikte zaman `asp-prerender-module` etiket Yardımcısı `asp-prerender-data` etiket Yardımcısı, sunucu tarafı JavaScript Razor görünümünden bağlamsal bilgi geçirmek için kullanılabilir. Örneğin, kullanıcı verilerini aşağıdaki biçimlendirme geçirir `main-server` Modülü:

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

Alınan `UserName` bağımsız değişkeni yerleşik JSON seri hale getirici kullanılarak serileştirilmiş ve depolanan `params.data` nesnesi. Aşağıdaki Açısal örnekte, veri içinde kişiselleştirilmiş tebrik oluşturmak için kullanılan bir `h1` öğe:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

Not: Etiket Yardımcıları geçirilen özellik adları ile temsil edilen **PascalCase** gösterimi. JavaScript, burada aynı özellik adları temsil edilen ile karşılaştırın **camelCase**. Varsayılan JSON seri hale getirme yapılandırması için bu fark sorumludur.

Önceki kod örneğinde genişletmek için verileri sunucudan görünümüne hydrating tarafından geçirilebilir `globals` özelliği için sağlanan `resolve` işlevi:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList` İçinde tanımlanan dizi `globals` nesnesi bağlı tarayıcının genel `window` nesnesi. Bu değişken hoisting genel kapsam için aynı veri kez sunucuda ve istemcide yeniden yükleme için özellikle ilgili olarak çaba, çoğaltma ortadan kaldırır.

![Pencere nesnesi bağlı postList genel değişkeni](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack geliştirme Ara

[Webpack geliştirme Ara](https://webpack.github.io/docs/webpack-dev-middleware.html) yapabildiği Webpack derlemeler kaynakları isteğe bağlı bir kolaylaştırılmış geliştirme iş akışı sunar. Ara yazılım otomatik olarak derler ve sayfa tarayıcıda yeniden yüklendiğinde istemci-tarafı kaynaklar sunar. Alternatif bir üçüncü taraf bağımlılığı veya özel kod değiştiğinde Webpack projenin npm derleme betiğindeki el ile çağırma yaklaşımdır. Bir npm yapı komut dosyası içinde *package.json* dosya, aşağıdaki örnekte gösterilir:

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yükleyin:
* [ASPNET webpack](https://www.npmjs.com/package/aspnet-webpack) npm paket:

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>Yapılandırma

Aşağıdaki kodda aracılığıyla HTTP istek ardışık düzenini içine Webpack Dev Ara kayıtlı *haline* dosyanın `Configure` yöntemi:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware` Genişletme yöntemi çağrılır, önce [statik dosya barındırma kaydetme](xref:fundamentals/static-files) aracılığıyla `UseStaticFiles` genişletme yöntemi. Yalnızca uygulama geliştirme modunda çalıştığında güvenlik nedenleriyle, ara yazılımın kaydedin.

*Webpack.config.js* dosyanın `output.publicPath` özelliği bildirir izlemek için ara yazılım `dist` değişiklikleri için klasör:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>Sık kullanılan modülü değiştirme

Webpack'ın düşünün [etkin modülü değiştirme](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html) (HMR) özelliği bir evrimi olarak [Webpack geliştirme Ara](#webpack-dev-middleware). HMR hepsi aynı avantajlar sunar, ancak otomatik olarak değişiklikleri derledikten sonra sayfa içeriği güncelleştirerek daha fazla geliştirme iş akışı kolaylaştırır. Bu geçerli bellek içi durumu ve hata ayıklama oturumu SPA engel tarayıcının bir yenileme ile karıştırmayın. Webpack geliştirme Ara hizmeti ve değişiklikleri tarayıcıya gönderilen anlamına gelir tarayıcı arasındaki bir canlı bağlantı yoktur.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yükleyin:
* [webpack hot Ara](https://www.npmjs.com/package/webpack-hot-middleware) npm paket:

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>Yapılandırma

MVC'ın HTTP istek ardışık düzeninde içine HMR bileşen kaydedilmelidir `Configure` yöntemi:

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

İle true olarak [Webpack geliştirme Ara](#webpack-dev-middleware), `UseWebpackDevMiddleware` genişletme yöntemi çağrılır, önce `UseStaticFiles` genişletme yöntemi. Yalnızca uygulama geliştirme modunda çalıştığında güvenlik nedenleriyle, ara yazılımın kaydedin.

*Webpack.config.js* dosya tanımlamalıdır bir `plugins` boş bırakılır olsa bile, dizi:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

Tarayıcıda uygulama yüklendikten sonra geliştirici araçları konsol sekmesi HMR Etkinleştirme Onayı sağlar:

![Sık kullanılan modülü değiştirme bağlı ileti](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>Yönlendirme Yardımcıları

Çoğu ASP.NET Core tabanlı SPAs içinde sunucu tarafı ek olarak istemci tarafı yönlendirmesi isteyeceksiniz. SPA ve MVC yönlendirme sistemleri girişim bağımsız olarak çalışabilir. Yoktur, ancak bir kenar servis talebi taşıyor sınar: 404 HTTP yanıtlarını tanımlama.

Senaryoyu göz önünde bulundurun uzantısız bir yolu `/some/page` kullanılır. İstek düzeni-bir sunucu tarafı yol eşleşme değil, ancak bir istemci-tarafı yol kendi desenle eşleşen varsayalım. Şimdi bir gelen istek için göz önünde bulundurun `/images/user-512.png`, hangi genellikle bekliyor sunucudaki bir görüntü dosyasını bulmak. İstenen kaynak yol herhangi bir sunucu tarafı yol veya statik dosya eşleşmiyorsa, istemci-tarafı uygulamanın bunu işlemesi olası değil — genellikle bir 404 HTTP durum kodu döndürülecek istersiniz.

### <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yükleyin:
* İstemci-tarafı yönlendirme npm paket. Angular örnek olarak kullanarak:

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>Yapılandırma

Adlı bir genişletme yöntemi `MapSpaFallbackRoute` kullanılan `Configure` yöntemi:

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

İpucu: Yollar yapılandırılmış sırayla değerlendirilir. Sonuç olarak, `default` rota önceki kod örneğinde kullanılan ilk desen eşleştirme için.

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>Yeni proje oluşturma

JavaScriptServices önceden yapılandırılmış uygulama şablonları sağlar. SpaServices bu şablonları farklı çerçeveler ve kitaplıklar Angular, Aurelia, çakıştırma, tepki ve Vue gibi ile birlikte kullanılır.

Bu şablonlar, aşağıdaki komutu çalıştırarak .NET Core CLI yüklenebilir:

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

Kullanılabilir SPA şablonların listesi görüntülenir:

| Şablonlar                                 | Kısa Ad | Dil | Etiketler        |
|:------------------------------------------|:-----------|:---------|:------------|
| MVC ASP.NET Core Açısal ile             | Açısal    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core Aurelia ile             | aurelia    | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core Knockout.js ile         | Boşaltılan   | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core React.js ile            | tepki      | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core React.js ve Redux  | reactredux | [C#]     | Web/MVC/SPA |
| MVC ASP.NET Core Vue.js ile              | VUE        | [C#]     | Web/MVC/SPA | 

SPA şablonlarından birini kullanarak yeni bir proje oluşturmak için içermesi **kısa ad** şablonunun [dotnet yeni](/dotnet/core/tools/dotnet-new) komutu. Aşağıdaki komutu Açısal uygulama ASP.NET Core için sunucu tarafı yapılandırılmış MVC ile oluşturur:

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>Çalışma zamanı yapılandırma modunu ayarlama

İki birincil çalışma zamanı yapılandırma modlarından mevcuttur:
* **Geliştirme**:
    * Hata ayıklama kolaylaştırmak için kaynak eşlemeleri içerir.
    * İstemci tarafı kodlar performansı en iyi duruma değil.
* **Üretim**:
    * Kaynak eşlemeleri dışlar.
    * İstemci tarafı kodu paketleme ve küçültme aracılığıyla en iyi duruma getirir.

ASP.NET Core adlı bir ortam değişkeni kullanır `ASPNETCORE_ENVIRONMENT` yapılandırma modu depolamak için. Bkz:  **[ortamını](xref:fundamentals/environments#setting-the-environment)**  daha fazla bilgi için.

### <a name="running-with-net-core-cli"></a>.NET Core CLI ile çalıştırma

Npm paketleri ve gerekli NuGet proje kök dizininde aşağıdaki komutu çalıştırarak geri yükleyin:

```console
dotnet restore && npm i
```

Derleme ve uygulamayı çalıştırın:

```console
dotnet run
```

Localhost göre uygulama başlangıç [çalışma zamanı yapılandırma modu](#runtime-config-mode). Gezinme `http://localhost:5000` giriş sayfasını tarayıcıda görüntüler.

### <a name="running-with-visual-studio-2017"></a>Visual Studio 2017 çalışır

Açık *.csproj* tarafından oluşturulan dosya [dotnet yeni](/dotnet/core/tools/dotnet-new) komutu. Gerekli NuGet ve npm'yi paketler proje üzerinde otomatik olarak geri yüklenir. Bu geri yükleme işlemi birkaç dakika sürebilir ve tamamlandığında çalıştırılmaya hazır bir uygulamadır. Yeşil düğmesine veya tuşuna tıklayın `Ctrl + F5`, ve tarayıcıda uygulama giriş sayfası açılır. Uygulama göre localhost üzerinde çalışan [çalışma zamanı yapılandırma modu](#runtime-config-mode). 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>Uygulamayı test etme

SpaServices şablonları kullanarak istemci tarafı testleri çalıştırmak için önceden yapılandırılmış [Karma](https://karma-runner.github.io/1.0/index.html) ve [Jasmine](https://jasmine.github.io/). Karma bu testler için test Çalıştırıcısı iken jasmine framework JavaScript için test popüler bir birimi değil. Karma çalışmak üzere yapılandırılmış [Webpack geliştirme Ara](#webpack-dev-middleware) sağlayacak şekilde Geliştirici durdurmak ve değişiklikler yapılmadan her zaman testi çalıştırmak için gerekli değildir. Test çalışması ya da test çalışması karşı çalışan kod olup olmadığını belirtir, test otomatik olarak çalıştırılır.

Açısal uygulama örnek olarak kullanarak, iki Jasmine test çalışmaları zaten için sağlanan `CounterComponent` içinde *counter.component.spec.ts* dosyası:

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

Proje kökündeki komut istemi açın ve aşağıdaki komutu çalıştırın:

```console
npm test
```

Tanımlanan ayarları okur Karma test Çalıştırıcısı komut dosyasını başlatır *karma.conf.js* dosya. Diğer ayarları arasında *karma.conf.js* aracılığıyla yürütülecek test dosyaları tanımlar, `files` dizisi:

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>Uygulama yayımlama

Oluşturulan istemci-tarafı varlıkları ve yayımlanan ASP.NET Core yapıları dağıtmak için hazır bir pakete birleştirme sıkıcı olabilir. Thankfully, tüm yayın işlem SpaServices adlı özel bir MSBuild hedef ile düzenler `RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild hedef aşağıdaki sorumlulukları vardır:
1. Npm paket geri yükleme
1. Bir üretim düzeyde yapı ve üçüncü taraf, istemci-tarafı varlıkları oluşturma
1. Bir üretim düzeyde yapı ve özel istemci-tarafı varlıkları oluşturma
1. Webpack oluşturulan varlıkları yayımla klasörüne kopyalayın

MSBuild hedef çalıştırırken çağrılır:

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>Ek kaynaklar

* [Açısal belgeleri](https://angular.io/docs)
