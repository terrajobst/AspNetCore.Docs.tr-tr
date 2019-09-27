---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 09/22/2019
uid: getting-started
ms.openlocfilehash: c9cd5e05f52c8bdefa931adc654087dac91e2f05
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317758"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="cf330-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="cf330-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="cf330-104">Bu öğreticide, bir ASP.NET Core Web uygulaması oluşturmak ve çalıştırmak için .NET Core komut satırı arabirimi 'nin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="cf330-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="cf330-105">Şunları yapmayı öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="cf330-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cf330-106">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cf330-106">Create a web app project.</span></span>
> * <span data-ttu-id="cf330-107">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="cf330-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="cf330-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cf330-108">Run the app.</span></span>
> * <span data-ttu-id="cf330-109">Razor sayfasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="cf330-109">Edit a Razor page.</span></span>

<span data-ttu-id="cf330-110">Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="cf330-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="cf330-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="cf330-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.0-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="cf330-113">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="cf330-113">Create a web app project</span></span>

<span data-ttu-id="cf330-114">Bir komut kabuğu açın ve şu komutu girin:</span><span class="sxs-lookup"><span data-stu-id="cf330-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="cf330-115">Önceki komut:</span><span class="sxs-lookup"><span data-stu-id="cf330-115">The preceding command:</span></span>

* <span data-ttu-id="cf330-116">Yeni bir Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="cf330-116">Creates a new web app.</span></span>  
* <span data-ttu-id="cf330-117">Parametresi, uygulama için kaynak dosyalarla *aspnetcoreapp* adlı bir dizin oluşturur. `-o`</span><span class="sxs-lookup"><span data-stu-id="cf330-117">The `-o` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="cf330-118">Geliştirme sertifikasına güven</span><span class="sxs-lookup"><span data-stu-id="cf330-118">Trust the development certificate</span></span>

<span data-ttu-id="cf330-119">HTTPS geliştirme sertifikasına güvenin:</span><span class="sxs-lookup"><span data-stu-id="cf330-119">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="cf330-120">Windows</span><span class="sxs-lookup"><span data-stu-id="cf330-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="cf330-121">Yukarıdaki komutta aşağıdaki iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="cf330-121">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

<span data-ttu-id="cf330-123">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="cf330-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="cf330-124">macOS</span><span class="sxs-lookup"><span data-stu-id="cf330-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="cf330-125">Yukarıdaki komut aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="cf330-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="cf330-126">*HTTPS geliştirme sertifikasına güvenmek istendi. Sertifika zaten güvenilmiyorsa, şu komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="cf330-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="cf330-127">Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="cf330-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="cf330-128">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="cf330-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="cf330-129">Linux</span><span class="sxs-lookup"><span data-stu-id="cf330-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="cf330-130">Linux için Windows alt sistemi için bkz. [Linux Için Windows alt sistemi 'NDEN https sertifikasına güvenin](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="cf330-130">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="cf330-131">HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="cf330-131">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="cf330-132">Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="cf330-132">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="cf330-133">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="cf330-133">Run the app</span></span>

<span data-ttu-id="cf330-134">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="cf330-134">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="cf330-135">Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa, öğesine [https://localhost:5001](https://localhost:5001)gidin.</span><span class="sxs-lookup"><span data-stu-id="cf330-135">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="cf330-136">Gizlilik ve tanımlama bilgisi ilkesini kabul etmek için **kabul et** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="cf330-136">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="cf330-137">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="cf330-137">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="cf330-138">Razor sayfasını düzenleme</span><span class="sxs-lookup"><span data-stu-id="cf330-138">Edit a Razor page</span></span>

<span data-ttu-id="cf330-139">*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin ve kaydedin:</span><span class="sxs-lookup"><span data-stu-id="cf330-139">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="cf330-140">Sayfasına gidin [https://localhost:5001](https://localhost:5001), sayfayı yenileyin ve değişikliklerin görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="cf330-140">Browse to [https://localhost:5001](https://localhost:5001), refresh the page and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf330-141">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="cf330-141">Next steps</span></span>

<span data-ttu-id="cf330-142">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="cf330-142">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cf330-143">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cf330-143">Create a web app project.</span></span>
> * <span data-ttu-id="cf330-144">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="cf330-144">Trust the development certificate.</span></span>
> * <span data-ttu-id="cf330-145">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="cf330-145">Run the project.</span></span>
> * <span data-ttu-id="cf330-146">Değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="cf330-146">Make a change.</span></span>

<span data-ttu-id="cf330-147">ASP.NET Core hakkında daha fazla bilgi edinmek için bkz. giriş bölümünde önerilen öğrenme yolu:</span><span class="sxs-lookup"><span data-stu-id="cf330-147">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
