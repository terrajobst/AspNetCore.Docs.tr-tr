---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Entity Framework (VB) ile model sınıfları oluşturma | Microsoft Docs
author: microsoft
description: Bu öğreticide, ASP.NET MVC Microsoft Entity Framework ile kullanmayı öğrenin. Bir ADO.NET varlık Da oluşturmak için varlık Sihirbazı'nı kullanmayı öğrenin...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: c1f64f57d4c23fe225a8268042104254e17dc456
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752734"
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Entity Framework (VB) ile model sınıfları oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu öğreticide, ASP.NET MVC Microsoft Entity Framework ile kullanmayı öğrenin. Bir ADO.NET varlık veri modeli oluşturmak için varlık Sihirbazı'nı kullanmayı öğrenin. Bu öğretici boyunca seçin, Ekle, Güncelleştir ve Entity Framework kullanarak veritabanı verileri silme işlemini gösteren bir web uygulaması ekleriz.


Bu öğreticinin amacı, bir ASP.NET MVC uygulaması oluşturma sırasında Microsoft Entity Framework kullanarak veri erişim sınıfları nasıl oluşturacağınızı açıklar sağlamaktır. Bu öğretici, Microsoft Entity Framework'ün önceki bilgi varsayar. Bu öğreticinin sonunda, Entity Framework seçin, ekleme, güncelleştirme ve veritabanı kayıtlarını silme hakkında bilgi edinin.

Microsoft Entity Framework, bir veritabanından veri erişim katmanını otomatik olarak oluşturmanızı sağlayan bir nesne ilişkisel eşleme (O/RM) bir araçtır. Entity Framework, veri erişim sınıfları el ile oluşturma yorucu bir süreç iş kaçınmanızı sağlar.

> [!NOTE] 
> 
> ASP.NET MVC ile Entity Framework Microsoft arasındaki temel bağlantı yoktur. Entity Framework, ASP.NET MVC ile kullanabileceğiniz çeşitli alternatifler vardır. Örneğin, Microsoft SQL veya NHibernate SubSonic LINQ gibi diğer O/RM araçları kullanarak MVC Model sınıflarınızı oluşturabilir.


Microsoft Entity Framework ASP.NET MVC ile nasıl kullanabileceğinizi anlamak için basit bir örnek bir uygulama oluşturacağız. Görüntülemek ve düzenlemek film veritabanı kayıtlarını olanak tanıyan bir film veritabanı uygulaması oluşturacağız.

Bu öğreticide, Visual Studio 2008 veya Visual Web Developer 2008 Service Pack 1 sahibi olduğunuzu varsayar. Service Pack 1, Entity Framework kullanmak için gerekir. Visual Studio 2008 Service Pack 1 veya hizmet paketi 1 ile Visual Web Developer şu adresten indirebilirsiniz:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Film örnek veritabanını oluşturma

Film veritabanı uygulaması şu sütunları içeren film adlı bir veritabanı tablosu kullanır:

| Sütun adı | Veri Türü | Null değerlere izin verilsin mi? | Birincil anahtar mı? |
| --- | --- | --- | --- |
| Kimliği | int | False | Doğru |
| Başlık | nvarchar(100) | False | False |
| Direktörü | nvarchar(100) | False | False |

Bu tablo, aşağıdaki adımları izleyerek bir ASP.NET MVC projesini ekleyebilirsiniz:

1. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinde veri klasörü **Ekle, yeni öğe.**
2. Gelen **Yeni Öğe Ekle** iletişim kutusunda **SQL Server veritabanı**, veritabanı MoviesDB.mdf ad verin ve tıklayın **Ekle** düğmesi.
3. Sunucu Gezgini/veritabanı Gezgini penceresi açmak için MoviesDB.mdf dosyasına çift tıklayın.
4. MoviesDB.mdf veritabanı bağlantısı genişletin tablolarını klasörü sağ tıklatın ve menü seçeneğini **Yeni Tablo Ekle**.
5. Tablo Tasarımcısı'nda, kimlik, başlık ve Direktörü sütunları ekleyin.
6. Tıklayın **Kaydet** düğmesine (disket simgesini vardır) yeni bir tablo adı filmlerle kaydetmek için.

Film veritabanı tablosu oluşturduktan sonra tabloya bazı örnek veriler eklemeniz gerekir. Menü seçeneği filmler tabloya sağ tıklayıp **tablo verilerini Göster**. Sahte film verileri görünen kılavuza girebilirsiniz.

