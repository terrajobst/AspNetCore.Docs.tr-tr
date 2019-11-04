---
page_type: sample
description: Bu öğreticide, ASP.NET Core bir gRPC hizmeti ve gRPC istemcisinin nasıl oluşturulacağı gösterilmektedir.
languages:
- csharp
products:
- dotnet-core
- aspnet-core
- vs
urlFragment: create-grpc-client
ms.openlocfilehash: b9feb9eed62177358fffc0d7da582f625a431e32
ms.sourcegitcommit: 9e85c2562df5e108d7933635c830297f484bb775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73463049"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a>Visual Studio 'Yu kullanarak ASP.NET Core 3,0 ' de gRPC istemcisi ve sunucusu oluşturma

Bu öğreticide, bir .NET Core [GRPC](https://grpc.io/docs/guides/) istemcisinin ve bir ASP.NET Core GRPC sunucusunun nasıl oluşturulacağı gösterilmektedir.

Sonda, gRPC Greeter hizmeti ile iletişim kuran bir gRPC istemcisine sahip olacaksınız.

Bu öğreticide, siz;

* GRPC sunucusu oluşturun.
* GRPC istemcisi oluşturun.
* GRPC istemci hizmetini gRPC Greeter hizmeti ile test edin.

## <a name="create-a-grpc-service"></a>GRPC hizmeti oluşturma

* Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.
* **Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin.
* **İleri** Seç
* Projeyi **Grpcgreeter**olarak adlandırın. Kodu kopyalayıp yapıştırdığınızda ad alanlarının eşleşmesi için, proje *Grpcgreeter* adında bir ad vermek önemlidir.
* **Oluştur** ' u seçin
* **Yeni ASP.NET Core Web uygulaması oluştur** iletişim kutusunda:
  * Açılan menülerde **.NET Core** ve **ASP.NET Core 3,0** ' i seçin. 
  * **GRPC hizmeti** şablonunu seçin.
  * **Oluştur** ' u seçin

### <a name="run-the-service"></a>Hizmeti çalıştırın

* Hata ayıklayıcı olmadan gRPC hizmetini çalıştırmak için `Ctrl+F5` ' a basın.

  Visual Studio, hizmeti bir komut isteminde çalıştırır.

Günlükler `https://localhost:5001` ' da dinleme hizmetini gösterir.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

> [!NOTE]
> GRPC şablonu, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)kullanmak üzere yapılandırılmıştır. gRPC istemcilerinin, sunucuyu çağırmak için HTTPS kullanması gerekir.
>
> macOS, TLS ile ASP.NET Core gRPC 'yi desteklemez. MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir. Daha fazla bilgi için bkz. [macOS 'Ta GRPC ve ASP.NET Core](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).

### <a name="examine-the-project-files"></a>Proje dosyalarını inceleyin

*Grpcgreeter* proje dosyaları:

* *Greet. proto*: *prototips/Greet. proto* dosyası `Greeter` GRPC 'Yi tanımlar ve GRPC sunucu varlıklarını oluşturmak için kullanılır. Daha fazla bilgi için bkz. [gRPC 'ye giriş](xref:grpc/index).
* *Hizmetler* klasörü: `Greeter` hizmetinin uygulamasını içerir.
* *appSettings. JSON*: Kestrel tarafından kullanılan protokol gibi yapılandırma verilerini içerir. Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.
* *Program.cs*: GRPC hizmeti için giriş noktasını içerir. Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.
* *Startup.cs*: uygulama davranışını yapılandıran kodu içerir. Daha fazla bilgi için bkz. [uygulama başlatma](xref:fundamentals/startup).

## <a name="create-the-grpc-client-in-a-net-console-app"></a>Bir .NET konsol uygulamasında gRPC istemcisini oluşturma

* Visual Studio 'nun ikinci bir örneğini açın.
* Menü çubuğundan **dosya** > **Yeni** > **Proje** ' yi seçin.
* **Yeni proje oluştur** Iletişim kutusunda **konsol uygulaması (.NET Core)** seçeneğini belirleyin.
* **İleri** Seç
* **Ad** metin kutusuna "GrpcGreeterClient" yazın.
* **Oluştur**' u seçin.

### <a name="add-required-packages"></a>Gerekli paketleri Ekle

