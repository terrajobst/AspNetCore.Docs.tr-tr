---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Veritabanı oluşturma | Microsoft Docs
author: shanselman
description: ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868080"
---
<a name="creating-a-database"></a>Veritabanı oluşturma
====================
tarafından [Scott Hanselman](https://github.com/shanselman)

> ASP.NET MVC temelleri tanıtır bir başlangıç Öğreticisi budur. Okuyan ve yazan bir veritabanından basit bir web uygulaması oluşturacaksınız. Ziyaret [ASP.NET MVC öğrenme Merkezi](../../../index.md) diğer ASP.NET MVC öğreticiler ve örnekleri bulunamadı.


Bu bölümde biz depolamak ve film verilerimizi almak için kullanacağınız yeni bir SQL Express veritabanı oluşturmak için adımıdır. Görünüm seçin ve Visual Web Developer IDE içinde | Sunucu Gezgini. Veri bağlantılarını sağ tıklayın ve bağlantı Ekle tıklatın...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

Veri Kaynağı Seç iletişim kutusunda, Microsoft SQL Server'ı seçin ve devam et seçin.

![](getting-started-with-mvc-part4/_static/image2.png)

Bağlantı Ekle iletişim kutusuna girin ". \SQLEXPRESS" sunucu adınızı ve "Filmler" Yeni veritabanı adı girin.

[![Bağlantı iletişim kutusu ekleme](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Tamam'ı tıklatın ve bu veritabanını oluşturmak isteyip istemediğiniz sorulur. Evet'i seçin.

[![Film oluşturulsun mu?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Artık sunucu Gezgini'nde boş bir veritabanı açıyor.

![Yeni tablo ekleme](getting-started-with-mvc-part4/_static/image7.png)

Tablolarda sağ tıklatıp Tablo Ekle öğesini tıklatın. Tablo Tasarımcısı görünür. Sütun kimliği, başlık, ReleaseDate, tarzını ve fiyat ekleyin. Kimlik sütunu sağ tıklayın ve birincil anahtar tıklatın ayarlayın. Burada, gibi görünüyor hangi my tasarım alanlarda verilmiştir.

[![Veritabanı Tablosu Düzenleyicisi](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Ayrıca, kimlik sütunu seçin ve aşağıdaki sütun özellikleri altında "Kimlik belirtimi" "Evet" olarak değiştirin.

[![IsIdentity - sütun özellikleri](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Bitti var olduğunda, araç çubuğunda Kaydet simgesine tıklayın veya dosyayı seçin | Menüden kaydedin ve tablonuzun adının "**film**" (tekil). Bir veritabanı ve tablo var ki!

[![Bir ad seçin](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Sunucu Gezgini için geri dönün ve film tablo sağ tıklayın ve ardından "Tablo verileri göster." Veritabanımıza bazı veriler nedenle birkaç filmler girin.

[![Veritabanı tablosu düzenleme](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Model oluşturma

Şimdi, geçin geri IDE'nin sağ tarafındaki Çözüm Gezgini ve modeller klasörü sağ tıklatın ve Ekle | Yeni öğe.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Bizim yeni veritabanından bir varlık modeli oluşturmak için yapacağız. Bu sınıf kümesi bize sorgulamak ve bizim veritabanındaki verileri işlemek daha kolay hale gelir Projemizin ekler. İletişim kutusunun sol taraftaki veri düğümü seçin ve ardından ADO.NET varlık veri modeli öğesi şablonu seçin. Movies.edmx adı.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

"Ekle" düğmesini tıklatın. Bu, ardından "varlık veri modeli Sihirbazı" başlatacak.

Açılır yeni iletişim kutusunda, veritabanından Oluştur'u seçin. Bir veritabanı yalnızca yaptık olduğundan, biz yalnızca Entity Framework bizim yeni bir veritabanı ve onun tablo hakkında söylememiz gerekir. Yanına bizim veritabanı bağlantısı bizim web uygulamasının yapılandırma Kaydet'i tıklatın. Şimdi, tablolar ve film denetle bitiş onay kutusunu ve'ı tıklatın.

[![Varlık veri modeli Sihirbazı](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Şimdi biz bizim yeni Entity Framework Designer film tabloya bakın ve kodundan erişim.

[![Film - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Tasarım yüzeyine "Film" sınıfı görebilirsiniz. Bu sınıf Veritabanımıza "Film" tablosunda eşler ve tablo içeren bir sütuna içindeki her bir özellik eşler. "Film" sınıfın her örneği "Film" tablo içindeki satır karşılık gelir.

Varsayılan adlandırma ve Entity Framework tarafından kullanılan kuralları eşleme hoşlanmıyorsanız, değiştirmek veya bunları özelleştirmek için Entity Framework Tasarımcısı'nı kullanabilirsiniz. Bu uygulama için biz varsayılanları kullanın ve yalnızca dosyası olarak Kaydet-değil.

Şimdi, şimdi bazı gerçek verilerle çalışmak!

> [!div class="step-by-step"]
> [Önceki](getting-started-with-mvc-part3.md)
> [sonraki](getting-started-with-mvc-part5.md)
