---
title: TypeScript ve WebPack ile ASP.NET Core SignalR kullanın
author: ssougnez
description: Bu öğreticide, Web paketini istemcisi TypeScript 'te yazılmış bir ASP.NET Core SignalR Web uygulaması paketleyip derlemek üzere yapılandırırsınız.
ms.author: bradyg
ms.custom: mvc
ms.date: 04/23/2019
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 628fbb9940ad14cb15e3abd88b8b6a524b24d70a
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773955"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>TypeScript ve WebPack ile ASP.NET Core SignalR kullanın

, [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Ade](https://twitter.com/Scott_Addie) tarafından

[WebPack](https://webpack.js.org/) , geliştiricilerin bir Web uygulamasının istemci tarafı kaynaklarını paketleyip oluşturmalarına olanak sağlar. Bu öğreticide, istemcisinin [TypeScript](https://www.typescriptlang.org/)'te yazıldığı bir ASP.NET Core SignalR Web uygulamasında WebPack 'in kullanımı gösterilmektedir.

Bu öğreticide şunların nasıl yapıladığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir başlatıcı ASP.NET Core SignalR uygulaması yapı iskelesi
> * SignalR TypeScript istemcisini yapılandırma
> * WebPack kullanarak derleme işlem hattı yapılandırma
> * SignalR sunucusunu yapılandırma
> * İstemci ve sunucu arasındaki iletişimi etkinleştir

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [.NET Core SDK 3,0 veya üzeri](https://www.microsoft.com/net/download/all)
* [NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 3,0 veya üzeri](https://www.microsoft.com/net/download/all)
* [C#Visual Studio Code sürüm 1.17.1 veya üzeri için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)

---

## <a name="create-the-aspnet-core-web-app"></a>ASP.NET Core Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio 'Yu, *Path* ortam değişkeninde NPM için arama yapmak üzere yapılandırın. Varsayılan olarak, Visual Studio yükleme dizininde bulunan NPM sürümünü kullanır. Visual Studio 'da şu yönergeleri izleyin:

1. **Araçlar** **Seçenekler projeler ve çözümler** WebPaketYönetimi> dış Web Araçları ' na gidin. > > >
1. Listeden *$ (yol)* girişini seçin. Girdiyi listedeki ikinci konuma taşımak için yukarı oka tıklayın.

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio yapılandırması tamamlandı. Projeyi oluşturma zamanı.

1. **Dosya** > **Yeni** Proje menü seçeneğini kullanın ve ASP.NET Core Web uygulaması şablonunu seçin. >
1. Projeyi *Signalrwebpack*olarak adlandırın ve **Oluştur**' u seçin.
1. Hedef çerçeve açılır listesinden *.NET Core* ' u seçin ve çerçeve Seçicisi açılır listesinden *ASP.NET Core 3,0* ' ı seçin. **Boş** şablonu seçin ve **Oluştur**' u seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**Tümleşik terminalde**aşağıdaki komutu çalıştırın:

```console
dotnet new web -o SignalRWebPack
```

.NET Core 'u hedefleyen boş bir ASP.NET Core Web uygulaması, bir *Signalrwebpack* dizininde oluşturulur.

---

## <a name="configure-webpack-and-typescript"></a>WebPack ve TypeScript yapılandırma

Aşağıdaki adımlar, TypeScript 'in JavaScript 'e dönüştürülmesini ve istemci tarafı kaynaklarını paketlemeyi yapılandırır.

1. Bir *Package. JSON* dosyası oluşturmak için proje kökünde aşağıdaki komutu yürütün:

    ```console
    npm init -y
    ```

1. Vurgulanan özelliği *Package. JSON* dosyasına ekleyin:

    [!code-json[package.json](signalr-typescript-webpack/sample/3.x/snippets/package1.json?highlight=4)]

    Özelliği, bir sonraki `true` adımda paket yükleme uyarılarını engelleyecek şekilde ayarlanıyor. `private`

1. Gerekli NPM paketlerini yükler. Proje kökünden aşağıdaki komutu yürütün:

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    Aklınızda bazı komut ayrıntıları:

    * Sürüm numarası her paket adı `@` için işareti izler. NPM bu özel paket sürümlerini yüklüyor.
    * Seçeneği `-E` , NPM 'nin [semantik sürüm](https://semver.org/) aralığı işleçlerini *Package. JSON*öğesine yazmanın varsayılan davranışını devre dışı bırakır. Örneğin, `"webpack": "4.29.3"` `"webpack": "^4.29.3"`yerine kullanılır. Bu seçenek, daha yeni paket sürümlerine istenmeden yükseltme yapılmasını engeller.

    Daha ayrıntılı bilgi için resmi [NPM-Install](https://docs.npmjs.com/cli/install) docs bölümüne bakın.

1. *Package. JSON* dosyasının özelliğiniaşağıdakikodparçacığıyladeğiştirin:`scripts`

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Betiklerin bazı açıklamaları:

    * `build`: İstemci tarafı kaynaklarınızı geliştirme modunda paketler ve dosya değişikliklerini izler. Dosya izleyici, bir proje dosyası her değiştiğinde paketin yeniden oluşturulmasına neden olur. `mode` Seçeneği, Tree gerçekleşmesi ve minbirleşme gibi üretim iyileştirmeleri devre dışı bırakır. Yalnızca geliştirme `build` aşamasında kullanın.
    * `release`: İstemci tarafı kaynaklarınızı üretim modunda paketlayın.
    * `publish`: , İstemci tarafı kaynaklarını üretim modunda paketleyip betiğiçalıştırır.`release` Uygulamayı yayımlamak için .NET Core CLI [Publish](/dotnet/core/tools/dotnet-publish) komutunu çağırır.

1. Aşağıdaki içeriğe sahip proje kökünde *WebPack. config. js*adlı bir dosya oluşturun:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/3.x/webpack.config.js)]

    Yukarıdaki dosya Web paketi derlemesini yapılandırır. Aklınızda bazı yapılandırma ayrıntıları:

    * Özelliği, dağ 'ın varsayılan değerini geçersiz kılar. `output` Paket, *Wwwroot* dizininde yayınlanır.
    * Dizi `resolve.extensions` , SignalR istemci JavaScript 'i içeri aktarmak için *. js* içerir.

1. Proje kökünde yeni bir *src* dizini oluşturun. Amaç, projenin istemci tarafı varlıklarını depolardır.

1. Aşağıdaki içerikle *src/index.html* oluşturun.

    [!code-html[index.html](signalr-typescript-webpack/sample/3.x/src/index.html)]

    Önceki HTML, giriş sayfasının ortak işaretlemesini tanımlar.

1. Yeni bir *src/CSS* dizini oluşturun. Amacı, projenin *. css* dosyalarını depolamadır.

1. Aşağıdaki içerikle *src/CSS/Main. css* oluşturun:

    [!code-css[main.css](signalr-typescript-webpack/sample/3.x/src/css/main.css)]

    Önceki *Main. css* dosyası uygulamayı stiller.

1. Şu içerikle *src/tsconfig. JSON* oluşturun:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/3.x/src/tsconfig.json)]

    Yukarıdaki kod, TypeScript derleyicisini [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript üretecek şekilde yapılandırır.

1. Aşağıdaki içeriğe sahip *src/index. TS* oluşturun:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Önceki TypeScript, DOM öğelerine başvuruları alır ve iki olay işleyicisini ekler:

    * `keyup`: Bu olay, Kullanıcı metin kutusuna olarak `tbMessage`tanımlanan bir şeyi yazdığında ateşlenir. Kullanıcı **ENTER** tuşuna bastığında işlevçağrılır.`send`
    * `click`: Bu olay Kullanıcı **Gönder** düğmesine tıkladığında ateşlenir. `send` İşlev çağrılır.

## <a name="configure-the-aspnet-core-app"></a>ASP.NET Core uygulamasını yapılandırma

1. Yönteminde, [usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)öğesine çağrılar ekleyin. `Startup.Configure`

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseStaticDefaultFiles&highlight=2-3)]

   Yukarıdaki kod, kullanıcının tam URL 'sini veya Web uygulamasının kök URL 'sini girmeksizin *Dizin. html* dosyasını bulmasını ve sunmasını sağlar.

1. `Startup.Configure` Metodun sonunda, bir */hub* yolunu `ChatHub` hub 'a eşleyin. Merhaba Dünya görüntülenen kodu değiştirin *!* Aşağıdaki satırla: 

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_UseSignalR&highlight=3)]

