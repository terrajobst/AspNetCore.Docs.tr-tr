---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: a6a5023594aec01370143e7d1f35fb45c109122a
ms.sourcegitcommit: 13940eb53c68664b11a2d685ee17c78faab1945d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2018
ms.locfileid: "47860946"
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="becc5-103">ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="becc5-103">Get started with ASP.NET Core</span></span>

<span data-ttu-id="becc5-104">Bu belge, oluşturma ve ASP.NET Core uygulaması çalıştırmak için adımlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="becc5-104">This document provides steps for creating and running an ASP.NET Core app.</span></span>

::: moniker range=">= aspnetcore-2.1"

1. <span data-ttu-id="becc5-105">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="becc5-105">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

2. <span data-ttu-id="becc5-106">Bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="becc5-106">Create an ASP.NET Core project.</span></span> <span data-ttu-id="becc5-107">Bir komut kabuğunu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="becc5-107">Open a command shell and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

3. <span data-ttu-id="becc5-108">HTTPS geliştirme sertifikasına güvenmek:</span><span class="sxs-lookup"><span data-stu-id="becc5-108">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="becc5-109">Windows</span><span class="sxs-lookup"><span data-stu-id="becc5-109">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="becc5-110">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="becc5-110">The preceding command displays the following dialog:</span></span>

  ![Güvenlik Uyarısı iletişim kutusu](_static/cert.png)

  <span data-ttu-id="becc5-112">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="becc5-112">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="becc5-113">macOS</span><span class="sxs-lookup"><span data-stu-id="becc5-113">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="becc5-114">Yukarıdaki komut, şu iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="becc5-114">The preceding command displays the following message:</span></span>

  <span data-ttu-id="becc5-115">*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="becc5-115">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="becc5-116">\* Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="becc5-116">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="becc5-117">Parola: \*</span><span class="sxs-lookup"><span data-stu-id="becc5-117">Password:\*</span></span>

  <span data-ttu-id="becc5-118">Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="becc5-118">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="becc5-119">Linux</span><span class="sxs-lookup"><span data-stu-id="becc5-119">Linux</span></span>](#tab/linux)

  <span data-ttu-id="becc5-120">HTTPS geliştirme sertifikasına güvenmek nasıl Linux dağıtımınız için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="becc5-120">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

4. <span data-ttu-id="becc5-121">Uygulamayı çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="becc5-121">Run the app:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

5. <span data-ttu-id="becc5-122">Gözat [ http://localhost:5001 ](http://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="becc5-122">Browse to [http://localhost:5001](http://localhost:5001).</span></span>  <span data-ttu-id="becc5-123">Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="becc5-123">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="becc5-124">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="becc5-124">This app doesn't keep personal information.</span></span>

6. <span data-ttu-id="becc5-125">Açık *Pages/About.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="becc5-125">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

7. <span data-ttu-id="becc5-126">Gözat [ http://localhost:5001/About ](http://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="becc5-126">Browse to [http://localhost:5001/About](http://localhost:5001/About) and verify the changes are displayed.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

1. <span data-ttu-id="becc5-127">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="becc5-127">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="becc5-128">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="becc5-128">Create a new ASP.NET Core project.</span></span>

   <span data-ttu-id="becc5-129">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="becc5-129">Open a command shell.</span></span> <span data-ttu-id="becc5-130">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="becc5-130">Enter the following command:</span></span>

   ```console
   dotnet new razor -o aspnetcoreapp
   ```

3. <span data-ttu-id="becc5-131">Uygulama ile aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="becc5-131">Run the app with the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

4. <span data-ttu-id="becc5-132">Gözat [ http://localhost:5000 ](http://localhost:5000).</span><span class="sxs-lookup"><span data-stu-id="becc5-132">Browse to [http://localhost:5000](http://localhost:5000).</span></span>

5. <span data-ttu-id="becc5-133">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="becc5-133">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="becc5-134">Sunucusundaki zamandır @DateTime.Now":</span><span class="sxs-lookup"><span data-stu-id="becc5-134">The time on the server is @DateTime.Now":</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="becc5-135">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="becc5-135">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

1. <span data-ttu-id="becc5-136">.NET Core'u yükleme **SDK yükleyicisi** 1.0.4 SDK'dan için [.NET Core tüm indirmeler sayfasına](https://www.microsoft.com/net/download/all).</span><span class="sxs-lookup"><span data-stu-id="becc5-136">Install the .NET Core **SDK Installer** for SDK 1.0.4 from the [.NET Core All Downloads page](https://www.microsoft.com/net/download/all).</span></span>

2. <span data-ttu-id="becc5-137">Yeni bir ASP.NET Core projesi için bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="becc5-137">Create a folder for a new ASP.NET Core project.</span></span>

   <span data-ttu-id="becc5-138">Bir komut kabuğunu açın.</span><span class="sxs-lookup"><span data-stu-id="becc5-138">Open a command shell.</span></span> <span data-ttu-id="becc5-139">Aşağıdaki komutları girin:</span><span class="sxs-lookup"><span data-stu-id="becc5-139">Enter the following commands:</span></span>

   ```console
   mkdir aspnetcoreapp
   cd aspnetcoreapp
   ```

3. <span data-ttu-id="becc5-140">Makinenizde bir sonraki SDK sürümü yüklü değilse, oluşturun bir *global.json* 1.0.4 seçmek için dosya SDK.</span><span class="sxs-lookup"><span data-stu-id="becc5-140">If you have installed a later SDK version on your machine, create a *global.json* file to select the 1.0.4 SDK.</span></span>

   ```json
   {
     "sdk": { "version": "1.0.4" }
   }
   ```

4. <span data-ttu-id="becc5-141">Yeni bir ASP.NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="becc5-141">Create a new ASP.NET Core project.</span></span>

   ```console
   dotnet new web
   ```

5. <span data-ttu-id="becc5-142">Paketleri geri yükleyin.</span><span class="sxs-lookup"><span data-stu-id="becc5-142">Restore the packages.</span></span>

   ```console
   dotnet restore
   ```

6. <span data-ttu-id="becc5-143">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="becc5-143">Run the app.</span></span>

   ```console
   dotnet run
   ```

   <span data-ttu-id="becc5-144">[Çalıştırma dotnet](/dotnet/core/tools/dotnet-run) gerekirse komut uygulamayı ilk olarak oluşturur.</span><span class="sxs-lookup"><span data-stu-id="becc5-144">The [dotnet run](/dotnet/core/tools/dotnet-run) command builds the app first, if needed.</span></span>

7. <span data-ttu-id="becc5-145">konumuna gözatın `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="becc5-145">Browse to `http://localhost:5000`.</span></span>

[!INCLUDE [next steps](~/includes/getting-started/next-steps.md)]

::: moniker-end
