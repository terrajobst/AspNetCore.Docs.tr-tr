---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: "2. Kısım: Veri erişim katmanı | Microsoft Docs"
author: JoeStagner
description: "Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 2. kısım, veri erişim katmanı ekleme kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a>2. Kısım: Veri erişim katmanı
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 2. kısım, veri erişim katmanı ekleme kapsar.


## <a id="_Toc260221668"></a>Veri erişim katmanı ekleme

E-ticaret uygulamamız iki veritabanlarında bağlıdır.

Standart ASP.NET üyelik veritabanı için müşteri bilgileri kullanacağız. Bizim alışveriş sepeti ve ürün kataloğu için şu SQL Express veritabanı gibi uygulamanız.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Veritabanı (Commerce.mdf) uygulamanın uygulamada oluşturduktan\_ilerlemeden .NET Entity Framework kullanarak bizim veri erişim katmanı oluşturmak için veri klasörü.

Adlı bir klasör oluşturacağız "veri\_erişim" ve bunları bu klasörü sağ tıklatın ve "Yeni Öğe Ekle" seçin.

"Yüklenmiş şablonlarda" öğesini ve ardından "ADO.NET varlık veri modeli" EDM girin\_Commerce.edmx adı olarak ve "Ekle" düğmesini tıklatın.

![](tailspin-spyworks-part-2/_static/image2.jpg)

"Veritabanı oluştur"'i seçin.

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Oluşturma ve kaydedin.

Şimdi biz ilk özelliğimizi – ürün kategorisi menüsünden eklemek hazır olursunuz.

>[!div class="step-by-step"]
[Önceki](tailspin-spyworks-part-1.md)
[sonraki](tailspin-spyworks-part-3.md)
