---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: (C#) veritabanı verilerinin tablosunu görüntüleme | Microsoft Docs
author: microsoft
description: Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem göstermektedir. Ben bir HTML veritabanı kayıt kümesini veri biçimlendirme için iki yöntem göster...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: d0d3f6a574a4b923d5da73ccb2ab3bfbd6f305ef
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754431"
---
<a name="displaying-a-table-of-database-data-c"></a>(C#) veritabanı verilerinin tablosunu görüntüleme
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem göstermektedir. Ben bir HTML tablosu veritabanı kayıtları bir dizi biçimlendirme için iki yöntem gösterir. İlk olarak, doğrudan bir görünüm içindeki veritabanı kayıtlarını nasıl biçimlendirebilirsiniz ı gösterir. Ardından, ben nasıl veritabanı kayıtlarını biçimlendirilirken kısmi avantajlarından yararlanabilirsiniz gösterir.


Bu öğreticide bir ASP.NET MVC uygulamasındaki HTML tablosu veritabanı verilerinin nasıl görüntüleyebilir açıklamak için hedefidir. İlk olarak, bir kayıt kümesi otomatik olarak görüntüleyen bir görünüm oluşturmak için Visual Studio'ya dahil olan yapı iskelesi araçları kullanmayı öğrenin. Ardından, kısmi bir şablon olarak veritabanı kayıtlarını biçimlendirilirken kullanmayı öğrenin.

## <a name="create-the-model-classes"></a>Model sınıfları oluşturma

Film veritabanı tablosundan kayıt kümesini görüntülemek için kullanacağız. Film veritabanı tablo şu sütunları içerir:

<a id="0.3_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(200) | False |
| Direktörü | NVarchar(50) | False |
| DateReleased | DateTime | False |


ASP.NET MVC uygulamamıza filmler tablosunda göstermek için bir model sınıfı oluşturmak ihtiyacımız var. Bu öğreticide, bizim model sınıfları oluşturmak için Microsoft Entity Framework kullanın.

> [!NOTE] 
> 
> Bu öğreticide, Microsoft Entity Framework kullanırız. Ancak, bir veritabanından LINQ to SQL veya NHibernate ADO.NET dahil olmak üzere bir ASP.NET MVC uygulaması ile etkileşim kurmak için çeşitli farklı teknolojiler kullanabileceğinizi anlamak önemlidir.


Varlık veri modeli Sihirbazı başlatmak için aşağıdaki adımları izleyin:

1. Çözüm Gezgini penceresinde ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.
2. Seçin **veri** kategori seçip alt **ADO.NET varlık veri modeli** şablonu.
3. Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.

Ekle düğmesine tıkladıktan sonra (bkz. Şekil 1) varlık veri modeli Sihirbazı görüntülenir. Sihirbazı tamamlamak için aşağıdaki adımları izleyin:

1. İçinde **Choose Model Contents** adım, select **veritabanından Oluştur** seçeneği.
2. İçinde **veri bağlantınızı seçin** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları için. Tıklayın **sonraki** düğmesi.
3. İçinde **veritabanı nesnelerinizi seçin** adım, tablolar düğümünü genişletin, filmler tabloyu seçin. Ad alanı girin *modelleri* tıklatıp **son** düğmesi.


[![SQL sınıflarına LINQ oluşturma](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Şekil 01**: LINQ to SQL sınıfları oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image2.png))


Varlık veri modeli Sihirbazı tamamladıktan sonra varlık veri modeli Tasarımcısı açılır. Tasarımcı filmler varlık görüntülenmesi gerekir (bkz: Şekil 2).


[![Varlık veri modeli Tasarımcısı](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Şekil 02**: Varlık veri modeli Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image4.png))


Biz devam etmeden önce bir değişiklik yapmak ihtiyacımız var. Varlık veri Sihirbazı adlı bir model sınıfı oluşturur *filmler* film veritabanı tablosunu temsil eden. Filmler sınıfı belirli bir filmi temsil etmek için kullanacağız çünkü olması için sınıfın adını değiştirmek ihtiyacımız *film* yerine *filmler* (tekil yerine çoğul).

Tasarımcı yüzeyinde sınıf adına çift tıklayın ve sınıfın adını film film değiştirin. Bu değişikliği yaptıktan sonra tıklayın **Kaydet** (disket simgesi) düğmesine film sınıfı oluşturun.

## <a name="create-the-movies-controller"></a>Denetleyici filmler oluşturun

Veritabanı Kayıtlarımız temsil etmek için bir yolunu sunuyoruz, bir denetleyici filmler koleksiyonunu döndürür oluşturabiliriz. Visual Studio Çözüm Gezgini penceresinde denetleyicileri klasörü sağ tıklatın ve menü seçeneğini **Ekle, denetleyici** (bkz: Şekil 3).


[![Menü denetleyicisi ekleme](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Şekil 03**: denetleyicisi menü Ekle ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image6.png))


Zaman **denetleyici Ekle** iletişim kutusu açılır, denetleyici adı MovieController girin (bkz: Şekil 4). Tıklayın **Ekle** düğmesini yeni denetleyici ekleyin.


[![Denetleyici Ekle iletişim kutusu](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Şekil 04**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image8.png))


Biz, böylece veritabanı kayıt kümesini döndürür film denetleyici tarafından kullanıma sunulan İNDİS() eylem değiştirmeniz gerekir. Denetleyici Denetleyici 1 listeleme benzer şekilde değiştirin.

**1 – Controllers\MovieController.cs listeleme**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Listeleme 1'de MoviesDBEntities sınıfı MoviesDB veritabanı temsil etmek için kullanılır. Bu sınıf kullanmak için bu gibi MvcApplication1.Models ad alanı içeri aktarmanız gerekir:

MvcApplication1.Models kullanma;

İfade *varlıklar. MovieSet.ToList()* film veritabanı tablosundan tüm filmlere kümesini döndürür.

## <a name="create-the-view"></a>Görünüm oluşturma

HTML tablosu halinde veritabanı kayıt kümesini görüntülemek için en kolay yolu, Visual Studio tarafından sağlanan yapı iskelesi yararlanmak sağlamaktır.

Menü seçeneği seçerek uygulamanızı **yapı, yapı çözümü**. Uygulamanızı açmadan önce derlemelidir **Görünüm Ekle** iletişim kutusu veya veri sınıflarınızı iletişim kutusunda görünmez.

Menü seçeneği İNDİS() eylem sağ tıklayıp **Görünüm Ekle** (bkz: Şekil 5).


[![Görünüm ekleme](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Şekil 05**: Görünüm ekleme ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image10.png))


İçinde **Görünüm Ekle** iletişim kutusunda etiketli onay **kesin türü belirtilmiş görünüm oluşturmak**. Film sınıfı olarak seçin **görüntülemek veri sınıfı**. Seçin *listesi* olarak **içeriği görüntüle** (bkz. Şekil 6). Bu seçenekleri filmler listesini görüntüleyen bir kesin türü belirtilmiş görünüm oluşturur.


[![Görünüm Ekle iletişim kutusu](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Şekil 06**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image12.png))


Tıkladıktan sonra **Ekle** 2 liste görünümünde düğme otomatik olarak oluşturulur. Bu görünüm, her bir filmi özelliklerini görüntüler ve filmler toplulukta tekrarlama için gerekli kodu içerir.

**2 – Views\Movie\Index.aspx listeleme**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Menü seçeneğini belirleyerek uygulamayı çalıştırabilirsiniz **hata ayıklama, hata ayıklamayı Başlat** (veya F5 tuşuna basarak). Uygulamayı çalıştıran, Internet Explorer başlatır. Ardından /Movie URL'sine gidin, Şekil 7'de bir sayfa görürsünüz.


[![Filmler tablosu](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Şekil 07**: filmler tablosu ([tam boyutlu görüntüyü görmek için tıklatın](displaying-a-table-of-database-data-cs/_static/image14.png))


Şekil 7'de veritabanı kayıtlarını kılavuz görünümünü ilgili hiçbir şeyi beğenmezseniz dizin görünümünün yalnızca değiştirebilirsiniz. Örneğin, değiştirebileceğiniz *DateReleased* başlığına *yayımlanma tarihi* dizin görünümünün değiştirerek.

## <a name="create-a-template-with-a-partial"></a>İle kısmi bir şablon oluşturma

Çok karmaşık bir görünümünü alır, kısmi görünüm bozucu başlatmak için iyi bir fikirdir. Kısmi bölümleri kullanarak kendi görünümlerinizi anlamak ve sürdürmek daha kolay hale getirir. Kısmi, oluşturacağız film veritabanı kayıtların her birinde biçimlendirmek için bir şablon olarak kullanabiliriz.

Kısmi oluşturmak için aşağıdaki adımları izleyin:

1. Views\Movie klasörü sağ tıklatın ve menü seçeneğini **Görünüm Ekle**.
2. Etiketli onay *(.ascx) kısmi görünüm oluşturma*.
3. Kısmi ad *MovieTemplate*.
4. Etiketli onay **kesin türü belirtilmiş görünüm oluşturmak**.
5. Film olarak seçin *görüntülemek veri sınıfı*.
6. Boş olarak işaretleyin *içeriği görüntüle*.
7. Tıklayın **Ekle** düğmesini kısmi projenize ekleyin.

Bu adımları tamamladıktan sonra 3 listeleme gibi aramak için kısmi MovieTemplate değiştirin.

**3 – Views\Movie\MovieTemplate.ascx listeleme**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Kısmi listeleme 3'te kayıt tek bir satır için bir şablonu içerir.

Listeleme 4'te değiştirilmiş dizini görünümü, kısmi MovieTemplate kullanır.

**4 – Views\Movie\Index.aspx listeleme**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

Görünüm Listesi 4'te filmler tümünün yinelenen bir foreach döngüsü içeriyor. Her bir filmi kısmi MovieTemplate film biçimlendirmek için kullanılır. MovieTemplate RenderPartial() yardımcı yöntemini çağırarak işlenir.

Veritabanı kayıtlarını çok aynı HTML tablosu değiştirilmiş Index görünümünü işler. Ancak, görünümü önemli ölçüde basitleştirilmiştir.


Bir dize döndürmediğinden RenderPartial() yöntemi diğer yardımcı yöntemlerinin bir çoğu uygulamadan farklıdır. Bu nedenle, RenderPartial() yöntemi kullanarak çağırmalıdır &lt;% Html.RenderPartial(); %&gt; yerine &lt;% Html.RenderPartial(); = %&gt;.


## <a name="summary"></a>Özet

Bu öğreticinin amacı, bir veritabanı kayıt kümesi ile HTML tablosu halinde nasıl görüntüleyebileceğiniz açıklamak için oluştu. İlk olarak, Microsoft Entity Framework avantajlarından yararlanarak veritabanı kayıt kümesini bir denetleyici eylemi dönüş hakkında bilgi edindiniz. Ardından, otomatik olarak öğelerin bir koleksiyonunu görüntüleyen bir görünüm oluşturmak için Visual Studio yapı iskelesi kullanmayı öğrendiniz. Son olarak, kısmi avantajlarından yararlanarak görünümü basitleştirmek hakkında bilgi edindiniz. Kısmi bir şablon olarak kullanabilir, böylece her veritabanı kaydı biçimlendirebilirsiniz öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](creating-model-classes-with-linq-to-sql-cs.md)
> [İleri](performing-simple-validation-cs.md)
