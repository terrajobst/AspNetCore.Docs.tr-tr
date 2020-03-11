---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: 047fd7a74d3d53f68a730d67b63c65fe6bda529f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658468"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="a2fb5-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="a2fb5-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="a2fb5-104">Bu öğreticide, .NET Core CLI kullanılarak ASP.NET Core bir Web uygulamasının nasıl oluşturulacağı ve çalıştırılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-104">This tutorial shows how to create and run an ASP.NET Core web app using the .NET Core CLI.</span></span>

<span data-ttu-id="a2fb5-105">Şunları öğrenirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a2fb5-106">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-106">Create a web app project.</span></span>
> * <span data-ttu-id="a2fb5-107">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-107">Trust the development certificate.</span></span>
> * <span data-ttu-id="a2fb5-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-108">Run the app.</span></span>
> * <span data-ttu-id="a2fb5-109">Razor sayfasını düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-109">Edit a Razor page.</span></span>

<span data-ttu-id="a2fb5-110">Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="a2fb5-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="a2fb5-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a><span data-ttu-id="a2fb5-113">Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="a2fb5-113">Create a web app project</span></span>

<span data-ttu-id="a2fb5-114">Bir komut kabuğu açın ve şu komutu girin:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-114">Open a command shell, and enter the following command:</span></span>

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

<span data-ttu-id="a2fb5-115">Önceki komut:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-115">The preceding command:</span></span>

* <span data-ttu-id="a2fb5-116">Yeni bir Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-116">Creates a new web app.</span></span>  
* <span data-ttu-id="a2fb5-117">`-o aspnetcoreapp` parametresi, uygulama için kaynak dosyalarla *aspnetcoreapp* adlı bir dizin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-117">The `-o aspnetcoreapp` parameter creates a directory named *aspnetcoreapp* with the source files for the app.</span></span>

### <a name="trust-the-development-certificate"></a><span data-ttu-id="a2fb5-118">Geliştirme sertifikasına güven</span><span class="sxs-lookup"><span data-stu-id="a2fb5-118">Trust the development certificate</span></span>

<span data-ttu-id="a2fb5-119">HTTPS geliştirme sertifikasına güvenin:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-119">Trust the HTTPS development certificate:</span></span>

# <a name="windows"></a>[<span data-ttu-id="a2fb5-120">Windows</span><span class="sxs-lookup"><span data-stu-id="a2fb5-120">Windows</span></span>](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="a2fb5-121">Yukarıdaki komutta aşağıdaki iletişim kutusu görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-121">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

<span data-ttu-id="a2fb5-123">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız **Evet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-123">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macos"></a>[<span data-ttu-id="a2fb5-124">macOS</span><span class="sxs-lookup"><span data-stu-id="a2fb5-124">macOS</span></span>](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="a2fb5-125">Yukarıdaki komut aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-125">The preceding command displays the following message:</span></span>

<span data-ttu-id="a2fb5-126">*Https geliştirme sertifikasına güvenmek istendi. Sertifikaya zaten güvenilmiyorsa, şu komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span><span class="sxs-lookup"><span data-stu-id="a2fb5-126">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted, we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`</span></span>

<span data-ttu-id="a2fb5-127">Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-127">This command might prompt you for your password to install the certificate on the system keychain.</span></span> <span data-ttu-id="a2fb5-128">Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-128">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linux"></a>[<span data-ttu-id="a2fb5-129">Linux</span><span class="sxs-lookup"><span data-stu-id="a2fb5-129">Linux</span></span>](#tab/linux)

<span data-ttu-id="a2fb5-130">HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-130">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

<span data-ttu-id="a2fb5-131">Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span><span class="sxs-lookup"><span data-stu-id="a2fb5-131">For more information, see [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)</span></span>

## <a name="run-the-app"></a><span data-ttu-id="a2fb5-132">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="a2fb5-132">Run the app</span></span>

<span data-ttu-id="a2fb5-133">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-133">Run the following commands:</span></span>

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

<span data-ttu-id="a2fb5-134">Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa, [https://localhost:5001](https://localhost:5001)gidin.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-134">After the command shell indicates that the app has started, browse to [https://localhost:5001](https://localhost:5001).</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="a2fb5-135">Razor sayfasını düzenleme</span><span class="sxs-lookup"><span data-stu-id="a2fb5-135">Edit a Razor page</span></span>

<span data-ttu-id="a2fb5-136">*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin ve kaydedin:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-136">Open *Pages/Index.cshtml* and modify and save the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="a2fb5-137">[https://localhost:5001](https://localhost:5001)gidin, sayfayı yenileyin ve değişikliklerin görüntülendiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-137">Browse to [https://localhost:5001](https://localhost:5001), refresh the page, and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2fb5-138">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="a2fb5-138">Next steps</span></span>

<span data-ttu-id="a2fb5-139">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a2fb5-140">Bir Web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-140">Create a web app project.</span></span>
> * <span data-ttu-id="a2fb5-141">Geliştirme sertifikasına güvenin.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-141">Trust the development certificate.</span></span>
> * <span data-ttu-id="a2fb5-142">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-142">Run the project.</span></span>
> * <span data-ttu-id="a2fb5-143">Değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="a2fb5-143">Make a change.</span></span>

<span data-ttu-id="a2fb5-144">ASP.NET Core hakkında daha fazla bilgi edinmek için bkz. giriş bölümünde önerilen öğrenme yolu:</span><span class="sxs-lookup"><span data-stu-id="a2fb5-144">To learn more about ASP.NET Core, see the recommended learning path in the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
