---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
title: 'Yineleme #1 – (C#) uygulama oluşturma | Microsoft Docs'
author: microsoft
description: 'İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve D...'
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: db0f160b-901c-46d3-865e-7ab6cd4ed68d
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-cs
msc.type: authoredcontent
ms.openlocfilehash: cecb295a54ac8bdfb63a3fee7ea0693c6998748b
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39395951"
---
<a name="iteration-1--create-the-application-c"></a>Yineleme #1 – (C#) uygulamayı oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-1-create-the-application-cs/_static/contactmanager_1_cs1.zip)

> İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetimi ASP.NET MVC uygulama (VB)

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama kişilerin bir listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 - form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 - birbirine sıkı şekilde bağlı uygulama olun. Bu dördüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 - birim testleri oluşturun. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 - test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 - Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Bu ilk yinelemeyi temel uygulamayı ekleriz. Kişi Yöneticisi hızlı ve kolay şekilde derleme olmaktır. Sonraki yinelemelerde biz tasarımını uygulama geliştirin.

Kişi Yöneticisi, temel bir veritabanı odaklı uygulama uygulamasıdır. Uygulama, yeni kişi oluşturun, var olan kişileri düzenleyip kişileri silmek için kullanabilirsiniz.

Bu yineleme, biz aşağıdaki adımları tamamlayın:

1. ASP.NET MVC uygulaması
2. Bizim kişiler depolamak için bir veritabanı oluşturun
3. Microsoft Entity Framework ile bizim veritabanı için bir model sınıfı oluşturma
4. Bir denetleyici eylemi ve tüm veritabanı kişiler listesi sağlıyor görünümü oluşturma
5. Denetleyici eylemleri ve veritabanında yeni kişi oluşturun sağlıyor bir görünüm oluşturma
6. Denetleyici eylemleri ve veritabanında var olan bir kişi düzenleme sağlıyor bir görünüm oluşturma
7. Denetleyici eylemleri ve veritabanında var olan bir kişi Sil sağlıyor bir görünüm oluşturma

## <a name="software-prerequisites"></a>Yazılım önkoşulları

ASP.NET MVC uygulamalarında Visual Studio 2008 veya Visual Web Developer 2008 (Visual Web Developer Visual Studio'nun Gelişmiş özelliklerin tümünü içermez Visual Studio'nun Ücretsiz bir sürümü) bilgisayarınızda yüklü olması gerekir. Visual Studio 2008'in deneme sürümünü veya Visual Web Developer şu adresten indirebilirsiniz:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Visual Web Developer ile ASP.NET MVC uygulamaları için Visual Web Developer Hizmet Paketi 1 yüklü olmalıdır. Service Pack 1 içermeyen, Web Uygulama projeleri oluşturulamıyor.


ASP.NET MVC çerçevesi. ASP.NET MVC çerçevesi şu adresten indirebilirsiniz:

[https://www.asp.net/mvc](../../../index.md)

Bu öğreticide, bir veritabanına erişmek için Microsoft Entity Framework kullanın. Entity Framework, .NET Framework 3.5 Service Pack 1 ile dahildir. Bu hizmet paketinin şu konumdan indirebilirsiniz:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp; displaylang = tr](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Bu indirmeleri tek tek her gerçekleştirme alternatif olarak, Web Platformu Yükleyicisi (Web PI) yararlanabilirsiniz. Web PI şu adresten indirebilirsiniz:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC projesi

ASP.NET MVC Web uygulaması projesi. Visual Studio'yu başlatın ve menü seçeneğini **dosya, yeni proje**. **Yeni proje** iletişim kutusu görünür (bkz. Şekil 1). Seçin **Web** proje türü ve **ASP.NET MVC Web uygulaması** şablonu. Yeni projenizi adlandırın *ContactManager* ve Tamam düğmesine tıklayın.


.NET Framework 3.5 üstündeki aşağı açılan listeden, seçili olduğundan emin olun sağ **yeni proje** iletişim. Aksi takdirde, t kazanılan ASP.NET MVC Web uygulaması şablonu görünür.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image1.jpg)](iteration-1-create-the-application-cs/_static/image1.png)

**Şekil 01**: Yeni Proje iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image2.png))


