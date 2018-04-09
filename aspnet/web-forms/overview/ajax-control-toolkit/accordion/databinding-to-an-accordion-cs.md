---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: Veri bağlama için bir Accordion (C#) | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle w bildirilir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a3ec242c4d5312026ddbc8282ef1b4c3142915a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="databinding-to-an-accordion-c"></a>Veri bağlama için bir Accordion (C#)
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)

> AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle sayfa içinde bildirilen, ancak veri kaynağına bağlama daha fazla esneklik sunar.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti Accordion denetiminde birden çok bölmeleri sağlar ve bunlardan biri aynı anda görüntülenecek kullanıcı sağlar. Paneller genellikle sayfa içinde bildirilen, ancak veri kaynağına bağlama daha fazla esneklik sunar.

## <a name="steps"></a>Adımlar

Öncelikle, bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

Ardından, bir veri kaynağı sayfasına ekleyin. Çok sınırlı miktarda veri kullanabilmeniz için biz yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçin. Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, geçerli sürümde hata tablo adı öneki değil şunları aklınızda (`Vendor`) ile `Purchasing`. Aşağıdaki biçimlendirmede doğru sözdizimi gösterilmektedir:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

Veri kaynağı adı (kimlik) unutmayın. Bu çok kimliği sonra kullanılmalıdır `DataSourceID` Accordion denetiminin özelliği:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

Accordion denetim içinde denetimin üstbilgisi de dahil olmak üzere çeşitli bölümleri için şablonlar sağlayabilir (`<HeaderTemplate>`) ve içeriği (`<ContentTemplate>`). Bu öğeleri içinde yalnızca çıktı verileri veri kaynağı, kullanarak `DataBinder.Eval()` yöntemi:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

Sayfa yüklendiğinde, bu sunucu tarafı kodu ile accordion için veri kaynağı bağlı olması gerekir:

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

Bu örnek sonuçlandırmak için başvurulan iki CSS sınıfları Accordion denetiminde tanımlamanız gerekir (özelliklerinde `HeaderCssClass` ve `ContentCssClass`). Aşağıdaki biçimlendirmede koyun `<head>` sayfasının bölümünde:

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


[![Veriler accordion, doğrudan veri kaynağından gelir](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)

Veriler accordion, doğrudan veri kaynağından gelir ([tam boyutlu görüntüyü görüntülemek için tıklatın](databinding-to-an-accordion-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](dynamically-adding-an-accordion-pane-cs.md)
