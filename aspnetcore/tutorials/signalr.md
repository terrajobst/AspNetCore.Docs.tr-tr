---
title: SignalR ASP.NET Core ile çalışmaya başlama
author: rachelappel
description: Bu öğreticide, ASP.NET Core için SignalR kullanarak uygulama oluşturun.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830561"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>SignalR ASP.NET Core ile çalışmaya başlama

Tarafından [Rachel Appel](https://twitter.com/rachelappel)

Bu öğretici, ASP.NET Core için SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temellerini öğretir.

   ![Çözüm](signalr/_static/signalr-get-started-finished.png)

Bu öğreticide, aşağıdaki SignalR geliştirme görevleri gösterir:

> [!div class="checklist"]
> * Bir SignalR üzerinde ASP.NET Core web uygulaması oluşturun.
> * İçeriği istemcilere göndermek için bir SignalR hub'ı oluşturun.
> * Değiştirme `Startup` sınıfı ve uygulamayı yapılandırır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımları yükleyin:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümünü veya üstünü **ASP.NET ve web geliştirme** iş yükü
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core SDK 2.1 veya üzeri](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio Code için C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>SignalR istemcisi ve sunucusu barındıran bir ASP.NET Core projesi oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Kullanım **dosya** > **yeni proje** menüsünde seçenek ve **ASP.NET Core Web uygulaması**. Projeyi adlandırın *SignalRChat*.

   ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

2. Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmaktır. Ardından **Tamam**. Olduğundan emin olun **ASP.NET Core 2.1** SignalR eski sürümleri .NET üzerinde çalışır ancak framework seçiciden seçilir.

   ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio içerir `Microsoft.AspNetCore.SignalR` parçası olarak sunucu kitaplıklarını içeren paket kendi **ASP.NET Core Web uygulaması** şablonu. Ancak, SignalR için JavaScript istemci kitaplığı kullanılarak yüklenmelidir *npm*.

3. Aşağıdaki komutları çalıştırın **Paket Yöneticisi Konsolu** penceresinden proje kök dizini:

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. İçinde "/ signalr" adlı yeni bir klasör oluşturun *LIB* projenizdeki klasör. Kopyalama *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  bu klasöre.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Gelen **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. JavaScript istemci kitaplığını kullanarak yükleme *npm*.

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. İçinde "/ signalr" adlı yeni bir klasör oluşturun *LIB* projenizdeki klasör. Kopyalama *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  bu klasöre.

---

## <a name="create-the-signalr-hub"></a>SignalR hub'ı oluşturma

Bir hub'ı istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı olarak hizmet veren bir sınıftır.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. Seçim yaparak, projeye bir sınıf ekleyin **dosya** > **yeni** > **dosya** seçerek **Visual C# sınıfı**. Sınıf adı `ChatHub` ve dosya *ChatHub.cs*.

2. Devralınan `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Özellikleri ve olayları göndermeye ve almaya verilerin yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.

3. Oluşturma `SendMessage` yönteminin, tüm bağlı sohbet istemcilere bir ileti gönderir. Fark döndürür bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)SignalR zaman uyumsuz olduğu için. Zaman uyumsuz kod daha iyi ölçeklenir.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Açık *SignalRChat* Visual Studio code'da klasörü.

2. Bir sınıf seçerek projeye ekleyin. **dosya** > **yeni dosya** menüsünde. Sınıf adı `ChatHub` ve dosya *ChatHub.cs*.

3. Devralınan `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Özellikler ve olaylar istemciye gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.

4. Ekleme bir `SendMessage` sınıfı için yöntemi. `SendMessage` Yöntem, tüm bağlı sohbet istemcilere bir ileti gönderir. Fark döndürür bir [görev](/dotnet/api/system.threading.tasks.task)SignalR zaman uyumsuz olduğu için. Zaman uyumsuz kod daha iyi ölçeklenir.

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>Projeyi SignalR kullanacak şekilde yapılandırma

Böylece isteklerini iletmek için SignalR bilir SignalR sunucusunun yapılandırılması gerekir.

1. Bir SignalR projesi yapılandırmak için projenin değiştirme `Startup.ConfigureServices` yöntemi.

   `services.AddSignalR` SignalR Hizmetleri için kullanılabilir hale getirir [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem.

1. Hub'larıyla yolları yapılandırmak `UseSignalR` içinde `Configure` yöntemi. `app.UseSignalR` SignalR için ekler [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>SignalR istemci kodu oluşturma

1. Adlı bir JavaScript dosyası ekleyin *chat.js*, *wwwroot\js* klasör. Aşağıdaki kodu ekleyin:

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   Önceki HTML adı ve ileti alanları ve bir Gönder düğmesi görüntüler. Alttaki komut dosyası başvuruları dikkat edin: SignalR başvurusu ve *chat.js*.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Seçin **hata ayıklama** > **ayıklamadan Başlat** bir tarayıcıyı başlatın ve Web sitesinin yerel olarak yüklemek için. Adres çubuğundan URL'yi kopyalayın.

1. Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın ve adres çubuğuna URL'yi yapıştırın.

1. Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklayın **Gönder** düğmesi. Her iki sayfalarında, adını ve iletisini anında görüntülenir.

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5). Programın çalıştırılması, bir tarayıcı penceresi açılır.

1. Başka bir tarayıcı penceresi açın ve Web sitesinin yerel olarak yükleyin.

1. Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklayın **Gönder** düğmesi. Her iki sayfalarında, adını ve iletisini anında görüntülenir.

---

  ![Çözüm](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>İlgili kaynaklar

[ASP.NET Core signalr'a giriş](xref:signalr/introduction)
