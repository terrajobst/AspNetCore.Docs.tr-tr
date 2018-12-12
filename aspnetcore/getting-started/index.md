---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: cf9e731f7638687b3f40b42864ef7ee8f5522b39
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284363"
---
# <a name="tutorial-get-started-with-aspnet-core"></a><span data-ttu-id="0f878-103">Öğretici: ASP.NET Core kullanmaya başlayın</span><span class="sxs-lookup"><span data-stu-id="0f878-103">Tutorial: Get started with ASP.NET Core</span></span>

<span data-ttu-id="0f878-104">Bu öğreticide, ASP.NET Core web uygulaması oluşturmak için .NET Core komut satırı arabirimi kullanmayı gösterir.</span><span class="sxs-lookup"><span data-stu-id="0f878-104">This tutorial shows how to use the .NET Core command-line interface to create an ASP.NET Core web app.</span></span>

<span data-ttu-id="0f878-105">Öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="0f878-105">You'll learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f878-106">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0f878-106">Create a web app project.</span></span>
> * <span data-ttu-id="0f878-107">Yerel HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0f878-107">Enable local HTTPS.</span></span>
> * <span data-ttu-id="0f878-108">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0f878-108">Run the app.</span></span>
> * <span data-ttu-id="0f878-109">Bir Razor sayfası düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="0f878-109">Edit a Razor page.</span></span>

<span data-ttu-id="0f878-110">Sonunda, yerel makinenizde çalışan bir çalışan web uygulaması oluşturmuş olacaksınız.</span><span class="sxs-lookup"><span data-stu-id="0f878-110">At the end, you'll have a working web app running on your local machine.</span></span>

![Web uygulama ana sayfası](_static/home-page.png)

## <a name="prerequisites"></a><span data-ttu-id="0f878-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="0f878-112">Prerequisites</span></span>

