---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: '2. Bölüm: Veri erişim katmanı | Microsoft Docs'
author: JoeStagner
description: Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 2. bölüm, veri erişim katmanı ekleyerek kapsar.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5b5ae08b157054bf0f3231782127ee3110f36d00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757242"
---
<a name="part-2-data-access-layer"></a>2. Bölüm: Veri erişim katmanı
====================
tarafından [ALi Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklenebilir uygulamalar oluşturmak için nasıl çok basit olduğunu gösterir. Bu, alışveriş, kullanıma alma ve yönetim gibi bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici serisinin tüm Tailspin Spyworks örnek uygulamayı oluşturmak için gerçekleştirilen adımlar ayrıntılı olarak açıklanmaktadır. 2. bölüm, veri erişim katmanı ekleyerek kapsar.


## <a id="_Toc260221668"></a>  Veri erişim katmanı ekleme

E-ticaret uygulamamız iki veritabanlarında bağlıdır.

Standart ASP.NET üyelik veritabanı müşteri bilgileri kullanacağız. Alışveriş sepeti ve ürün kataloğu için size bir SQL Express veritabanı gibi uygulayacaksınız.

![](tailspin-spyworks-part-2/_static/image1.jpg)

' % S'veritabanı (Commerce.mdf) uygulamanın uygulamada oluşturduğunuza\_biz devam edebilirsiniz, veri erişim katmanı .NET Entity Framework kullanarak oluşturmak için veri klasörü.

Adlı bir klasör oluşturacağız "veri\_erişim" ve bunları bu klasörü sağ tıklatın ve "Yeni Öğe Ekle"'i seçin.

"Yüklü şablonlarında" öğesini ve ardından "ADO.NET varlık veri modeli" EDM girin\_Commerce.edmx adı olarak "Ekle" düğmesine tıklayın.

![](tailspin-spyworks-part-2/_static/image2.jpg)

"Veritabanından oluştur" öğesini seçin.

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Kaydet ve derleyin.

Artık ilk özelliğimiz – bir ürün kategorisini menü eklemek hazırız.

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-1.md)
> [İleri](tailspin-spyworks-part-3.md)
