---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 22e9c982921cc03d89506e18ff99bf481027dda6
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077666"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="2b95c-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="2b95c-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="2b95c-104">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="2b95c-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="2b95c-105">Bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2b95c-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="2b95c-106">Bir komut kabuğu'nu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2b95c-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    <span data-ttu-id="2b95c-107">[! [] (~/İncludes/webapp-alias-notice.md) içerir [](~/includes/webapp-alias-notice.md)]</span><span class="sxs-lookup"><span data-stu-id="2b95c-107">[!INCLUDE [](~/includes/webapp-alias-notice.md) [](~/includes/webapp-alias-notice.md)]</span></span>

3. <span data-ttu-id="2b95c-108">HTTPS geliştirme sertifikası güven:</span><span class="sxs-lookup"><span data-stu-id="2b95c-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2b95c-109">Windows</span><span class="sxs-lookup"><span data-stu-id="2b95c-109">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="2b95c-110">macOS</span><span class="sxs-lookup"><span data-stu-id="2b95c-110">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="2b95c-111">Linux</span><span class="sxs-lookup"><span data-stu-id="2b95c-111">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="2b95c-112">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2b95c-112">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="2b95c-113">Gözat [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="2b95c-113">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="2b95c-114">Tıklatın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="2b95c-114">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="2b95c-115">Bu uygulamayı, kişisel bilgi korumayarak.</span><span class="sxs-lookup"><span data-stu-id="2b95c-115">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="2b95c-116">Açık *Pages/About.cshtml* ve aşağıdaki vurgulanmış biçimlendirmeyi sayfasıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="2b95c-116">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="2b95c-117">Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="2b95c-117">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="2b95c-118">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="2b95c-118">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="2b95c-119">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2b95c-119">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="2b95c-120">Bir komut kabuğu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="2b95c-120">Open a command shell.</span></span> <span data-ttu-id="2b95c-121">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="2b95c-121">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="2b95c-122">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2b95c-122">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="2b95c-123">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="2b95c-123">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="2b95c-124">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="2b95c-124">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="2b95c-125">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="2b95c-125">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="2b95c-126">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="2b95c-126">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="2b95c-127">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="2b95c-127">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="2b95c-128">Yeni bir ASP.NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2b95c-128">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="2b95c-129">Bir komut kabuğu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="2b95c-129">Open a command shell.</span></span> <span data-ttu-id="2b95c-130">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="2b95c-130">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="2b95c-131">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="2b95c-131">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="2b95c-132">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2b95c-132">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="2b95c-133">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="2b95c-133">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="2b95c-134">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2b95c-134">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="2b95c-135">[Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulama ilk olarak, derlemeler.</span><span class="sxs-lookup"><span data-stu-id="2b95c-135">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="2b95c-136">Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="2b95c-136">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
