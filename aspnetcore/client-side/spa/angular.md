---
title: Angular proje şablonunu ASP.NET Core ile kullanma
author: SteveSandersonMS
description: Angular ve angular CLı için ASP.NET Core tek sayfalı uygulama (SPA) proje şablonunu kullanmaya nasıl başlaleyeceğinizi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: stevesa
ms.custom: mvc
ms.date: 03/07/2019
uid: spa/angular
ms.openlocfilehash: 150b2176eac2e68c1ef9ec6deabb087836ff84ce
ms.sourcegitcommit: cb6015f737b6a93127016ab0f21b58e34b624ff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77004272"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>Angular proje şablonunu ASP.NET Core ile kullanma

Güncelleştirilmiş angular proje şablonu, zengin, istemci tarafı Kullanıcı arabirimi (UI) uygulamak için angular ve angular CLı kullanan ASP.NET Core uygulamalar için uygun bir başlangıç noktası sağlar.

Şablon, bir API arka ucu görevi gören bir ASP.NET Core projesi ve Kullanıcı arabirimi olarak davranacak angular CLı projesi oluşturma ile eşdeğerdir. Şablon, her iki proje türünü tek bir uygulama projesinde barındırmanın kolaylığını sunar. Sonuç olarak, uygulama projesi tek bir birim olarak oluşturulup yayımlanabilir.

## <a name="create-a-new-app"></a>Yeni bir uygulama oluşturma

ASP.NET Core 2,1 yüklüyse, angular proje şablonunu yüklemeye gerek yoktur.

Boş bir dizinde komut `dotnet new angular` kullanarak komut isteminden yeni bir proje oluşturun. Örneğin, aşağıdaki komutlar uygulamayı *Yeni bir uygulama* dizininde oluşturur ve bu dizine geçer:

```dotnetcli
dotnet new angular -o my-new-app
cd my-new-app
```

Uygulamayı Visual Studio 'dan veya .NET Core CLI çalıştırın:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

Oluşturulan *. csproj* dosyasını açın ve uygulamayı buradan normal olarak çalıştırın.

Yapı işlemi ilk çalıştırmada NPM bağımlılıklarını geri yükler ve bu işlem birkaç dakika sürebilir. Sonraki derlemeler çok daha hızlıdır.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

`Development`değerine sahip `ASPNETCORE_Environment` adlı bir ortam değişkenine sahip olduğunuzdan emin olun. Windows 'da (PowerShell olmayan istemlerinde) `SET ASPNETCORE_Environment=Development`çalıştırın. Linux veya macOS üzerinde `export ASPNETCORE_Environment=Development`çalıştırın.

Uygulamanın doğru bir şekilde derlemelerin doğrulanması için [DotNet derlemesini](/dotnet/core/tools/dotnet-build) çalıştırın. İlk çalıştırmada, derleme işlemi NPM bağımlılıklarını geri yükler ve bu işlem birkaç dakika sürebilir. Sonraki derlemeler çok daha hızlıdır.

Uygulamayı başlatmak için [DotNet çalıştırmasını](/dotnet/core/tools/dotnet-run) çalıştırın. Aşağıdakine benzer bir ileti günlüğe kaydedilir:

```console
Now listening on: http://localhost:<port>
```

Bu URL 'yi bir tarayıcıda gezin.

Uygulama arka planda angular CLı sunucusunun bir örneğini başlatır. Aşağıdakine benzer bir ileti günlüğe kaydedilir: *ng Live Development Server, localhost üzerinde dinleme yapıyor:&lt;otherport&gt;, tarayıcınızı http://localhost:&lt ; diğer bağlantı noktası&gt;/ açın* . Bu iletiyi yoksayın&mdash;birleştirilmiş ASP.NET Core ve angular CLı uygulamasının URL 'SI **değildir** .

---

Proje şablonu, bir ASP.NET Core uygulaması ve angular uygulaması oluşturur. ASP.NET Core uygulaması veri erişimi, yetkilendirme ve diğer sunucu tarafı sorunları için kullanılmak üzere tasarlanmıştır. *Clientapp* alt dizininde bulunan angular uygulamasının tüm Kullanıcı arabirimi sorunları için kullanılması amaçlanmıştır.

## <a name="add-pages-images-styles-modules-etc"></a>Sayfa, resim, stil, modül vb. ekleyin

