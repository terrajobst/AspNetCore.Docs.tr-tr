---
uid: signalr/get-started-signalr-core
title: "ASP.NET Core üzerinde SignalR ile çalışmaya başlama"
author: rachelappel
ms.author: rachelap
description: "Bu öğreticide, SignalR için ASP.NET Core kullanarak bir uygulama oluşturun."
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
ms.openlocfilehash: 5e16569aa492e3639aa97abd241610b361fb0c56
ms.sourcegitcommit: 53ee14b9c8200f44705d8997c3619fa874192d45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="e75f6-103">Öğretici: ASP.NET Core için SignalR ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="e75f6-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="e75f6-104">Tarafından [Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="e75f6-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="e75f6-105">Bu öğretici, SignalR için ASP.NET Core kullanarak gerçek zamanlı bir uygulama oluşturma temelleri öğretir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![Çözüm](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="e75f6-107">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e75f6-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="e75f6-108">Bu öğretici aşağıdaki SignalR geliştirme görevleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="e75f6-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e75f6-109">Bir ASP.NET Core web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e75f6-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="e75f6-110">İçeriği istemcilere göndermek için bir SignalR hub'ı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e75f6-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="e75f6-111">SignalR JavaScript kitaplığı iletilerini göndermek ve hub güncelleştirmelerini görüntülemek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="e75f6-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e75f6-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e75f6-112">Prerequisites</span></span>

<span data-ttu-id="e75f6-113">Aşağıdaki yazılımı yükleyin:</span><span class="sxs-lookup"><span data-stu-id="e75f6-113">Install the following software:</span></span>

* <span data-ttu-id="e75f6-114">[.NET core 2.1.0 Önizleme 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) veya daha yenisi</span><span class="sxs-lookup"><span data-stu-id="e75f6-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="e75f6-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 veya ASP.NET ve web geliştirme iş yükü ile sonraki bir sürümü</span><span class="sxs-lookup"><span data-stu-id="e75f6-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="e75f6-116">npm</span><span class="sxs-lookup"><span data-stu-id="e75f6-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="e75f6-117">SignalR istemcisi ve sunucusu barındıran bir ASP.NET Core projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e75f6-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="e75f6-118">Kullanım **dosya** > **yeni proje** menü seçeneğini ve seçin **ASP.NET çekirdek Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="e75f6-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="e75f6-119">Proje adı `SignalRChat`.</span><span class="sxs-lookup"><span data-stu-id="e75f6-119">Name the project `SignalRChat`.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="e75f6-121">Seçin **Web uygulaması** Razor sayfalarının kullanarak bir proje oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="e75f6-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="e75f6-122">Ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e75f6-122">Then select **Ok**.</span></span> <span data-ttu-id="e75f6-123">Olduğundan emin olun **ASP.NET Core 2.1** SignalR .NET eski sürümlerinde çalışır ancak framework seçicisini seçilir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![Visual Studio'da yeni proje iletişim kutusu](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="e75f6-125">SignalR sunucu tarafı kodu konak kitaplıkları proje şablonu dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="e75f6-126">İstemci tarafı JavaScript ile ayrı olarak yüklemeniz [npm](https://www.npmjs.com/).</span><span class="sxs-lookup"><span data-stu-id="e75f6-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="e75f6-127">Kopya *signalr.js* gelen *node_modules\\ @aspnet\signalr\dist\browser*  için *wwwroot\lib* projenizdeki klasöre.</span><span class="sxs-lookup"><span data-stu-id="e75f6-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="e75f6-128">SignalR hub'ı Oluştur</span><span class="sxs-lookup"><span data-stu-id="e75f6-128">Create the SignalR Hub</span></span>

<span data-ttu-id="e75f6-129">Bir hub istemci ve sunucu birbirine yöntemlerini çağırmaya izin veren üst düzey bir işlem hattı görevi gören bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="e75f6-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="e75f6-130">Bir sınıf projeye seçerek eklemek **dosya** > **yeni** > **dosya** ve seçerek **Visual C# sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e75f6-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="e75f6-131">Devralınan `Microsoft.AspNetCore.SignalR.Hub`.</span><span class="sxs-lookup"><span data-stu-id="e75f6-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="e75f6-132">`Hub` Özellikleri ve olayları gönderme ve alma veri yanı sıra bağlantıları ve grupları yönetmek için sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="e75f6-133">Oluşturma `Send` tüm bağlı sohbet istemcilere bir ileti gönderir yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e75f6-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="e75f6-134">Döndürdüğü fark bir `Task`, SignalR zaman uyumsuz olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="e75f6-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="e75f6-135">Zaman uyumsuz kodu daha iyi ölçeklenir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="e75f6-136">SignalR kullanmak üzere proje yapılandırma</span><span class="sxs-lookup"><span data-stu-id="e75f6-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="e75f6-137">SignalR için isteklerini iletmek için bilir böylece SignalR sunucusunun yapılandırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="e75f6-138">Bir SignalR projesi yapılandırmak için değiştirin `ConfigureServices` yöntemi uygulamanın `Startup` yapılan bir çağrı ekleyerek sınıfı `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="e75f6-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="e75f6-139">`services.AddSignalR` SignalR parçası olarak ekler [ASP.NET Core Ara](xref:fundamentals/middleware/index) ardışık düzen.</span><span class="sxs-lookup"><span data-stu-id="e75f6-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="e75f6-140">Yollar kullanılarak, hub'larınız için yapılandırma `UseSignalR`.</span><span class="sxs-lookup"><span data-stu-id="e75f6-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="e75f6-141">SignalR istemci kodu oluşturma</span><span class="sxs-lookup"><span data-stu-id="e75f6-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="e75f6-142">İçeriği Değiştir *Pages\Index.cshtml* aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e75f6-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="e75f6-143">Önceki HTML adını ve ileti alanları ve bir gönderme düğmesi görüntüler.</span><span class="sxs-lookup"><span data-stu-id="e75f6-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="e75f6-144">Komut dosyası başvuruları altındaki dikkat edin: SignalR başvurusu ve *chat.js*.</span><span class="sxs-lookup"><span data-stu-id="e75f6-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="e75f6-145">Bir JavaScript dosyası ekleme *wwwroot\js* adlı klasörü *chat.js* ve aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e75f6-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="e75f6-146">Uygulama Çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e75f6-146">Run the app</span></span>

1. <span data-ttu-id="e75f6-147">Seçin **hata ayıklama** > **Başlat hata ayıklama olmadan** bir tarayıcıyı başlatın ve Web sitesi yerel olarak yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="e75f6-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="e75f6-148">Adres çubuğundan URL'yi kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="e75f6-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="e75f6-149">Başka bir tarayıcı örneğini (herhangi bir tarayıcıda) açın ve URL'yi adres çubuğuna yapıştırın.</span><span class="sxs-lookup"><span data-stu-id="e75f6-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="e75f6-150">Ya da tarayıcı seçin, bir ad ve ileti girin ve tıklatın **Gönder** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e75f6-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="e75f6-151">Ad ve ileti iki sayfalarında anında görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="e75f6-151">The name and message are displayed on both pages instantly.</span></span>

  ![Çözüm](get-started-signalr-core/_static/signalr-get-started-finished.png)
