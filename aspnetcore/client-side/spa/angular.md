---
title: ASP.NET Core ile Angular proje şablonu kullanın
author: SteveSandersonMS
description: ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile Angular ve Angular CLI'yi kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 8283fe9e96acb57942040dd4c90fabd204a19663
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326049"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>ASP.NET Core ile Angular proje şablonu kullanın

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> Bu belge hakkında Angular proje şablonu, ASP.NET Core 2.0 sürümünde yer almıyor. Bu, el ile güncelleştirebilirsiniz yeni Angular hakkında şablonudur. Şablon içinde ASP.NET Core 2.1 varsayılan olarak dahil edilir.

::: moniker-end

Güncelleştirilmiş Angular proje şablonu, Angular ve Angular CLI'yi zengin, istemci tarafı kullanıcı arabirimini (UI) kullanarak uygulamaları ASP.NET Core için uygun bir başlama noktası sağlar.

Şablon, bir API arka ucu görev yapacak bir ASP.NET Core projesi ve bir kullanıcı Arabirimi görev yapacak bir Angular CLI'yi projesi oluşturmaya eşdeğerdir. Şablon iki proje türü tek bir uygulama projesinde barındırma kolaylık sunar. Sonuç olarak, uygulama projesi oluşturulabilen ve tek bir birim olarak yayımlandı.