## <a name="creating-the-adonet-entity-data-model"></a>ADO.NET varlık veri modeli oluşturma

Entity Framework kullanmak için bir varlık veri modeli oluşturmak gerekir. Visual Studio yararlanabilir *varlık veri modeli Sihirbazı* bir varlık veri modeli bir veritabanından otomatik olarak oluşturmak için.

Aşağıdaki adımları uygulayın:

1. Çözüm Gezgini penceresinde modeller klasörü sağ tıklatın ve menü seçeneğini **Ekle, yeni öğe**.
2. İçinde **Yeni Öğe Ekle** iletişim kutusunda, veri kategorisi seçin (bkz. Şekil 1).
3. Seçin **ADO.NET varlık veri modeli** şablonu, varlık veri modeli MoviesDBModel.edmx ad verin ve tıklayın **Ekle** düğmesi. Tıklayarak **Ekle** düğmesi veri modeli Sihirbazı başlatılır.
4. İçinde **Choose Model Contents** adım öğesini **bir veritabanından Oluştur** seçeneğini ve tıklayın **sonraki** (bkz: Şekil 2) düğmesi.
5. İçinde **veri bağlantınızı seçin** adım, MoviesDB.mdf veritabanı bağlantısını seçin, bağlantı ayarları MoviesDBEntities adlandırın ve tıklayın varlıkları girin **sonraki** (bkz: Şekil 3) düğmesi.
6. İçinde **veritabanı nesnelerinizi seçin** adım, film veritabanı tablosunu seçin ve tıklayın **son** (bkz: Şekil 4) düğmesi.

Bu adımları tamamladıktan sonra ADO.NET varlık veri modeli Tasarımcısı'nı (varlık Tasarımcısı) açılır.

**Şekil 1 – yeni bir varlık veri modeli oluşturma**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Şekil 2: Model içeriğini adımını seçin**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Şekil 3 – veri bağlantınızı seçin**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Şekil 4-veritabanı nesnelerinizi seçin**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>ADO.NET varlık veri modeli değiştirme

Bir varlık veri modeli oluşturduktan sonra varlık Tasarımcısı avantajlarından yararlanarak model değiştirebilirsiniz (bkz: Şekil 5). Varlık Tasarımcısı, herhangi bir zamanda Çözüm Gezgini penceresinde modelleri klasördeki MoviesDBModel.edmx dosyasına çift tıklayarak açabilirsiniz.

**5-şekil ADO.NET varlık veri modeli Tasarımcısı**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Örneğin, varlık Tasarımcısı, varlık veri modeli Sihirbazı oluşturur sınıfların adlarını değiştirmek için kullanabilirsiniz. Sihirbaz, filmler adlı yeni bir veri erişim sınıfı oluşturuldu. Diğer bir deyişle, sihirbaz sınıfı çok aynı adı taşıyan bir veritabanı tablosu getirdi. Bu sınıf belirli bir filmi örneği temsil eden kullanacağız çünkü biz filmler sınıftan film için yeniden adlandırmanız gerekir.

Bir varlık sınıfı yeniden adlandırmak isterseniz, varlık tasarımcısında sınıf adına çift tıklayın ve yeni bir ad girin (bkz. Şekil 6). Alternatif olarak, Özellikler penceresinde bir varlığın adı, varlık Tasarımcısı'nda bir varlık seçildikten sonra değiştirebilirsiniz.

**Şekil 6: Varlık adının değiştirilmesi**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Kaydet düğmesine (disket simgesi) tıklayarak bir değişiklik yaptıktan sonra varlık veri modeli kaydetmeyi unutmayın. Arka planda, varlık Tasarımcısı, Visual Basic .NET sınıf kümesi oluşturur. Bu sınıflar Çözüm Gezgini penceresinde MoviesDBModel.Designer.vb dosyasını açarak görüntüleyebilirsiniz.


Varlık Tasarımcısı kullandığınızda, değişikliklerin üzerine yazılır olduğundan Designer.vb olarak adlandırılır dosyasındaki kodu değiştirmeyin. Designer.vb olarak adlandırılır dosyasında tanımlanan varlık sınıfları genişletmek istediğiniz sonra oluşturabileceğiniz *kısmi sınıflar* içinde dosyaları'nı ayırın.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Entity Framework ile veritabanı kayıtlarını seçme

