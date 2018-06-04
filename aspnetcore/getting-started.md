---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 1b4d765c08c67a65a8752e7f650d4e61ec0d2fbf
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34687463"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="0e7fb-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="0e7fb-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="0e7fb-104">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="0e7fb-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="0e7fb-105">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="0e7fb-106">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0e7fb-107">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="0e7fb-108">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0e7fb-108">Enter the following command:</span></span>

    ```terminal
    dotnet new webapp -o aspnetcoreapp
    ```

3. <span data-ttu-id="0e7fb-109">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0e7fb-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="0e7fb-110">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="0e7fb-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="0e7fb-111">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="0e7fb-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="0e7fb-112">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="0e7fb-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="0e7fb-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="0e7fb-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="0e7fb-114">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="0e7fb-115">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="0e7fb-115">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="0e7fb-116">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-116">Create a new .NET Core project.</span></span>

   <span data-ttu-id="0e7fb-117">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0e7fb-118">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-118">On Windows, open a command prompt.</span></span> <span data-ttu-id="0e7fb-119">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0e7fb-119">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="0e7fb-120">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0e7fb-120">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="0e7fb-121">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="0e7fb-121">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="0e7fb-122">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="0e7fb-122">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="0e7fb-123">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="0e7fb-123">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="0e7fb-124">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="0e7fb-124">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="0e7fb-125">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-125">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="0e7fb-126">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="0e7fb-126">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="0e7fb-127">Yeni bir .NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-127">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="0e7fb-128">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-128">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="0e7fb-129">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-129">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="0e7fb-130">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-130">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="0e7fb-131">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-131">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="0e7fb-132">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-132">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="0e7fb-133">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-133">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="0e7fb-134">[Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulama ilk olarak, derlemeler.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-134">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="0e7fb-135">Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="0e7fb-135">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end