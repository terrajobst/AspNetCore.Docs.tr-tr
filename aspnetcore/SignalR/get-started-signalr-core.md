---
title: "ASP.NET Core üzerinde SignalR ile çalışmaya başlama"
author: rachelappel
description: "Bu öğreticide, SignalR için ASP.NET Core kullanarak bir uygulama oluşturun."
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="0ae2c-103">Öğretici: ASP.NET Core için SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="0ae2c-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="0ae2c-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="0ae2c-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="0ae2c-105">Bu öğretici, SignalR için ASP.NET Core kullanarak gerçek zamanlı bir uygulama oluşturma temelleri öğretir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Çözüm](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="0ae2c-107">Bu öğretici aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="0ae2c-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0ae2c-108">Bir SignalR ASP.NET Core web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="0ae2c-109">İçeriği istemcilere göndermek için bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="0ae2c-110">Değiştirme `Startup` sınıfı ve uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="0ae2c-111">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0ae2c-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="prerequisites"></a><span data-ttu-id="0ae2c-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0ae2c-112">Prerequisites</span></span>

<span data-ttu-id="0ae2c-113">Aşağıdaki yazılımı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="0ae2c-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ae2c-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ae2c-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="0ae2c-115">[.NET core 2.1.0 Önizleme 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="0ae2c-115">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="0ae2c-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) sürüm 15.6 veya üstü **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="0ae2c-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="0ae2c-117">npm</span><span class="sxs-lookup"><span data-stu-id="0ae2c-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ae2c-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ae2c-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="0ae2c-119">[.NET core 2.1.0 Önizleme 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="0ae2c-119">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* [<span data-ttu-id="0ae2c-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ae2c-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download) 
* [<span data-ttu-id="0ae2c-121">C# Visual Studio kodu</span><span class="sxs-lookup"><span data-stu-id="0ae2c-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="0ae2c-122">npm</span><span class="sxs-lookup"><span data-stu-id="0ae2c-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="0ae2c-123">SignalR istemcisi ve sunucusu barındıran bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0ae2c-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ae2c-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ae2c-124">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0ae2c-125">Kullanım **dosya** > **yeni proje** menü seçeneğini ve seçin **ASP.NET çekirdek Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="0ae2c-126">Proje adı *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-126">Name the project *SignalRChat*.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="0ae2c-128">Seçin **Web uygulaması** Razor sayfalarının kullanarak bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="0ae2c-129">Ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-129">Then select **OK**.</span></span> <span data-ttu-id="0ae2c-130">Olduğundan emin olun **ASP.NET Core 2.1** SignalR .NET eski sürümlerinde çalışır ancak framework seçicisini seçilir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. <span data-ttu-id="0ae2c-132">' Nde projeye sağ **Çözüm Gezgini** > **Ekle** > **yeni öğe** > **npm yapılandırma dosyası** .</span><span class="sxs-lookup"><span data-stu-id="0ae2c-132">Right-click the project in **Solution Explorer** > **Add** > **New Item** > **npm Configuration File**.</span></span> <span data-ttu-id="0ae2c-133">Dosya adı *package.json*.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-133">Name the file *package.json*.</span></span>

4. <span data-ttu-id="0ae2c-134">Aşağıdaki komutu çalıştırın **Paket Yöneticisi Konsolu** penceresinden proje kök:</span><span class="sxs-lookup"><span data-stu-id="0ae2c-134">Run the following command in the **Package Manager Console** window, from the project root:</span></span>

    ```console
      npm install @aspnet/signalr
    ```
5. <span data-ttu-id="0ae2c-135">Kopya *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  için *wwwroot\lib* projenizdeki klasöre.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ae2c-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ae2c-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="0ae2c-137">Gelen **tümleşik Terminal**, aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0ae2c-137">From the **Integrated Terminal**, run the following command:</span></span>
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. <span data-ttu-id="0ae2c-138">JavaScript istemci kitaplığını kullanarak yükleme *npm*.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-138">Install the JavaScript client library using *npm*.</span></span>

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a><span data-ttu-id="0ae2c-139">SignalR hub'ı Oluştur</span><span class="sxs-lookup"><span data-stu-id="0ae2c-139">Create the SignalR Hub</span></span>

<span data-ttu-id="0ae2c-140">Bir hub istemci ve sunucu birbirine yöntemlerini çağırmaya izin veren üst düzey bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-140">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ae2c-141">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ae2c-141">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0ae2c-142">Bir sınıf projeye seçerek eklemek **dosya** > **yeni** > **dosya** ve seçerek **Visual C# sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-142">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span>

1. <span data-ttu-id="0ae2c-143">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-143">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0ae2c-144">`Hub` Özellikleri ve olayları gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-144">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="0ae2c-145">Oluşturma `SendMessage` tüm bağlı sohbet istemcilere bir ileti gönderir yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-145">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="0ae2c-146">Döndürdüğü fark bir [görev](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), SignalR zaman uyumsuz olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-146">Notice it returns a [Task](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="0ae2c-147">Zaman uyumsuz kodu daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-147">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ae2c-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ae2c-148">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="0ae2c-149">Açık *SignalRChat* Visual Studio Code klasöründe.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-149">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

1. <span data-ttu-id="0ae2c-150">Bir sınıf projeye seçerek eklemek **dosya** > **yeni dosya** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-150">Add a class to the project by selecting **File** > **New File** from the menu.</span></span>

1. <span data-ttu-id="0ae2c-151">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-151">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="0ae2c-152">`Hub` Özellikleri ve olayları istemciye gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-152">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

1. <span data-ttu-id="0ae2c-153">Ekleme bir `SendMessage` sınıfına yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-153">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="0ae2c-154">`SendMessage` Yöntem, tüm bağlı sohbet istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-154">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="0ae2c-155">Döndürdüğü fark bir [görev](/dotnet/api/system.threading.tasks.task), SignalR zaman uyumsuz olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-155">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="0ae2c-156">Zaman uyumsuz kodu daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-156">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="0ae2c-157">SignalR kullanmak üzere proje yapılandırma</span><span class="sxs-lookup"><span data-stu-id="0ae2c-157">Configure the project to use SignalR</span></span>

<span data-ttu-id="0ae2c-158">SignalR için isteklerini iletmek için bilir böylece SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-158">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="0ae2c-159">Bir SignalR projesi yapılandırmak için projenin değiştirin `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-159">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="0ae2c-160">`services.AddSignalR` SignalR parçası olarak ekler [ara yazılım](xref:fundamentals/middleware/index) ardışık düzen.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-160">`services.AddSignalR` adds SignalR as part of the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="0ae2c-161">Yollar kullanılarak, hub'larınız için yapılandırma `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-161">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="0ae2c-162">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="0ae2c-162">Create the SignalR client code</span></span>

1. <span data-ttu-id="0ae2c-163">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="0ae2c-163">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="0ae2c-164">Önceki HTML adını ve ileti alanları ve bir gönderme düğmesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-164">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="0ae2c-165">Komut dosyası başvuruları altındaki dikkat edin: SignalR başvurusu ve *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-165">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="0ae2c-166">Adlı bir JavaScript dosyası ekleme *chat.js*, *wwwroot\js* klasör.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-166">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="0ae2c-167">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="0ae2c-167">Add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="0ae2c-168">Uygulama Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0ae2c-168">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0ae2c-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0ae2c-169">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="0ae2c-170">Seçin **hata ayıklama** > **Başlat hata ayıklama olmadan** bir tarayıcıyı başlatın ve Web sitesi yerel olarak yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-170">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="0ae2c-171">Adres çubuğundan URL'yi kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-171">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="0ae2c-172">Başka bir tarayıcı örneğini (herhangi bir tarayıcıda) açın ve URL'yi adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-172">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="0ae2c-173">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-173">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0ae2c-174">Ad ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-174">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="0ae2c-175">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="0ae2c-175">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="0ae2c-176">Tuşuna **hata ayıklama** derlemek ve programı çalıştırmak için (F5).</span><span class="sxs-lookup"><span data-stu-id="0ae2c-176">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="0ae2c-177">Programın çalıştırılması, bir tarayıcı penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-177">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="0ae2c-178">Başka bir tarayıcı penceresi açın ve Web sitesi yerel olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-178">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="0ae2c-179">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="0ae2c-180">Ad ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="0ae2c-180">The name and message are displayed on both pages instantly.</span></span>

-----

  ![Çözüm](get-started-signalr-core/_static/signalr-get-started-finished.png)