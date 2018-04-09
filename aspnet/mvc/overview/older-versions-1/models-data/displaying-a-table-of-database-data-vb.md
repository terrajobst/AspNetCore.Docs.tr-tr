---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Bir tablo veritabanı verilerinin (VB) görüntüleyen | Microsoft Docs
author: microsoft
description: Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem gösterilmektedir. I HTML veritabanı kayıt kümesinin eri biçimlendirme için iki yöntem göster...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: dd871520f98aaae2d7b33d83b1646eb9eee88821
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="displaying-a-table-of-database-data-vb"></a>Bir tablo veritabanı verilerinin (VB) görüntüleme
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> Bu öğreticide, ı veritabanı kayıt kümesini görüntülemek için iki yöntem gösterilmektedir. I veritabanı kayıtlarını HTML tablosunda bir dizi biçimlendirme için iki yöntem gösterir. İlk olarak, doğrudan bir görünüm içindeki veritabanı kayıtlarını nasıl biçimlendirebilirsiniz ı gösterir. Ardından, t nasıl, kısmi veritabanı kayıtlarını biçimlendirme sırasında yararlanabilir göstermektedir.


Bu öğretici, bir ASP.NET MVC uygulamasındaki HTML tablosu veritabanı verilerinin nasıl görüntüleyebilirsiniz açıklamak için hedefidir. İlk olarak, Visual Studio'daki yapı iskelesi araçları otomatik olarak bir kayıt kümesi görüntüleyen bir görünüm oluşturmak için nasıl kullanılacağını öğrenin. Ardından, veritabanı kayıtlarını biçimlendirilirken bir kısmi bir şablon olarak kullanmak üzere nasıl öğrenin.

## <a name="create-the-model-classes"></a>Model sınıfları oluşturma

Film veritabanı tablosundan kayıt kümesini görüntülemek olacak. Film veritabanı tablo şu sütunları içerir:

<a id="0.4_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | False |
| Başlık | Nvarchar(200) | False |
| Director | NVarchar(50) | False |
| DateReleased | DateTime | False |


ASP.NET MVC uygulamamız filmler tabloda temsil etmek için size bir model sınıfı oluşturmanız gerekir. Bu öğreticide, bizim modeli sınıfları oluşturmak için Microsoft Entity Framework kullanın.

> [!NOTE] 
> 
> Bu öğreticide, Microsoft Entity Framework kullanırız. Ancak, bir veritabanından LINQ-SQL, NHibernate ya da ADO.NET dahil olmak üzere bir ASP.NET MVC uygulaması ile etkileşim kurmak için çeşitli farklı teknolojiler kullanabileceğinizi anlamak önemlidir.


Varlık veri modeli Sihirbazı'nı başlatmak için şu adımları izleyin:

1. Çözüm Gezgini penceresinin ve menü seçeneğini seçin modelleri klasörünü sağ tıklatın **Ekle, yeni öğe**.
2. Seçin **veri** kategori ve select **ADO.NET varlık veri modeli** şablonu.
3. Veri modelinizi adını verin *MoviesDBModel.edmx* tıklatıp **Ekle** düğmesi.

Ekle düğmesine tıkladıktan sonra (bkz: Şekil 1) varlık veri modeli Sihirbazı görünür. Sihirbazı tamamlamak için aşağıdaki adımları izleyin:

1. İçinde **Model içeriği seçin** adım, select **veritabanından Oluştur** seçeneği.
2. İçinde **veri bağlantınızı** adım, kullanın *MoviesDB.mdf* veri bağlantısı ve ad *MoviesDBEntities* bağlantı ayarları. Tıklatın **sonraki** düğmesi.
3. İçinde **veritabanı nesnelerinizi** adım, tablolar düğümünü genişletin, filmler tabloyu seçin. Ad alanı girin *modelleri* tıklatıp **son** düğmesi.


[![LINQ SQL sınıfları için oluşturma](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Şekil 01**: SQL sınıfları için oluşturma LINQ ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image2.png))


