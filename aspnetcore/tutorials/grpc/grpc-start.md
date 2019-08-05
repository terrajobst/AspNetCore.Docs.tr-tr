---
title: ASP.NET Core .NET Core gRPC istemcisi ve sunucusu oluşturma
author: juntaoluo
description: Bu öğreticide, ASP.NET Core bir gRPC hizmeti ve gRPC istemcisinin nasıl oluşturulacağı gösterilmektedir. GRPC hizmeti projesi oluşturmayı, Proto dosyasını düzenlemeyi ve çift yönlü akış araması eklemeyi öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 06/12/2019
uid: tutorials/grpc/grpc-start
ms.openlocfilehash: 6a3a7446a488ef54d99d6c7605980c18890b9ad0
ms.sourcegitcommit: 4fe3ae892f54dc540859bff78741a28c2daa9a38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2019
ms.locfileid: "68776636"
---
# <a name="tutorial-create-a-grpc-client-and-server-in-aspnet-core"></a><span data-ttu-id="59b44-104">Öğretici: ASP.NET Core bir gRPC istemcisi ve sunucusu oluşturma</span><span class="sxs-lookup"><span data-stu-id="59b44-104">Tutorial: Create a gRPC client and server in ASP.NET Core</span></span>

<span data-ttu-id="59b44-105">[John Luo](https://github.com/juntaoluo) tarafından</span><span class="sxs-lookup"><span data-stu-id="59b44-105">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="59b44-106">Bu öğreticide, bir .NET Core [GRPC](https://grpc.io/docs/guides/) istemcisinin ve bir ASP.NET Core GRPC sunucusunun nasıl oluşturulacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="59b44-106">This tutorial shows how to create a .NET Core [gRPC](https://grpc.io/docs/guides/) client and an ASP.NET Core gRPC Server.</span></span>

<span data-ttu-id="59b44-107">Sonda, gRPC Greeter hizmeti ile iletişim kuran bir gRPC istemcisine sahip olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="59b44-107">At the end, you'll have a gRPC client that communicates with the gRPC Greeter service.</span></span>

<span data-ttu-id="59b44-108">[Örnek kodu görüntüle veya indir](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([indirme](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="59b44-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="59b44-109">Bu öğreticide şunları yaptınız:</span><span class="sxs-lookup"><span data-stu-id="59b44-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="59b44-110">GRPC sunucusu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59b44-110">Create a gRPC Server.</span></span>
> * <span data-ttu-id="59b44-111">GRPC istemcisi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59b44-111">Create a gRPC client.</span></span>
> * <span data-ttu-id="59b44-112">GRPC istemci hizmetini gRPC Greeter hizmeti ile test edin.</span><span class="sxs-lookup"><span data-stu-id="59b44-112">Test the gRPC client service with the gRPC Greeter service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59b44-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="59b44-113">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-114">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="59b44-115">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="59b44-115">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="59b44-116">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-116">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-grpc-service"></a><span data-ttu-id="59b44-117">GRPC hizmeti oluşturma</span><span class="sxs-lookup"><span data-stu-id="59b44-117">Create a gRPC service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-118">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="59b44-119">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="59b44-119">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="59b44-120">**Yeni proje oluştur** iletişim kutusunda **ASP.NET Core Web uygulaması**' nı seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-120">In the **Create a new project** dialog, select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="59b44-121">**İleri** Seç</span><span class="sxs-lookup"><span data-stu-id="59b44-121">Select **Next**</span></span>
* <span data-ttu-id="59b44-122">Projeyi **Grpcgreeter**olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="59b44-122">Name the project **GrpcGreeter**.</span></span> <span data-ttu-id="59b44-123">Kodu kopyalayıp yapıştırdığınızda ad alanlarının eşleşmesi için, proje *Grpcgreeter* adında bir ad vermek önemlidir.</span><span class="sxs-lookup"><span data-stu-id="59b44-123">It's important to name the project *GrpcGreeter* so the namespaces will match when you copy and paste code.</span></span>
* <span data-ttu-id="59b44-124">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="59b44-124">Select **Create**</span></span>
* <span data-ttu-id="59b44-125">**Yeni ASP.NET Core Web uygulaması oluştur** iletişim kutusunda:</span><span class="sxs-lookup"><span data-stu-id="59b44-125">In the **Create a new ASP.NET Core Web Application** dialog:</span></span>
  * <span data-ttu-id="59b44-126">Açılan menülerde **.NET Core** ve **ASP.NET Core 3,0** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-126">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdown menus.</span></span> 
  * <span data-ttu-id="59b44-127">**GRPC hizmeti** şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-127">Select the **gRPC Service** template.</span></span>
  * <span data-ttu-id="59b44-128">**Oluştur**’u seçin</span><span class="sxs-lookup"><span data-stu-id="59b44-128">Select **Create**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="59b44-129">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="59b44-129">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="59b44-130">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="59b44-130">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="59b44-131">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="59b44-131">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="59b44-132">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59b44-132">Run the following commands:</span></span>

  ```console
  dotnet new grpc -o GrpcGreeter
  code -r GrpcGreeter
  ```

  * <span data-ttu-id="59b44-133">Komut grpcgreeter klasöründe yeni bir GRPC hizmeti oluşturur. `dotnet new`</span><span class="sxs-lookup"><span data-stu-id="59b44-133">The `dotnet new` command creates a new gRPC service in the *GrpcGreeter* folder.</span></span>
  * <span data-ttu-id="59b44-134">Komut, Visual Studio Code yeni bir örneğinde *grpcgreeter* klasörünü açar. `code`</span><span class="sxs-lookup"><span data-stu-id="59b44-134">The `code` command opens the *GrpcGreeter* folder in a new instance of Visual Studio Code.</span></span>

  <span data-ttu-id="59b44-135">**Gerekli varlıkların derlenmesi ve hata ayıklaması için ' grpcgreeter ' içinde eksik bir iletişim kutusu görüntülenir. Bunları ekleyin mi?**</span><span class="sxs-lookup"><span data-stu-id="59b44-135">A dialog box appears with **Required assets to build and debug are missing from 'GrpcGreeter'. Add them?**</span></span>
* <span data-ttu-id="59b44-136">**Evet** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="59b44-136">Select **Yes**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="59b44-137">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="59b44-138">Terminalden aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59b44-138">From a terminal, run the following commands:</span></span>

```console
  dotnet new grpc -o GrpcGreeter
  cd GrpcGreeter
```

<span data-ttu-id="59b44-139">Yukarıdaki komutlar, gRPC hizmeti oluşturmak için [.NET Core CLI](/dotnet/core/tools/dotnet) kullanır.</span><span class="sxs-lookup"><span data-stu-id="59b44-139">The preceding commands use the [.NET Core CLI](/dotnet/core/tools/dotnet) to create a gRPC service.</span></span>

### <a name="open-the-project"></a><span data-ttu-id="59b44-140">Projeyi açın</span><span class="sxs-lookup"><span data-stu-id="59b44-140">Open the project</span></span>

<span data-ttu-id="59b44-141">Visual Studio 'da **dosya > aç**' ı seçin ve ardından *grpcgreeter. sln* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-141">From Visual Studio, select **File > Open**, and then select the *GrpcGreeter.sln* file.</span></span>

<!-- End of VS tabs -->

---

### <a name="run-the-service"></a><span data-ttu-id="59b44-142">Hizmeti çalıştırın</span><span class="sxs-lookup"><span data-stu-id="59b44-142">Run the service</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-143">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="59b44-144">Hata `Ctrl+F5` ayıklayıcı olmadan GRPC hizmetini çalıştırmak için tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="59b44-144">Press `Ctrl+F5` to run the gRPC service without the debugger.</span></span>

  <span data-ttu-id="59b44-145">Visual Studio, hizmeti bir komut isteminde çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="59b44-145">Visual Studio runs the service in a command prompt.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="59b44-146">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-146">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="59b44-147">İle`dotnet run`komut satırından GRPC Greeter projesi *grpcgreeter* öğesini çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="59b44-147">Run the gRPC Greeter project *GrpcGreeter* from the command line using `dotnet run`.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="59b44-148">Günlükler hizmeti dinlediği `https://localhost:5001`hizmetini gösterir.</span><span class="sxs-lookup"><span data-stu-id="59b44-148">The logs show the service listening on `https://localhost:5001`.</span></span>

```console
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
```

### <a name="examine-the-project-files"></a><span data-ttu-id="59b44-149">Proje dosyalarını inceleyin</span><span class="sxs-lookup"><span data-stu-id="59b44-149">Examine the project files</span></span>

<span data-ttu-id="59b44-150">*Grpcgreeter* proje dosyaları:</span><span class="sxs-lookup"><span data-stu-id="59b44-150">*GrpcGreeter* project files:</span></span>

* <span data-ttu-id="59b44-151">*Greet. proto*: *Prototips/Greet. proto* dosyası `Greeter` GRPC 'yi tanımlar ve GRPC sunucu varlıklarını oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="59b44-151">*greet.proto*: The *Protos/greet.proto* file defines the `Greeter` gRPC and is used to generate the gRPC server assets.</span></span> <span data-ttu-id="59b44-152">Daha fazla bilgi için bkz. [gRPC 'ye giriş](xref:grpc/index).</span><span class="sxs-lookup"><span data-stu-id="59b44-152">For more information, see [Introduction to gRPC](xref:grpc/index).</span></span>
* <span data-ttu-id="59b44-153">*Hizmetler* klasörü: `Greeter` Hizmetin uygulamasını içerir.</span><span class="sxs-lookup"><span data-stu-id="59b44-153">*Services* folder: Contains the implementation of the `Greeter` service.</span></span>
* <span data-ttu-id="59b44-154">*appSettings. JSON*: Kestrel tarafından kullanılan protokol gibi yapılandırma verilerini içerir.</span><span class="sxs-lookup"><span data-stu-id="59b44-154">*appSettings.json*: Contains configuration data, such as protocol used by Kestrel.</span></span> <span data-ttu-id="59b44-155">Daha fazla bilgi için bkz. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="59b44-155">For more information, see <xref:fundamentals/configuration/index>.</span></span>
* <span data-ttu-id="59b44-156">*Program.cs*: GRPC hizmeti için giriş noktasını içerir.</span><span class="sxs-lookup"><span data-stu-id="59b44-156">*Program.cs*: Contains the entry point for the gRPC service.</span></span> <span data-ttu-id="59b44-157">Daha fazla bilgi için bkz. <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="59b44-157">For more information, see <xref:fundamentals/host/generic-host>.</span></span>
* <span data-ttu-id="59b44-158">*Startup.cs*: Uygulama davranışını yapılandıran kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="59b44-158">*Startup.cs*: Contains code that configures app behavior.</span></span> <span data-ttu-id="59b44-159">Daha fazla bilgi için bkz. [uygulama başlatma](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="59b44-159">For more information, see [App startup](xref:fundamentals/startup).</span></span>

## <a name="create-the-grpc-client-in-a-net-console-app"></a><span data-ttu-id="59b44-160">Bir .NET konsol uygulamasında gRPC istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="59b44-160">Create the gRPC client in a .NET console app</span></span>

## <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-161">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-161">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="59b44-162">Visual Studio 'nun ikinci bir örneğini açın.</span><span class="sxs-lookup"><span data-stu-id="59b44-162">Open a second instance of Visual Studio.</span></span>
* <span data-ttu-id="59b44-163">Menü çubuğundan **Dosya** > **Yeni** > **Proje** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-163">Select **File** > **New** > **Project** from the menu bar.</span></span>
* <span data-ttu-id="59b44-164">**Yeni proje oluştur** Iletişim kutusunda **konsol uygulaması (.NET Core)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="59b44-164">In the **Create a new project** dialog, select **Console App (.NET Core)**.</span></span>
* <span data-ttu-id="59b44-165">**İleri** Seç</span><span class="sxs-lookup"><span data-stu-id="59b44-165">Select **Next**</span></span>
* <span data-ttu-id="59b44-166">**Ad** metin kutusuna "GrpcGreeterClient" yazın.</span><span class="sxs-lookup"><span data-stu-id="59b44-166">In the **Name** text box, enter "GrpcGreeterClient".</span></span>
* <span data-ttu-id="59b44-167">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-167">Select **Create**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="59b44-168">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="59b44-168">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="59b44-169">Açık [tümleşik Terminalini](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="59b44-169">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="59b44-170">Dizinleri (`cd`), projeyi içerecek bir klasöre değiştirin.</span><span class="sxs-lookup"><span data-stu-id="59b44-170">Change directories (`cd`) to a folder which will contain the project.</span></span>
* <span data-ttu-id="59b44-171">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59b44-171">Run the following commands:</span></span>

```console
dotnet new console -o GrpcGreeterClient
code -r GrpcGreeterClient
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="59b44-172">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-172">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="59b44-173">*Grpcgreeterclient*adlı bir konsol uygulaması oluşturmak için [buradaki](/dotnet/core/tutorials/using-on-mac-vs-full-solution) yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="59b44-173">Follow the instructions [here](/dotnet/core/tutorials/using-on-mac-vs-full-solution) to create a console app with the name *GrpcGreeterClient*.</span></span>

<!-- End of VS tabs -->

---

### <a name="add-required-packages"></a><span data-ttu-id="59b44-174">Gerekli paketleri Ekle</span><span class="sxs-lookup"><span data-stu-id="59b44-174">Add required packages</span></span>

<span data-ttu-id="59b44-175">GRPC istemci projesi aşağıdaki paketleri gerektirir:</span><span class="sxs-lookup"><span data-stu-id="59b44-175">The gRPC client project requires the following packages:</span></span>

* <span data-ttu-id="59b44-176">.NET Core istemcisini içeren [GRPC .net. Client](https://www.nuget.org/packages/Grpc.Net.Client).</span><span class="sxs-lookup"><span data-stu-id="59b44-176">[Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client), which contains the .NET Core client.</span></span>
* <span data-ttu-id="59b44-177">İçin C#prototipsiz Ileti API 'Leri içeren [Google. protoarabellek](https://www.nuget.org/packages/Google.Protobuf/).</span><span class="sxs-lookup"><span data-stu-id="59b44-177">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/), which contains protobuf message APIs for C#.</span></span>
* <span data-ttu-id="59b44-178">Prototipleme dosyaları için araç desteğini C# Içeren [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="59b44-178">[Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/), which contains C# tooling support for protobuf files.</span></span> <span data-ttu-id="59b44-179">Araç çalışma zamanında gerekli değildir, bu nedenle bağımlılık ile `PrivateAssets="All"`işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="59b44-179">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-180">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-180">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="59b44-181">Paket Yöneticisi Konsolu (PMC) veya NuGet Paketlerini Yönet ' i kullanarak paketleri yükler.</span><span class="sxs-lookup"><span data-stu-id="59b44-181">Install the packages using either the Package Manager Console (PMC) or Manage NuGet Packages.</span></span>

#### <a name="pmc-option-to-install-packages"></a><span data-ttu-id="59b44-182">Paket yüklemek için PMC seçeneği</span><span class="sxs-lookup"><span data-stu-id="59b44-182">PMC option to install packages</span></span>

* <span data-ttu-id="59b44-183">Visual Studio 'da **Araçlar** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi konsolu** ' nu seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-183">From Visual Studio, select **Tools** > **NuGet Package Manager** > **Package Manager Console**</span></span>
* <span data-ttu-id="59b44-184">**Paket Yöneticisi konsolu** penceresinde, *Grpcgreeterclient. csproj* dosyasının bulunduğu dizine gidin.</span><span class="sxs-lookup"><span data-stu-id="59b44-184">From the **Package Manager Console** window, navigate to the directory in which the *GrpcGreeterClient.csproj* file exists.</span></span>
* <span data-ttu-id="59b44-185">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59b44-185">Run the following commands:</span></span>

 ```powershell
Install-Package Grpc.Net.Client
Install-Package Google.Protobuf
Install-Package Grpc.Tools
```

#### <a name="manage-nuget-packages-option-to-install-packages"></a><span data-ttu-id="59b44-186">Paket yüklemek için NuGet Paketlerini Yönet seçeneği</span><span class="sxs-lookup"><span data-stu-id="59b44-186">Manage NuGet Packages option to install packages</span></span>

* <span data-ttu-id="59b44-187">**Çözüm Gezgini** > **NuGet Paketlerini Yönet** ' de projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="59b44-187">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
* <span data-ttu-id="59b44-188">**Tarayıcı** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-188">Select the **Browse** tab.</span></span>
* <span data-ttu-id="59b44-189">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="59b44-189">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="59b44-190">**Araştır** sekmesinden **GRPC .net. Client** paketini seçin ve ardından **Install**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-190">Select the **Grpc.Net.Client** package from the **Browse** tab and select **Install**.</span></span>
* <span data-ttu-id="59b44-191">`Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="59b44-191">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="59b44-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="59b44-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="59b44-193">**Tümleşik terminalden**aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="59b44-193">Run the following commands from the **Integrated Terminal**:</span></span>

```console
dotnet add GrpcGreeterClient.csproj package Grpc.Net.Client
dotnet add GrpcGreeterClient.csproj package Google.Protobuf
dotnet add GrpcGreeterClient.csproj package Grpc.Tools
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="59b44-194">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="59b44-195">**Çözüm bölmesi** paket > **Ekle** ' de paketler klasörüne sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="59b44-195">Right-click the **Packages** folder in **Solution Pad** > **Add Packages**</span></span>
* <span data-ttu-id="59b44-196">Arama kutusuna **GRPC .net. Client** girin.</span><span class="sxs-lookup"><span data-stu-id="59b44-196">Enter **Grpc.Net.Client** in the search box.</span></span>
* <span data-ttu-id="59b44-197">Sonuçlar bölmesinden **GRPC .net. Client** paketini seçin ve **paket Ekle** ' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-197">Select the **Grpc.Net.Client** package from the results pane and select **Add Package**</span></span>
* <span data-ttu-id="59b44-198">`Google.Protobuf` Ve`Grpc.Tools`için tekrarlayın.</span><span class="sxs-lookup"><span data-stu-id="59b44-198">Repeat for `Google.Protobuf` and `Grpc.Tools`.</span></span>

---

### <a name="add-greetproto"></a><span data-ttu-id="59b44-199">Greet. proto Ekle</span><span class="sxs-lookup"><span data-stu-id="59b44-199">Add greet.proto</span></span>

* <span data-ttu-id="59b44-200">GRPC istemci projesinde bir **Prototips** klasörü oluşturun.</span><span class="sxs-lookup"><span data-stu-id="59b44-200">Create a **Protos** folder in the gRPC client project.</span></span>
* <span data-ttu-id="59b44-201">**Protos\bilgisem. proto** dosyasını GRPC Greeter hizmetinden GRPC istemci projesine kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="59b44-201">Copy the **Protos\greet.proto** file from the gRPC Greeter service to the gRPC client project.</span></span>
* <span data-ttu-id="59b44-202">*Grpcgreeterclient. csproj* proje dosyasını düzenleyin:</span><span class="sxs-lookup"><span data-stu-id="59b44-202">Edit the *GrpcGreeterClient.csproj* project file:</span></span>

  # <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-203">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-203">Visual Studio</span></span>](#tab/visual-studio) 

  <span data-ttu-id="59b44-204">Projeye sağ tıklayın ve **Proje dosyasını Düzenle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-204">Right-click the project and select **Edit Project File**.</span></span>

  # <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="59b44-205">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="59b44-205">Visual Studio Code</span></span>](#tab/visual-studio-code) 

  <span data-ttu-id="59b44-206">*Grpcgreeterclient. csproj* dosyasını seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-206">Select the *GrpcGreeterClient.csproj* file.</span></span>

  # <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="59b44-207">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-207">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  <span data-ttu-id="59b44-208">Projeye sağ tıklayın ve **dosyayı düzenle > araçlar**' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="59b44-208">Right-click the project and select **Tools > Edit File**.</span></span>

  ---

* <span data-ttu-id="59b44-209">**Greet. proto** dosyasına başvuran `<Protobuf>` bir öğesi olan bir öğe grubu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="59b44-209">Add an item group with a `<Protobuf>` element that refers to the **greet.proto** file:</span></span>

  ```XML
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```

### <a name="create-the-greeter-client"></a><span data-ttu-id="59b44-210">Greeter istemcisini oluşturma</span><span class="sxs-lookup"><span data-stu-id="59b44-210">Create the Greeter client</span></span>

<span data-ttu-id="59b44-211">`GrpcGreeter` Ad alanındaki türleri oluşturmak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="59b44-211">Build the project to create the types in the `GrpcGreeter` namespace.</span></span> <span data-ttu-id="59b44-212">`GrpcGreeter` Türler yapı işlemi tarafından otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="59b44-212">The `GrpcGreeter` types are generated automatically by the build process.</span></span>

<span data-ttu-id="59b44-213">GRPC Client *program.cs* dosyasını aşağıdaki kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="59b44-213">Update the gRPC client *Program.cs* file with the following code:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet2)]

<span data-ttu-id="59b44-214">*Program.cs* , GRPC istemcisinin giriş noktasını ve mantığını içerir.</span><span class="sxs-lookup"><span data-stu-id="59b44-214">*Program.cs* contains the entry point and logic for the gRPC client.</span></span>

<span data-ttu-id="59b44-215">Greeter istemcisi şu şekilde oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="59b44-215">The Greeter client is created by:</span></span>

* <span data-ttu-id="59b44-216">GRPC `HttpClient` hizmetine bağlantı oluşturmak için bilgileri içeren bir örneği oluşturma.</span><span class="sxs-lookup"><span data-stu-id="59b44-216">Instantiating an `HttpClient` containing the information for creating the connection to the gRPC service.</span></span>
* <span data-ttu-id="59b44-217">Greeter istemcisini oluşturmak için kullanarak: `HttpClient`</span><span class="sxs-lookup"><span data-stu-id="59b44-217">Using the `HttpClient` to construct the Greeter client:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="59b44-218">Greeter istemcisi zaman uyumsuz `SayHello` yöntemi çağırır.</span><span class="sxs-lookup"><span data-stu-id="59b44-218">The Greeter client calls the asynchronous `SayHello` method.</span></span> <span data-ttu-id="59b44-219">`SayHello` Çağrının sonucu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="59b44-219">The result of the `SayHello` call is displayed:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet&highlight=7-9)]

## <a name="test-the-grpc-client-with-the-grpc-greeter-service"></a><span data-ttu-id="59b44-220">GRPC istemcisini gRPC Greeter hizmeti ile test etme</span><span class="sxs-lookup"><span data-stu-id="59b44-220">Test the gRPC client with the gRPC Greeter service</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="59b44-221">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-221">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="59b44-222">Greeter hizmetinde, hata ayıklayıcı olmadan `Ctrl+F5` sunucuyu başlatmak için tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="59b44-222">In the Greeter service, press `Ctrl+F5` to start the server without the debugger.</span></span>
* <span data-ttu-id="59b44-223">Projede, hata ayıklayıcı olmadan istemcisini başlatmak için tuşuna basın `Ctrl+F5`. `GrpcGreeterClient`</span><span class="sxs-lookup"><span data-stu-id="59b44-223">In the `GrpcGreeterClient` project, press `Ctrl+F5` to start the client without the debugger.</span></span>

### <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="59b44-224">Visual Studio Code/Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="59b44-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="59b44-225">Greeter hizmetini başlatın.</span><span class="sxs-lookup"><span data-stu-id="59b44-225">Start the Greeter service.</span></span>
* <span data-ttu-id="59b44-226">İstemcisini başlatın.</span><span class="sxs-lookup"><span data-stu-id="59b44-226">Start the client.</span></span>

<!-- End of combined VS/Mac tabs -->

---

<span data-ttu-id="59b44-227">İstemci, "GreeterClient" adını içeren bir iletiyle hizmete bir tebrik gönderir.</span><span class="sxs-lookup"><span data-stu-id="59b44-227">The client sends a greeting to the service with a message containing its name "GreeterClient".</span></span> <span data-ttu-id="59b44-228">Hizmet, "Hello GreeterClient" iletisini yanıt olarak gönderir.</span><span class="sxs-lookup"><span data-stu-id="59b44-228">The service sends the message "Hello GreeterClient" as a response.</span></span> <span data-ttu-id="59b44-229">Komut isteminde "Hello GreeterClient" yanıtı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="59b44-229">The "Hello GreeterClient" response is displayed in the command prompt:</span></span>

```console
Greeting: Hello GreeterClient
Press any key to exit...
```

<span data-ttu-id="59b44-230">GRPC hizmeti, komut istemine yazılan günlüklerde başarılı çağrının ayrıntılarını kaydeder.</span><span class="sxs-lookup"><span data-stu-id="59b44-230">The gRPC service records the details of the successful call in the logs written to the command prompt.</span></span>

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

### <a name="next-steps"></a><span data-ttu-id="59b44-231">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="59b44-231">Next steps</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