*Clientapp* dizini standart ANGULAR CLI uygulaması içerir. Daha fazla bilgi için bkz. resmi [angular belgeleri](https://https://angular.io) .

Bu şablon tarafından oluşturulan angular uygulaması ve angular CLı 'nın kendisi tarafından oluşturulan (`ng new`aracılığıyla) hafif farklar vardır; Ancak, uygulamanın özellikleri değiştirilmez. Şablon tarafından oluşturulan uygulama, [önyükleme](https://getbootstrap.com/)tabanlı bir düzen ve temel bir yönlendirme örneği içerir.

## <a name="run-ng-commands"></a>Çalıştır komutları

Bir komut isteminde *clientapp* alt dizinine geçin:

```console
cd ClientApp
```

`ng` aracı genel olarak yüklüyse, komutlarından herhangi birini çalıştırabilirsiniz. Örneğin, `ng lint`, `ng test`veya diğer [angular CLI komutlarından](https://angular.io/cli)herhangi birini çalıştırabilirsiniz. ASP.NET Core uygulamanız uygulamanızın sunucu tarafı ve istemci tarafı bölümlerine sunma konusunda anlaştığından, `ng serve` çalışmasına gerek yoktur. Dahili olarak, geliştirme sırasında `ng serve` kullanır.

`ng` aracı yüklü değilse, bunun yerine `npm run ng` çalıştırın. Örneğin, `npm run ng lint` veya `npm run ng test`çalıştırabilirsiniz.

## <a name="install-npm-packages"></a>NPM paketlerini yükler

Üçüncü taraf NPM paketlerini yüklemek için *clientapp* alt dizininde bir komut istemi kullanın. Örneğin:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>Yayımla ve dağıt

Geliştirme aşamasında uygulama, geliştirici kolaylığı için iyileştirilmiş bir modda çalışır. Örneğin, JavaScript demeti kaynak eşlemeleri içerir (hata ayıklarken, özgün TypeScript kodunuzu görebilirsiniz). Uygulama TypeScript, HTML ve CSS dosya değişikliklerini diskte izler ve bu dosya değişikliğini gördüğünde otomatik olarak yeniden derlenir ve yeniden yükler.

Üretimde, uygulamanızın performans için iyileştirilmiş bir sürümünü sunar. Bu otomatik olarak gerçekleşecek şekilde yapılandırılmıştır. Yayımladığınızda, derleme yapılandırması, istemci tarafı kodunuzun küçültülmüş, önceden derlenmiş bir derlemesini yayar. Geliştirme derlemesinin aksine, üretim derlemesi, Node. js ' nin sunucuya yüklenmesini gerektirmez (sunucu tarafı işlemesini (SSR) etkinleştirmediğiniz müddetçe).

Standart [ASP.NET Core barındırma ve dağıtım yöntemleri](xref:host-and-deploy/index)kullanabilirsiniz.

## <a name="run-ng-serve-independently"></a>Bağımsız olarak "ng hizmeti" Çalıştır

Proje, ASP.NET Core uygulaması geliştirme modunda başladığında arka planda kendi angular CLı sunucusu örneğini başlatacak şekilde yapılandırılmıştır. Ayrı bir sunucuyu el ile çalıştırmak zorunda olmadığınızdan bu kullanışlıdır.

Bu varsayılan Kurulumun bir dezavantajı vardır. C# Kodunuzun her değiştirilişinde ve ASP.NET Core uygulamanızın yeniden başlatılması gerektiğinde, ANGULAR CLI sunucusu yeniden başlatılır. Yeniden başlatmak için yaklaşık 10 saniye gereklidir. Sık karşılaşılan C# kod düzenlemeleri yapıyorsanız ve ANGULAR CLI 'nın yeniden başlatılmasını beklemek Istemiyorsanız, angular clı sunucusunu ASP.NET Core işleminden bağımsız olarak dışarıdan çalıştırın. Bunu yapmak için:

1. Bir komut isteminde *clientapp* alt dizinine geçin ve ANGULAR CLI geliştirme sunucusunu başlatın:

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > *Package. JSON* içindeki yapılandırmanın kullanılması için `ng serve`değil, ANGULAR CLI geliştirme sunucusunu başlatmak için `npm start` kullanın. Angular CLı sunucusuna ek parametreler geçirmek için bunları *Package. JSON* dosyanızdaki ilgili `scripts` satırına ekleyin.

2. ASP.NET Core uygulamanızı, kendi kendine birini başlatmak yerine dış angular CLı örneğini kullanacak şekilde değiştirin. *Başlangıç* sınıfınıza `spa.UseAngularCliServer` çağrılmasını aşağıdaki kodla değiştirin:

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

ASP.NET Core uygulamanızı başlattığınızda, angular CLı sunucusunu başlatmayacaktır. Bunun yerine el ile başlattığınız örnek kullanılır. Bu, daha hızlı başlamasını ve yeniden başlatılmasını sağlar. Artık, istemci uygulamanızı her seferinde yeniden oluşturmak için angular CLı bekliyor.

### <a name="pass-data-from-net-code-into-typescript-code"></a>.NET kodundan verileri TypeScript koduna geçirme

SSR sırasında, ASP.NET Core uygulamanızdaki istek başına verileri angular uygulamanıza geçirmek isteyebilirsiniz. Örneğin, tanımlama bilgisi bilgilerini veya bir veritabanından okunan bir şeyi geçirebilirsiniz. Bunu yapmak için *Başlangıç* sınıfınızı düzenleyin. `UseSpaPrerendering`için geri çağırmada, `options.SupplyData` için aşağıdaki gibi bir değer ayarlayın:

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` geri çağırma, isteğe bağlı olarak, isteğe bağlı, JSON ile seri hale getirilebilir verileri (örneğin, dizeler, Boole değerleri veya sayılar) geçirmenize olanak sağlar. *Main. Server. TS* kodunuz bunu `params.data`olarak alır. Örneğin, yukarıdaki kod örneği `createServerRenderer` geri çağırması için `params.data.isHttpsRequest` olarak bir Boole değeri geçirir. Bunu uygulamanızın diğer bölümlerine, angular tarafından desteklenen herhangi bir yolla geçirebilirsiniz. Örneğin, bkz. *Main. Server. TS* , `BASE_URL` değerini Oluşturucusu tarafından almak için bildirildiği herhangi bir bileşene geçirir.

### <a name="drawbacks-of-ssr"></a>SSR 'nin dezavantajları

Tüm uygulamalar SSR 'den faydalanır. Birincil avantaj, performans algılanır. Yavaş bir ağ bağlantısı veya yavaş mobil cihazlarda uygulamanıza ulaşan ziyaretçiler, JavaScript paketlerini getirme veya ayrıştırma sırasında bile ilk Kullanıcı arabirimine hızlı bir şekilde bakın. Ancak, çoğu maça genellikle uygulamanın neredeyse anında göründüğü hızlı bilgisayarlarda hızlı, dahili şirket ağları üzerinde kullanılır.

Aynı zamanda SSR 'yi etkinleştirmenin önemli sakıncaları vardır. Geliştirme sürecinizi karmaşıklık altına ekler. Kodunuzun iki farklı ortamda çalışması gerekir: istemci tarafı ve sunucu tarafı (ASP.NET Core bir Node. js ortamında çağrılır). Göz önünde bulundurmanız gereken bazı şeyler aşağıda verilmiştir:

* SSR, üretim sunucularınızda bir Node. js yüklemesi gerektirir. Azure uygulama hizmetleri gibi bazı dağıtım senaryolarında bu durum otomatik olarak yapılır, ancak Azure Service Fabric gibi diğerleri için de değildir.
* `BuildServerSideRenderer` yapı bayrağını etkinleştirmek *node_modules* dizininizin yayımlamasına neden olur. Bu klasör, dağıtım süresini artıran 20000 + dosyalarını içerir.
* Kodunuzu bir Node. js ortamında çalıştırmak için, `window` veya `localStorage`gibi tarayıcıya özgü JavaScript API 'Lerinin varlığına bağlı olamaz. Kodunuz (veya başvuru yaptığınız bazı üçüncü taraf kitaplığı) Bu API 'Leri kullanmayı denediğinde SSR sırasında bir hata alırsınız. Örneğin, birçok yerde tarayıcıya özgü API 'Lere başvurduğundan jQuery kullanmayın. Hataları önlemek için SSR 'yi kullanmaktan veya tarayıcıya özgü API 'lerden ya da kitaplıklardan kaçınmalısınız. Bu tür API 'lere yapılan çağrıları SSR sırasında çağrıdıklarından emin olmak için, denetimleri içinde kaydırabilirsiniz. Örneğin, JavaScript veya TypeScript kodunda aşağıdakiler gibi bir denetim kullanın:

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:security/authentication/identity/spa>
