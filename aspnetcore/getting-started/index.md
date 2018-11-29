---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/31/2018
uid: getting-started
ms.openlocfilehash: 5b5384b0bfa933f40f82513b02f7a14367fbef76
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549095"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="6f4a1-103">Öğretici: ASP.NET Core ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="6f4a1-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="6f4a1-104">Bu öğreticide, ASP.NET Core web uygulaması oluşturmak için .NET Core komut satırı arabirimi kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span> <span data-ttu-id="6f4a1-105">Öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6f4a1-106">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-106">Create a web app project.</span></span>
> * <span data-ttu-id="6f4a1-107">Yerel HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="6f4a1-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-108">Run the app.</span></span>
> * <span data-ttu-id="6f4a1-109">Bir Razor sayfası düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-109">Edit a Razor page.</span></span>

<span data-ttu-id="6f4a1-110">Sonunda, yerel makinenizde çalışan bir çalışan web uygulaması oluşturmuş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulama ana sayfası](_static/home-page.png)


## <a name="prerequisites"></a><span data-ttu-id="6f4a1-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6f4a1-112">Prerequisites</span></span>

* <span data-ttu-id="6f4a1-113">Yükleme [!INCLUDE [](~/includes/2.1-SDK.md)].</span><span class="sxs-lookup"><span data-stu-id="6f4a1-113">Install the [!INCLUDE [](~/includes/2.1-SDK.md)].</span></span>

## <a name="create-a-web-app-project"></a><span data-ttu-id="6f4a1-114">Bir web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="6f4a1-114">Create a web app project</span></span>

* <span data-ttu-id="6f4a1-115">Bir komut kabuğunu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-115">Open a command shell, and enter the following command:</span></span>

   ```console
   dotnet new webapp -o aspnetcoreapp
   ```

## <a name="enable-local-https"></a><span data-ttu-id="6f4a1-116">Yerel HTTPS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="6f4a1-116">Enable local HTTPS</span></span>

* <span data-ttu-id="6f4a1-117">HTTPS geliştirme sertifikasına güvenmek:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="6f4a1-118">Windows</span><span class="sxs-lookup"><span data-stu-id="6f4a1-118">Windows</span></span>](#tab/windows)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="6f4a1-119">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-119">The preceding command displays the following dialog:</span></span>

  ![Güvenlik Uyarısı iletişim kutusu](_static/cert.png)

  <span data-ttu-id="6f4a1-121">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="6f4a1-122">macOS</span><span class="sxs-lookup"><span data-stu-id="6f4a1-122">macOS</span></span>](#tab/macos)

  ```console
  dotnet dev-certs https --trust
  ```

  <span data-ttu-id="6f4a1-123">Yukarıdaki komut, şu iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-123">The preceding command displays the following message:</span></span>

  <span data-ttu-id="6f4a1-124">*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
  <span data-ttu-id="6f4a1-125">\* Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>
  
  <span data-ttu-id="6f4a1-126">Parola: \*</span><span class="sxs-lookup"><span data-stu-id="6f4a1-126">Password:\*</span></span>

  <span data-ttu-id="6f4a1-127">Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="6f4a1-128">Linux</span><span class="sxs-lookup"><span data-stu-id="6f4a1-128">Linux</span></span>](#tab/linux)

  <span data-ttu-id="6f4a1-129">HTTPS geliştirme sertifikasına güvenmek nasıl Linux dağıtımınız için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>
   
---

## <a name="run-the-app"></a><span data-ttu-id="6f4a1-130">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="6f4a1-130">Run the app</span></span>

* <span data-ttu-id="6f4a1-131">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-131">Run the following commands:</span></span>

   ```console
   cd aspnetcoreapp
   dotnet run
   ```

* <span data-ttu-id="6f4a1-132">Gözat [ https://localhost:5001 ](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="6f4a1-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="6f4a1-133">Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="6f4a1-134">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="6f4a1-135">Bir Razor sayfası Düzenle</span><span class="sxs-lookup"><span data-stu-id="6f4a1-135">Edit a Razor page</span></span>

* <span data-ttu-id="6f4a1-136">Açık *Pages/About.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-136">Open *Pages/About.cshtml* and modify the page with the following highlighted markup:</span></span>

   [!code-cshtml[](sample/getting-started/about.cshtml?highlight=9)]

* <span data-ttu-id="6f4a1-137">Gözat [ https://localhost:5001/About ](https://localhost:5001/About) ve değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-137">Browse to [https://localhost:5001/About](https://localhost:5001/About) and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6f4a1-138">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="6f4a1-138">Next steps</span></span>

<span data-ttu-id="6f4a1-139">Bu öğreticide şunları öğrendiniz: nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6f4a1-140">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-140">Create a web app project.</span></span>
> * <span data-ttu-id="6f4a1-141">Yerel HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="6f4a1-142">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-142">Run the project.</span></span>
> * <span data-ttu-id="6f4a1-143">Bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-143">Make a change.</span></span>

<span data-ttu-id="6f4a1-144">ASP.NET Core hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="6f4a1-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>



> [!NOTE]
> <span data-ttu-id="6f4a1-145">ASP.NET Core içindekiler tablosuna yönelik önerilmiş olan yeni bir yapının kullanılabilirliğini test ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="6f4a1-145">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="6f4a1-146">Geçerli veya önerilen içindekiler tablosunda 7 farklı konuyu bulmaya ilişkin alıştırmayı denemek için vaktiniz varsa lütfen [çalışmaya katılmak için buraya tıklayın](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="6f4a1-146">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>