---
uid: mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
title: 'Yineleme #1 – (VB) uygulaması oluşturma | Microsoft Docs'
author: microsoft
description: 'İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve D....'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 5b033582-1646-42c2-b20d-7edc8814e970
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-1-create-the-application-vb
msc.type: authoredcontent
ms.openlocfilehash: 52029816bd9f37c3d5c3321d3c5e60599314a33b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877066"
---
<a name="iteration-1--create-the-application-vb"></a>Yineleme #1 – (VB) uygulama oluşturma
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-1-create-the-application-vb/_static/contactmanager_1_vb1.zip)

> İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve silme (CRUD).


## <a name="building-a-contact-management-aspnet-mvc-application-vb"></a>Bir kişi yönetim ASP.NET MVC uygulaması (VB) oluşturma

Bu öğretici serisinde tamamlamak için tüm kişi yönetim uygulamanın başından oluşturun. Kişi Yöneticisi uygulama kişi listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamak sağlar.

Biz uygulamayı birden çok kez oluşturun. Her bir yineleme, biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı, her değişiklik nedeni anlamak etkinleştirmek için hedefidir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - iyi Ara uygulama olun. Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.

- Yineleme #3 - form doğrulama ekleyin. Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de e-posta adresleri ve telefon numaralarını doğrulayın.

- Yineleme #4 - gevşek uygulama olun. Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, uygulamamız havuz deseni ve bağımlılık ekleme düzeni kullanmak üzere yeniden.

- Yineleme #5 - birim testleri oluşturma. Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve bizim denetleyicileri ve Doğrulama mantığı için birim testleri oluşturma.

- Yineleme #6 - teste dayalı geliştirme kullanın. Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede kişi grupları ekleyin.

- Yineleme #7 - Ajax işlevselliği ekleyin. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Bu ilk yinelemede temel uygulama oluşturun. Hedef, hızlı ve kolay şekilde olası Contact Manager oluşturmaktır. Sonraki yinelemelerde uygulama tasarımını geliştirin.

Bir temel veritabanı temelli uygulama Contact Manager uygulamasıdır. Uygulama, yeni kişiler oluşturma, var olan kişileri düzenleme ve kişileri silmek için kullanabilirsiniz.

Bu yinelemede aşağıdaki adımları tamamlayın:

1. ASP.NET MVC uygulaması
2. Bizim kişiler depolamak için bir veritabanı oluşturun
3. Veritabanımıza Microsoft Entity Framework için bir model sınıfı oluşturma
4. Bir denetleyici eylemi ve bize tüm kişileri veritabanındaki listelemek etkinleştirir görünümü oluşturma
5. Denetleyici eylemleri ve bize veritabanında yeni bir kişi oluşturmak sağlayan bir görünüm oluşturma
6. Denetleyici eylemleri ve bize veritabanında varolan bir kişinin düzenlemek sağlayan bir görünüm oluşturma
7. Denetleyici eylemleri ve bize veritabanında varolan bir kişiyi silmek sağlayan bir görünüm oluşturma

## <a name="software-prerequisites"></a>Yazılım önkoşulları

ASP.NET MVC uygulamalarında Visual Studio 2008 veya Visual Web Developer 2008 (Visual Web Developer bir ücretsiz Visual Studio Gelişmiş özelliklerin tümünü içermiyorsa Visual Studio sürümüdür) bilgisayarınızda yüklü olması gerekir. Visual Studio 2008'in deneme sürümünü veya Visual Web Developer aşağıdaki adresinden indirebilirsiniz:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

> [!NOTE] 
> 
> Visual Web Developer ile ASP.NET MVC uygulamaları için Visual Web Developer Hizmet Paketi 1'in yüklü olması gerekir. Service Pack 1 içermeyen Web Uygulama projeleri oluşturulamıyor.


ASP.NET MVC çerçevesi. ASP.NET MVC çerçevesi aşağıdaki adresinden indirebilirsiniz:

