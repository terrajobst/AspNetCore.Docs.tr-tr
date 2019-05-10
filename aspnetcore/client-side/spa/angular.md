---
title: ASP.NET Core ile Angular proje şablonu kullanın
author: SteveSandersonMS
description: ASP.NET Core tek sayfa uygulama (SPA) proje şablonu ile Angular ve Angular CLI'yi kullanmaya nasıl başlayacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: stevesa
ms.custom: mvc
ms.date: 03/07/2019
uid: spa/angular
ms.openlocfilehash: 6d0107ef52d63a0f6f5713c518ddc54ac4230d53
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64899999"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>ASP.NET Core ile Angular proje şablonu kullanın

Güncelleştirilmiş Angular proje şablonu, Angular ve Angular CLI'yi zengin, istemci tarafı kullanıcı arabirimini (UI) kullanarak uygulamaları ASP.NET Core için uygun bir başlama noktası sağlar.

Şablon, bir API arka ucu görev yapacak bir ASP.NET Core projesi ve bir kullanıcı Arabirimi görev yapacak bir Angular CLI'yi projesi oluşturmaya eşdeğerdir. Şablon iki proje türü tek bir uygulama projesinde barındırma kolaylık sunar. Sonuç olarak, uygulama projesi oluşturulabilen ve tek bir birim olarak yayımlandı.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

ASP.NET Core 2.1 yüklü varsa, Angular proje şablonu yüklemek için gerek yoktur.

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

Uygulama arka planda Angular CLI'yi sunucusu olan bir örneğini başlatır. Aşağıdakine benzer bir ileti günlüğe kaydedilir: *Live geliştirme sunucu komut localhost üzerinde dinleme:&lt;otherport&gt;, tarayıcınızı açmak http://localhost:&lt; otherport&gt;/*. Bu iletiyi yoksayın&mdash;sahip **değil** birleşik ASP.NET Core ve Angular CLI'yi app URL'si.

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

Üretim ortamında, uygulamanızın performansını en iyi duruma getirilmiş bir sürümünü işlevi görür. Bu, otomatik olarak gerçekleştirilmesi için yapılandırılır. Yayımladığınızda, derleme yapılandırmasını bir küçültülmüş yayar, istemci tarafı kodunuzun derleme tamamlanan-ın-time (AoT) derlenmiş. Geliştirme derleme, üretim yapı sunucusuna (sunucu tarafı işleme (SSR) etkin sürece) yüklenmesi Node.js gerektirmez.

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

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authentication/identity/spa>
