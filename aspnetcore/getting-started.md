---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 5/31/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: 0b5de87b99e57df16f663f13d41e750a29ea6276
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252054"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="e5506-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="e5506-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="e5506-104">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="e5506-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="e5506-105">Bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5506-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="e5506-106">Bir komut kabuğu'nu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="e5506-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="e5506-108">HTTPS geliştirme sertifikası güven:</span><span class="sxs-lookup"><span data-stu-id="e5506-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="e5506-109">Windows</span><span class="sxs-lookup"><span data-stu-id="e5506-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](getting-started/_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="e5506-110">macOS</span><span class="sxs-lookup"><span data-stu-id="e5506-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="e5506-111">Linux</span><span class="sxs-lookup"><span data-stu-id="e5506-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="e5506-112">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e5506-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="e5506-113">Gözat [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="e5506-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="e5506-114">Tıklatın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="e5506-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="e5506-115">Bu uygulamayı, kişisel bilgi korumayarak.</span><span class="sxs-lookup"><span data-stu-id="e5506-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="e5506-116">Açık *Pages/About.cshtml* ve aşağıdaki vurgulanmış biçimlendirmeyi sayfasıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e5506-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    <span data-ttu-id="e5506-117">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="e5506-117">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9)]</span></span>

7. <span data-ttu-id="e5506-118">Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e5506-118">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="e5506-119">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="e5506-119">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="e5506-120">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5506-120">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="e5506-121">Bir komut kabuğu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="e5506-121">Open a command shell.</span></span> <span data-ttu-id="e5506-122">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="e5506-122">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="e5506-123">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e5506-123">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="e5506-124">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="e5506-124">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="e5506-125">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="e5506-125">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="e5506-126">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="e5506-126">The time on the server is @DateTime.Now":</span></span>

    <span data-ttu-id="e5506-127">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span><span class="sxs-lookup"><span data-stu-id="e5506-127">[!code-cshtml[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]</span></span>

6. <span data-ttu-id="e5506-128">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e5506-128">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="e5506-129">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="e5506-129">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="e5506-130">Yeni bir ASP.NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5506-130">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="e5506-131">Bir komut kabuğu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="e5506-131">Open a command shell.</span></span> <span data-ttu-id="e5506-132">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="e5506-132">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="e5506-133">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="e5506-133">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="e5506-134">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="e5506-134">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="e5506-135">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="e5506-135">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="e5506-136">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e5506-136">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="e5506-137">[Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulama ilk olarak, derlemeler.</span><span class="sxs-lookup"><span data-stu-id="e5506-137">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="e5506-138">Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e5506-138">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
