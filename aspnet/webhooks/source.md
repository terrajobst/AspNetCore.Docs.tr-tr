---
uid: webhooks/source
title: ASP.NET Web kancaları kaynak kodu ve NuGet paketleri | Microsoft Docs
author: rick-anderson
description: ASP.NET Web kancaları kaynak kodu ve NuGet paketleri bağlantılar
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.openlocfilehash: 1ee4adc2a28054ed1f856d7e4e991b34972a70e8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802189"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="655b4-103">ASP.NET Web kancaları kaynak kodu ve NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="655b4-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="655b4-104">Microsoft ASP.NET WebHooks modülleri Microsoft ASP.NET ailesinin bir parçasıdır ve olarak barındırılan bir [GitHub üzerinde açık kaynak proje](https://github.com/aspnet/WebHooks).</span><span class="sxs-lookup"><span data-stu-id="655b4-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="655b4-105">Biz katkılarını kabul eder ancak lütfen bakmak yani [katkı kılavuzunu](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) bir çekme isteği göndermeden önce.</span><span class="sxs-lookup"><span data-stu-id="655b4-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="655b4-106">Bu çevrimiçi belgeleri okuduğunuz şimdi de olarak barındırılan [GitHub üzerinde açık kaynak](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) ve Katkıları da kabul eder.</span><span class="sxs-lookup"><span data-stu-id="655b4-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="655b4-107">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="655b4-107">NuGet packages</span></span>

<span data-ttu-id="655b4-108">[NuGet paketlerini](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) üç bölüme ayrılır:</span><span class="sxs-lookup"><span data-stu-id="655b4-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="655b4-109">[Ortak](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): göndericiler ile alıcılar arasında paylaşılan ortak bir paket.</span><span class="sxs-lookup"><span data-stu-id="655b4-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="655b4-110">[Gönderen](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): paketleri kendi Web kancaları başkalarına göndermek destekleyen bir dizi.</span><span class="sxs-lookup"><span data-stu-id="655b4-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="655b4-111">Web kancaları göndermek için işlevselliği, daha ayrıntılı olarak açıklanan [gönderme Web kancaları](sending/index.md).</span><span class="sxs-lookup"><span data-stu-id="655b4-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="655b4-112">[Alıcılar](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): paketleri Web kancaları diğerlerinden almayı destekleyen bir dizi.</span><span class="sxs-lookup"><span data-stu-id="655b4-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="655b4-113">Web kancaları almak için işlevselliği, daha ayrıntılı olarak açıklanan [alma Web kancaları](receiving/index.md).</span><span class="sxs-lookup"><span data-stu-id="655b4-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