Film kayıt listesini görüntüleyen bir sayfa oluşturarak film veritabanı uygulamamızı oluşturmaya başlayalım. Giriş denetleyicisine listeleme 1 İNDİS() adlı bir eylem kullanıma sunar. İNDİS() eylemi, Entity Framework avantajlarından yararlanarak tüm film kayıtlar film veritabanı tablosundan döndürür.

**1 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Denetleyici 1 listeleyen bir oluşturucu içerdiğine dikkat edin. Oluşturucu isimli bir sınıf seviyesi alanını başlatır \_db. \_Db alan Microsoft Entity Framework tarafından oluşturulan veritabanı varlıkları temsil eder. \_Db alandır varlık tasarımcısı tarafından oluşturulan MoviesDBEntities sınıfının örneği.

\_Db alan İNDİS() eylemi içinde film veritabanı tablosundan kayıtları almak için kullanılır. İfade \_db. MovieSet, film veritabanı tablosunun tüm kayıtları temsil eder. ToList() yöntemi film nesnelerin genel bir koleksiyona filmler kümesini dönüştürmek için kullanılır:, (film) listesi.

LINQ to Entities'de yardımıyla film kayıtlar alınır. LINQ listeleme 1 İNDİS() eylemi kullanan *yöntem sözdizimi* veritabanı kayıt kümesini almak için. Tercih ederseniz, LINQ kullanabileceğiniz *sorgu söz dizimi* yerine. Aşağıdaki iki deyimi çok aynı şeyi yapar:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

En kullanışlı bulabileceğiniz hangi LINQ söz dizimi – yöntem sözdizimi veya sorgu söz dizimi – kullanın. İki yaklaşım arasında performans farkı yoktur: tek fark stili.

2 liste görünümünde film kayıtları görüntülemek için kullanılır.

**2 – Views\Home\Index.aspx listeleme**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

2 liste görünümünde içeren bir **her** döngü, her bir film kayıt yinelenir ve film kaydın başlığı ve Direktörü özelliklerin değerlerini görüntüler. Bir düzenleme ve silme bağlantısı her bir kaydın yanındaki görüntülendiğini dikkat edin. Ayrıca, film Ekle bağlantısını görünümün alt kısmında görünür (bkz. Şekil 7).

**Şekil 7 – dizini görünümü**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Dizin görünüm bir *türü belirtilmiş Görünüm*. Dizin görünümünün sahip bir &lt;% @ sayfa %&gt; Inherits özniteliği içeren yönergesi. Inherits özniteliği, türü kesin belirlenmiş genel liste film nesneler koleksiyonunu – listesini, (film) ViewData.Model özelliğini uygular.

## <a name="inserting-database-records-with-the-entity-framework"></a>Entity Framework ile veritabanı kayıtlar ekleme

Entity Framework, yeni kayıtlar bir veritabanı tablosuna eklemek kolay hale getirmek için kullanabilirsiniz. Liste 3'teki yeni kayıtlar film veritabanı tablosuna eklemek için kullanabileceğiniz ve giriş denetleyici sınıfına eklenen iki yeni eylemler içerir.

**3 – Controllers\HomeController.vb (Add yöntemleri) listeleme**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

İlk Add() eylemi yalnızca bir görünüm verir. Yeni bir film veritabanı eklemek için bir form görünümü içerir (bkz. Şekil 8) kaydedin. Formu gönderdiğinde, ikinci Add() eylemi çağrılır.

İkinci Add() eylemi AcceptVerbs özniteliği ile donatılmış, dikkat edin. Bu eylem, yalnızca bir HTTP POST işlemi gerçekleştirirken çağrılabilir. Diğer bir deyişle, bu eylem yalnızca bir HTML formu aktarırken çağrılabilir.

İkinci Add() eylem, ASP.NET MVC TryUpdateModel() yönteminin yardımıyla Entity Framework film sınıfının yeni bir örneğini oluşturur. TryUpdateModel() metodu Add() yönteme FormCollection alanlarını alır ve film sınıfı HTML form alanlarını bu değerleri atar.


Entity Framework'ü kullanırken TryUpdateModel veya UpdateModel yöntemleri bir varlık sınıfı özelliklerini güncelleştirmek için kullanırken "teknik"özelliklerin listesini belirtmeniz gerekir.


Ardından, bazı basit form doğrulaması Add() eylemi gerçekleştirir. Eylem başlık ve Direktörü özellikleri değerlere sahip olduğunu doğrular. Daha sonra bir doğrulama hatası varsa, doğrulama hatası iletisini ModelState için eklenir.