ASP.NET MVC uygulaması **birim testi projesi oluşturma** iletişim kutusu görüntülenir. ASP.NET MVC uygulamanızı oluştururken bir birim test projesi çözümünüze ekleyin ve oluşturmak istediğinizi belirtmek için bu iletişim kutusunu kullanabilirsiniz. Biz de kazandık t olması bu yineleme birim testleri oluşturma, seçeneğini seçmelisiniz **Evet, birim testi projesi oluşturma** çünkü bir sonraki yinelemede birim testleri eklemeyi planlıyoruz. Yeni bir ASP.NET MVC projesi oluşturduğunuzda Test projesine ekleme, ASP.NET MVC projesi oluşturulduktan sonra bir Test projesi eklemeye kıyasla daha kolaydır.

> [!NOTE] 
> 
> Visual Web Developer Test projeleri desteklemediğinden, birim testi projesi oluşturma iletişim kutusu Visual Web Developer kullanırken almıyor.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image2.jpg)](iteration-1-create-the-application-cs/_static/image3.png)

**Şekil 02**: birim testi projesi oluşturma iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image4.png))


ASP.NET MVC uygulamasını Visual Studio Çözüm Gezgini penceresinde görünür (bkz: Şekil 3). Bu pencere menü seçeneğini seçerek açabilirsiniz t don Solution Explorer penceresi görürseniz **görünümü, Çözüm Gezgini**. Çözüm iki proje içerdiğine dikkat edin: ASP.NET MVC projesi ve Test projesi. ASP.NET MVC projesi ContactManager olarak adlandırılır ve Test projesi ContactManager.Tests olarak adlandırılır.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image3.jpg)](iteration-1-create-the-application-cs/_static/image5.png)

**Şekil 03**: Çözüm Gezgini penceresinde ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Örnek proje dosyaları siliniyor

ASP.NET MVC proje şablonu denetleyicileri ve görünümleri için örnek dosyalarını içerir. Yeni bir ASP.NET MVC uygulaması oluşturmadan önce bu dosyalar silmeniz gerekir. Bir dosya veya klasörü sağ tıklatın ve menü seçeneğini belirleyerek dosyaları ve klasörleri Çözüm Gezgini penceresinde silebilir **Sil**.

ASP.NET MVC projeden aşağıdaki dosyaları silin yapmanız gerekir:

- \Controllers\HomeController.cs

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Ve Test projesinden aşağıdaki dosyası silmek gerekir:

\Controllers\HomeControllerTest.cs

## <a name="creating-the-database"></a>Veritabanı oluşturma

Kişi Yöneticisi uygulama bir veritabanı odaklı web uygulamasıdır. Kişi bilgilerini depolamak için bir veritabanı kullanın.

Microsoft SQL Server, Oracle, MySQL ve IBM DB2 veritabanları dahil olmak üzere herhangi bir modern veritabanı ile ASP.NET MVC çerçevesi. Bu öğreticide, bir Microsoft SQL Server veritabanını kullanıyoruz. Visual Studio yüklediğinizde, Microsoft SQL Server Microsoft SQL Server veritabanının ücretsiz bir sürümü Express yükleme seçeneği ile sağlanır.

Uygulamayı sağ tıklayarak yeni bir veritabanı oluşturmak\_veri klasörü Çözüm Gezgini penceresinde ve menü seçeneğini belirleyerek **Ekle, yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **veri** kategorisi ve **SQL Server veritabanı** şablonu (bkz: Şekil 4). Yeni veritabanı ContactManagerDB.mdf adlandırın ve Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image4.jpg)](iteration-1-create-the-application-cs/_static/image7.png)

**Şekil 04**: yeni bir Microsoft SQL Server Express veritabanı oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image8.png))


Yeni veritabanı oluşturduktan sonra veritabanı uygulamada görünür\_Çözüm Gezgini penceresinde veri klasörü. Sunucu Gezgini penceresini açın ve veritabanına bağlanmak için ContactManager.mdf dosyasına çift tıklayın.

> [!NOTE] 
> 
> Sunucu Gezgini penceresini veritabanı Gezgini penceresi Microsoft Visual Web Developer söz konusu olduğunda çağrılır.


