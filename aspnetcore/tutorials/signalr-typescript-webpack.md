---
title: TypeScript ve Web ile ASP.NET Core SignalR kullanma
author: ssougnez
description: Bu öğreticide, Web, istemci, içinde TypeScript yazılmış bir ASP.NET Core SignalR web uygulaması derleme ve paket için yapılandırın.
ms.author: bradyg
ms.custom: mvc
ms.date: 11/30/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 8292ab2e0ad1f5c67ac7f15c280b49700f6717ad
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836330"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>TypeScript ve Web ile ASP.NET Core SignalR kullanma

Tarafından [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Addie](https://twitter.com/Scott_Addie)

[Web](https://webpack.js.org/) geliştiricilerin paket ve derleme bir web uygulamasının istemci-tarafı kaynakları sağlar. Web istemcisi yazılmış bir ASP.NET Core SignalR web uygulaması kullanarak bu eğitimde [TypeScript](https://www.typescriptlang.org/).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir başlangıç ASP.NET Core SignalR uygulaması iskelesini
> * SignalR TypeScript istemciyi Yapılandırma
> * Web kullanarak bir derleme işlem hattı yapılandırın
> * SignalR sunucusunu yapılandırma
> * İstemci ve sunucu arasında iletişimi etkinleştirin

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr-typescript-webpack/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

[!INCLUDE [Prerequisites](~/includes/net-core-prereqs-vs-vsc-2.2.md)]

## <a name="create-the-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

' Nde npm aramak için Visual Studio'yu yapılandırma *yolu* ortam değişkeni. Varsayılan olarak, Visual Studio, yükleme dizininde bulunan npm sürümünü kullanır. Visual Studio'da bu yönergeleri izleyin:

1. Gidin **Araçları** > **seçenekleri** > **projeler ve çözümler** > **WebpaketYönetimi**  >  **Dış Web Araçları**.
1. Seçin *$(PATH)* listeden girişi. Giriş listede ikinci konuma taşımak için yukarı oka tıklayın.

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio yapılandırması tamamlandı. Projeyi oluşturmak için zaman var.

1. Kullanım **dosya** > **yeni** > **proje** menüsünde seçenek ve **ASP.NET Core Web uygulaması** şablonu.
1. Projeyi adlandırın *SignalRWebPack*seçip **Tamam**.
1. Seçin *.NET Core* açılır ve select hedef çerçeveden *ASP.NET Core 2.2* framework Seçici açılan menüsü'nden. Seçin **boş** şablonu ve select **Tamam**.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Aşağıdaki komutu çalıştırın **tümleşik Terminalini**:

```console
dotnet new web -o SignalRWebPack
```

Boş bir ASP.NET Core web uygulaması .NET Core'u hedefleyen oluşturulan bir *SignalRWebPack* dizin.

---

## <a name="configure-webpack-and-typescript"></a>Web ve TypeScript yapılandırın

Aşağıdaki adımlar, JavaScript, TypeScript, dönüştürme ve istemci tarafı kaynaklarını paketleme yapılandırın.

1. Aşağıdaki komutu yürütün oluşturmak için proje kök dizininde bir *package.json* dosyası:

    ```console
    npm init -y
    ```

1. Vurgulanan özellik ekleyin *package.json* dosyası:

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    Ayarı `private` özelliğini `true` sonraki adımda paket yükleme uyarıları engeller.

1. Gerekli npm paketlerini yükleyin. Proje kökünden aşağıdaki komutu yürütün:

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Dikkat edilecek bazı komut ayrıntıları:

    * Bir sürüm numarasından `@` her paket adı için oturum açın. npm, bu belirli bir paket sürümlerini yükler.
    * `-E` Seçeneği, yazma npm'ın varsayılan davranışı devre dışı bırakır [semantic versioning](https://semver.org/) işleçler için aralığı *package.json*. Örneğin, `"webpack": "4.12.0"` yerine kullanılan `"webpack": "^4.12.0"`. Bu seçenek, istenmeyen yükseltmeleri için yeni bir paket sürümlerini önler.

    Resmi görmek [npm yükleme](https://docs.npmjs.com/cli/install) daha fazla ayrıntı için belgeleri.

1. Değiştirin `scripts` özelliği *package.json* aşağıdaki kod parçacığı dosyası:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Bazı komut dosyaları açıklaması:

    * `build`: İstemci tarafı kaynaklarınızı geliştirme modunda oluşturur ve dosya değişikliklerini izler. Dosya İzleyici, paket her zaman bir proje dosya değişiklikleri yeniden neden olur. `mode` Seçeneği, ağaç sallayabilir ve küçültme gibi üretim iyileştirmeleri devre dışı bırakır. Yalnızca `build` geliştirme.
    * `release`: İstemci tarafı kaynaklarınızı üretim modunda oluşturur.
    * `publish`: Çalıştırmaları `release` üretim modunda istemci-tarafı kaynakları paket betiği. .NET Core CLI'ın çağırdığı [yayımlama](/dotnet/core/tools/dotnet-publish) uygulamayı yayımlamak için komutu.

1. Adlı bir dosya oluşturun *webpack.config.js*, proje kökündeki aşağıdaki içeriğe sahip:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Önceki dosya Web Derleme yapılandırır. Dikkat edilecek bazı yapılandırma ayrıntıları:

    * `output` Özelliğini geçersiz kılar, varsayılan değerini *dist*. Paket yerine yayıldığını *wwwroot* dizin.
    * `resolve.extensions` Dizi içeren *.js* SignalR istemci JavaScript içeri aktarmak için.

1. Yeni bir *src* proje kök dizini. Amacı, projenin istemci-tarafı varlıkları depolamaktır.

1. Oluşturma *src/index.html* aşağıdaki içeriğe sahip.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Önceki HTML spotuna Demirbaş biçimlendirme tanımlar.

1. Yeni bir *src/css* dizin. Amacı, projenin depolamaktır *.css* dosyaları.

1. Oluşturma *src/css/main.css* aşağıdaki içeriğe sahip:

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Önceki *main.css* dosya stiller uygulama.

1. Oluşturma *src/tsconfig.json* aşağıdaki içeriğe sahip:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Yukarıdaki kod üretmek için TypeScript derleyicisi yapılandırır [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 ile uyumlu JavaScript.

1. Oluşturma *src/index.ts* aşağıdaki içeriğe sahip:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Önceki TypeScript DOM öğeleri için başvuru alır ve iki olay işleyicisi ekler:

    * `keyup`: Kullanıcı bir şey olarak tanımlanmış metin kutusuna yazdığında, bu olay harekete `tbMessage`. `send` İşlevi, kullanıcının bastığında çağrılır **Enter** anahtarı.
    * `click`: Kullanıcı tıkladığında bu olay harekete **Gönder** düğmesi. `send` İşlevi çağrılır.

## <a name="configure-the-aspnet-core-app"></a>ASP.NET Core uygulaması yapılandırma

1. Sağlanan kod `Startup.Configure` yöntemi görüntüler *Merhaba Dünya!*. Değiştirin `app.Run` yöntem çağrısının çağrılarıyla [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Yukarıdaki kod bulun ve hizmet sunucuya sağlar *index.html* olmadığını tam URL'sini veya web uygulaması kök URL'sini kullanıcının girdiği dosya.

1. Çağrı [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) içinde `Startup.ConfigureServices` yöntemi. SignalR Hizmetleri projenize ekler.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Harita bir */hub* yönlendirmek `ChatHub` hub. Sonunda aşağıdaki satırları ekleyin `Startup.Configure` yöntemi:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Adlı yeni bir dizin oluşturma *Hubs*, proje kökündeki. Amacı sonraki adımda oluşturduğunuz SignalR hub depolamaktır.

1. Hub'ı oluşturma *Hubs/ChatHub.cs* aşağıdaki kod ile:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. Üstüne aşağıdaki kodu ekleyin *Startup.cs* çözümlemek için dosya `ChatHub` başvurusu:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>İstemci ve sunucu iletişimi etkinleştirin

Uygulama şu anda ileti göndermek için basit bir form görüntüler. Bunu yapmak denediğinizde hiçbir şey olmaz. Sunucu, belirli bir yol dinleme ancak gönderilen iletilerle hiçbir şey yapmaz.

1. Proje kök dizinini aşağıdaki komutu yürütün:

    ```console
    npm install @aspnet/signalr
    ```

    Önceki komutta yükler [SignalR TypeScript istemci](https://www.npmjs.com/package/@aspnet/signalr), istemcinin sunucuya ileti göndermek izin verir.

1. Vurgulanmış kodu ekleyin *src/index.ts* dosyası:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Yukarıdaki kod, sunucudan iletileri alma destekler. `HubConnectionBuilder` Sınıf sunucu bağlantısı yapılandırmak için yeni bir oluşturucu oluşturur. `withUrl` İşlevi hub URL'sini yapılandırır.

    SignalR, bir istemci ve sunucu arasında ileti alışverişi sağlar. Her ileti, belirli bir adı vardır. Örneğin, iletileri adıyla olabilir `messageReceived` iletileri bölgede yeni bir ileti görüntülemek için sorumlu mantıksal yürütün. Aracılığıyla belirli bir ileti için dinleme yapılabilir `on` işlevi. Herhangi bir sayıda ileti adları dinleyebilirsiniz. Yazarın adı ve alınan ileti içeriği gibi iletisi parametreleri geçirmek mümkündür. İstemci bir ileti aldıktan sonra yeni bir `div` öğesi oluşturulduğunda yazarın adı ve şu iletiyle içeriği kendi `innerHTML` özniteliği. Ana eklenir `div` iletileri görüntülemeyi öğesi.

1. İstemci bir ileti alabilir, ileti göndermek için yapılandırın. Vurgulanmış kodu ekleyin *src/index.ts* dosyası:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    WebSockets bağlantısı üzerinden ileti gönderme, arama gerektirir `send` yöntemi. Yöntemin ilk parametresi ileti adıdır. İleti verileri diğer parametreler inhabits. Bu örnekte, bir ileti olarak tanımlanır. `newMessage` sunucuya gönderilir. Kullanıcı adı ve kullanıcı bir metin kutusunda girişi ileti oluşur. Gönderme çalışırsa, metin kutusunun değerini temizlenir.

1. Vurgulanan yöntemine ekleyin `ChatHub` sınıfı:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Sunucu bunları aldıktan sonra önceki kod bağlı tüm kullanıcılar için alınan iletiler yayımlar. Genel gereksizdir `on` tüm iletileri almak için yöntemi. İleti adı eklerini sonra adlı bir yöntem.

    Bu örnekte, TypeScript istemci olarak tanımlanan bir ileti gönderir. `newMessage`. C# `NewMessage` yöntemi istemci tarafından gönderilen verilerin bekliyor. İçin bir çağrı yapılır [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) metodunda [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Alınan iletiler, hub'ına bağlı tüm istemcilere gönderilir.

## <a name="test-the-app"></a>Uygulamayı test etme

Uygulama aşağıdaki adımlarla çalışır durumda olduğunu doğrulayın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Web Çalıştır *yayın* modu. Kullanarak **Paket Yöneticisi Konsolu** penceresinde proje kök dizininde aşağıdaki komutu yürütün. Proje kök dizininde değilse girin `cd SignalRWebPack` komutu ulaşmadan önce.

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Seçin **hata ayıklama** > **ayıklamadan Başlat** Haya ayıklayıcı eklemeden uygulamayı bir tarayıcıda başlatmak için. *Wwwroot/index.html* dosya hizmet `http://localhost:<port_number>`.

1. Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın. Adres çubuğuna URL'yi yapıştırın.

1. Ya da tarayıcı seçin, bir **ileti** metin kutusu seçeneğine tıklayıp **Gönder** düğmesi. Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Web Çalıştır *yayın* proje kök dizininde aşağıdaki komutu yürüterek modu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Yapı ve proje kök dizininde aşağıdaki komutu çalıştırarak uygulamayı çalıştırın:

    ```console
    dotnet run
    ```

    Web sunucusu uygulama başlar ve komut localhost üzerinde kullanılabilir hale getirir.

1. Bir tarayıcıda `http://localhost:<port_number>`. *Wwwroot/index.html* dosya hizmet. Adres çubuğundan URL'yi kopyalayın.

1. Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın. Adres çubuğuna URL'yi yapıştırın.

1. Ya da tarayıcı seçin, bir **ileti** metin kutusu seçeneğine tıklayıp **Gönder** düğmesi. Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.

---

![Her iki Tarayıcı pencerelerinde görüntülenen ileti](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
