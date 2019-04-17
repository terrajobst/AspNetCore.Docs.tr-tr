---
title: "Öğretici: ASP.NET core'da gRPC kullanmaya başlama"
author: juntaoluo
description: Bu öğretici serisinde, ASP.NET Core, gRPC bir hizmet oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 2/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: a67050331cc8563b1baf5312b92910c1bbdfbd69
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672573"
---
# <a name="tutorial-get-started-with-grpc-service-in-aspnet-core"></a>Öğretici: ASP.NET Core’da gRPC hizmeti ile çalışmaya başlama

Tarafından [John Luo](https://github.com/juntaoluo)

Bu öğretici, ASP.NET Core, gRPC hizmeti oluşturmaya ilişkin temel bilgileri öğretir.

Sonunda, greetings yankılayan gRPC hizmet sahip olacaksınız.

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC hizmeti oluşturun.
> * GRPC hizmet çalıştırın.
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

### <a name="run-the-service"></a>Hizmet çalıştırma

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Hata Ayıklayıcı olmadan gRPC hizmeti çalıştırmak için CTRL + F5 tuşlarına basın.

  Visual Studio hizmeti, bir komut istemi'nde çalışır. Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.

  ![Yeni ASP.NET Core Web uygulaması](grpc-start/_static/server_start.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code'u / Visual Studio Mac için](#tab/visual-studio-code+visual-studio-mac)

* GRPC Greeter projeyi GrpcGreeter kullanılarak komut satırından çalıştırmak `dotnet run`. Hizmet üzerinde dinleme başlatıldı günlükleri göster `http://localhost:50051`.

```console
dbug: Grpc.AspNetCore.Server.Internal.GrpcServiceBinder[1]
      Added gRPC method 'SayHello' to service 'Greet.Greeter'. Method type: 'Unary', route pattern: '/Greet.Greeter/SayHello'.
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\gh\Docs\aspnetcore\tutorials\grpc\grpc-start\samples\GrpcGreeter
```

<!-- End of combined VS/Mac tabs -->

---

Sonraki öğreticiye Greeter hizmeti test etmek için gerekli bir gRPC istemci derleme işlemi gösterilecektir.

### <a name="examine-the-project-files-of-the-grpc-project"></a>GRPC projenin proje dosyalarını inceleyin

GrpcGreeter dosyaları:

* greet.proto: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır. Daha fazla bilgi için bkz. <xref:grpc/index>.
* *Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.
* *appSettings.json*: Kestrel tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.
* *Program.cs*: GRPC hizmeti için giriş noktası içerir. Daha fazla bilgi için bkz. <xref:fundamentals/host/web-host>.
* Startup.cs: Uygulama davranışını yapılandıran kodunu içerir. Daha fazla bilgi için bkz. <xref:fundamentals/startup>.

### <a name="test-the-service"></a>Hizmeti test etme

## <a name="additional-resources"></a>Ek kaynaklar

Bu öğreticide şunları yaptınız:

> [!div class="checklist"]
> * GRPC hizmet oluşturdunuz.
> * GRPC hizmet çalıştı.
> * Proje dosyalarını incelenir.

> [!div class="step-by-step"]
> [Sonraki: Bir .NET Core gRPC istemcisi oluşturma](xref:tutorials/grpc/grpc-client)