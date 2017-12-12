---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: "Veri erişim katmanı (C#) oluşturma | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide biz çok baştan başlamanız ve veri erişim katmanı (bir veritabanı olarak bilgilere erişmek için yazılan veri kümeleri kullanarak DAL), oluşturmak."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: c610f84cfb82f38f9c67b757aa341c7a1497369c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-data-access-layer-c"></a>Veri erişim katmanı (C#) oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) veya [PDF indirin](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> Bu öğreticide biz çok baştan başlamanız ve veri erişim katmanı (bir veritabanı olarak bilgilere erişmek için yazılan veri kümeleri kullanarak DAL), oluşturmak.


## <a name="introduction"></a>Giriş

Web geliştiricileri, verileri ile çalışma geçici bizim yaşamlarını Uzayda Döndür. Kod almak ve ve toplamak ve onu özetlemek için web sayfaları değiştirmek için verileri depolamak için veritabanlarını oluşturuyoruz. ASP.NET 2. 0 ' Bu ortak desenler uygulamak için teknikleri inceleyeceksiniz uzun serideki ilk öğreticide budur. Oluşturma ile başlayacağız bir [yazılım mimarisi](http://en.wikipedia.org/wiki/Software_architecture) özel iş kurallarını uygulayan bir veri erişim katmanı (yazılan veri kümeleri, iş mantığı katmanı (BLL) kullanarak DAL,) oluşur ve ASP.NET oluşan bir sunu katmanı sayfalar ortak bir sayfa düzeni paylaşır. Bu arka uç öğrenmeniz, biz raporlamasına, taşırsınız yerleştirilmiş sonra görüntülemek nasıl gösteren özetleyin, toplamak ve bir web uygulamasından veri doğrulama. Bu öğreticileri, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Her öğretici C# ve Visual Basic sürümlerinde kullanılabilir ve kullanılan tam kod yüklenmesini içerir. (Bu ilk öğreticide oldukça uzun, ancak geri kalan çok daha fazla digestible yığınlar halinde sunulur.)