## <a name="create-a-new-app"></a>Yeni uygulama oluştur

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 kullanıyorsanız, seçtiğiniz olun [güncelleştirilmiş Angular proje şablonu yüklü](xref:spa/index#installation).

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 yüklü varsa, Angular proje şablonu yüklemek için gerek yoktur.

::: moniker-end

Yeni Proje Oluştur komutunu kullanarak bir komut isteminden `dotnet new angular` boş bir dizin içinde. Örneğin, aşağıdaki komutları uygulamada oluşturma bir *-yeni-Uygulamam* dizini ve bu dizine geçin:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Visual Studio veya .NET Core CLI uygulamayı çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Oluşturulan açın *.csproj* dosya ve uygulama normal olarak oradan çalıştırın.

Derleme işlemi birkaç dakika sürebilir ilk çalıştırma npm bağımlılıkları yükler. Ardışık derlemeler çok daha hızlıdır.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Adlı bir ortam değişkeni sahip olduğunuzdan emin olun `ASPNETCORE_Environment` değeriyle `Development`. (PowerShell olmayan istemleri içinde) Windows üzerinde çalıştırma `SET ASPNETCORE_Environment=Development`. Linux veya macOS üzerinde çalıştıran `export ASPNETCORE_Environment=Development`.

Çalıştırma [dotnet derleme](/dotnet/core/tools/dotnet-build) uygulamayı doğrulamak için yapılar doğru. İlk çalıştırılmasında oluşturma işlemi birkaç dakika sürebilir npm bağımlılıkları, geri yükler. Ardışık derlemeler çok daha hızlıdır.

Çalıştırma [çalıştırma dotnet](/dotnet/core/tools/dotnet-run) uygulamasını başlatmak için. Aşağıdakine benzer bir ileti günlüğe kaydedilir:

```console
Now listening on: http://localhost:<port>
```

Bir tarayıcıda bu URL'ye gidin.

Uygulama arka planda Angular CLI'yi sunucusu olan bir örneğini başlatır. Aşağıdakine benzer bir ileti günlüğe kaydedilir: *Live geliştirme sunucu komut localhost üzerinde dinleme:&lt;otherport&gt;, tarayıcınızı açmak http://localhost:&lt; otherport&gt; /*  . Bu iletiyi yoksayın&mdash;sahip **değil** birleşik ASP.NET Core ve Angular CLI'yi app URL'si.

---

Proje şablonu, ASP.NET Core uygulaması ve Angular uygulaması oluşturur. ASP.NET Core uygulaması, veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır. Bulunan uygulamayı Angular *ClientApp* alt dizini kullanır, tüm kullanıcı Arabirimi sorunları için kullanılmak üzere tasarlanmıştır.

## <a name="add-pages-images-styles-modules-etc"></a>Sayfaları, görüntüler, stil, modüller, vb. ekleyin.

*ClientApp* dizini bir standart Angular CLI'yi uygulaması içerir. Resmi görmek [Angular belgeleri](https://github.com/angular/angular-cli/wiki) daha fazla bilgi için.

Bu şablonla oluşturulan Angular uygulama ve Angular CLI tarafından kendi oluşturduğunuz bir arasında küçük farklar vardır (aracılığıyla `ng new`); ancak, uygulama özelliklerini değiştirilmez. Şablon tarafından oluşturulan uygulama içeren bir [önyükleme](https://getbootstrap.com/)-temel düzen ve basit bir yönlendirme örneği.

## <a name="run-ng-commands"></a>NG komutları çalıştırın

Komut isteminde, geçiş *ClientApp* alt dizini:

```console
cd ClientApp
```

Varsa `ng` genel yüklü aracı çalıştırabilirsiniz, komutlardan herhangi birine. Örneğin, çalıştırabileceğiniz `ng lint`, `ng test`, veya herhangi başka bir [Angular CLI komutları](https://github.com/angular/angular-cli/wiki#additional-commands). Çalıştırılacak gerek yoktur `ng serve` , uygulamanızın sunucu tarafı hem istemci tarafı bölümlerini hizmet ile ASP.NET Core uygulamanızı oluşturulmasıyla ilgili olduğu için. Dahili olarak kullandığı `ng serve` geliştirme.

Öğeniz yoksa `ng` yüklü aracı çalıştırma `npm run ng` bunun yerine. Örneğin, çalıştırabileceğiniz `npm run ng lint` veya `npm run ng test`.

## <a name="install-npm-packages"></a>Npm paketlerini yükle

Üçüncü taraf npm paketlerini yüklemek için bir komut isteminde kullanmak *ClientApp* alt. Örneğin:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Yayımlama ve dağıtma

Geliştirmede uygulama geliştiriciye kolaylık sağlamak için en iyi duruma getirilmiş bir modda çalışır. Örneğin, (hata ayıklama sırasında özgün TypeScript kodunuzu görebilmeniz için) kaynak eşlemeleri JavaScript paketleri içerir. Uygulama diskteki TypeScript, HTML ve CSS dosyası değişiklikler için izleyen ve otomatik olarak yeniden derler ve bu dosyaları değiştirmek gördüğünde yeniden yükler.

Üretim ortamında, uygulamanızın performansını en iyi duruma getirilmiş bir sürümünü işlevi görür. Bu, otomatik olarak gerçekleştirilmesi için yapılandırılır. Yayımladığınızda, derleme yapılandırmasını bir küçültülmüş yayar, istemci tarafı kodunuzun derleme tamamlanan-ın-time (AoT) derlenmiş. Geliştirme derleme, üretim yapı sunucusunda yüklü olması Node.js gerektirmeyen (etkinleştirilmiş olduğu sürece [sunucu tarafı prerendering](#server-side-rendering)).

Standart kullanabileceğiniz [ASP.NET Core barındırma ve dağıtma yöntemleri](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>"Ng hizmet vermemesini" bağımsız olarak çalıştırma

Proje, ASP.NET Core uygulaması geliştirme modunda başlatıldığında, arka planda Angular CLI'yi sunucunun kendi örneğini başlatmak için yapılandırılır. Bu, ayrı bir sunucuya el ile çalıştırmak zorunda olmadığınız için uygundur.

Bu varsayılan ayarı bir dezavantajı vardır. C# kodunuzu ve ASP.NET Core uygulamasını yeniden başlatmak için gereken her değiştirdiğinizde, Angular CLI'yi sunucuyu yeniden başlatır. Yaklaşık 10 saniye yedekleme başlatmak için gereklidir. Sık kullanılan C# kod düzenleme yapıyorsanız ve yeniden başlatmak Angular CLI için beklemek istemiyorsanız, Angular CLI'yi sunucu dışarıdan, ASP.NET Core işlemden bağımsız olarak çalıştırın. Bunu yapmak için:

1. Komut isteminde, geçiş *ClientApp* alt ve Angular CLI'yi geliştirme sunucusu başlatma:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Kullanım `npm start` Angular CLI'yi geliştirme sunucusu başlatılamıyor değil `ng serve`, böylece yapılandırmada *package.json* uyulduğundan. Ek parametreleri Angular CLI'yi sunucuya geçirmek için bunları ilgili ekleme `scripts` satırına, *package.json* dosya.

2. ASP.NET Core uygulamanızı biri kendi başlatma yerine dış Angular CLI örneği kullanacak şekilde değiştirin. İçinde *başlangıç* sınıfı, yerine `spa.UseAngularCliServer` aşağıdaki çağrı:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

ASP.NET Core uygulamanızı başlattığınızda, Angular CLI'yi sunucusu başlatma olmaz. El ile başlatılan örneği yerine kullanılır. Bu başlatma ve hızlı yeniden başlatma için sağlar. İstemci uygulamanızı her seferinde yeniden oluşturmasıyla Angular CLI için artık bekliyor.

## <a name="server-side-rendering"></a>Sunucu tarafı işleme

Bir performans özelliği, sunucu hem de istemcide çalışan Angular uygulamanızı önceden işlenecek seçim yapabilirsiniz. Bu tarayıcılar, hatta indiriliyor ve, JavaScript paketleri yürütmeden önce görüntülemek için uygulamanızın ilk kullanıcı Arabirimi temsil eden HTML biçimlendirmesi aldığınız anlamına gelir. Çoğu uygulama bu adlı bir Angular özellikten gelen [Angular Evrensel](https://universal.angular.io/).

> [!TIP]
> Sunucu tarafı işleme etkinleştirme (SSR) birkaç fazladan zorluk hem geliştirme ve dağıtım sırasında ortaya çıkarır. Okuma [SSR dezavantajları](#drawbacks-of-ssr) SSR gereksinimleriniz için uygun olup olmadığını belirlemek için.

SSR etkinleştirmek için projenizi eklemeleri bir dizi yapmanız gerekir.

İçinde *başlangıç* sınıfı *sonra* yapılandırır satırı `spa.Options.SourcePath`, ve *önce* çağrısı `UseAngularCliServer` veya `UseProxyToSpaDevelopmentServer`, aşağıdakileri ekleyin:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

Geliştirme modunda, betik çalıştırarak SSR paket oluşturmak Bu kod çalışır `build:ssr`, tanımlanan *ClientApp\package.json*. Bu adlı bir Angular uygulama derlemeleri `ssr`, henüz tanımlı değil.

Sonunda `apps` içindeki dizi *ClientApp/.angular-cli.json*, ek bir uygulama adıyla tanımlama `ssr`. Aşağıdaki seçenekleri kullanın:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Bu yeni SSR özellikli uygulama yapılandırmasını iki ek dosyalar gerektirir: *tsconfig.server.json* ve *main.server.ts*. *Tsconfig.server.json* dosyasını TypeScript derleme seçenekleri belirtir. *Main.server.ts* dosya SSR sırasında kod giriş noktası olarak görev yapar.

Adlı yeni bir dosya ekleme *tsconfig.server.json* içinde *ClientApp/src* (varolan yanı sıra *tsconfig.app.json*), aşağıdakileri içeren:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Bu dosya adlı bir modül için aramak için Angular AoT derleyici yapılandırır `app.server.module`. Bu, yeni bir dosya oluşturarak ekleyin *ClientApp/src/app/app.server.module.ts* (varolan yanı sıra *app.module.ts*) aşağıdakileri içeren:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Bu modül istemci-tarafı devralan `app.module` ve fazladan Angular modüllerine SSR sırasında kullanılabilir tanımlar.

Bu geri çağırma yeni `ssr` girişi *.angular cli.json* adlı bir giriş noktası dosyası başvurulan *main.server.ts*. Bu dosya henüz eklemediniz ve artık bunu yapmak için zamanı geldi. En yeni bir dosya oluşturun *ClientApp/src/main.server.ts* (varolan yanı sıra *main.ts*), aşağıdakileri içeren:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Bu dosyanın kodu çalıştığında ne ASP.NET Core için her bir isteği yürütür `UseSpaPrerendering` eklediğiniz bir ara yazılım *başlangıç* sınıfı. Alma ile ilgilenen `params` (örneğin, istenen URL) .NET kodu ve sonuçta elde edilen HTML almak için Angular SSR API çağrıları yapma.

Kesinlikle konuşulur, bu SSR geliştirme modunda etkinleştirmek yeterli olur. Böylece uygulamanızın düzgün çalıştığını yayımlandığında bir son değişiklik yapmak için gereklidir. Uygulamanızın ana içinde *.csproj* dosya, ayarlama `BuildServerSideRenderer` özellik değerini `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Bu yapı işlemini çalıştırın yapılandırır `build:ssr` yayımlama sırasında ve SSR dosya sunucusuna dağıtın. Bu etkinleştirme SSR üretimde başarısız olur.

Angular kod HTML olarak sunucuda uygulamanızı geliştirme veya üretim modunda çalışırken, önceden işler. İstemci tarafı kod, normal olarak yürütür.

### <a name="pass-data-from-net-code-into-typescript-code"></a>TypeScript koduna .NET kodundan verisini geçirin

SSR sırasında istek başına veri Angular uygulamanıza ASP.NET Core uygulamanızı geçirmek isteyebilirsiniz. Örneğin, tanımlama bilgilerini geçirebiliriz veya bir şey bir veritabanından okuyamadı. Bunu yapmak için Düzenle, *başlangıç* sınıfı. İçin geri çağırma içinde `UseSpaPrerendering`, ayarlamak için bir değer `options.SupplyData` aşağıdaki gibi:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` Rastgele geçirdiğiniz geri çağırma sağlar, istek başına, JSON seri hale getirilebilir veri (için örnek, dizeler, Boole değerlerini veya sayı). *Main.server.ts* kod olarak aldığı `params.data`. Örneğin, yukarıdaki kod örneğinde bir Boole değeri olarak geçirir `params.data.isHttpsRequest` içine `createServerRenderer` geri çağırma. Angular tarafından desteklenen herhangi bir şekilde uygulamanızın diğer kısımlarına geçirebilirsiniz. Örneğin, nasıl *main.server.ts* geçirir `BASE_URL` alması, Oluşturucusu bildirilen herhangi bir bileşen değeri.

### <a name="drawbacks-of-ssr"></a>SSR dezavantajları

Tüm uygulamalar, SSR ' yararlanabilir. Birincil avantajı algılanan performansı. JavaScript paketleri ayrıştıramadık ya da fetch biraz sürdüğünü olsa bile uygulamanız yavaş ağ bağlantısı üzerinden veya yavaş mobil cihazlarına ulaşma ziyaretçiler ilk kullanıcı Arabirimi hızlı bir şekilde bakın. Ancak, birçok Spa'lar çoğunlukla hızlı bilgisayarlarda hızlı, iç şirket ağları üzerinden uygulama neredeyse anında göründüğü kullanılır.

Aynı zamanda, SSR etkinleştirmek için önemli engelleri vardır. Geliştirme sürecinizin karmaşıklık ekler. İki farklı ortamlarda kodunuzun çalıştırmanız gerekir: istemci tarafı ve sunucu tarafı (ortamında ASP.NET çekirdek çağrılan bir Node.js). Aklınızda bulundurun gereken bazı noktalar şunlardır:

* SSR, üretim sunucularında bir Node.js yükleme gerektirir. Otomatik olarak çalışması için Azure Service Fabric gibi diğer bazı dağıtım senaryolarında, Azure App Services gibi ancak budur.
* Etkinleştirme `BuildServerSideRenderer` bayrağı nedenleri derleme, *node_modules* yayımlamak için dizin. Bu klasör, dağıtım süresini artırır 20.000 + dosyaları içerir.
* Bir Node.js ortamda kodunuzu çalıştırmak için tarayıcı özel JavaScript API varlığı üzerinde gibi güvenemezsiniz `window` veya `localStorage`. Bu API'leri kullanmak kodunuzu (veya başvuru bazı üçüncü taraf kitaplık) çalışırsa, SSR sırasında bir hata alırsınız. Örneğin, tarayıcı özel API'leri çeşitli yerlerde başvurduğu için jQuery kullanmayın. Hataları önlemek için SSR önlemek veya tarayıcı özel API'leri veya kitaplıklarını kaçınmak gerekir. Bu API çağrıları SSR sırasında çağrılan olmayan çoğaltılmadığını denetler kaydırın. Örneğin, aşağıdaki gibi bir denetimi, JavaScript veya TypeScript kodu kullanın:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
