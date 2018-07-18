---
title: SignalR ASP.NET Core ile çalışmaya başlama
author: tdykstra
description: Bu öğreticide, ASP.NET Core için SignalR kullanarak uygulama oluşturun.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 83be28b30cf06eeea37e8d76b3f6444ffd9a10e8
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095497"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a><span data-ttu-id="12ac4-103">SignalR ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="12ac4-103">Get started with SignalR on ASP.NET Core</span></span>

<span data-ttu-id="12ac4-104">Bu öğretici, ASP.NET Core için SignalR kullanarak gerçek zamanlı bir uygulama oluşturmanın temellerini öğretir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-104">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Çözüm](signalr/_static/signalr-get-started-finished.png)

<span data-ttu-id="12ac4-106">Bu öğreticide, aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="12ac4-106">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="12ac4-107">Bir SignalR üzerinde ASP.NET Core web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12ac4-107">Create a SignalR on ASP.NET Core web app.</span></span>
> * <span data-ttu-id="12ac4-108">İçeriği istemcilere göndermek için bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="12ac4-108">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="12ac4-109">Değiştirme `Startup` sınıfı ve uygulamayı yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="12ac4-109">Modify the `Startup` class and configure the app.</span></span>

<span data-ttu-id="12ac4-110">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="12ac4-110">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="12ac4-111">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="12ac4-111">Prerequisites</span></span>

<span data-ttu-id="12ac4-112">Aşağıdaki yazılımları yükleyin:</span><span class="sxs-lookup"><span data-stu-id="12ac4-112">Install the following software:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12ac4-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12ac4-113">Visual Studio</span></span>](#tab/visual-studio)

