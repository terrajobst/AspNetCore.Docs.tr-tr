---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
title: (C#) Repeater denetimiyle HoverMenu kullanma | Microsoft Docs
author: wenz
description: 'AJAX Denetim Araç Seti HoverMenu denetiminde basit açılan etkisi sağlar: fare işaretçisini bir öğenin geldiğinde bir specifi açılır pencere görünür...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e7700e7b-edc3-4183-a713-70e507cc7490
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 309cd64d8b62505c1546317a8aced7f50048af0c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384700"
---
<a name="using-hovermenu-with-a-repeater-control-c"></a>(C#) Repeater denetimiyle HoverMenu kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1CS.pdf)

> AJAX Denetim Araç Seti HoverMenu denetiminde basit açılan etkisi sağlar: fare işaretçisini bir öğenin geldiğinde, belirtilen konumda bir açılır pencere görünür. Repeater'da içindeki bu denetimi kullanmak mümkündür.


## <a name="overview"></a>Genel Bakış

`HoverMenu` AJAX Denetim Araç Seti denetiminde basit açılan etkisi sağlar: fare işaretçisini bir öğenin geldiğinde, belirtilen konumda bir açılır pencere görünür. Repeater'da içindeki bu denetimi kullanmak mümkündür.

## <a name="steps"></a>Adımlar

İlk olarak bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample1.aspx)]

Ardından, bir veri kaynağı sayfasına ekleyin. Sınırlı miktarda veri kullanmak için yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçiyoruz. Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, güncel sürümdeki bir hatayı tablo adı öneki değil gerektiğini unutmayın (`Vendor`) ile `Purchasing`. Aşağıdaki biçimlendirmede doğru sözdizimini gösterir:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample2.aspx)]

Ardından, kalıcı açılan hizmet veren bir panel ekleyin:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample3.aspx)]

Şimdi, `HoverMenuExtender` dönüştürülerek. Veri kaynağındaki her öğenin kendi açılan alır, böylece yineleyici içinde 's genişletici yerleştirmeniz gerekir `<ItemTemplate>` bölümü. Biçimlendirme şöyledir:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-cs/samples/sample4.aspx)]

Veri kaynağındaki her öğe sağ tarafta açılır pencere görüntüler artık (`PopupPosition` özniteliği) bir gecikme 50 milisaniye sonra (`PopDelay` özniteliği).


[![Yineleyicideki her öğenin yanında vurgulu menüsünde görünür](using-hovermenu-with-a-repeater-control-cs/_static/image2.png)](using-hovermenu-with-a-repeater-control-cs/_static/image1.png)

Yineleyicideki her bir öğenin üzerine gelindiğinde kullanılacak menüsünün yanında ([tam boyutlu görüntüyü görmek için tıklatın](using-hovermenu-with-a-repeater-control-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-hovermenu-with-a-repeater-control-vb.md)
