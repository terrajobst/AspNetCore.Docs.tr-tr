---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: "Entity Framework (VB) ile modeli sınıfları oluşturma | Microsoft Docs"
author: microsoft
description: "Bu öğreticide, ASP.NET MVC Microsoft Entity Framework ile kullanmayı öğrenin. Bir ADO.NET varlık Da oluşturmak için varlık Sihirbazı'nı kullanmayı öğrenin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: efc190d856fe9ebf1c09e0ae4758aabb1e3254dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Entity Framework (VB) ile modeli sınıfları oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu öğreticide, ASP.NET MVC Microsoft Entity Framework ile kullanmayı öğrenin. Bir ADO.NET varlık veri modeli oluşturmak için varlık Sihirbazı'nı kullanmayı öğrenin. Bu öğretici süresince seçin, Ekle, Güncelleştir ve Entity Framework kullanarak veritabanı verilerini silmek üzere verilmektedir bir web uygulaması oluşturun.


Bu öğretici, bir ASP.NET MVC uygulaması oluştururken Microsoft Entity Framework kullanarak veri erişim sınıfları nasıl oluşturabileceğinizi açıklamak için hedefidir. Bu öğreticide Microsoft Entity Framework'ün önceki bilgi varsayar. Bu öğreticide sonuna tarafından nasıl Entity Framework kullanma seçin, ekleme, güncelleştirme ve veritabanı kayıtlarını sil anlamanız.

Microsoft Entity Framework Veri erişim katmanı veritabanından otomatik olarak oluşturmanıza olanak sağlayan bir nesne ilişkisel eşleme (O/RM) bir araçtır. Entity Framework veri erişimi sınıfları el ile oluşturmanın can sıkıcı iş önlemek etkinleştirir.

> [!NOTE] 
> 
> ASP.NET MVC Microsoft Entity Framework arasındaki temel bağlantı yoktur. ASP.NET MVC ile kullanabileceğiniz Entity Framework birkaç alternatifleri vardır. Örneğin, Microsoft LINQ SQL, NHibernate veya SubSonic gibi diğer O/RM araçları kullanarak, MVC Model sınıflarınızı oluşturabilirsiniz.


Microsoft Entity Framework ASP.NET MVC ile nasıl kullanabileceğiniz göstermek için biz basit örnek bir uygulama oluşturmak. Görüntüleme ve film veritabanı kayıtlarını düzenleme olanak tanıyan film veritabanı uygulaması oluşturacağız.

Bu öğreticide, Visual Studio 2008 veya Visual Web Developer 2008 Service Pack 1 sahip olduğunuzu varsayar. Entity Framework kullanmak için Service Pack 1 gerekir. Visual Studio 2008 Service Pack 1 veya Service Pack 1 ile Visual Web Developer aşağıdaki adresinden indirebilirsiniz:

