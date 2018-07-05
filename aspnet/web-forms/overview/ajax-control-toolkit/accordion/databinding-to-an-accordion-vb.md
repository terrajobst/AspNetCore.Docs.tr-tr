---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: (VB) bir Accordion'a veri bağlama | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle w bildirilen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: bc11aa96577919c2ca5dfca4ffc6301da67645ea
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377434"
---
<a name="databinding-to-an-accordion-vb"></a>(VB) bir Accordion'a veri bağlama
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)

> AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle sayfa içinde bildirilen, ancak bir veri kaynağına bağlama daha fazla esneklik sunar.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti Accordion denetimi birden fazla bölme sağlar ve bunlardan biri aynı anda görüntüleme izin verir. Paneller genellikle sayfa içinde bildirilen, ancak bir veri kaynağına bağlama daha fazla esneklik sunar.

## <a name="steps"></a>Adımlar

İlk olarak bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

Ardından, bir veri kaynağı sayfasına ekleyin. Sınırlı miktarda veri kullanmak için yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçiyoruz. Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, güncel sürümdeki bir hatayı tablo adı öneki değil gerektiğini unutmayın (`Vendor`) ile `Purchasing`. Aşağıdaki biçimlendirmede doğru sözdizimini gösterir:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

Veri kaynağı adı (ID) unutmayın. Bu çok kimliği daha sonra kullanılmalıdır `DataSourceID` Accordion denetimi özelliği:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

Accordion denetimi içinde denetimi üst bilgisi dahil olmak üzere çeşitli bölümlerini şablonları sağlayabilirsiniz (`<HeaderTemplate>`) ve içeriği (`<ContentTemplate>`). Bu öğeleri içinde yalnızca veri verileri çıktı kullanarak kaynak `DataBinder.Eval()` yöntemi:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

Sayfa yüklendiğinde, bu sunucu tarafı kodu ile bir accordion'a veri kaynağı bağlı olmalıdır:

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

Bu örnek sonuçlandırmak için başvurulan iki CSS sınıfları Accordion denetimi tanımlamanız gerekir (özelliklerinde `HeaderCssClass` ve `ContentCssClass`). Aşağıdaki biçimlendirmede koymak `<head>` sayfasının bölümünde:

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


[![Accordion verileri doğrudan veri kaynağından gelir.](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)

Accordion verileri doğrudan veri kaynağından gelir ([tam boyutlu görüntüyü görmek için tıklatın](databinding-to-an-accordion-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](dynamically-adding-an-accordion-pane-cs.md)
> [İleri](dynamically-adding-an-accordion-pane-vb.md)