* [<span data-ttu-id="12ac4-114">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="12ac4-114">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="12ac4-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 sürümünü veya üstünü **ASP.NET ve web geliştirme** iş yükü</span><span class="sxs-lookup"><span data-stu-id="12ac4-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>
* <span data-ttu-id="12ac4-116">[npm](https://www.npmjs.com/get-npm) (Node.js için Paket Yöneticisi)</span><span class="sxs-lookup"><span data-stu-id="12ac4-116">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12ac4-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12ac4-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

* [<span data-ttu-id="12ac4-118">.NET core SDK 2.1 veya üzeri</span><span class="sxs-lookup"><span data-stu-id="12ac4-118">.NET Core SDK 2.1 or later</span></span>](https://www.microsoft.com/net/download/all)
* [<span data-ttu-id="12ac4-119">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12ac4-119">Visual Studio Code</span></span>](https://code.visualstudio.com/download)
* [<span data-ttu-id="12ac4-120">Visual Studio Code için C#</span><span class="sxs-lookup"><span data-stu-id="12ac4-120">C# for Visual Studio Code</span></span>](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* <span data-ttu-id="12ac4-121">[npm](https://www.npmjs.com/get-npm) (Node.js için Paket Yöneticisi)</span><span class="sxs-lookup"><span data-stu-id="12ac4-121">[npm](https://www.npmjs.com/get-npm) (package manager for Node.js)</span></span>

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="12ac4-122">SignalR istemcisi ve sunucusu barındıran bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="12ac4-122">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12ac4-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12ac4-123">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="12ac4-124">Kullanım **dosya** > **yeni proje** menüsünde seçenek ve **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="12ac4-124">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="12ac4-125">Projeyi adlandırın *SignalRChat*.</span><span class="sxs-lookup"><span data-stu-id="12ac4-125">Name the project *SignalRChat*.</span></span>

   ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="12ac4-127">Seçin **Web uygulaması** Razor sayfaları kullanan bir proje oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="12ac4-127">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="12ac4-128">Ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="12ac4-128">Then select **OK**.</span></span> <span data-ttu-id="12ac4-129">Olduğundan emin olun **ASP.NET Core 2.1** SignalR eski sürümleri .NET üzerinde çalışır ancak framework seçiciden seçilir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-129">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

   ![Visual Studio'da yeni proje iletişim kutusu](signalr/_static/signalr-new-project-choose-type.png)

<span data-ttu-id="12ac4-131">Visual Studio içerir `Microsoft.AspNetCore.SignalR` parçası olarak sunucu kitaplıklarını içeren paket kendi **ASP.NET Core Web uygulaması** şablonu.</span><span class="sxs-lookup"><span data-stu-id="12ac4-131">Visual Studio includes the `Microsoft.AspNetCore.SignalR` package containing its server libraries as part of its **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="12ac4-132">Ancak, SignalR için JavaScript istemci kitaplığı kullanılarak yüklenmelidir *npm*.</span><span class="sxs-lookup"><span data-stu-id="12ac4-132">However, the JavaScript client library for SignalR must be installed using *npm*.</span></span>

3. <span data-ttu-id="12ac4-133">Aşağıdaki komutları çalıştırın **Paket Yöneticisi Konsolu** penceresinden proje kök dizini:</span><span class="sxs-lookup"><span data-stu-id="12ac4-133">Run the following commands in the **Package Manager Console** window, from the project root:</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. <span data-ttu-id="12ac4-134">İçinde "/ signalr" adlı yeni bir klasör oluşturun *wwwroot/lib* projenizdeki klasör.</span><span class="sxs-lookup"><span data-stu-id="12ac4-134">Create a new folder named "signalr" inside the  *wwwroot/lib* folder in your project.</span></span> <span data-ttu-id="12ac4-135">Kopyalama *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  bu klasöre.</span><span class="sxs-lookup"><span data-stu-id="12ac4-135">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12ac4-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12ac4-136">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="12ac4-137">Gelen **tümleşik Terminalini**, aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="12ac4-137">From the **Integrated Terminal**, run the following command:</span></span>

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. <span data-ttu-id="12ac4-138">JavaScript istemci kitaplığını kullanarak yükleme *npm*.</span><span class="sxs-lookup"><span data-stu-id="12ac4-138">Install the JavaScript client library using *npm*.</span></span>

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. <span data-ttu-id="12ac4-139">İçinde "/ signalr" adlı yeni bir klasör oluşturun *LIB* projenizdeki klasör.</span><span class="sxs-lookup"><span data-stu-id="12ac4-139">Create a new folder named "signalr" inside the  *lib* folder in your project.</span></span> <span data-ttu-id="12ac4-140">Kopyalama *signalr.js* dosya *node_modules\\ @aspnet\signalr\dist\browser*  bu klasöre.</span><span class="sxs-lookup"><span data-stu-id="12ac4-140">Copy the *signalr.js* file from *node_modules\\@aspnet\signalr\dist\browser* to this folder.</span></span>

---

## <a name="create-the-signalr-hub"></a><span data-ttu-id="12ac4-141">SignalR hub'ı oluşturma</span><span class="sxs-lookup"><span data-stu-id="12ac4-141">Create the SignalR Hub</span></span>

<span data-ttu-id="12ac4-142">Bir hub'ı istemci ve sunucu birbirleri üzerinde yöntemleri çağırmak izin veren bir üst düzey bir işlem hattı olarak hizmet veren bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="12ac4-142">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12ac4-143">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12ac4-143">Visual Studio</span></span>](#tab/visual-studio/)

1. <span data-ttu-id="12ac4-144">Seçim yaparak, projeye bir sınıf ekleyin **dosya** > **yeni** > **dosya** seçerek **Visual C# sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="12ac4-144">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> <span data-ttu-id="12ac4-145">Sınıf adı `ChatHub` ve dosya *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="12ac4-145">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

2. <span data-ttu-id="12ac4-146">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="12ac4-146">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="12ac4-147">`Hub` Özellikleri ve olayları göndermeye ve almaya verilerin yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-147">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

3. <span data-ttu-id="12ac4-148">Oluşturma `SendMessage` yönteminin, tüm bağlı sohbet istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-148">Create the `SendMessage` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="12ac4-149">Fark döndürür bir [görev](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx)SignalR zaman uyumsuz olduğu için.</span><span class="sxs-lookup"><span data-stu-id="12ac4-149">Notice it returns a [Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx), because SignalR is asynchronous.</span></span> <span data-ttu-id="12ac4-150">Zaman uyumsuz kod daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-150">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12ac4-151">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12ac4-151">Visual Studio Code</span></span>](#tab/visual-studio-code/)

1. <span data-ttu-id="12ac4-152">Açık *SignalRChat* Visual Studio code'da klasörü.</span><span class="sxs-lookup"><span data-stu-id="12ac4-152">Open the *SignalRChat* folder in Visual Studio Code.</span></span>

2. <span data-ttu-id="12ac4-153">Bir sınıf seçerek projeye ekleyin. **dosya** > **yeni dosya** menüsünde.</span><span class="sxs-lookup"><span data-stu-id="12ac4-153">Add a class to the project by selecting **File** > **New File** from the menu.</span></span> <span data-ttu-id="12ac4-154">Sınıf adı `ChatHub` ve dosya *ChatHub.cs*.</span><span class="sxs-lookup"><span data-stu-id="12ac4-154">Name the class `ChatHub` and the file *ChatHub.cs*.</span></span>

3. <span data-ttu-id="12ac4-155">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="12ac4-155">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="12ac4-156">`Hub` Özellikler ve olaylar istemciye gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-156">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data to clients.</span></span>

4. <span data-ttu-id="12ac4-157">Ekleme bir `SendMessage` sınıfı için yöntemi.</span><span class="sxs-lookup"><span data-stu-id="12ac4-157">Add a `SendMessage` method to the class.</span></span> <span data-ttu-id="12ac4-158">`SendMessage` Yöntem, tüm bağlı sohbet istemcilere bir ileti gönderir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-158">The `SendMessage` method sends a message to all connected chat clients.</span></span> <span data-ttu-id="12ac4-159">Fark döndürür bir [görev](/dotnet/api/system.threading.tasks.task)SignalR zaman uyumsuz olduğu için.</span><span class="sxs-lookup"><span data-stu-id="12ac4-159">Notice it returns a [Task](/dotnet/api/system.threading.tasks.task), because SignalR is asynchronous.</span></span> <span data-ttu-id="12ac4-160">Zaman uyumsuz kod daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-160">Asynchronous code scales better.</span></span>

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="12ac4-161">Projeyi SignalR kullanacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="12ac4-161">Configure the project to use SignalR</span></span>

<span data-ttu-id="12ac4-162">Böylece isteklerini iletmek için SignalR bilir SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-162">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="12ac4-163">Bir SignalR projesi yapılandırmak için projenin değiştirme `Startup.ConfigureServices` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="12ac4-163">To configure a SignalR project, modify the project's `Startup.ConfigureServices` method.</span></span>

   <span data-ttu-id="12ac4-164">`services.AddSignalR` SignalR Hizmetleri için kullanılabilir hale getirir [bağımlılık ekleme](xref:fundamentals/dependency-injection) sistem.</span><span class="sxs-lookup"><span data-stu-id="12ac4-164">`services.AddSignalR` makes the SignalR services available to the [dependency injection](xref:fundamentals/dependency-injection) system.</span></span>

1. <span data-ttu-id="12ac4-165">Hub'larıyla yolları yapılandırmak `UseSignalR` içinde `Configure` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="12ac4-165">Configure routes to your hubs with `UseSignalR` in the `Configure` method.</span></span> <span data-ttu-id="12ac4-166">`app.UseSignalR` SignalR için ekler [ara yazılım](xref:fundamentals/middleware/index) işlem hattı.</span><span class="sxs-lookup"><span data-stu-id="12ac4-166">`app.UseSignalR` adds SignalR to the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="12ac4-167">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="12ac4-167">Create the SignalR client code</span></span>

1. <span data-ttu-id="12ac4-168">Adlı bir JavaScript dosyası ekleyin *chat.js*, *wwwroot\js* klasör.</span><span class="sxs-lookup"><span data-stu-id="12ac4-168">Add a JavaScript file, named *chat.js*, to the *wwwroot\js* folder.</span></span> <span data-ttu-id="12ac4-169">Aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="12ac4-169">Add the following code to it:</span></span>

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. <span data-ttu-id="12ac4-170">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="12ac4-170">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   <span data-ttu-id="12ac4-171">Önceki HTML adı ve ileti alanları ve bir Gönder düğmesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="12ac4-171">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="12ac4-172">Alttaki komut dosyası başvuruları dikkat edin: SignalR başvurusu ve *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="12ac4-172">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="12ac4-173">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="12ac4-173">Run the app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="12ac4-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="12ac4-174">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="12ac4-175">Seçin **hata ayıklama** > **ayıklamadan Başlat** bir tarayıcıyı başlatın ve Web sitesinin yerel olarak yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="12ac4-175">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="12ac4-176">Adres çubuğundan URL'yi kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="12ac4-176">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="12ac4-177">Başka bir tarayıcı örneğinde (herhangi bir tarayıcıda) açın ve adres çubuğuna URL'yi yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="12ac4-177">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="12ac4-178">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklayın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="12ac4-178">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="12ac4-179">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-179">The name and message are displayed on both pages instantly.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="12ac4-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="12ac4-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="12ac4-181">Tuşuna **hata ayıklama** oluşturup programı çalıştırın (F5).</span><span class="sxs-lookup"><span data-stu-id="12ac4-181">Press **Debug** (F5) to build and run the program.</span></span> <span data-ttu-id="12ac4-182">Programın çalıştırılması, bir tarayıcı penceresi açılır.</span><span class="sxs-lookup"><span data-stu-id="12ac4-182">Running the program opens a browser window.</span></span>

1. <span data-ttu-id="12ac4-183">Başka bir tarayıcı penceresi açın ve Web sitesinin yerel olarak yükleyin.</span><span class="sxs-lookup"><span data-stu-id="12ac4-183">Open another browser window and load the website locally in it.</span></span>

1. <span data-ttu-id="12ac4-184">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklayın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="12ac4-184">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="12ac4-185">Her iki sayfalarında, adını ve iletisini anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="12ac4-185">The name and message are displayed on both pages instantly.</span></span>

---

  ![Çözüm](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a><span data-ttu-id="12ac4-187">İlgili kaynaklar</span><span class="sxs-lookup"><span data-stu-id="12ac4-187">Related resources</span></span>

[<span data-ttu-id="12ac4-188">ASP.NET Core signalr'a giriş</span><span class="sxs-lookup"><span data-stu-id="12ac4-188">Introduction to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
