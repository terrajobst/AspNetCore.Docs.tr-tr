---
title: ASP.NET Core SignalR TypeScript ve Webpack ile kullanma
author: ssougnez
description: Bu öğreticide, paket ve, istemci TypeScript içinde yazılmış bir ASP.NET Core SignalR web uygulaması oluşturmak için Webpack yapılandırın.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: e4b00fa61ea0becba7d678a0b7c94d2d30f06740
ms.sourcegitcommit: 2941e24d7f3fd3d5e88d27e5f852aaedd564deda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37126431"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>ASP.NET Core SignalR TypeScript ve Webpack ile kullanma

Tarafından [Sébastien Sougnez](https://twitter.com/ssougnez) ve [Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) paketini ve web uygulamasının istemci-tarafı kaynakları derlemeleri geliştiricileri sağlar. Bu öğretici, istemci yazılmış bir ASP.NET Core SignalR web uygulamasında Webpack kullanarak gösterir [TypeScript](https://www.typescriptlang.org/).

Bu öğreticide, bilgi nasıl yapılır:

> [!div class="checklist"]
> * Bir başlangıç ASP.NET Core SignalR uygulaması iskele
> * SignalR TypeScript istemciyi Yapılandırma
> * Webpack kullanarak bir yapı ardışık düzenini yapılandırın
> * SignalR sunucusunu yapılandırın
> * İstemci ve sunucu arasındaki iletişimi etkinleştirmek

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımı yükleyin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) ile [npm](https://www.npmjs.com/)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) sürüm 15.7 veya üstü **ASP.NET ve web geliştirme** iş yükü

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) ile [npm](https://www.npmjs.com/)

---

## <a name="create-the-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Npm aramak için Visual Studio'yu yapılandırma *yolu* ortam değişkeni. Varsayılan olarak, Visual Studio kendi yükleme dizininde bulunan npm sürümünü kullanır. Visual Studio'da bu yönergeleri izleyin:

1. Gidin **Araçları** > **seçenekleri** > **projeler ve çözümler** > **Web paket Yönetimi**  >  **Dış Web Araçları**.
1. Seçin *$(PATH)* listeden girişi. Giriş listede ikinci konuma taşımak için yukarı oka tıklayın. Bir kenara ilk giriş projenin yerel paketlerine başvuruyor.

    ![Visual Studio yapılandırması](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio yapılandırması tamamlandı. Projeyi oluşturmak için zaman yapılır.

1. Kullanım **dosya** > **yeni** > **proje** menü seçeneğini ve seçin **ASP.NET çekirdek Web uygulaması** Şablon.
1. Proje adı *SignalRWebPack*, tıklatıp **Tamam** düğmesi.
1. Seçin *.NET Core* açılır ve select hedef çerçeveden *ASP.NET Core 2.1* framework Seçici açılan'den. Seçin **boş** şablonu ve tıklatın **Tamam** düğmesi.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

Aşağıdaki komutu çalıştırın **tümleşik Terminal**:

```console
dotnet new web -o SignalRWebPack
```

Boş bir ASP.NET Core web uygulaması .NET Core hedefleme oluşturulan bir *SignalRWebPack* dizini.

---

## <a name="configure-webpack-and-typescript"></a>Webpack ve TypeScript yapılandırın

Aşağıdaki adımları TypeScript JavaScript dönüştürülmesi ve istemci tarafı kaynaklarını paketleme yapılandırın.

1. Proje kök dizininde oluşturmak için aşağıdaki komutu yürütün bir *package.json* dosyası:

    ```console
    npm init -y
    ```

1. Vurgulanan özelliği Ekle *package.json* dosyası:

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    Ayarı `private` özelliğine `true` paket yükleme uyarıların bir sonraki adımda engeller.

1. Gerekli npm paketlerini yükleyin. Proje kökünden aşağıdaki komutu yürütün:

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    Dikkat edilecek bazı komut ayrıntıları:

    * Bir sürüm numarası izleyen `@` imzalamak için her paket adı. npm, bu özel paketi sürümleri yüklenir.
    * `-E` Seçeneği, yazma npm'ın varsayılan davranışı devre dışı bırakır [anlamsal sürüm oluşturma](https://semver.org/) aralığı işleçleri *package.json*. Örneğin, `"webpack": "4.12.0"` yerine kullanılan `"webpack": "^4.12.0"`. Bu seçenek, yeni paketi sürümleri istenmeyen yükseltmeler önler.

    Resmi görmek [npm yükleme](https://docs.npmjs.com/cli/install) daha fazla ayrıntı için belgeleri.

1. Değiştir `scripts` özelliği *package.json* aşağıdaki kod parçacığıyla dosyası:

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    Komut dosyaları bazı açıklaması:

    * `build`: Sunmaktadır istemci-tarafı kaynaklarınızı geliştirme modunda ve için dosya değişiklikleri izler. Dosya İzleyici proje dosya değişiklikleri her zaman yeniden oluşturmak paket neden olur. `mode` Seçeneği, ağaç sallayabilir ve küçültme gibi üretim iyileştirmeleri devre dışı bırakır. Yalnızca `build` geliştirme.
    * `release`: İstemci-tarafı kaynaklarınızı üretim modunda sunmaktadır.
    * `publish`: Çalışan `release` üretim modunda istemci-tarafı kaynaklar gruplanacağını komut dosyası. .NET Core CLI çağırır [yayımlama](/dotnet/core/tools/dotnet-publish) uygulama yayımlamak için komutu.

1. Adlı bir dosya oluşturun *webpack.config.js*, proje kök aşağıdaki içeriğe sahip:

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    Önceki dosya Webpack derleme yapılandırır. Dikkat edilecek bazı yapılandırma ayrıntıları:

    * `output` Özelliği varsayılan değerini geçersiz kılar *dağ*. Paket bunun yerine, gösterilen *wwwroot* dizin.
    * `resolve.extensions` Dizi içeriyor *.js* SignalR istemci JavaScript almak için.

1. Yeni bir *src* proje kök dizini. Amacı, projenin istemci-tarafı varlıkları depolamaktır.

1. Oluşturma *src/index.html* aşağıdaki içeriğe sahip.

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    Önceki HTML giriş sayfası'nın Demirbaş biçimlendirme tanımlar.

1. Yeni bir *src/css* dizin. Amacı projenin depolamaktır *.css* dosyaları.

1. Oluşturma *src/css/main.css* aşağıdaki içeriğe sahip:

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    Yukarıdaki *main.css* dosya stiller uygulama.

1. Oluşturma *src/tsconfig.json* aşağıdaki içeriğe sahip:

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    Önceki kod üretmek için TypeScript derleyici yapılandırır [ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 uyumlu JavaScript.

1. Oluşturma *src/index.ts* aşağıdaki içeriğe sahip:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    Önceki TypeScript DOM öğelere başvurular alır ve iki olay işleyicileri ekler:

    * `keyup`: Kullanıcı bir şey olarak tanımlanan metin kutusuna yazdığında bu olay harekete `tbMessage`. `send` Çağrıldığında kullanıcı bastığında **Enter** anahtarı.
    * `click`: Bu olay kullanıcı tıklattığında ateşlenir **Gönder** düğmesi. `send` İşlevi çağrılır.

## <a name="configure-the-aspnet-core-app"></a>ASP.NET Core uygulamayı yapılandırma

1. Sağlanan kod `Startup.Configure` yöntemi görüntüler *Hello World!*. Değiştir `app.Run` yöntemi çağrısı çağrıları ile [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ve [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_).

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    Önceki kod bulun ve hizmet sunucuya verir *index.html* tam URL'sini veya web uygulamasının kök URL'si kullanıcının girdiği olup olmadığını dosya.

1. Çağrı [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) içinde `Startup.ConfigureServices` yöntemi. SignalR Hizmetleri projenize ekler.

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. Harita bir */hub* yönlendirmek `ChatHub` hub. Sonuna şu satırları ekleyin `Startup.Configure` yöntemi:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. Adlı yeni bir dizin oluşturma *hub*, proje kök. Amacı sonraki adımda oluşturduğunuz SignalR hub depolamaktır.

1. Hub'ı oluşturma *Hubs/ChatHub.cs* aşağıdaki kod ile:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. En üstte aşağıdaki kodu ekleyin *haline* çözümlemek için dosya `ChatHub` başvurusu:

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>İstemci ve sunucu iletişimi etkinleştir

Uygulama şu anda iletileri göndermek için basit bir form görüntüler. Bunu yapmak denediğinizde hiçbir şey olmaz. Sunucu, özel bir yol dinleme ancak gönderilen iletilerle hiçbir şey yapmaz.

1. Proje kökündeki aşağıdaki komutu yürütün:

    ```console
    npm install @aspnet/signalr
    ```

    Yukarıdaki komut yükler [SignalR TypeScript istemci](https://www.npmjs.com/package/@aspnet/signalr), sunucuya iletileri göndermek istemci sağlar.

1. Vurgulanmış kodu ekleyin *src/index.ts* dosyası:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    Önceki kod iletileri sunucudan alma destekler. `HubConnectionBuilder` Sınıfı sunucu bağlantısı yapılandırmak için yeni bir oluşturucusu oluşturur. `withUrl` İşlevi hub URL'sini yapılandırır.

    SignalR iletileri bir istemci ve sunucu arasında exchange sağlar. Her ileti belirli bir adı vardır. Örneğin, iletileri ada sahip olabilir `messageReceived` yeni ileti iletileri bölgesinde görüntülemek için sorumlu mantığı yürütün. Aracılığıyla belirli bir ileti dinleme yapılabilir `on` işlevi. Herhangi bir sayıda iletisi adları dinleme. Yazar adı ve alınan ileti içeriği gibi iletisi parametreleri geçirmek mümkündür. İstemcisi bir iletiyi aldıktan sonra yeni bir `div` öğesi oluşturulduğunda yazar adı ve ileti içeriği kendi `innerHTML` özniteliği. Ana eklenen `div` iletilerin görüntüleme öğesi.

1. İstemci bir ileti alabilir, iletileri göndermek için yapılandırın. Vurgulanmış kodu ekleyin *src/index.ts* dosyası:

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    WebSockets bağlantısı üzerinden ileti gönderme gerektirir arama `send` yöntemi. Yöntemin ilk parametresi ileti adıdır. İleti verileri diğer parametreler inhabits. Bu örnekte, bir ileti tanımlanması `newMessage` sunucuya gönderilir. Kullanıcı adı ve kullanıcı bir metin kutusunda girişi, ileti oluşur. Gönderme çalışırsa, metin kutusu değerini temizlenir.

1. Vurgulanan yöntemine ekleyin `ChatHub` sınıfı:

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    Sunucu bunları aldıktan sonra tüm bağlı kullanıcılara alınan iletiler önceki kod yayınlar. Genel sahip gereksizdir `on` tüm iletileri almak için yöntem. İleti adı son eklerini sonra adlı bir yöntem.

    Bu örnekte, TypeScript istemci olarak tanımlanan bir ileti gönderir `newMessage`. C# `NewMessage` yöntemi istemci tarafından gönderilen verileri bekliyor. İçin bir çağrı yapılır [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) yöntemi [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all). Alınan iletileri hub'ına bağlı tüm istemcilere gönderilir.

## <a name="test-the-app"></a>Uygulamayı test etme

Uygulamanın aşağıdaki adımlarla çalıştığını onaylayın.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Webpack Çalıştır *yayın* modu. Kullanarak **Paket Yöneticisi Konsolu** penceresinde, proje kök dizininde aşağıdaki komutu yürütün:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Seçin **hata ayıklama** > **Başlat hata ayıklama olmadan** hata ayıklayıcı eklemeden bir tarayıcıda uygulamayı başlatmak için. *Wwwroot/index.html* adresindeki dosya sunulan `http://localhost:<port_number>`.

1. Başka bir tarayıcı örneği (herhangi bir tarayıcıda) açın. URL'yi adres çubuğuna yapıştırın.

1. Ya da tarayıcı seçin, bir **ileti** metin kutusu ve tıklatın **Gönder** düğmesi. Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. Webpack Çalıştır *yayın* proje kök dizininde aşağıdaki komutu çalıştırarak modu:

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. Derleme ve proje kök dizininde aşağıdaki komutu çalıştırarak uygulamayı çalıştırın:

    ```console
    dotnet run
    ```

    Web sunucusu uygulaması başlatır ve localhost üzerinde kullanılabilir hale getirir.

1. Bir tarayıcıda `http://localhost:<port_number>`. *Wwwroot/index.html* dosya hizmet. Adres çubuğundan URL'yi kopyalayın.

1. Başka bir tarayıcı örneği (herhangi bir tarayıcıda) açın. URL'yi adres çubuğuna yapıştırın.

1. Ya da tarayıcı seçin, bir **ileti** metin kutusu ve tıklatın **Gönder** düğmesi. Benzersiz kullanıcı adı ve ileti iki sayfalarında anında görüntülenir.

---

![Her iki tarayıcı pencerelerini görüntülenen iletisi](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>