Doğrulama hataları varsa yeni bir film kayıt Entity Framework yardımıyla film veritabanı tablosuna eklenir. Yeni kayıt, aşağıdaki iki kod satırı ile veritabanına eklenir:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

Kodun ilk satırını yeni film varlık Entity Framework tarafından izleniyor filmler kümesine ekler. İkinci kod satırının, temel alınan veritabanına geri izleniyor filmler hangi değişiklikler yapılmıştır kaydeder.

**Şekil 8 – Ekle görüntüle**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Entity Framework ile veritabanı kayıtlarını güncelleştiriliyor

Yalnızca yeni bir veritabanı kaydı eklemek için izlenen bir yaklaşım Entity Framework ile bir veritabanı kaydını düzenlemek için neredeyse aynı yaklaşımı takip edebilirsiniz. 4 listeleme Edit() adlı iki yeni denetleyici eylemleri içerir. İlk Edit() eylemin film kaydı düzenlemek için bir HTML formuna döndürür. Veritabanını güncellemek ikinci Edit() eylemi çalışır.

**4 – Controllers\HomeController.vb (düzenleme metotlarını) listeleme**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

İkinci Edit() eylemi düzenlenmekte olan film kimlik eşleştiren veritabanından film kaydı alarak başlatır. Aşağıdaki LINQ to Entities deyimi belirli bir kimlik eşleştiren ilk veritabanı kaydı Dallarınızla:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Ardından, TryUpdateModel() yöntemi film entity öğesinin özellikleri için değerleri HTML form alanlarını atamak için kullanılır. Güncelleştirilecek tam özelliklerini belirtmek için bir beyaz liste sağlanır dikkat edin.

Ardından, birkaç basit doğrulaması filmi hem Direktörü özellikleri değerlere sahip olduğunu doğrulamak için gerçekleştirilir. Her iki özellik için bir değer eksik gerekirse ModelState için doğrulama hata iletisi eklenir ve ModelState.IsValid false değerini döndürür.

Doğrulama hatası varsa, son olarak, ardından temel film veritabanı tablosu ile herhangi bir değişiklik SaveChanges() yöntemi çağırarak güncelleştirilir.

Veritabanı kayıtlarını düzenleme yaparken veritabanı güncelleştirmesi gerçekleştiren denetleyici eylemini düzenlenmekte olan kaydın kimliğini geçirmeniz gerekir. Aksi takdirde, denetleyici eylemi, temel alınan veritabanında güncelleştirmek için kayıt bilmez. Listeleme 5'te yer alan düzenleme görünümü düzenlenmekte olan veritabanı kaydın kimliğini temsil eden gizli bir form alanı içerir.

**Listing 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Entity Framework ile veritabanı kayıtlarını silme

Bu öğreticide çıkmanın ihtiyacımız, son veritabanı işlemi veritabanı kayıtlarını siliniyor. Belirli bir veritabanı kaydını silmek için denetleyici eylem listeleme 6'da kullanabilirsiniz.

**6--listeleme \Controllers\HomeController.vb (silme eylemi)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Delete() eylem ilk kimlik eşleştiren varlık eyleme geçirilen film alır. Ardından, film SaveChanges() yöntemi tarafından izlenen DeleteObject() yöntemi çağırarak veritabanından silinir. Son olarak, kullanıcı dizini görünümüne yönlendirilir.

## <a name="summary"></a>Özet

Bu öğreticide, ASP.NET MVC ve Microsoft Entity Framework avantajlarından yararlanarak veritabanı temelli web uygulamaları nasıl oluşturabileceğinizi göstermek için oluştu. Seçin, ekleme, güncelleştirme olanak sağlayan bir uygulama oluşturmak ve veritabanı kayıtlarını silmek öğrendiniz.

İlk olarak, bir varlık veri modelinden Visual Studio içinde oluşturulacak varlık veri modeli Sihirbazı'nı nasıl kullanabileceğinizi ele almıştık. Ardından, LINQ to Entities bir veritabanı tablosundan veritabanı kayıt kümesini almak için nasıl kullanılacağını öğrenin. Son olarak, Entity Framework ekleme, güncelleştirme ve veritabanı kayıtlarını silmek için kullandık.

> [!div class="step-by-step"]
> [Önceki](validation-with-the-data-annotation-validators-cs.md)
> [İleri](creating-model-classes-with-linq-to-sql-vb.md)