GRPC istemci projesi aşağıdaki paketleri gerektirir:

* .NET Core istemcisini içeren [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client).
* İçin C#prototipsiz Ileti API 'Leri içeren [Google. protoarabellek](https://www.nuget.org/packages/Google.Protobuf/).
* Prototipleme dosyaları için araç desteğini C# Içeren [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/). Alet oluşturma paketi çalışma zamanında gerekli değildir, bu nedenle bağımlılık `PrivateAssets="All"` olarak işaretlenir.

Paket Yöneticisi Konsolu (PMC) veya NuGet Paketlerini Yönet ' i kullanarak paketleri yükler.

#### <a name="pmc-option-to-install-packages"></a>Paket yüklemek için PMC seçeneği

* Visual Studio 'da **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin.
* **Paket Yöneticisi konsolu** penceresinde, *Grpcgreeterclient. csproj* dosyasının bulunduğu dizine gidin.
* Aşağıdaki komutları çalıştırın:

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a>Paket yüklemek için NuGet Paketlerini Yönet seçeneği

* **Çözüm Gezgini** >  ' de projeye sağ tıklayıp**NuGet Paketlerini Yönet**
* **Tarayıcı** sekmesini seçin.
* Arama kutusuna **GRPC .net. Client** girin.
* **Araştır** sekmesinden **GRPC .net. Client** paketini seçin ve ardından **Install**' ı seçin.
* `Google.Protobuf` ve `Grpc.Tools`için yineleyin.

### <a name="add-greetproto"></a>Greet. proto Ekle

* GRPC istemci projesinde bir **Prototips** klasörü oluşturun.
* **Protos\bilgisem. proto** dosyasını GRPC Greeter hizmetinden GRPC istemci projesine kopyalayın.
* *Grpcgreeterclient. csproj* proje dosyasını düzenleyin:

  Projeye sağ tıklayın ve **Proje dosyasını Düzenle**' yi seçin.

* **Greet. proto** dosyasına başvuran `<Protobuf>` öğesiyle bir öğe grubu ekleyin:

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a>Greeter istemcisini oluşturma

`GrpcGreeter` ad alanındaki türleri oluşturmak için projeyi derleyin. `GrpcGreeter` türleri yapı işlemi tarafından otomatik olarak oluşturulur.

GRPC Client *program.cs* dosyasını aşağıdaki kodla güncelleştirin:

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;
using GrpcGreeter;
using Grpc.Net.Client;

namespace GrpcGreeterClient
{
    class Program
    {
        static async Task Main(string[] args)
        {
            // The port number(5001) must match the port of the gRPC server.
            var channel = GrpcChannel.ForAddress("https://localhost:5001");
            var client = new Greeter.GreeterClient(channel);
            var reply = await client.SayHelloAsync(
                              new HelloRequest { Name = "GreeterClient" });
            Console.WriteLine("Greeting: " + reply.Message);
            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

*Program.cs* , GRPC istemcisinin giriş noktasını ve mantığını içerir.

Greeter istemcisi şu şekilde oluşturulur:

* GRPC hizmetine bağlantı oluşturma bilgilerini içeren bir `GrpcChannel` örnekleniyor.
* Greeter istemcisini oluşturmak için `GrpcChannel` kullanma.

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a>GRPC istemcisini gRPC Greeter hizmeti ile test etme

* Greeter hizmetinde, sunucuyu hata ayıklayıcı olmadan başlatmak için `Ctrl+F5` ' a basın.
* `GrpcGreeterClient` projede, hata ayıklayıcı olmadan istemcisini başlatmak için `Ctrl+F5` ' a basın.

İstemci, "GreeterClient" adını içeren bir iletiyle hizmete bir tebrik gönderir. Hizmet, "Hello GreeterClient" iletisini yanıt olarak gönderir. Komut isteminde "Hello GreeterClient" yanıtı görüntülenir:

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

GRPC hizmeti, komut istemine yazılan günlüklerde başarılı çağrının ayrıntılarını kaydeder.

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="docs-help--next-steps-for-grpc"></a>Docs yardım & gRPC için sonraki adımlar

* [ASP.NET Core gRPC 'ye giriş](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [ile gRPC HizmetleriC#](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [GRPC hizmetlerini C Core 'dan ASP.NET Core geçirme](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
