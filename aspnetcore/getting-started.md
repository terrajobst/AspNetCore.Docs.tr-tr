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
ms.openlocfilehash: e814277663ff5a964171a71ebb6e0f094e0ddc60
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="f7cca-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="f7cca-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.0"

1. <span data-ttu-id="f7cca-104">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="f7cca-104">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="f7cca-105">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f7cca-105">Create a new .NET Core project.</span></span>

   <span data-ttu-id="f7cca-106">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="f7cca-106">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f7cca-107">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="f7cca-107">On Windows, open a command prompt.</span></span> <span data-ttu-id="f7cca-108">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="f7cca-108">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="f7cca-109">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f7cca-109">Run the app with the following commands:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="f7cca-110">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="f7cca-110">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="f7cca-111">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="f7cca-111">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="f7cca-112">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="f7cca-112">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="f7cca-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="f7cca-113">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="f7cca-114">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="f7cca-114">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="f7cca-115">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="f7cca-115">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="f7cca-116">Yeni bir .NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f7cca-116">Create a folder for a new .NET Core project.</span></span>

   <span data-ttu-id="f7cca-117">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="f7cca-117">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="f7cca-118">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="f7cca-118">On Windows, open a command prompt.</span></span>

   ```terminal
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="f7cca-119">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="f7cca-119">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="f7cca-120">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f7cca-120">Create a new .NET Core project.</span></span>

   ```terminal
   dotnet new web
   ```

5. <span data-ttu-id="f7cca-121">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="f7cca-121">Restore the packages.</span></span>

    ```terminal
    dotnet restore
    ```

6. <span data-ttu-id="f7cca-122">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f7cca-122">Run the app.</span></span>

   ```terminal
   dotnet run
   ```

   <span data-ttu-id="f7cca-123">[Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulama ilk olarak, derlemeler.</span><span class="sxs-lookup"><span data-stu-id="f7cca-123">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="f7cca-124">Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="f7cca-124">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end