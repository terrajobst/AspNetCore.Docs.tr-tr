---
title: ASP.NET Core .NET Core gRPC istemcisi ve sunucusu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core bir gRPC hizmeti ve gRPC istemcisinin nasıl oluşturulacağı gösterilmektedir. GRPC hizmeti projesi oluşturmayı, Proto dosyasını düzenlemeyi ve çift yönlü akış araması eklemeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: b56195f3da4ff95f292852902c6e33a40e96b582
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862984"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="b347d-104">Öğretici: ASP.NET Core bir gRPC istemcisi ve sunucusu oluşturma</span><span class="sxs-lookup"><span data-stu-id="b347d-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="b347d-105">[John Luo](https://github.com/juntaoluo) tarafından</span><span class="sxs-lookup"><span data-stu-id="b347d-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="b347d-106">Bu öğreticide, bir .NET Core [GRPC](https://grpc.io/docs/guides/) istemcisinin ve bir ASP.NET Core GRPC sunucusunun nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="b347d-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="b347d-107">Sonda, gRPC Greeter hizmeti ile iletişim kuran bir gRPC istemcisine sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="b347d-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="b347d-108">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="b347d-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="b347d-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="b347d-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b347d-110">GRPC sunucusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b347d-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="b347d-111">GRPC istemcisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b347d-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="b347d-112">GRPC istemci hizmetini gRPC Greeter hizmeti ile test edin.</span><span class="sxs-lookup"><span data-stu-id="b347d-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b347d-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b347d-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b347d-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b347d-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b347d-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="b347d-117">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="b347d-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b347d-119">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="b347d-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b347d-120">**Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="b347d-121">**İleri** Seç</span><span class="sxs-lookup"><span data-stu-id="b347d-121">Select **Next**</span></span>
* <span data-ttu-id="b347d-122">Projeyi **Grpcgreeter**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="b347d-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="b347d-123">Kodu kopyalayıp yapıştırdığınızda ad alanlarının eşleşmesi için, proje *Grpcgreeter* adında bir ad vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="b347d-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="b347d-124">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="b347d-124">Select **Create**</span></span>
* <span data-ttu-id="b347d-125">**Yeni ASP.NET Core Web uygulaması oluştur** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="b347d-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="b347d-126">Açılan menülerde **.NET Core** ve **ASP.NET Core 3,0** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="b347d-127">**GRPC hizmeti** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="b347d-128">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="b347d-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b347d-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b347d-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b347d-130">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b347d-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b347d-131">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b347d-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b347d-132">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b347d-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="b347d-133">Komut grpcgreeter klasöründe yeni bir GRPC hizmeti oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="b347d-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="b347d-134">Komut, Visual Studio Code yeni bir örneğinde *grpcgreeter* klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="b347d-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="b347d-135">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' grpcgreeter ' içinde eksik bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="b347d-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="b347d-136">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="b347d-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b347d-137">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b347d-138">Terminalden aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b347d-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="b347d-139">Yukarıdaki komutlar, gRPC hizmeti oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="b347d-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="b347d-140">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="b347d-140">Open the project</span></span>

<span data-ttu-id="b347d-141">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *grpcgreeter. sln* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="b347d-142">Hizmeti çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b347d-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b347d-144">Hata `Ctrl+F5` ayıklayıcı olmadan GRPC hizmetini çalıştırmak için tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b347d-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="b347d-145">Visual Studio, hizmeti bir komut isteminde çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="b347d-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b347d-146">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b347d-147">İle`dotnet run`komut satırından GRPC Greeter projesi *grpcgreeter* öğesini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b347d-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="b347d-148">Günlükler hizmeti dinlediği `https://localhost:5001`hizmetini gösterir.</span><span class="sxs-lookup"><span data-stu-id="b347d-148">The logs show the service listening on `https://localhost:5001`.</span></span>

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
> <span data-ttu-id="b347d-149">GRPC şablonu, [Aktarım Katmanı Güvenliği (TLS)](https://tools.ietf.org/html/rfc5246)kullanmak üzere yapılandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="b347d-149">The gRPC template is configured to use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="b347d-150">gRPC istemcilerinin, sunucuyu çağırmak için HTTPS kullanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="b347d-150">gRPC clients need to use HTTPS to call the server.</span></span>
>
> <span data-ttu-id="b347d-151">macOS, TLS ile ASP.NET Core gRPC 'yi desteklemez.</span><span class="sxs-lookup"><span data-stu-id="b347d-151">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="b347d-152">MacOS 'ta gRPC hizmetlerini başarıyla çalıştırmak için ek yapılandırma gerekir.</span><span class="sxs-lookup"><span data-stu-id="b347d-152">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="b347d-153">Daha fazla bilgi için bkz. [macOS 'Ta GRPC ve ASP.NET Core](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span><span class="sxs-lookup"><span data-stu-id="b347d-153">For more information, see [gRPC and ASP.NET Core on macOS](xref:grpc/aspnetcore#grpc-and-aspnet-core-on-macos).</span></span>

### <a name="examine-the-project-files"></a><span data-ttu-id="b347d-154">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="b347d-154">Examine the project files</span></span>

<span data-ttu-id="b347d-155">*Grpcgreeter* proje dosyaları:</span><span class="sxs-lookup"><span data-stu-id="b347d-155">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="b347d-156">*Greet. proto*: *Prototips/Greet. proto* dosyası `Greeter` GRPC 'yi tanımlar ve GRPC sunucu varlıklarını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b347d-156">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="b347d-157">Daha fazla bilgi için bkz. [gRPC 'ye giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="b347d-157">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="b347d-158">*Hizmetler* klasörü: `Greeter` Hizmetin uygulamasını içerir.</span><span class="sxs-lookup"><span data-stu-id="b347d-158">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="b347d-159">*appSettings. JSON*: Kestrel tarafından kullanılan protokol gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="b347d-159">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="b347d-160">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="b347d-160">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="b347d-161">*Program.cs*: GRPC hizmeti için giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="b347d-161">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="b347d-162">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="b347d-162">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="b347d-163">*Startup.cs*: Uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="b347d-163">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="b347d-164">Daha fazla bilgi için bkz. [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="b347d-164">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="b347d-165">Bir .NET konsol uygulamasında gRPC istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="b347d-165">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-166">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-166">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b347d-167">Visual Studio 'nun ikinci bir örneğini açın.</span><span class="sxs-lookup"><span data-stu-id="b347d-167">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="b347d-168">Menü çubuğundan **Dosya** > **Yeni** > **Proje** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-168">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="b347d-169">**Yeni proje oluştur** Iletişim kutusunda **konsol uygulaması (.NET Core)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="b347d-169">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="b347d-170">**İleri** Seç</span><span class="sxs-lookup"><span data-stu-id="b347d-170">Select **Next**</span></span>
* <span data-ttu-id="b347d-171">**Ad** metin kutusuna "GrpcGreeterClient" yazın.</span><span class="sxs-lookup"><span data-stu-id="b347d-171">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="b347d-172">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-172">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b347d-173">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b347d-173">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b347d-174">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="b347d-174">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="b347d-175">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="b347d-175">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="b347d-176">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b347d-176">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b347d-177">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-177">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="b347d-178">*Grpcgreeterclient*adlı bir konsol uygulaması oluşturmak için [buradaki](/dotnet/core/tutorials/using-on-mac-vs-full-solution) yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="b347d-178">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="b347d-179">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="b347d-179">Add required packages</span></span>

<span data-ttu-id="b347d-180">GRPC istemci projesi aşağıdaki paketleri gerektirir:</span><span class="sxs-lookup"><span data-stu-id="b347d-180">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="b347d-181">.NET Core istemcisini içeren [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client).</span><span class="sxs-lookup"><span data-stu-id="b347d-181">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="b347d-182">İçin C#prototipsiz Ileti API 'Leri içeren [Google. protoarabellek](https://www.nuget.org/packages/Google.Protobuf/).</span><span class="sxs-lookup"><span data-stu-id="b347d-182">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="b347d-183">Prototipleme dosyaları için araç desteğini C# Içeren [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="b347d-183">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="b347d-184">Araç çalışma zamanında gerekli değildir, bu nedenle bağımlılık ile `PrivateAssets="All"`işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="b347d-184">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-185">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-185">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b347d-186">Paket Yöneticisi Konsolu (PMC) veya NuGet Paketlerini Yönet ' i kullanarak paketleri yükler.</span><span class="sxs-lookup"><span data-stu-id="b347d-186">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="b347d-187">Paket yüklemek için PMC seçeneği</span><span class="sxs-lookup"><span data-stu-id="b347d-187">PMC option to install packages</span></span>

* <span data-ttu-id="b347d-188">Visual Studio 'da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-188">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="b347d-189">**Paket Yöneticisi konsolu** penceresinde, *Grpcgreeterclient. csproj* dosyasının bulunduğu dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="b347d-189">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="b347d-190">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b347d-190">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="b347d-191">Paket yüklemek için NuGet Paketlerini Yönet seçeneği</span><span class="sxs-lookup"><span data-stu-id="b347d-191">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="b347d-192">**Çözüm Gezgini** > **NuGet Paketlerini Yönet** ' de projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="b347d-192">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="b347d-193">**Tarayıcı** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-193">Select the **Browse** tab.</span></span>
* <span data-ttu-id="b347d-194">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="b347d-194">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="b347d-195">**Araştır** sekmesinden **GRPC .net. Client** paketini seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-195">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="b347d-196">`Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="b347d-196">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b347d-197">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b347d-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="b347d-198">**Tümleşik terminalden**aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b347d-198">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b347d-199">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-199">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b347d-200">**Çözüm bölmesi** paket > **Ekle** ' de paketler klasörüne sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="b347d-200">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="b347d-201">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="b347d-201">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="b347d-202">Sonuçlar bölmesinden **GRPC .net. Client** paketini seçin ve **paket Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-202">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="b347d-203">`Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="b347d-203">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="b347d-204">Greet. proto Ekle</span><span class="sxs-lookup"><span data-stu-id="b347d-204">Add greet.proto</span></span>

* <span data-ttu-id="b347d-205">GRPC istemci projesinde bir **Prototips** klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b347d-205">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="b347d-206">**Protos\bilgisem. proto** dosyasını GRPC Greeter hizmetinden GRPC istemci projesine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="b347d-206">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="b347d-207">*Grpcgreeterclient. csproj* proje dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="b347d-207">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-208">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="b347d-209">Projeye sağ tıklayın ve **Proje dosyasını Düzenle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-209">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="b347d-210">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b347d-210">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="b347d-211">*Grpcgreeterclient. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-211">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="b347d-212">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-212">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="b347d-213">Projeye sağ tıklayın ve **dosyayı düzenle > araçlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="b347d-213">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="b347d-214">**Greet. proto** dosyasına başvuran `<Protobuf>` bir öğesi olan bir öğe grubu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="b347d-214">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="b347d-215">Greeter istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="b347d-215">Create the Greeter client</span></span>

<span data-ttu-id="b347d-216">`GrpcGreeter` Ad alanındaki türleri oluşturmak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="b347d-216">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="b347d-217">`GrpcGreeter` Türler yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b347d-217">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="b347d-218">GRPC Client *program.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b347d-218">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="b347d-219">*Program.cs* , GRPC istemcisinin giriş noktasını ve mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="b347d-219">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="b347d-220">Greeter istemcisi şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="b347d-220">The Greeter client is created by:</span></span>

* <span data-ttu-id="b347d-221">GRPC `HttpClient` hizmetine bağlantı oluşturmak için bilgileri içeren bir örneği oluşturma.</span><span class="sxs-lookup"><span data-stu-id="b347d-221">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="b347d-222">Greeter istemcisini oluşturmak için kullanarak: `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="b347d-222">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="b347d-223">Greeter istemcisi zaman uyumsuz `SayHello` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="b347d-223">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="b347d-224">`SayHello` Çağrının sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="b347d-224">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="b347d-225">GRPC istemcisini gRPC Greeter hizmeti ile test etme</span><span class="sxs-lookup"><span data-stu-id="b347d-225">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b347d-226">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-226">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b347d-227">Greeter hizmetinde, hata ayıklayıcı olmadan `Ctrl+F5` sunucuyu başlatmak için tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="b347d-227">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="b347d-228">Projede, hata ayıklayıcı olmadan istemcisini başlatmak için tuşuna basın `Ctrl+F5`. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="b347d-228">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="b347d-229">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b347d-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="b347d-230">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="b347d-230">Start the Greeter service.</span></span>
* <span data-ttu-id="b347d-231">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="b347d-231">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="b347d-232">İstemci, "GreeterClient" adını içeren bir iletiyle hizmete bir tebrik gönderir.</span><span class="sxs-lookup"><span data-stu-id="b347d-232">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="b347d-233">Hizmet, "Hello GreeterClient" iletisini yanıt olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="b347d-233">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="b347d-234">Komut isteminde "Hello GreeterClient" yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="b347d-234">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="b347d-235">GRPC hizmeti, komut istemine yazılan günlüklerde başarılı çağrının ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="b347d-235">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="b347d-236">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="b347d-236">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
