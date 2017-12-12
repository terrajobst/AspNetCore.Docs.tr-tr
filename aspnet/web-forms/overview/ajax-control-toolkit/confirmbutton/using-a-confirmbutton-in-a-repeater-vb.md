---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: "Yineleyici (VB) içinde bir ConfirmButton kullanarak | Microsoft Docs"
author: wenz
description: "AJAX Denetim araç setindeki ConfirmButton genişletici Evet oluşturur/Kullanıcı bir düğmesine tıkladığında herhangi bir açılır pencere (LinkButton denetim dahil). Yalnızca Evet ise..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 5da8491c157ad6f35c2c25803680f262a35ce6e1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a>Yineleyici (VB) içinde bir ConfirmButton kullanma
====================
tarafından [Christian Wenz](https://github.com/wenz)

[Kodu indirme](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) veya [PDF indirin](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)

> AJAX Denetim araç setindeki ConfirmButton genişletici Evet oluşturur/Kullanıcı bir düğmesine tıkladığında herhangi bir açılır pencere (LinkButton denetim dahil). Yalnızca Evet tıklandığında, düğmenin eylemini, aksi takdirde iptal yürütülür. Bu aynı zamanda bir yineleyici mümkündür.


## <a name="overview"></a>Genel Bakış

AJAX Denetim araç setindeki ConfirmButton genişletici Evet oluşturur/Kullanıcı bir düğmesine tıkladığında herhangi bir açılır pencere (LinkButton denetim dahil). Yalnızca Evet tıklandığında, düğmenin eylemini, aksi takdirde iptal yürütülür. Bu aynı zamanda bir yineleyici mümkündür.

## <a name="steps"></a>Adımlar

Öncelikle, bir veri kaynağı gereklidir. Bu örnekte, AdventureWorks veritabanını ve Microsoft SQL Server 2005 Express Edition kullanır. Veritabanı (express edition dahil) bir Visual Studio yükleme isteğe bağlı bir parçasıdır ve ayrıca altında ayrı bir yükleme olarak kullanılabilir [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). AdventureWorks veritabanını SQL Server 2005 örnekler ve örnek veritabanları parçasıdır (adresinden [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). En kolay yolu veritabanını için Microsoft SQL Server Management Studio Express kullanmaktır ([https://www.microsoft.com/downloads/details.aspx? FamilyID c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796 =&amp;DisplayLang = tr](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) ve attach `AdventureWorks.mdf` veritabanı dosyası.

Bu örnek için SQL Server 2005 Express Edition'ın örneğini çağrıldığından emin olan varsayıyoruz `SQLEXPRESS` ve web sunucusu; ile aynı makinede bulunan varsayılan kurulum de budur. Kurulumunuzu farklıysa, veritabanı için bağlantı bilgilerini uymak zorunda.

ASP.NET AJAX ve Denetim Araç Seti işlevselliğini etkinleştirmek için `ScriptManager` denetim gerekir yerleştirmek herhangi bir yere sayfada (ancak içinde `<form>` öğesi):

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

Ardından, bir veri kaynağı gereklidir. Basitleştirmek amacıyla, yalnızca ilk beş girişlerini AdventureWorks satıcılar tablosundaki alınır. Veri kaynağı, tablo adı oluşturmak için Visual Studio Sihirbazı'nı kullanırken dikkat edin (`Vendors`) şu anda doğru ile eklenmedi `Purchasing`. Aşağıdaki biçimlendirmede doğru olur:

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

Bu veri kaynağı yineleyici içinde kullanılabilir. Her zamanki gibi `DataBinder.Eval()` yöntemi veri kaynağından veri alır. `ConfirmButtonExtender` Denetim sonra yerleştirilmelidir içinde `<ItemTemplate>` olan her veri kaynağı girişi için görünmesi yineleyici bölümü.

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


[![Her giriş veri kaynağından yanındaki Onayla düğmesi görünür](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)

Her giriş veri kaynağından yanındaki Onayla düğmesi görünür ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))

>[!div class="step-by-step"]
[Önceki](using-a-confirmbutton-in-a-repeater-cs.md)
