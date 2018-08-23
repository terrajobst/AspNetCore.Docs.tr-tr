---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Sürükle ve bırak ReorderList aracılığıyla (C#) | Microsoft Docs
author: wenz
description: AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin geçerli sırasını gelecektir...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 15ae6ae60381f3f656f667a97dac72dbb283c80e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752349"
---
<a name="drag-and-drop-via-reorderlist-c"></a>(C#) ReorderList sürükle ve bırak
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> AJAX Denetim Araç Seti ReorderList denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin geçerli sırasını sunucuda kalıcı hale.


## <a name="overview"></a>Genel Bakış

`ReorderList` AJAX Denetim Araç Seti denetimi sürükle ve bırak kullanıcı tarafından sıralanabilir bir liste sağlar. Listenin geçerli sırasını sunucuda kalıcı hale.

## <a name="steps"></a>Adımlar

`ReorderList` Denetim listesi veritabanından veri bağlamayı destekler. Hepsinden önemlisi, sıra liste öğesi için veri deposuna yazma değişiklikleri de destekler.

Bu örnek, Microsoft SQL Server 2005 Express Edition veri deposu olarak kullanır. Veritabanı express sürüm dahil olmak üzere Visual Studio yüklemesi isteğe bağlı (ve boş) bir parçasıdır. Altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

Microsoft SQL Server Management Studio Express kullanmak için veritabanı ayarlamak için en kolay yolu olan ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Sunucuya, çift tıklayarak `Databases` ve yeni bir veritabanı oluşturun (sağ tıklatın ve seçin `New Database`) olarak adlandırılan `Tutorials`.

Bu veritabanında adlı yeni bir tablo oluşturma `AJAX` aşağıdaki dört sütun ile:

- `id` (NULL olmayan birincil anahtar, tamsayı, kimlik)
- `char` (char(1), NULL)
- `description` (varchar(50), NULL)
- `position` (int, NULL)


[![AJAX Tablo düzeni](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

AJAX tablo düzenini ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image3.png))


Ardından, tabloda değerlerin birkaç ile doldurun. Unutmayın `position` sütun öğelerin sıralama düzenini tutar.


[![AJAX tablosundaki ilk veri](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

AJAX tablosundaki ilk veri ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image6.png))


Oluşturulacak sonraki adım gerektirir bir `SqlDataSource` , tablo ve yeni veritabanı ile iletişim kurmak için denetim. Veri kaynağı desteklemelidir `SELECT` ve `UPDATE` SQL komutları. Daha sonra liste öğelerini sırası değiştirildiğinde, `ReorderList` denetimi otomatik olarak gönderir veri kaynağı için iki değer `Update` komut: yeni konumunu ve öğesinin kimliği. Bu nedenle, gereksinimleri veri kaynağının bir `<UpdateParameters>` bölümü bu iki değer için:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

`ReorderList` Aşağıdaki öznitelikleri ayarlamak denetim gerekir:

- `AllowReorder`: Liste öğeleri düzenlenebilir olup olmadığı
- `DataSourceID`: Veri kaynağı kimliği
- `DataKeyField`: Birincil anahtar sütunu veri kaynağındaki adı
- `SortOrderField`: Liste öğeleri için sıralama düzeni sağlar veri kaynak sütunu

İçinde `<DragHandleTemplate>` ve `<ItemTemplate>` bölümleri, liste düzeninin göre ayarlanabilir. Ayrıca, veri bağlama mümkündür kullanarak `Eval()` yöntemi, burada görüldüğü gibi:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Aşağıdaki CSS stil bilgileri (bulunulan `<DragHandleTemplate>` bölümünü `ReorderList` denetimi) sürükleme tutamacı geldiğinde fare işaretçisi uygun şekilde değiştirir emin olur:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Son olarak, bir `ScriptManager` denetim sayfa için ASP.NET AJAX başlatır:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Bu örnekte tarayıcıda çalıştırın ve liste öğeleri biraz yeniden düzenleyebilirsiniz. Daha sonra sayfayı yeniden yükleyin ve/veya veritabanı bir bakalım. Değiştirilen konumları tutulmuş ve değerler tarafından yansıtılır `position` sütun veritabanı ve tüm herhangi bir kod yalnızca işaretleme kullanarak.


[![Veritabanı değişiklikleri yeni liste öğesi sırasına göre verileri](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Veritabanı değişiklikleri yeni listesine göre verileri öğe sırasını ([tam boyutlu görüntüyü görmek için tıklatın](drag-and-drop-via-reorderlist-cs/_static/image9.png))

> [!div class="step-by-step"]
> [Önceki](using-postbacks-with-reorderlist-cs.md)
> [İleri](using-postbacks-with-reorderlist-vb.md)
