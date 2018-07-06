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
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET Web kancaları kaynak kodu ve NuGet paketleri

Microsoft ASP.NET WebHooks modülleri Microsoft ASP.NET ailesinin bir parçasıdır ve olarak barındırılan bir [GitHub üzerinde açık kaynak proje](https://github.com/aspnet/WebHooks). Biz katkılarını kabul eder ancak lütfen bakmak yani [katkı kılavuzunu](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) bir çekme isteği göndermeden önce.

Bu çevrimiçi belgeleri okuduğunuz şimdi de olarak barındırılan [GitHub üzerinde açık kaynak](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) ve Katkıları da kabul eder.

## <a name="nuget-packages"></a>NuGet paketleri

[NuGet paketlerini](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) üç bölüme ayrılır:

* [Ortak](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): göndericiler ile alıcılar arasında paylaşılan ortak bir paket.

* [Gönderen](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): paketleri kendi Web kancaları başkalarına göndermek destekleyen bir dizi. Web kancaları göndermek için işlevselliği, daha ayrıntılı olarak açıklanan [gönderme Web kancaları](sending/index.md).

* [Alıcılar](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): paketleri Web kancaları diğerlerinden almayı destekleyen bir dizi. Web kancaları almak için işlevselliği, daha ayrıntılı olarak açıklanan [alma Web kancaları](receiving/index.md).