[https://www.asp.net/mvc](../../../index.md)

Bu öğreticide, bir veritabanına erişmek için Microsoft Entity Framework kullanın. Entity Framework .NET Framework 3.5 Service Pack 1 ile dahil edilir. Bu hizmet paketi aşağıdaki konumdan yükleyebilirsiniz:

[https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?familyid=ab99342f-5d1a-413d-8319-81da479ab0d7&amp;displaylang=en)

Bu indirmeleri tek tek her gerçekleştirme alternatif olarak, Web Platformu Yükleyicisi (Web PI) avantajından yararlanabilirsiniz. Web PI aşağıdaki adresinden indirebilirsiniz:

[https://www.asp.net/downloads/essential/](https://www.asp.net/downloads/essential)

## <a name="aspnet-mvc-project"></a>ASP.NET MVC Project

ASP.NET MVC Web uygulaması projesi. Visual Studio'yu başlatın ve menü seçeneğini **dosya, yeni proje**. **Yeni proje** iletişim kutusu görüntülenir (bkz: Şekil 1). Seçin **Web** proje türü ve **ASP.NET MVC Web uygulaması** şablonu. Yeni projeniz ad *ContactManager* ve Tamam düğmesine tıklayın.


.NET Framework 3.5 üst aşağı açılan listeden seçili olduğundan emin olun sağında **yeni proje** iletişim. Aksi takdirde, t won ASP.NET MVC Web uygulaması şablonu görünür.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image1.jpg)](iteration-1-create-the-application-vb/_static/image1.png)

**Şekil 01**: Yeni Proje iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image2.png))


ASP.NET MVC uygulaması **birim testi projesi oluşturma** iletişim kutusu görüntülenir. Oluşturun ve ASP.NET MVC uygulamanızın oluşturduğunuzda, çözümünüz için birim testi projesi eklemek isteyip istemediğinizi belirtmek için bu iletişim kutusunu kullanabilirsiniz. Biz won t rağmen olması bu yineleme birim testleri oluşturma, seçeneği seçmelisiniz **Evet, bir birim testi projesi oluşturma** sonraki yinelemede birim testleri ekleme planlıyoruz olduğundan. Yeni bir ASP.NET MVC proje ilk oluşturduğunuzda bir Test projesi ekleme, ASP.NET MVC Proje oluşturulduktan sonra bir Test projesi eklemekten daha kolaydır.

> [!NOTE] 
> 
> Visual Web Developer Test projeleri desteklemediği için birim testi projesi oluştur iletişim kutusu Visual Web Developer kullanırken elde değil.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image2.jpg)](iteration-1-create-the-application-vb/_static/image3.png)

**Şekil 02**: birim testi projesi oluştur iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image4.png))


ASP.NET MVC uygulaması Visual Studio Çözüm Gezgini penceresinde görüntülenir (bkz: Şekil 3). Bu pencere menü seçeneğini seçerek açabilirsiniz sonra t tan Solution Explorer penceresi görürseniz **görünüm, Çözüm Gezgini**. Çözüm iki proje içerdiğine dikkat edin: ASP.NET MVC proje ve Test projesi. ASP.NET MVC projesinin ContactManager adlandırılır ve Test projesinin ContactManager.Tests olarak adlandırılır.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image3.jpg)](iteration-1-create-the-application-vb/_static/image5.png)

**Şekil 03**: Solution Explorer penceresi ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image6.png))


## <a name="deleting-the-project-sample-files"></a>Proje örnek dosyaları silme

ASP.NET MVC proje şablonu örnek dosyalarını denetleyicileri ve görünümler içerir. Yeni bir ASP.NET MVC uygulaması oluşturmadan önce bu dosyaları silmeniz gerekir. Bir dosya veya klasöre sağ tıklayıp menü seçeneğini seçerek Solution Explorer penceresi bulunan dosya ve klasörleri silebilirsiniz **silmek**.

ASP.NET MVC projeden aşağıdaki dosyaları silin gerekir:

- \Controllers\HomeController.vb

- \Views\Home\About.aspx

- \Views\Home\Index.aspx

Ve aşağıdaki dosya Test projesinden silmeniz gerekir:

\Controllers\HomeControllerTest.vb

## <a name="creating-the-database"></a>Veritabanı oluşturuluyor

İlgili Kişi Yöneticisi uygulama bir veritabanı kaynaklı web uygulamasıdır. Kişi bilgilerini depolamak için bir veritabanı kullanırız.

Microsoft SQL Server, Oracle, MySQL ve IBM DB2 veritabanları dahil olmak üzere herhangi bir modern veritabanı ile ASP.NET MVC çerçevesi. Bu öğreticide, bir Microsoft SQL Server veritabanını kullanın. Visual Studio yüklediğinizde, Microsoft SQL Server Microsoft SQL Server veritabanının ücretsiz sürüm olan Express yükleme seçeneği ile sağlanır.

