---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Veritabanı oluşturma | Microsoft Docs
author: shanselman
description: ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: fff974edfaffeb164879c10b8e70bd3482c47554
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813766"
---
<a name="creating-a-database"></a>Veritabanı oluşturma
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC ile ilgili temel bilgileri tanıtan bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC eğitim Merkezi](../../../index.md) diğer ASP.NET MVC, öğreticilerimiz ve örneklerimizden bulunacak.


Bu bölümde, depolama ve alma film verilerimizi kullanacağız yeni bir SQL Express veritabanı oluşturmak için kullanacağız. Visual Web Developer IDE içinde görünümü seçin | Sunucu Gezgini. Veri bağlantılarını sağ tıklayın ve Bağlantısı Ekle...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Veri Kaynağı Seç iletişim kutusunda, Microsoft SQL Server'ı seçin ve devam'ı seçin.

![](getting-started-with-mvc-part4/_static/image2.png)

Bağlantı Ekle iletişim kutusuna girin ". \SQLEXPRESS" sunucu adınız ve yeni veritabanınızın adı olarak "Filmler" girin.

[![Bağlantısı Ekle iletişim kutusu](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Tamam'ı tıklatın ve o veritabanı oluşturmak istiyorsanız istenir. Evet'i seçin.

[![Filmler oluşturulsun mu?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Şimdi, boş bir veritabanı sunucu Gezgini'nde aradığınızı bulacaksınız.

![Yeni Tablo Ekle](getting-started-with-mvc-part4/_static/image7.png)

Tablolarda sağ tıklayın ve Tablo Ekle'ı tıklatın. Tablo Tasarımcısı görüntülenir. Kimlik, başlık, ReleaseDate, tarz ve fiyat için sütunları ekleyin. Kimlik sütunu üzerinde sağ tıklayın ve birincil anahtar tıklatın ayarlayın. Gibi görünüyor hangi benim tasarım alanları aşağıda verilmiştir.

[![Veritabanı Tablosu Düzenleyicisi](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Ayrıca, kimlik sütunu seçin ve aşağıdaki sütun özellikleri altında "Kimlik belirtimi" "Evet" olarak değiştirin.

[![IsIdentity - sütun özellikleri](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Bitti Başardınız, araç çubuğunda Kaydet simgesine tıklayın veya dosyayı seçin | Menüden kaydedin ve tablonuzun adı "**film**" (tekil). Bir veritabanı ve tablo yapılandırdığımıza göre!

[![Bir ad seçin](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Sunucu Gezgini geri dönün ve film tablo sağ tıklayın ve ardından "Show tablo verilerini." Bazı veriler veritabanımızdaki sahip olması birkaç filmler girin.

[![Veritabanı tablo düzenleme](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Model oluşturma

Artık, Çözüm Gezgini'nde IDE'nin sağ taraftaki geri dön geçin ve modeller klasörü sağ tıklatın ve Ekle'yi seçin | Yeni öğe.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Yeni sunduğumuz veritabanından bir varlık modeli oluşturmak için ekleyeceğiz. Bu bizim veritabanımızdaki içinde veri sorgulama ve düzenleme için kolaylaştıran Projemizin sınıf kümesi ekleyeceksiniz. İletişim kutusunun sol tarafında veri düğümünü seçin ve ardından ADO.NET varlık veri modeli öğe şablonu seçin. Movies.edmx adlandırın.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

"Ekle" düğmesine tıklayın. Bu, ardından "varlık veri modeli Sihirbazı" başlatılır.

Açılan yeni iletişim kutusunda, veritabanından Oluştur'u seçin. Bir veritabanı yalnızca yaptık olduğundan, biz yalnızca Entity Framework sunduğumuz yeni bir veritabanı ve onun tablo hakkında söylemeniz gerekir. Yanındaki bizim sunduğumuz web uygulamanın yapılandırma veritabanı bağlantısında Kaydet'e tıklayın. Şimdi, tablolar ve film kontrol onay kutusuna tıklayın ve son.

[![Varlık veri modeli Sihirbazı](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Şimdi biz Entity Framework Designer yeni film tablodaki bakın ve koddan erişebilirsiniz.

[![Filmler - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Tasarım yüzeyinde, bir "Film" sınıf görebilirsiniz. Bu sınıf veritabanımızdaki "Film" tablosunda eşlenir ve içindeki her bir özellik bir sütun tablosu ile eşlenir. "Film" sınıfın her örneğini "Film" tablo içindeki satır karşılık gelir.

Varsayılan adlandırma ve Entity Framework tarafından kullanılan kuralları eşleme kullanmak istemiyorsanız, değiştirmek veya bunları özelleştirmek için Entity Framework designer'ı kullanabilirsiniz. Bu uygulama için biz varsayılan ayarları kullanın ve yeni dosyayı farklı kaydet-olduğu.

Şimdi, bazı gerçek verilerle birlikte çalışalım!

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part3.md)
> [İleri](getting-started-with-mvc-part5.md)