Bu öğreticileri için biz yerleştirilen Northwind veritabanı Microsoft SQL Server 2005 Express Edition sürümü kullanmaya başlayacağınız **uygulama\_veri** dizin. Veritabanı dosyası yanı sıra **uygulama\_veri** klasör durumunda farklı veritabanı sürümü kullanmak istediğiniz veritabanı oluşturmak için SQL komut dosyaları da içerir. Bu komut dosyalarını olması da olabilir [doğrudan Microsoft'tan yüklenen](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), dilerseniz. Northwind veritabanı farklı bir SQL Server sürümünü kullanıyorsanız, güncelleştirmeniz gerekecektir **NORTHWNDConnectionString** uygulamanın içinde ayarı **Web.config** dosya. Web uygulaması, dosya sistemi tabanlı Web sitesi projesi olarak Visual Studio 2005 Professional sürümü kullanılarak oluşturuldu. Ancak, tüm öğreticileri çalışır eşit olarak Visual Studio 2005, ücretsiz sürümü ile yanı sıra [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
Bu öğreticide biz çok baştan başlamanız ve veri erişim katmanı (ikinci öğreticide iş mantığı katmanı (BLL) oluşturma ve sayfa düzeni ve üçüncü Gezinti çalışma arkasından DAL), oluşturmak. Üçüncü bir foundation oluşturacaksınız sonra öğreticileri ilk üç düzenlenir. Biz bu ilk öğreticide, böylece Visual Studio'yu ayarlama yangın ve başlayalım çok sahibiz!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>1. adım: bir Web projesi oluşturma ve veritabanına bağlanma

Bizim veri erişim düzeyi (DAL) oluşturabilmeniz için önce ilk olarak bir web sitesi oluşturun ve bizim Veritabanı Kurulumu ihtiyacımız. Yeni bir dosya sistemi tabanlı ASP.NET web sitesi oluşturmaya başlayın. Bunu başarmak için Dosya menüsüne gidin ve yeni Web sitesi iletişim kutusunu görüntüleme yeni Web sitesi seçin. ASP.NET Web sitesi şablonunu seçin, dosya sistemine konum aşağı açılan listesi ayarlamak, web sitesi yerleştirmek için bir klasör seçin ve dil C# ayarlayın.


[![Yeni bir dosya sistemi tabanlı Web sitesi oluşturma](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Şekil 1**: New File System-Based Web sitesi oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image3.png))


Bu yeni bir web sitesi ile oluşturacak bir **Default.aspx** ASP.NET sayfası ve bir **uygulama\_veri** klasör.

Oluşturulan web sitesi ile Visual Studio'nun Sunucu Gezgininde veritabanına bir başvuru ekleyin sonraki adımdır. Bir veritabanı için Sunucu Gezgini ekleyerek tabloları, saklı yordamlar, görünümler ve Visual Studio vb. tüm ekleyebilirsiniz. Ayrıca, tablo verileri görüntülemek veya kendi sorguları Sorgu Oluşturucusu el ile veya grafik oluşturabilirsiniz. Biz yazılan veri kümeleri için DAL derlerken Ayrıca, biz noktası Visual Studio yazılan veri kümeleri oluşturulması veritabanı gerekir. Bu bağlantı bilgisini zamandaki o noktada sağladığımız olsa da, Visual Studio açılır listesini Server Explorer'da zaten kayıtlı veritabanları otomatik olarak doldurur.

Northwind veritabanı için Sunucu Gezgini ekleme adımları, SQL Server 2005 Express Edition veritabanında kullanmak istediğiniz bağımlı **uygulama\_veri** klasörü veya bir Microsoft SQL Server 2000 veya 2005 varsa Bunun yerine kullanmak istediğiniz veritabanı sunucusu kurulumu.

## <a name="using-a-database-in-theappdatafolder"></a>Bir veritabanı içinde App kullanarak\_DataFolder

Bir SQL Server 2000 veya 2005 veritabanı sunucusuna bağlanmak için yok veya veritabanı bir veritabanı sunucusuna eklemek zorunda kalmamak, yalnızca istediğiniz, indirilen websit içinde bulunan Northwind veritabanı SQL Server 2005 Express Edition sürümü kullanabilirsiniz e's **uygulama\_veri** klasörü (**NORTHWND. MDF**).

Bir veritabanı yerleştirilen **uygulama\_veri** klasörü için Sunucu Gezgini otomatik olarak eklenir. SQL Server 2005 Express makinenize yüklü sürümü, varsayarak NORTHWND adlı bir düğüm görmeniz gerekir. Sunucu Gezgininde MDF genişletin ve onun tabloları, görünümleri, saklı yordam ve benzeri (bkz. Şekil 2) keşfedin.

**Uygulama\_veri** klasörü ayrıca Microsoft Access tutun **.mdb** hangi SQL Server'dekiler gibi Sunucu Gezgini otomatik olarak eklenen dosyalar. SQL Server seçeneklerinden herhangi birini kullanmak istemiyorsanız, her zaman yapabilecekleriniz [Northwind veritabanı dosyası bir Microsoft Access sürümünü indirmek](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) ve içine bırakma **uygulama\_veri** dizin. Access veritabanları olarak olmayan Koru unutmayın, ancak zengin SQL Server ve web sitesi senaryolarda kullanılmak üzere tasarlanmamıştır. Ayrıca, birkaç 35 + öğreticileri Access tarafından desteklenmeyen bazı veritabanı düzeyi özellikleri yararlanacak olan.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Bir Microsoft SQL Server 2000 veya 2005 veritabanı sunucusu veritabanına bağlanma

Alternatif olarak, bir veritabanı sunucusunda yüklü Northwind veritabanına bağlanabilir. Veritabanı sunucusu yüklü Northwind veritabanı zaten yoksa, öncelikle bu veritabanı sunucusuna bu öğreticinin indirme ya da dahil yükleme komut dosyası çalıştırarak eklemeniz gerekir [Northwind SQL Server 2000 sürümü indirme ve yükleme komut dosyası](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) Microsoft'un web sitesinden doğrudan.

Yüklenen veritabanı olduktan sonra Visual Studio Server Explorer'da için veri bağlantıları düğümüne sağ tıklayın ve bağlantı Ekle seçin gidin. Sunucu Gezgini görünümüne gidin görmüyorsanız, / Sunucu Gezgini ya da isabet Ctrl + Alt + S. Bu kimlik doğrulama bilgilerini ve veritabanı adını bağlanmak için sunucunun belirtebileceğiniz Bağlantı Ekle iletişim kutusunu getirir. Başarılı bir şekilde veritabanı bağlantısı bilgilerini yapılandırdıktan ve Tamam düğmesine tıklandığında sonra veritabanı veri bağlantıları düğümü altında bir düğüm olarak eklenir. Tabloları, görünümleri, saklı yordamlar ve benzeri keşfetmek için veritabanı düğümü genişletebilirsiniz.


![Veritabanı sunucunuzun Northwind veritabanına bir bağlantı Ekle](creating-a-data-access-layer-cs/_static/image4.png)

**Şekil 2**: veritabanı sunucunuzun Northwind veritabanına bir bağlantı Ekle


## <a name="step-2-creating-the-data-access-layer"></a>2. adım: veri erişim katmanı oluşturma

İle çalışırken, verileri bir veri özgü mantığı (bir web uygulaması'nda sunu katmanı oluşturan ASP.NET sayfaları yapma) doğrudan sunu katmanına katıştırmak için seçenektir. Bu, ASP.NET sayfa kod bölümünde ADO.NET kod yazma veya bir işaretleme bölümünden SqlDataSource denetimi kullanarak form alabilir. Her iki durumda da, bu yaklaşım, veri erişim mantığı sıkı bir şekilde sunu katmanı ile couples. Önerilen yaklaşım, ancak, veri erişim mantığı sunu katmanı ayırmak için gerekir. Bu ayrı bir katman DAL kısaca, veri erişim katmanı olarak adlandırılır ve genel ayrı bir sınıf kitaplığı proje uygulanır. Bu katmanlı mimarisinin avantajları belgelendiğinden (aşağıdaki avantajları hakkında bilgi için bu öğreticinin sonunda "Başka okumalar" bölümüne bakın) ve biz bu dizide sürer yaklaşımdır.

Veritabanına bir bağlantı oluşturma gibi temel alınan veri kaynağına özgü tüm kod veren **seçin**, **Ekle**, **güncelleştirme**, ve  **SİLME** DAL komutlar ve benzeri bulunmalıdır. Sunu katmanı bu tür veri erişim kodu yönelik tüm başvuruları içermemesi gerekir, ancak bunun yerine tüm veri istekleri için DAL çağrılarını olmanız gerekir. Veri erişim katmanları genellikle temel alınan veritabanı veri erişmek için yöntemler içerir. Örneğin, Northwind veritabanı sahip **ürünleri** ve **kategorileri** ürünler satış ve ait oldukları kategorileri için kayıt tabloları. Bizim DAL biz yöntemler gibi olacaktır:

- **GetCategories(),** tüm kategorileri hakkında bilgiler döndürecektir
- **GetProducts()**, ürünlerin tamamı hakkında bilgi döndürecektir
- **GetProductsByCategoryID (*adlı kullanıcı, Categoryıd'si*) **, belirtilen bir kategoriye ait tüm ürünleri döndürecektir
- **GetProductByProductID (*ProductID*) **, belirli bir ürünü hakkında bilgi döndürecektir

Bu yöntem çağrıldığında, veritabanına bağlanmak, uygun sorgu vermek ve sonuçlar döndürebilir. Bu sonuçları nasıl döndürürüz önemlidir. Bu yöntemleri yalnızca bir veri kümesi veya veritabanı sorgusunun doldurulmuş DataReader döndürebilir ancak kullanarak bu sonuçlar ideal olarak döndürülmelidir *türü kesin belirlenmiş nesnelerin*. Geniş yazılmış bir nesne karşıtı biri, şema çalışma zamanına kadar bilinmiyor iken kesin türü belirtilmiş bir nesne, şema aracılığı derleme zamanında tanımlanan biridir.

Bunları doldurmak için kullanılan veritabanı sorgusunun döndürdüğü sütunlara göre kendi şema tanımlamış Örneğin, DataReader ve veri kümesi (varsayılan) geniş yazılmış nesneleridir. Belirli bir sütuna ihtiyacımız gibi sözdizimini kullanmak için geniş yazılmış bir DataTable erişmek için:   ***DataTable*. Satırlar [*dizin*] ["*columnName*"]**. Bu örnekte DataTable nesnesinin gevşek yazarak bir dize veya sıra dizini kullanarak sütun adı erişmek için ihtiyacımız olgu tarafından sergilenen. Kesin türü belirtilmiş bir DataTable Öte yandan, her özellikleri olarak uygulanan sütunları benzer kodunda sonuçlanan olacaktır:   ***DataTable*. Satırlar [*dizin*].* columnName***.

Türü kesin belirlenmiş nesnelerin döndürmek için geliştiricilerin kendi özel iş nesneleri oluşturmak veya yazılan veri kümeleri kullanabilirsiniz. Özellikleri genellikle iş nesnesi temel alınan veritabanı tablosunun sütunları yansıtacak bir sınıfı temsil eder gibi bir iş nesnesi geliştirici tarafından uygulanır. Yazılan veri kümesi, sizin için bir veritabanı şeması ve kesin türü belirtilmiş-bu şemaya göre üyeleri göre Visual Studio tarafından oluşturulan bir sınıftır. Yazılan veri kümesi kendisini ADO.NET veri kümesi, DataTable ve DataRow sınıfları genişleten sınıflardan oluşur. Kesin türü belirtilmiş DataTables ek olarak yazılan veri kümeleri artık ayrıca veri kümesi'nin DataTables doldurma ve veritabanında yapılan değişikliklerin DataTables içinde geri yayılıyor yöntemleriyle sınıflardır TableAdapters içerir.

> [!NOTE]
> Olumlu ve olumsuz özel iş nesnelerine karşı yazılan veri kümeleri kullanma hakkında daha fazla bilgi için bkz [veri katmanı bileşenleri tasarlama ve veri katmanlarını aracılığıyla geçirme](https://msdn.microsoft.com/en-us/library/ms978496.aspx).


Kesin türü belirtilmiş veri kümeleri için bu öğreticileri mimarisi kullanacağız. Şekil 3 yazılan veri kümeleri kullanan bir uygulama farklı katmanları arasında iş akışı gösterilmektedir.


[![Tüm veri erişim kodu DAL ile sahip](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Şekil 3**: tüm veri erişim kodu DAL ile gönderilen ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Türü belirtilmiş veri kümesi ve tablo bağdaştırıcısı oluşturma

Bizim DAL oluşturmaya başlamak için Projemizin için yazılmış bir veri kümesi ekleyerek başlatın. Bunu başarmak için Çözüm Gezgini'nde proje düğümüne sağ tıklayın ve Yeni Öğe Ekle'yi seçin. Veri kümesi seçeneği şablonları listesinden seçin ve adlandırın **Northwind.xsd**.


[![Yeni bir veri kümesi projenize eklemek için seçin](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Şekil 4**: yeni veri kümesi için projenize eklemek için seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image10.png))


Ekle, veri kümesine eklemek için istendiğinde tıkladıktan sonra **uygulama\_kod** klasörü, Evet'i seçin. Yazılan veri kümesi Tasarımcısı sonra görüntülenir ve ilk TableAdapter yazılan veri kümesine eklemenize olanak sağlayan TableAdapter Yapılandırma Sihirbazı başlar.

Yazılan veri kümesi veri kesin türü belirtilmiş koleksiyonu olarak hizmet verir; her biri sırayla kesin türü belirtilmiş DataRow örneklerini oluşan kesin türü belirtilmiş DataTable örnekleri, oluşur. Bu öğretici serisinde çalışmak için ihtiyacımız temel alınan veritabanı tabloların her biri için kesin türü belirtilmiş bir DataTable oluşturacağız. DataTable için oluşturmayla başlayalım **ürünleri** tablo.

Kesin türü belirtilmiş DataTables kendi temel veritabanı tablodan veri erişim hakkında hiçbir bilgi içermez aklınızda bulundurun. DataTable doldurmak için verileri almak için veri erişim katmanı işlevleri bir TableAdapter sınıf kullanırız. İçin bizim **ürünleri** DataTable, TableAdapter yöntemleri içerecek **GetProducts()**,  **GetProductByCategoryID (*adlı kullanıcı, Categoryıd'si*) ** ve benzeri sunu katmanı çağıran. DataTable nesnesinin katmanlar arasında veri iletmek için kullanılan türü kesin belirlenmiş nesnelerin olarak hizmet verecek rolüdür.

TableAdapter Yapılandırma Sihirbazı'nı çalışmak için hangi veritabanı seçmenizi isteyerek başlar. Aşağı açılan liste Server Explorer'da bu veritabanlarını gösterir. Sunucu Gezgini Northwind veritabanı eklemediyseniz, bunu yapmak için şu anda yeni bağlantı düğmeyi tıklatabilirsiniz.


[![Northwind veritabanı aşağı açılan listeden seçin](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Şekil 5**: Northwind veritabanı aşağı açılan listeden seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image13.png))


Veritabanını seçtikten sonra İleri'yi tıklatmadan, bağlantı dizesinde kaydetmek isteyip istemediğiniz sorulur **Web.config** dosya. Bağlantı dizesi kaydederek, sabit bir bağlantı dizesi bilgisi gelecekte değişirse, şeyler basitleştirir TableAdapter sınıflarda kodlanmış sahip kaçının. Yapılandırma dosyasında bağlantı dizesini kaydetmeyi seçerseniz, yerleştirilir  **&lt;connectionStrings&gt;**  olabilir bölüm [isteğe bağlı olarak şifrelenmiş](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) geliştirilmiş için Güvenlik ya da yöneticiler için daha uygundur IIS GUI yönetim aracı içinde yeni ASP.NET 2.0 özellik sayfası aracılığıyla daha sonra değiştirilmiş.


[![Bağlantı dizesi Web.config dosyasına kaydedin](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Şekil 6**: bağlantı dizesini Kaydet **Web.config** ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image16.png))


Ardından, kesin türü belirtilmiş veri kümesini doldururken kullanmak bizim TableAdapter için ilk yöntem sunar ve ilk kesin türü belirtilmiş DataTable için şema tanımlamak ihtiyacımız. Bu iki adımı aynı anda bizim DataTable yansıtılan istiyoruz tablosundan sütunların döndüren bir sorgu oluşturarak yapılır. Sihirbazın sonunda Biz bu sorgu için bir yöntem adı verin. Bu yöntem, elde edilir sonra bizim sunu katmanı çağrılabilir. Yöntemi, tanımlı sorguyu yürütmek ve kesin türü belirtilmiş bir DataTable doldurabilirsiniz.

SQL sorgusu tanımlama başlamak için biz öncelikle TableAdapter sorgu sorunu nasıl istiyoruz belirtmeniz gerekir. Biz geçici SQL deyimini kullanın, yeni bir saklı yordam oluşturun veya varolan bir saklı yordamı kullanın. Geçici SQL deyimleri bu öğreticileri için kullanacağız. Başvurmak [Brian Noyes](http://briannoyes.net/)kullanıcının makalesi, [Visual Studio 2005 veri kümesi Tasarımcısı ile veri erişim katmanı yapı](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) saklı yordamları kullanma örneği için.


[![Geçici SQL deyimi kullanarak verileri Sorgulama](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Şekil 7**: geçici SQL deyimi kullanarak verileri Sorgulama ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image19.png))


Bu noktada SQL sorgusunda el ile yazabilirsiniz. İlk yöntem TableAdapter içinde oluştururken, genellikle karşılık gelen DataTable ifade edilmesi gerekir bu sütunları döndürüldüğü bir sorgu istiyorsanız. Biz tüm sütunları ve bulunan tüm satırlar döndüren bir sorgu oluşturarak gerçekleştirebilir **ürünleri** tablosu:


[![SQL sorgusu metin kutusuna girin](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Şekil 8**: SQL sorgusu içine metin kutusuna girin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image22.png))


Alternatif olarak, Sorgu Oluşturucu'yu kullanın ve grafik sorgu Şekil 9'da gösterildiği gibi oluşturun.


[![Sorgu grafik, sorgu Düzenleyicisi'ni kullanarak oluşturma](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Şekil 9**: Sorgu grafik, sorgu Düzenleyicisi aracılığıyla oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image25.png))


Sorguyu oluşturduktan sonra ancak sonraki ekrana geçmeden önce Gelişmiş Seçenekler düğmesini tıklatın. Web sitesi projelerinde "Generate INSERT, Update ve Delete deyimlerinde" yalnızca Gelişmiş seçeneği varsayılan olarak seçili olan; bir sınıf kitaplığı ya da Windows projesi bu sihirbazı çalıştırırsanız "iyimser eşzamanlılık kullan" seçeneğini da seçilir. Şu an için "iyimser eşzamanlılık kullan" seçeneğinin işaretini kaldırın. Sonraki öğreticilerde biz iyimser eşzamanlılık inceleyeceğiz.


[![Yalnızca Generate INSERT, Update ve Delete deyimleri seçeneği seçin](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Şekil 10**: yalnızca Generate INSERT, Update ve Delete deyimleri seçeneği seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image28.png))


Gelişmiş Seçenekleri doğruladıktan sonra son ekran devam etmek için İleri'yi tıklatın. Burada size TableAdapter eklemek için hangi yöntemlerin seçmeniz istenir. Veri doldurmak için iki modeli vardır:

- **DataTable dolgu** bir parametre olarak bir DataTable tablosundaki alır ve bunu doldurduğundan bir yöntem oluşturulur bu yaklaşımda sorgu sonuçlarına dayalı. ADO.NET DataAdapter sınıfı, örneğin, bu deseni uygular, **Fill()** yöntemi.
- **DataTable dönmek** bu yaklaşımı yöntemi oluşturur ve sizin için DataTable doldurur ve yöntemleri dönüş değeri olarak döndürür.

Birini veya her ikisini bu desenleri uygulamak TableAdapter olabilir. Burada sağlanan yöntemleri de yeniden adlandırabilirsiniz. Şimdi biz yalnızca bu öğreticileri boyunca ikinci düzeni kullanmaya başlayacağınız olsa bile iki onay kutusunun işaretli bırakın. Ayrıca, şirketinizdeki yerine genel yeniden adlandırma **GetData** yönteme **GetProducts**.

"GenerateDBDirectMethods," son onay kutusu işaretli değilse oluşturur **INSERT()**, **Update()**, ve **Delete()** TableAdapter için yöntemleri. Bu seçeneği işaretlemeden bırakın, tüm güncelleştirmeler TableAdapter'ın tek yapılması gerekir **Update()** yazılan veri kümesi, bir DataTable, tek bir DataRow satırının veya DataRow dizisini alır yöntemi. (Seçtiğiniz varsa denetlenmeyen "Generate INSERT, Update ve Delete deyimleri" Bu checkbox'ın gelişmiş özelliklerinden Şekil 9'daki seçeneği ayarı herhangi bir etkisi olacaktır.) Şimdi bu onay kutusu seçili bırakın.


[![GetData için yöntem adını GetProducts değiştirin](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Şekil 11**: yöntem adını değiştirmek **GetData** için **GetProducts** ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image31.png))


Son'u tıklatarak Sihirbazı tamamlayın. Sihirbaz kapandıktan sonra size yeni oluşturduğumuz DataTable gösteren veri kümesi Tasarımcısı döndürülür. Sütunlarda listesini görebilir **ürünleri** DataTable (**ProductID**, **ProductName**, vb.), yöntemlerinin yanı sıra  **Düzenleyen** (**Fill()** ve **GetProducts()**).


[![Yazılan veri kümesine düzenleyen ve ürünleri DataTable eklendi](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Şekil 12**: **ürünleri** DataTable ve **düzenleyen** yazılan veri kümesi için eklenene ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image34.png))


Bu noktada tek DataTable ile yazılmış bir veri kümesi sahibiz (**Northwind.Products**) ve kesin türü belirtilmiş bir DataAdapter sınıfı (**NorthwindTableAdapters.ProductsTableAdapter**) ile bir  **GetProducts()** yöntemi. Bu nesneler, kod gibi tüm ürünlerin listesini erişmek için kullanılabilir:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Bu kod bize bir bit veri erişim özgü kod yazmaya gerektirmiyordu. Biz herhangi ADO.NET sınıflarını örneği yok, herhangi bir bağlantı dizesi için SQL sorguları başvurmak olmadığına veya saklı yordamlar. Bunun yerine, TableAdapter alt düzey veri erişim kodu bize sağlar.

Bu örnekte kullanılan her de güçlü IntelliSense ve derleme zamanı tür denetimi sağlamak Visual Studio izin vererek yazılmış, nesnesidir. Ve TableAdapter tarafından döndürülen tüm DataTables en iyi GridView, DetailsView, DropDownList, CheckBoxList ve diğerleri birçok gibi Web denetimleri ASP.NET verilere bağlı olabilir. Aşağıdaki örnek tarafından döndürülen DataTable bağlama gösterilmektedir **GetProducts()** satırlardaki yalnızca bir scant üç kod içinde GridView yönteme **sayfa\_yük** olay işleyicisi.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Ürünleri listeler bir GridView görüntülenir](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Şekil 13**: ürünleri listeler, bir GridView görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image37.png))


Bu örnek şu üç kod satırı bizim ASP.NET sayfa yazma gerekirken **sayfa\_yük** olay işleyicisi, gelecekte inceleyeceğiz ObjectDataSource bildirimli olarak verileri almak için nasıl kullanılacağını öğreticileri DAL. ObjectDataSource ile herhangi bir kod yazmak zorunda kalırsınız değil ve disk belleği ve sıralama desteği de alırsınız!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>3. adım: Parametreli veri erişim katmanı yöntemlere ekleme

Bu noktada bizim **düzenleyen** sınıfına sahip bir yöntem **GetProducts()**, döndüğü tüm ürünleri veritabanında. Tüm ürünleri ile çalışmak için kesinlikle yararlı olsa da, biz belirli bir ürün veya belirli bir kategoriye ait tüm ürünleri hakkında bilgi almak için ne zaman isteyeceksiniz zamanlar vardır. Bu tür işlevler bizim veri erişim katmanı eklemek için şu parametreli yöntemleri TableAdapter ekleyebilirsiniz.

Ekleyelim  **GetProductsByCategoryID (*adlı kullanıcı, Categoryıd'si*) ** yöntemi. DAL, veri kümesi Tasarımcısı için dönüş yeni bir yöntem eklemek için sağ **düzenleyen** bölümünde ve Sorgu Ekle'ı seçin.


![TableAdapter üzerinde sağ tıklayın ve sorgu ekleme](creating-a-data-access-layer-cs/_static/image38.png)

**Şekil 14**: TableAdapter üzerinde sağ tıklayın ve sorgu ekleme


Biz öncelikle olup olmadığını biz geçici SQL deyimi ya da yeni veya varolan bir saklı yordamı kullanarak veritabanına erişmek istemediğiniz sorulur. Geçici SQL deyimi yeniden kullanmak için şimdi seçin. Ardından, ne tür bir SQL sorgusu biz kullanmak istediğiniz sorulur. Belirtilen bir kategoriye ait tüm ürünleri döndürülecek istiyoruz olduğundan, biz yazmak istediğiniz bir **seçin** satırlar döndüren bir ifade.


[![Satırlar döndüren bir SELECT deyimi oluşturmak için seçin](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Şekil 15**: Create seçin bir **seçin** deyimi döndürür satırlarını ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image41.png))


Sonraki adım, verilere erişmek için kullanılan SQL sorgusu tanımlamaktır. Belirli bir kategoriye ait ürünleri döndürülecek istiyoruz olduğundan, aynı kullanmam **seçin** from deyimi **GetProducts()**, ancak aşağıdaki ekleyin **burada** yan tümcesi: **burada adlı kullanıcı, Categoryıd'si = @CategoryID** .  **@CategoryID**  Parametresi TableAdapter sihirbazın oluşturma yöntemi (yani, boş değer atanabilir bir tamsayı) karşılık gelen türünde bir giriş parametresi gerektiğini gösterir.


[![Yalnızca belirtilen bir kategorideki ürünleri döndürmek için bir sorgu girin](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Şekil 16**: yalnızca dönüş ürünlere belirtilen bir kategorideki bir sorgu girin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image44.png))


Hangi veri yanı sıra kullanmak için oluşturulan yöntemler adlarını özelleştirin desenleri erişim seçeneğini belirledik son adımı. Dolgu deseni için adına değiştirelim **FillByCategoryID** ve dönüş için bir DataTable dönmek deseni (  **almak*X*** yöntemleri), kullanalım  **GetProductsByCategoryID**.


[![TableAdapter yöntemleri için adlar seçin](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Şekil 17**: TableAdapter yöntemleri için adlar seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image47.png))


Sihirbazı tamamladıktan sonra veri kümesi Tasarımcısı yeni TableAdapter yöntemler içerir.


![Kategoriye göre ürünler olabilir şimdi sorgulanamadı](creating-a-data-access-layer-cs/_static/image48.png)

**Şekil 18**: ürünleri olabilir şimdi izin ver sorgulanan kategoriye göre


Eklemek için bir dakikanızı ayırın bir  **GetProductByProductID (*ProductID*) ** teknikle yöntemi.

Bu parametreli sorgular doğrudan veri kümesi Tasarımcısı'ndan sınanabilir. TableAdapter yönteminde sağ tıklayın ve önizleme verileri seçin. Ardından, Önizleme'yi tıklatın ve parametrelerini kullanmak için değerleri girin.


[![Bu ürünler ait İçecekler kategorisini gösterilir](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Şekil 19**: olanlar İçecekler kategorisini ürünleri ait gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image51.png))


İle  **GetProductsByCategoryID (*adlı kullanıcı, Categoryıd'si*) ** bizim DAL yönteminde, biz şimdi oluşturabilir, belirtilen bir kategorideki yalnızca ürünleri görüntüler ASP.NET sayfası. Aşağıdaki örnek olan, Meşrubat kategorideki tüm ürünleri gösterir bir **adlı kullanıcı, Categoryıd'si** 1.

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Bu ürünlerden Meşrubat kategorisinde görüntülenir](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Şekil 20**: olanlar İçecekler kategorisini ürünlerinde görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>4. adım: Ekleme, güncelleştirme ve verileri silme

Ekleme, güncelleştirme ve verileri silme için yaygın olarak kullanılan iki modeli vardır. Veritabanını doğrudan düzeni çağrısı, ilk desen yöntemleri, çağrıldığında, oluşturursunuz sorunu bir **Ekle**, **güncelleştirme**, veya **silmek** komutu tek veritabanı kaydını üzerinde çalışır veritabanı. Bu tür yöntemleri genellikle karşılık skaler değerler (tamsayı, dize, Boole değerlerini, tarih/saat ve benzeri) bir dizi eklemek, güncelleştirmek veya silmek için değerleri geçirildi. Örneğin, için bu deseni ile **ürünleri** tablo delete yöntemini bir tamsayı parametresinde olması belirten **ProductID** INSERT yöntemi götürecek sırada silinecek kaydının bir dize **ProductName**, için ondalık **UnitPrice**, tamsayı **UnitsOnStock**ve benzeri.


[![Her bir INSERT, Update ve Delete isteği veritabanı anında gönderilir](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Şekil 21**: her INSERT, Update ve Delete isteği gönderilir veritabanı anında ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image57.png))


Bir tüm veri kümesi, DataTable ya da bir yöntem çağrısı DataRow koleksiyonunda düzeni toplu güncelleştirme bakın, diğer bütün düzeni güncelleştirmektir. Bu desen ile bir geliştirici siler, ekler ve bir DataTable tablosundaki DataRow değiştirir ve sonra bu DataRow ya da DataTable bir güncelleştirme yöntemi geçirir. Bu yöntem ardından geçirilen DataRow numaralandırır, bunlar değiştiren, eklenen veya silinen olup olmadığını belirler (DataRow nesnesinin aracılığıyla [RowState özelliği](https://msdn.microsoft.com/en-us/library/system.data.datarow.rowstate.aspx) değeri) ve her kayıt için uygun veritabanı isteği gönderir.


[![Update yöntemi çağrıldığında tüm değişiklikler veritabanı ile eşitlenir](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Şekil 22**: tüm değişiklikler, veritabanı ile eşitlenir, güncelleştirme yöntemi çağrıldığında ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter varsayılan olarak toplu güncelleştirme deseni kullanır, ancak ayrıca DB doğrudan düzenini destekler. Biz "Generate INSERT, Update ve Delete deyimlerinde" Gelişmiş özelliklerinden bizim TableAdapter oluştururken seçeneği bu yana **düzenleyen** içeren bir **Update()** yöntemi hangi toplu güncelleştirme deseni uygular. Özellikle, TableAdapter içeren bir **Update()** yazılan veri kümesi, kesin türü belirtilmiş bir DataTable ya da bir veya daha fazla DataRow geçirilen yöntemi. Bırakılırsa ne zaman ilk DB doğrudan deseni TableAdapter oluşturma da aracılığıyla uygulanacak "GenerateDBDirectMethods" onay kutusunu işaretli **INSERT()**, **Update()**, ve **Delete()**  yöntemleri.

TableAdapter'ın her iki veri değişikliği modelleri kullanmanıza **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** vermek için özellikler kendi **Ekle** , **Güncelleştirme**, ve **silmek** veritabanına komutları. İncelemek ve değiştirmek **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** veri kümesi Tasarımcısı'nda TableAdapter tıklayarak ve ardından giderek özellikleri Özellikler penceresi için. (TableAdapter ve seçili olduğundan emin olun **düzenleyen** Özellikleri penceresinde açılır listesinden seçilen bir nesnedir.)


[![TableAdapter InsertCommand, UpdateCommand ve DeleteCommand özellikleri vardır](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Şekil 23**: TableAdapter sahip **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** özellikleri ([görüntülemek için tıklatın tam boyutlu görüntüyü](creating-a-data-access-layer-cs/_static/image63.png))


İnceleyin veya bu veritabanı komut özelliklerden herhangi birini değiştirmek için tıklayın **CommandText** Sorgu Oluşturucu Kurulumu getirecek alt.


[![INSERT, UPDATE ve DELETE deyimleri Sorgu Oluşturucu'da yapılandırma](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Şekil 24**: yapılandırmak **Ekle**, **güncelleştirme**, ve **silmek** Sorgu Oluşturucusu deyimlerinde ([tam boyutlu görüntüyü görüntülemek için tıklatın ](creating-a-data-access-layer-cs/_static/image66.png))


Aşağıdaki kod örneğinde toplu güncelleştirme düzeni değil üretilmeyen ve stok veya daha az 25 birimleri olan tüm ürünleri fiyat çift için nasıl kullanılacağını gösterir:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Aşağıdaki kod, program aracılığıyla belirli bir ürünü silin, sonra bir güncelleştirme için DB doğrudan düzeni kullanın ve yeni bir tane eklemek verilmektedir:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Oluşturma özel INSERT, Update ve Delete yöntemleri

**INSERT()**, **Update()**, ve **Delete()** DB doğrudan yöntemi tarafından oluşturulan yöntemler özellikle fazla sayıda sütun sahip tablolar için biraz sıkıcı olabilir. Önceki kod örneğinde, ne özellikle NET değil IntelliSense'nın Yardım olmadan arayan **ürünleri** her giriş parametresi olarak tablo sütunu eşlendiğini **Update()** ve **INSERT()**  yöntemleri. Biz yalnızca istediğinizde tek bir sütun veya iki güncelleştirmek veya özelleştirilmiş istediğiniz zamanlar olabilir **INSERT()** olur, belki de yöntemi dönüş yeni eklenen kaydın değeri **kimlik** (otomatik artım) alan.

Özel bir yöntem oluşturmak için veri kümesi Tasarımcısı döndür. TableAdapter üzerinde sağ tıklayın ve TableAdapter sihirbazın döndüren sorgu Ekle'ı seçin. İkinci ekranda biz oluşturmak için sorgu türünü belirtebilirsiniz. Yeni bir ürün ekler ve yeni eklenen kaydın değerini döndüren bir yöntem oluşturalım **ProductID**. Bu nedenle, oluşturmak için kabul bir **Ekle** sorgu.


[![Ürünler tablosuna yeni bir satır eklemek için bir yöntem oluşturma](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Şekil 25**: yeni satır eklemek için bir yöntem oluşturma **ürünleri** tablosu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image69.png))


Sonraki ekranda **InsertCommand**'s **CommandText** görüntülenir. Bu sorgu ekleyerek büyütmek **kapsam seçin\_IDENTITY()** sorgu sonunda, eklenen son kimlik değeri döndürecektir bir **kimlik** aynı kapsamda sütun. (Bkz [teknik belgeler](https://msdn.microsoft.com/en-us/library/ms190315.aspx) hakkında daha fazla bilgi için **kapsam\_IDENTITY()** ve büyük olasılıkla istediğiniz neden [kapsamı kullan\_yerine IDENTITY() @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Bitirdiğinizden emin olun **Ekle** eklemeden önce deyimi noktalı virgül ile **seçin** deyimi.


[![SCOPE_IDENTITY() değerini döndürmek için sorgu büyütmek](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Şekil 26**: Return sorguya büyütmek **kapsam\_IDENTITY()** değeri ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image72.png))


Son olarak, yeni yöntem adı **InsertProduct**.


[![Yeni yöntem adı InsertProduct için ayarlama](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Şekil 27**: yeni yöntem adı ayarlamak **InsertProduct** ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image75.png))


Göreceksiniz veri kümesi Tasarımcısı döndüğünüzde **düzenleyen** yeni bir yöntem içerir **InsertProduct**. Bu yeni yöntemi her sütun için bir parametre yoksa **ürünleri** tablo olasılığı olan unuttunuz sonlandırmak **Ekle** deyimi noktalı virgül ile. Yapılandırma **InsertProduct** yöntemi ve bir noktalı virgül sınırlandırma olduğundan emin olun **Ekle** ve **seçin** deyimleri.

Varsayılan olarak, etkilenen satırların sayısı döndürmeleri anlamına gelir, yöntemleri sorunu sorgu olmayan yöntemleri ekleyin. Ancak, istiyoruz **InsertProduct** etkilenen satırların sayısını değil sorgu tarafından döndürülen değer döndürmek için yöntem. Bunu gerçekleştirmek için ayarlamak **InsertProduct** yöntemin **ExecuteMode** özelliğine **skaler**.


[![Skaler için ExecuteMode özelliğini değiştirin](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Şekil 28**: değişiklik **ExecuteMode** özelliğine **skaler** ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image78.png))


Aşağıdaki kodda gösterildiği bu yeni **InsertProduct** eylem yönteminde:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>5. adım: veri erişim katmanı Tamamlanıyor

Unutmayın **ProductsTableAdapters** sınıf döndürür **adlı kullanıcı, Categoryıd'si** ve **SupplierID** gelen değerleri **ürünleri** tablo, ancak içermeyen **CategoryName** sütundan **kategorileri** tablo veya **ŞirketAdı** sütundan **Üreticiler**bu büyük olasılıkla istiyoruz ürün bilgileri gösterilirken görüntülenecek sütunları olsa da, tablo. TableAdapter'ın ilk yöntemi büyütmek **GetProducts()**, her ikisi de dahil etmek için **CategoryName** ve **ŞirketAdı** güncelleştirecek sütun değerleri Bu da yeni sütunlar eklenecek DataTable kesin türü belirtilmiş.

Bu bir sorun ancak olarak TableAdapter'ın yöntemleri ekleme, güncelleştirme, oluşturabilir ve silme veri dışına ilk bu yöntem temel alır. Ekleme, güncelleştirme ve silme için Neyse ki, otomatik olarak oluşturulan yöntemleri alt sorgularda etkilenen **seçin** yan tümcesi. Bizim sorguları ekleme dikkat ederek tarafından **kategorileri** ve **Üreticiler** alt sorgular, olarak yerine **katılma** s, ki kaçının verileri değiştirmek için bu yöntemleri rework zorunda. Sağ **GetProducts()** yönteminde **düzenleyen** ve Yapılandır'ı seçin. Daha sonra ayarlamak **seçin** yan tümcesi görünüyor. böylece ister:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![SELECT deyimi güncelleştirmesi GetProducts() yöntemi](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Şekil 29**: güncelleştirme **seçin** bildirimi **GetProducts()** yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image81.png))


Güncelleştirdikten sonra **GetProducts()** DataTable, iki yeni sütun içereceği bu yeni bir sorgu kullanılacak yöntemi: **CategoryName** ve **üretici**.


![Ürünler DataTable iki yeni sütun var.](creating-a-data-access-layer-cs/_static/image82.png)

**Şekil 30**: **ürünleri** DataTable iki yeni sütun var.


Güncelleştirme için bir dakikanızı ayırın **seçin** yan tümcesinde  **GetProductsByCategoryID (*adlı kullanıcı, Categoryıd'si*) ** de yöntemi.

Güncelleştirmezseniz **GetProducts()** **seçin** kullanarak **katılma** sözdizimi veri kümesi Tasarımcısı edemez ekleme, güncelleştirme ve silme yöntemleri otomatik olarak oluşturmak DB kullanılarak veritabanı veri düzeni doğrudan. Bunun yerine, ile yaptığımız gibi el ile çok oluşturmanız gerekecek **InsertProduct** bu öğreticideki yöntemi. Ayrıca, el ile sağlamak zorunda kalırsınız **InsertCommand**, **UpdateCommand**, ve **DeleteCommand** özellik değerlerinin düzeni güncelleştirme toplu kullanmak istiyorsanız.

## <a name="adding-the-remaining-tableadapters"></a>Kalan TableAdapters ekleme

Şimdiye kadar biz yalnızca bir tek veritabanı tablosu için tek bir TableAdapter ile çalışma sırasında inceledik. Ancak, Northwind veritabanı çalışmak için web uygulamamızı ihtiyacımız birkaç ilişkili tabloları içerir. DataTables ilgili yazılmış bir veri kümesi birden çok içerebilir. Bu nedenle, bizim DAL tamamlamak için size DataTables Biz bu öğreticileri kullanmaya başlayacağınız diğer tablolar için eklemeniz gerekir. Yeni bir TableAdapter yazılmış bir veri kümesine eklemek için veri kümesi Tasarımcısı'nı açın, Tasarımcısı'nda sağ tıklayın ve Ekle'yi seçin / TableAdapter. Bu, yeni bir DataTable ve TableAdapter oluşturma ve biz Bu öğreticide daha önce incelenmesi Sihirbazı size yol.

Aşağıdaki TableAdapters ve aşağıdaki sorguları kullanarak yöntemleri oluşturmak için birkaç dakika sürebilir. Unutmayın sorgularda **düzenleyen** her ürünün kategori ve sağlayıcı adları şablonlarınızdan alt sorgular içerir. Ayrıca, aşağıdaki, zaten eklediğiniz **düzenleyen** sınıfının **GetProducts()** ve  **GetProductsByCategoryID (*adlı kullanıcı, Categoryıd'si*) ** yöntemleri.

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


[![Dört TableAdapters ekledikten sonra veri kümesi Tasarımcısı](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Şekil 31**: veri kümesi Tasarımcısı sonra dört TableAdapters eklenmiştir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>DAL için özel kod ekleme

DataTables yazılan veri kümesine eklenir ve TableAdapters bir XML şema tanımı dosyası olarak ifade edilir (**Northwind.xsd**). Üzerinde sağ tıklayarak bu şema bilgileri görüntüleyebilirsiniz **Northwind.xsd** dosya Çözüm Gezgini'nde ve Görünüm Kodu seçme.


[![Veri kümesi kategoriye için XML şema tanımı (XSD) dosyası belirtilmiş](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Şekil 32**: XML şema tanımı (XSD) dosya kategoriye yazılan veri kümesi için ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image88.png))


Bu şema bilgileri derlendiğinde veya bu noktada, üzerinden hata ayıklayıcısını kullanmaya geçebilirsiniz (gerekirse) çalışma zamanında çalışma zamanında C# veya Visual Basic kodu çevrilir. Görüntülemek için bu otomatik olarak oluşturulan kodu gidin sınıf görünümü ve ayrıntıya TableAdapter ya da yazılı DataSet sınıfları. Sınıf Görünümü ekranınızdaki görmüyorsanız, Görünüm menüsüne gidin ve buradan seçin veya Ctrl + Shift + C ulaştı. Sınıf görünümünden özellikleri, yöntemleri ve yazılan veri kümesi ve TableAdapter sınıflarının olayları görebilirsiniz. Belirli bir yöntem için kod görüntülemek üzere Sınıf Görünümü'nde yöntemi adına çift tıklayın veya üzerinde sağ tıklayın ve Tanıma Git seçin.


![Git tanımına sınıfı görünümünden seçerek otomatik olarak oluşturulan kodu inceleyin.](creating-a-data-access-layer-cs/_static/image89.png)

**Şekil 33**: Git tanımına sınıfı görünümünden seçerek otomatik olarak oluşturulan kodu inceleyin.


Otomatik olarak oluşturulan kodu harika zaman tasarrufu olabilirler, ancak kod genellikle çok geneldir ve bir uygulamanın benzersiz gereksinimlerini karşılamak için özelleştirilmiş olması gerekir. Otomatik olarak oluşturulan kodu genişletme rağmen oluşturulan kodda aracı "yeniden" ve özelleştirmelerinizi üzerine yazmak için zamanı göstermeyebilir riskidir. .NET 2. 0'ın yeni parçalı sınıf konseptiyle, bir sınıfın birden çok dosyaya bölme kolaydır. Bu bizim özelleştirmeleri üzerine Visual Studio hakkında endişelenmeye gerek kalmadan otomatik olarak oluşturulan sınıfları kendi yöntemleri, özellikleri ve olayları eklemek bize sağlar.

DAL özelleştirmek nasıl yapılacağını göstermek üzere ekleyelim bir **GetProducts()** yönteme **SuppliersRow** sınıfı. **SuppliersRow** sınıfı temsil eder, tek bir kayıtta **Üreticiler** tablo; birçok ürün her tedarikçi can sağlayıcısına sıfır şekilde **GetProducts()** olanlar döndürür Belirtilen sağlayıcı ürünler. Gerçekleştirmek için yeni bir sınıf dosyasında oluşturma **uygulama\_kod** adlı klasörü **SuppliersRow.cs** ve aşağıdaki kodu ekleyin:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Bu parçalı sınıf derleyiciye olduğunda oluşturma **Northwind.SuppliersRow** eklemek için sınıfı **GetProducts()** biz yalnızca tanımlanmış yöntemi. Projenizi derleme ve sınıf görünümüne dönmek göreceğiniz **GetProducts()** bir yöntemi olarak listelenmiş **Northwind.SuppliersRow**.


![Şimdi bir Northwind.SuppliersRow sınıfının GetProducts() yöntemi parçasıdır](creating-a-data-access-layer-cs/_static/image90.png)

**Şekil 34**: **GetProducts()** yöntemdir şimdi parçası **Northwind.SuppliersRow** sınıfı


**GetProducts()** yöntemi şimdi ürün kümesi belirli bir üretici için aşağıdaki kodu gösterildiği gibi numaralandırmak için kullanılabilir:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Bu veriler ayrıca ASP hiçbirinde görüntülenebilir. NET'in veri Web denetler. Şu sayfayı iki alan GridView denetimini kullanır:

- Her sağlayıcı adını görüntüler BoundField ve
- Tarafından döndürülen sonuçlar bağlı Bulletedlıst denetimi içeren bir TemplateField **GetProducts()** her sağlayıcı için yöntem.

Biz sonraki öğreticilerde gibi ana-ayrıntı raporları görüntülemek nasıl inceleyeceğiz. Şu an için bu örnek için eklenen özel yöntemini kullanarak göstermek için tasarlanmıştır **Northwind.SuppliersRow** sınıfı.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Sol sütunda sağ Their ürünleri tedarikçi şirket adı listelenir](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Şekil 35**: tedarikçi şirket adı Their ürünleri sağ sol sütununda listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Özet

Ne zaman DAL oluşturma bir web uygulaması oluşturma, sunu katmanı oluşturmaya başlamadan önce gerçekleşen ilk adımlarınız biri olmalıdır. Visual Studio ile yazılan veri kümelerinde dayalı bir DAL oluşturma 10-15 dakika içinde bir satır kod yazmadan yapılabilecek bir görevdir. İlerleyen öğreticileri bu DAL oluşturacaksınız. İçinde [sonraki öğretici](creating-a-business-logic-layer-cs.md) biz iş kuralları sayısını tanımlamak ve içinde ayrı bir iş mantığı katmanı uygulamak bkz.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Kesin türü belirtilmiş TableAdapters ve DataTables VS 2005 ve ASP.NET 2.0 kullanarak bir DAL oluşturma](https://weblogs.asp.net/scottgu/435498)
- [Veri katmanı bileşenleri tasarlama ve katmanları arasında veri geçirme](https://msdn.microsoft.com/en-us/library/ms978496.aspx)
- [Visual Studio 2005 veri kümesi Tasarımcısı ile veri erişim katmanı oluşturma](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [ASP.NET 2.0 yapılandırma bilgilerini şifrelemek uygulamaları](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter genel bakış](https://msdn.microsoft.com/en-us/library/bz9tthwx.aspx)
- [Türü belirtilmiş bir veri kümesi ile çalışma](https://msdn.microsoft.com/en-us/library/esbykkzb.aspx)
- [Visual Studio 2005 ve ASP.NET 2.0 kesin türü belirtilmiş veri erişimi kullanma](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [TableAdapter yöntemleri genişletme](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Bir saklı yordam skaler verilerini alma](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Bu öğreticide yer alan konularda video eğitim

- [ASP.NET uygulamalarında veri erişim katmanları](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Nasıl el ile bir veri kümesi Datagrid denetimine bağlama](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Bir ASP uygulamasından nasıl veri kümelerini ve filtreleri ile çalışma](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Cüneyt yeşil, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez ve Carlos Santos yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Sonraki](creating-a-business-logic-layer-cs.md)
