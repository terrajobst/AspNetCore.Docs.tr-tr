---
title: ASP.NET Core .NET Core gRPC istemcisi ve sunucusu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core bir gRPC hizmeti ve gRPC istemcisinin nasıl oluşturulacağı gösterilmektedir. GRPC hizmeti projesi oluşturmayı, Proto dosyasını düzenlemeyi ve çift yönlü akış araması eklemeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 8/26/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 8c0f57e69d4cdee5b5f5510d7db04991ed6df475
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250823"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="160b4-104">Öğretici: ASP.NET Core bir gRPC istemcisi ve sunucusu oluşturma</span><span class="sxs-lookup"><span data-stu-id="160b4-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="160b4-105">[John Luo](https://github.com/juntaoluo) tarafından</span><span class="sxs-lookup"><span data-stu-id="160b4-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="160b4-106">Bu öğreticide, bir .NET Core [GRPC](https://grpc.io/docs/guides/) istemcisinin ve bir ASP.NET Core GRPC sunucusunun nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="160b4-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="160b4-107">Sonda, gRPC Greeter hizmeti ile iletişim kuran bir gRPC istemcisine sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="160b4-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="160b4-108">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="160b4-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="160b4-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="160b4-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="160b4-110">GRPC sunucusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="160b4-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="160b4-111">GRPC istemcisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="160b4-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="160b4-112">GRPC istemci hizmetini gRPC Greeter hizmeti ile test edin.</span><span class="sxs-lookup"><span data-stu-id="160b4-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="160b4-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="160b4-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="160b4-117">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="160b4-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="160b4-119">Visual Studio 'Yu başlatın ve **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-119">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="160b4-120">Alternatif olarak, Visual Studio **Dosya** menüsünden **Yeni** > **Proje**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-120">Alternatively, from the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="160b4-121">**Yeni proje oluştur** Iletişim kutusunda **GRPC hizmeti** ' ni seçin ve **İleri**' yi seçin:</span><span class="sxs-lookup"><span data-stu-id="160b4-121">In the **Create a new project** dialog, select **gRPC Service** and select **Next**:</span></span>

  ![\* \* Yeni proje oluştur \* \* iletişim kutusu](~/tutorials/grpc/grpc-start/static/cnp.png)

* <span data-ttu-id="160b4-123">Projeyi **Grpcgreeter**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="160b4-123">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="160b4-124">Kodu kopyalayıp yapıştırdığınızda ad alanlarının eşleşmesi için, proje *Grpcgreeter* adında bir ad vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="160b4-124">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="160b4-125">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-125">Select **Create**.</span></span>
* <span data-ttu-id="160b4-126">**Yeni bir gRPC hizmeti oluştur** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="160b4-126">In the **Create a new gRPC service** dialog:</span></span>
  * <span data-ttu-id="160b4-127">**GRPC hizmeti** şablonu seçilidir.</span><span class="sxs-lookup"><span data-stu-id="160b4-127">The **gRPC Service** template is selected.</span></span>
  * <span data-ttu-id="160b4-128">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-128">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="160b4-130">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="160b4-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="160b4-131">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="160b4-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="160b4-132">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="160b4-132">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="160b4-133">Komut grpcgreeter klasöründe yeni bir GRPC hizmeti oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="160b4-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="160b4-134">Komut, Visual Studio Code yeni bir örneğinde *grpcgreeter* klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="160b4-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="160b4-135">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' grpcgreeter ' içinde eksik bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="160b4-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="160b4-136">**Evet**' i seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-136">Select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-137">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="160b4-138">Terminalden aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="160b4-138">From a terminal, run the following commands:</span></span>

```dotnetcli
dotnet new grpc -o GrpcGreeter
cd GrpcGreeter
```

<span data-ttu-id="160b4-139">Yukarıdaki komutlar, gRPC hizmeti oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="160b4-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="160b4-140">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="160b4-140">Open the project</span></span>

<span data-ttu-id="160b4-141">Visual Studio 'da **Dosya** > **Aç**' ı seçin ve ardından *grpcgreeter. sln* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-141">From Visual Studio, select **File** > **Open**, and then select the *GrpcGreeter.sln* file.</span></span>

---

### <a name="run-the-service"></a><span data-ttu-id="160b4-142">Hizmeti çalıştırın</span><span class="sxs-lookup"><span data-stu-id="160b4-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="160b4-144">Hata `Ctrl+F5` ayıklayıcı olmadan GRPC hizmetini çalıştırmak için tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="160b4-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="160b4-145">Visual Studio, hizmeti bir komut isteminde çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="160b4-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-146">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-146">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="160b4-147">İle`dotnet run`komut satırından GRPC Greeter projesi *grpcgreeter* öğesini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="160b4-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-148">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="160b4-149">İle`dotnet run`komut satırından GRPC Greeter projesi *grpcgreeter* öğesini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="160b4-149">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

---

<span data-ttu-id="160b4-150">Günlükler hizmeti dinlediği `https://localhost:5001`hizmetini gösterir.</span><span class="sxs-lookup"><span data-stu-id="160b4-150">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

> [!NOTE]
> <span data-ttu-id="160b4-151">GRPC şablonu, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)kullanmak üzere yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="160b4-151">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="160b4-152">gRPC istemcilerinin, sunucuyu çağırmak için HTTPS kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="160b4-152">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="160b4-153">macOS, TLS ile ASP.NET Core gRPC 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="160b4-153">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="160b4-154">MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="160b4-154">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="160b4-155">Daha fazla bilgi için bkz. [macOS üzerinde gRPC uygulaması ASP.NET Core başlatılamıyor](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="160b4-155">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="160b4-156">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="160b4-156">Examine the project files</span></span>

<span data-ttu-id="160b4-157">*Grpcgreeter* proje dosyaları:</span><span class="sxs-lookup"><span data-stu-id="160b4-157">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="160b4-158">*Greet. proto* &ndash; `Greeter` *prototips/Greet. proto* dosyası GRPC 'yi tanımlar ve GRPC sunucu varlıklarını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="160b4-158">*greet.proto* &ndash; The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="160b4-159">Daha fazla bilgi için bkz. [gRPC 'ye giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="160b4-159">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="160b4-160">*Hizmetler* klasörü: `Greeter` Hizmetin uygulamasını içerir.</span><span class="sxs-lookup"><span data-stu-id="160b4-160">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="160b4-161">*appSettings. JSON* &ndash; , Kestrel tarafından kullanılan protokol gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="160b4-161">*appSettings.json* &ndash; Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="160b4-162">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="160b4-162">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="160b4-163">Program.cs&ndash; , GRPC hizmeti için giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="160b4-163">*Program.cs* &ndash; Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="160b4-164">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="160b4-164">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="160b4-165">Startup.cs&ndash; , uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="160b4-165">*Startup.cs* &ndash; Contains code that configures app behavior.</span></span> <span data-ttu-id="160b4-166">Daha fazla bilgi için bkz. [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="160b4-166">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="160b4-167">Bir .NET konsol uygulamasında gRPC istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="160b4-167">Create the gRPC client in a .NET console app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-168">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="160b4-169">Visual Studio 'nun ikinci bir örneğini açın ve **Yeni proje oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-169">Open a second instance of Visual Studio and select **Create a new project**.</span></span>
* <span data-ttu-id="160b4-170">**Yeni proje oluştur** Iletişim kutusunda **konsol uygulaması (.NET Core)** öğesini seçin ve **İleri**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-170">In the **Create a new project** dialog, select **Console App (.NET Core)** and select **Next**.</span></span>
* <span data-ttu-id="160b4-171">**Ad** metin kutusuna **Grpcgreeterclient** girin ve **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-171">In the **Name** text box, enter **GrpcGreeterClient** and select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-172">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-172">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="160b4-173">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="160b4-173">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="160b4-174">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="160b4-174">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="160b4-175">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="160b4-175">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new console -o GrpcGreeterClient
  code -r GrpcGreeterClient
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-176">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-176">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="160b4-177">*Grpcgreeterclient*adlı bir konsol uygulaması oluşturmak için [Mac için Visual Studio kullanarak MacOS 'ta kapsamlı bir .NET Core çözümü oluşturma](/dotnet/core/tutorials/using-on-mac-vs-full-solution) konusundaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="160b4-177">Follow the instructions in [Building a complete .NET Core solution on macOS using Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

---

### <a name="add-required-packages"></a><span data-ttu-id="160b4-178">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="160b4-178">Add required packages</span></span>

<span data-ttu-id="160b4-179">GRPC istemci projesi aşağıdaki paketleri gerektirir:</span><span class="sxs-lookup"><span data-stu-id="160b4-179">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="160b4-180">.NET Core istemcisini içeren [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client).</span><span class="sxs-lookup"><span data-stu-id="160b4-180">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="160b4-181">İçin C#prototipsiz Ileti API 'Leri içeren [Google. protoarabellek](https://www.nuget.org/packages/Google.Protobuf/).</span><span class="sxs-lookup"><span data-stu-id="160b4-181">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="160b4-182">Prototipleme dosyaları için araç desteğini C# Içeren [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="160b4-182">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="160b4-183">Araç çalışma zamanında gerekli değildir, bu nedenle bağımlılık ile `PrivateAssets="All"`işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="160b4-183">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-184">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="160b4-185">Paket Yöneticisi Konsolu (PMC) veya NuGet Paketlerini Yönet ' i kullanarak paketleri yükler.</span><span class="sxs-lookup"><span data-stu-id="160b4-185">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="160b4-186">Paket yüklemek için PMC seçeneği</span><span class="sxs-lookup"><span data-stu-id="160b4-186">PMC option to install packages</span></span>

* <span data-ttu-id="160b4-187">Visual Studio 'da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-187">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="160b4-188">**Paket Yöneticisi konsol** penceresinde, dizini `cd GrpcGreeterClient` *grpcgreeterclient. csproj* dosyalarını içeren klasöre değiştirmek için komutunu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="160b4-188">From the **Package Manager Console** window, run `cd GrpcGreeterClient` to change directories to the folder containing the *GrpcGreeterClient.csproj* files.</span></span>
* <span data-ttu-id="160b4-189">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="160b4-189">Run the following commands:</span></span>

  ```powershell
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="160b4-190">Paket yüklemek için NuGet Paketlerini Yönet seçeneği</span><span class="sxs-lookup"><span data-stu-id="160b4-190">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="160b4-191">**Çözüm Gezgini** > **NuGet Paketlerini Yönet** ' de projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="160b4-191">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="160b4-192">**Tarayıcı** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-192">Select the **Browse** tab.</span></span>
* <span data-ttu-id="160b4-193">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="160b4-193">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="160b4-194">**Araştır** sekmesinden **GRPC .net. Client** paketini seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-194">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="160b4-195">`Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="160b4-195">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-196">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-196">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="160b4-197">**Tümleşik terminalden**aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="160b4-197">Run the following commands from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-198">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-198">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="160b4-199">**Çözüm bölmesi** paket > **Ekle** ' de paketler klasörüne sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="160b4-199">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="160b4-200">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="160b4-200">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="160b4-201">Sonuçlar bölmesinden **GRPC .net. Client** paketini seçin ve **paket Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-201">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="160b4-202">`Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="160b4-202">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="160b4-203">Greet. proto Ekle</span><span class="sxs-lookup"><span data-stu-id="160b4-203">Add greet.proto</span></span>

* <span data-ttu-id="160b4-204">GRPC istemci projesinde bir *Prototips* klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="160b4-204">Create a *Protos* folder in the gRPC client project.</span></span>
* <span data-ttu-id="160b4-205">*Protos\bilgisem. proto* dosyasını GRPC Greeter hizmetinden GRPC istemci projesine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="160b4-205">Copy the *Protos\greet.proto* file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="160b4-206">*Grpcgreeterclient. csproj* proje dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="160b4-206">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-207">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-207">Visual Studio</span></span>](#tab/visual-studio)

  <span data-ttu-id="160b4-208">Projeye sağ tıklayın ve **Proje dosyasını Düzenle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-208">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-209">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-209">Visual Studio Code</span></span>](#tab/visual-studio-code)

  <span data-ttu-id="160b4-210">*Grpcgreeterclient. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-210">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-211">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-211">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="160b4-212">Projeye sağ tıklayın ve **Araçlar** > **Dosya Düzenle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="160b4-212">Right-click the project and select **Tools** > **Edit File**.</span></span>

  ---

* <span data-ttu-id="160b4-213">*Greet. proto* dosyasına başvuran `<Protobuf>` bir öğesi olan bir öğe grubu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="160b4-213">Add an item group with a `<Protobuf>` element that refers to the *greet.proto* file:</span></span>

  ```xml
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="160b4-214">Greeter istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="160b4-214">Create the Greeter client</span></span>

<span data-ttu-id="160b4-215">`GrpcGreeter` Ad alanındaki türleri oluşturmak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="160b4-215">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="160b4-216">`GrpcGreeter` Türler yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="160b4-216">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="160b4-217">GRPC Client *program.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="160b4-217">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="160b4-218">*Program.cs* , GRPC istemcisinin giriş noktasını ve mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="160b4-218">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="160b4-219">Greeter istemcisi şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="160b4-219">The Greeter client is created by:</span></span>

* <span data-ttu-id="160b4-220">GRPC `HttpClient` hizmetine bağlantı oluşturmak için bilgileri içeren bir örneği oluşturma.</span><span class="sxs-lookup"><span data-stu-id="160b4-220">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="160b4-221">`HttpClient` ' Nı kullanarak GRPC kanalını ve Greeter istemcisini oluşturun:</span><span class="sxs-lookup"><span data-stu-id="160b4-221">Using the `HttpClient` to construct a gRPC channel and the Greeter client:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-5)]

<span data-ttu-id="160b4-222">Greeter istemcisi zaman uyumsuz `SayHello` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="160b4-222">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="160b4-223">`SayHello` Çağrının sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="160b4-223">The result of the `SayHello` call is displayed:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=6-8)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="160b4-224">GRPC istemcisini gRPC Greeter hizmeti ile test etme</span><span class="sxs-lookup"><span data-stu-id="160b4-224">Test the gRPC client with the gRPC Greeter service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="160b4-225">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-225">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="160b4-226">Greeter hizmetinde, hata ayıklayıcı olmadan `Ctrl+F5` sunucuyu başlatmak için tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="160b4-226">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="160b4-227">Projede, hata ayıklayıcı olmadan istemcisini başlatmak için tuşuna basın `Ctrl+F5`. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="160b4-227">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="160b4-228">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="160b4-228">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="160b4-229">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="160b4-229">Start the Greeter service.</span></span>
* <span data-ttu-id="160b4-230">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="160b4-230">Start the client.</span></span>


# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="160b4-231">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="160b4-231">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="160b4-232">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="160b4-232">Start the Greeter service.</span></span>
* <span data-ttu-id="160b4-233">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="160b4-233">Start the client.</span></span>

---

<span data-ttu-id="160b4-234">İstemci, adı *Greeterclient*olan bir iletiyle hizmete bir tebrik gönderir.</span><span class="sxs-lookup"><span data-stu-id="160b4-234">The client sends a greeting to the service with a message containing its name, *GreeterClient*.</span></span> <span data-ttu-id="160b4-235">Hizmet, "Hello GreeterClient" iletisini yanıt olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="160b4-235">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="160b4-236">Komut isteminde "Hello GreeterClient" yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="160b4-236">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="160b4-237">GRPC hizmeti, komut istemine yazılan günlüklerde başarılı çağrının ayrıntılarını kaydeder:</span><span class="sxs-lookup"><span data-stu-id="160b4-237">The gRPC service records the details of the successful call in the logs written to the command prompt:</span></span>

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

> [!NOTE]
> <span data-ttu-id="160b4-238">Bu makaledeki kod, gRPC hizmetini güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası gerektirir.</span><span class="sxs-lookup"><span data-stu-id="160b4-238">The code in this article requires the ASP.NET Core HTTPS development certificate to secure the gRPC service.</span></span> <span data-ttu-id="160b4-239">İstemci iletiyle `The remote certificate is invalid according to the validation procedure.`başarısız olursa, geliştirme sertifikası güvenilir değildir.</span><span class="sxs-lookup"><span data-stu-id="160b4-239">If the client fails with the message `The remote certificate is invalid according to the validation procedure.`, the development certificate is not trusted.</span></span> <span data-ttu-id="160b4-240">Bu sorunu gidermeye yönelik yönergeler için bkz. [Windows ve macOS 'ta ASP.NET Core https geliştirme sertifikasına güvenin](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="160b4-240">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

### <a name="next-steps"></a><span data-ttu-id="160b4-241">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="160b4-241">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
