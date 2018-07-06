---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Veri erişim katmanını (C#) oluşturma | Microsoft Docs
author: rick-anderson
description: Bu öğreticide size çok baştan başlamanız ve veri erişim katmanı (bir veritabanında bilgilere erişmek için türü belirtilmiş DataSets kullanarak DAL), oluşturmak.
ms.author: aspnetcontent
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 556771994fd80773ec1e86bcbab0f60f2bb821e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838760"
---
<a name="creating-a-data-access-layer-c"></a>Veri erişim katmanını (C#) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) veya [PDF olarak indirin](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> Bu öğreticide size çok baştan başlamanız ve veri erişim katmanı (bir veritabanında bilgilere erişmek için türü belirtilmiş DataSets kullanarak DAL), oluşturmak.


## <a name="introduction"></a>Giriş

Web geliştiriciler olarak, verilerle çalışma etrafında bizim hayatını çalışmalarınızı. Veritabanları ve web sayfaları, toplamak ve onu özetlemek için değiştirme ve almak için kod verileri depolamak için oluştururuz. ASP.NET 2.0 sürümünde bu ortak desenler uygulamak için teknikleri inceleyeceksiniz uzun serideki ilk öğreticide budur. Oluşturarak başlayacağız bir [yazılım mimarisi](http://en.wikipedia.org/wiki/Software_architecture) oluşan bir veri erişim katmanı (yazılan veri kümeleri, bir iş mantığı katmanı (BLL) kullanarak DAL,), özel iş kurallarını uygular ve ASP.NET ile oluşturulmuş bir sunum katmanı, sayfalar ortak bir sayfa düzeni paylaşır. Bu arka uç öğrenmeniz, raporlama, geçeceğiz düzenlenir sonra nasıl görüntüleneceğini gösteren özetlemenize, toplamak ve bir web uygulamasında veri doğrulama. Bu öğretici, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Her öğretici, C# ve Visual Basic sürümlerinde kullanılabilir ve kullanılan tüm kod indirilmesini içerir. (Bu ilk öğreticide oldukça uzun bağlıdır, ancak geri kalan fazlasını digestible öbekler halinde sunulur.)

Bu öğreticiler için yerleştirilmiş Northwind veritabanı Microsoft SQL Server 2005 Express Edition sürümü kullanacağız **uygulama\_veri** dizin. Veritabanı dosyası yanı sıra **uygulama\_veri** klasör farklı veritabanı sürümünü kullanacak şekilde istemeniz durumunda veritabanı oluşturmak için SQL komut dosyalarını da içerir. Bu betikler olması da olabilir [doğrudan Microsoft'tan indirilen](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), tercih ediyorsanız. Northwind veritabanı farklı bir SQL Server sürümünü kullanıyorsanız, güncelleştirmeniz gerekecektir **NORTHWNDConnectionString** uygulamanın ayarlama **Web.config** dosya. Bir dosya sistemi tabanlı Web sitesi projesi olarak Visual Studio 2005 Professional Edition'ı kullanarak web uygulaması oluşturuldu. Ancak, tüm öğreticileri çalışır ücretsiz sürümü Visual Studio 2005 ile eşit derecede iyi [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
Bu öğreticide size çok baştan başlamanız ve veri erişim katmanı (ikinci öğreticide iş mantığı katmanı (BLL) oluşturma ve sayfa düzeni ve üçüncü gezinme konusunda DAL), oluşturmak. Üçüncü bir kuruluş oluşturacaksınız sonra öğreticileri ilk üç içinde açıklanmıştır. Bu ilk öğreticide, böylece Visual Studio'yu yangın ve başlayalım çok yapılandırdığımıza göre!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>1. adım: bir Web projesi oluşturma ve veritabanı'na bağlanma

Bizim veri erişim katmanı (DAL) oluşturabiliriz önce öncelikle bir web sitesi oluşturabileceğinizi ve bizim Veritabanı Kurulumu ihtiyacımız var. Yeni bir dosya sistemi tabanlı ASP.NET web sitesi oluşturmaya başlayın. Bunu gerçekleştirmek için Dosya menüsüne gidin ve yeni Web sitesi iletişim kutusunda görüntüleme, yeni Web sitesi seçin. ASP.NET Web sitesi şablonu seçin, dosya sistem konumu aşağı açılan listesi olarak, web sitesine yerleştirmek için bir klasör seçin ve C# dilini ayarlama.


[![Yeni bir dosya sistemi tabanlı Web sitesi oluşturma](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Şekil 1**: New File System-Based Web sitesi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image3.png))


Bu yeni bir web sitesi ile oluşturacak bir **Default.aspx** ASP.NET sayfası ve bir **uygulama\_veri** klasör.

Oluşturulan web sitesi ile Visual Studio sunucu Gezgini'ndeki veritabanına bir başvuru eklemek sonraki adımdır. Bir veritabanı için sunucu Gezgini'ni ekleyerek tabloları, saklı yordamları, görünümleri ve Visual Studio vb. tüm ekleyebilirsiniz. Ayrıca tablo verilerini görüntüleyebilir veya kendi sorgu Sorgu Tasarımcısı el ile veya grafik oluşturma. Ayrıca, biz yazılan veri kümeleri için DAL oluşturduğunuzda noktası Visual Studio için türü belirtilmiş veri kümesi oluşturulması gereken veritabanı gerekir. Bu bağlantı bilgisini zaman içinde o noktadaki de sağlarken, Visual Studio sunucu Gezgini'nde zaten kayıtlı veritabanları, aşağı açılan listesini otomatik olarak doldurur.

SQL Server 2005 Express Edition veritabanına kullanmak istediğiniz Sunucu Gezginine Northwind veritabanına ekleme adımlarını bağımlı **uygulama\_veri** klasörü veya bir Microsoft SQL Server 2000 veya 2005 varsa Bunun yerine kullanmak istediğiniz veritabanı sunucusu Kurulumu

## <a name="using-a-database-in-theappdatafolder"></a>Bir veritabanı içinde App kullanarak\_DataFolder

Bir SQL Server 2000 veya 2005 veritabanı sunucusuna bağlanmak için sahip değil veya veritabanı bir veritabanı sunucusuna eklemek zorunda kalmamak basitçe istediğiniz, indirilen websit içinde bulunan Northwind veritabanının SQL Server 2005 Express Edition sürümü kullanabilirsiniz. e's **uygulama\_veri** klasörü (**NORTHWND. MDF**).

Bir veritabanı yerleştirilen **uygulama\_veri** klasör için sunucu Gezgini'ni otomatik olarak eklenir. SQL Server 2005 Express makinenizde yüklü sürümü olduğunu varsayarsak NORTHWND adlı bir düğüm görmeniz gerekir. Sunucu Gezgini'ndeki MDF genişletin ve kendi tablolar, görünümler, saklı yordam ve benzeri (bkz: Şekil 2) keşfedin.

**Uygulama\_veri** klasör ayrıca Microsoft Access tutun **.mdb** dosyaları, SQL Server dekiler gibi Sunucu Gezgini için otomatik olarak eklenir. SQL Server seçenekleri hiçbirini kullanmak istemiyorsanız, her zaman için [Northwind veritabanı dosyasını Microsoft Access sürümünü indirin](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) ve bırakın **uygulama\_veri** dizin. Access veritabanları olarak olmayan Canlı unutmayın, Bununla birlikte, özellik açısından zengin SQL Server ve web sitesi senaryolarında kullanılmak üzere tasarlanmamıştır. Ayrıca, birkaç 35 + öğreticiler erişim tarafından desteklenmeyen belirli bir veritabanı düzeyinde özellikleri yararlanacaktır.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Microsoft SQL Server 2000 veya 2005 veritabanı sunucusu veritabanına bağlanma

Alternatif olarak, bir veritabanı sunucusunda yüklü Northwind veritabanına bağlanabilir. Veritabanı sunucusu zaten yüklenmiş Northwind veritabanı yoksa, önce bu veritabanı sunucusuna bu öğreticinin indirme ya da dahil yükleme betiği çalıştırarak eklemelisiniz [Northwind SQL Server 2000 sürümü indiriliyor ve yükleme komut dosyası](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) Microsoft'un web sitesinden doğrudan.

Yüklü veritabanı oluşturduktan sonra Visual Studio'da Sunucu Gezgini için veri bağlantıları düğümüne sağ tıklayın ve bağlantı Ekle seçin gidin. Görünüme Git sunucu Gezgini'ni görmüyorsanız, / Sunucu Gezgini veya isabet Ctrl + Alt + S. Kimlik doğrulama bilgilerini ve veritabanı adını bu sunucuya bağlanmak için belirleyebileceğiniz Bağlantı Ekle iletişim kutusunu getirir. Başarılı bir şekilde veritabanı bağlantı bilgilerini yapılandırdıktan ve Tamam düğmesine tıklandığında sonra veritabanı veri bağlantıları düğümü altında bir düğüm olarak eklenir. Tablolar, görünümler, saklı yordamlar ve benzeri keşfetmek için veritabanı düğümü genişletebilirsiniz.


![Veritabanı sunucunuzun Northwind veritabanına bir bağlantı Ekle](creating-a-data-access-layer-cs/_static/image4.png)

**Şekil 2**: veritabanı sunucunuzun Northwind veritabanına bir bağlantı Ekle


## <a name="step-2-creating-the-data-access-layer"></a>2. adım: veri erişim katmanını oluşturma

İle çalışırken verileri bir seçenek (bir web uygulamasında, ASP.NET sayfaları yap sunu katmanını ayarlama) doğrudan sunu katmanına verilere özgü mantığı ekleme oluşturmaktır. Bu form, ASP.NET sayfa kod bölümünde ADO.NET kod yazma veya bir işaretleme bölümünden SqlDataSource denetimi kullanarak alabilir. Her iki durumda da, bu yaklaşım, veri erişim mantığına sıkı bir şekilde içeren sunu katmanı couples. Önerilen yaklaşım, ancak veri erişim mantığına sunu katmanı ayırmaktır. Bu ayrı bir katman DAL kısaca, veri erişim katmanı olarak adlandırılır ve genellikle ayrı bir sınıf kitaplığı projesi olarak uygulanır. Bu katmanlı mimari avantajları iyi belgelenmiştir (Bu avantajlar hakkında bilgi için bu öğreticinin sonunda "Başka okumalar" bölümüne bakın) ve biz bu dizide alacağınız yaklaşımdır.

Veritabanına bir bağlantı oluşturma gibi temel veri kaynağına özgü tüm kodu verme **seçin**, **Ekle**, **güncelleştirme**, ve  **SİLME** komutları vb. DAL içinde bulunmalıdır. Sunu katmanı gibi veri erişim kodu herhangi bir referansı içermemesi gerekir, ancak bunun yerine tüm veri istekleri için DAL çağırıyor olmanız gerekir. Veri erişim katmanları, genellikle temel alınan veritabanı verilerine erişmek için yöntemler içerir. Örneğin, Northwind veritabanına sahip **ürünleri** ve **kategorileri** ürünler için satış ve ait oldukları kategorileri kayıt tablolar. Bizim DAL biz gibi yöntemleri vardır:

- **GetCategories(),** tüm kategorileri hakkında bilgi döndürülür
- **GetProducts()**, tüm ürünleri hakkında bilgi döndürülür
- **GetProductsByCategoryID (*CategoryID*)**, belirtilen bir kategoriye ait tüm ürünleri döndürülür
- **GetProductByProductID(*productID*)**, which will return information about a particular product

Bu yöntemler çağrıldığında, veritabanına bağlanmak, uygun sorgu sorunu ve sonuçları döndürür. Biz bu sonuçları nasıl iade önemlidir. Bu yöntemler, yalnızca bir veri kümesi veya veritabanı sorgusunun doldurulmuş DataReader döndürebilir, ancak kullanarak bu sonuçları ideal olarak döndürülmelidir *türü kesin belirlenmiş nesnelerin*. Ancak bunun tersini de geniş yazılmış bir nesne bir çalışma zamanına kadar olan şema bilinmiyor türü kesin belirlenmiş bir nesne olan şema aracılığı derleme zamanında tanımlanmış biridir.

Bunları doldurmak için kullanılan veritabanı sorgusunun döndürdüğü sütun şema tanımlamış gibi DataReader ve veri kümesini (varsayılan) geniş yazılmış nesneleridir. Geniş yazılmış DataTable ihtiyacımız gibi bir söz dizimi kullanmak için belirli bir sütuna erişmek için:  <strong><em>DataTable</em>. Satırlar [<em>dizin</em>] ["<em>columnName</em>"]</strong>. Sütun adı bir dize veya sıra dizini kullanarak erişmek için gereken olgu tarafından sergilenen DataTable'nın bu örneğinde gevşek yazarak. Kesin türü belirtilmiş DataTable, öte yandan, her bir özellik olarak uygulanan sütunlarını benzeyen kod výsledek olacaktır:  <strong><em>DataTable</em>. Satırlar [<em>dizin</em>]. *columnName</strong>*.

Kesin olarak belirlenmiş nesneler döndürmek için geliştiricilerin kendi özel iş nesneler oluşturmak veya türü belirtilmiş veri kümeleri kullanabilirsiniz. Özellikleri genellikle iş nesnesi temel alınan veritabanı tablosunun sütunları gösterecek bir sınıfı temsil ettiğinden, bir iş nesnesi geliştirici tarafından uygulanır. Türü belirtilmiş veri kümesi, sizin için bir veritabanı şeması ve üyeleri bu şemaya göre kesin göre Visual Studio tarafından oluşturulan bir sınıftır. Türü belirtilmiş veri kümesine erişiminizin DataRow ADO.NET DataSet ve DataTable sınıfları genişleten sınıftan oluşur. Kesin türü belirtilmiş DataTable ek olarak, yazılan veri kümeleri artık ayrıca TableAdapter bağdaştırıcılarını veri kümesinin DataTable doldurmak ve veritabanına değişikliklerini DataTable içinde yayma yöntemlerini sınıflar içerir.

> [!NOTE]
> Olumlu ve olumsuz nesnelerine karşı özel iş türü belirtilmiş veri kümeleri kullanma hakkında daha fazla bilgi için başvurmak [veri katmanı bileşenleri tasarlama ve veri katmanları aracılığıyla geçirme](https://msdn.microsoft.com/library/ms978496.aspx).


Kesin türü belirtilmiş veri kümeleri için bu öğreticileri mimarisi kullanacağız. Şekil 3'te, yazılan veri kümelerini kullanan bir uygulamanın farklı Katmanlar arasındaki iş akışını gösterilir.


[![Tüm veri erişim kodu, DAL için sahip](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Şekil 3**: tüm veri erişim kodu DAL ile gönderilen ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Türü belirtilmiş veri kümesi ve tablo bağdaştırıcısı oluşturma

Bizim DAL oluşturmaya başlamak için türü belirtilmiş veri kümesi için Projemizin ekleyerek başlayın. Bunu yapmak için Çözüm Gezgini'nde proje düğümüne sağ tıklayın ve Yeni Öğe Ekle'yi seçin. Şablonlar listesinden veri kümesi seçeneğini seçin ve adlandırın **Northwind.xsd**.


[![Yeni bir veri kümesi projenize eklemek seçin](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Şekil 4**: yeni bir veri kümesi için projenize eklemek seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image10.png))


Ekleme, veri kümesine eklemek için istendiğinde tıkladıktan sonra **uygulama\_kod** klasörü, Evet'i seçin. Türü belirtilmiş veri kümesi için tasarımcı görüntülenir ve türü belirtilmiş veri kümesi, ilk TableAdapter eklemenize olanak sağlayan, TableAdapter Yapılandırma Sihirbazı başlar.

Türü belirtilmiş veri kümesi bir kesin türü belirtilmiş veri koleksiyonu görev yapar; Bu, her biri, sırayla kesin türü belirtilmiş DataRow örneklerini oluşur kesin türü belirtilmiş DataTable örnekleri, oluşur. Bu öğretici serisinde çalışmak için gereken temel alınan veritabanı tabloların her biri için kesin türü belirtilmiş DataTable oluşturacağız. Bir DataTable için oluşturmaya başlayalım **ürünleri** tablo.

Kesin türü belirtilmiş DataTable, temel alınan veritabanı tablosundan veri erişim konusunda herhangi bir bilgi içermediğini aklınızda bulundurun. DataTable doldurmak için verileri almak için bizim veri erişim katmanı işlevleri bir TableAdapter sınıfı kullanırız. İçin sunduğumuz **ürünleri** DataTable, TableAdapter yöntemleri içerecek **GetProducts()**, **GetProductByCategoryID (*CategoryID*)** ve benzeri biz sunu katmanı çağıran. DataTable'nın katmanlar arasında veri iletmek için kullanılan türü kesin olarak belirtilmiş nesneler olarak görev yapacak rolüdür.

TableAdapter Yapılandırma Sihirbazı ile çalışmak için hangi veritabanı seçmenizi isteyerek başlar. Aşağı açılan listede bu veritabanlarını sunucu Gezgini'nde gösterilir. Sunucu Gezgini için Northwind veritabanı eklemediyseniz, bunu yapmak için şu anda yeni bağlantı düğmesi tıklayabilirsiniz.


[![Northwind veritabanı aşağı açılan listeden seçin.](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Şekil 5**: Northwind veritabanına aşağı açılan listeden seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image13.png))


Veritabanını seçtikten sonra İleri'ye tıklama, bağlantı dizesini kaydetmek isteyip istemediğiniz sorulur **Web.config** dosya. Bağlantı dizesi kaydederek bu sabit TableAdapter sınıfları, bağlantı dizesi bilgilerini gelecekte değişirse, şeyler basitleştirir kodlanmış olması önlenir. Yapılandırma dosyasında bağlantı dizesini kaydetmek tercih ederseniz yerleştirilir **&lt;connectionStrings&gt;** olabilecek bölüm [isteğe bağlı olarak şifrelenmiş](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) geliştirilmiş Güvenlik ya da yöneticiler için daha uygundur IIS GUI yönetim aracını içinde yeni ASP.NET 2.0 özellik sayfası aracılığıyla daha sonra değiştirilmiş.


[![Bağlantı dizesini Web.config dosyasına kaydedin](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Şekil 6**: bağlantı dizesini Kaydet **Web.config** ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image16.png))


Ardından, ilk kesin türü belirtilmiş DataTable için şema tanımlamak ve ilk yöntem için kesin türü belirtilmiş veri kümesini doldururken kullanmak, TableAdapter sağlamak ihtiyacımız var. Bu iki adımı, aynı anda bizim DataTable yansıtılan istediğimiz sütunları tablodan döndüren bir sorgu oluşturarak yapılır. Sihirbazın sonunda Biz bu sorgu için bir yöntem ad vereceksiniz. Sonra elde edilir, bu yöntem bizim sunu katmanını çağrılabilir. Bu yöntem, tanımlanan sorguyu ve kesin türü belirtilmiş DataTable doldurmak.

SQL sorgusu tanımlama kullanmaya başlamak için biz öncelikle TableAdapter sorgu vermek istiyoruz nasıl belirtmeniz gerekir. Biz bir geçici SQL deyimini kullanın, yeni bir saklı yordam oluşturmak veya mevcut bir saklı yordamı kullanın. Bu öğreticiler için geçici SQL deyimleri kullanacağız. Başvurmak [Brian Noyes](http://briannoyes.net/)kullanıcının makalesi, [Visual Studio 2005 veri kümesi Tasarımcısı ile veri erişim katmanını oluşturma](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) saklı yordamlar kullanma örneği için.


[![Geçici SQL deyimi kullanarak verileri Sorgulama](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Şekil 7**: geçici SQL deyimi kullanarak verileri Sorgulama ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image19.png))


Bu noktada SQL sorgusuna el ile yazabilirsiniz. İlk yöntem Düzenleyici içindeki TableAdapter oluştururken genellikle karşılık gelen DataTable ifade edilmesi gerekir. Bu sütunları döndürüldüğü bir sorgu olmasını istersiniz. Biz bunu tüm sütunları ve bulunan tüm satırlar döndüren bir sorgu oluşturarak gerçekleştirmenin **ürünleri** tablosu:


[![TextBox'a SQL sorgusunu girin](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Şekil 8**: SQL sorgu içine metin kutusuna ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image22.png))


Alternatif olarak, sorgu Tasarımcısını kullanın ve grafik sorgusu, Şekil 9'da gösterildiği gibi oluşturun.


[![Sorgu, sorgu Düzenleyicisi'ni kullanarak grafik oluşturun](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Şekil 9**: Sorgu grafik sorgu Düzenleyicisi aracılığıyla oluşturun ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image25.png))


Sorguyu oluşturduktan sonra ancak sonraki ekrana geçmeden önce Gelişmiş Seçenekler düğmesine tıklayın. Web sitesi projelerinde, "oluşturma INSERT, Update ve Delete deyimlerini" yalnızca Gelişmiş seçenek varsayılan olarak seçili olur; bir sınıf kitaplığı veya bir Windows projeden bu sihirbazı çalıştırırsanız "iyimser eşzamanlılık kullan" seçeneğini da seçilir. Şimdilik "iyimser eşzamanlılık kullan" seçeneğini işaretsiz bırakın. İyimser eşzamanlılık sonraki öğreticilerde inceleyeceğiz.


[![Yalnızca Generate INSERT, Update ve Delete deyimleri seçeneği seçin](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Şekil 10**: yalnızca Generate INSERT, Update ve Delete deyimleri seçeneği seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image28.png))


Gelişmiş Seçenekleri doğruladıktan sonra son ekrana devam etmek için İleri'ye tıklayın. Burada biz Tableadapter'a eklemek için hangi yöntemlerin seçmeniz istenir. Veri doldurmak için iki Düzen vardır:

- **Bir DataTable Doldur** DataTable bir parametre olarak alır ve onu doldurur bir yöntem oluşturulur. Bu yaklaşımda, sorgu sonuçlarına göre. ADO.NET DataAdapter sınıfı, örneğin, bu desenle uygulayan kendi **Fill()** yöntemi.
- **Bir DataTable Döndür** bu yaklaşımı yöntemi oluşturur ve DataTable sizin içi doldurur ve yöntemlerin dönüş değeri olarak döndürür.

TableAdapter birini veya her ikisini bu desenleri uygulamak olabilir. Burada sağlanan yöntemleri de yeniden adlandırabilirsiniz. Yalnızca Bu öğretici boyunca ikinci Düzen kullanacağız ancak şimdi iki onay kutusunun işaretli bırakın. Ayrıca, şimdi yerine genel Yeniden Adlandır **GetData** yönteme **GetProducts**.

Son "GenerateDBDirectMethods," onay kutusunu işaretlediyseniz, oluşturur **INSERT()**, **Update()**, ve **Delete()** TableAdapter yöntemleri. Bu seçeneği işaretlemeden bırakın, tüm güncelleştirmeleri TableAdapter bağdaştırıcısının tek yapılması gerekir **Update()** türü belirtilmiş veri kümesi, bir DataTable, tek bir DataRow veya bir dizi DataRow alan yöntemi. (Belirttiyseniz denetlenmeyen "Generate INSERT, Update ve Delete deyimleri" Bu checkbox'ın gelişmiş özelliklerinden Şekil 9'daki seçeneğini ayar, herhangi bir etkisi olacaktır.) Şimdi bu onay kutusunu seçili bırakın.


[![Yöntem adına GetData GetProducts Değiştir](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Şekil 11**: yöntem adını değiştirmek **GetData** için **GetProducts** ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image31.png))


Bitiş tıklayarak Sihirbazı tamamlayın. Sihirbaz kapandıktan sonra biz oluşturduğumuz DataTable gösteren veri kümesi Tasarımcısı döndürülür. Sütun listesinde görebilirsiniz **ürünleri** DataTable (**ProductID**, **ProductName**, vb.), yöntemlerinin yanı sıra  **Düzenleyen** (**Fill()** ve **GetProducts()**).


[![Ürünleri DataTable ve düzenleyen türü belirtilmiş veri kümesi eklendi](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Şekil 12**: **ürünleri** DataTable ve **düzenleyen** türü belirtilmiş veri kümesi eklendi ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image34.png))


Türü belirtilmiş veri kümesi tek bir DataTable ile bu noktada sahibiz (**Northwind.Products**) ve kesin tür belirtilmiş bir DataAdapter sınıfı (**NorthwindTableAdapters.ProductsTableAdapter**) ile bir  **GetProducts()** yöntemi. Bu nesneler, kod gibi tüm ürünlerin listesini erişmek için kullanılabilir:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Bu kod bir bit veri erişimi özel kod yazmak bize gerektirmiyor. Biz herhangi bir ADO.NET sınıflarını örneklemek yoktu, SQL sorguları, tüm bağlantı dizeleri başvurmak zorunda olmadığı veya saklı yordamları. Bunun yerine, TableAdapter alt düzey veri erişim kodu bize sağlıyor.

Bu örnekte kullanılan her nesne, IntelliSense ve derleme zamanı tür denetimi sağlamak Visual Studio sağlayan kesin, ayrıca. Ve TableAdapter tarafından döndürülen tüm DataTable'nın en iyi GridView DetailsView, DropDownList, CheckBoxList ve gibi birkaç başka Web denetimleri ASP.NET verilere bağlı olabilir. Tarafından döndürülen DataTable bağlama aşağıdaki örnekte gösterildiği **GetProducts()** yöntemi yalnızca bir scant üç satır içinde kod içinde GridView'a **sayfa\_yük** olay işleyicisi.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Ürünleri listeler GridView görüntülenir](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Şekil 13**: ürünleri listeler GridView görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image37.png))


Bu örnekte biz bizim ASP.NET sayfa üç satır kod yazmak gerekli **sayfa\_yük** olay işleyicisi, gelecekte inceleyeceğiz bildirimli olarak verileri almak için ObjectDataSource kullanma öğreticileri DAL. ObjectDataSource ile kod yazmadan olmaması ve sayfalama ve sıralama desteği de!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>3. adım: Veri erişim katmanı için yöntemleri parametreli ekleme

Bu noktada bizim **düzenleyen** sınıfında bir yöntem **GetProducts()**, döndüren tüm ürünleri veritabanında. Tüm ürünleri ile çalışmak için kesinlikle kullanışlı olsa da, size belirli bir ürün veya belirli bir kategoriye ait tüm ürünleri hakkında bilgi almak için ne zaman isteyeceksiniz zamanlar vardır. Bu işlevselliğin bizim için veri erişim katmanı eklemek için yöntemleri parametreli Tableadapter'a ekleyebiliriz.

Ekleyelim **GetProductsByCategoryID (*CategoryID*)** yöntemi. Veri kümesi Tasarımcısı için dönüş DAL için yeni bir yöntem eklemek için sağ **düzenleyen** bölümünde ve Sorgu Ekle öğesini seçin.


![TableAdapter öğesinde sağ tıklayın ve Sorgu Ekle](creating-a-data-access-layer-cs/_static/image38.png)

**Şekil 14**: Tableadapter'a sağ tıklayın ve Sorgu Ekle


Biz öncelikle olup olmadığını biz geçici SQL deyimi veya yeni veya mevcut bir saklı yordamı kullanarak veritabanına erişmek istemediğiniz sorulur. Geçici SQL deyimi yeniden kullanmak üzere şimdi seçin. Ardından, hangi SQL sorgu türünü kullanmak istiyoruz istenir. Belirtilen bir kategoriye ait tüm ürünleri döndürmek istediğimiz olduğundan, biz yazmak istediğiniz bir **seçin** satır döndüren bir ifade.


[![Satır döndüren SELECT deyimi oluşturulacağını seçin](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Şekil 15**: Create seçin bir **seçin** deyimi olan satırları döndürür ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image41.png))


Sonraki adım, verilere erişmek için kullanılan SQL sorgusunun tanımlamaktır. Belirli bir kategoriye ait ürünleri döndürmek istediğimiz olduğundan, aynı kullanmam <strong>seçin</strong> deyimden <strong>GetProducts()</strong>, ancak aşağıdaki <strong>burada</strong> yan tümcesi: <strong>nerede CategoryID = @CategoryID</strong> . <strong>@CategoryID</strong> Parametresi için TableAdapter Sihirbazı'nı oluşturma yöntemi giriş parametresi (yani, boş değer atanabilir bir tamsayı) ilgili türden gerektiğini gösterir.


[![Yalnızca belirtilen bir kategoride ürünleri döndürmek için bir sorgu girin](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Şekil 16**: yalnızca dönüş ürünlere belirtilen bir kategorideki bir sorgu girin ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image44.png))


Hangi veri erişim desenlerini yanı sıra kullanmak için oluşturulan yöntemler adlarını özelleştirme tercih edebilirsiniz son adımı. Dolgu deseni için adına değiştirelim <strong>FillByCategoryID</strong> ve için döndürülecek bir DataTable Döndür desen ( <strong>alma*X</strong>*  yöntemleri), kullanalım  <strong>GetProductsByCategoryID</strong>.


[![TableAdapter metotları adlarını seçin](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Şekil 17**: TableAdapter yöntemleri için bir ad seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image47.png))


DataSet Designer, Sihirbazı tamamladıktan sonra yeni TableAdapter yöntemleri içerir.


![Kategoriye göre ürünler olabilir artık sorgulanmasını](creating-a-data-access-layer-cs/_static/image48.png)

**Şekil 18**: kategoriye göre ürünler olabilir artık sorgulanabilir


Eklemek için birkaç dakikanızı bir **GetProductByProductID (*ProductID*)** teknikle yöntemi.

Bu parametreli sorgular veri kümesi Tasarımcısı'ndan doğrudan test edilebilir. TableAdapter yönteminde sağ tıklayın ve önizleme verileri seçin. Ardından, için parametreleri kullanın ve önizleme için değerleri girin.


[![Bu ürünler ait İçecekler kategorisindeki gösterilir](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Şekil 19**: Bu İçecekler kategorisindeki ürünlerin ait gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image51.png))


İle **GetProductsByCategoryID (*CategoryID*)** bizim DAL yöntemi artık oluştururuz bir ASP.NET sayfasını belirtilen bir kategoride yalnızca ürünleri görüntüler. Aşağıdaki örnek, olan İçecekler kategorideki tüm ürünleri gösterir. bir **CategoryID** 1.

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Bu ürünlerin İçecekler kategorisindeki görüntülenir](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Şekil 20**: Bu ürünleri İçecekler kategorisindeki görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>4. adım: Ekleme, güncelleştirme ve verileri silme

Ekleme, güncelleştirme ve verileri silmek için kullanılan iki deseni vardır. Veritabanını doğrudan deseni çağırmalıyım, birinci desen yöntemleri, çağrıldığında kapsamında sorunu bir **Ekle**, **güncelleştirme**, veya **Sil** komutu Veritabanı tek veritabanı kaydını üzerinde çalışır. Bu tür yöntemler, genellikle bir dizi karşılık gelen skaler değer (tamsayı, dizeler, Boole değerlerini, tarih/saat vb.) eklemek, güncelleştirmek veya silmek için değerleri geçirilir. Örneğin, bu deseni ile **ürünleri** tablo delete yöntemini bir tam sayı parametresi olması belirten **ProductID** INSERT yöntemi götürecek sırada silinecek kaydın bir dize **ProductName**, bir ondalık için **UnitPrice**, tamsayı **UnitsOnStock**ve benzeri.


[![Her bir INSERT, Update ve Delete isteği veritabanı anında gönderilir](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Şekil 21**: her ekleme, güncelleştirme ve silme isteği gönderildi veritabanı anında ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image57.png))


Bir tüm veri kümesi, DataTable ya da bir yöntem çağrısının DataRow koleksiyonunda deseni toplu güncelleştirmek için başvuracağınız, diğer bütün deseni güncelleştirmektir. Bu desene sahip bir geliştirici siler, ekler, bir DataTable tablosundaki DataRow değiştirir ve sonra bu DataRow ya da DataTable bir güncelleştirme yönteme geçirir. Bu yöntem sonra geçirilen DataRow sıralar, bunlar, eklenen, silinmiş veya değiştirilmiş olup olmadığını belirler (DataRow nesnesinin aracılığıyla [RowState özelliği](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) değer) ve her kayıt için uygun veritabanı isteği yayınlar.


[![Güncelleştirme yöntemi çağrıldığında tüm değişiklikler veritabanı ile eşitlenir](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Şekil 22**: güncelleştirme yöntemi çağrıldığında, tüm değişiklikleri veritabanı ile senkronize edilir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter varsayılan olarak toplu güncelleştirme deseni kullanır, ancak DB doğrudan düzeni de destekler. Biz "Generate INSERT, Update ve Delete deyimlerini" Gelişmiş özelliklerinden bizim TableAdapter oluştururken seçeneği bu yana **düzenleyen** içeren bir **Update()** yöntemi Bu, toplu güncelleştirme desenini uygular. Özellikle, TableAdapter'ı içeren bir **Update()** türü belirtilmiş veri kümesi, kesin türü belirtilmiş DataTable ya da bir veya daha fazla DataRow geçirilebilir yöntemi. Bırakılırsa ne zaman önce DB doğrudan deseni TableAdapter'ı oluşturma da aracılığıyla uygulanacak "GenerateDBDirectMethods" onay kutusunun **INSERT()**, **Update()**, ve **Delete()**  yöntemleri.

TableAdapter bağdaştırıcısının her iki veri değiştirme desenleri kullanın **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** vermek için özellikler kendi **Ekle** , **Güncelleştirme**, ve **Sil** veritabanına komutları. İnceleyin ve değiştirme **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** veri kümesi Tasarımcısı'nda TableAdapter bağdaştırıcısının tıklayarak ve ardından Özellikler Özellikler penceresinde. (TableAdapter ve seçtiğinizden emin olun **düzenleyen** Özellikler penceresinde açılan listesinden seçilen bir nesnedir.)


[![TableAdapter'in InsertCommand ve UpdateCommand DeleteCommand özellikleri](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Şekil 23**: TableAdapter'in **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** özellikleri ([görünümü için tam boyutlu Görüntü](creating-a-data-access-layer-cs/_static/image63.png))


İnceleme veya bu veritabanı komutunun özelliklerinden herhangi birini değiştirmek için tıklayın **CommandText** alt özellik Sorgu Oluşturucu ortaya çıkarır.


[![INSERT, UPDATE ve DELETE deyimleri Sorgu Oluşturucu'da yapılandırma](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Şekil 24**: yapılandırma **Ekle**, **güncelleştirme**, ve **Sil** deyimlerinde Sorgu Oluşturucusu ([tam boyutlu görüntüyügörüntülemekiçintıklayın](creating-a-data-access-layer-cs/_static/image66.png))


Aşağıdaki kod örneği, fiyat değil üretilmeyen ve 25 birimleri stoktaki ya da daha az olan tüm ürünlerin çift toplu güncelleştirme deseni kullanmayı gösterir:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Aşağıdaki kod, program aracılığıyla belirli bir ürünü silebilir, ardından bir güncelleştirme için DB doğrudan düzeni kullanın ve ardından yeni bir tane ekleyin gösterilmektedir:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Oluşturma özel INSERT, Update ve Delete yöntemleri

**INSERT()**, **Update()**, ve **Delete()** DB doğrudan yöntemi tarafından oluşturulan yöntemler özellikle fazla sayıda sütun içeren tablolar için biraz hantal olabilir. IntelliSense'nın ne özellikle açık değilse yardımı olmadan önceki kod örneğinde mi arıyorsunuz **ürünleri** her giriş parametresi olarak tablo sütunu eşlendiğini **Update()** ve **INSERT()**  yöntemleri. Biz yalnızca istediğinizde tek bir sütun veya iki güncelleştirme veya özelleştirilmiş istediğiniz zamanlar olabilir **INSERT()** olur, belki de yöntemi yeni eklenen kaydın değerini döndürmek **kimlik** (otomatik artış) alan.

Özel bir yöntem oluşturmak için veri kümesini tasarımcıya dönün. TableAdapter öğesinde sağ tıklayın ve eklemek için TableAdapter Sihirbazı'nı döndüren sorguyu seçin. İkinci ekranda biz oluşturmak için sorgu türünü belirtebilirsiniz. Yeni ürün ekler ve sonra yeni eklenen kaydın değerini döndüren bir yöntem oluşturalım **ProductID**. Bu nedenle, oluşturmak için iyileştirilmiş bir **Ekle** sorgu.


[![Ürünler tablosuna yeni bir satır eklemek için bir yöntem oluşturma](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Şekil 25**: yeni satır eklemek için bir yöntem oluşturma **ürünleri** tablo ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image69.png))


Sonraki ekranda **InsertCommand**'s **CommandText** görünür. Bu sorgu ekleyerek büyütmek **kapsamı seçin\_IDENTITY()** sorgunun sonunda, eklenen son kimlik değeri döndürülür bir **kimlik** aynı kapsamda sütun. (Bkz [teknik belgeler](https://msdn.microsoft.com/library/ms190315.aspx) hakkında daha fazla bilgi için **kapsam\_IDENTITY()** ve büyük olasılıkla istediğiniz neden [kapsamı kullanan\_IDENTITY() yerine @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Bitirdiğinizden emin olun **Ekle** eklemeden önce deyimi noktalı virgül ile **seçin** deyimi.


[![SCOPE_IDENTITY() değeri döndürmek için sorguyu büyütmek](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Şekil 26**: Sorgu dönmek büyütmek **kapsam\_IDENTITY()** değeri ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image72.png))


Son olarak, yeni yöntemin adı **InsertProduct**.


[![InsertProduct için yeni bir yöntem adı ayarlayın](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Şekil 27**: yeni bir yöntem adı kümesine **InsertProduct** ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image75.png))


Göreceksiniz için veri kümesi Tasarımcısı döndüğünüzde **düzenleyen** içeren yeni bir yöntem **InsertProduct**. Bu yeni yöntemi her sütun için bir parametre yoksa **ürünleri** tablo olasılığı olan unuttum sonlandırmak **Ekle** deyimi noktalı virgül ile. Yapılandırma **InsertProduct** yöntemi ve bir noktalı virgül sınırlandırma olduğundan emin olun **Ekle** ve **seçin** deyimleri.

Varsayılan olarak, yöntemleri sorunu sorgu olmayan yöntemleri, etkilenen satır sayısını döndürürler anlamı ekleyin. Ancak, istediğimiz **InsertProduct** etkilenen satır sayısını değil sorgu tarafından döndürülen değer döndürmek için yöntemi. Bunu gerçekleştirmek için ayarlamak **InsertProduct** yöntemin **ExecuteMode** özelliğini **skaler**.


[![Skaler için ExecuteMode özelliğini değiştirme](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Şekil 28**: değişiklik **ExecuteMode** özelliğini **skaler** ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image78.png))


Aşağıdaki kod bu yeni gösterir **InsertProduct** eylem yöntemi:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>5. adım: veri erişim katmanı Tamamlanıyor

Unutmayın **ProductsTableAdapters** sınıfı döndürür **CategoryID** ve **SupplierID** değerlerini **ürünleri** tablo, ancak içermeyen **CategoryName** sütundan **kategorileri** tablo veya **CompanyName** sütundan **tedarikçileri**tablosuna ve bu büyük olasılıkla ürün bilgileri gösterilirken görüntülemeyi istediğimiz sütunları olmasına rağmen. TableAdapter bağdaştırıcısının başlangıç yöntemini, biz genişletebilirsiniz **GetProducts()**, her ikisi de içerecek şekilde **CategoryName** ve **CompanyName** güncelleştirecek sütun değerleri Bu yeni sütunların de içerecek şekilde kesin türü belirtilmiş DataTable.

Bu sorun, ancak ekleme, TableAdapter bağdaştırıcısının yöntemler olarak güncelleştiriliyor, sunabilir ve silme veri alanlarını bu başlangıç yöntemini temel alır. Ekleme, güncelleştirme ve silme olmayan için Neyse ki, otomatik olarak oluşturulan yöntemler alt sorgularda etkilenen **seçin** yan tümcesi. Bizim sorgulara ekleme dikkat ederek tarafından **kategorileri** ve **tedarikçileri** alt sorgularda, olarak yerine **katılın** s, ki önlemek verileri değiştirmek için bu yöntemleri yeniden zorunda. Sağ **GetProducts()** yönteminde **düzenleyen** ve Yapılandır'ı seçin. Daha sonra Ayarla **seçin** yan tümcesi, aşağıdaki şekilde gözükmesi ister:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![SELECT deyimi güncelleştirmesi GetProducts() yöntemi](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Şekil 29**: güncelleştirme **seçin** bildirimi **GetProducts()** yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image81.png))


Güncelleştirdikten sonra **GetProducts()** DataTable iki yeni sütun içerecektir bu yeni bir sorgu yöntemi: **CategoryName** ve **üretici**.


![Ürünleri DataTable iki yeni sütun yok](creating-a-data-access-layer-cs/_static/image82.png)

**Şekil 30**: **ürünleri** DataTable iki yeni sütunu var.


Güncelleştirme için bir dakikanızı ayırın **seçin** yan tümcesinde **GetProductsByCategoryID (*CategoryID*)** yöntemi de.

Güncelleştirmezseniz **GetProducts()** **seçin** kullanarak **katılın** veri kümesi Tasarımcısı söz dizimi ekleme, güncelleştirme ve silme yöntemleri otomatik olarak oluşturmak mümkün olmayacak DB doğrudan desenini kullanarak veritabanı verileri. Bunun yerine, ile yaptığımız gibi el ile çok oluşturmak zorunda kalırsınız **InsertProduct** Bu öğreticinin önceki kısımlarındaki yöntemi. Ayrıca, el ile sağlamanız gerekir **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** özellik değerlerinin toplu düzeni kullanmak istiyorsanız.

## <a name="adding-the-remaining-tableadapters"></a>Kalan TableAdapter'ları ekleme

Şimdiye kadar biz yalnızca tek bir veritabanı tablosu için tek bir TableAdapter ile çalışma sırasında inceledik. Ancak, Northwind veritabanı ile çalışmak için web uygulamamızı gerekir birkaç ilgili tabloları içerir. DataTable ilgili türü belirtilmiş veri kümesi birden çok içerebilir. Bu nedenle bizim DAL tamamlamak için Biz bu öğreticilerde kullanacağız diğer tablolar için DataTable eklemeniz gerekir. Türü belirtilmiş veri kümesi için yeni bir TableAdapter eklemek için veri kümesi Tasarımcısı'nı açın, tasarımcıda sağ tıklayın ve Ekle'yi seçin / TableAdapter. Yeni bir DataTable ve TableAdapter oluşturmak ve biz Bu öğreticinin önceki bölümlerinde incelenen sihirbaz size yol gösterir.

Aşağıdaki TableAdapter'ları ve yöntemlerini kullanarak aşağıdaki sorguları oluşturmak için birkaç dakika sürebilir. Unutmayın sorgularda **düzenleyen** her ürün kategorisi ve tedarikçi adları almak için alt sorgular içerir. Ayrıca, takip varsa önceden eklediğiniz **düzenleyen** sınıfın **GetProducts()** ve **GetProductsByCategoryID (*CategoryID* )** yöntemleri.

- **Düzenleyen**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![Dört TableAdapters eklendikten sonra veri kümesi Tasarımcısı](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Şekil 31**: veri kümesi Tasarımcısı sonra dört TableAdapters eklendi ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>DAL için özel kod ekleme

Türü belirtilmiş DataSet nesnesine eklenen DataTables ve TableAdapters bir XML şema tanımı dosyası olarak ifade edilir (**Northwind.xsd**). Sağ tıklayarak bu şema bilgileri görüntüleyebilirsiniz **Northwind.xsd** dosya Çözüm Gezgini'nde ve kodu görüntüle seçme.


[![XML şema tanımı (XSD) dosyası kategoriye için türü belirtilmiş veri kümesi](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Şekil 32**: kategoriye türü belirtilmiş veri kümesi için XML şema tanımı (XSD) dosyası ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image88.png))


Bu şema bilgileri derlendiğinde veya bu noktada, üzerinden hata ayıklayıcı ile adım (gerekirse) çalışma zamanında C# veya Visual Basic kodu tasarım zamanında çevrilir. Görüntülemek için otomatik olarak oluşturulan bu kod gidin sınıf görünümü ve ayrıntıya TableAdapter veya türü belirtilmiş veri kümesi sınıfları. Sınıf Görünümü ekranınızdaki görmüyorsanız, Görünüm menüsüne gidin ve buradan seçin veya Ctrl + Shift + C tuşlarına basın. Sınıf Görünümü özellikleri, yöntemleri ve olayları türü belirtilmiş veri kümesi ve TableAdapter sınıfının görebilirsiniz. Belirli bir yöntem kodunu görüntülemek için Sınıf Görünümü'nde yöntem adına çift tıklayın veya sağ tıklayın ve Tanıma Git'ı seçin.


![Sınıf Görünümü tanımına Git seçerek otomatik olarak oluşturulan kod İnceleme](creating-a-data-access-layer-cs/_static/image89.png)

**Şekil 33**: otomatik olarak oluşturulan kod sınıf görünümünden tanımına Git seçerek inceleyin


Otomatik olarak oluşturulan kod harika zaman tasarrufu olabilir, ancak kod genellikle çok geneldir ve uygulamanın benzersiz ihtiyaçlarını karşılamak için özelleştirilmiş olması gerekir. Otomatik olarak oluşturulan kodu genişletme, oluşturulan kod aracı "yeniden" ve özelleştirmelerinizi üzerine yazmayı zamanı karar verebilirsiniz riskidir. .NET 2.0'ın yeni kısmi sınıf kavramı sayesinde, bir sınıfı birden çok dosyaya bölme kolaydır. Bu bizim özelleştirmeleri üzerine Visual Studio hakkında endişelenmenize gerek kalmadan kendi yöntemlerini, özelliklerini ve olaylarını otomatik olarak oluşturulan sınıfa eklemek sağlıyor.

DAL özelleştirme göstermek için ekleyelim bir **GetProducts()** yönteme **SuppliersRow** sınıfı. **SuppliersRow** sınıfı temsil eder, tek bir kayıtta **tedarikçileri** tablo; çok ürünlerin her tedarikçi can sağlayıcısı sıfır şekilde **GetProducts()** olanlar döndürür Belirtilen sağlayıcı ürünleri. Gerçekleştirmek için yeni bir sınıf dosyası oluşturma **uygulama\_kod** adlı klasöre **SuppliersRow.cs** ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Bu kısmi sınıf derleyiciye olduğunda oluşturmaya **Northwind.SuppliersRow** içerecek şekilde sınıfı **GetProducts()** yalnızca tanımladığımız yöntemi. Projenizi derleyin ve ardından sınıf görünümüne dönmek göreceğiniz **GetProducts()** yöntemi listelenmiş **Northwind.SuppliersRow**.


![GetProducts() yöntemi artık Northwind.SuppliersRow sınıfı bir parçasıdır](creating-a-data-access-layer-cs/_static/image90.png)

**Şekil 34**: **GetProducts()** yöntemi artık bir parçası olan **Northwind.SuppliersRow** sınıfı


**GetProducts()** yöntemi artık kullanılabilir ürünler kümesini belirli bir üretici için aşağıdaki kodun gösterdiği gibi numaralandırmak için:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Bu veriler, ayrıca, tüm ASP görüntülenebilir. NET veri Web denetimleri. Şu sayfaya bir GridView denetimi ile iki alan kullanır:

- Her bir sağlayıcı adını görüntüler bir BoundField ve
- Tarafından döndürülen sonuçlar bağlı Bulletedlıst denetimi içeren bir TemplateField **GetProducts()** her üretici için yöntemi.

Sonraki öğreticilerde bu ana öğe-ayrıntı raporları görüntülemek nasıl inceleyeceğiz. Şimdilik, bu örnek için eklenen özel yöntemi kullanarak göstermek için tasarlanmıştır **Northwind.SuppliersRow** sınıfı.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Tedarikçi şirket adı sol sütunda, sağ Their ürünleri listelenir](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Şekil 35**: tedarikçi şirket adı sol sütunda, sağ Their ürünleri listelenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Özet

Ne zaman DAL oluşturma bir web uygulaması oluşturma, sunu katmanı oluşturma başlamadan önce gerçekleşen ilk adımlarınızı biri olmalıdır. Visual Studio ile temel türü belirtilmiş veri kümeleri üzerinde bir DAL oluşturma 10-15 dakika içinde bir kod satırı yazmadan yapılabilecek bir görevdir. İlerletme öğreticiler bu DAL üzerinde oluşturacaksınız. İçinde [sonraki öğreticiye](creating-a-business-logic-layer-cs.md) biz bir dizi iş kuralları tanımlayın ve bunları ayrı bir iş mantığı katmanı uygulama öğrenin.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [TableAdapters kesin türü belirtilmiş DataTable VS 2005 ve ASP.NET 2.0 ile bir DAL oluşturma](https://weblogs.asp.net/scottgu/435498)
- [Veri katmanı bileşenleri tasarlama ve veri katmanları aracılığıyla geçirme](https://msdn.microsoft.com/library/ms978496.aspx)
- [Visual Studio 2005 veri kümesi Tasarımcısı ile veri erişim katmanını oluşturma](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [ASP.NET 2.0 yapılandırma bilgilerini şifrelemek uygulamalar](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter genel bakış](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Yazılan veri kümesi ile çalışma](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Kesin türü belirtilmiş veri erişimi Visual Studio 2005 ve ASP.NET 2.0 kullanma](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [TableAdapter yöntemleri genişletme](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Bir saklı yordamdan skaler veri alma](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda eğitim videosu

- [ASP.NET Uygulamalarında Veri Erişim Katmanları](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Nasıl el ile bir veri kümesi DataGrid'e bağlama](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Bir ASP uygulamasından veri kümeleri ve filtreler ile çalışma hakkında](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler, cüneyt'in yeşil, Hilton Giesenow, Dennis Patterson ve Konuğu, Liz Shulok, Abel Gomez ve Carlos Santos yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](creating-a-business-logic-layer-cs.md)
