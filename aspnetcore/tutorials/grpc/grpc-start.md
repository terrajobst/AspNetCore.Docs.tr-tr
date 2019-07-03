---
title: ASP.NET Core .NET Core gRPC istemci ve sunucu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core, hizmet ve gRPC gRPC istemci oluşturma işlemi gösterilmektedir. GRPC hizmeti projesi oluşturmak, proto dosyasını düzenleyin ve bir çift yönlü çağrı akış ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 8507c3dcefeb61a4dd34a1ff967c082d287cf646
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555896"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="6660d-104">Öğretici: ASP.NET Core gRPC istemci ve sunucu oluşturma</span><span class="sxs-lookup"><span data-stu-id="6660d-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="6660d-105">Tarafından [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="6660d-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="6660d-106">Bu öğreticide, .NET Core oluşturma işlemi gösterilmektedir [gRPC](https://grpc.io/docs/guides/) istemci ve sunucu bir ASP.NET Core gRPC.</span><span class="sxs-lookup"><span data-stu-id="6660d-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="6660d-107">Sonunda, gRPC Greeter hizmet ile iletişim kuran gRPC istemci gerekir.</span><span class="sxs-lookup"><span data-stu-id="6660d-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="6660d-108">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="6660d-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="6660d-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="6660d-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6660d-110">GRPC bir sunucu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6660d-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="6660d-111">GRPC istemci oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6660d-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="6660d-112">GRPC Greeter hizmeti ile gRPC İstemci hizmetini test edin.</span><span class="sxs-lookup"><span data-stu-id="6660d-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6660d-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6660d-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6660d-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6660d-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6660d-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="6660d-117">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="6660d-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6660d-119">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="6660d-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="6660d-120">İçinde **yeni bir proje oluşturma** iletişim kutusunda **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="6660d-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="6660d-121">Seçin **İleri**</span><span class="sxs-lookup"><span data-stu-id="6660d-121">Select **Next**</span></span>
* <span data-ttu-id="6660d-122">Projeyi adlandırın **GrpcGreeter**.</span><span class="sxs-lookup"><span data-stu-id="6660d-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="6660d-123">Projeyi adlandırın önemlidir *GrpcGreeter* kodu kopyalayıp, ad alanlarını eşleşecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="6660d-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="6660d-124">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="6660d-124">Select **Create**</span></span>
* <span data-ttu-id="6660d-125">İçinde **yeni bir ASP.NET Core Web uygulaması oluşturma** iletişim:</span><span class="sxs-lookup"><span data-stu-id="6660d-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="6660d-126">Seçin **.NET Core** ve **ASP.NET Core 3.0** açılan menüler içinde.</span><span class="sxs-lookup"><span data-stu-id="6660d-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="6660d-127">Seçin **gRPC hizmet** şablonu.</span><span class="sxs-lookup"><span data-stu-id="6660d-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="6660d-128">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="6660d-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6660d-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6660d-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6660d-130">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6660d-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6660d-131">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="6660d-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="6660d-132">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6660d-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="6660d-133">`dotnet new` Komut yeni bir gRPC hizmetinde oluşturur *GrpcGreeter* klasör.</span><span class="sxs-lookup"><span data-stu-id="6660d-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="6660d-134">`code` Komutu açılır *GrpcGreeter* Visual Studio Code yeni bir örneğini klasöründe.</span><span class="sxs-lookup"><span data-stu-id="6660d-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="6660d-135">Bir iletişim kutusu görünür **gerekli varlıkları oluşturun ve hata ayıklama 'GrpcGreeter' eksik. Bunları eklensin mi?**</span><span class="sxs-lookup"><span data-stu-id="6660d-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="6660d-136">Seçin **Evet**</span><span class="sxs-lookup"><span data-stu-id="6660d-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6660d-137">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6660d-138">Bir terminalde aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6660d-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="6660d-139">Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) gRPC hizmet oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="6660d-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="6660d-140">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="6660d-140">Open the project</span></span>

<span data-ttu-id="6660d-141">Visual Studio'dan seçin **Dosya > Aç**ve ardından *GrpcGreeter.sln* dosya.</span><span class="sxs-lookup"><span data-stu-id="6660d-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="6660d-142">Hizmet çalıştırma</span><span class="sxs-lookup"><span data-stu-id="6660d-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6660d-144">Tuşuna `Ctrl+F5` hata ayıklayıcı olmadan gRPC hizmeti çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="6660d-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="6660d-145">Visual Studio hizmeti, bir komut istemi'nde çalışır.</span><span class="sxs-lookup"><span data-stu-id="6660d-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6660d-146">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="6660d-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6660d-147">GRPC Greeter projeyi Çalıştır *GrpcGreeter* kullanarak komut satırından `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6660d-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="6660d-148">Üzerinde dinleme hizmeti günlükleri göster `http://localhost:50051`.</span><span class="sxs-lookup"><span data-stu-id="6660d-148">The logs show the service listening on `http://localhost:50051`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="6660d-149">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="6660d-149">Examine the project files</span></span>

<span data-ttu-id="6660d-150">*GrpcGreeter* proje dosyaları:</span><span class="sxs-lookup"><span data-stu-id="6660d-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="6660d-151">*greet.proto*: *Protos/greet.proto* dosyası tanımlar `Greeter` gRPC ve gRPC sunucu varlıkları oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6660d-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="6660d-152">Daha fazla bilgi için [gRPC giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="6660d-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="6660d-153">*Hizmetleri* klasörü: Uygulamasını içerir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="6660d-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="6660d-154">*appSettings.json*: Kestrel'i tarafından kullanılan Protokolü gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="6660d-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="6660d-155">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="6660d-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="6660d-156">*Program.cs*: GRPC hizmeti için giriş noktası içerir.</span><span class="sxs-lookup"><span data-stu-id="6660d-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="6660d-157">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="6660d-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="6660d-158">*Startup.cs*: Uygulama davranışını yapılandıran kodunu içerir.</span><span class="sxs-lookup"><span data-stu-id="6660d-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="6660d-159">Daha fazla bilgi için [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="6660d-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="6660d-160">GRPC istemci bir .NET konsol uygulaması oluşturun</span><span class="sxs-lookup"><span data-stu-id="6660d-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6660d-162">Visual Studio ikinci bir örneğini açın.</span><span class="sxs-lookup"><span data-stu-id="6660d-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="6660d-163">Seçin **dosya** > **yeni** > **proje** menü çubuğundan.</span><span class="sxs-lookup"><span data-stu-id="6660d-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="6660d-164">İçinde **yeni bir proje oluşturma** iletişim kutusunda **konsol uygulaması (.NET Core)** .</span><span class="sxs-lookup"><span data-stu-id="6660d-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="6660d-165">Seçin **İleri**</span><span class="sxs-lookup"><span data-stu-id="6660d-165">Select **Next**</span></span>
* <span data-ttu-id="6660d-166">İçinde **adı** metin kutusunda, "GrpcGreeterClient" girin.</span><span class="sxs-lookup"><span data-stu-id="6660d-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="6660d-167">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="6660d-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6660d-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6660d-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6660d-169">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="6660d-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="6660d-170">Dizinleri (`cd`) proje içeren bir klasör.</span><span class="sxs-lookup"><span data-stu-id="6660d-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="6660d-171">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6660d-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6660d-172">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="6660d-173">Yönergeleri izleyerek [burada](/dotnet/core/tutorials/using-on-mac-vs-full-solution) adıyla bir konsol uygulaması oluşturmak için *GrpcGreeterClient*.</span><span class="sxs-lookup"><span data-stu-id="6660d-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="6660d-174">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="6660d-174">Add required packages</span></span>

<span data-ttu-id="6660d-175">GRPC istemci projesi, aşağıdaki paketler gereklidir:</span><span class="sxs-lookup"><span data-stu-id="6660d-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="6660d-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), .NET Core istemci içerir.</span><span class="sxs-lookup"><span data-stu-id="6660d-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="6660d-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), protobuf içeren API'leri için ileti C#.</span><span class="sxs-lookup"><span data-stu-id="6660d-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="6660d-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), içeren C# protobuf dosyaları için araç desteği.</span><span class="sxs-lookup"><span data-stu-id="6660d-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="6660d-179">Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="6660d-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="6660d-181">Paket Yöneticisi Konsolu (PMC) veya NuGet paketlerini Yönet'i kullanarak paketleri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6660d-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="6660d-182">PMC seçeneği paketleri yüklemek için</span><span class="sxs-lookup"><span data-stu-id="6660d-182">PMC option to install packages</span></span>

