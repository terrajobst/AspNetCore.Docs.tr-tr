---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 116a22bce80257948bfcc02c03a74a4b5568b8b5
ms.sourcegitcommit: 4649814d1ae32248419da4e8f8242850fd8679a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2019
ms.locfileid: "71975686"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="1dbbb-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="1dbbb-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="1dbbb-104">Bu öğreticide, bir ASP.NET Core Web uygulaması oluşturmak ve çalıştırmak için .NET Core komut satırı arabirimi 'nin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="1dbbb-105">Şunları yapmayı öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1dbbb-106">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-106">Create a web app project.</span></span>
> * <span data-ttu-id="1dbbb-107">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="1dbbb-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-108">Run the app.</span></span>
> * <span data-ttu-id="1dbbb-109">Razor sayfasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-109">Edit a Razor page.</span></span>

<span data-ttu-id="1dbbb-110">Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="1dbbb-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="1dbbb-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="1dbbb-113">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="1dbbb-113">Create a web app project</span></span>

<span data-ttu-id="1dbbb-114">Bir komut kabuğu açın ve şu komutu girin:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="1dbbb-115">Önceki komut:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-115">The preceding command:</span></span>

* <span data-ttu-id="1dbbb-116">Yeni bir Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-116">Creates a new web app.</span></span>  
* <span data-ttu-id="1dbbb-117">@No__t-0 parametresi, uygulama için kaynak dosyalarla *aspnetcoreapp* adlı bir dizin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="1dbbb-118">Geliştirme sertifikasına güven</span><span class="sxs-lookup"><span data-stu-id="1dbbb-118">Trust the development certificate</span></span>

<span data-ttu-id="1dbbb-119">HTTPS geliştirme sertifikasına güvenin:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1dbbb-120">Windows</span><span class="sxs-lookup"><span data-stu-id="1dbbb-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="1dbbb-121">Yukarıdaki komutta aşağıdaki iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-121">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

<span data-ttu-id="1dbbb-123">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="1dbbb-124">macOS</span><span class="sxs-lookup"><span data-stu-id="1dbbb-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="1dbbb-125">Yukarıdaki komut aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="1dbbb-126">@no__t-HTTPS geliştirme sertifikası istendi. Sertifika zaten güvenilmiyorsa, şu komutu çalıştıracağız: \* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="1dbbb-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="1dbbb-127">Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="1dbbb-128">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="1dbbb-129">Linux</span><span class="sxs-lookup"><span data-stu-id="1dbbb-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="1dbbb-130">HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-130">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="1dbbb-131">Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="1dbbb-131">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="1dbbb-132">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="1dbbb-132">Run the app</span></span>

<span data-ttu-id="1dbbb-133">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-133">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="1dbbb-134">Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa [https://localhost:5001](https://localhost:5001)' e gidin.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-134">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="1dbbb-135">Razor sayfasını düzenleme</span><span class="sxs-lookup"><span data-stu-id="1dbbb-135">Edit a Razor page</span></span>

<span data-ttu-id="1dbbb-136">*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin ve kaydedin:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-136">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="1dbbb-137">[@No__t-1](https://localhost:5001)' e gidin, sayfayı yenileyin ve değişikliklerin görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-137">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1dbbb-138">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="1dbbb-138">Next steps</span></span>

<span data-ttu-id="1dbbb-139">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1dbbb-140">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-140">Create a web app project.</span></span>
> * <span data-ttu-id="1dbbb-141">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-141">Trust the development certificate.</span></span>
> * <span data-ttu-id="1dbbb-142">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-142">Run the project.</span></span>
> * <span data-ttu-id="1dbbb-143">Değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="1dbbb-143">Make a change.</span></span>

<span data-ttu-id="1dbbb-144">ASP.NET Core hakkında daha fazla bilgi edinmek için bkz. giriş bölümünde önerilen öğrenme yolu:</span><span class="sxs-lookup"><span data-stu-id="1dbbb-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
