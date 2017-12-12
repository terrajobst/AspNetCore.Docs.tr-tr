---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: "Sürükleme ve bırakma (VB) ReorderList | Microsoft Docs"
author: wenz
description: /Data-Access/Tutorials/master-detail-Using-a-bulleted-List-of-master-records-with-a-details-DataList-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: e193a31fc86b7e8733d0b2fba371d99c62783d6c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="drag-and-drop-via-reorderlist-vb"></a>Sürükle ve bırak ReorderList (VB) aracılığıyla
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> AJAX Denetim Araç Seti, ReorderList denetimi sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listeden geçerli sırasını sunucuda kalıcı.


## <a name="overview"></a>Genel Bakış

`ReorderList` AJAX Denetim Araç Seti denetiminde sürükle ve bırak aracılığıyla kullanıcı tarafından sıralanabilir bir liste sağlar. Listeden geçerli sırasını sunucuda kalıcı.

## <a name="steps"></a>Adımlar

`ReorderList` Denetim listesi veritabanından veri bağlamayı destekler. En önemlisi, liste öğesi veri deposu {değişiklikleri yazma da destekler.

Bu örnek Microsoft SQL Server 2005 Express Edition veri deposu olarak kullanır. Veritabanı express edition dahil olmak üzere bir Visual Studio yükleme, isteğe bağlı (ve ücretsiz) parçasıdır. Altında ayrı bir yükleme olarak kullanılabilir [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

En kolay yolu veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Sunucuya, çift `Databases` ve yeni bir veritabanı oluşturun (sağ tıklatın ve seçin `New Database`) olarak adlandırılan `Tutorials`.

Bu veritabanında adlı yeni bir tablo oluşturmak `AJAX` aşağıdaki dört sütun ile:

- `id`(birincil anahtar, tamsayı, kimlik, NULL)
- `char`(char(1), NULL)
- `description`(varchar(50), NULL)
- `position`(int, NULL)


[![AJAX Tablo düzeni](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

AJAX tablo düzenini ([tam boyutlu görüntüyü görüntülemek için tıklatın](drag-and-drop-via-reorderlist-vb/_static/image3.png))


Ardından, tablo değerleri birkaç ile doldurun. Unutmayın `position` sütun öğeleri sıralama düzenini tutar.


[![AJAX tablosundaki ilk veri](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

İlk veri AJAX tablosundaki ([tam boyutlu görüntüyü görüntülemek için tıklatın](drag-and-drop-via-reorderlist-vb/_static/image6.png))


Oluşturmak için sonraki adım gerektiren bir `SqlDataSource` , tablo ve yeni veritabanı ile iletişim kurmak için denetim. Veri kaynağı desteklemelidir `SELECT` ve `UPDATE` SQL komutları. Liste öğelerini sırasını daha sonra değiştirildiğinde `ReorderList` denetimi veri kaynağı için iki değer otomatik olarak gönderir `Update` komutu: yeni konumu ve öğenin kimliği. Bu nedenle, gereksinimlerini veri kaynağı bir `<UpdateParameters>` bölüm bu iki değer için:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

`ReorderList` Denetim gereken aşağıdaki öznitelikleri ayarlamak:

- `AllowReorder`: Liste öğeleri olup düzenlenebilir
- `DataSourceID`: Veri kaynağı kimliği
- `DataKeyField`: Birincil anahtar sütunu, veri kaynağı adı
- `SortOrderField`: Liste öğeleri için sıralama düzeni sağlar veri kaynağı sütunu

İçinde `<DragHandleTemplate>` ve `<ItemTemplate>` bölümler, listesinin düzenini olabilir denetleyecek şekilde. Ayrıca, veri bağlama mümkündür kullanarak `Eval()` yöntemi, burada görüldüğü gibi:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Aşağıdaki CSS stil bilgileri (başvurulan `<DragHandleTemplate>` bölümünü `ReorderList` denetimi) ne zaman sürükleyin tanıtıcı gezinen fare işaretçisini uygun şekilde değiştirir emin olur:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Son olarak, bir `ScriptManager` denetim sayfa için ASP.NET AJAX başlatır:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Bu örneği tarayıcıda çalıştırmak ve liste öğeleri biraz yeniden düzenleyebilirsiniz. Ardından, sayfayı yeniden yükleyin ve/veya veritabanı bakın. Değiştirilen konumları tutulmuş ve değerler de yansıtılır `position` veritabanı ve sütun herhangi kodu olmadan tüm yeni markup kullanarak.


[![Veritabanı değişikliklerini yeni liste öğesi sırası göre verileri](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Veritabanı değişiklikleri yeni listesine göre veri öğesi sipariş ([tam boyutlu görüntüyü görüntülemek için tıklatın](drag-and-drop-via-reorderlist-vb/_static/image9.png))

>[!div class="step-by-step"]
[Önceki](using-postbacks-with-reorderlist-vb.md)
