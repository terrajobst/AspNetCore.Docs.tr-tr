---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "3. Kısım: Düzeni ve kategorisi menüsünden | Microsoft Docs"
author: JoeStagner
description: "Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 3 ekleme düzeni ve kategorisi menüsünden kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a>3. Kısım: Düzeni ve kategori menüsü
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 3 ekleme düzeni ve kategorisi menüsünden kapsar.


## <a id="_Toc260221669"></a>Bazı düzeni ve bir kategori menüsü ekleme

Div bizim ürün kategorisi menüsünden içerecek soluna sütun için bizim site ana sayfasında ekleyeceğiz.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

Bizim Style.css dosyasına eklediğimiz CSS sınıfı tarafından istenen hizalama ve diğer biçimlendirme sağlanacak unutmayın.

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

Ürün kategorisi menüsünden, var olan ürün kategorileri ve menü öğelerini oluşturma ve karşılık gelen bağlantıları için ticaret veritabanı sorgulayarak çalışma zamanında dinamik olarak oluşturulur.

Bunu gerçekleştirmek için'de iki ASP kullanacağız. NET'in güçlü veri denetimleri. "Varlık veri kaynağı" ve "ListView" denetimi.

![](tailspin-spyworks-part-3/_static/image1.jpg)

Şimdi "Tasarım görünümüne" ve Yardımcıları bizim denetimleri yapılandırmak için kullanın.

![](tailspin-spyworks-part-3/_static/image2.jpg)

Şimdi EntityDataSource ID özelliği için EDS ayarlamak\_kategori\_menüsüne ve ardından "Veri kaynağında yapılandırmak".

![](tailspin-spyworks-part-3/_static/image3.jpg)

Varlık veri kaynağı modeli ticaret Veritabanımıza oluşturduğumuz yükleyen bize için oluşturulmuş CommerceEntities bağlantıyı seçin ve "İleri" yi tıklatın.

![](tailspin-spyworks-part-3/_static/image4.jpg)

"Kategoriler" varlık kümesi adı seçin ve diğer seçenekleri varsayılan olarak bırakın. "Son" düğmesini tıklatın.

Şimdi şimdi biz sayfamızı ListView getirilen ListView denetimi örneği kimliği özelliğini ayarlayın\_ProductsMenu ve kendi Yardımcısını etkinleştirin.

![](tailspin-spyworks-part-3/_static/image5.jpg)

Ancak biz veri öğesi görünümünü biçimlendirmek için denetim seçeneklerini kullanabilirsiniz ve kod kaynağı görünümünde girer şekilde biçimlendirme, bizim menü oluşturma yalnızca basit bir biçimlendirme ister.

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

Lütfen "Eval" deyimi unutmayın: &lt;% # Eval("CategoryName") %&gt;

ASP.NET sözdizimi &lt;% # %&gt; ne olursa olsun içinde yer alır ve sonuçları "satır" çıktı yürütmek için çalışma zamanı yönlendiren bir toplu kuralıdır.

Veri öğelerinin, bağlı koleksiyonun geçerli girişi için bildirir Eval("CategoryName") deyimi, varlık modeli öğe adları değerini "CatagoryName" getirin. Bu çok güçlü bir özellik kısa sözdizimi aşağıdaki gibidir.

Uygulamayı Şimdi Çalıştır olanak sağlar.

![](tailspin-spyworks-part-3/_static/image6.jpg)

Bizim ürün kategorisi menüsünden şimdi görüntülenir ve ProductsList.aspx adlı zaman biz biz menü öğesi bağlantı noktalarına henüz uygulamak için sahip olduğumuz bir sayfa görebilirsiniz kategori menü öğeleri birinin üzerine getirin ve içeren bir dinamik sorgu dizesi bağımsız değişkeni, oluşturduğumuz olduğunu unutmayın  Kategori Kimliği.

>[!div class="step-by-step"]
[Önceki](tailspin-spyworks-part-2.md)
[sonraki](tailspin-spyworks-part-4.md)
