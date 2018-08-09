---
title: 'Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama'
author: tdykstra
description: Bu öğreticide, ASP.NET Core için SignalR kullanan bir sohbet uygulaması oluşturma.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/08/2018
uid: tutorials/signalr
ms.openlocfilehash: 2c1c46b4a608eb0d39287a5261ed7c1f847a644e
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722483"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>Öğretici: SignalR üzerinde ASP.NET Core ile çalışmaya başlama

Bu öğreticide SignalR kullanarak gerçek zamanlı bir uygulama oluşturmaya ilişkin temel bilgileri size öğretir. Aşağıdakilerin nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * SignalR üzerinde ASP.NET Core kullanan bir web uygulaması oluşturun.
> * Bir SignalR hub'ı sunucuda oluşturun.
> * SignalR hub'ına JavaScript istemcilerinden bağlanın.
> * Bağlanan tüm istemciler için herhangi bir istemciden ileti göndermek için hub'ı kullanın.

Sonunda bir çalışma sohbet uygulaması oluşturmuş olacaksınız:

![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample)).

## <a name="prerequisites"></a>Önkoşullar

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 sürüm 15.7.3 veya üzeri](https://www.visualstudio.com/downloads/) ile **ASP.NET ve web geliştirme** iş yükü
* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (SignalR JavaScript istemci kitaplığı için kullanılan Node.js için Paket Yöneticisi.)

---

## <a name="create-the-project"></a>Projeyi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* Menüden **Dosya > Yeni proje**.

* İçinde **yeni proje** iletişim kutusunda **yüklü > Visual C# > Web > ASP.NET Core Web uygulaması**. Projeyi adlandırın *SignalRChat*.

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

* Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmak için.

* Hedef Framework'ü olduğundan emin olun **ASP.NET Core 2.1**ve ardından **Tamam**. 

  ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* Yeni bir proje için kullanabileceğiniz bir klasöre açın.

* İçinde **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="add-the-signalr-client-library"></a>SignalR istemci kitaplığı Ekle

SignalR server kitaplığı dahil [Microsoft.AspnetCore.App metapackage](xref:fundamentals/metapackage-app). Ancak Node.js Paket Yöneticisi olan npm'den JavaScript istemci kitaplığını alma gerekir.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* İçinde **Paket Yöneticisi Konsolu** (PMC) proje klasörüne değiştirin (içeren *SignalRChat.csproj* dosyası).

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. Yeni proje klasörüne geçin.

  ```console
  cd SignalRChat
  ``` 

---

* Oluşturmak için npm Başlatıcı çalıştırma bir *package.json* dosyası:

  ```console
  npm init -y
  ```

  Komut çıktısı aşağıdaki örneğe benzer oluşturur:

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* İstemci Kitaplığı paketi yükleyin:

  ```console
  npm install @aspnet/signalr
  ```

  Komut çıktısı aşağıdaki örneğe benzer oluşturur:

  ```
  npm : npm notice created a lockfile as package-lock.json. You should commit this file.
  At line:1 char:1
  + npm install @aspnet/signalr
  + ~~~~~~~~~~~~~~~~~~~~~~~~~~~
      + CategoryInfo          : NotSpecified: (npm notice crea...mmit this file.:String) [], RemoteException
      + FullyQualifiedErrorId : NativeCommandError
  WARN
   SignalRChat@1.0.0 No description
  WARN
   SignalRChat@1.0.0 No repository field.
  + @aspnet/signalr@1.0.2
  added 1 package in 1.398s
  ```

`npm install` Altında bir alt komut indirdiğiniz JavaScript istemci Kitaplığı *node_modules*. Buradan altında bir klasöre kopyalayın *wwwroot* , sohbet uygulaması web sayfasından başvurabilirsiniz.

* Oluşturma bir *signalr* klasöründe *wwwroot/lib*.

* Kopyalama *signalr.js* dosya *node_modules/@aspnet/signalr/dist/browser* yeni *wwwroot/lib/signalr* klasör.

## <a name="create-the-signalr-hub"></a>SignalR hub'ı oluşturma

A [hub](xref:signalr/hubs) istemci-sunucu iletişimi işleyen bir üst düzey işlem hattı hizmet veren bir sınıftır.

* Oluşturma SignalRChat proje klasöründe bir *Hubs* klasör.

* İçinde *Hubs* klasör oluşturma bir *ChatHub.cs* dosyasındaki kodu aşağıdaki kodla:

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  `ChatHub` Sınıfından devralan SignalR öğesinden [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) sınıfı. `Hub` Sınıfı, bağlantılar, grupları ve mesajlaşma yönetir.

  `SendMessage` Yöntemi herhangi bir bağlı istemci tarafından çağrılabilir. Bu, tüm istemcilere alınan ileti gönderir. SignalR kodudur maksimum ölçeklenebilirlik sağlamak için zaman uyumsuz.

## <a name="configure-the-project-to-use-signalr"></a>Projeyi SignalR kullanacak şekilde yapılandırma

SignalR sunucusu, SignalR için SignalR isteklerini iletmek için yapılandırılmalıdır.

* Aşağıdaki vurgulanmış kodu ekleyin *Startup.cs* dosya.

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  Bu değişiklikler için SignalR ekler [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem ve [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.

## <a name="create-the-signalr-client-code"></a>SignalR istemci kodu oluşturma

* İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki:

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  Yukarıdaki kod:

  * Metin kutuları için adı ve ileti metni ve bir Gönder düğmesi oluşturur.
  * İçeren bir liste oluşturur `id="messagesList"` SignalR hub'ından alınan iletileri görüntülemek için.
  * SignalR için komut dosyası başvuruları içerir ve *chat.js* sonraki adımda oluşturduğunuz uygulama kodu.

* İçinde *wwwroot/js* klasör oluşturma bir *chat.js* dosyasındaki kodu aşağıdaki kodla:

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  Yukarıdaki kod:

  * Oluşturur ve bir bağlantı başlatır.
  * Gönder düğmesine hub'a iletiler gönderen bir işleyici ekler.
  * Bağlantı nesnesi için hub'ından iletiler alan ve bunları listeye ekler bir işleyici ekler.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

* Tuşuna **CTRL + F5** uygulamayı hata ayıklama olmadan çalıştırmak için.

* Adres çubuğundan URL'yi kopyalayın, başka bir tarayıcı örneğinde veya sekmesi açın ve adres çubuğuna URL'yi yapıştırın.

* Ya da tarayıcı seçin, bir ad ve ileti girin ve seçin **Gönder** düğmesi.

  Her iki sayfalarında, adını ve iletisini anında görüntülenir.

  ![SignalR örnek uygulaması](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> Uygulama işe yaramazsa, tarayıcı Geliştirici Araçları'nı (F12) açın ve konsoluna gidin. HTML ve JavaScript kodunuza ilgili hatalar görebilirsiniz. Örneğin, eklediğiniz varsayalım *signalr.js* yönlendirilmiş değerinden farklı bir klasöre. Bu durumda bu dosyaya başvuru çalışmaz ve bir 404 hatası konsolunda görürsünüz.
> ![signalr.js bulunamadı hatası](signalr/_static/f12-console.png)

## <a name="next-steps"></a>Sonraki adımlar

Farklı etki alanlarından bir SignalR uygulamalarına bağlanabilmeniz için istemcilerin istiyorsanız, çıkış noktaları arası kaynak paylaşımı (CORS) etkinleştirme gerekir. Daha fazla bilgi için [çıkış noktaları arası kaynak paylaşımı](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing).

SignalR hub'ları ve JavaScript istemcileri hakkında daha fazla bilgi için şu kaynaklara bakın:

* [ASP.NET Core signalr'a giriş](xref:signalr/introduction)
* [ASP.NET Core signalr'da hubs'ı kullanma](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript istemcisi](xref:signalr/javascript-client)