* <span data-ttu-id="6660d-183">Visual Studio'dan seçin **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**</span><span class="sxs-lookup"><span data-stu-id="6660d-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="6660d-184">Gelen **Paket Yöneticisi Konsolu** penceresinde olduğu dizine gidin *GrpcGreeterClient.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="6660d-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="6660d-185">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6660d-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="6660d-186">Paketleri yüklemek için seçeneği NuGet paketlerini Yönet</span><span class="sxs-lookup"><span data-stu-id="6660d-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="6660d-187">Projeye sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="6660d-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="6660d-188">Seçin **Gözat** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="6660d-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="6660d-189">Girin **Grpc.Core** arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="6660d-189">Enter **Grpc.Core** in the search box.</span></span>
* <span data-ttu-id="6660d-190">Seçin **Grpc.Core** gelen paket **Gözat** sekmenize **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="6660d-190">Select the **Grpc.Core** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="6660d-191">Yineleme için `Google.Protobuf` ve `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="6660d-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6660d-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6660d-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="6660d-193">Aşağıdaki komutları çalıştırın **tümleşik Terminalini**:</span><span class="sxs-lookup"><span data-stu-id="6660d-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6660d-194">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="6660d-195">Sağ **paketleri** klasöründe **çözüm bölmesi** > **paketleri Ekle**</span><span class="sxs-lookup"><span data-stu-id="6660d-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="6660d-196">Girin **Grpc.Net.Client** arama kutusuna.</span><span class="sxs-lookup"><span data-stu-id="6660d-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="6660d-197">Seçin **Grpc.Net.Client** seçin ve sonuçlar bölmesinden paketini **' paket Ekle**</span><span class="sxs-lookup"><span data-stu-id="6660d-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="6660d-198">Yineleme için `Google.Protobuf` ve `Grpc.Tools`.</span><span class="sxs-lookup"><span data-stu-id="6660d-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="6660d-199">Greet.proto ekleyin</span><span class="sxs-lookup"><span data-stu-id="6660d-199">Add greet.proto</span></span>

* <span data-ttu-id="6660d-200">Oluşturma bir **Protos** gRPC istemci proje klasöründe.</span><span class="sxs-lookup"><span data-stu-id="6660d-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="6660d-201">Kopyalama **Protos\greet.proto** Greeter hizmet gRPC dosyasına gRPC istemci projesi.</span><span class="sxs-lookup"><span data-stu-id="6660d-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="6660d-202">Düzen *GrpcGreeterClient.csproj* proje dosyası:</span><span class="sxs-lookup"><span data-stu-id="6660d-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="6660d-204">Projeye sağ tıklayıp **proje dosyası düzenleme**.</span><span class="sxs-lookup"><span data-stu-id="6660d-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="6660d-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6660d-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="6660d-206">Seçin *GrpcGreeterClient.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="6660d-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="6660d-207">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="6660d-208">Projeye sağ tıklayıp **Araçlar > Dosya Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="6660d-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="6660d-209">Bir öğe grubuna ekleme bir `<Protobuf>` başvurduğu öğesi **greet.proto** dosyası:</span><span class="sxs-lookup"><span data-stu-id="6660d-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="6660d-210">Greeter istemcisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6660d-210">Create the Greeter client</span></span>

<span data-ttu-id="6660d-211">Türlerini oluşturmak için projeyi derleyin `GrpcGreeter` ad alanı.</span><span class="sxs-lookup"><span data-stu-id="6660d-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="6660d-212">`GrpcGreeter` Türleri yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6660d-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="6660d-213">GRPC istemci güncelleştirmesi *Program.cs* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="6660d-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="6660d-214">*Program.cs* gRPC istemci için giriş noktası ve mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="6660d-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="6660d-215">Greeter istemci tarafından oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="6660d-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="6660d-216">Örnekleme bir `HttpClient` gRPC hizmeti bağlantısı oluşturma hakkında bilgi içeren.</span><span class="sxs-lookup"><span data-stu-id="6660d-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="6660d-217">Kullanarak `HttpClient` Greeter istemci oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="6660d-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-9)]

<span data-ttu-id="6660d-218">Greeter istemci zaman uyumsuz olarak çağırır `SayHello` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6660d-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="6660d-219">Sonucu `SayHello` çağrı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6660d-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=10-12)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="6660d-220">GRPC Greeter hizmet gRPC istemciyle test etme</span><span class="sxs-lookup"><span data-stu-id="6660d-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6660d-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6660d-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6660d-222">Greeter hizmetinde basın `Ctrl+F5` hata ayıklayıcı olmadan sunucu başlatma.</span><span class="sxs-lookup"><span data-stu-id="6660d-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="6660d-223">İçinde `GrpcGreeterClient` tuşuna basın, proje `Ctrl+F5` hata ayıklayıcı olmadan sunucu başlatma.</span><span class="sxs-lookup"><span data-stu-id="6660d-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the server without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="6660d-224">Visual Studio Code'u / Visual Studio Mac için</span><span class="sxs-lookup"><span data-stu-id="6660d-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="6660d-225">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="6660d-225">Start the Greeter service.</span></span>
* <span data-ttu-id="6660d-226">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="6660d-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="6660d-227">İstemci bir karşılama "GreeterClient" adını içeren bir iletiyle hizmetine gönderir.</span><span class="sxs-lookup"><span data-stu-id="6660d-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="6660d-228">Hizmet yanıt olarak "Hello GreeterClient" iletisi gönderir.</span><span class="sxs-lookup"><span data-stu-id="6660d-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="6660d-229">"Hello GreeterClient" yanıt komut isteminde görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="6660d-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="6660d-230">GRPC hizmeti, komut istemine yazılan günlüklerinde başarılı görüşmesinin ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="6660d-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:50051
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\GH\aspnet\docs\4\Docs\aspnetcore\tutorials\grpc\grpc-start\sample\GrpcGreeter
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST http://localhost:50051/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 78.32260000000001ms 200 application/grpc
```

### <a name="next-steps"></a><span data-ttu-id="6660d-231">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="6660d-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
