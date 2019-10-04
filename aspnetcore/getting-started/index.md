---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: 798f1ee87c05d886d8991e3f0230c8ebc6341ba8
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925097"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="0c966-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="0c966-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="0c966-104">Bu öğreticide, bir ASP.NET Core Web uygulaması oluşturmak ve çalıştırmak için .NET Core komut satırı arabirimi 'nin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="0c966-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="0c966-105">Şunları yapmayı öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="0c966-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0c966-106">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0c966-106">Create a web app project.</span></span>
> * <span data-ttu-id="0c966-107">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="0c966-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="0c966-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0c966-108">Run the app.</span></span>
> * <span data-ttu-id="0c966-109">Razor sayfasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="0c966-109">Edit a Razor page.</span></span>

<span data-ttu-id="0c966-110">Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="0c966-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="0c966-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0c966-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="0c966-113">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0c966-113">Create a web app project</span></span>

<span data-ttu-id="0c966-114">Bir komut kabuğu açın ve şu komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0c966-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="0c966-115">Önceki komut:</span><span class="sxs-lookup"><span data-stu-id="0c966-115">The preceding command:</span></span>

* <span data-ttu-id="0c966-116">Yeni bir Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="0c966-116">Creates a new web app.</span></span>  
* <span data-ttu-id="0c966-117">Parametresi, uygulama için kaynak dosyalarla *aspnetcoreapp* adlı bir dizin oluşturur. `-o aspnetcoreapp`</span><span class="sxs-lookup"><span data-stu-id="0c966-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="0c966-118">Geliştirme sertifikasına güven</span><span class="sxs-lookup"><span data-stu-id="0c966-118">Trust the development certificate</span></span>

<span data-ttu-id="0c966-119">HTTPS geliştirme sertifikasına güvenin:</span><span class="sxs-lookup"><span data-stu-id="0c966-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0c966-120">Windows</span><span class="sxs-lookup"><span data-stu-id="0c966-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="0c966-121">Yukarıdaki komutta aşağıdaki iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0c966-121">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

<span data-ttu-id="0c966-123">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="0c966-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0c966-124">macOS</span><span class="sxs-lookup"><span data-stu-id="0c966-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="0c966-125">Yukarıdaki komut aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0c966-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="0c966-126">*HTTPS geliştirme sertifikasına güvenmek istendi. Sertifika zaten güvenilmiyorsa, şu komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="0c966-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="0c966-127">Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="0c966-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="0c966-128">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="0c966-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0c966-129">Linux</span><span class="sxs-lookup"><span data-stu-id="0c966-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="0c966-130">Linux için Windows alt sistemi için bkz. [Linux Için Windows alt sistemi 'NDEN https sertifikasına güvenin](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="0c966-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="0c966-131">HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="0c966-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="0c966-132">Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="0c966-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="0c966-133">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0c966-133">Run the app</span></span>

<span data-ttu-id="0c966-134">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0c966-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="0c966-135">Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa, öğesine [https://localhost:5001](https://localhost:5001)gidin.</span><span class="sxs-lookup"><span data-stu-id="0c966-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="0c966-136">Razor sayfasını düzenleme</span><span class="sxs-lookup"><span data-stu-id="0c966-136">Edit a Razor page</span></span>

<span data-ttu-id="0c966-137">*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin ve kaydedin:</span><span class="sxs-lookup"><span data-stu-id="0c966-137">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="0c966-138">Sayfasına gidin [https://localhost:5001](https://localhost:5001), sayfayı yenileyin ve değişikliklerin görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0c966-138">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c966-139">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0c966-139">Next steps</span></span>

<span data-ttu-id="0c966-140">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="0c966-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0c966-141">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0c966-141">Create a web app project.</span></span>
> * <span data-ttu-id="0c966-142">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="0c966-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="0c966-143">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0c966-143">Run the project.</span></span>
> * <span data-ttu-id="0c966-144">Değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="0c966-144">Make a change.</span></span>

<span data-ttu-id="0c966-145">ASP.NET Core hakkında daha fazla bilgi edinmek için bkz. giriş bölümünde önerilen öğrenme yolu:</span><span class="sxs-lookup"><span data-stu-id="0c966-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
