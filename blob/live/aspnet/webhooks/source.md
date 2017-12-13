---
uid: webhooks/source
title: "ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri | Microsoft Docs"
author: rick-anderson
description: "ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri bağlantılar"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="d39c6-103">ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="d39c6-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="d39c6-104">Microsoft ASP.NET WebHooks modülleri Microsoft ASP.NET ailesinin bir parçasıdır ve olarak barındırılan bir [github'da açık kaynaklı proje](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="d39c6-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="d39c6-105">Bu size Katkıları kabul ancak lütfen bakmak anlamına gelir [katkı yönergeleri](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) bir çekme isteği göndermeden önce.</span><span class="sxs-lookup"><span data-stu-id="d39c6-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="d39c6-106">Okumakta olduğunuz bu çevrimiçi belgeleri şimdi de olarak barındırılan [github'da açık kaynaklı](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) ve ayrıca Katkıları kabul eder.</span><span class="sxs-lookup"><span data-stu-id="d39c6-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="d39c6-107">Nuget paketleri</span><span class="sxs-lookup"><span data-stu-id="d39c6-107">Nuget Packages</span></span>

<span data-ttu-id="d39c6-108">Microsoft ASP.NET WebHooks, ayrıca bunları görmek için Visual Studio'da Önizleme bayrağı seçilecek olduğu anlamına gelir Nuget paketlerini önizleme olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="d39c6-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="d39c6-109">[Nuget paketlerini](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) üç bölüme devided:</span><span class="sxs-lookup"><span data-stu-id="d39c6-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="d39c6-110">[Ortak](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): göndericiler ile alıcılar arasında paylaşılan ortak bir paket.</span><span class="sxs-lookup"><span data-stu-id="d39c6-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="d39c6-111">[Gönderen](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): kendi Web Kancalarını başkalarına göndermek amacıyla destekleyen paketler kümesi.</span><span class="sxs-lookup"><span data-stu-id="d39c6-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="d39c6-112">Web kancası göndermek için işlevselliği, daha ayrıntılı olarak açıklandığı [gönderme Kancalarını](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="d39c6-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="d39c6-113">[Alıcıları](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): bir Web kancası diğer bilgisayarlardan almasını destekleyen paketler kümesi.</span><span class="sxs-lookup"><span data-stu-id="d39c6-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="d39c6-114">Web kancası alma işlevselliği, daha ayrıntılı olarak açıklanan [alma Web Kancalarını](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="d39c6-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