Uygulamayı sağ tıklayarak yeni bir veritabanı oluşturmak\_Solution Explorer penceresi ve menü seçeneğini belirleyerek verileri klasöründe **Ekle, yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **veri** kategori ve **SQL Server veritabanı** şablonu (Şekil 4'e bakın). Yeni bir veritabanı ContactManagerDB.mdf adlandırın ve Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image4.jpg)](iteration-1-create-the-application-vb/_static/image7.png)

**Şekil 04**: yeni bir Microsoft SQL Server Express veritabanı oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image8.png))


Yeni veritabanı oluşturduktan sonra veritabanı uygulamada görünür\_Solution Explorer penceresi veri klasörü. Sunucu Gezgini penceresini açın ve veritabanına bağlanmak için ContactManager.mdf dosyasını çift tıklatın.

> [!NOTE] 
> 
> Sunucu Gezgini penceresini Microsoft Visual Web Developer olması durumunda veritabanı Gezgini penceresi adı verilir.


Veritabanı tabloları, görünümleri, tetikleyiciler ve saklı yordamlar gibi yeni veritabanı nesneleri oluşturmak için Sunucu Gezgini penceresini kullanabilirsiniz. Tables klasörünü sağ tıklatın ve menü seçeneğini seçmek **Yeni Tablo Ekle**. (Bkz. Şekil 5) veritabanı Tablo Tasarımcısı görüntülenir.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image5.jpg)](iteration-1-create-the-application-vb/_static/image9.png)

**Şekil 05**: veritabanı Tablo Tasarımcısı'nda ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image10.png))


Aşağıdaki sütunları içeren bir tablo oluşturmak ihtiyacımız var:

<a id="0.2_table01"></a>


| **Sütun adı** | **Veri türü** | **Null değerlere izin ver** |
| --- | --- | --- |
| Kimliği | int | false |
| FirstName | nvarchar(50) | false |
| Soyadı | nvarchar(50) | false |
| Telefon | nvarchar(50) | false |
| E-posta | nvarchar(255) | false |


İlk sütun kimliği sütunu özeldir. ID sütunu bir kimlik sütunu ve bir birincil anahtar sütun işaretlemek gerekir. Bir sütunu sütun özellikler (Şekil 6'ın altındaki arama) genişletme ve kimlik belirtimi özelliğini aşağı kaydırma tarafından bir kimlik sütunu olduğunu gösterir. Ayarlama **(olan kimlik)** özelliğinin değeri **Evet**.

Sütun seçerek ve bir anahtar simgesi ile düğmesini tıklatarak bir birincil anahtar sütunu olarak bir sütun işaretleyin. Bir sütun birincil anahtar sütunu olarak işaretlendikten sonra yanındaki sütuna anahtarının bir simge görüntülenir (Şekil 6'ya bakın).

Tablo oluşturma işlemini tamamladıktan sonra yeni bir tablo kaydetmek için Kaydet düğmesine (bir disket simgesi ile düğmesi) tıklatın. Yeni tablo adını verin *kişiler*.

Kişiler veritabanı tablosu oluşturma bitiş sonra tabloya bazı kayıtları eklemeniz gerekir. Sunucu Gezgini penceresinde Kişiler tablosuna sağ tıklayın ve menü seçeneğini **Show Table Data**. Bir veya daha fazla kişi görünür kılavuzunda girin.

## <a name="creating-the-data-model"></a>Veri modeli oluşturma

ASP.NET MVC uygulamasına modeller, görünümleri ve denetleyicilerini oluşur. Önceki bölümde oluşturduğumuz kişiler tablosunu temsil eden bir Model sınıfı oluşturmaya başlayın.

Bu öğreticide, bir model sınıfı veritabanından otomatik olarak oluşturmak üzere Microsoft Entity Framework kullanın.

> [!NOTE] 
> 
> ASP.NET MVC çerçevesi Microsoft Entity Framework herhangi bir şekilde bağlı değildir. ASP.NET MVC NHibernate, LINQ to SQL ya da ADO.NET alternatif veritabanı erişim teknolojilerini kullanabilirsiniz.


Veri modeli sınıfları oluşturmak için aşağıdaki adımları izleyin:

1. Çözüm Gezgini penceresinde modeller klasörü sağ tıklatın ve seçin **Ekle, yeni öğe**. **Yeni Öğe Ekle** iletişim kutusu görüntülenir (Şekil 6'ya bakın).
2. Seçin **veri** kategori ve **ADO.NET varlık veri modeli** şablonu. Veri modelinizi ad *ContactManagerModel.edmx* tıklatıp **Ekle** düğmesi. Varlık veri modeli Sihirbazı'nı (bkz. Şekil 7) görünür.
3. İçinde **Model içeriği seçin** adım, select **veritabanından Oluştur** (bkz. Şekil 7).
4. İçinde **veri bağlantınızı** adım, ContactManagerDB.mdf veritabanını seçin ve bir ad girin *ContactManagerDBEntities* varlık bağlantı ayarları (bkz. Şekil 8) için.
5. İçinde **veritabanı nesnelerinizi** adım, (bkz. Şekil 9) tabloları etiketli onay kutusunu seçin. Veri modeli (yalnızca bir olduğundan, kişiler tablo), veritabanında bulunan tüm tabloları içerir. Ad alanı girin *modelleri*. Sihirbazı tamamlamak için Son düğmesini tıklatın.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image6.jpg)](iteration-1-create-the-application-vb/_static/image11.png)

**Şekil 06**: Yeni Öğe Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image12.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image7.jpg)](iteration-1-create-the-application-vb/_static/image13.png)

