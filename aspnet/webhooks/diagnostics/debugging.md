---
uid: webhooks/diagnostics/debugging
title: ASP.NET hata ayıklama Web kancaları | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları hata ayıklamak nasıl.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801994"
---
# <a name="aspnet-webhooks-debugging"></a><span data-ttu-id="2c2e7-103">ASP.NET hata ayıklama Web kancaları</span><span class="sxs-lookup"><span data-stu-id="2c2e7-103">ASP.NET WebHooks debugging</span></span>  

## <a name="debugging-in-azure"></a><span data-ttu-id="2c2e7-104">Azure'da hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="2c2e7-104">Debugging in Azure</span></span>

<span data-ttu-id="2c2e7-105">Web uygulamanızı Azure'da çalışırken hata ayıklama için öğreticiye bakın [Visual Studio kullanarak Azure App Service'te bir web uygulaması sorunlarını giderme](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="2c2e7-105">To debug your Web Application while running in Azure, please see the tutorial [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="debugging-with-source-and-symbols"></a><span data-ttu-id="2c2e7-106">Kaynak ve simgeler ile hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="2c2e7-106">Debugging with Source and Symbols</span></span>

<span data-ttu-id="2c2e7-107">Kendi kod hata ayıklama ek olarak, Microsoft ASP.NET WebHooks doğrudan ve hatta hata ayıklamak mümkündür tüm .NET.</span><span class="sxs-lookup"><span data-stu-id="2c2e7-107">In addition to debugging your own code, it is possible to debug directly into Microsoft ASP.NET WebHooks, and in fact all of .NET.</span></span> <span data-ttu-id="2c2e7-108">Bu, yerel olarak veya uzaktan hata ayıklama bağımsız olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="2c2e7-108">This works regardless of whether you debug locally or remotely.</span></span> <span data-ttu-id="2c2e7-109">İlk olarak, giderek kaynak ve simgeleri bulmak için Visual Studio yapılandırma **hata ayıklama** ardından **seçenekler ve ayarlar**.</span><span class="sxs-lookup"><span data-stu-id="2c2e7-109">First, configure Visual Studio to find the source and symbols by going to **Debug** and then **Options and Settings**.</span></span> <span data-ttu-id="2c2e7-110">Bu gibi seçenekleri ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="2c2e7-110">Set the options like this:</span></span>

![Seçenekler ve ayarlar](_static/SourceSymbols.png)

<span data-ttu-id="2c2e7-112">Ardından bir bağlantı ekleyin [symbolsource.org](http://symbolsource.org) kaynak ve sembol karşıdan yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="2c2e7-112">Then add a link to [symbolsource.org](http://symbolsource.org) for downloading the source and symbols.</span></span> <span data-ttu-id="2c2e7-113">Git **sembolleri** sekmesinde menüsünden izleyebilirim ve bir simge konumu olarak aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="2c2e7-113">Go to the **Symbols** tab of the menu above and add the following as a symbol location:</span></span>

```
http://srv.symbolsource.org/pdb/Public
```

<span data-ttu-id="2c2e7-114">Ayrıca, önbellek dizini kısa bir ad olduğundan emin olun; Aksi takdirde dosya adları değil yüklenecek semboller neden olacak uzun elde edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2c2e7-114">In addition, make sure that the cache directory has a short name; otherwise the file names can get too long which will cause the symbols to not load.</span></span> <span data-ttu-id="2c2e7-115">Bir örnek yoludur:</span><span class="sxs-lookup"><span data-stu-id="2c2e7-115">A sample path is:</span></span>

```
C:\SymCache
```

<span data-ttu-id="2c2e7-116">Ayarları şuna benzer görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="2c2e7-116">The settings should look similar to this:</span></span>

![Seçenekleri sembol dosyası konumu örneği](_static/SymSource.png)
