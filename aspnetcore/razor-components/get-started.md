---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenleri framework ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/get-started
ms.openlocfilehash: c83af10fd84bc8238f5fe20c66b91ba17de80ae3
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668158"
---
# <a name="get-started-with-razor-components"></a><span data-ttu-id="042f3-103">Razor bileşenleri ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="042f3-103">Get started with Razor Components</span></span>

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="setup"></a><span data-ttu-id="042f3-104">Kurulum</span><span class="sxs-lookup"><span data-stu-id="042f3-104">Setup</span></span>

<span data-ttu-id="042f3-105">Aşağıdakileri yükleyin:</span><span class="sxs-lookup"><span data-stu-id="042f3-105">Install the following:</span></span>

1. <span data-ttu-id="042f3-106">[.NET core 2.1 SDK](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.500 veya üzeri).</span><span class="sxs-lookup"><span data-stu-id="042f3-106">[.NET Core 2.1 SDK](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.500 or later).</span></span>
1. <span data-ttu-id="042f3-107">[Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15,9 veya üzeri) ile *ASP.NET ve web geliştirme* Seçili iş yükü.</span><span class="sxs-lookup"><span data-stu-id="042f3-107">[Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15.9 or later) with the *ASP.NET and web development* workload selected.</span></span>
1. <span data-ttu-id="042f3-108">En son [Blazor dil Hizmetleri Uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.</span><span class="sxs-lookup"><span data-stu-id="042f3-108">The latest [Blazor Language Services extension](https://go.microsoft.com/fwlink/?linkid=870389) from the Visual Studio Marketplace.</span></span>
1. <span data-ttu-id="042f3-109">Komut satırında Blazor şablonları:</span><span class="sxs-lookup"><span data-stu-id="042f3-109">The Blazor templates on the command-line:</span></span>

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

<span data-ttu-id="042f3-110">Visual Studio'dan ilk projenizi oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="042f3-110">To create your first project from Visual Studio:</span></span>

1. <span data-ttu-id="042f3-111">Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="042f3-111">Select **File** > **New Project** > **Web** > **ASP.NET Core Web Application**.</span></span>
1. <span data-ttu-id="042f3-112">Emin **.NET Core** ve **ASP.NET Core 2.1** üstünde seçilir.</span><span class="sxs-lookup"><span data-stu-id="042f3-112">Make sure **.NET Core** and **ASP.NET Core 2.1** are selected at the top.</span></span>
1. <span data-ttu-id="042f3-113">Blazor şablonu seçip **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="042f3-113">Choose the Blazor template and select **OK**.</span></span>

   ![Yeni uygulama iletişim kutusu](https://msdnshared.blob.core.windows.net/media/2018/07/new-blazor-app-dialog-0.5.0.png)

1. <span data-ttu-id="042f3-115">Tuşuna **Ctrl-F5** uygulamayı çalıştırmak için *hata ayıklayıcı olmadan*.</span><span class="sxs-lookup"><span data-stu-id="042f3-115">Press **Ctrl-F5** to run the app *without the debugger*.</span></span> <span data-ttu-id="042f3-116">Hata ayıklayıcısı ile çalıştırma (**F5**) şu anda desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="042f3-116">Running with the debugger (**F5**) isn't supported at this time.</span></span>

<span data-ttu-id="042f3-117">Yeni bir Blazor uygulama komut satırından oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="042f3-117">To create a new Blazor app from the command-line:</span></span>

```console
dotnet new blazor -o BlazorApp1
cd BlazorApp1
dotnet run
```

<span data-ttu-id="042f3-118">Tebrikler!</span><span class="sxs-lookup"><span data-stu-id="042f3-118">Congrats!</span></span> <span data-ttu-id="042f3-119">Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!</span><span class="sxs-lookup"><span data-stu-id="042f3-119">You just ran your first Blazor app!</span></span>

![Blazor uygulama ana sayfası](https://msdnshared.blob.core.windows.net/media/2018/04/blazor-bootstrap-4.png)

## <a name="help--feedback"></a><span data-ttu-id="042f3-121">Yardım ve geri bildirim</span><span class="sxs-lookup"><span data-stu-id="042f3-121">Help & feedback</span></span>

<span data-ttu-id="042f3-122">Geri bildiriminiz Blazor için Deneysel bu aşamasında bizim için özellikle önemlidir.</span><span class="sxs-lookup"><span data-stu-id="042f3-122">Your feedback is especially important to us during this experimental phase for Blazor.</span></span> <span data-ttu-id="042f3-123">Bir sorunla karşılaşırsanız veya Blazor çalışırken sorularınız varsa lütfen bize bildirin!</span><span class="sxs-lookup"><span data-stu-id="042f3-123">If you run into issues or have questions while trying out Blazor, please let us know!</span></span>

* <span data-ttu-id="042f3-124">[Github'da dosya sorunları](https://github.com/aspnet/AspNetCore/issues) çalıştırmanız halinde herhangi bir sorun veya geliştirme önerilerde bulunmak için.</span><span class="sxs-lookup"><span data-stu-id="042f3-124">[File issues on GitHub](https://github.com/aspnet/AspNetCore/issues) for any problems you run into or to make suggestions for improvements.</span></span>
* <span data-ttu-id="042f3-125">Sohbet bize ve Blazor toplulukla üzerinde [Gitter](https://gitter.im/aspnet/blazor) takılı kalmasına ya da paylaşmak için nasıl Blazor çalıştığından sizin için.</span><span class="sxs-lookup"><span data-stu-id="042f3-125">Chat with us and the Blazor community on [Gitter](https://gitter.im/aspnet/blazor) if you get stuck or to share how Blazor is working for you.</span></span>

<span data-ttu-id="042f3-126">Blazor denemenize sonra lütfen bize bizim ürün içi ankete katılarak düşüncelerinizi iletin.</span><span class="sxs-lookup"><span data-stu-id="042f3-126">After you've tried out Blazor, please let us know what you think by taking our in-product survey.</span></span> <span data-ttu-id="042f3-127">Yalnızca Blazor proje şablonlarından birini çalıştırıldığında, uygulama giriş sayfasında görüntülenen anket bağlantıya tıklayın:</span><span class="sxs-lookup"><span data-stu-id="042f3-127">Just click the survey link shown on the app home page when running one of the Blazor project templates:</span></span>

![Blazor anketi](https://msdnshared.blob.core.windows.net/media/2018/05/blazor-survey-new.png)

## <a name="next-steps"></a><span data-ttu-id="042f3-129">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="042f3-129">Next steps</span></span>

<xref:tutorials/first-blazor-app>