**Şekil 07**: Model içeriği seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image14.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image8.jpg)](iteration-1-create-the-application-vb/_static/image15.png)

**Şekil 08**: veri bağlantınızı seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image16.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image9.jpg)](iteration-1-create-the-application-vb/_static/image17.png)

**Şekil 09**: veritabanı nesnelerinizi ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image18.png))


Entity Data Model Designer, varlık veri modeli Sihirbazı tamamladıktan sonra görünür. Tasarımcı Modellenen her tablo için karşılık gelen bir sınıf görüntüler. Kişiler adlı bir sınıf görmeniz gerekir.

Varlık veri modeli Sihirbazı'nı sınıf adları veritabanı tablo adlarını temel alarak oluşturur. Neredeyse her zaman sihirbaz tarafından oluşturulan sınıf adını değiştirmeniz gerekir. Kişiler Sınıf Tasarımcısı'nda sağ tıklayın ve menü seçeneğini **yeniden adlandırma**. Sınıfın adı (çoğul) kişilerden (tekil) kişiye değiştirin. Sınıfı, sınıf adını değiştirdikten sonra Şekil 10 gibi görünmesi gerekir.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image10.jpg)](iteration-1-create-the-application-vb/_static/image19.png)

**Şekil 10**: kişi sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image20.png))


Bu noktada, veritabanı modelimizi oluşturduk. Belirli bir kişi kaydı bizim veritabanında temsil etmek için size kişi sınıfını kullanabilirsiniz.

## <a name="creating-the-home-controller"></a>Ev denetleyicisi oluşturma

Sonraki adım, bizim giriş denetleyicisi oluşturmaktır. Bir ASP.NET MVC uygulamasındaki çağrılan varsayılan denetleyicisi giriş denetleyicisidir.

Çözüm Gezgini penceresinde denetleyicileri klasörüne sağ tıklayıp menü seçeneğini seçerek giriş denetleyici sınıfını oluşturmak **Ekle, denetleyici** (bkz. Şekil 11). Etiketli onay kutusunu fark **oluşturma, güncelleştirme ve ayrıntıları senaryoları için eylem yöntemleri eklemek**. ' Yi tıklatmadan önce bu onay kutusunun işaretli olduğundan emin olun **Ekle** düğmesi.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image11.jpg)](iteration-1-create-the-application-vb/_static/image21.png)

**Şekil 11**: Giriş denetleyicisi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image22.png))


Giriş denetleyicisini oluşturduğunuzda, listeleme 1'de sınıf alın.

**Listing 1 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample1.vb)]

## <a name="listing-the-contacts"></a>Kişileri listeleme

Kişiler veritabanı tablosunda kayıtları görüntülemek için biz İNDİS() eylem ve bir dizin görünüm oluşturmanız gerekir.

Giriş denetleyicisi İNDİS() eylem zaten var. Biz, böylece listeleme 2 gibi görünüyor. Bu yöntem değiştirmeniz gerekir.

**Listing 2 - Controllers\HomeController.vb**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample2.vb)]

2. listeleme giriş denetleyici sınıfının adlı özel bir alan içerdiğine dikkat edin \_varlıklar. \_Varlıklar alanını temsil eden varlık veri modeli. Kullanırız \_varlıklar alan veritabanıyla iletişim kurmak için.