* [<span data-ttu-id="0f878-113">.NET core SDK'sını 2.2</span><span class="sxs-lookup"><span data-stu-id="0f878-113">.NET Core 2.2 SDK</span></span>](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a><span data-ttu-id="0f878-114">Bir web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="0f878-114">Create a web app project</span></span>

<span data-ttu-id="0f878-115">Bir komut kabuğunu açın ve aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="0f878-115">Open a command shell, and enter the following command:</span></span>

```console
dotnet new webapp -o aspnetcoreapp
```

## <a name="enable-local-https"></a><span data-ttu-id="0f878-116">Yerel HTTPS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="0f878-116">Enable local HTTPS</span></span>

<span data-ttu-id="0f878-117">HTTPS geliştirme sertifikasına güvenmek:</span><span class="sxs-lookup"><span data-stu-id="0f878-117">Trust the HTTPS development certificate:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="0f878-118">Windows</span><span class="sxs-lookup"><span data-stu-id="0f878-118">Windows</span></span>](#tab/windows)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="0f878-119">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0f878-119">The preceding command displays the following dialog:</span></span>

![Güvenlik Uyarısı iletişim kutusu](_static/cert.png)

<span data-ttu-id="0f878-121">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="0f878-121">Select **Yes** if you agree to trust the development certificate.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="0f878-122">macOS</span><span class="sxs-lookup"><span data-stu-id="0f878-122">macOS</span></span>](#tab/macos)

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="0f878-123">Yukarıdaki komut, şu iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="0f878-123">The preceding command displays the following message:</span></span>

<span data-ttu-id="0f878-124">*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span><span class="sxs-lookup"><span data-stu-id="0f878-124">*Trusting the HTTPS development certificate was requested. If the certificate is not already trusted we will run the following command:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.</span></span>  
<span data-ttu-id="0f878-125">\* Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir.</span><span class="sxs-lookup"><span data-stu-id="0f878-125">\*This command might prompt you for your password to install the certificate on the system keychain.</span></span>

<span data-ttu-id="0f878-126">Parola: \*</span><span class="sxs-lookup"><span data-stu-id="0f878-126">Password:\*</span></span>

<span data-ttu-id="0f878-127">Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.</span><span class="sxs-lookup"><span data-stu-id="0f878-127">Enter your password if you agree to trust the development certificate.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="0f878-128">Linux</span><span class="sxs-lookup"><span data-stu-id="0f878-128">Linux</span></span>](#tab/linux)

<span data-ttu-id="0f878-129">HTTPS geliştirme sertifikasına güvenmek nasıl Linux dağıtımınız için belgelere bakın.</span><span class="sxs-lookup"><span data-stu-id="0f878-129">See the documentation for your Linux distribution on how to trust the HTTPS development certificate.</span></span>

---

## <a name="run-the-app"></a><span data-ttu-id="0f878-130">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="0f878-130">Run the app</span></span>

<span data-ttu-id="0f878-131">Aşağıdaki komutları çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="0f878-131">Run the following commands:</span></span>

```console
cd aspnetcoreapp
dotnet run
```

<span data-ttu-id="0f878-132">Gözat [ https://localhost:5001 ](https://localhost:5001).</span><span class="sxs-lookup"><span data-stu-id="0f878-132">Browse to [https://localhost:5001](https://localhost:5001).</span></span> <span data-ttu-id="0f878-133">Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için.</span><span class="sxs-lookup"><span data-stu-id="0f878-133">Click **Accept** to accept the privacy and cookie policy.</span></span> <span data-ttu-id="0f878-134">Bu uygulama, kişisel bilgileri tutmak değil.</span><span class="sxs-lookup"><span data-stu-id="0f878-134">This app doesn't keep personal information.</span></span>

## <a name="edit-a-razor-page"></a><span data-ttu-id="0f878-135">Bir Razor sayfası Düzenle</span><span class="sxs-lookup"><span data-stu-id="0f878-135">Edit a Razor page</span></span>

<span data-ttu-id="0f878-136">Açık *Pages/Index.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="0f878-136">Open *Pages/Index.cshtml* and modify the page with the following highlighted markup:</span></span>

[!code-cshtml[](sample/index.cshtml?highlight=9)]

<span data-ttu-id="0f878-137">Gözat [ https://localhost:5001 ](https://localhost:5001), değişiklikleri görüntülenir doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="0f878-137">Browse to [https://localhost:5001](https://localhost:5001), and verify the changes are displayed.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f878-138">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="0f878-138">Next steps</span></span>

<span data-ttu-id="0f878-139">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="0f878-139">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="0f878-140">Bir web uygulaması projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="0f878-140">Create a web app project.</span></span>
> * <span data-ttu-id="0f878-141">Yerel HTTPS etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="0f878-141">Enable local HTTPS.</span></span>
> * <span data-ttu-id="0f878-142">Projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="0f878-142">Run the project.</span></span>
> * <span data-ttu-id="0f878-143">Bir değişiklik yapın.</span><span class="sxs-lookup"><span data-stu-id="0f878-143">Make a change.</span></span>

<span data-ttu-id="0f878-144">ASP.NET Core hakkında daha fazla bilgi için girişine bakın:</span><span class="sxs-lookup"><span data-stu-id="0f878-144">To learn more about ASP.NET Core, see the introduction:</span></span>

> [!div class="nextstepaction"]
> <xref:index>

> [!NOTE]
> <span data-ttu-id="0f878-145">ASP.NET Core içindekiler için önerilen ve yeni bir yapıyı kullanılabilirliğini test ediyoruz.</span><span class="sxs-lookup"><span data-stu-id="0f878-145">We're testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span> <span data-ttu-id="0f878-146">Bulma yedi farklı konular, geçerli ya da önerilen içindekiler bölümünden bir alıştırma denemek için birkaç dakikanız varsa lütfen [çalışmada katılmak için burayı tıklatın](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="0f878-146">If you have a few minutes to try an exercise of finding seven different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
