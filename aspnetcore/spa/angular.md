---
title: ASP.NET Core ile Açısal proje şablonu kullanın
author: SteveSandersonMS
description: Angular ve Açısal CLI için ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile başlayacağınızı öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: b4e48f40c3d4e3167e7fdb3534d2c33b3544592c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>ASP.NET Core ile Açısal proje şablonu kullanın

> [!NOTE]
> Bu belge hakkında Açısal proje şablonu ASP.NET Core 2. 0 ' dahil değildir. Bunu el ile güncelleştirme yapabilmeniz için daha yeni Açısal hakkında şablonudur. Şablon ASP.NET Core 2.1 içinde varsayılan olarak dahil edilir.

Güncelleştirilmiş Açısal proje şablonu, zengin, istemci tarafı kullanıcı arabirimini (UI) uygulamak için Angular ve Açısal CLI kullanarak uygulamaları ASP.NET Core için uygun bir başlama noktası sağlar.

Bir API arka ucu olarak görev yapması için bir ASP.NET Core proje ve bir kullanıcı Arabirimi hareket edecek bir Açısal CLI projesi oluşturmak için şablon eşdeğerdir. Şablon tek uygulama projesinde her iki proje türleri barındırma rahatlık sunar. Sonuç olarak, uygulama projesi oluşturulur ve tek bir birim olarak yayımlandı.

## <a name="create-a-new-app"></a>Yeni uygulama oluştur