Veritabanı tabloları, görünümleri, tetikleyiciler ve saklı yordamlar gibi yeni veritabanı nesneleri oluşturmak için Sunucu Gezgini penceresini kullanabilirsiniz. Tabloları klasörü sağ tıklatın ve menü seçeneğini **Yeni Tablo Ekle**. Veritabanı Tablo Tasarımcısı (bkz: Şekil 5) görünür.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image5.jpg)](iteration-1-create-the-application-cs/_static/image9.png)

**Şekil 05**: veritabanı Tablo Tasarımcısı'nda ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image10.png))


Biz şu sütunları içeren bir tablo oluşturmanız gerekir:

<a id="0.1_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | false |
| FirstName | nvarchar(50) | false |
| Soyadı | nvarchar(50) | false |
| Telefon | nvarchar(50) | false |
| E-posta | nvarchar(255) | false |


İlk sütun, kimlik sütunu özeldir. Kimlik sütunu bir kimlik sütunu ve birincil anahtar sütunu işaretlemek gerekir. Bir sütunun kimlik sütunu sütun özellikler (Şekil 6'ın altındaki arama) ve kimlik belirtimi özelliği aşağı kaydırma gerektiğini belirtmiş olursunuz. Ayarlama **(kimlik olan)** özellik değerine **Evet**.

Sütun seçerek ve bir anahtar simgesi ile düğmeye tıklandığında bir sütun birincil anahtar sütunu olarak işaretleyin. Bir sütun birincil anahtar sütunu olarak işaretlendikten sonra yanındaki sütuna bir anahtar simgesi görünür (bkz. Şekil 6).

Tablo oluşturma işlemini tamamladıktan sonra yeni bir tablo kaydetmek için Kaydet düğmesine (düğme bir disket simgesi) tıklayın. Yeni tablonuzun adını verin *kişiler*.

Kişiler veritabanı tablosu oluşturma bitiş sonra tabloya bazı kayıtları eklemeniz gerekir. Sunucu Gezgini penceresinde kişi tabloya sağ tıklayıp menü seçeneğini **tablo verilerini Göster**. Bir veya daha fazla kişi görünen kılavuz girin.

## <a name="creating-the-data-model"></a>Veri modeli oluşturma

ASP.NET MVC uygulamasını modelleri, görünümleri ve denetleyicileri içerir. Biz, önceki bölümde oluşturduğumuz kişiler tablosunu temsil eden bir Model sınıfı oluşturarak başlayın.

Bu öğreticide, bir model sınıfı veritabanından otomatik olarak oluşturmak için Microsoft Entity Framework kullanın.

> [!NOTE] 
> 
> ASP.NET MVC çerçevesi, herhangi bir şekilde Microsoft Entity Framework bağlı değildir. ASP.NET MVC, NHibernate, LINQ to SQL ve ADO.NET dahil olmak üzere alternatif veritabanı erişim teknolojileri ile kullanabilirsiniz.


Veri modeli sınıfları oluşturmak için aşağıdaki adımları izleyin:

1. Çözüm Gezgini penceresinde modelleri klasörüne sağ tıklayıp **Ekle, yeni öğe**. **Yeni Öğe Ekle** iletişim kutusu görünür (bkz. Şekil 6).
2. Seçin **veri** kategorisi ve **ADO.NET varlık veri modeli** şablonu. Veri modelinizi ad *ContactManagerModel.edmx* tıklatıp **Ekle** düğmesi. (Bkz. Şekil 7) varlık veri modeli Sihirbazı görüntülenir.
3. İçinde **Choose Model Contents** adım, select **veritabanından Oluştur** (bkz. Şekil 7).
4. İçinde **veri bağlantınızı seçin** adım, ContactManagerDB.mdf veritabanını seçin ve adını *ContactManagerDBEntities* varlık bağlantı ayarlarını (bkz. Şekil 8) için.
5. İçinde **veritabanı nesnelerinizi seçin** adım, tabloları (bkz. Şekil 9) etiketli onay kutusunu işaretleyin. Veri modeli (yalnızca bir tane olduğunu, kişiler tablosunu), veritabanındaki tüm tabloları içerir. Ad alanı girin *modelleri*. Sihirbazı tamamlamak için Son düğmesini tıklatın.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image6.jpg)](iteration-1-create-the-application-cs/_static/image11.png)