İNDİS() yöntemi tüm kişileri kişiler veritabanı tablosundan gösteren bir görünüm verir. İfade \_varlıklar. ContactSet.ToList() kişiler genel bir listesini döndürür.

Şimdi görüyoruz ve oluşturulan dizini denetleyicisi oluşturmamız sonraki dizin görünümünün gerekir. Dizin görünümünün oluşturmadan önce menü seçeneğini seçerek uygulamanızı derleyin **yapı, yapı çözümü**. Projenizi sırada görüntülenmesini model sınıflarına bir görünüm eklemeden önce her zaman derleme **Görünüm Ekle** iletişim.

Dizin görünümünün İNDİS() yöntemi sağ tıklayıp menü seçeneğini seçerek oluşturduğunuz **Görünüm Ekle** (bkz. Şekil 12). Bu menü seçeneğini seçerek açılır **Görünüm Ekle** iletişim (bkz. Şekil 13).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image12.jpg)](iteration-1-create-the-application-vb/_static/image23.png)

**Şekil 12**: dizini görünümü ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image24.png))


İçinde **Görünüm Ekle** iletişim kutusunda, etiketli onay kutusunu işaretleyin **kesin türü belirtilmiş görünüm oluşturmak**. Görünüm veri sınıfı ContactManager.Contact ve Görünüm İçerik listesini seçin. Bu seçenekleri kişi kayıtların listesini görüntüleyen bir görünüm oluşturur.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image13.jpg)](iteration-1-create-the-application-vb/_static/image25.png)

**Şekil 13**: Görünüm Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image26.png))


Tıkladığınızda **Ekle** düğmesi, listeleme 3'te dizin görünümünün oluşturulur. Bildirim &lt;% @ sayfa %&gt; dosyanın üst kısmında görünür yönergesi. Dizin görünümünün ViewPage devralan&lt;IEnumerable&lt;ContactManager.Models.Contact&gt; &gt; sınıfı. Diğer bir deyişle, Model sınıfı görünümünde kişi varlıkları listesini temsil eder.

Dizin görünümünün gövdesini Model sınıfı tarafından temsil edilen kişileri dolaşır foreach döngüsü içerir. Kişi sınıfın her bir özellik değeri bir HTML tablosu içinde görüntülenir.

**Listing 3 - Views\Home\Index.aspx (unmodified)**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample3.aspx)]

Dizin görünümünün bir değişiklik yapmanız gerekir. Ayrıntılar görünümü oluşturmadığınızı olduğundan, biz ayrıntıları bağlantıyı kaldırabilirsiniz. Bulun ve aşağıdaki kodu dizin görünümden kaldır:

{.id = item.Id})%&gt;

Dizin görünümünün değiştirdikten sonra kişinin yöneticisi uygulamayı çalıştırabilirsiniz. Menü seçeneğinin hata ayıklama, hata ayıklamayı Başlat seçin veya F5 tuşuna basmanız yeterlidir. Uygulamayı çalıştırın ilk kez Şekil 14'te iletişim alın. Seçeneğini **hata ayıklamayı etkinleştirmek üzere Web.config dosyasında değişiklik** ve Tamam düğmesine tıklayın.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image14.jpg)](iteration-1-create-the-application-vb/_static/image27.png)

**Şekil 14**: hata ayıklamasını etkinleştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image28.png))


Dizin görünümünün varsayılan olarak döndürülür. Bu görünüm tüm kişiler veritabanı tablosundan verileri listeler (bkz. Şekil 15).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image15.jpg)](iteration-1-create-the-application-vb/_static/image29.png)

**Şekil 15**: dizin görünümünün ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image30.png))


Dizin görünümünün görünümün altındaki Yeni Oluştur etiketli bağlantı içerdiğine dikkat edin. Sonraki bölümde, yeni kişiler oluşturma öğrenin.

## <a name="creating-new-contacts"></a>Yeni kişiler oluşturma

Yeni kişiler oluşturmak için kullanıcıları etkinleştirmek üzere şu iki Create() Eylemler giriş denetleyiciye eklemeniz gerekir. Yeni bir kişi oluşturmak için bir HTML formuna döndüren bir Create() eylem oluşturmak gerekir. Biz yeni kişinin gerçek veritabanı Ekle gerçekleştirir ikinci bir Create() eylem oluşturmanız gerekir.

Giriş denetleyicisine eklemek için ihtiyacımız yeni Create() yöntemler listeleme 4'te bulunur.

**4 - Controllers\HomeController.vb (yöntemleriyle Oluştur) listeleme**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample4.vb)]