Entity Data Model Designer, varlık veri modeli Sihirbazı tamamladıktan sonra açılır. Tasarımcı filmler varlık görüntülenmelidir (bkz: Şekil 2).


[![Entity Data Model Designer](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Şekil 02**: Entity Data Model Designer ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image4.png))


Biz devam etmeden önce bir değişiklik yapmanız gerekir. Varlık veri Sihirbazı adlı bir model sınıfı oluşturur *filmler* , filmler veritabanı tablosu temsil eder. Film sınıfı belirli bir filmi temsil etmek için kullanacağız için olması için sınıf adını değiştirmek gerekiyor *film* yerine *filmler* (tekil yerine çoğul).

Sınıf Tasarımcısı yüzeyinde adına çift tıklayın ve film film sınıfın adını değiştirin. Bu değişikliği yaptıktan sonra tıklatın **kaydetmek** film sınıfı oluşturmak için (disket simgesi) düğmesine tıklayın.

## <a name="create-the-movies-controller"></a>Film denetleyicisi oluşturun

Veritabanı Kayıtlarımıza temsil etmek için bir yol sahibiz, filmler koleksiyonunu döndürür bir denetleyici oluşturabiliriz. Visual Studio Çözüm Gezgini penceresi içinde denetleyicileri klasörünü sağ tıklatın ve menü seçeneğini **Ekle, denetleyici** (bkz: Şekil 3).


[![Denetleyici menü ekleme](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Şekil 03**: denetleyicisi menü Ekle ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image6.png))


