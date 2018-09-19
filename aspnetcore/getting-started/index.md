---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 06129200834607188052f44a888749c51662f638
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011611"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="9614e-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="9614e-103">Get started with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="9614e-104">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="9614e-104">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="9614e-105">Bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9614e-105">Create an ASP.NET Core project.</span></span> <span data-ttu-id="9614e-106">Bir komut kabuğunu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="9614e-106">Open a command shell and enter the following command:</span></span>

    ```console
    dotnet new webapp -o aspnetcoreapp
    ```

    [!INCLUDE [](~/includes/webapp-alias-notice.md)]

3. <span data-ttu-id="9614e-107">HTTPS geliştirme sertifikasına güvenmek:</span><span class="sxs-lookup"><span data-stu-id="9614e-107">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="9614e-108">Windows</span><span class="sxs-lookup"><span data-stu-id="9614e-108">Windows</span></span>](#tab/windows)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="9614e-109">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="9614e-109">The preceding command displays the following dialog:</span></span>

   ![Güvenlik Uyarısı iletişim kutusu](_static/cert.png)

   <span data-ttu-id="9614e-111">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="9614e-111">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="9614e-112">macOS</span><span class="sxs-lookup"><span data-stu-id="9614e-112">macOS</span></span>](#tab/macos)

    ```console
    dotnet dev-certs https --trust
    ```

   <span data-ttu-id="9614e-113">Yukarıdaki komut, şu iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="9614e-113">The preceding command displays the following message:</span></span>

   <span data-ttu-id="9614e-114">*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="9614e-114">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>
   <span data-ttu-id="9614e-115">*Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir. Parola:*</span><span class="sxs-lookup"><span data-stu-id="9614e-115">*This command might prompt you for your password to install the certificate on the system keychain. Password:*</span></span>

   <span data-ttu-id="9614e-116">Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="9614e-116">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="9614e-117">Linux</span><span class="sxs-lookup"><span data-stu-id="9614e-117">Linux</span></span>](#tab/linux)

   <span data-ttu-id="9614e-118">HTTPS geliştirme sertifikasına güvenmek Linux dağıtımınız için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="9614e-118">See the documentation for your Linux distribution on how to trust the HTTPS development certificate</span></span>
   
---

4. <span data-ttu-id="9614e-119">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9614e-119">Run the app:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="9614e-120">Gözat [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="9614e-120">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="9614e-121">Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="9614e-121">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="9614e-122">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="9614e-122">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="9614e-123">Açık *Pages/About.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9614e-123">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="9614e-124">Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9614e-124">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="9614e-125">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="9614e-125">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="9614e-126">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9614e-126">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="9614e-127">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="9614e-127">Open a command shell.</span></span> <span data-ttu-id="9614e-128">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="9614e-128">Enter the following command:</span></span>

    ```console
    dotnet new razor -o aspnetcoreapp
    ```

3. <span data-ttu-id="9614e-129">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="9614e-129">Run the app with the following commands:</span></span>

    ```console
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="9614e-130">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="9614e-130">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="9614e-131">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="9614e-131">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="9614e-132">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="9614e-132">The time on the server is @DateTime.Now":</span></span>

    [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="9614e-133">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9614e-133">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="9614e-134">.NET Core'u yükleme **SDK yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="9614e-134">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="9614e-135">Yeni bir ASP.NET Core projesi için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9614e-135">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="9614e-136">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="9614e-136">Open a command shell.</span></span> <span data-ttu-id="9614e-137">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="9614e-137">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="9614e-138">Makinenizde bir sonraki SDK sürümü yüklü değilse, oluşturun bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="9614e-138">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="9614e-139">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9614e-139">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="9614e-140">Paketleri geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="9614e-140">Restore the packages.</span></span>

    ```console
    dotnet restore
    ```

6. <span data-ttu-id="9614e-141">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9614e-141">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="9614e-142">[Çalıştırma dotnet](/dotnet/core/tools/dotnet-run) gerekirse komut uygulamayı ilk olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9614e-142">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="9614e-143">konumuna gözatın `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9614e-143">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