**Şekil 06**: Yeni Öğe Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image12.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image7.jpg)](iteration-1-create-the-application-cs/_static/image13.png)

**Şekil 07**: Choose Model Contents ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image14.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image8.jpg)](iteration-1-create-the-application-cs/_static/image15.png)

**Şekil 08**: veri bağlantınızı seçin ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image16.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image9.jpg)](iteration-1-create-the-application-cs/_static/image17.png)

**Şekil 09**: veritabanı nesnelerinizi seçin ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image18.png))


Varlık veri modeli Sihirbazı tamamladıktan sonra varlık veri modeli Tasarımcısı görüntülenir. Tasarımcı modellenmiş her tabloya karşılık gelen bir sınıf görüntüler. Kişi adlı bir sınıf görmeniz gerekir.

Varlık veri modeli Sihirbazı'nı veritabanı tablo adlarına göre sınıf adları oluşturur. Neredeyse her zaman, sihirbaz tarafından oluşturulan sınıfın adını değiştirmeniz gerekir. Menü seçeneği kişiler Sınıf Tasarımcısı'nda sağ tıklayıp **Yeniden Adlandır**. Sınıfın adı (çoğul) kişilerden (tekil) kişiye değiştirin. Sınıfı, sınıf adını değiştirdikten sonra Şekil 10 gibi görünmelidir.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image10.jpg)](iteration-1-create-the-application-cs/_static/image19.png)

**Şekil 10**: kişi sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image20.png))


Bu noktada, veritabanı modelimizi oluşturduk. Belirli bir ilgili kişi kaydı veritabanımızda yer temsil etmek için ilgili kişi sınıf kullanabiliriz.

## <a name="creating-the-home-controller"></a>Giriş denetleyicisi oluşturma

Sonraki adım, giriş denetleyicimizin oluşturmaktır. Giriş denetleyicisine, bir ASP.NET MVC uygulamasındaki çağrılır varsayılan denetleyicisidir.

Çözüm Gezgini penceresinde denetleyicileri klasörüne sağ tıklayarak ve menü seçeneğini belirleyerek giriş denetleyici sınıfını oluşturmak **Ekle, denetleyici** (bkz. Şekil 11). Etiketli onay kutusunu fark **oluşturma, güncelleştirme ve ayrıntıları senaryoları için eylem yöntemleri ekleme**. Tıklamadan önce bu onay kutusunun işaretli olduğundan emin olun **Ekle** düğmesi.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image11.jpg)](iteration-1-create-the-application-cs/_static/image21.png)

**Şekil 11**: Giriş denetleyicisine ekleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image22.png))


Giriş denetleyicisine oluşturduğunuzda, sınıf listesi 1'de alın.

**1 - Controllers\HomeController.cs listeleme**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample1.cs)]

## <a name="listing-the-contacts"></a>Kişiler listesi

Kişiler veritabanı tablosu, kayıtları görüntülemek için biz İNDİS() eylem ve bir dizin görünüm oluşturmanız gerekir.

Giriş denetleyicisine İNDİS() eylem zaten içeriyor. 2 listeleme gibi görünüyor. böylece, bu yöntem değiştirmek ihtiyacımız var.

**2 - Controllers\HomeController.cs listeleme**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample2.cs)]

Adlı bir özel alan 2 listeleme giriş controller sınıfında içerdiğine dikkat edin \_varlıklar. \_Varlık alanı veri modelindeki varlıkları temsil eder. Kullandığımız \_varlıkları alan veritabanıyla iletişim kurmak için.

Tüm kişileri kişileri veritabanı tablosundan temsil eden bir görünüm İNDİS() yöntemi döndürür. İfade \_varlıklar. ContactSet.ToList() Kişiler listesi genel bir liste olarak döndürür.

Artık görüyoruz ve oluşturulan dizin denetleyicisi biz ardından dizini görünüm oluşturmanız gerekir. Dizin görünümünün oluşturmadan önce menü seçeneğini seçerek uygulamanızı derleyin **yapı, yapı çözümü**. Görüntülenecek sırada model sınıfları listesi için bir görünüm eklemeden önce projenizin her zaman yeniden derlemelisiniz **Görünüm Ekle** iletişim.

