---
uid: webhooks/source
title: ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri | Microsoft Docs
author: rick-anderson
description: ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri bağlantılar
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
ms.locfileid: "27709976"
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri

Microsoft ASP.NET WebHooks modülleri Microsoft ASP.NET ailesinin bir parçasıdır ve olarak barındırılan bir [github'da açık kaynaklı proje](https://github.com/aspnet/WebHooks). Bu size Katkıları kabul ancak lütfen bakmak anlamına gelir [katkı yönergeleri](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) bir çekme isteği göndermeden önce.

Okumakta olduğunuz bu çevrimiçi belgeleri şimdi de olarak barındırılan [github'da açık kaynaklı](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) ve ayrıca Katkıları kabul eder.

## <a name="nuget-packages"></a>NuGet paketleri

[NuGet paketlerini](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) üç bölüme ayrılır:

* [Ortak](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): göndericiler ile alıcılar arasında paylaşılan ortak bir paket.

* [Gönderen](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): kendi Web Kancalarını başkalarına göndermek amacıyla destekleyen paketler kümesi. Web kancası göndermek için işlevselliği, daha ayrıntılı olarak açıklandığı [gönderme Kancalarını](sending/index.md).

* [Alıcıları](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): bir Web kancası diğer bilgisayarlardan almasını destekleyen paketler kümesi. Web kancası alma işlevselliği, daha ayrıntılı olarak açıklanan [alma Web Kancalarını](receiving/index.md).
