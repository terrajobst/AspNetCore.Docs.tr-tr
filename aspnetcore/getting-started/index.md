---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 5/15/2019
uid: getting-started
ms.openlocfilehash: 9227dcfbc84376d9d73bc6fc0dd76085779acae1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610317"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="42ca9-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="42ca9-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="42ca9-104">Bu öğreticide, .NET Core komut satırı arabirimi oluşturmak ve bir ASP.NET Core web uygulaması çalıştırmak için kullanılacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="42ca9-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="42ca9-105">Öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="42ca9-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42ca9-106">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="42ca9-106">Create a web app project.</span></span>
> * <span data-ttu-id="42ca9-107">Geliştirme sertifikasına güvenmek.</span><span class="sxs-lookup"><span data-stu-id="42ca9-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="42ca9-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="42ca9-108">Run the app.</span></span>
> * <span data-ttu-id="42ca9-109">Bir Razor sayfası düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="42ca9-109">Edit a Razor page.</span></span>

<span data-ttu-id="42ca9-110">Sonunda, yerel makinenizde çalışan bir çalışan web uygulaması oluşturmuş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="42ca9-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulama ana sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="42ca9-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="42ca9-112">Prerequisites</span></span>

* [<span data-ttu-id="42ca9-113">.NET core SDK'sını 2.2</span><span class="sxs-lookup"><span data-stu-id="42ca9-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="42ca9-114">Bir web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="42ca9-114">Create a web app project</span></span>

<span data-ttu-id="42ca9-115">Bir komut kabuğunu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="42ca9-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="42ca9-116">Geliştirme sertifikasına güvenmek</span><span class="sxs-lookup"><span data-stu-id="42ca9-116">Trust the development certificate</span></span>

<span data-ttu-id="42ca9-117">HTTPS geliştirme sertifikasına güvenmek:</span><span class="sxs-lookup"><span data-stu-id="42ca9-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="42ca9-118">Windows</span><span class="sxs-lookup"><span data-stu-id="42ca9-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="42ca9-119">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="42ca9-119">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

<span data-ttu-id="42ca9-121">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="42ca9-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="42ca9-122">macOS</span><span class="sxs-lookup"><span data-stu-id="42ca9-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="42ca9-123">Yukarıdaki komut, şu iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="42ca9-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="42ca9-124">*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="42ca9-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="42ca9-125">Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="42ca9-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="42ca9-126">Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="42ca9-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="42ca9-127">Linux</span><span class="sxs-lookup"><span data-stu-id="42ca9-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="42ca9-128">Linux için Windows alt sistemi için bkz: [Linux için Windows alt sistemi güven HTTPS sertifikadan](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="42ca9-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="42ca9-129">HTTPS geliştirme sertifikasına güvenmek nasıl Linux dağıtımınız için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="42ca9-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="42ca9-130">Daha fazla bilgi için [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="42ca9-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="42ca9-131">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="42ca9-131">Run the app</span></span>

<span data-ttu-id="42ca9-132">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="42ca9-132">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="42ca9-133">Komut kabuğu uygulama başladığını belirtir sonra Gözat [ https://localhost:5001 ](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="42ca9-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="42ca9-134">Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="42ca9-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="42ca9-135">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="42ca9-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="42ca9-136">Bir Razor sayfası Düzenle</span><span class="sxs-lookup"><span data-stu-id="42ca9-136">Edit a Razor page</span></span>

<span data-ttu-id="42ca9-137">Açık *Pages/Index.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="42ca9-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="42ca9-138">Gözat [ https://localhost:5001 ](https://localhost:5001), değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="42ca9-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42ca9-139">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="42ca9-139">Next steps</span></span>

<span data-ttu-id="42ca9-140">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="42ca9-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="42ca9-141">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="42ca9-141">Create a web app project.</span></span>
> * <span data-ttu-id="42ca9-142">Geliştirme sertifikasına güvenmek.</span><span class="sxs-lookup"><span data-stu-id="42ca9-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="42ca9-143">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="42ca9-143">Run the project.</span></span>
> * <span data-ttu-id="42ca9-144">Bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="42ca9-144">Make a change.</span></span>

<span data-ttu-id="42ca9-145">ASP.NET Core hakkında daha fazla bilgi için önerilen öğrenme yolunu giriş bakın:</span><span class="sxs-lookup"><span data-stu-id="42ca9-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