1. Yönteminde addsignalr öğesini çağırın. [](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) `Startup.ConfigureServices` SignalR hizmetlerini projenize ekler.

   [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_AddSignalR)]

1. Proje kökünde *hub*olarak adlandırılan yeni bir dizin oluşturun. Amacı, bir sonraki adımda oluşturulan SignalR hub 'ını deposağlamaktır.

1. Aşağıdaki kodla hub *hub 'ları/ChatHub. cs* oluşturun:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. `ChatHub` Başvuruyu çözümlemek için *Startup.cs* dosyasının en üstüne aşağıdaki kodu ekleyin:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/3.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>İstemci ve sunucu iletişimini etkinleştir

Uygulama Şu anda ileti göndermek için basit bir form görüntülüyor. Bunu yapmayı denediğinizde hiçbir şey olmaz. Sunucu belirli bir yolu dinliyor, ancak gönderilen iletilerle hiçbir şey yapmıyor.

1. Proje kökünde aşağıdaki komutu yürütün:

    ```console
    npm install @aspnet/signalr
    ```

    Yukarıdaki komut, istemcisinin sunucuya ileti göndermesini sağlayan [SignalR TypeScript istemcisini](https://www.npmjs.com/package/@aspnet/signalr)yüklüyor.

1. Vurgulanan kodu *src/index. TS* dosyasına ekleyin:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Yukarıdaki kod, sunucudan ileti almayı destekler. `HubConnectionBuilder` Sınıfı, sunucu bağlantısını yapılandırmak için yeni bir Oluşturucu oluşturur. `withUrl` İşlevi hub URL 'sini yapılandırır.

    SignalR, istemci ile sunucu arasında ileti alışverişi yapılmasını mümkün. Her ileti belirli bir ada sahiptir. Örneğin, ileti bölgesindeki yeni iletiyi görüntülemekten sorumlu mantığı `messageReceived` çalıştıran bir ada sahip iletilere sahip olabilirsiniz. Belirli bir iletiyi dinlemek, `on` işlevi aracılığıyla yapılabilir. Herhangi bir sayıda ileti adını dinleyebilmeniz gerekir. Ayrıca, yazarın adı ve alınan iletinin içeriği gibi parametreleri iletiye geçirmek da mümkündür. İstemci bir ileti aldıktan sonra yazarın adı ve onun `div` `innerHTML` özniteliğinde ileti içeriğiyle yeni bir öğe oluşturulur. İletileri görüntüleyen Main `div` öğesine eklenir.

1. Artık istemci bir ileti aldığına göre, ileti gönderecek şekilde yapılandırın. Vurgulanan kodu *src/index. TS* dosyasına ekleyin:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/3.x/src/index.ts?highlight=34-35)]

    WebSockets bağlantısı aracılığıyla bir ileti gönderildiğinde `send` yönteminin çağrılması gerekir. Yöntemin ilk parametresi ileti adıdır. İleti verileri diğer parametreleri geçersiz kılar. Bu örnekte, sunucusuna gönderildiği şekilde `newMessage` tanımlanan bir ileti. İleti, bir metin kutusundan Kullanıcı adından ve Kullanıcı girişinden oluşur. Gönderme işlemi çalışırsa metin kutusu değeri temizlenir.