Dizin görünümünün İNDİS() yöntemi sağ tıklayarak ve menü seçeneğini belirleyerek oluşturma **Görünüm Ekle** (bkz. Şekil 12). Bu menü seçeneğini belirleyerek açılır **Görünüm Ekle** iletişim (bkz. Şekil 13).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image12.jpg)](iteration-1-create-the-application-cs/_static/image23.png)

**Şekil 12**: dizini görünümü ekleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image24.png))


İçinde **Görünüm Ekle** iletişim kutusunda etiketli onay **kesin türü belirtilmiş görünüm oluşturmak**. Görünüm veri sınıfı ContactManager.Models.Contact ve içerik listesini görüntüle'ı seçin. Şu seçenekleri belirleyerek, kişi kayıtlarını içeren bir liste olarak görüntüleyen bir görünüm oluşturur.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image13.jpg)](iteration-1-create-the-application-cs/_static/image25.png)

**Şekil 13**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image26.png))


Tıkladığınızda **Ekle** düğmesi, listeleme 3 dizin görünümünde oluşturulur. Bildirim &lt;% @ sayfa %&gt; dosyanın üst kısmında görünür yönergesi. Dizin görünümünün ViewPage devralan&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; sınıfı. Diğer bir deyişle, kişi varlıkları listesi görünümünde Model sınıfı temsil eder.

Model sınıfı tarafından temsil edilen kişileri yinelenen bir foreach döngü gövdesi dizin görünümünün içerir. Kişi sınıfın her bir özellik değeri, bir HTML tablosu içinde görüntülenir.

**3 - Views\Home\Index.aspx (üzerinde değişiklik yapılmadan) listeleme**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample3.aspx)]

Dizin görünümünün bir değişiklik yapmanız gerekir. Ayrıntılar görünümü oluşturmadığınızı çünkü ayrıntıları bağlantı kaldırabiliriz. Bulun ve aşağıdaki kodu dizin görünümden kaldır:

{ id=item.Id })%&gt;

Dizin görünümünün değiştirdikten sonra Kişi Yöneticisi uygulamayı çalıştırabilirsiniz. Menü seçeneği hata ayıklama, hata ayıklamayı Başlat'ı seçin veya F5 tuşuna basmanız yeterlidir. Uygulamayı çalıştırmak ilk kez Şekil 14'te iletişim kutusu. Seçeneğini **hata ayıklamayı etkinleştirmek için Web.config dosyasını değiştirme** ve Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image14.jpg)](iteration-1-create-the-application-cs/_static/image27.png)

**Şekil 14**: hata ayıklamayı etkinleştirme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image28.png))


Dizin görünümünün varsayılan olarak döndürülür. Bu görünüm tüm kişileri veritabanı tablosundan veri listelenir (bkz. Şekil 15).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image15.jpg)](iteration-1-create-the-application-cs/_static/image29.png)

**Şekil 15**: dizin görünümünün ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image30.png))


Dizin görünümünün yeni görünüm alt kısmındaki Oluştur etiketli bağlantı içerdiğine dikkat edin. Sonraki bölümde, yeni kişiler oluşturma konusunda bilgi edinin.

## <a name="creating-new-contacts"></a>Yeni kişi oluşturma

Kullanıcılarının yeni kişileri oluşturmasına olanak sağlamak için şu iki Create() eylemi giriş denetleyicisine eklemeniz gerekir. Yeni bir kişi oluşturmak için bir HTML formuna döndüren bir Create() eylem için oluşturmamız gerekir. Yeni iletişim asıl veritabanını ekleme gerçekleştirir ikinci bir Create() eylemi için oluşturmamız gerekir.

Giriş denetleyicisine eklemek için gereken yeni Create() yöntemleri listeleme 4'te yer alır.

**4 - Controllers\HomeController.cs (oluşturma yöntemleriyle) listeleme**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample4.cs)]

İkinci Create() yöntemini yalnızca HTTP POST tarafından çağrılabilir olsa ilk Create() yöntemi bir HTTP GET ile çağrılabilir. Diğer bir deyişle, ikinci Create() yöntemi yalnızca bir HTML formu aktarırken çağrılabilir. İlk Create() yöntemi yalnızca yeni kişi oluşturmaya HTML formu içeren bir görünüm verir. İkinci Create() yöntem daha da ilginçtir: veritabanına yeni kişi ekler.