İkinci Create() yöntemi yalnızca bir HTTP POST tarafından çağrılabilir sırada bir HTTP GET ile ilk Create() yöntemi çağrılabilir. Diğer bir deyişle, ikinci Create() yöntemi yalnızca bir HTML formuna nakil sırasında çağrılabilir. İlk Create() yöntemi yalnızca yeni bir kişi oluşturmak için HTML formu içeren bir görünüm verir. İkinci Create() çok daha ilginç bir yöntemdir: yeni kişi veritabanına ekler.

İkinci Create() yöntemi kişiyi sınıfının bir örneği kabul edecek şekilde değiştirilmiş dikkat edin. HTML formundan gönderilen form değerleri bu kişi sınıfına ASP.NET MVC çerçevesi tarafından otomatik olarak bağlanır. Her form oluştur HTML form alanından kişi parametre özelliğine atanır.

Kişi parametresi [bağ] özniteliğiyle tasarlandığına dikkat edin. [Bind] özniteliği, ilgili kişi kimliği özelliği bağlamanın dışında tutmak için kullanılır. ID özelliği temsil eden bir kimlik özelliği olduğundan, biz t ID özelliği ayarlamak istiyorsanız güncelleştireceğinizi.

Create() yöntemin gövdesine Entity Framework yeni kişi veritabanına eklemek için kullanılır. Yeni kişi kişiler varolan kümesine eklenir ve bu değişiklikleri alttaki veritabanına geri göndermek için SaveChanges() yöntemi çağrılır.

İki Create() yöntemlerinden birini sağ tıklayıp menü seçeneğini seçerek yeni kişiler oluşturma için bir HTML formuna oluşturabileceğiniz **Görünüm Ekle** (bkz. Şekil 16).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image16.jpg)](iteration-1-create-the-application-vb/_static/image31.png)

**Şekil 16**: oluşturma görünümü ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image32.png))


İçinde **Görünüm Ekle** iletişim kutusunda **ContactManager.Contact** sınıfı ve **oluşturma** içeriği görüntüle seçeneğini (Şekil 17 bakın). Tıkladığınızda **Ekle** button, Görünüm otomatik olarak oluşturulan bir oluşturun.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image17.jpg)](iteration-1-create-the-application-vb/_static/image33.png)

**Şekil 17**: açılımı bir sayfayı görmenizin ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image34.png))


Oluştur görünümünün form alanlarını her ilgili kişi sınıf özelliklerini içerir. Oluştur görünümünün kodunu listeleme 5'te bulunur.

**Listing 5 - Views\Home\Create.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample5.aspx)]

Create() yöntemleri değiştirme ve oluşturma görünümü ekleme sonra kişinin yöneticisi uygulamayı çalıştırın ve yeni kişiler oluşturma. Tıklatın **Yeni Oluştur** oluşturma görünümüne gitmek için dizin görünümünde görüntülenen bağlantı. Şekil 18 görünümünde görmeniz gerekir.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image18.jpg)](iteration-1-create-the-application-vb/_static/image35.png)

**Şekil 18**: Görünüm Oluştur ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image36.png))


## <a name="editing-contacts"></a>Kişiler düzenleme

İlgili kişi kaydını düzenlemek için İşlevler ekleme, yeni kişi kayıtları oluşturmak için işlevsellik ekleme için çok benzer. İlk olarak, biz giriş denetleyici sınıfına iki yeni düzenleme yöntemi eklemeniz gerekir. Bu yeni Edit() yöntemleri listeleme 6'yer alır.

**6 - Controllers\HomeController.vb (yöntemleriyle Düzenle) listeleme**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample6.vb)]

İlk Edit() yöntemi, bir HTTP GET işlemi tarafından çağrılır. ID parametresi düzenlenmekte kişi kayıt kimliğini temsil eden bu yönteme geçirilir. Entity Framework kimliği ile eşleşen bir kişi almak için kullanılır Bir kaydı düzenlemek için bir HTML formu içeren bir görünümü döndürülür.

İkinci Edit() yöntemi veritabanına gerçek güncelleştirme gerçekleştirir. Bu yöntem kişiyi sınıfının bir örneği parametre olarak kabul eder. ASP.NET MVC çerçevesi form alanları düzenleme formundan bu sınıfa otomatik olarak bağlar. T güncelleştireceğinizi bildirimi (kimliği özelliğinin değeri ihtiyacımız) bir kişi düzenlerken [Bind] özniteliği içerir.

