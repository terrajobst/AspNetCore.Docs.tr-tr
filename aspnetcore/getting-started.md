---
title: ASP.NET çekirdeği ile çalışmaya başlama
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici.
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: c2f18c69901a5a6503314d508a776e6985872681
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="6c641-103">ASP.NET çekirdeği ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="6c641-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="6c641-104">ASP.NET Core en son sürümü için bu yönergeleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="6c641-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="6c641-105">Bkz: [ASP.NET Core 1.1 ile çalışmaya başlama](xref:getting-started-1.1) bu belgenin 1.1 sürümünü için.</span><span class="sxs-lookup"><span data-stu-id="6c641-105">See [Getting Started with ASP.NET Core 1.1](xref:getting-started-1.1) for the 1.1 version of this document.</span></span>

1. <span data-ttu-id="6c641-106">Yükleme [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span><span class="sxs-lookup"><span data-stu-id="6c641-106">Install the [!INCLUDE [](~/includes/net-core-sdk-download-link.md)].</span></span>

2. <span data-ttu-id="6c641-107">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6c641-107">Create a new .NET Core project.</span></span>

   <span data-ttu-id="6c641-108">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="6c641-108">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="6c641-109">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="6c641-109">On Windows, open a command prompt.</span></span> <span data-ttu-id="6c641-110">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="6c641-110">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
3. <span data-ttu-id="6c641-111">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6c641-111">Run the app.</span></span>

    <span data-ttu-id="6c641-112">Uygulamayı çalıştırmak için aşağıdaki komutları kullanın:</span><span class="sxs-lookup"><span data-stu-id="6c641-112">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

4. <span data-ttu-id="6c641-113">Göz atın [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="6c641-113">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

5. <span data-ttu-id="6c641-114">Açık <em>Pages/About.cshtml</em> ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="6c641-114">Open <em>Pages/About.cshtml</em> and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="6c641-115">Sunucusundaki zamandır @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="6c641-115">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

6. <span data-ttu-id="6c641-116">Gözat [ http://localhost:5000/About ](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="6c641-116">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="6c641-117">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="6c641-117">Next steps</span></span>

<span data-ttu-id="6c641-118">Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="6c641-118">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="6c641-119">ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="6c641-119">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="6c641-120">Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6c641-120">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="6c641-121">Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="6c641-121">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
