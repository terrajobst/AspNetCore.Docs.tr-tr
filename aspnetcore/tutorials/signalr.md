---
title: SignalR ASP.NET Core ile çalışmaya başlama
author: rachelappel
description: Bu öğreticide, ASP.NET Core için SignalR kullanarak uygulama oluşturun.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830561"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="2010a-103">SignalR ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="2010a-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="2010a-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="2010a-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="2010a-105">Bu öğretici, ASP.NET Core için SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temellerini öğretir.</span><span class="sxs-lookup"><span data-stu-id="2010a-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Çözüm](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="2010a-107">Bu öğreticide, aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="2010a-107">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2010a-108">Bir SignalR üzerinde ASP.NET Core web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2010a-108">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="2010a-109">İçeriği istemcilere göndermek için bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2010a-109">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="2010a-110">Değiştirme `Startup` sınıfı ve uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2010a-110">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="2010a-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2010a-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2010a-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="2010a-112">Prerequisites</span></span>

<span data-ttu-id="2010a-113">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="2010a-113">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2010a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2010a-114">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="2010a-115">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="2010a-115">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="2010a-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümünü veya üstünü **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="2010a-116">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* [<span data-ttu-id="2010a-117">npm</span><span class="sxs-lookup"><span data-stu-id="2010a-117">npm</span></span>](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2010a-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2010a-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="2010a-119">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="2010a-119">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="2010a-120">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2010a-120">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="2010a-121">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="2010a-121">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [<span data-ttu-id="2010a-122">npm</span><span class="sxs-lookup"><span data-stu-id="2010a-122">npm</span></span>](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="2010a-123">SignalR istemcisi ve sunucusu barındıran bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="2010a-123">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2010a-124">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2010a-124">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="2010a-125">Kullanım **dosya** > **yeni proje** menüsünde seçenek ve **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="2010a-125">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="2010a-126">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="2010a-126">Name the project *SignalRChat*.</span></span>

   ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="2010a-128">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="2010a-128">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="2010a-129">Ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="2010a-129">Then select **OK**.</span></span> <span data-ttu-id="2010a-130">Olduğundan emin olun **ASP.NET Core 2.1** SignalR eski sürümleri .NET üzerinde çalışır ancak framework seçiciden seçilir.</span><span class="sxs-lookup"><span data-stu-id="2010a-130">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="2010a-132">Visual Studio içerir `Microsoft.AspNetCore.SignalR` parçası olarak sunucu kitaplıklarını içeren paket kendi **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="2010a-132">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="2010a-133">Ancak, SignalR için JavaScript istemci kitaplığı kullanılarak yüklenmelidir *npm*.</span><span class="sxs-lookup"><span data-stu-id="2010a-133">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="2010a-134">Aşağıdaki komutları çalıştırın **Paket Yöneticisi Konsolu** penceresinden proje kök dizini:</span><span class="sxs-lookup"><span data-stu-id="2010a-134">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="2010a-135">İçinde "/ signalr" adlı yeni bir klasör oluşturun *LIB* projenizdeki klasör.</span><span class="sxs-lookup"><span data-stu-id="2010a-135">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="2010a-136">Kopyalama *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  bu klasöre.</span><span class="sxs-lookup"><span data-stu-id="2010a-136">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2010a-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2010a-137">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="2010a-138">Gelen **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2010a-138">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="2010a-139">JavaScript istemci kitaplığını kullanarak yükleme *npm*.</span><span class="sxs-lookup"><span data-stu-id="2010a-139">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="2010a-140">İçinde "/ signalr" adlı yeni bir klasör oluşturun *LIB* projenizdeki klasör.</span><span class="sxs-lookup"><span data-stu-id="2010a-140">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="2010a-141">Kopyalama *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  bu klasöre.</span><span class="sxs-lookup"><span data-stu-id="2010a-141">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="2010a-142">SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="2010a-142">Create the SignalR Hub</span></span>

<span data-ttu-id="2010a-143">Bir hub'ı istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı olarak hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="2010a-143">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2010a-144">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2010a-144">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="2010a-145">Seçim yaparak, projeye bir sınıf ekleyin **dosya** > **yeni** > **dosya** seçerek **Visual C# sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="2010a-145">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="2010a-146">Sınıf adı `ChatHub` ve dosya *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="2010a-146">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="2010a-147">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="2010a-147">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="2010a-148">`Hub` Özellikleri ve olayları göndermeye ve almaya verilerin yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="2010a-148">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="2010a-149">Oluşturma `SendMessage` yönteminin, tüm bağlı sohbet istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="2010a-149">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="2010a-150">Fark döndürür bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)SignalR zaman uyumsuz olduğu için.</span><span class="sxs-lookup"><span data-stu-id="2010a-150">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="2010a-151">Zaman uyumsuz kod daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="2010a-151">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2010a-152">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2010a-152">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="2010a-153">Açık *SignalRChat* Visual Studio code'da klasörü.</span><span class="sxs-lookup"><span data-stu-id="2010a-153">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="2010a-154">Bir sınıf seçerek projeye ekleyin. **dosya** > **yeni dosya** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="2010a-154">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="2010a-155">Sınıf adı `ChatHub` ve dosya *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="2010a-155">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="2010a-156">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="2010a-156">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="2010a-157">`Hub` Özellikler ve olaylar istemciye gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="2010a-157">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="2010a-158">Ekleme bir `SendMessage` sınıfı için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2010a-158">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="2010a-159">`SendMessage` Yöntem, tüm bağlı sohbet istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="2010a-159">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="2010a-160">Fark döndürür bir [görev](/dotnet/api/system.threading.tasks.task)SignalR zaman uyumsuz olduğu için.</span><span class="sxs-lookup"><span data-stu-id="2010a-160">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="2010a-161">Zaman uyumsuz kod daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="2010a-161">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="2010a-162">Projeyi SignalR kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="2010a-162">Configure the project to use SignalR</span></span>

<span data-ttu-id="2010a-163">Böylece isteklerini iletmek için SignalR bilir SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="2010a-163">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="2010a-164">Bir SignalR projesi yapılandırmak için projenin değiştirme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2010a-164">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="2010a-165">`services.AddSignalR` SignalR Hizmetleri için kullanılabilir hale getirir [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem.</span><span class="sxs-lookup"><span data-stu-id="2010a-165">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="2010a-166">Hub'larıyla yolları yapılandırmak `UseSignalR` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="2010a-166">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="2010a-167">`app.UseSignalR` SignalR için ekler [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.</span><span class="sxs-lookup"><span data-stu-id="2010a-167">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="2010a-168">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="2010a-168">Create the SignalR client code</span></span>

1. <span data-ttu-id="2010a-169">Adlı bir JavaScript dosyası ekleyin *chat.js*, *wwwroot\js* klasör.</span><span class="sxs-lookup"><span data-stu-id="2010a-169">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="2010a-170">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2010a-170">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="2010a-171">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="2010a-171">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="2010a-172">Önceki HTML adı ve ileti alanları ve bir Gönder düğmesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="2010a-172">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="2010a-173">Alttaki komut dosyası başvuruları dikkat edin: SignalR başvurusu ve *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="2010a-173">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="2010a-174">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="2010a-174">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2010a-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2010a-175">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="2010a-176">Seçin **hata ayıklama** > **ayıklamadan Başlat** bir tarayıcıyı başlatın ve Web sitesinin yerel olarak yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="2010a-176">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="2010a-177">Adres çubuğundan URL'yi kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="2010a-177">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="2010a-178">Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="2010a-178">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="2010a-179">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklayın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2010a-179">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="2010a-180">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2010a-180">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2010a-181">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2010a-181">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="2010a-182">Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5).</span><span class="sxs-lookup"><span data-stu-id="2010a-182">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="2010a-183">Programın çalıştırılması, bir tarayıcı penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="2010a-183">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="2010a-184">Başka bir tarayıcı penceresi açın ve Web sitesinin yerel olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="2010a-184">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="2010a-185">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklayın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2010a-185">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="2010a-186">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="2010a-186">The name and message are displayed on both pages instantly.</span></span>

---

  ![Çözüm](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="2010a-188">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2010a-188">Related resources</span></span>

[<span data-ttu-id="2010a-189">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="2010a-189">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
