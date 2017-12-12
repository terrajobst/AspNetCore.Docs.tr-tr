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
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Web Kancalarını kaynak kodu ve NuGet paketleri

Microsoft ASP.NET WebHooks modülleri Microsoft ASP.NET ailesinin bir parçasıdır ve olarak barındırılan bir [github'da açık kaynaklı proje](https://github.com/aspnet/WebHooks). Bu size Katkıları kabul ancak lütfen bakmak anlamına gelir [katkı yönergeleri](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) bir çekme isteği göndermeden önce.

Okumakta olduğunuz bu çevrimiçi belgeleri şimdi de olarak barındırılan [github'da açık kaynaklı](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) ve ayrıca Katkıları kabul eder.

## <a name="nuget-packages"></a>Nuget paketleri

Microsoft ASP.NET WebHooks, ayrıca bunları görmek için Visual Studio'da Önizleme bayrağı seçilecek olduğu anlamına gelir Nuget paketlerini önizleme olarak kullanılabilir.

[Nuget paketlerini](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) üç bölüme devided:

* [Ortak](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): göndericiler ile alıcılar arasında paylaşılan ortak bir paket.

* [Gönderen](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): kendi Web Kancalarını başkalarına göndermek amacıyla destekleyen paketler kümesi. Web kancası göndermek için işlevselliği, daha ayrıntılı olarak açıklandığı [gönderme Kancalarını](sending/index.md).

* [Alıcıları](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): bir Web kancası diğer bilgisayarlardan almasını destekleyen paketler kümesi. Web kancası alma işlevselliği, daha ayrıntılı olarak açıklanan [alma Web Kancalarını](receiving/index.md).
