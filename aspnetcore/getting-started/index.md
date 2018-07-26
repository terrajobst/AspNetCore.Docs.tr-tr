---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 7ab9f303d74786c4ac76f002d0f2c66371e78cb8
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228588"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="d807f-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="d807f-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="d807f-104">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="d807f-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="d807f-105">Bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d807f-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="d807f-106">Bir komut kabuğunu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="d807f-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="d807f-107">HTTPS geliştirme sertifikasına güvenmek:</span><span class="sxs-lookup"><span data-stu-id="d807f-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="d807f-108">Windows</span><span class="sxs-lookup"><span data-stu-id="d807f-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="d807f-109">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="d807f-109">The preceding command displays the following dialog:</span></span>

   ![Güvenlik Uyarısı iletişim kutusu](_static/cert.png)

   <span data-ttu-id="d807f-111">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="d807f-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="d807f-112">macOS</span><span class="sxs-lookup"><span data-stu-id="d807f-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="d807f-113">Yukarıdaki komut, şu iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="d807f-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="d807f-114">*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *parolanızı üzerinde sistem Anahtarlık sertifikayı yüklemek bu komutu isteyebilir.    Parola:*</span><span class="sxs-lookup"><span data-stu-id="d807f-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'` *This command might prompt you for your password to install the certificate on the system keychain.    Password:*</span></span>

   <span data-ttu-id="d807f-115">Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="d807f-115">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="d807f-116">Linux</span><span class="sxs-lookup"><span data-stu-id="d807f-116">Linux</span></span>](#tab/linux)

   <a name="see-the-documentation-for-your-linux-distribution-on-how-to-trust-the-https-development-certificate"></a><span data-ttu-id="d807f-117">HTTPS geliştirme sertifikasına güvenmek Linux dağıtımınız için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="d807f-117">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
---

4. <span data-ttu-id="d807f-118">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d807f-118">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="d807f-119">Gözat [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="d807f-119">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="d807f-120">Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="d807f-120">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="d807f-121">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="d807f-121">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="d807f-122">Açık *Pages/About.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="d807f-122">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="d807f-123">Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d807f-123">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="d807f-124">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="d807f-124">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="d807f-125">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d807f-125">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="d807f-126">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="d807f-126">Open a command shell.</span></span> <span data-ttu-id="d807f-127">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="d807f-127">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="d807f-128">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d807f-128">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="d807f-129">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="d807f-129">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="d807f-130">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="d807f-130">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="d807f-131">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="d807f-131">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="d807f-132">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="d807f-132">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="d807f-133">.NET Core'u yükleme **SDK yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="d807f-133">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="d807f-134">Yeni bir ASP.NET Core projesi için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d807f-134">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="d807f-135">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="d807f-135">Open a command shell.</span></span> <span data-ttu-id="d807f-136">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="d807f-136">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="d807f-137">Makinenizde bir sonraki SDK sürümü yüklü değilse, oluşturun bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="d807f-137">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="d807f-138">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="d807f-138">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="d807f-139">Paketleri geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d807f-139">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="d807f-140">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d807f-140">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="d807f-141">[Çalıştırma dotnet](/dotnet/core/tools/dotnet-run) gerekirse komut uygulamayı ilk olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="d807f-141">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="d807f-142">Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="d807f-142">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]
::: moniker-end
