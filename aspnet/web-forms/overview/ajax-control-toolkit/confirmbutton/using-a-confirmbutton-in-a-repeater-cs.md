---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: (C#) Repeater'da ConfirmButton kullanma | Microsoft Docs
author: wenz
description: Evet, AJAX Denetim Araç Seti ConfirmButton genişletici oluşturur/Kullanıcı bir düğmeyi tıkladığında açılır penceresi (LinkButton denetim dahil). Yalnızca Evet ise...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: fd0cdde5db44d1437000008f1fb5e7b13f0ca77a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805242"
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a>(C#) Repeater'da ConfirmButton kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indir](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)

> Evet, AJAX Denetim Araç Seti ConfirmButton genişletici oluşturur/Kullanıcı bir düğmeyi tıkladığında açılır penceresi (LinkButton denetim dahil). Yalnızca Evet tıklandığında, düğmenin eylemi, aksi takdirde iptal yürütülür. Bu aynı zamanda repeater'da mümkündür.


## <a name="overview"></a>Genel Bakış

Evet, AJAX Denetim Araç Seti ConfirmButton genişletici oluşturur/Kullanıcı bir düğmeyi tıkladığında açılır penceresi (LinkButton denetim dahil). Yalnızca Evet tıklandığında, düğmenin eylemi, aksi takdirde iptal yürütülür. Bu aynı zamanda repeater'da mümkündür.

## <a name="steps"></a>Adımlar

İlk olarak bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express sürüm dahil) bir Visual Studio yüklemesi isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir indirme olarak kullanılabilir [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanı SQL Server 2005 örnekleri ve örnek veritabanları parçasıdır (adresinden [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu, veritabanını ayarlamak için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve ekleme `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulumu da budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX Denetim Araç Seti ve işlevlerini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

Ardından, bir veri kaynağı gereklidir. Basitleştirmek amacıyla, yalnızca ilk beş girişleri satıcıları tablosunda AdventureWorks alınır. Tablo adı veri kaynağını oluşturmak için Visual Studio Sihirbazı'nı kullanırken dikkat edin (`Vendors`) şu anda doğru ön ekine sahip değil `Purchasing`. Aşağıdaki biçimlendirmede doğru olur:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

Bu veri kaynağı, bir yineleyici içinde kullanılabilir. Her zamanki şekilde `DataBinder.Eval()` yöntemi, veri kaynağından veri alır. `ConfirmButtonExtender` Denetim sonra yerleştirilmelidir içinde `<ItemTemplate>` BT'nin veri kaynağındaki her giriş için görünmesi yineleyicinin bölümü.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


[![Her giriş veri kaynağından yanında Onayla düğmesi görünür](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)

Her giriş veri kaynağından yanında Onayla düğmesi görünür ([tam boyutlu görüntüyü görmek için tıklatın](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Next](using-a-confirmbutton-in-a-repeater-vb.md)