Entity Framework değiştirilmiş kişi veritabanına kaydetmek için kullanılır. Özgün kişinin veritabanından ilk alınmalıdır. Ardından, ilgili değişiklikleri kaydetmek için Entity Framework ApplyPropertyChanges() yöntemi çağrılır. Son olarak, temel alınan veritabanı değişiklikleri kalıcı hale getirmek için Entity Framework SaveChanges() yöntemi çağrılır.

Edit() yöntemi sağ tıklayıp Ekle görüntüle menü seçeneğini seçerek düzenleme formu içeren görünüm oluşturabilir. Görünüm Ekle iletişim kutusunda seçin **ContactManager.Models.Contact** sınıfı ve **Düzenle** görüntülemek içeriği (bkz. Şekil 19).


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image19.jpg)](iteration-1-create-the-application-vb/_static/image37.png)

**Şekil 19**: düzenleme görünümü ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image38.png))


Yeni bir düzenleme Görünümü Ekle düğmesine tıkladığınızda otomatik olarak oluşturulur. Oluşturulan HTML formu her kişi sınıfı (7 listeleme bakın) özelliklerini karşılık gelen alan içeriyor.

**Listing 7 - Views\Home\Edit.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample7.aspx)]

## <a name="deleting-contacts"></a>Kişileri silme

Kişiler silmek istiyorsanız giriş denetleyici sınıfına iki Delete() eylem eklemeniz gerekir. İlk Delete() eylem silme onayı formu görüntüler. İkinci Delete() eylemi gerçek silme gerçekleştirir.

> [!NOTE] 
> 
> Daha sonra böylece destekliyorsa Ajax silmek tek bir adımda yineleme # 7'de, biz Kişi Yöneticisi'ni değiştirin.


İki yeni Delete() yöntemleri listeleme 8'de yer alır.

**8 - Controllers\HomeController.vb (Delete yöntemleri) listeleme**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample8.vb)]

İlgili kişi kaydını veritabanından silmek için onay form ilk Delete() yöntemi döndürür (Figure20 bakın). İkinci Delete() yöntemi veritabanında gerçek silme işlemi gerçekleştirir. Özgün kişinin veritabanından aldıktan sonra veritabanı silme gerçekleştirmek için Entity Framework DeleteObject() ve SaveChanges() yöntemleri denir.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image20.jpg)](iteration-1-create-the-application-vb/_static/image39.png)

**Şekil 20**: silme onayı Görünüm ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image40.png))


(Bkz. Şekil 21) ilgili kişi kayıtları silmek için bir bağlantı içeren dizin görünümü değiştirmek gerekir. Aşağıdaki kod düzenleme bağlantısını içeren aynı tablo hücresi eklemeniz gerekir:

{.id = item.Id})%&gt;


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image21.jpg)](iteration-1-create-the-application-vb/_static/image41.png)

**Şekil 21**: dizin düzenleme bağlantısını görünümüyle ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image42.png))


Ardından, biz silme onayı görünüm oluşturmanız gerekir. Giriş denetleyici sınıfı Delete() yönteminde sağ tıklayın ve Ekle görüntüle menü seçeneğini belirleyin. Görünüm Ekle iletişim kutusu (bkz. Şekil 22) görüntülenir.

Aksine listesi, oluşturma ve düzenleme görünümler söz konusu olduğunda, Görünüm Ekle iletişim kutusu bir Delete görünümü oluşturmak için bir seçenek içermiyor. Bunun yerine, seçin **ContactManager.Models.Contact** veri sınıfı ve **boş** içeriği görüntüleyin. İçerik seçeneği bize görünümü kendisini oluşturmak gerektirecektir boş görünüm seçebilirsiniz.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image22.jpg)](iteration-1-create-the-application-vb/_static/image43.png)

**Şekil 22**: silme onayı görünümü ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image44.png))


Delete Görünümü içeriğini listeleme 9'yer alır. Bu görünüm onaylar bir formu içeren karşılamadığını belirli bir kişi olması gerekir (bkz. Şekil 21) silindi.

**Listing 9 - Views\Home\Delete.aspx**

[!code-aspx[Main](iteration-1-create-the-application-vb/samples/sample9.aspx)]

## <a name="changing-the-name-of-the-default-controller"></a>Varsayılan denetleyicinin adını değiştirme

Sizi kişiler ile çalışmak için bizim denetleyici sınıfı adı HomeController sınıf olarak adlandırılır rahatsız. Shouldn t denetleyicisi ContactController adlandırılması?