1. Vurgulanan yöntemi `ChatHub` sınıfa ekleyin:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/3.x/Hubs/ChatHub.cs?highlight=8-11)]

    Yukarıdaki kod, sunucu onları aldıktan sonra tüm bağlı kullanıcılara ileti almış iletileri yayınlar. Tüm iletileri almak için genel `on` bir yöntem olması gereksizdir. İleti adından sonra adlandırılmış bir yöntem.

    Bu örnekte, TypeScript istemcisi olarak `newMessage`tanımlanan bir ileti gönderir. C# istemci tarafından gönderilen verileri bekliyor. `NewMessage` [Istemcilerdeki](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) [sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemine bir çağrı yapılır. Alınan iletiler, hub 'a bağlı tüm istemcilere gönderilir.

## <a name="test-the-app"></a>Uygulamayı test etme

Uygulamanın aşağıdaki adımlarla çalıştığından emin olun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Web paketini *yayın* modunda çalıştırın. **Paket Yöneticisi konsol** penceresini kullanarak, proje kökünde aşağıdaki komutu yürütün. Proje kökünde değilseniz, komutu girmeden önce girin `cd SignalRWebPack` .

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Hata ayıklayıcıyı eklemeden uygulamayı tarayıcıda başlatmak için hata ayıklama**olmadan Başlat** ' **ı seçin.**  >  *Wwwroot/index.html* dosyası ' de `http://localhost:<port_number>`sunulur.

   Derleme hataları alırsanız, çözümü kapatıp yeniden açmayı deneyin. 

1. Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın. URL 'YI adres çubuğuna yapıştırın.

1. Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın. Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Proje kökünde aşağıdaki komutu yürüterek Web paketini *yayın* modunda çalıştırın:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Proje kökünde aşağıdaki komutu yürüterek uygulamayı derleyin ve çalıştırın:

    ```console
    dotnet run
    ```

    Web sunucusu uygulamayı başlatır ve localhost üzerinde kullanılabilir hale getirir.

1. İçin `http://localhost:<port_number>`bir tarayıcı açın. *Wwwroot/index.html* dosyası sunulur. Adres çubuğundan URL 'YI kopyalayın.

1. Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın. URL 'YI adres çubuğuna yapıştırın.

1. Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın. Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.

---

![ileti hem tarayıcı penceresinde görüntüleniyor](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ASP.net ve Web geliştirme** iş yüküyle [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
* [.NET core SDK 2.2 veya üzeri](https://www.microsoft.com/net/download/all)
* [NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET core SDK 2.2 veya üzeri](https://www.microsoft.com/net/download/all)
* [C#Visual Studio Code sürüm 1.17.1 veya üzeri için](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [NPM](https://www.npmjs.com/) ile [Node. js](https://nodejs.org/)

---

## <a name="create-the-aspnet-core-web-app"></a>ASP.NET Core Web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Visual Studio 'Yu, *Path* ortam değişkeninde NPM için arama yapmak üzere yapılandırın. Varsayılan olarak, Visual Studio yükleme dizininde bulunan NPM sürümünü kullanır. Visual Studio 'da şu yönergeleri izleyin:

1. **Araçlar** **Seçenekler projeler ve çözümler** WebPaketYönetimi> dış Web Araçları ' na gidin. > > >
1. Listeden *$ (yol)* girişini seçin. Girdiyi listedeki ikinci konuma taşımak için yukarı oka tıklayın.

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio yapılandırması tamamlandı. Projeyi oluşturma zamanı.

1. **Dosya** > **Yeni** Proje menü seçeneğini kullanın ve ASP.NET Core Web uygulaması şablonunu seçin. >
1. Projeyi *Signalrwebpack*olarak adlandırın ve **Oluştur**' u seçin.
1. Hedef çerçeve açılır listesinden *.NET Core* ' u seçin ve çerçeve Seçicisi açılır listesinden *ASP.NET Core 2,2* ' ı seçin. **Boş** şablonu seçin ve **Oluştur**' u seçin.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

**Tümleşik terminalde**aşağıdaki komutu çalıştırın:

```console
dotnet new web -o SignalRWebPack
```

.NET Core 'u hedefleyen boş bir ASP.NET Core Web uygulaması, bir *Signalrwebpack* dizininde oluşturulur.

---

## <a name="configure-webpack-and-typescript"></a>WebPack ve TypeScript yapılandırma

Aşağıdaki adımlar, TypeScript 'in JavaScript 'e dönüştürülmesini ve istemci tarafı kaynaklarını paketlemeyi yapılandırır.

1. Bir *Package. JSON* dosyası oluşturmak için proje kökünde aşağıdaki komutu yürütün:

    ```console
    npm init -y
    ```

1. Vurgulanan özelliği *Package. JSON* dosyasına ekleyin:

    [!code-json[package.json](signalr-typescript-webpack/sample/2.x/snippets/package1.json?highlight=4)]

    Özelliği, bir sonraki `true` adımda paket yükleme uyarılarını engelleyecek şekilde ayarlanıyor. `private`

1. Gerekli NPM paketlerini yükler. Proje kökünden aşağıdaki komutu yürütün:

    ```console
    npm install -D -E clean-webpack-plugin@1.0.1 css-loader@2.1.0 html-webpack-plugin@4.0.0-beta.5 mini-css-extract-plugin@0.5.0 ts-loader@5.3.3 typescript@3.3.3 webpack@4.29.3 webpack-cli@3.2.3
    ```

    Aklınızda bazı komut ayrıntıları:

    * Sürüm numarası her paket adı `@` için işareti izler. NPM bu özel paket sürümlerini yüklüyor.
    * Seçeneği `-E` , NPM 'nin [semantik sürüm](https://semver.org/) aralığı işleçlerini *Package. JSON*öğesine yazmanın varsayılan davranışını devre dışı bırakır. Örneğin, `"webpack": "4.29.3"` `"webpack": "^4.29.3"`yerine kullanılır. Bu seçenek, daha yeni paket sürümlerine istenmeden yükseltme yapılmasını engeller.

    Daha ayrıntılı bilgi için resmi [NPM-Install](https://docs.npmjs.com/cli/install) docs bölümüne bakın.

1. *Package. JSON* dosyasının özelliğiniaşağıdakikodparçacığıyladeğiştirin:`scripts`

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Betiklerin bazı açıklamaları:

    * `build`: İstemci tarafı kaynaklarınızı geliştirme modunda paketler ve dosya değişikliklerini izler. Dosya izleyici, bir proje dosyası her değiştiğinde paketin yeniden oluşturulmasına neden olur. `mode` Seçeneği, Tree gerçekleşmesi ve minbirleşme gibi üretim iyileştirmeleri devre dışı bırakır. Yalnızca geliştirme `build` aşamasında kullanın.
    * `release`: İstemci tarafı kaynaklarınızı üretim modunda paketlayın.
    * `publish`: , İstemci tarafı kaynaklarını üretim modunda paketleyip betiğiçalıştırır.`release` Uygulamayı yayımlamak için .NET Core CLI [Publish](/dotnet/core/tools/dotnet-publish) komutunu çağırır.

1. Aşağıdaki içeriğe sahip proje kökünde *WebPack. config. js*adlı bir dosya oluşturun:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/2.x/webpack.config.js)]

    Yukarıdaki dosya Web paketi derlemesini yapılandırır. Aklınızda bazı yapılandırma ayrıntıları:

    * Özelliği, dağ 'ın varsayılan değerini geçersiz kılar. `output` Paket, *Wwwroot* dizininde yayınlanır.
    * Dizi `resolve.extensions` , SignalR istemci JavaScript 'i içeri aktarmak için *. js* içerir.

1. Proje kökünde yeni bir *src* dizini oluşturun. Amaç, projenin istemci tarafı varlıklarını depolardır.

1. Aşağıdaki içerikle *src/index.html* oluşturun.

    [!code-html[index.html](signalr-typescript-webpack/sample/2.x/src/index.html)]

    Önceki HTML, giriş sayfasının ortak işaretlemesini tanımlar.

1. Yeni bir *src/CSS* dizini oluşturun. Amacı, projenin *. css* dosyalarını depolamadır.

1. Aşağıdaki içerikle *src/CSS/Main. css* oluşturun:

    [!code-css[main.css](signalr-typescript-webpack/sample/2.x/src/css/main.css)]

    Önceki *Main. css* dosyası uygulamayı stiller.

1. Şu içerikle *src/tsconfig. JSON* oluşturun:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/2.x/src/tsconfig.json)]

    Yukarıdaki kod, TypeScript derleyicisini [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript üretecek şekilde yapılandırır.

1. Aşağıdaki içeriğe sahip *src/index. TS* oluşturun:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Önceki TypeScript, DOM öğelerine başvuruları alır ve iki olay işleyicisini ekler:

    * `keyup`: Bu olay, Kullanıcı metin kutusuna olarak `tbMessage`tanımlanan bir şeyi yazdığında ateşlenir. Kullanıcı **ENTER** tuşuna bastığında işlevçağrılır.`send`
    * `click`: Bu olay Kullanıcı **Gönder** düğmesine tıkladığında ateşlenir. `send` İşlev çağrılır.

## <a name="configure-the-aspnet-core-app"></a>ASP.NET Core uygulamasını yapılandırma

1. `Startup.Configure` Yönteminde belirtilen kod *Merhaba Dünya!* görüntülüyor. Yöntem çağrısını [usedefaultfiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [usestaticfiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)çağrılarıyla değiştirin. `app.Run`

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Yukarıdaki kod, kullanıcının tam URL 'sini veya Web uygulamasının kök URL 'sini girmeksizin *Dizin. html* dosyasını bulmasını ve sunmasını sağlar.

1. `Startup.ConfigureServices` Yönteminde [addsignalr](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) öğesini çağırın. SignalR hizmetlerini projenize ekler.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_AddSignalR)]

1. Bir */hub* yolunu `ChatHub` hub 'a eşleyin. `Startup.Configure` Yönteminin sonuna aşağıdaki satırları ekleyin:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_UseSignalR)]

::: moniker-end

1. Proje kökünde *hub*olarak adlandırılan yeni bir dizin oluşturun. Amacı, bir sonraki adımda oluşturulan SignalR hub 'ını deposağlamaktır.

1. Aşağıdaki kodla hub *hub 'ları/ChatHub. cs* oluşturun:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. `ChatHub` Başvuruyu çözümlemek için *Startup.cs* dosyasının en üstüne aşağıdaki kodu ekleyin:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/2.x/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>İstemci ve sunucu iletişimini etkinleştir

Uygulama Şu anda ileti göndermek için basit bir form görüntülüyor. Bunu yapmayı denediğinizde hiçbir şey olmaz. Sunucu belirli bir yolu dinliyor, ancak gönderilen iletilerle hiçbir şey yapmıyor.

1. Proje kökünde aşağıdaki komutu yürütün:

    ```console
    npm install @aspnet/signalr
    ```

    Yukarıdaki komut, istemcisinin sunucuya ileti göndermesini sağlayan [SignalR TypeScript istemcisini](https://www.npmjs.com/package/@aspnet/signalr)yüklüyor.

1. Vurgulanan kodu *src/index. TS* dosyasına ekleyin:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Yukarıdaki kod, sunucudan ileti almayı destekler. `HubConnectionBuilder` Sınıfı, sunucu bağlantısını yapılandırmak için yeni bir Oluşturucu oluşturur. `withUrl` İşlevi hub URL 'sini yapılandırır.

    SignalR, istemci ile sunucu arasında ileti alışverişi yapılmasını mümkün. Her ileti belirli bir ada sahiptir. Örneğin, ileti bölgesindeki yeni iletiyi görüntülemekten sorumlu mantığı `messageReceived` çalıştıran bir ada sahip iletilere sahip olabilirsiniz. Belirli bir iletiyi dinlemek, `on` işlevi aracılığıyla yapılabilir. Herhangi bir sayıda ileti adını dinleyebilmeniz gerekir. Ayrıca, yazarın adı ve alınan iletinin içeriği gibi parametreleri iletiye geçirmek da mümkündür. İstemci bir ileti aldıktan sonra yazarın adı ve onun `div` `innerHTML` özniteliğinde ileti içeriğiyle yeni bir öğe oluşturulur. İletileri görüntüleyen Main `div` öğesine eklenir.

1. Artık istemci bir ileti aldığına göre, ileti gönderecek şekilde yapılandırın. Vurgulanan kodu *src/index. TS* dosyasına ekleyin:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/2.x/src/index.ts?highlight=34-35)]

    WebSockets bağlantısı aracılığıyla bir ileti gönderildiğinde `send` yönteminin çağrılması gerekir. Yöntemin ilk parametresi ileti adıdır. İleti verileri diğer parametreleri geçersiz kılar. Bu örnekte, sunucusuna gönderildiği şekilde `newMessage` tanımlanan bir ileti. İleti, bir metin kutusundan Kullanıcı adından ve Kullanıcı girişinden oluşur. Gönderme işlemi çalışırsa metin kutusu değeri temizlenir.

1. Vurgulanan yöntemi `ChatHub` sınıfa ekleyin:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/2.x/Hubs/ChatHub.cs?highlight=8-11)]

    Yukarıdaki kod, sunucu onları aldıktan sonra tüm bağlı kullanıcılara ileti almış iletileri yayınlar. Tüm iletileri almak için genel `on` bir yöntem olması gereksizdir. İleti adından sonra adlandırılmış bir yöntem.

    Bu örnekte, TypeScript istemcisi olarak `newMessage`tanımlanan bir ileti gönderir. C# istemci tarafından gönderilen verileri bekliyor. `NewMessage` [Istemcilerdeki](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) [sendadsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemine bir çağrı yapılır. Alınan iletiler, hub 'a bağlı tüm istemcilere gönderilir.