İkinci Create() yöntem kişi sınıfının bir örneğini kabul etmek için değiştirilmiş dikkat edin. HTML formundan gönderilen form değerleri bu kişi sınıfına ASP.NET MVC çerçevesi tarafından otomatik olarak bağlanır. Her form oluştur HTML form alanından bir kişi parametre özelliğine atanır.

Kişi parametresi [bağlama] özniteliği ile donatılmış, dikkat edin. [Bağlama] özniteliği, ilgili kişi kodu özellik bağlamasını dışlamak için kullanılır. ID özelliği bir kimlik özelliği temsil ettiğinden, şu t ID özelliği ayarlamak istiyorsanız ki.

Create() yöntemin gövdesinde, Entity Framework, yeni kişi veritabanına eklemek için kullanılır. Yeni kişi kişilerin mevcut kümesine eklenir ve bu değişiklikler temel alınan veritabanına geri göndermek için SaveChanges() yöntemi çağrılır.

Yeni kişileri iki Create() yöntemlerden birini sağ tıklayarak ve menü seçeneğini belirleyerek oluşturmak için bir HTML formuna oluşturabileceğiniz **Görünüm Ekle** (bkz. Şekil 16).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image16.jpg)](iteration-1-create-the-application-cs/_static/image31.png)

**Şekil 16**: oluşturma görünümü ekleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image32.png))


İçinde **Görünüm Ekle** iletişim kutusunda **ContactManager.Models.Contact** sınıfı ve **Oluştur** içeriği görüntüle seçeneğini (bkz. Şekil 17). Tıkladığınızda **Ekle** görünümü otomatik olarak oluşturulan bir oluşturma düğmesi.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image17.jpg)](iteration-1-create-the-application-cs/_static/image33.png)

**Şekil 17**: explode bir sayfayı görmeden ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image34.png))


Oluştur görünümünün her bir ilgili kişi sınıf özelliklerini için form alanlarını içerir. Oluştur görünümünün kodunu listeleme 5'te eklenmiştir.

**Listing 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample5.aspx)]

Create() yöntemleri değiştirme ve oluşturma görünümü ekleme sonra kişinin yöneticisi uygulamayı çalıştırın ve yeni kişiler oluşturma. Tıklayın **Yeni Oluştur** oluşturma görünümüne gitmek için dizin görünümünde görüntülenen bağlantı. Şekil 18 görünümünde görmeniz gerekir.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image18.jpg)](iteration-1-create-the-application-cs/_static/image35.png)

**Şekil 18**: Görünüm Oluştur ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image36.png))


## <a name="editing-contacts"></a>Kişiler düzenleme

Bir ilgili kişi kaydı düzenlemek için işlevsellik ekleme yeni kişi kayıtları oluşturmak için ekleme işlevine benzer. İlk olarak, şu iki yeni düzenleme metotlarını giriş denetleyici sınıfına eklemeniz gerekir. Bu yeni Edit() yöntemleri listeleme 6'da yer alır.

**6 - Controllers\HomeController.cs (düzenleme metotlarını ile) listeleme**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample6.cs)]

İlk Edit() yöntemi bir HTTP GET işlemi tarafından çağrılır. Bir kimliği parametre düzenlenmekte olan ilgili kişi kaydı kimliği temsil eden bu yönteme iletilir. Entity Framework kimlik eşleştiren bir kişi almak için kullanılır Bir kaydı düzenlemek için bir HTML formu içeren bir görünümü döndürülür.

İkinci Edit() yöntemi veritabanına gerçek güncelleştirme gerçekleştirir. Bu yöntem kişi sınıfının bir örneği bir parametre olarak kabul eder. ASP.NET MVC çerçevesi form alanlarını düzenleme formu bu sınıf için otomatik olarak bağlar. T ki dikkat edin (kimliği özelliğinin değeri ihtiyacımız) bir kişiyi düzenlerken [bağlama] özniteliği içerir.

Entity Framework, değiştirilmiş ilgili veritabanına kaydetmek için kullanılır. Özgün kişinin ilk veritabanından alınmalıdır. Ardından, ilgili değişiklikleri kaydetmek için Entity Framework ApplyPropertyChanges() yöntemi çağrılır. Son olarak, temel alınan veritabanına değişiklikleri kalıcı hale getirmek için Entity Framework SaveChanges() yöntemi çağrılır.

