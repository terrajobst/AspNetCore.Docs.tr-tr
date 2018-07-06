---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
title: (VB) Repeater denetimiyle ModalPopup kullanma | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Ayrıca, bu Sözl kullanmak da mümkündür...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 0c8e74f1-b3ba-4ca9-a1c5-f5c4831a359a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c51ddd4b2bcc17c7d8c5dee0926903bea6ac749f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826620"
---
<a name="using-modalpopup-with-a-repeater-control-vb"></a>(VB) Repeater denetimiyle ModalPopup kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2VB.pdf)

> AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Repeater'da içindeki bu denetimi kullanmak mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim Araç Seti ModalPopup denetiminde istemci-tarafı yollardan kalıcı açılan pencere oluşturmak için basit bir yol sunar. Repeater'da içindeki bu denetimi kullanmak mümkündür.

## <a name="steps"></a>Adımlar

İlk olarak bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası. Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda. ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample1.aspx)]

Ardından, bir veri kaynağı sayfasına ekleyin. Sınırlı miktarda veri kullanmak için yalnızca ilk beş girişleri AdventureWorks veritabanını Satıcı tablosunda seçiyoruz. Veri kaynağı oluşturmak için Visual Studio Yardımcısı'nı kullanıyorsanız, güncel sürümdeki bir hatayı tablo adı öneki değil gerektiğini unutmayın (`Vendor`) ile `Purchasing`. Aşağıdaki biçimlendirmede doğru sözdizimini gösterir:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample2.aspx)]

Ardından, kalıcı açılan hizmet veren bir panel ekleme. İçerdiği bir `Button` denetim açılan kapatın:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample3.aspx)]

Yineleyici içinde iş açılan yapmak için `ModalPopupExtender` denetim yerleştirmek, içinde `<ItemTemplate>` yineleyicinin bölümü. Bu nedenle paneli repeater dışında ancak genişletici içinde. Yineleyici için biçimlendirme şöyledir:

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-vb/samples/sample4.aspx)]

Ardından, her bir veri kaynağı öğe kalıcı açılan tetikleyen bir düğmeyle yanında görüntülenir.


[![Her veri kaynağı girişi için kalıcı açılan tetiklenebilir](using-modalpopup-with-a-repeater-control-vb/_static/image2.png)](using-modalpopup-with-a-repeater-control-vb/_static/image1.png)

Her veri kaynağı girişi için kalıcı açılan tetiklenebilir ([tam boyutlu görüntüyü görmek için tıklatın](using-modalpopup-with-a-repeater-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Önceki](launching-a-modal-popup-window-from-server-code-vb.md)
> [İleri](handling-postbacks-from-a-modalpopup-vb.md)
