---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
title: (VB) bir FormView'da TextBoxWatermark kullanma | Microsoft Docs
author: wenz
description: Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir. Bir kullanıcı kutusuna tıkladığında, ben...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 41497361-7fba-4825-b36c-f58d79522a88
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-in-a-formview-vb
msc.type: authoredcontent
ms.openlocfilehash: 1bb3ad274ba6c8617643fa4d7894a73de4b5cdff
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396191"
---
<a name="using-textboxwatermark-in-a-formview-vb"></a>(VB) bir FormView'da TextBoxWatermark kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark1.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark1VB.pdf)

> Bir metin kutusu içinde gösterilir böylece AJAX Denetim Araç Seti TextBoxWatermark denetiminde metin kutusunu genişletir. Kullanıcı kutusuna tıkladığında boşaltılır. Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir. Bu aynı zamanda FormView denetimine mümkündür.


## <a name="overview"></a>Genel Bakış

`TextBoxWatermark` AJAX Denetim Araç Seti denetiminde metin kutusuna bir metin kutusu içinde gösterilir böylece genişletir. Kullanıcı kutusuna tıkladığında boşaltılır. Kullanıcının metin girmeden kutusu ayrılırsa doldurulmuş metni görüntülenir. Bu da içinde olası bir `FormView` denetimi.

## <a name="steps"></a>Adımlar

İlk olarak bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample1.aspx)]

Ardından, bir veri kaynağı destekleyen sayfasına ekleme `DELETE`, `INSERT` ve `UPDATE` SQL deyimleri. Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, güncel sürümdeki bir hatayı tablo adı öneki değil gerektiğini unutmayın (`Vendor`) ile `Purchasing`. Aşağıdaki biçimlendirmede doğru sözdizimini gösterir:

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample2.aspx)]

Adı unutmayın (`ID`) veri kaynağının de kullanılacağından `DataSourceID` özelliği `FormView` denetimi. `<InsertItemTemplate>` Bölümünü `FormView` tarafından genişletilmiş bir metin kutusu içeren `TextBoxWatermarkExtender` denetimi. Genişletici şablon içinde ve dışında bulunduğu emin olun.

[!code-aspx[Main](using-textboxwatermark-in-a-formview-vb/samples/sample3.aspx)]

Artık kullanıcı değiştiğinde INSERT moduna `FormView` denetimine metin alanına yeni satıcı performanstan doldurulmuş için `TextBoxWatermarkExtender` denetimi. Metin kutusu içine bir tıklayın kayboluyor filler metin sağlar.


[![Filigran alanında genişletici gelir](using-textboxwatermark-in-a-formview-vb/_static/image2.png)](using-textboxwatermark-in-a-formview-vb/_static/image1.png)

Filigran alanında genişletici gelir ([tam boyutlu görüntüyü görmek için tıklatın](using-textboxwatermark-in-a-formview-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](using-textboxwatermark-with-validation-controls-cs.md)
> [İleri](using-textboxwatermark-with-validation-controls-vb.md)
