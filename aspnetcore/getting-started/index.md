---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/15/2019
uid: getting-started
ms.openlocfilehash: d1edf91f1b37ba2b69732471dc6c1f306ac5ad24
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081116"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="401db-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="401db-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="401db-104">Bu öğreticide, bir ASP.NET Core Web uygulaması oluşturmak ve çalıştırmak için .NET Core komut satırı arabirimi 'nin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="401db-104">This tutorial shows how to use the .NET Core command-line interface to create and run an ASP.NET Core web app.</span></span>

<span data-ttu-id="401db-105">Şunları yapmayı öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="401db-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="401db-106">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="401db-106">Create a web app project.</span></span>
> * <span data-ttu-id="401db-107">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="401db-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="401db-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="401db-108">Run the app.</span></span>
> * <span data-ttu-id="401db-109">Razor sayfasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="401db-109">Edit a Razor page.</span></span>

<span data-ttu-id="401db-110">Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="401db-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="401db-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="401db-112">Prerequisites</span></span>

* [<span data-ttu-id="401db-113">.NET Core 2,2 SDK</span><span class="sxs-lookup"><span data-stu-id="401db-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="401db-114">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="401db-114">Create a web app project</span></span>

<span data-ttu-id="401db-115">Bir komut kabuğu açın ve şu komutu girin:</span><span class="sxs-lookup"><span data-stu-id="401db-115">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a><span data-ttu-id="401db-116">Geliştirme sertifikasına güven</span><span class="sxs-lookup"><span data-stu-id="401db-116">Trust the development certificate</span></span>

<span data-ttu-id="401db-117">HTTPS geliştirme sertifikasına güvenin:</span><span class="sxs-lookup"><span data-stu-id="401db-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="401db-118">Windows</span><span class="sxs-lookup"><span data-stu-id="401db-118">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="401db-119">Yukarıdaki komutta aşağıdaki iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="401db-119">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

<span data-ttu-id="401db-121">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="401db-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="401db-122">macOS</span><span class="sxs-lookup"><span data-stu-id="401db-122">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="401db-123">Yukarıdaki komut aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="401db-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="401db-124">*HTTPS geliştirme sertifikasına güvenmek istendi. Sertifika zaten güvenilmiyorsa, şu komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="401db-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="401db-125">Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="401db-125">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="401db-126">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="401db-126">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="401db-127">Linux</span><span class="sxs-lookup"><span data-stu-id="401db-127">Linux</span></span>](#tab/linux)

<span data-ttu-id="401db-128">Linux için Windows alt sistemi için bkz. [Linux Için Windows alt sistemi 'NDEN https sertifikasına güvenin](xref:security/enforcing-ssl#wsl).</span><span class="sxs-lookup"><span data-stu-id="401db-128">For Windows Subsystem for Linux, see [Trust HTTPS certificate from Windows Subsystem for Linux](xref:security/enforcing-ssl#wsl).</span></span>

<span data-ttu-id="401db-129">HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="401db-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="401db-130">Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="401db-130">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="401db-131">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="401db-131">Run the app</span></span>

<span data-ttu-id="401db-132">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="401db-132">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="401db-133">Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa, öğesine [https://localhost:5001](https://localhost:5001)gidin.</span><span class="sxs-lookup"><span data-stu-id="401db-133">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="401db-134">Gizlilik ve tanımlama bilgisi ilkesini kabul etmek için **kabul et** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="401db-134">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="401db-135">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="401db-135">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="401db-136">Razor sayfasını düzenleme</span><span class="sxs-lookup"><span data-stu-id="401db-136">Edit a Razor page</span></span>

<span data-ttu-id="401db-137">*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="401db-137">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="401db-138">[https://localhost:5001](https://localhost:5001)' A gidin ve değişikliklerin görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="401db-138">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="401db-139">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="401db-139">Next steps</span></span>

<span data-ttu-id="401db-140">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="401db-140">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="401db-141">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="401db-141">Create a web app project.</span></span>
> * <span data-ttu-id="401db-142">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="401db-142">Trust the development certificate.</span></span>
> * <span data-ttu-id="401db-143">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="401db-143">Run the project.</span></span>
> * <span data-ttu-id="401db-144">Değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="401db-144">Make a change.</span></span>

<span data-ttu-id="401db-145">ASP.NET Core hakkında daha fazla bilgi edinmek için bkz. giriş bölümünde önerilen öğrenme yolu:</span><span class="sxs-lookup"><span data-stu-id="401db-145">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