> [https://www.ASP.NET/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Film örnek veritabanı oluşturma

Film veritabanı uygulaması şu sütunları içerir filmler adlı bir veritabanı tablosu kullanır:

| Sütun adı | Veri Türü | Null değerlere izin verilsin mi? | Birincil anahtarı mı? |
| --- | --- | --- | --- |
| Kimliği | int | False | Doğru |
| Başlık | Nvarchar(100) | False | False |
| Director | Nvarchar(100) | False | False |

Bu tablo, aşağıdaki adımları izleyerek, bir ASP.NET MVC projesinde ekleyebilirsiniz:

1. Uygulamayı sağ\_menü seçeneğini seçin ve Çözüm Gezgini penceresinin veri klasörü **Ekle, yeni öğe.**
2. Gelen **Yeni Öğe Ekle** iletişim kutusunda **SQL Server veritabanı**, veritabanı MoviesDB.mdf adını verin ve tıklayın **Ekle** düğmesi.
3. Sunucu Gezgini/veritabanı Gezgini penceresi açmak için MoviesDB.mdf dosyasını çift tıklatın.
4. MoviesDB.mdf veritabanı bağlantısı'nı genişletin, tablolar klasörünü sağ tıklatın ve menü seçeneğini belirleyin **Yeni Tablo Ekle**.
5. Tablo Tasarımcısı'nda kimliği, başlık ve Director sütunları ekleyin.
6. Tıklatın **kaydetmek** (disket simgesine sahip) düğmesini yeni bir tablo adı filmler ile kaydetmek için.

Film veritabanı tablosu oluşturduktan sonra bazı örnek veriler tabloya eklemeniz gerekir. Film tabloyu sağ tıklatın ve menü seçeneğini **Show Table Data**. Görüntülenen kılavuza sahte film verileri girebilirsiniz.

## <a name="creating-the-adonet-entity-data-model"></a>ADO.NET varlık veri modeli oluşturma

Entity Framework kullanmak için bir varlık veri modeli oluşturmanız gerekir. Visual Studio yararlanabilir *varlık veri modeli Sihirbazı* bir varlık veri modeli veritabanından otomatik olarak oluşturmak için.

Aşağıdaki adımları uygulayın:

1. Çözüm Gezgini penceresinde modeller klasörü sağ tıklatın ve menü seçeneğini **Ekle, yeni öğe**.
2. İçinde **Yeni Öğe Ekle** iletişim kutusunda, veri kategorisi seçin (bkz: Şekil 1).
3. Seçin **ADO.NET varlık veri modeli** şablonu, varlık veri modeli MoviesDBModel.edmx adını verin ve tıklatın **Ekle** düğmesi. Tıklatarak **Ekle** düğmesi veri modeli Sihirbazı'nı başlatır.
4. İçinde **Model içeriği seçin** seçin, adım **bir veritabanından Oluştur** seçeneğini ve tıklayın **sonraki** (bkz: Şekil 2) düğmesine tıklayın.
5. İçinde **veri bağlantınızı** adım, MoviesDB.mdf veritabanı bağlantısı seçin, bağlantı ayarlarını MoviesDBEntities adlandırın ve tıklayın varlıkları girin **sonraki** (bkz: Şekil 3) düğmesine tıklayın.
6. İçinde **veritabanı nesnelerinizi** adım, film veritabanı tabloyu seçin ve **son** (bkz: Şekil 4) düğmesine tıklayın.

Bu adımları tamamladıktan sonra ADO.NET varlık veri modeli Tasarımcısı'nı (Entity Designer) açar.

**Şekil 1 – yeni bir varlık veri modeli oluşturma**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Şekil 2 – modeli içeriği adım seçin**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Şekil 3 – veri bağlantınızı seçin**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Şekil 4 – veritabanı nesnelerini seçin**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>ADO.NET varlık veri modeli değiştirme

Bir varlık veri modeli oluşturduktan sonra modelin Entity Designer yararlanarak değiştirebilirsiniz (bkz. Şekil 5). Çözüm Gezgini penceresi içinde modelleri klasöründe bulunan MoviesDBModel.edmx dosyasına çift tıklayarak dosyayı herhangi bir zamanda Entity Designer açabilirsiniz.

**ADO.NET varlık veri modeli Tasarımcısı 5 – Şekil**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Örneğin, varlık Model veri Sihirbazı oluşturur sınıflarının adlarını değiştirmek için Entity Designer kullanın. Sihirbaz filmler adlı yeni bir veri erişim sınıfı oluşturuldu. Diğer bir deyişle, sihirbazın sınıfı veritabanı tablosunun çok aynı adı verdi. Bu sınıf belirli bir filmi örneği temsil eden kullanacağız çünkü biz filmler sınıfından film için yeniden adlandırmanız gerekir.

Bir varlık sınıfı yeniden adlandırmak isterseniz, sınıf adını Entity Designer'da çift tıklayın ve yeni bir ad girin (bkz. Şekil 6). Alternatif olarak, bir varlık Entity Designer'da seçtikten sonra Özellikler penceresinde bir varlığın adı değiştirebilirsiniz.

**Şekil 6 – bir varlık adı değiştirme**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Varlık veri modelinizi (disket simgesi) Kaydet düğmesine tıklayarak değişiklik yaptıktan sonra kaydetmeyi unutmayın. Arka planda Entity Designer Visual Basic .NET sınıfları kümesi oluşturur. Çözüm Gezgini penceresinden MoviesDBModel.Designer.vb dosyasını açarak bu sınıfların görüntüleyebilirsiniz.


Değişikliklerinizi Entity Designer bir sonraki kullanışınızda üzerine yazılacak beri Designer.vb olarak adlandırılır dosyasındaki kodu değiştirmeyin. Designer.vb olarak adlandırılır dosyasında tanımlanan sınıflar işlevselliğini genişletmek istediğiniz sonra oluşturabileceğiniz *kısmi sınıflar* dosyaları'te ayırın.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Entity Framework veritabanı kayıtları seçme

Film veritabanı uygulamamızı oluşturmaya film kayıtların listesini görüntüleyen bir sayfa oluşturarak başlayalım. İNDİS() adlı bir eylem listeleme 1 giriş denetleyicisi sunar. İNDİS() eylem film kayıtların tümünü Entity Framework yararlanarak film veritabanı tablosundan döndürür.

**1 – Controllers\HomeController.vb listeleme**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Denetleyici listeleme 1'deki bir oluşturucu içerdiğine dikkat edin. Adlı bir sınıf düzeyi alan oluşturucu başlatır \_db. \_Db alan Microsoft Entity Framework tarafından oluşturulan veritabanı varlıkları temsil eder. \_Db alandır varlık tasarımcısı tarafından oluşturulan MoviesDBEntities sınıfının bir örneği.

\_Db alan içinde İNDİS() eylemi filmler veritabanı tablosundan kayıtları almak için kullanılır. İfade \_db. MovieSet tüm kayıtlar filmler veritabanı tablosundan temsil eder. ToList() yöntemi film nesneler genel bir koleksiyona filmler kümesi dönüştürmek için kullanılır: listesi, (film).

Film kayıtları, LINQ to Entities yardımıyla alınır. LINQ listeleme 1 İNDİS() eylemini kullanan *yöntem sözdizimi* veritabanı kayıt kümesi alınamadı. İsterseniz, LINQ kullanabilirsiniz *sorgu sözdizimi* yerine. Aşağıdaki iki ifade çok aynı şeyi yapar:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

En kullanışlı bulabileceğiniz hangi LINQ sözdizimini – yöntem sözdizimi veya sorgu söz dizimi – kullanın. Performans iki yaklaşım arasında fark yoktur – stili yalnızca farktır.

Listeleme 2 görünümünde film kayıtları görüntülemek için kullanılır.

**2 – Views\Home\Index.aspx listeleme**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

Listeleme 2 görünümünde içeren bir **her** her film kaydı tekrarlanan ve film kaydın başlık ve Director özelliklerinin değerlerini görüntüleyen döngü. Bir düzenleme ve silme bağlantı yanındaki her kayıt görüntülenir dikkat edin. Ayrıca, bir ekleme film bağlantı görünümün en altında görüntülenen (bkz. Şekil 7).

**Şekil 7 – dizini görünümü**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

Dizin görünümdür bir *yazılan Görünüm*. Dizin görünümünün sahip bir &lt;% @ sayfa %&gt; Inherits özniteliği içeren yönergesi. Inherits özniteliği, türü kesin belirlenmiş genel listesi film nesneler koleksiyonunu – bir listesi, (film) ViewData.Model özelliğine çevirir.

## <a name="inserting-database-records-with-the-entity-framework"></a>Entity Framework veritabanı kayıtlarını ekleme

Entity Framework kolaylaştıran yeni kayıtlar bir veritabanı tablosuna eklemek için kullanabilirsiniz. Liste 3'teki yeni kayıtlar film veritabanı tablosuna eklemek için kullanabileceğiniz giriş denetleyici sınıfı eklenen iki yeni eylemleri içerir.

**3 – Controllers\HomeController.vb (Ekle yöntemleri) listeleme**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

İlk Add() eylem yalnızca bir görünüm verir. Yeni bir filmi veritabanı eklemek için bir form görünümü içerir (bkz. Şekil 8) kaydedin. Formu gönderdiğinde, ikinci Add() eylemi çağrılır.

İkinci Add() eylemi AcceptVerbs özniteliği ile tasarlandığına dikkat edin. Bu eylem, yalnızca bir HTTP POST işlemi gerçekleştirirken çağrılabilir. Diğer bir deyişle, bu eylem yalnızca bir HTML formuna nakil sırasında çağrılabilir.

İkinci Add() eylem ASP.NET MVC TryUpdateModel() yöntemi yardımıyla Entity Framework film sınıfının yeni bir örneğini oluşturur. TryUpdateModel() yöntemi Add() yönteme geçirilen FormCollection alanları alır ve bu HTML form alanların değerlerini film sınıfına atar.


Entity Framework kullanırken, bir varlık sınıfı özelliklerini güncelleştirmek için TryUpdateModel veya UpdateModel yöntemi kullanırken, "Beyaz"özelliklerin listesini sağlamanız gerekir.


Ardından, bazı basit form doğrulama Add() eylemi gerçekleştirir. Eylem başlık ve Director özellikleri değerlere sahip olduğunu doğrular. Bir doğrulama hatası varsa, bir doğrulama hata iletisi ModelState için eklenir.

Doğrulama hataları varsa yeni bir filmi kayıt Entity Framework yardımıyla filmler veritabanı tablosuna eklenir. Yeni kayıttaki aşağıdaki iki kod satırı ile veritabanına eklenir:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

Kod ilk satırını yeni film varlık Entity Framework tarafından izleniyor filmler kümesine ekler. İkinci satır kod değişiklikleri temel veritabanına izleniyor filmler yapılmıştır kaydeder.

**Şekil 8 – Ekle görünümü**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Entity Framework ile veritabanı kayıtlarını güncelleştirme

Yalnızca yeni bir veritabanı kaydı eklemek için izlenen bir yaklaşım Entity Framework veritabanı kaydını düzenlemek için neredeyse aynı yaklaşımı izleyebilirsiniz. 4 listeleme Edit() adlı iki yeni denetleyici eylemleri içerir. İlk Edit() eylem film kaydını düzenlemek için bir HTML formuna döndürür. Veritabanını güncelleştirmek ikinci Edit() eylemi çalışır.

**4 – Controllers\HomeController.vb (düzenleme yöntemleri) listeleme**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

İkinci Edit() eylemi düzenlenen film kimliğini eşleşen veritabanından film kaydı alarak başlatır. Aşağıdaki LINQ to Entities deyimi belirli bir kimlik numarası eşleşen ilk veritabanı kaydını alan:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Ardından, TryUpdateModel() yöntemi film varlık özelliklerini HTML form alanların değerlerini atamak için kullanılır. Beyaz liste güncelleştirmek için tam özelliklerini belirtmek için sağlanan dikkat edin.

Ardından, filmi ve Director özellikleri değerlere sahip olduğunu doğrulamak için bazı basit bir doğrulama gerçekleştirilir. Her iki özellik değeri eksik, ModelState için bir doğrulama hata iletisi eklenir ve ModelState.IsValid false değerini döndürür.

Doğrulama hatası varsa, son olarak, daha sonra temel filmler veritabanı tablosu herhangi bir değişiklikle SaveChanges() yöntemini çağırarak güncelleştirilir.

Veritabanı kayıtlarını düzenlerken, veritabanı güncelleştirme gerçekleştirir denetleyici eylemi düzenlenmekte kayıt kimliğini geçmesi gerekir. Aksi takdirde, denetleyici eylemi, temel veritabanında güncelleştirmek için kayıt bilmez. Listeleme 5'te yer alan düzenleme görünümü düzenlenmekte veritabanı kayıt kimliğini temsil eden gizli bir form alanı içerir.

**5 – Views\Home\Edit.aspx listeleme**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Entity Framework veritabanı kayıtları silme

Bu öğreticide üstesinden gelmek için ihtiyacımız, son veritabanı işlemi veritabanı kayıtlarını siliyor. Belirli veritabanı kaydını silmek için listeleme 6'denetleyici eylemini kullanın.

**6--listeleme \Controllers\HomeController.vb (Delete eylem)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

Delete() Eylem Kimliği eşleşen varlık eyleme geçirilen film ilk alır. Ardından, film, SaveChanges() yöntemi tarafından izlenen DeleteObject() yöntemini çağırarak veritabanından silinir. Son olarak, kullanıcı dizin görünümüne yönlendirilir.

## <a name="summary"></a>Özet

ASP.NET MVC ve Microsoft Entity Framework yararlanarak veritabanı tarafından yönetilen web uygulamalarının nasıl oluşturabilirsiniz göstermek için bu öğreticinin amacı oluştu. Seç, Ekle, Güncelleştir olanak tanıyan bir uygulama oluşturmak ve veritabanı kayıtlarını silmek öğrendiniz.

İlk olarak, bir varlık veri modeli Visual Studio'dan oluşturmak için varlık veri modeli Sihirbazı'nı nasıl kullanabileceğiniz açıklanmıştır. Ardından, LINQ to Entities veritabanı kayıt kümesinin bir veritabanı tablosunun almak için nasıl kullanılacağını öğrenin. Son olarak, Entity Framework ekleme, güncelleştirme ve veritabanı kayıtlarını sil kullandık.

>[!div class="step-by-step"]
[Önceki](validation-with-the-data-annotation-validators-cs.md)
[sonraki](creating-model-classes-with-linq-to-sql-vb.md)