Zaman **denetleyici Ekle** iletişim kutusu görüntülenirse, denetleyici adı MovieController girin (Şekil 4'e bakın). Tıklatın **Ekle** düğmesi yeni denetleyici ekleyin.


[![Denetleyici Ekle iletişim kutusu](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Şekil 04**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image8.png))


Biz, böylece veritabanı kayıtlarını kümesini döndürür film denetleyici tarafından kullanıma sunulan İNDİS() eylem değiştirmeniz gerekir. Denetleyici denetleyicisi listeleme 1 gibi görünüyor şekilde değiştirin.

**1 – Controllers\MovieController.vb listeleme**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Listeleme 1'de MoviesDBEntities sınıfı MoviesDB veritabanı göstermek için kullanılır. İfade *varlıklar. MovieSet.ToList()* filmler veritabanı tablosundan tüm filmler kümesini döndürür.

## <a name="create-the-view"></a>Görünüm Oluştur

Visual Studio tarafından sağlanan askılama özelliğinin avantajlarından yararlanmak için bir HTML tablosunda bir veritabanı kayıt kümesini görüntülemek için en kolay yolu olan.

Menü seçeneğini seçerek uygulamanızı **yapı, yapı çözümü**. Uygulamanızı açmadan önce oluşturmanız gerekir **Görünüm Ekle** iletişim ya da veri sınıfları iletişim kutusunda görünmeyecektir.

İNDİS() eyleme sağ tıklayın ve menü seçeneğini **Görünüm Ekle** (bkz. Şekil 5).


[![Bir görünümü ekleme](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Şekil 05**: bir görünüm ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image10.png))


İçinde **Görünüm Ekle** iletişim kutusunda, etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**. Film sınıf olarak seçin **görüntülemek veri sınıfı**. Seçin *listesi* olarak **görüntülemek içerik** (bkz. Şekil 6). Bu seçenekleri filmler listesini görüntüleyen kesin türü belirtilmiş bir görünüm oluşturur.


[![Görünüm Ekle iletişim kutusu](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Şekil 06**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image12.png))


Tıklattıktan sonra **Ekle** düğmesi, listeleme 2 görünümünde otomatik olarak oluşturulur. Bu görünüm filmler toplulukta tekrarlama ve her bir filmi özelliklerini görüntülemek için gerekli kodu içerir.

**Listing 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Menü seçeneğini belirleyerek uygulamayı çalıştırabilirsiniz **hata ayıklama, hata ayıklamayı Başlat** (veya F5 tuşuna basmak). Uygulamayı çalıştıran Internet Explorer başlatır. Ardından /Movie URL'ye gidin, Şekil 7'deki sayfası görürsünüz.


[![Film tablosu](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Şekil 07**: filmler tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](displaying-a-table-of-database-data-vb/_static/image14.png))


Şekil 7'de veritabanı kayıtlarını kılavuzunun görünümünü hakkında hiçbir şey hoşlanmıyorsanız dizin görünümünün yalnızca değiştirebilirsiniz. Örneğin, değiştirebileceğiniz *DateReleased* başlığına *yayımlanma* dizin görünümünün değiştirerek.

## <a name="create-a-template-with-a-partial"></a>Kısmi ile bir şablon oluşturma

Bir görünümü çok karmaşık aldığında kısmi görünümü çiğnemekten başlatmak için iyi bir fikirdir. Kısmi kullanarak kendi görünümlerinizi anlamak ve sürdürmek daha kolay hale getirir. Kısmi, oluşturacağız biz her film veritabanı kayıtlarını biçimlendirmek için bir şablon olarak kullanabilirsiniz.

Kısmi oluşturmak için aşağıdaki adımları izleyin:

1. Views\Movie klasörünü sağ tıklatın ve menü seçeneğini **Görünüm Ekle**.
2. Etiketli onay kutusunu işaretleyin *(.ascx) kısmi görünüm oluşturma*.
3. Kısmi adını *MovieTemplate*.
4. Etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**.
5. Film olarak seçin *görüntülemek veri sınıfı*.
6. Boş olarak işaretleyin *görüntülemek içerik*.
7. Tıklatın **Ekle** düğmesi kısmi projenize ekleyin.

Bu adımları tamamladıktan sonra listeleme 3 gibi aramak için kısmi MovieTemplate değiştirin.

**3 – Views\Movie\MovieTemplate.ascx listeleme**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Kısmi listeleme 3'te kayıt tek bir satır için bir şablonu içerir.

Listeleme 4 değiştirilmiş dizin görünümünde kısmi MovieTemplate kullanır.

**Listing 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

For listeleme 4 görünümünde içeren tüm filmler tekrarlanan her bir döngü. Her film için kısmi MovieTemplate film biçimlendirmek için kullanılır. MovieTemplate RenderPartial() yardımcı yöntemini çağırarak işlenir.

Veritabanı kayıtlarını çok aynı HTML tablosu değiştirilmiş Index görünümünü işler. Ancak, görünümü önemli ölçüde basitleştirilmiştir.


Bir dize döndürmediğinden diğer yardımcı yöntemlerinin bir çoğunu RenderPartial() yöntemi farklıdır. Bu nedenle, RenderPartial() yöntemini kullanarak çağırmalısınız &lt;Html.RenderPartial() %&gt; yerine &lt;% = Html.RenderPartial() %&gt;.


## <a name="summary"></a>Özet

Bu öğreticinin amacı, bir HTML tablosunda veritabanı kayıt kümesinin nasıl görüntüleyebilirsiniz açıklamak için oluştu. İlk olarak, Microsoft Entity Framework yararlanarak bir denetleyici eylemi bir veritabanı kayıt kümesi döndürmek nasıl öğrendiniz. Ardından, Visual Studio yapı iskelesi öğeleri koleksiyonu otomatik olarak görüntüleyen bir görünüm oluşturmak için nasıl kullanılacağı hakkında bilgi edindiniz. Son olarak, kısmi yararlanarak görünümü basitleştirmek nasıl öğrendiniz. Kısmi bir şablon olarak kullanabilir, böylece her veritabanı kaydı biçimlendirebilirsiniz öğrendiniz.

> [!div class="step-by-step"]
> [Önceki](creating-model-classes-with-linq-to-sql-vb.md)
> [sonraki](performing-simple-validation-vb.md)