## <a name="test-the-app"></a>Uygulamayı test etme

Uygulamanın aşağıdaki adımlarla çalıştığından emin olun.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Web paketini *yayın* modunda çalıştırın. **Paket Yöneticisi konsol** penceresini kullanarak, proje kökünde aşağıdaki komutu yürütün. Proje kökünde değilseniz, komutu girmeden önce girin `cd SignalRWebPack` .

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Hata ayıklayıcıyı eklemeden uygulamayı tarayıcıda başlatmak için hata ayıklama**olmadan Başlat** ' **ı seçin.**  >  *Wwwroot/index.html* dosyası ' de `http://localhost:<port_number>`sunulur.

1. Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın. URL 'YI adres çubuğuna yapıştırın.

1. Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın. Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Proje kökünde aşağıdaki komutu yürüterek Web paketini *yayın* modunda çalıştırın:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Proje kökünde aşağıdaki komutu yürüterek uygulamayı derleyin ve çalıştırın:

    ```console
    dotnet run
    ```

    Web sunucusu uygulamayı başlatır ve localhost üzerinde kullanılabilir hale getirir.

1. İçin `http://localhost:<port_number>`bir tarayıcı açın. *Wwwroot/index.html* dosyası sunulur. Adres çubuğundan URL 'YI kopyalayın.

1. Başka bir tarayıcı örneği (herhangi bir tarayıcı) açın. URL 'YI adres çubuğuna yapıştırın.

1. Tarayıcı seçin, **ileti** metin kutusuna bir şey yazın ve **Gönder** düğmesine tıklayın. Benzersiz Kullanıcı adı ve ileti anında her iki sayfada da görüntülenir.

---

![ileti hem tarayıcı penceresinde görüntüleniyor](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