Bu sorunu gidermek kolay. İlk olarak, biz giriş denetleyicinin adı yeniden düzenlemeniz gerekir. HomeController sınıfı Visual Studio Kod düzenleyicisinde açın, sınıfın adını sağ tıklatın ve menü seçeneğini **yeniden adlandırma**. Bu menü seçeneğini seçerek yeniden adlandır iletişim kutusunu açar.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image23.jpg)](iteration-1-create-the-application-vb/_static/image45.png)

**Şekil 23**: bir denetleyici adı yeniden düzenleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image46.png))


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image24.jpg)](iteration-1-create-the-application-vb/_static/image47.png)

**Şekil 24**: yeniden adlandır iletişim kutusu kullanılarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image48.png))


Denetleyici sınıfı yeniden adlandırırsanız, Visual Studio klasöründe görünümleri de adını güncelleştirin. Visual Studio \Views\Home klasörü \Views\Contact klasörü yeniden adlandırır.

Bu değişikliği yaptıktan sonra uygulamanızı bir giriş denetleyicisi artık yok. Uygulamanızı çalıştırdığınızda, Şekil 25 hata sayfası elde edersiniz.


[![Yeni Proje iletişim kutusu](iteration-1-create-the-application-vb/_static/image25.jpg)](iteration-1-create-the-application-vb/_static/image49.png)

**Şekil 25**: varsayılan denetleyicisi yok ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-1-create-the-application-vb/_static/image50.png))


Biz yerine Giriş denetleyicisi kişi denetleyicisi kullanmak için Global.asax dosyasında varsayılan rota güncelleştirmeniz gerekir. Global.asax dosyasını açın ve varsayılan rota (10 listeleme bakın) tarafından kullanılan varsayılan denetleyicisi değiştirin.

**10 - Global.asax.vb listeleme**

[!code-vb[Main](iteration-1-create-the-application-vb/samples/sample10.vb)]

Bu değişiklikleri yaptıktan sonra kişinin Yöneticisi'ni düzgün çalışır. Şimdi, onu varsayılan denetleyicisi olarak kişi denetleyici sınıfını kullanır.

## <a name="summary"></a>Özet

Bu ilk yinelemede temel bir kişi Yöneticisi uygulama en hızlı yolu olası oluşturduk. Biz ilk kodunu bizim denetleyicileri ve görünümler için otomatik olarak oluşturmak için Visual Studio avantajlarından sürdü. Biz de avantajı bizim veritabanı modeli sınıflarını otomatik olarak oluşturmak için Entity Framework'ün sürdü.

Şu anda, biz listesinde, oluşturmak, düzenlemek ve Kişi Yöneticisi uygulamayla ilgili kişi kayıtlarını sil. Diğer bir deyişle, biz tüm veritabanı kaynaklı web uygulaması tarafından gerekli temel veritabanı işlemlerini gerçekleştirebilirsiniz.

Ne yazık ki, uygulamamız bazı sorunlar vardır. İlk ve bu haklıymış istemeyebilir, kişinin Yöneticisi uygulaması en cazip uygulama değil. Bazı tasarım çalışması gerekir. Sonraki yinelemede nasıl biz varsayılan görünüm ana sayfası ve uygulama görünümünü iyileştirmek için geçişli stil sayfası değiştirebilirsiniz adresindeki göreceğiz.

İkinci olarak, biz tüm form doğrulama uygulanan değil. Örneğin, bir şey yok herhangi bir form alanları için değerleri girmeden oluşturma kişi formu göndermelerini engellemek için. Ayrıca, geçersiz telefon numaraları ve e-posta adreslerini girebilirsiniz. Yineleme #3 form doğrulama sorunu üstesinden gelmek başlatın.

Son ve en önemlisi, kişinin Yöneticisi uygulamasının geçerli yinelemeye kolayca tutulan veya değiştirilemez. Örneğin, veritabanı erişim mantığı sağ denetleyicisi eylemlere baked. Başka bir deyişle, biz bizim denetleyicileri değiştirmeden bizim veri erişim kodu değiştiremezsiniz. Sonraki yinelemelerde Contact Manager değiştirmek için daha esnek olmak için uygulayabileceğiniz yazılım tasarım desenleri keşfedin.

> [!div class="step-by-step"]
> [Önceki](iteration-7-add-ajax-functionality-cs.md)
> [sonraki](iteration-2-make-the-application-look-nice-vb.md)
