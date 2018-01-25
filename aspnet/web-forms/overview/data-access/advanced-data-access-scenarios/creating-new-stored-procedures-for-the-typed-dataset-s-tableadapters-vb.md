---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: "Türü belirtilmiş veri kümesi'nin TableAdapters (VB) için saklı yordamlar yeni oluşturma | Microsoft Docs"
author: rick-anderson
description: "Önceki eğitimlerine biz bizim kodda SQL deyimlerini oluşturduktan ve deyimleri yürütülecek veritabanına aktarılabilir. Alternatif bir yaklaşım s kullanmaktır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: b2262df1a56ffa88a22d9dc8000bd0c300fea72e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Türü belirtilmiş veri kümesi'nin TableAdapters (VB) için saklı yordamlar yeni oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) veya [PDF indirin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> Önceki eğitimlerine biz bizim kodda SQL deyimlerini oluşturduktan ve deyimleri yürütülecek veritabanına aktarılabilir. Alternatif bir yaklaşım saklı yordamlar SQL deyimlerini veritabanını önceden tanımlanmış nerede kullanmaktır. Bu öğreticide TableAdapter bize yeni saklı yordamları Oluştur Sihirbazı'nı nasıl öğrenin.


## <a name="introduction"></a>Giriş

Yazılan veri kümeleri bu öğreticileri veri erişim düzeyi (DAL) kullanır. ' Da anlatıldığı gibi [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md) öğretici, yazılı veri kümeleri oluşur kesin türü belirtilmiş DataTables ve TableAdapters. DataTables mantıksal varlıklar veri erişim işlemleri gerçekleştirmesi için temel alınan veritabanı TableAdapters arabirimiyle çalışırken sistem temsil eder. Bu, DataTables verilerle doldurma, skaler veri döndüren sorgular yürütme ve ekleme, güncelleştirme ve veritabanından kayıtları silme içerir.

TableAdapters tarafından yürütülen SQL komutlarını ya da geçici SQL deyimlerini da olabilen `SELECT columnList FROM TableName`, veya saklı yordamlar. TableAdapters bizim mimarisinde geçici SQL deyimlerini kullanın. Ancak, birçok geliştiricilerinin ve veritabanı yöneticilerinin, saklı yordamları üzerinden güvenlik, Bakım ve güncellenebilirliğini nedeniyle geçici SQL deyimlerini tercih. Başkalarının ardently geçici SQL deyimleri kendi esnekliğinde için tercih edilir. Kendi iş ı saklı yordamları geçici SQL deyimlerini favor, ancak önceki öğreticileri basitleştirmek için geçici SQL deyimlerini kullanmayı seçtiniz.

Ne zaman bir TableAdapter tanımlama veya yeni yöntemler ekleme, TableAdapter s Sihirbazı'nı yalnızca olarak yeni saklı yordamlar oluşturmak veya var olan saklı yordamları geçici SQL deyimleri kullanmak için yaptığı gibi kullanmak kolaylaştırır. Bu öğreticide biz saklı yordamları otomatik oluşturma TableAdapter s Sihirbazı nasıl inceleyeceğiz. Sonraki öğreticide biz mevcut veya el ile oluşturulan saklı yordamları kullanmak için TableAdapter s yöntemlerini yapılandırmak nasıl ele alacağız.

> [!NOTE]
> Ramiz Howard'ın blog girişine bakın [tan kullanım saklı yordamları henüz t?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) ve [Frans Bouma](https://weblogs.asp.net/fbouma/) s blog girdisi [saklı yordamlar olan bozuk, M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) için Canlı bir tartışma konusu avantajları ve dezavantajları hakkında saklı yordamlar ve geçici SQL.


## <a name="stored-procedure-basics"></a>Saklı yordam temelleri