ASP.NET Core 2.0 kullanıyorsanız, seçtiğiniz olun [güncelleştirilmiş Açısal proje şablonu yüklü](xref:spa/index#installation). ASP.NET Core 2.1 varsa, yüklemek için gerek yoktur.

Komutunu kullanarak bir komut isteminden yeni bir proje oluşturun `dotnet new angular` boş bir dizinde. Örneğin, aşağıdaki komutları uygulaması oluşturmak bir *-yeni-Uygulamam* dizini ve bu dizine geçin:

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Uygulama, Visual Studio veya .NET Core CLI çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Oluşturulan açmak *.csproj* dosya ve uygulama buradan normal olarak çalıştırın.

Oluşturma işlemi birkaç dakika sürebilir ilk çalıştırmada npm bağımlılıkları yükler. Sonraki derlemeleri çok daha hızlıdır.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Adlı bir ortam değişkeni olduğundan emin olun `ASPNETCORE_Environment` değerini `Development`. (İçinde olmayan PowerShell komut istemleri) Windows üzerinde çalışan `SET ASPNETCORE_Environment=Development`. Linux veya macOS, çalıştırılan `export ASPNETCORE_Environment=Development`.

Çalıştırma [dotnet yapı](/dotnet/core/tools/dotnet-build) uygulama doğrulamak için derlemeler doğru. İlk çalıştırılmasında oluşturma işlemi birkaç dakika sürebilir npm bağımlılıkları yükler. Sonraki derlemeleri çok daha hızlıdır.

Çalıştırma [çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) uygulamayı başlatmak için. Aşağıdakine benzer bir ileti günlüğe kaydedilir:

```console
Now listening on: http://localhost:<port>
```

Bu URL'yi bir tarayıcıda gidin.

Arka planda Açısal CLI sunucu örneği oluşturan uygulamayı başlatır. Aşağıdakine benzer bir ileti günlüğe kaydedilir: <em>NG Canlı geliştirme sunucusu localhost üzerinde dinleme:&lt;otherport&gt;, tarayıcınızı açmak http://localhost:&lt; otherport&gt; /</em>  . Bu iletiyi göz ardı edin&mdash;sahip <strong>değil</strong> birleşik ASP.NET Core ve Açısal CLI uygulama için URL.

---

Proje şablonu, bir ASP.NET Core uygulama ve Açısal bir uygulama oluşturur. ASP.NET Core uygulama veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır. Bulunan Açısal uygulama *ClientApp* alt, tüm kullanıcı Arabirimi sorunları için kullanılmak üzere tasarlanmıştır.

## <a name="add-pages-images-styles-modules-etc"></a>Sayfaları, görüntüler, stil, modüller, vb. ekleyin.

*ClientApp* dizini standart Açısal CLI uygulama içerir. Resmi görmek [Açısal belgelerine](https://github.com/angular/angular-cli/wiki) daha fazla bilgi için.

Bu şablonla oluşturulan Açısal uygulama ve Açısal CLI tarafından kendi oluşturduğunuz bir arasında küçük farklar vardır (aracılığıyla `ng new`); ancak, uygulama özelliklerini farklı değildir. Şablon tarafından oluşturulan uygulama içeren bir [önyükleme](https://getbootstrap.com/)-tabanlı düzeni ve temel bir yönlendirme örneği.

## <a name="run-ng-commands"></a>NG komutlarını çalıştırın

Bir komut isteminde, geçiş *ClientApp* alt:

```console
cd ClientApp
```

Varsa `ng` genel yüklü aracı, çalıştırabilirsiniz kendi komutlardan herhangi birini. Örneğin, çalıştırabilirsiniz `ng lint`, `ng test`, ya da herhangi bir diğer [Açısal CLI komutları](https://github.com/angular/angular-cli/wiki#additional-commands). Çalıştırmak için gerekli `ng serve` yine de uygulamanız sunucu tarafı ve istemci tarafı bölümlerini hizmet veren ile ASP.NET Core uygulamanızı oluşturulmasıyla ilgili olduğu için. Dahili olarak kullanan `ng serve` geliştirme.

Sahip değilseniz `ng` yüklü, aracı çalıştırmak `npm run ng` yerine. Örneğin, çalıştırabilirsiniz `npm run ng lint` veya `npm run ng test`.

## <a name="install-npm-packages"></a>Npm paket yüklemek için

Üçüncü taraf npm paketleri yüklemek için bir komut isteminde kullanın *ClientApp* alt dizin. Örneğin:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Yayımlama ve dağıtma

Geliştirme, uygulama geliştiriciye kolaylık sağlamak için en iyi hale getirilmiş bir modda çalışır. Örneğin, (hata ayıklama sırasında özgün TypeScript kodunuzu görebilmeniz için) kaynak eşlemeleri JavaScript paketleri içerir. Uygulama diskte TypeScript, HTML ve CSS dosya değişiklikleri izleyen ve otomatik olarak yeniden derler ve bu dosyaları değiştirmek gördüğünde yeniden yükler.

Üretimde, uygulamanızın performansını en iyi duruma getirilmiş sürüm hizmet. Bu, otomatik olarak gerçekleşecek şekilde yapılandırılır. Yayımladığınızda, derleme yapılandırmasını bir küçültülmüş yayar, tamamlanan-in-time (Uygulama Nesne Ağacı), istemci-tarafı kodunuzun yapı derlenmiş. Geliştirme yapı farklı olarak, üretim yapı sunucuya yüklenmesi Node.js gerektirmez (etkinleştirilmiş olduğu sürece [sunucu tarafı prerendering](#server-side-rendering)).

Standart kullanabilirsiniz [ASP.NET Core barındırma ve dağıtma yöntemleri](xref:host-and-deploy/index).

## <a name="run-ng-serve-independently"></a>"Ng hizmet vermemesini" bağımsız olarak çalıştırma

Proje ASP.NET Core uygulama geliştirme modunda başlatıldığında Açısal CLI sunucu örneğini arka planda başlatmak için yapılandırılır. Ayrı bir sunucuya el ile çalıştırmak olmadığından, bu uygundur.

Bu varsayılan kurulum bir dezavantajı yoktur. C# kodunuzu ve ASP.NET uygulama yeniden başlatılmalıdır çekirdek her değiştirdiğinizde Açısal CLI sunucuyu yeniden başlatır. Yaklaşık 10 saniye yeniden başlatılması gerekiyor. Sık C# kod düzenlemeleri yapıyorsanız ve yeniden başlatmak Açısal CLI için beklemek istemiyorsanız, harici olarak Açısal CLI sunucunun bağımsız olarak ASP.NET Core işlemini çalıştırın. Bunu yapmak için:

1. Bir komut isteminde, geçiş *ClientApp* alt ve başlatma Açısal CLI geliştirme sunucusu:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Kullanım `npm start` Açısal CLI geliştirme sunucusu değil başlatmak için `ng serve`, böylece yapılandırmada *package.json* dikkate. Ek parametreleri Açısal CLI sunucuya geçirmek için bunları ilgili Ekle `scripts` satırından, *package.json* dosya.

2. ASP.NET Core uygulamanızı birini kendi başlatmayı yerine dış Açısal CLI örnek kullanacak şekilde değiştirin. İçinde *başlangıç* sınıfı, yerine `spa.UseAngularCliServer` aşağıdaki çağırmayla:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

ASP.NET Core uygulamanızı başlattığınızda Açısal CLI sunucu başlatma olmaz. El ile başlatılan örnek yerine kullanılır. Bu, başlatma ve hızlı yeniden sağlar. Artık her zaman istemci uygulamanızı yeniden Açısal CLI için bekliyor.

## <a name="server-side-rendering"></a>Sunucu tarafı işleme

Bir performans özelliği sunucu yanı sıra istemci üzerinde çalışan Açısal uygulamanıza oluşturma öncesi seçebilirsiniz. Bu tarayıcılar bunlar bile indiriliyor ve JavaScript paketleri yürütmeden önce görüntülemek için uygulamanızın ilk UI temsil eden HTML biçimlendirmesi aldığınız anlamına gelir. Bu uygulama çoğunu gelen adlı bir Açısal özelliğinden [Açısal Evrensel](https://universal.angular.io/).

> [!TIP]
> Sunucu tarafı işleme etkinleştirme (SSR) geliştirme ve dağıtım sırasında fazladan zorluk hem birtakım sunar. Okuma [SSR eksileri](#drawbacks-of-ssr) SSR gereksinimleriniz için iyi bir tercihtir olup olmadığını belirlemek için.

SSR etkinleştirmek için projenize eklemeleri sayısı yapmanız gerekir.

İçinde *başlangıç* sınıfı, *sonra* yapılandırır satır `spa.Options.SourcePath`, ve *önce* çağrısı `UseAngularCliServer` veya `UseProxyToSpaDevelopmentServer`, aşağıdakileri ekleyin:

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

Komut dosyası çalıştırarak SSR paket oluşturmak bu kodu geliştirme modunda çalışır `build:ssr`, içinde tanımlanan *ClientApp\package.json*. Bu adlı bir Açısal uygulama derlemeler `ssr`, henüz tanımlı değil. 

Sonunda `apps` içinde dizi *ClientApp/.angular-cli.json*, ek bir uygulama adıyla tanımlamak `ssr`. Aşağıdaki seçenekleri kullanın:

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

Bu yeni SSR özellikli uygulama yapılandırma iki ek dosyalar gerektirir: *tsconfig.server.json* ve *main.server.ts*. *Tsconfig.server.json* dosya TypeScript derleme seçeneklerini belirtir. *Main.server.ts* dosya sırasında SSR kod giriş noktası olarak hizmet verir.

Adlı yeni bir dosya ekleme *tsconfig.server.json* içinde *ClientApp/src* (varolan yanında *tsconfig.app.json*), aşağıdakileri içeren:

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

Bu dosya adlı bir modül için aranacak Angular'ın Uygulama Nesne Ağacı derleyici yapılandırır `app.server.module`. Bu, yeni bir dosya oluşturarak eklersiniz *ClientApp/src/app/app.server.module.ts* (varolan yanında *app.module.ts*) aşağıdakileri içeren: 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

Bu modül istemci-tarafı devralan `app.module` ve ek Açısal modüllerine SSR sırasında kullanılabilir tanımlar.

Sözcüğünün yeni `ssr` girişi *.angular cli.json* adlı bir giriş noktası dosyası başvurulan *main.server.ts*. Bu dosyayı henüz eklemediniz ve bunu yapmak için zaman sunulmuştur. En yeni bir dosya oluşturun *ClientApp/src/main.server.ts* (varolan yanında *main.ts*), aşağıdakileri içeren:

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

Bu dosyanın kodu çalıştırıldığında ne ASP.NET Core her istek için yürütür `UseSpaPrerendering` eklediğiniz ara yazılımı *başlangıç* sınıfı. Alma ile ilgilenen `params` (örneğin, istenen URL) .NET kodu ve sonuçta elde edilen HTML almak için Açısal SSR API çağrıları yapma. 

Kesinlikle konuşulur, bu SSR geliştirme modunda etkinleştirmek yeterli olur. Böylece yayımlandığında, uygulamanızın düzgün çalıştığını bir son değişiklik yapmak için gereklidir. Uygulamanızın ana içinde *.csproj* dosya, ayarlamak `BuildServerSideRenderer` özellik değerine `true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

Bu derleme işleminin çalışmasına yapılandırır `build:ssr` yayımlama sırasında ve SSR dosyaları sunucuya dağıtın. Bu etkinleştirmezseniz SSR üretimde başarısız olur.

Açısal kod HTML olarak sunucuda uygulamanızı geliştirme veya üretim modunda çalışırken, önceden işler. İstemci tarafı kodu normal olarak yürütür.

### <a name="pass-data-from-net-code-into-typescript-code"></a>Veri .NET kodundan TypeScript koda geçirme

SSR sırasında istek başına veri ASP.NET Core uygulamanızdan Açısal uygulamanıza iletmek isteyebilirsiniz. Örneğin, tanımlama bilgileri geçirebilirdiniz veya bir şey bir veritabanından okuyamadı. Bunu yapmak için düzenleme, *başlangıç* sınıfı. İçin geri çağırma içinde `UseSpaPrerendering`, için bir değer ayarlamanız `options.SupplyData` aşağıdaki gibi:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` Rasgele geçirdiğiniz geri çağırma sağlar, istek başına, JSON seri hale getirilebilir veri (için örnek, dizeleri, Boole değerlerini veya numaralarını). *Main.server.ts* kod aldığında bu olarak `params.data`. Örneğin, yukarıdaki kod örneğinde bir Boole değeri olarak geçirir `params.data.isHttpsRequest` içine `createServerRenderer` geri çağırma. Bu, uygulamanızın Angular tarafından desteklenen herhangi bir yolla diğer bölümleri geçirebilirsiniz. Örneğin, nasıl *main.server.ts* geçirir `BASE_URL` Oluşturucusu alması bildirilmiş herhangi bir bileşen için değer.

### <a name="drawbacks-of-ssr"></a>SSR eksileri

Tüm uygulamaları SSR yararlanır. Birincil avantajı algılanan performansı. Fetch veya JavaScript paketleri ayrıştırılamıyor biraz uzun sürebilir olsa bile bir yavaş ağ bağlantısı üzerinden veya yavaş mobil cihazlarda uygulamanızı ulaşmasını ziyaretçileri ilk UI hızlı bakın. Bununla birlikte, birçok SPAs çoğunlukla hızlı bilgisayarlarda hızlı, iç şirket ağları üzerinden uygulama neredeyse anında göründüğü kullanılır.

Aynı anda SSR etkinleştirme için önemli dezavantajları vardır. Geliştirme sürecinizi karmaşıklık ekler. Kodunuzu iki farklı ortamlarda çalıştırmanız gerekir: istemci ve sunucu tarafı (ASP.NET çekirdek çağrılan bir ortamda Node.js). De hesaba katılmalıdır gereken bazı şeyler şunlardır:

* SSR üretim sunucularınızda Node.js yükleme gerektirir. Bu otomatik olarak Azure App Services gibi bazı dağıtım senaryoları için kullanılabilir ancak Azure Service Fabric gibi diğer durumdur.
* Etkinleştirme `BuildServerSideRenderer` bayrağı nedenler yapı, *node_modules* yayımlamak için dizin. Bu klasör, hangi dağıtım süresini artırır 20.000 + dosyaları içerir.
* Bir Node.js ortamda kodunuzu çalıştırmak için tarayıcı özel JavaScript API varlığını gibi bağlı olamaz `window` veya `localStorage`. Kodunuzu (veya başvuru bazı üçüncü taraf kitaplık) bu API'leri kullanmaya çalışırsa SSR sırasında bir hata iletisi alırsınız. Örneğin, birçok yerde tarayıcıya özgü API başvurduğundan jQuery kullanmayın. Hataları önlemek için SSR önlemek veya tarayıcıya özgü API ya da kitaplıkları kaçının gerekir. Bu tür API'leri yapılan her çağrı sırasında SSR çağrılan olmayan emin olmak için denetimleri kaydırın. Örneğin, aşağıdaki gibi bir denetimi JavaScript veya TypeScript kodda kullanın:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
