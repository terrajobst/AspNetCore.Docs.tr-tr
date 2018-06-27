---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: eb049dea2800cf2e12c044b88d1664ee80bb95a5
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2018
ms.locfileid: "36291674"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="6e95a-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="6e95a-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="6e95a-104">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="6e95a-104">Install the [!INCLUDE[](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="6e95a-105">Bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e95a-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="6e95a-106">Bir komut kabuğu'nu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="6e95a-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="6e95a-107">HTTPS geliştirme sertifikası güven:</span><span class="sxs-lookup"><span data-stu-id="6e95a-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6e95a-108">Windows</span><span class="sxs-lookup"><span data-stu-id="6e95a-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following dialog:

    ![Security warning dialog](_static/cert.png)

    Select **Yes** if you agree to trust the development certificate.

# <a name="macostabmacos"></a>[<span data-ttu-id="6e95a-109">macOS</span><span class="sxs-lookup"><span data-stu-id="6e95a-109">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

    The preceding command displays the following message:

    *Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:*
    `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`
    *This command might prompt you for your password to install the certificate on the system keychain.
    Password:*

    Enter your password if you agree to trust the development certificate.

# <a name="linuxtablinux"></a>[<span data-ttu-id="6e95a-110">Linux</span><span class="sxs-lookup"><span data-stu-id="6e95a-110">Linux</span></span>](#tab/linux)

    See the documentation for your Linux distribution on how to trust the HTTPS development certificate
---

4. <span data-ttu-id="6e95a-111">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6e95a-111">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="6e95a-112">Gözat [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="6e95a-112">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="6e95a-113">Tıklatın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="6e95a-113">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="6e95a-114">Bu uygulamayı, kişisel bilgi korumayarak.</span><span class="sxs-lookup"><span data-stu-id="6e95a-114">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="6e95a-115">Açık *Pages/About.cshtml* ve aşağıdaki vurgulanmış biçimlendirmeyi sayfasıyla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6e95a-115">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="6e95a-116">Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6e95a-116">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="6e95a-117">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="6e95a-117">Install the [!INCLUDE[](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="6e95a-118">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e95a-118">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="6e95a-119">Bir komut kabuğu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="6e95a-119">Open a command shell.</span></span> <span data-ttu-id="6e95a-120">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="6e95a-120">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="6e95a-121">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6e95a-121">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="6e95a-122">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="6e95a-122">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="6e95a-123">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="6e95a-123">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="6e95a-124">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="6e95a-124">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="6e95a-125">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6e95a-125">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="6e95a-126">.NET Core yükleme **SDK'sı yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfası](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="6e95a-126">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="6e95a-127">Yeni bir ASP.NET Core proje için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e95a-127">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="6e95a-128">Bir komut kabuğu'nu açın.</span><span class="sxs-lookup"><span data-stu-id="6e95a-128">Open a command shell.</span></span> <span data-ttu-id="6e95a-129">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="6e95a-129">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="6e95a-130">Makinenizde sonraki bir SDK sürümünü yüklediyseniz, oluşturma bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="6e95a-130">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="6e95a-131">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6e95a-131">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="6e95a-132">Paketler geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="6e95a-132">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="6e95a-133">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6e95a-133">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="6e95a-134">[Çalıştırmak dotnet](/dotnet/core/tools/dotnet-run) gerekirse komutu uygulama ilk olarak, derlemeler.</span><span class="sxs-lookup"><span data-stu-id="6e95a-134">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="6e95a-135">Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="6e95a-135">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE[next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
