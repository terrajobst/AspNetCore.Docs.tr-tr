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
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660932"
---
# <a name="create-a-grpc-client-and-server-in-aspnet-core-30-using-visual-studio"></a><span data-ttu-id="07c57-102">Visual Studio 'Yu kullanarak ASP.NET Core 3,0 ' de gRPC istemcisi ve sunucusu oluşturma</span><span class="sxs-lookup"><span data-stu-id="07c57-102">Create a gRPC client and server in ASP.NET Core 3.0 using Visual Studio</span></span>

<span data-ttu-id="07c57-103">Bu öğreticide, bir .NET Core [GRPC](https://grpc.io/docs/guides/) istemcisinin ve bir ASP.NET Core GRPC sunucusunun nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="07c57-103">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="07c57-104">Sonda, gRPC Greeter hizmeti ile iletişim kuran bir gRPC istemcisine sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="07c57-104">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="07c57-105">Bu öğreticide, siz;</span><span class="sxs-lookup"><span data-stu-id="07c57-105">In this tutorial, you;</span></span>

* <span data-ttu-id="07c57-106">GRPC sunucusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="07c57-106">Create a gRPC Server.</span></span>
* <span data-ttu-id="07c57-107">GRPC istemcisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="07c57-107">Create a gRPC client.</span></span>
* <span data-ttu-id="07c57-108">GRPC istemci hizmetini gRPC Greeter hizmeti ile test edin.</span><span class="sxs-lookup"><span data-stu-id="07c57-108">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="create-a-grpc-service"></a><span data-ttu-id="07c57-109">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="07c57-109">Create a gRPC service</span></span>

* <span data-ttu-id="07c57-110">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-110">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="07c57-111">**Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-111">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="07c57-112">**İleri**’yi seçin</span><span class="sxs-lookup"><span data-stu-id="07c57-112">Select **Next**</span></span>
* <span data-ttu-id="07c57-113">Projeyi **Grpcgreeter**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="07c57-113">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="07c57-114">Kodu kopyalayıp yapıştırdığınızda ad alanlarının eşleşmesi için, proje *Grpcgreeter* adında bir ad vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="07c57-114">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="07c57-115">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="07c57-115">Select **Create**</span></span>
* <span data-ttu-id="07c57-116">**Yeni ASP.NET Core Web uygulaması oluştur** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="07c57-116">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="07c57-117">Açılan menülerde **.NET Core** ve **ASP.NET Core 3,0** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-117">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="07c57-118">**GRPC hizmeti** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-118">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="07c57-119">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="07c57-119">Select **Create**</span></span>

### <a name="run-the-service"></a><span data-ttu-id="07c57-120">Hizmeti çalıştırma</span><span class="sxs-lookup"><span data-stu-id="07c57-120">Run the service</span></span>

* <span data-ttu-id="07c57-121">Hata ayıklayıcı olmadan gRPC hizmetini çalıştırmak için `Ctrl+F5` tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="07c57-121">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="07c57-122">Visual Studio, hizmeti bir komut isteminde çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="07c57-122">Visual Studio runs the service in a command prompt.</span></span>

<span data-ttu-id="07c57-123">Günlükler, `https://localhost:5001`üzerinde dinleme hizmetini gösterir.</span><span class="sxs-lookup"><span data-stu-id="07c57-123">The logs show the service listening on `https://localhost:5001`.</span></span>

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
> <span data-ttu-id="07c57-124">GRPC şablonu, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)kullanmak üzere yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="07c57-124">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="07c57-125">gRPC istemcilerinin, sunucuyu çağırmak için HTTPS kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="07c57-125">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="07c57-126">macOS, TLS ile ASP.NET Core gRPC 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="07c57-126">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="07c57-127">MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="07c57-127">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="07c57-128">Daha fazla bilgi için bkz. [macOS 'Ta GRPC ve ASP.NET Core](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span><span class="sxs-lookup"><span data-stu-id="07c57-128">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="07c57-129">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="07c57-129">Examine the project files</span></span>

<span data-ttu-id="07c57-130">*Grpcgreeter* proje dosyaları:</span><span class="sxs-lookup"><span data-stu-id="07c57-130">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="07c57-131">*Greet. proto*: *prototips/Greet. proto* dosyası `Greeter` GRPC 'Yi tanımlar ve GRPC sunucu varlıklarını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="07c57-131">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="07c57-132">Daha fazla bilgi için bkz. [gRPC 'ye giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="07c57-132">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="07c57-133">*Hizmetler* klasörü: `Greeter` hizmetinin uygulamasını içerir.</span><span class="sxs-lookup"><span data-stu-id="07c57-133">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="07c57-134">*appSettings. JSON*: Kestrel tarafından kullanılan protokol gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="07c57-134">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="07c57-135">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="07c57-135">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="07c57-136">*Program.cs*: GRPC hizmeti için giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="07c57-136">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="07c57-137">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="07c57-137">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="07c57-138">*Startup.cs*: uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="07c57-138">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="07c57-139">Daha fazla bilgi için bkz. [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="07c57-139">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="07c57-140">Bir .NET konsol uygulamasında gRPC istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="07c57-140">Create the gRPC client in a .NET console app</span></span>

* <span data-ttu-id="07c57-141">Visual Studio 'nun ikinci bir örneğini açın.</span><span class="sxs-lookup"><span data-stu-id="07c57-141">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="07c57-142">Menü çubuğundan **dosya** > **Yeni** > **projesi** öğesini seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-142">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="07c57-143">**Yeni proje oluştur** Iletişim kutusunda **konsol uygulaması (.NET Core)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="07c57-143">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="07c57-144">**İleri**’yi seçin</span><span class="sxs-lookup"><span data-stu-id="07c57-144">Select **Next**</span></span>
* <span data-ttu-id="07c57-145">**Ad** metin kutusuna "GrpcGreeterClient" yazın.</span><span class="sxs-lookup"><span data-stu-id="07c57-145">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="07c57-146">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-146">Select **Create**.</span></span>

### <a name="add-required-packages"></a><span data-ttu-id="07c57-147">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="07c57-147">Add required packages</span></span>

<span data-ttu-id="07c57-148">GRPC istemci projesi aşağıdaki paketleri gerektirir:</span><span class="sxs-lookup"><span data-stu-id="07c57-148">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="07c57-149">.NET Core istemcisini içeren [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client).</span><span class="sxs-lookup"><span data-stu-id="07c57-149">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="07c57-150">İçin C#prototipsiz Ileti API 'Leri içeren [Google. protoarabellek](https://www.nuget.org/packages/Google.Protobuf/).</span><span class="sxs-lookup"><span data-stu-id="07c57-150">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="07c57-151">Prototipleme dosyaları için araç desteğini C# Içeren [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="07c57-151">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="07c57-152">Araç, çalışma zamanında gerekli değildir, bu nedenle bağımlılık `PrivateAssets="All"`olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="07c57-152">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

<span data-ttu-id="07c57-153">Paket Yöneticisi Konsolu (PMC) veya NuGet Paketlerini Yönet ' i kullanarak paketleri yükler.</span><span class="sxs-lookup"><span data-stu-id="07c57-153">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="07c57-154">Paket yüklemek için PMC seçeneği</span><span class="sxs-lookup"><span data-stu-id="07c57-154">PMC option to install packages</span></span>

* <span data-ttu-id="07c57-155">Visual Studio 'da **araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-155">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="07c57-156">**Paket Yöneticisi konsolu** penceresinde, *Grpcgreeterclient. csproj* dosyasının bulunduğu dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="07c57-156">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="07c57-157">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="07c57-157">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="07c57-158">Paket yüklemek için NuGet Paketlerini Yönet seçeneği</span><span class="sxs-lookup"><span data-stu-id="07c57-158">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="07c57-159">**NuGet paketlerini yönetmek** > **Çözüm Gezgini** ' de projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="07c57-159">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="07c57-160">**Gözat** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-160">Select the **Browse** tab.</span></span>
* <span data-ttu-id="07c57-161">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="07c57-161">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="07c57-162">**Araştır** sekmesinden **GRPC .net. Client** paketini seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-162">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="07c57-163">`Google.Protobuf` ve `Grpc.Tools`için yineleyin.</span><span class="sxs-lookup"><span data-stu-id="07c57-163">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="add-greetproto"></a><span data-ttu-id="07c57-164">Greet. proto Ekle</span><span class="sxs-lookup"><span data-stu-id="07c57-164">Add greet.proto</span></span>

* <span data-ttu-id="07c57-165">GRPC istemci projesinde bir **Prototips** klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="07c57-165">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="07c57-166">**Protos\bilgisem. proto** dosyasını GRPC Greeter hizmetinden GRPC istemci projesine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="07c57-166">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="07c57-167">*Grpcgreeterclient. csproj* proje dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="07c57-167">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  <span data-ttu-id="07c57-168">Projeye sağ tıklayın ve **Proje dosyasını Düzenle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="07c57-168">Right-click the project and select **Edit Project File**.</span></span>

* <span data-ttu-id="07c57-169">**Greet. proto** dosyasına başvuran `<Protobuf>` öğesi olan bir öğe grubu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="07c57-169">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="07c57-170">Greeter istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="07c57-170">Create the Greeter client</span></span>

<span data-ttu-id="07c57-171">`GrpcGreeter` ad alanındaki türleri oluşturmak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="07c57-171">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="07c57-172">`GrpcGreeter` türleri yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="07c57-172">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="07c57-173">GRPC Client *program.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="07c57-173">Update the gRPC client *Program.cs* file with the following code:</span></span>

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

<span data-ttu-id="07c57-174">*Program.cs* , GRPC istemcisinin giriş noktasını ve mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="07c57-174">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="07c57-175">Greeter istemcisi şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="07c57-175">The Greeter client is created by:</span></span>

* <span data-ttu-id="07c57-176">GRPC hizmetine bağlantı oluşturma bilgilerini içeren bir `GrpcChannel` örnekleniyor.</span><span class="sxs-lookup"><span data-stu-id="07c57-176">Instantiating a `GrpcChannel` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="07c57-177">Greeter istemcisini oluşturmak için `GrpcChannel` kullanma.</span><span class="sxs-lookup"><span data-stu-id="07c57-177">Using the `GrpcChannel` to construct the Greeter client.</span></span>

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="07c57-178">GRPC istemcisini gRPC Greeter hizmeti ile test etme</span><span class="sxs-lookup"><span data-stu-id="07c57-178">Test the gRPC client with the gRPC Greeter service</span></span>

* <span data-ttu-id="07c57-179">Greeter hizmetinde, hata ayıklayıcı olmadan sunucuyu başlatmak için `Ctrl+F5` ' a basın.</span><span class="sxs-lookup"><span data-stu-id="07c57-179">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="07c57-180">`GrpcGreeterClient` projede, hata ayıklayıcı olmadan istemcisini başlatmak için `Ctrl+F5` ' a basın.</span><span class="sxs-lookup"><span data-stu-id="07c57-180">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

<span data-ttu-id="07c57-181">İstemci, "GreeterClient" adını içeren bir iletiyle hizmete bir tebrik gönderir.</span><span class="sxs-lookup"><span data-stu-id="07c57-181">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="07c57-182">Hizmet, "Hello GreeterClient" iletisini yanıt olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="07c57-182">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="07c57-183">Komut isteminde "Hello GreeterClient" yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="07c57-183">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="07c57-184">GRPC hizmeti, komut istemine yazılan günlüklerde başarılı çağrının ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="07c57-184">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="docs-help--next-steps-for-grpc"></a><span data-ttu-id="07c57-185">Docs yardım & gRPC için sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="07c57-185">Docs help & next steps for gRPC</span></span>

* [<span data-ttu-id="07c57-186">ASP.NET Core gRPC 'ye giriş</span><span class="sxs-lookup"><span data-stu-id="07c57-186">Introduction to gRPC on ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/index?view=aspnetcore-3.0)
* [<span data-ttu-id="07c57-187">ile gRPC HizmetleriC#</span><span class="sxs-lookup"><span data-stu-id="07c57-187">gRPC services with C#</span></span>](https://docs.microsoft.com/aspnet/core/grpc/basics?view=aspnetcore-3.0)
* [<span data-ttu-id="07c57-188">GRPC hizmetlerini C Core 'dan ASP.NET Core geçirme</span><span class="sxs-lookup"><span data-stu-id="07c57-188">Migrating gRPC services from C-core to ASP.NET Core</span></span>](https://docs.microsoft.com/aspnet/core/grpc/migration?view=aspnetcore-3.0)