Edit() yöntemi sağ tıklayın ve Ekle görüntüle menü seçeneğini belirleyerek düzenleme formu içeren görünümü oluşturabilirsiniz. Görünüm Ekle iletişim kutusunda, seçmek **ContactManager.Models.Contact** sınıfı ve **Düzenle** içeriği görüntüle (bkz. Şekil 19).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image19.jpg)](iteration-1-create-the-application-cs/_static/image37.png)

**Şekil 19**: bir düzenleme görünümü ekleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image38.png))


Ekle düğmesine tıkladığınızda yeni bir düzenleme görünümü otomatik olarak oluşturulur. Oluşturulan HTML formu her kişi sınıfı (7 listeleme bakın) özelliklerinin karşılık gelen alanları içerir.

**Listing 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Kişileri silme

Ardından kişiler silmek istiyorsanız giriş denetleyici sınıfına iki Delete() eylem eklemeniz gerekir. İlk Delete() eylemi silme onayı form görüntüler. İkinci Delete() eylemi gerçek silme gerçekleştirir.

> [!NOTE] 
> 
> Daha sonra Ajax silme tek bir adımda destekler, böylece yineleme # 7'de, biz Kişi Yöneticisi değiştirin.


İki yeni Delete() yöntemleri listeleme 8'de yer alır.

**8 - Controllers\HomeController.cs (Delete metotlarını) listeleme**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample8.cs)]

Bir ilgili kişi kaydı veritabanından silmek için bir onay formu ilk Delete() yöntemi döndürür (Figure20 bakın). İkinci Delete() yöntem veritabanında gerçek silme işlemini gerçekleştirir. Özgün kişinin veritabanından alındıktan sonra veritabanı silme gerçekleştirme Entity Framework DeleteObject() ve SaveChanges() yöntemleri çağrılır.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image20.jpg)](iteration-1-create-the-application-cs/_static/image39.png)

**Şekil 20**: silme onayı görüntüle ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image40.png))


(Bkz. Şekil 21) ilgili kişi kaydı silmek için bir bağlantı içeren dizini görünümünü değiştirmek ihtiyacımız var. Aşağıdaki kodu düzenleme bağlantısını içeren aynı tablo hücresi eklemeniz gerekir:

Html.ActionLink ({kimliği = öğe. % Kimliği})&gt;


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image21.jpg)](iteration-1-create-the-application-cs/_static/image41.png)

**Şekil 21**: dizin düzenleme bağlantısını görünümüyle ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image42.png))


Ardından, biz silme onayı görünüm oluşturmanız gerekir. Giriş denetleyicisine sınıfındaki Delete() yöntemi sağ tıklayın ve Ekle görüntüle menü seçeneğini belirleyin. Görünüm Ekle iletişim kutusu (bkz. Şekil 22) görünür.

Farklı olarak liste, oluşturma ve düzenleme görünümler söz konusu olduğunda, Görünüm Ekle iletişim kutusu silme görünümü oluşturmak için bir seçenek içermiyor. Bunun yerine, seçin **ContactManager.Models.Contact** veri sınıfı ve **boş** içeriği görüntüle. Bize kendimize görünümü oluşturmak içerik seçeneği gerektirir boş Görünüm seçme.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image22.jpg)](iteration-1-create-the-application-cs/_static/image43.png)

**Şekil 22**: silme onayı görünüm ekleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image44.png))


Delete görünümün içeriğini listeleme 9'da yer alır. Bu görünüm onaylayan bir formu içeren belirli bir kişi veya depolamamayı olmalıdır (bkz: Şekil 21) silindi.

**Listing 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-cs/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Varsayılan denetleyici adının değiştirilmesi

Kişiler ile çalışmak için sunduğumuz denetleyici sınıfı adı HomeController sınıf olarak adlandırılır sizi rahatsız. Paylaşılmamalıdır t denetleyici ContactController adlandırılması?

