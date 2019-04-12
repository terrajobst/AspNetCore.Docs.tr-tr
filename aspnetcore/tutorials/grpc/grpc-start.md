---
title: "Öğretici: ASP.NET core'da gRPC kullanmaya başlama"
author: juntaoluo
description: Bu öğretici serisinde, ASP.NET Core, gRPC bir hizmet oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 84c98ed341adc48d4ecbeda4305100c492ffb5e1
ms.sourcegitcommit: 9b7fcb4ce00a3a32e153a080ebfaae4ef417aafa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/12/2019
ms.locfileid: "59516241"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Öğretici: ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama

Tarafından [John Luo](https://github.com/juntaoluo)

Bu öğretici, ASP.NET Core, gRPC hizmeti oluşturmaya ilişkin temel bilgileri öğretir.

Sonunda, greetings yankılayan gRPC hizmet sahip olacaksınız.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC hizmeti oluşturun.
> * Hizmet çalıştırın.
> * Proje dosyalarını inceleyin.

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="create-a-grpc-service"></a>GRPC hizmeti oluşturma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.
* Yeni bir ASP.NET Core Web uygulaması oluşturun.
  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/np_3_0.1.png)
* Projeyi adlandırın **GrpcGreeter**. Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.
  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/np_3_0.2.png)
* Seçin **.NET Core** ve **ASP.NET Core 3.0** açılır. Seçin **gRPC hizmet** şablonu.

  Aşağıdaki başlangıç projesini oluşturulur:

  ![Çözüm Gezgini](grpc-start/_static/se3.0.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).
* Dizinleri (`cd`) proje içeren bir klasör.
* Aşağıdaki komutları çalıştırın:

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * `dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.
  * `code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.

  Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**
* Seçin **Evet**

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Mac için Visual Studio](#tab/visual-studio-mac)

Bir terminalde aşağıdaki komutları çalıştırın:

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.

### <a name="open-the-project"></a>Projeyi açın

Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.

<!-- End of VS tabs -->

---

### <a name="test-the-service"></a>Hizmeti test etme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Olun **GrpcGreeter.Server** başlangıç projesi olarak ayarlayın ve hata ayıklayıcı olmadan gRPC hizmeti çalıştırmak için Ctrl + F5 tuşlarına basın.

  Visual Studio hizmeti, bir komut istemi'nde çalışır. Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_start.png)

* Hizmet çalışmaya başladıktan sonra ayarlanmış **GrpcGreeter.Client** hata ayıklayıcı olmadan istemciyi çalıştırmak için başlangıç projesi ve Ctrl + F5 tuşuna basın.

  İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir. Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/client.png)

  Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_complete.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* Sunucu projesi GrpcGreeter.Server kullanılarak komut satırından çalıştırmak `dotnet run`. Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\example\GrpcGreeter\GrpcGreeter.Server
```

* İstemci projesi GrpcGreeter.Client kullanılarak ayrı bir komut satırından çalıştırmak `dotnet run`.

İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir. Hizmet, komut satırında görüntülenen yanıt olarak bir "Hello GreeterClient" ileti gönderir.

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

Hizmet, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\tp\GrpcGreeter\GrpcGreeter.Server
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Hosting.Internal.GenericWebHostService[2]
      Request finished in 107.46730000000001ms 200 application/grpc
```

<!-- End of combined VS/Mac tabs -->

---

### <a name="examine-the-project-files-of-the-grpc-project"></a>GRPC projenin proje dosyalarını inceleyin

GrpcGreeter.Server dosyaları:

* greet.proto: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır. Daha fazla bilgi için bkz. <xref:grpc/index>.
* *Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.
* *appSettings.json*: Kestrel tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.
* *Program.cs*: GRPC hizmeti için giriş noktası içerir. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.
* Startup.cs

Uygulama davranışını yapılandıran kodunu içerir. Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

gRPC istemci GrpcGreeter.Client dosyası:

*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC hizmet oluşturdunuz.
> * Hizmet ve istemci hizmeti test etmek kaldı.
> * Proje dosyalarını incelenir.
