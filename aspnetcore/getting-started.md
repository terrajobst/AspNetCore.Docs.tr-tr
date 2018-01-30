---
title: "ASP.NET Core 2.0 ile çalışmaya başlama"
author: rick-anderson
description: "Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulamanın çalıştığı hızlı bir öğretici."
manager: wpickett
ms.author: riande
ms.date: 10/18/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: getting-started
ms.openlocfilehash: eb1fd748029743ca6991927cc95b612ed1975338
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="get-started-with-aspnet-core"></a><span data-ttu-id="665b1-103">ASP.NET çekirdeği ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="665b1-103">Get Started with ASP.NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="665b1-104">ASP.NET Core en son sürümü için bu yönergeleri verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="665b1-104">These instructions are for the latest version of ASP.NET Core.</span></span> <span data-ttu-id="665b1-105">Önceki bir sürümü ile çalışmaya başlamak için mi arıyorsunuz?</span><span class="sxs-lookup"><span data-stu-id="665b1-105">Looking to get started with an earlier version?</span></span> <span data-ttu-id="665b1-106">Bkz: [Bu öğretici 1.1 sürümünü](xref:getting-started-1.1).</span><span class="sxs-lookup"><span data-stu-id="665b1-106">See [the 1.1 version of this tutorial](xref:getting-started-1.1).</span></span>

1. <span data-ttu-id="665b1-107">Yükleme [.NET Core](https://www.microsoft.com/net/core/).</span><span class="sxs-lookup"><span data-stu-id="665b1-107">Install [.NET Core](https://www.microsoft.com/net/core/).</span></span>

2. <span data-ttu-id="665b1-108">Yeni bir .NET Core projesi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="665b1-108">Create a new .NET Core project.</span></span>

   <span data-ttu-id="665b1-109">MacOS ve Linux üzerinde bir terminal penceresi açın.</span><span class="sxs-lookup"><span data-stu-id="665b1-109">On macOS and Linux, open a terminal window.</span></span> <span data-ttu-id="665b1-110">Windows, bir komut istemi açın.</span><span class="sxs-lookup"><span data-stu-id="665b1-110">On Windows, open a command prompt.</span></span> <span data-ttu-id="665b1-111">Aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="665b1-111">Enter the following command:</span></span>

    ```terminal
    dotnet new razor -o aspnetcoreapp
    ```
    
4. <span data-ttu-id="665b1-112">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="665b1-112">Run the app.</span></span>

    <span data-ttu-id="665b1-113">Uygulamayı çalıştırmak için aşağıdaki komutları kullanın:</span><span class="sxs-lookup"><span data-stu-id="665b1-113">Use the following commands to run the app:</span></span>

    ```terminal
    cd aspnetcoreapp
    dotnet run
    ```

5. <span data-ttu-id="665b1-114">Browse to [http://localhost:5000](http://localhost:5000)</span><span class="sxs-lookup"><span data-stu-id="665b1-114">Browse to [http://localhost:5000](http://localhost:5000)</span></span>

6. <span data-ttu-id="665b1-115">Açık *Pages/About.cshtml* ve iletiyi görüntülemek için sayfanın değiştirme "Hello, world!</span><span class="sxs-lookup"><span data-stu-id="665b1-115">Open *Pages/About.cshtml* and modify the page to display the message "Hello, world!</span></span> <span data-ttu-id="665b1-116">Sunucusundaki zamandır @DateTime.Now ":</span><span class="sxs-lookup"><span data-stu-id="665b1-116">The time on the server is @DateTime.Now ":</span></span>

    [!code-html[Main](getting-started/sample/getting-started/about.cshtml?highlight=9&range=1-9)]

7. <span data-ttu-id="665b1-117">Gözat [http://localhost: 5000/hakkında](http://localhost:5000/About) ve değişiklikleri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="665b1-117">Browse to [http://localhost:5000/About](http://localhost:5000/About) and verify the changes.</span></span>

### <a name="next-steps"></a><span data-ttu-id="665b1-118">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="665b1-118">Next steps</span></span>

<span data-ttu-id="665b1-119">Başlama öğreticileri için bkz: [ASP.NET Core öğreticileri](tutorials/index.md)</span><span class="sxs-lookup"><span data-stu-id="665b1-119">For getting-started tutorials, see [ASP.NET Core Tutorials](tutorials/index.md)</span></span>

<span data-ttu-id="665b1-120">ASP.NET temel kavramlar ve mimari bir giriş için bkz: [ASP.NET Core giriş](index.md) ve [ASP.NET Core Temelleri](fundamentals/index.md).</span><span class="sxs-lookup"><span data-stu-id="665b1-120">For an introduction to ASP.NET Core concepts and architecture, see [ASP.NET Core Introduction](index.md) and [ASP.NET Core Fundamentals](fundamentals/index.md).</span></span>

<span data-ttu-id="665b1-121">Bir ASP.NET Core uygulama .NET Core veya .NET Framework temel sınıf kitaplığı ve çalışma zamanı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="665b1-121">An ASP.NET Core app can use the .NET Core or .NET Framework Base Class Library and runtime.</span></span> <span data-ttu-id="665b1-122">Daha fazla bilgi için bkz: [.NET Core ve .NET Framework arasında seçim yapma](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span><span class="sxs-lookup"><span data-stu-id="665b1-122">For more information, see [Choosing between .NET Core and .NET Framework](https://docs.microsoft.com/dotnet/articles/standard/choosing-core-framework-server).</span></span>
