---
title: "ASP.NET Core üzerinde SignalR ile çalışmaya başlama"
author: rachelappel
ms.author: rachelap
description: "Bu öğreticide, SignalR için ASP.NET Core kullanarak bir uygulama oluşturun."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>Öğretici: ASP.NET Core için SignalR ile çalışmaya başlama

Tarafından [Rachel Appel](https://twitter.com/rachelappel)

Bu öğretici, SignalR için ASP.NET Core kullanarak gerçek zamanlı bir uygulama oluşturma temelleri öğretir.

   ![Çözüm](get-started-signalr-core/_static/signalr-get-started-finished.png)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu öğretici aşağıdaki SignalR geliştirme görevleri gösterir:

> [!div class="checklist"]
> * Bir ASP.NET Core web uygulaması oluşturun.
> * İçeriği istemcilere göndermek için bir SignalR hub'ı oluşturun.
> * SignalR JavaScript kitaplığı iletilerini göndermek ve hub güncelleştirmelerini görüntülemek için kullanın.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yazılımı yükleyin:

* [.NET core 2.1.0 Önizleme 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) veya daha yenisi
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 veya ASP.NET ve web geliştirme iş yükü ile sonraki bir sürümü
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>SignalR istemcisi ve sunucusu barındıran bir ASP.NET Core projesi oluşturma

1. Kullanım **dosya** > **yeni proje** menü seçeneğini ve seçin **ASP.NET çekirdek Web uygulaması**. Proje adı `SignalRChat`.

  ![Visual Studio'da yeni proje iletişim kutusu](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. Seçin **Web uygulaması** Razor sayfalarının kullanarak bir proje oluşturmak için. Ardından **Tamam**. Olduğundan emin olun **ASP.NET Core 2.1** SignalR .NET eski sürümlerinde çalışır ancak framework seçicisini seçilir.

  ![Visual Studio'da yeni proje iletişim kutusu](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  SignalR sunucu tarafı kodu konak kitaplıkları proje şablonu dahil edilir. İstemci tarafı JavaScript ile ayrı olarak yüklemeniz [npm](https://www.npmjs.com/).

  ```console
   npm install @aspnet/signalr
  ```

3. Kopya *signalr.js* gelen *node_modules\\ @aspnet\signalr\dist\browser*  için *wwwroot\lib* projenizdeki klasöre.

## <a name="create-the-signalr-hub"></a>SignalR hub'ı Oluştur

Bir hub istemci ve sunucu birbirine yöntemlerini çağırmaya izin veren üst düzey bir işlem hattı görevi gören bir sınıftır.

1. Bir sınıf projeye seçerek eklemek **dosya** > **yeni** > **dosya** ve seçerek **Visual C# sınıfı**. 

1. Devralınan `Microsoft.AspNetCore.SignalR.Hub`. `Hub` Özellikleri ve olayları gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.

1. Oluşturma `Send` tüm bağlı sohbet istemcilere bir ileti gönderir yöntemi. Döndürdüğü fark bir `Task`, SignalR zaman uyumsuz olarak çağrılır. Zaman uyumsuz kodu daha iyi ölçeklenir.

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>SignalR kullanmak üzere proje yapılandırma

SignalR için isteklerini iletmek için bilir böylece SignalR sunucusunun yapılandırılması gerekir.

1. Bir SignalR projesi yapılandırmak için değiştirin `ConfigureServices` yöntemi uygulamanın `Startup` yapılan bir çağrı ekleyerek sınıfı `services.AddSignalR`.

  `services.AddSignalR` SignalR parçası olarak ekler [ASP.NET Core Ara](xref:fundamentals/middleware/index) ardışık düzen.

1. Yollar kullanılarak, hub'larınız için yapılandırma `UseSignalR`.

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>SignalR istemci kodu oluşturma

1. İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  Önceki HTML adını ve ileti alanları ve bir gönderme düğmesi görüntüler. Komut dosyası başvuruları altındaki dikkat edin: SignalR başvurusu ve *chat.js*.

1. Bir JavaScript dosyası ekleme *wwwroot\js* adlı klasörü *chat.js* ve aşağıdaki kodu ekleyin:

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>Uygulama Çalıştırma

1. Seçin **hata ayıklama** > **Başlat hata ayıklama olmadan** bir tarayıcıyı başlatın ve Web sitesi yerel olarak yüklemek için. Adres çubuğundan URL'yi kopyalayın.

1. Başka bir tarayıcı örneğini (herhangi bir tarayıcıda) açın ve URL'yi adres çubuğuna yapıştırın.

1. Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklatın **Gönder** düğmesi. Ad ve ileti iki sayfalarında anında görüntülenir.

  ![Çözüm](get-started-signalr-core/_static/signalr-get-started-finished.png)