Tüm programlama dili için ortak bir yapı işlevlerdir. Bir işlev işlevi çağrıldığında yürütülen deyimlerin bir koleksiyonudur. İşlevler giriş parametreleri kabul edebilir ve isteğe bağlı olarak bir değer döndürebilir. *[Saklı yordamlar](http://en.wikipedia.org/wiki/Stored_procedure)*  programlama dillerindeki işlevler ile benzer şekilde paylaşma veritabanı yapılar. Saklı yordam saklı yordam çağrıldığında, yürütülen T-SQL deyimlerini kümesinden oluşur. Saklı yordam birçok giriş parametreleri için sıfır kabul edebilir ve skaler değerler, çıktı parametreleri döndürebilir veya, sonuç en yaygın olarak, ayarlar `SELECT` sorgular.

> [!NOTE]
> Saklı yordamlar görmemeleri sprocs veya Sp'ler adlandırılır.


Saklı yordamlar kullanılarak oluşturulan [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL ifadesi. Örneğin, aşağıdaki T-SQL komut dosyası adlı bir saklı yordam oluşturur `GetProductsByCategoryID` adlı tek bir parametre alan `@CategoryID` ve döndürür `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` bu sütunların alanları `Products` eşleşen bir tablo `CategoryID` değeri:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Bu saklı yordam oluşturulduktan sonra aşağıdaki sözdizimi kullanılarak çağrılabilir:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> Sonraki öğreticide Visual Studio IDE aracılığıyla saklı yordamlar oluşturulmasına inceleyeceğiz. Bu öğreticide, ancak biz saklı yordamları bize için otomatik olarak üret TableAdapter sihirbazın izin çağıracaksınız.


Yalnızca veri döndüren ek olarak, saklı yordamlar, tek bir işlem kapsamı içinde birden çok veritabanı komutları gerçekleştirmek için genellikle kullanılır. Adlı bir saklı yordam `DeleteCategory`, örneğin, sürebilir bir `@CategoryID` parametre ve iki gerçekleştirmek `DELETE` deyimleri: ilgili ürünler ve belirtilen kategori silindiğinde ikinci bir silmek için önce bir. Saklı yordam içinde birden fazla deyim olduğundan *değil* otomatik olarak kaydırılan bir işlem içinde. Ek T-SQL komutlarıyla saklı yordam birden çok komut atomik bir işlem olarak davranılır s sağlamak için verilmesi gerekir. Sonraki öğreticide bir işlem kapsamı içinde saklı yordamı s komutları sarmalama göreceğiz.

Saklı yordamlar içinde bir mimari kullanırken, bir geçici SQL deyimini yürütmeden yerine belirli bir saklı yordam veri erişim katmanı s yöntemleri çağırır. Bu, (veritabanı) yürütülen SQL deyimlerini konumunu, uygulama s mimarisi içinde tanımlanan sahip olmak yerine merkezde toplar. Bu merkezileşmeyi tartışmaya açık bir şekilde bulmak, çözümlemek ve sorguları ayarlama kolaylaştırır ve nerede ve nasıl veritabanı kullanılıyor konusunda daha net bir resim sağlanmıştır.

Saklı yordam temelleri hakkında daha fazla bilgi için bu öğreticinin sonunda daha fazla bilgi bölümü kaynaklarında başvurun.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>1. adım: Gelişmiş Veri erişim katmanı senaryoları Web sayfaları oluşturma

Biz saklı yordamları kullanarak bir DAL oluşturma bizim tartışma başlamadan önce öncelikle bu ve sonraki birkaç öğreticileri için ihtiyacımız Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `AdvancedDAL`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Gelişmiş Veri erişim katmanı senaryoları öğreticileri için ASP.NET sayfaları ekleme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Şekil 1**: Gelişmiş Veri erişim katmanı senaryoları öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `AdvancedDAL` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Son olarak, bu sayfaları girişlere eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme ekledikten sonra toplu hale verileriyle çalışmaya ekleyin `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık Gelişmiş DAL senaryoları öğreticileri için öğeleri içerir.


![Site Haritasını girişleri Gelişmiş DAL senaryoları öğreticileri için şimdi içerir.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Şekil 3**: Site Haritası girişleri Gelişmiş DAL senaryoları öğreticileri için şimdi içerir.


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>2. adım: Yeni bir TableAdapter yapılandırma saklı yordamlar

Let s saklı yordamları yerine geçici SQL deyimlerini kullanan bir veri erişim katmanı oluşturma göstermek için yazılan yeni bir veri kümesinde oluşturma `~/App_Code/DAL` adlı klasörü `NorthwindWithSprocs.xsd`. Biz bu işlem ayrıntılı aracılığıyla önceki eğitimlerine adım adım olduğundan, biz hızla burada adımlarla devam edecek. Geri başvurmak takılı veya daha fazla adım adım yönergeler oluşturma ve yazılmış bir veri kümesi yapılandırma gerekirse, [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-vb.md) Öğreticisi.

Yeni bir veri kümesi üzerinde sağ tıklayarak projeye ekleyin `DAL` Yeni Öğe Ekle seçme ve Şekil 4'te gösterildiği gibi veri kümesi şablonu seçilmesi klasör.


[![NorthwindWithSprocs.xsd adlı projeye yeni türü belirtilmiş veri kümesi Ekle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Şekil 4**: adlı projesi için yeni bir yazılan veri kümesi ekleyin `NorthwindWithSprocs.xsd` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Bu yeni yazılan veri kümesi oluşturma, kendi Tasarımcısı'nı açın, yeni bir TableAdapter oluşturma ve TableAdapter Yapılandırma Sihirbazı'nı başlatın. TableAdapter Yapılandırma Sihirbazı'nı s ilk adımı bize çalışmak istediğiniz veritabanını seçin sorar. Aşağı açılan listesinde Northwind veritabanına bağlantı dizesi listelenmelidir. Bunu seçin ve İleri'yi tıklatın.

Bu sonraki ekranından nasıl TableAdapter veritabanına erişmek için kullanması seçebilirsiniz. Önceki eğitimlerine seçtik ilk seçenek, Use SQL deyimleri. Bu öğretici için ikinci seçeneği seçin, yeni saklı yordamlar oluşturmak ve İleri'yi tıklatın.


[![Yeni saklı yordamlar oluşturmak için TableAdpater isteyin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Şekil 5**: Yeni saklı yordamlar oluşturmak için TableAdpater isteyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Yalnızca geçici SQL deyimlerini kullanarak, aşağıdaki adımda biz istendiği gibi `SELECT` TableAdapter s ana sorgu bildirimi. Ancak kullanmak yerine `SELECT` deyimi, doğrudan bir geçici sorguyu gerçekleştirmek için buraya girilen TableAdapter s Sihirbazı'nı bu içeren bir saklı yordam oluşturacak `SELECT` sorgu.

Aşağıdaki `SELECT` bu TableAdapter sorgu:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![SEÇME sorgusu girin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Şekil 6**: girin `SELECT` sorgu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> Yukarıdaki sorgu biraz farklıdır ana sorgudan `ProductsTableAdapter` içinde `Northwind` yazılan veri kümesi. Sözcüğünün `ProductsTableAdapter` içinde `Northwind` yazılan veri kümesi kategori adı ve her ürün s kategorisi ve tedarikçi şirket adını geri getirmek için iki bağıntılı alt sorgular içerir. Yaklaşan içinde [TableAdapter kullanım birleşimler için güncelleştirme](updating-the-tableadapter-to-use-joins-vb.md) Biz bu ekleme konumunda görünür öğretici ilgili verileri bu TableAdapter.


Gelişmiş Seçenekler düğmesini için bir dakikanızı ayırın. Buradan Sihirbazı'nı da INSERT, update ve delete deyimleri TableAdapter, iyimser eşzamanlılık kullanılıp kullanılmayacağını ve veri tablosu ekler ve güncelleştirmeleri sonra olup yenilenmesi gereken oluşturup oluşturmayacağını belirtebilirsiniz. Generate INSERT, Update ve Delete deyimleri seçeneği varsayılan olarak işaretli. İşaretli bırakın. Bu öğretici için kullanım iyimser eşzamanlılık seçenekleri işaretlemeden bırakın.

TableAdapter Sihirbazı tarafından otomatik olarak oluşturulan saklı yordamlar içerdiğinde, veri tablosu seçeneği yenileme yoksayılır görüntülenir. Adım 3'te göreceğiz olarak bu onay kutusunun işaretli olup, sonuçta elde edilen ekleme ve güncelleştirme bağımsız olarak saklı yordamlar yalnızca eklenen veya yalnızca güncelleştirilen kaydı alın.


![Generate INSERT, Update ve Delete deyimleri seçeneğini işaretli bırakın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Şekil 7**: Generate INSERT, Update ve Delete deyimleri seçeneğini işaretli bırakın


> [!NOTE]
> Kullanım iyimser eşzamanlılık seçenek işaretlenirse, sihirbaz ek koşullar ekleyecektir `WHERE` diğer alanlara bir değişiklik olursa güncelleştirilen veri önlemek yan tümcesi. Geri başvurmak [uygulama iyimser eşzamanlılık](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) TableAdapter s yerleşik iyimser eşzamanlılık denetim özelliğini kullanma hakkında daha fazla bilgi için Öğreticisi.


Girdikten sonra `SELECT` sorgulamak ve Generate INSERT, Update ve Delete deyimleri seçeneği işaretlidir onaylama, İleri'yi tıklatın. Şekil 8'de gösterilen bu sonraki ekranda, sihirbaz oluşturacak saklı yordamlar adlarını seçme, ekleme, güncelleştirme ve verileri silme ister. Bu saklı yordamları adlarına değişiklik `Products_Select`, `Products_Insert`, `Products_Update`, ve `Products_Delete`.


[![Saklı yordamlar yeniden adlandırma](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Şekil 8**: saklı yordamlar yeniden adlandırın ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


TableAdapter Sihirbazı'nı dört saklı yordamlar oluşturmak için kullanacağı T-SQL görmek için Önizleme SQL betiği düğmesini tıklatın. Önizleme SQL betiği iletişim kutusundan komut dosyasını bir dosyaya kaydedin veya panoya kopyalayın.


![Saklı yordamlar oluşturmak için kullanılan SQL komut dosyası Önizleme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Şekil 9**: SQL komut dosyası saklı yordamları üretmek için kullanılan Önizleme


Saklı yordamlar adlandırdıktan sonra TableAdapter s adına karşılık gelen yöntemleri yanındaki'ı tıklatın. Geçici SQL deyimlerini kullanırken gibi varolan bir DataTable doldurun veya yeni bir tane döndüren yöntemler oluşturabilirsiniz. Biz TableAdapter ekleme, güncelleştirme ve silme kayıtlarını DB doğrudan desenini içerip içermeyeceğini de belirtebilirsiniz. Seçili tüm üç onay kutularını bırakır, ancak dönüş DataTable yönteme yeniden adlandırma `GetProducts` (Şekil 10'da gösterildiği gibi).


[![Yöntem adı dolgu ve GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Şekil 10**: yöntemler ad `Fill` ve `GetProducts` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Sihirbaz gerçekleştireceğiniz adımlar özetini görmek için İleri'yi tıklatın. Son düğmesine tıklayarak Sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra veri kümesi s artık içermelidir Tasarımcısı döndürülecek `ProductsDataTable`.


[![Yeni eklenen ProductsDataTable kümesi s Tasarımcısı gösterir](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Şekil 11**: DataSet s Tasarımcısı gösteren yeni eklenen `ProductsDataTable` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>3. adım: yeni oluşturulan saklı yordamlar inceleniyor

Adım 2'de otomatik olarak kullanılan TableAdapter Sihirbazı'nı seçme, ekleme, güncelleştirme ve verileri silmek için saklı yordamlar oluşturuldu. Bu saklı yordamları görüntülenebilir veya Sunucu Gezgini'nden gidip veritabanı s saklı yordamlar klasörü araştırıp aracılığıyla Visual Studio değiştirilemiyor. Şekil 12 gösterildiği gibi Northwind veritabanı dört yeni saklı yordamları içerir: `Products_Delete`, `Products_Insert`, `Products_Select`, ve `Products_Update`.


![2. adımda oluşturduğunuz dört saklı yordamlar veritabanı s saklı yordamlar klasöründe bulunabilir.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Şekil 12**: 2. adımda oluşturduğunuz dört saklı yordamlar veritabanı s saklı yordamlar klasöründe bulunabilir


> [!NOTE]
> Sunucu Gezgini görmüyorsanız Görünüm menüsüne gidin ve Sunucu Gezgini seçeneğini belirleyin. Adım 2'den eklenen ürünle ilgili saklı yordamlar görmüyorsanız yenileyin üzerinde saklı yordamlar klasörüne sağ tıklayıp seçerek deneyin.


Görüntülemek veya saklı yordam değiştirmek için Sunucu Gezgini adına çift tıklayın veya alternatif olarak, saklı yordam sağ tıklayın ve Aç'ı seçin. Şekil 13 göstermektedir `Products_Delete` açıldığında saklı yordamı,.


[![Saklı yordamlar açılabilir ve gelen Visual Studio içinde değiştirdi.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Şekil 13**: saklı yordamları açılabilir ve değiştiren gelen içinde Visual Studio ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


Her ikisi de içeriğini `Products_Delete` ve `Products_Select` saklı yordamlar oldukça basit. `Products_Insert` Ve `Products_Update` saklı yordamlar, diğer yandan, her ikisi de gerçekleştirmek gibi daha yakın bir inceleme garanti bir `SELECT` sonra deyimi kendi `INSERT` ve `UPDATE` deyimleri. Örneğin, aşağıdaki SQL oluşturur `Products_Insert` saklı yordamı:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Saklı yordam giriş parametreleri olarak kabul `Products` tarafından döndürülen sütunları `SELECT` TableAdapter s Sihirbazı'nı ve bu değerleri belirtilen sorgu kullanıldığı bir `INSERT` deyimi. Aşağıdaki `INSERT` deyimi, bir `SELECT` sorgu döndürmek için kullanılır `Products` sütun değerlerini (de dahil olmak üzere `ProductID`) yeni eklenen kaydın. Toplu güncelleştirme düzeni olarak otomatik olarak kullanarak yeni bir kayıt ekleme yeni eklenen güncelleştirdiğinde bu yenileme özelliği yararlıdır `ProductRow` örnekleri `ProductID` veritabanı tarafından atanan otomatik artan değerlerle özellikleri.

Aşağıdaki kod, bu özellik gösterir. İçerdiği bir `ProductsTableAdapter` ve `ProductsDataTable` için oluşturulan `NorthwindWithSprocs` yazılan veri kümesi. Yeni bir ürün oluşturarak veritabanına eklenen bir `ProductsRow` değerlerini sağlayarak ve TableAdapter s çağırma örneği `Update` tümleştirilmesidir yöntemi `ProductsDataTable`. Dahili olarak, TableAdapter s `Update` yöntemi numaralandırır `ProductsRow` geçirilen DataTable durumlarda (Bu örnekte, yalnızca bir olduğundan - bir yalnızca eklediğimiz) ve gerçekleştirir uygun ekleme, update veya delete komutu. Bu durumda, `Products_Insert` saklı yordam yürütüldüğünde, bu da yeni bir kayıt ekler `Products` tablo ve yeni eklenen kaydın ayrıntılarını döndürür. `ProductsRow` Örneği s `ProductID` değer daha sonra güncelleştirilen. Sonra `Update` yöntemi tamamlandı, yeni eklenen kayıt s erişebilmeniz için `ProductID` aracılığıyla değer `ProductsRow` s `ProductID` özelliği.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

`Products_Update` Saklı yordam benzer şekilde içeren bir `SELECT` sonra deyimi kendi `UPDATE` deyimi.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Bu saklı yordamı Not dahildir iki giriş parametreleri için `ProductID`: `@Original_ProductID` ve `@ProductID`. Bu işlev için birincil anahtarı burada değişebilir senaryoları sağlar. Örneğin, bir çalışan veritabanında her çalışan kaydı çalışan s sosyal güvenlik numarası kendi birincil anahtarı olarak kullanabilirsiniz. Var olan çalışan s sosyal güvenlik numarası değiştirmek için yeni sosyal güvenlik numarası ve özgün sağlanmalıdır. İçin `Products` tablo, bu tür işlevselliği gerekli değildir çünkü `ProductID` sütunu bir `IDENTITY` sütun ve hiçbir zaman değiştirilmelidir. Aslında, `UPDATE` deyiminde `Products_Update` saklı yordam içermiyor t dahil `ProductID` sütun listesinde sütun. Bu nedenle, while `@Original_ProductID` kullanılan `UPDATE` deyimi s `WHERE` yan tümcesi olduğu için gereksiz `Products` tablo ve tarafından değiştirilmesi `@ProductID` parametresi. Saklı yordam s parametrelerini değiştirirken Bu saklı yordamı kullanın TableAdapter yöntemleri de güncellenir önemlidir.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>4. adım: bir saklı yordam s parametreleri değiştirme ve TableAdapter güncelleştiriliyor

Bu yana `@Original_ProductID` parametredir gereksiz, let s Kaldır ondan `Products_Update` saklı yordamı tamamen. Açık `Products_Update` saklı yordamı, silme `@Original_ProductID` parametresi hem de `WHERE` yan tümcesi `UPDATE` deyimi, parametre adı kullanılan değişiklik `@Original_ProductID` için `@ProductID`. Bu değişiklikleri yaptıktan sonra T-SQL saklı yordam içinde aşağıdaki gibi görünmelidir:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Bu değişiklikleri veritabanına kaydetmek için araç çubuğunda Kaydet simgesine tıklayın veya Ctrl + S ulaştı. Bu noktada, `Products_Update` saklı yordam beklediğiniz olmayan bir `@Original_ProductID` giriş parametresi, ancak TableAdapter bu tür iletmek için yapılandırılır. TableAdapter gönderecek parametreleri görebilir `Products_Update` saklı yordamı veri kümesi Tasarımcısı'nda TableAdapter seçmek için Özellikler penceresini gidip içinde üç nokta dokunarak tarafından `UpdateCommand` s `Parameters` koleksiyonu. Bu Şekil 14'te gösterilen parametreler koleksiyonu Düzenleyicisi iletişim kutusunu açar.


![Parametreler Koleksiyonu Düzenleyicisi listeleri kullanılan parametreleri Products_Update geçirilen saklı yordamı](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Şekil 14**: parametreler koleksiyonu Düzenleyicisi listeleri kullanılan parametreleri geçirilen `Products_Update` saklı yordamı


Bu parametre yalnızca seçerek buradan kaldırabilirsiniz `@Original_ProductID` üyeleri ve Kaldır düğmesini tıklatarak listesinden parametresi.

Alternatif olarak, tüm yöntemleri için TableAdapter Tasarımcısı'nda sağ tıklayıp Yapılandır'i seçerek kullanılan parametreleri yenileyebilirsiniz. Bu kullanılan seçme, ekleme, güncelleştirme, için saklı yordamlar listeleme TableAdapter Yapılandırma Sihirbazı'nı getirir ve beklediğiniz almak saklı yordamlar, parametreleri birlikte silme. Güncelleştirme aşağı açılan listede tıklatırsanız görebilirsiniz `Products_Update` saklı yordamlar, şimdi artık içeren giriş parametreleri beklenen `@Original_ProductID` (bkz. Şekil 15). Yalnızca TableAdapter tarafından kullanılan parametre koleksiyonuna otomatik olarak güncelleştirmek için Son'u tıklatın.


[![Kendi yöntemleri parametre koleksiyonları yenilemek için alternatif olarak TableAdapter s Yapılandırma Sihirbazı'nı kullanabilirsiniz](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Şekil 15**: TableAdapter s Yapılandırma Sihirbazı'nı Yenile ITS yöntemleri parametre koleksiyonlara alternatif olarak kullanabilirsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>5. adım: Ek TableAdapter yöntemler ekleme

Gösterilen 2. adım, yeni bir TableAdapter oluşturma sırasında otomatik olarak oluşturulan karşılık gelen saklı yordamlar kolaydır. Aynı ek yöntemleri bir TableAdapter eklerken geçerlidir. Bunu göstermek için ekleme s sağlayan bir `GetProductByProductID(productID)` yönteme `ProductsTableAdapter` 2. adımda oluşturulan. Bu yöntem sürer giriş olarak bir `ProductID` değeri ve belirtilen ürün ayrıntılarını döndürür.

TableAdapter üzerinde sağ tıklayıp bağlam menüsünden Sorgu Ekle seçerek başlatın.


![TableAdapter için yeni bir sorgu ekleme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Şekil 16**: TableAdapter için yeni bir sorgu ekleme


Bu TableAdapter nasıl veritabanına erişmek için öncelikle ister TableAdapter sorgu Yapılandırma Sihirbazı başlar. Oluşturulan yeni bir saklı yordam için yeni bir saklı yordam seçeneği oluşturma seçin ve İleri'yi tıklatın.


[![Yeni bir saklı yordam seçeneği oluşturma seçin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Şekil 17**: yeni bir saklı yordam seçeneği oluşturma seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


Sonraki ekranda bize görüntülenecektir satır veya tek bir skaler değer bir dizi döndürür veya gerçekleştirmek olup olmadığını yürütülecek sorgu türünü tanımlamak için soran bir `UPDATE`, `INSERT`, veya `DELETE` deyimi. Bu yana `GetProductByProductID(productID)` yöntemi bir satırı döndürür, satır seçeneği seçili ve sonraki isabet döndüren Seç bırakın.


[![Satır seçeneği döndüren Seç](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Şekil 18**: satır seçeneği döndüren Seç ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


Sonraki ekranda yalnızca saklı yordamın adını listeler TableAdapter s ana sorgu görüntüler (`dbo.Products_Select`). Saklı yordam adı şununla değiştirin `SELECT` belirtilen ürün için ürün alanlarının tümünü döndürür deyimi:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Saklı yordam adı seçme sorgusu ile değiştir](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Şekil 19**: saklı yordam adı ile değiştirin bir `SELECT` sorgu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


Sonraki ekranda, oluşturulacak saklı yordam adı ister. Bir ad girin `Products_SelectByProductID` ve İleri'yi tıklatın.


[![Yeni saklı yordam Products_SelectByProductID adı](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Şekil 20**: yeni bir saklı yordam adı `Products_SelectByProductID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


Sihirbazın son adım adları oluşturulan yanı sıra dolgu kullanılıp kullanılmayacağını belirtmek yöntemi DataTable desenini değiştirin, DataTable desen veya her ikisi de iade olanak tanır. Bu yöntem için kullanıma her iki seçenek bırakır, ancak yöntemlerini yeniden adlandırmak `FillByProductID` ve `GetProductByProductID`. Sihirbaz gerçekleştirin ve sonra Sihirbazı tamamlamak için Son'u tıklatın adımları özetini görüntüleme için İleri'yi tıklatın.


[![TableAdapter s yöntemleri FillByProductID ve GetProductByProductID olarak yeniden adlandırın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Şekil 21**: TableAdapter s yöntemlerini yeniden adlandırmak `FillByProductID` ve `GetProductByProductID` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


Sihirbazı tamamladıktan sonra TableAdapter kullanılabilir, yeni bir yöntem sahip `GetProductByProductID(productID)` , çağrıldığında, yürütecek `Products_SelectByProductID` saklı yordamı yalnızca oluşturuldu. Saklı yordamlar klasörüne ayrıntılara ve açarak bu yeni bir saklı yordamdan Sunucu Gezgini görüntülemek için bir dakikanızı ayırın `Products_SelectByProductID` (bunu görmüyorsanız saklı yordamlar klasörü sağ tıklatın ve Yenile seçeneğini seçin).

Unutmayın `SelectByProductID` depolanan yordamı alır `@ProductID` giriş parametresi olarak ve yürütür `SELECT` Sihirbazı'nda girdiğimiz deyimi.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>6. adım: bir iş mantığı katmanı sınıfı oluşturma

Eğitmen serisi, sunu katmanı tüm kendi aramalarının iş mantığı katmanı (BLL) içinde yapılan katmanlı mimari korumak biz genişletmeniz. Bu tasarım kararına uyması için oluşturmamız ilk ürün veri sunu katmanı erişmeden önce BLL sınıfı yeni yazılan veri kümesi için gerekir.

Adlı yeni bir sınıf dosyası oluşturma `ProductsBLLWithSprocs.vb` içinde `~/App_Code/BLL` klasörü ve aşağıdaki kodu ekleyin:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Bu sınıf taklit eder `ProductsBLL` sınıf semantiğini kullanır ancak önceki öğreticileri gelen `ProductsTableAdapter` ve `ProductsDataTable` nesnelerin `NorthwindWithSprocs` veri kümesi. Örneğin, sahip olmak yerine bir `Imports NorthwindTableAdapters` deyimi sınıf dosyası olarak başlangıcında `ProductsBLL` yapar, `ProductsBLLWithSprocs` sınıfını kullanır `Imports NorthwindWithSprocsTableAdapters`. Benzer şekilde, `ProductsDataTable` ve `ProductsRow` bu sınıfta kullanılan nesneler ile önek `NorthwindWithSprocs` ad alanı. `ProductsBLLWithSprocs` Sınıfı sağlar iki veri erişim yöntemleri `GetProducts` ve `GetProductByProductID`, ve yöntemleri eklemek, güncelleştirmek ve tek bir ürün örneğini silin.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>7. adım: çalışma`NorthwindWithSprocs`sunu katmanı kümesinden

Saklı yordamlar erişmek ve temel veritabanı veri değiştirmek için kullandığı bir DAL bu noktada oluşturduk. İlkel BLL ayrıca tüm ürünleri veya ekleme, güncelleştirme yöntemlerini yanı sıra belirli bir ürünü ve silme ürünleri almak için yöntemleri oluşturdunuz. Let s Bu öğretici yuvarlanacak BLL s kullanan ASP.NET sayfası oluşturma `ProductsBLLWithSprocs` görüntülemek, güncelleştirme ve kayıtları silme sınıfı.

Açık `NewSprocs.aspx` sayfasındaki `AdvancedDAL` klasörü ve adlandırma tasarımcıya araç kutusu'ndan bir GridView sürükleyip `Products`. GridView s akıllı etiket seçin adlı yeni bir ObjectDataSource bağlamak `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLLWithSprocs` Şekil 22'de gösterildiği gibi sınıfı.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Şekil 22**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


SELECT sekmesi açılır listede iki seçenek vardır `GetProducts` ve `GetProductByProductID`. GridView tüm ürünleri görüntülenecek istiyoruz beri seçin `GetProducts` yöntemi. Güncelleştirme, ekleme ve silme sekmeleri her açılan listelerde yalnızca bir yöntem gerekir. Bu aşağı açılan listeler her seçili kendi uygun yöntemi sahip olduğundan emin olun ve Son'u tıklatın.

ObjectDataSource Sihirbaz tamamlandıktan sonra Visual Studio BoundFields ve bir CheckBoxField ürün veri alanları GridView ekleyecektir. GridView s yerleşik düzenleme ve silme özellikleri düzenlemeyi etkinleştir ve Seçenekleri akıllı etiketinde mevcut Enable Deleting denetleyerek açın.


[![Sayfa düzenleme ve silme desteği etkinleştirilmiş ile GridView içeriyor](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Şekil 23**: sayfa düzenleme ve silme desteği etkin GridView içeriyor ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Biz ullanıcı ObjectDataSource s Sihirbazı'nın tamamlanma önceki öğreticileri Visual Studio kümeleri ele `OldValuesParameterFormatString` özgün özelliğine\_{0}. Bu {0} sırayla çalışması veri değişikliği özellikler için varsayılan değerine döndürülmesi düzgün verilen bizim BLL yöntemleri tarafından beklenen parametre gerekir. Bu nedenle, ayarladığınızdan emin olun `OldValuesParameterFormatString` {0} özelliğine veya özelliği tamamen Tanımlayıcı Sözdizimi kaldırın.

Düzenleme ve silme GridView desteği ve ObjectDataSource s döndürme üzerinde kapatma veri kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra `OldValuesParameterFormatString` özelliği varsayılan değerine, sayfa s bildirim temelli biçimlendirme görünmelidir aşağıdakine benzer:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

Bu noktada biz GridView doğrulama, dahil etmek için düzenleme arabirimi özelleştirerek tutuyor sahip `CategoryID` ve `SupplierID` sütunları DropDownLists işlemek ve benzeri. Biz de Sil düğmesini istemci tarafı doğrulama ekleyebilirsiniz ve, bu geliştirmeler uygulamak için zaman ayırmanız önerilir. Bu konularda önceki eğitimlerine ele alınmış olduğundan, ancak, bunları yeniden burada ele değil.

Bir tarayıcıda sayfa s çekirdek özellikleri olup, GridView veya geliştirme bağımsız olarak, test edin. Şekil 24 gösterildiği gibi sayfa başına düzenleme ve silme özellikleri satır sağlayan GridView ürünleri listeler.


[![Ürünleri görüntülenebilir, düzenlenmesi ve GridView silindi](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Şekil 24**: ürünleri görüntülenebilir, düzenlenen ve Silinen GridView gelen ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Özet

TableAdapters yazılmış bir veri kümesinde, geçici SQL deyimlerini kullanarak veritabanından veya saklı yordamlar aracılığıyla verilere erişebilir. TableAdapter Sihirbazı yeni sağlanabilir veya ne zaman saklı yordamlar çalışma, ya da var olan saklı yordamları kullanılabilir saklı yordamları dayalı bir `SELECT` sorgu. Bu öğreticide bize için otomatik olarak oluşturulan saklı yordamlar nasıl incelediniz.

Otomatik olarak oluşturulan yardımcı zamandan saklı yordamlar yaparken, burada Sihirbazı mevcut değil t tarafından oluşturulan saklı yordam Hizala ne bizim kendi oluşturduk ile bazı durumlar vardır. Bir örnektir `Products_Update` saklı her ikisi de beklenen yordamı `@Original_ProductID` ve `@ProductID` giriş parametreleri rağmen `@Original_ProductID` parametresi gereksiz.

Çoğu senaryoda saklı yordamları zaten oluşturulmuş olabilir veya sizi bunları el ile daha hassas bir denetime saklı yordam s komutları sahip olması amacıyla oluşturmak isteyebilirsiniz. Her iki durumda da, biz yöntemlerinden için var olan saklı yordamları kullanmak için TableAdapter istemek üzere istersiniz. Biz bunu sonraki öğreticide gerçekleştirmek nasıl göreceksiniz.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Saklı yordamlar oluşturmak ve sürdürmek](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Bir saklı yordam skaler verilerini alma](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server saklı yordamı temelleri](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Saklı yordamlar: Genel bakış](http://www.sqlteam.com/item.asp?ItemID=563)
- [Saklı yordam yazma](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Hilton Geisenow oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
[sonraki](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