Bu sorunu düzeltmek kolay bir işlemdir. İlk olarak, giriş denetleyicisine adını yeniden ihtiyacımız var. HomeController sınıf Visual Studio Kod Düzenleyicisi'nde açın, sınıfın adını sağ tıklayın ve menü seçeneğini **yeniden düzenleme, yeniden adlandırma**. Bu menü seçeneğini belirleyerek yeniden adlandır iletişim kutusunu açar.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image23.jpg)](iteration-1-create-the-application-cs/_static/image45.png)

**Şekil 23**: Denetleyici adı yeniden düzenleme ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image46.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image24.jpg)](iteration-1-create-the-application-cs/_static/image47.png)

**Şekil 24**: yeniden adlandır iletişim kutusunu kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image48.png))


Denetleyici sınıfınıza yeniden adlandırırsanız, Visual Studio klasöründe görünümleri de adını güncelleştirir. Visual Studio \Views\Contact klasöre \Views\Home klasörü yeniden adlandırır.

Bu değişikliği yaptıktan sonra uygulamanız artık giriş denetleyicisine sahip. Uygulamanızı çalıştırdığınızda, Şekil 25'hata sayfası alırsınız.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-cs/_static/image25.jpg)](iteration-1-create-the-application-cs/_static/image49.png)

**Şekil 25**: varsayılan denetleyici yok ([tam boyutlu görüntüyü görmek için tıklatın](iteration-1-create-the-application-cs/_static/image50.png))


Giriş denetleyicisine yerine kişi denetleyicisi kullanmak için Global.asax dosyasında varsayılan yönlendirmesini güncelleştirmek ihtiyacımız var. Global.asax dosyası açın ve varsayılan rota (10 listeleme bakın) tarafından kullanılan varsayılan denetleyicisi değiştirin.

**10 - Global.asax.cs listeleme**

[!code-csharp[Main](iteration-1-create-the-application-cs/samples/sample10.cs)]

Kişi Yöneticisi, bu değişiklikleri yaptıktan sonra düzgün çalışır. Şimdi, ilgili kişi denetleyicisi sınıf varsayılan denetleyicisi olarak kullanır.

## <a name="summary"></a>Özet

Bu ilk yinelemeyi, temel bir kişi Yöneticisi uygulaması en hızlı şekilde olası oluşturduk. Başlangıç kodunu görünümleri ve denetleyicileri otomatik olarak oluşturmak için Visual Studio'nun avantajı attık. Ayrıca bizim veritabanı modeli sınıfları otomatik olarak oluşturmak için Entity Framework avantajlarından attık.

Şu anda, biz listesinde, oluşturabilir, düzenleme ve Kişi Yöneticisi uygulamayla ilgili kişi kaydı silin. Diğer bir deyişle, biz bütün bir veritabanı odaklı web uygulaması tarafından gerekli temel veritabanı işlemleri gerçekleştirebilirsiniz.

Ne yazık ki, uygulamamız bazı sorunlar vardır. Bu havalı. ilk ve istemeyebilir, kişi Yöneticisi uygulama en cazip uygulaması değil. Bu, bazı tasarım çalışması gerekir. Bir sonraki yinelemede nasıl varsayılan görünüm ana sayfasını ve uygulama görünüşünü iyileştirmek için geçişli stil sayfası Değiştirebiliriz adresindeki inceleyeceğiz.

İkinci olarak, herhangi bir form doğrulaması uyguladık değil. Örneğin, bir şey yok herhangi bir form alanı değerlerini girmeden Oluştur iletişim formu göndermelerini engellemek için. Ayrıca, geçersiz telefon numaraları ve e-posta adreslerini girin. Yineleme #3 form doğrulama sorunu gidermek Başlat.

Son ve en önemlisi, kişi Yöneticisi uygulamasının geçerli yineleme kolayca tutulan veya değiştirilemez. Örneğin, veritabanı erişim mantığı sağ denetleyici eylemlerine hazır. Başka bir deyişle, biz bizim denetleyicileri değiştirmeden bizim veri erişim kodu değiştiremezsiniz. Sonraki yinelemelerde Kişi Yöneticisi değiştirilecek daha dayanıklı hale getirmek için uygulayabileceğiniz yazılım tasarım desenleri inceleyeceğiz.

> [!div class="step-by-step"]
> [Next](iteration-2-make-the-application-look-nice-cs.md)
