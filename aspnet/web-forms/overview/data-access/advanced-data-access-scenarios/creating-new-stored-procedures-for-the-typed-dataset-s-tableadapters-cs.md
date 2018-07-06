---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Türü belirtilmiş DataSet'in TableAdapter'ları için (C#) saklı yordamlar yeni oluşturma | Microsoft Docs
author: rick-anderson
description: Önceki öğreticilerde ki kodumuz içinde SQL deyimleri oluşturduktan ve veritabanına yürütülecek deyimler geçirilen. Alternatif bir yaklaşım s kullanmaktır...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 420c4f33a6df2d7d99daeaac7682ede12adfb122
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824776"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Yeni saklı yordam oluşturma türü belirtilmiş DataSet'in TableAdapter'ları için (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) veya [PDF olarak indirin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> Önceki öğreticilerde ki kodumuz içinde SQL deyimleri oluşturduktan ve veritabanına yürütülecek deyimler geçirilen. Alternatif bir yaklaşım saklı yordamlar SQL deyimleriyle veritabanını önceden tanımlanmış olduğu kullanmaktır. Bu öğreticide size TableAdapter Sihirbazı'nın bizim için yeni saklı yordamlar oluşturma konusunda bilgi edinin.


## <a name="introduction"></a>Giriş

Bu öğreticiler için veri erişim katmanı (DAL), yazılan veri kümelerini kullanır. Bölümünde açıklandığı gibi [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğreticide yazılı veri kümeleri oluşur kesin türü belirtilmiş DataTable ve TableAdapter bağdaştırıcıları. DataTable mantıksal varlıkların TableAdapters arabirimi veri erişim işini gerçekleştirmek için temel alınan veritabanı ile çalışırken sistem temsil eder. Bu, DataTables verilerle doldurma, skaler veri döndüren sorgular çalıştırma ve ekleme, güncelleştirme ve kayıtlarını veritabanından silmek içerir.

TableAdapter bağdaştırıcıları tarafından yürütülen SQL komutlarını geçici SQL deyimleri ya da olabilir `SELECT columnList FROM TableName`, ya da saklı yordamlar. Geçici SQL deyimleri mimarimiz TableAdapters öğesine kullanır. Ancak birçok geliştiricileri ve Veritabanı yöneticileri, saklı yordamları üzerinden güvenlik, Bakım ve güncellenebilirliğini nedeniyle geçici SQL deyimleri tercih. Diğerleri kendi esnekliğinde geçici SQL deyimlerini ardently tercih eder. Kendi iş miyim saklı yordamlar, geçici SQL deyimleri ayrıcalık tanı, ancak önceki öğreticiler basitleştirmek için geçici SQL deyimleri kullanmayı tercih etti.

Ne zaman bir TableAdapter tanımlama veya yeni yöntemler ekleme, TableAdapter s Sihirbazı'nı yalnızca olarak geçici SQL deyimlerini çalıştığı gibi yeni saklı yordamlar oluşturma veya mevcut saklı yordamlara kullanmayı kolaylaştırır. Bu öğreticide TableAdapter s Sihirbazı saklı yordamlarını otomatik olarak oluşturmak nasıl inceleyeceğiz. Sonraki öğreticide veya el ile oluşturulan mevcut saklı yordamları kullanmak için TableAdapter s yöntemlerini yapılandırmak nasıl tümleştirildiği incelenmektedir.

> [!NOTE]
> Rob Howard'ın blog girişine bakın [olmayan kullanım depolanan yordamları henüz?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) ve [Frans Bouma](https://weblogs.asp.net/fbouma/) s blog girişine [saklı yordamlar, hatalı, M Kay misiniz?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) avantajları ve dezavantajları hakkında canlı bir tartışma için saklı yordamlar ve geçici SQL.


## <a name="stored-procedure-basics"></a>Saklı yordam temelleri

Tüm programlama dillerinde ortak bir yapısı işlevlerdir. Bir işlev, işlev çağrıldığında yürütüldüğü deyimleri koleksiyonudur. İşlevler, parametrelerini kabul edebilir ve isteğe bağlı olarak bir değer döndürebilir. *[Saklı yordamlar](http://en.wikipedia.org/wiki/Stored_procedure)*  , programlama dillerindeki işlevleri ile birçok benzerliğe veritabanı yapılarıdır. Bir saklı yordam saklı yordam çağrıldığında yürütüldüğü T-SQL deyimleri kümesinden oluşur. Bir saklı yordam birçok giriş parametreleri için sıfır kabul edebilir ve skaler değerler, çıktı parametreleri döndürebilir veya sonucu en yaygın olarak ayarlar `SELECT` sorgular.

> [!NOTE]
> Saklı yordamlar önerilmesine sprocs veya SPs olarak adlandırılır.


Saklı yordamlar kullanılarak oluşturulur [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) T-SQL deyimi. Örneğin, aşağıdaki T-SQL betiğini adlı bir saklı yordam oluşturur `GetProductsByCategoryID` adlı tek bir parametre almayan `@CategoryID` ve döndürür `ProductID`, `ProductName`, `UnitPrice`, ve `Discontinued` bu sütun alanları `Products` eşleştirmesi olan tablo `CategoryID` değeri:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Bu saklı yordamı oluşturulduktan sonra aşağıdaki sözdizimi kullanılarak çağrılabilir:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> Sonraki öğreticide Visual Studio IDE ile saklı yordamlar oluşturulmasına inceleyeceğiz. Bu öğreticide, ancak bizim için otomatik olarak saklı yordamları üret TableAdapter Sihirbazı sağlamak için kullanacağız.


Yalnızca veri döndürmek yanı sıra, saklı yordamlar, tek bir işlem kapsamında birden fazla veritabanı komutlarını gerçekleştirmek için genellikle kullanılır. Adlı bir saklı yordam `DeleteCategory`, örneğin, sürebilir bir `@CategoryID` parametresi ve iki gerçekleştirmek `DELETE` deyimleri: ilgili ürün ve ikinci bir belirtilen kategori siliniyor silmek için önce bir. Bir saklı yordam içinde birden çok deyimleri *değil* otomatik olarak kaydırılan bir işlem içinde. Ek T-SQL komutlarını saklı yordamı birden çok komut, atomik işlem kabul edilir s emin olmak için verilmiş olması gerekir. Sonraki öğreticide bir işlem kapsamında bir saklı yordam s komutları sarmalama göreceğiz.

Saklı yordamları bir mimari kullanırken, veri erişim katmanı s yöntemleri bir geçici SQL deyimini yürütmeden yerine belirli bir saklı yordam çağırma. Bu, (veritabanında) çalıştırılan SQL deyimlerini konumunu, uygulama s mimarisi içinde tanımlanan yerine merkeze alır. Tartışmaya merkezileştirilmesi sorgularınızı ayarlamak bulmak ve çözümlemek kolaylaştırır ve nerede ve nasıl veritabanı kullanılıyor dair daha net bir resim sunar.

Saklı yordamı ile ilgili temel bilgiler hakkında daha fazla bilgi için bu öğreticinin sonunda daha fazla bilgi bölümdeki kaynaklar başvurun.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>1. adım: Gelişmiş veri erişimi katmanı senaryoları Web sayfası oluşturma

Biz saklı yordamları kullanarak bir DAL oluşturma ile ilgili bizim tartışma başlamadan önce ilk ASP.NET sayfaları Biz bu ve sonraki birkaç öğreticiler için gereksinim duyacağınız bizim Web sitesi projesi oluşturmak için bir dakikanızı ayırarak s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `AdvancedDAL`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Gelişmiş Veri erişim katmanı senaryoları öğreticileri için ASP.NET sayfaları ekleme](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Şekil 1**: Gelişmiş Veri erişim katmanı senaryoları öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `AdvancedDAL` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu nedenle, bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Son olarak, girişleri olarak bu sayfalar ekleme `Web.sitemap` dosya. Özellikle, toplu verilerle çalışma aşağıdaki işaretlemeyi ekleyin `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık Gelişmiş DAL senaryoları öğreticileri için öğeleri içerir.


![Site Haritası girişleri için Gelişmiş DAL senaryoları öğreticiler artık içerir.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Şekil 3**: Site Haritası girişleri için Gelişmiş DAL senaryoları öğreticiler artık içerir.


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>2. adım: Yeni bir TableAdapter yapılandırma saklı yordamları

Let s yerine geçici SQL deyimleri depolanan yordamları kullanan veri erişim katmanını oluşturma göstermek için yeni bir türü belirtilmiş DataSet oluşturmak `~/App_Code/DAL` adlı klasöre `NorthwindWithSprocs.xsd`. Biz bu işlemi ayrıntılı önceki öğreticilerdeki basamaklı olduğundan, biz buradaki adımları hızlı bir şekilde devam eder. Tıkanıp kalırsanız veya oluşturma ve yapılandırma türü belirtilmiş veri kümesi içinde daha ayrıntılı adım adım yönergelere ihtiyacınız varsa, kiracıurl [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğretici.

Sağ tıklayarak yeni bir veri kümesini projeye ekleyin `DAL` Add New Item seçme ve Şekil 4'te gösterildiği gibi veri kümesi şablonu seçip klasör.


[![NorthwindWithSprocs.xsd adlı projeye yeni bir türü belirtilmiş veri kümesi ekleyin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Şekil 4**: adlı proje için yeni bir türü belirtilmiş veri kümesi ekleme `NorthwindWithSprocs.xsd` ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Bu yeni türü belirtilmiş veri kümesi oluşturma, onun Tasarımcısı'nı açın, yeni bir TableAdapter oluşturmak ve TableAdapter Yapılandırma Sihirbazı'nı başlatın. TableAdapter Yapılandırma Sihirbazı'nı s ilk adımı bize çalışmak için veritabanı seçmek için sorar. Northwind veritabanına bağlantı dizesi, aşağı açılan listeden listelenmelidir. Bu seçeneği belirleyin ve İleri'ye tıklayın.

Bu sonraki ekranda TableAdapter veritabanına nasıl erişmeli seçebilirsiniz. Önceki öğreticilerde, seçtik ilk seçenek, SQL deyimi kullan. Bu öğretici için ikinci seçeneği belirleyin, yeni saklı yordamlar oluşturma ve İleri'ye tıklayın.


[![Yeni saklı yordamlar oluşturma TableAdpater isteyin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Şekil 5**: Yeni saklı yordamlar oluşturma için TableAdpater isteyin ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Yalnızca geçici SQL deyimlerini kullanarak, aşağıdaki adımda size istendiği gibi `SELECT` TableAdapter s ana sorgu deyimi. Ancak kullanmak yerine `SELECT` deyimi, doğrudan bir geçici sorgu gerçekleştirmek için buraya girilen TableAdapter s Sihirbazı bu içeren bir saklı yordam oluşturur `SELECT` sorgu.

Aşağıdaki `SELECT` bu TableAdapter sorgusu:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![SELECT sorgusu girin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Şekil 6**: girin `SELECT` sorgu ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> Yukarıdaki sorgu ana sorgudan biraz farklı `ProductsTableAdapter` içinde `Northwind` türü belirtilmiş veri kümesi. Bu geri çağırma `ProductsTableAdapter` içinde `Northwind` türü belirtilmiş veri kümesi için her bir ürün s kategorisi ve sağlayıcı şirket adı ve kategori adını geri getirmek için iki bağıntılı alt sorgular içerir. Yaklaşan içinde [kullanım katılan TableAdapter güncelleştirme](updating-the-tableadapter-to-use-joins-cs.md) bu TableAdapter için Biz bu ekleme sırasında görünür öğretici ilgili verileri.


Gelişmiş Seçenekler düğmesine için bir dakikanızı ayırın. Buradan Sihirbazı'nı da INSERT, update ve delete deyimleri TableAdapter, iyimser eşzamanlılık kullanılıp kullanılmayacağını ve veri tablosu ınsertler ve updateler sonra mi yenileneceğini oluşturup oluşturmayacağını belirtebilirsiniz. Generate INSERT, Update ve Delete deyimleri seçeneği varsayılan olarak denetlenir. İşaretli bırakın. Bu öğreticide, iyimser eşzamanlılık seçeneklerini kullanın işaretlemeden bırakın.

TableAdapter Sihirbazı tarafından otomatik olarak oluşturulan saklı yordamlar içerdiğinde, veri tablosu seçeneği yenileme göz ardı edilir görünür. Adım 3'te göreceğiz olarak bu onay kutusunun işaretli olup olmadığına, sonuçta elde edilen INSERT ve update bağımsız olarak saklı yordamlar just-eklenen ya da yalnızca güncelleştirilen kaydı alır.


![Generate INSERT, Update ve Delete deyimleri seçeneğinin işaretli bırakın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Şekil 7**: Generate INSERT, Update ve Delete deyimleri seçeneğinin işaretli bırakın


> [!NOTE]
> Kullanım iyimser eşzamanlılık seçenek işaretlenirse, sihirbaz ek koşullar ekleyecektir `WHERE` veri diğer alanlarda değişiklikler, güncelleştirilmesini engelleyecek yan tümcesi. Kiracıurl [iyimser eşzamanlılık uygulama](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) TableAdapter s yerleşik iyimser eşzamanlılık denetim özelliğini kullanma hakkında daha fazla bilgi için öğretici.


Girdikten sonra `SELECT` sorgulamak ve Generate INSERT, Update ve Delete deyimleri seçeneğinin işaretli olduğundan emin onaylayan, İleri'ye tıklayın. Şekil 8'de gösterilen bu sonraki ekranda, sihirbaz oluşturacak saklı yordamları adlarını seçme, ekleme, güncelleştirme ve verileri silme ister. Bu saklı yordamlar adlarına değişiklik `Products_Select`, `Products_Insert`, `Products_Update`, ve `Products_Delete`.


[![Saklı yordamları yeniden adlandır](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Şekil 8**: saklı yordamlar yeniden adlandır ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


TableAdapter Sihirbazı, dört saklı yordamları oluşturmak için kullanacağı T-SQL görmek için SQL betiğini Önizle düğmesine tıklayın. SQL betiğini Önizle iletişim kutusundan betik bir dosyaya kaydedin veya panoya kopyalayın.


![Saklı yordamları oluşturmak için kullanılan SQL betiğini](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Şekil 9**: SQL betiğini kullanılan saklı yordamları oluşturmak için Önizleme


Saklı yordamları adlandırdıktan sonra adı TableAdapter s karşılık gelen yöntemlere yanındaki tıklayın. Geçici SQL deyimleri kullanırken olduğu gibi mevcut bir DataTable Doldur veya yeni bir tane döndüren yöntemler oluşturabiliriz. Biz TableAdapter ekleme, güncelleme ve silme kayıtlarını DB doğrudan desenini içerip içermeyeceğini de belirtebilirsiniz. Tüm üç işaretli bırakın, ancak bir DataTable yöntem dönüş Yeniden Adlandır `GetProducts` (Şekil 10'da gösterildiği gibi).


[![Yöntem adı dolgu ve GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Şekil 10**: yöntemleri ad `Fill` ve `GetProducts` ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Sihirbaz gerçekleştireceğiniz adımlar özetini görmek için İleri'ye tıklayın. Son düğmesini tıklatarak Sihirbazı tamamlayın. Sihirbaz tamamlandıktan sonra veri kümesi Tasarımcısı artık içermelidir s döndürülecek `ProductsDataTable`.


[![Veri kümesi s Tasarımcı yeni eklenen ProductsDataTable gösterir](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Şekil 11**: DataSet s Tasarımcısı gösteren yeni eklenen `ProductsDataTable` ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>3. adım: yeni oluşturulan saklı yordamları İnceleme

Adım 2'de otomatik olarak kullanılan TableAdapter Sihirbazı seçme, ekleme, güncelleştirme ve verileri silmek için saklı yordamlar oluşturuldu. Bu saklı yordamlar, görüntülenen veya Sunucu Gezginine gidip veritabanı s saklı yordamlar klasörüne detaya Visual Studio aracılığıyla değiştirilebilir. Şekil 12 gösterildiği gibi Northwind veritabanına dört yeni saklı yordamları içerir: `Products_Delete`, `Products_Insert`, `Products_Select`, ve `Products_Update`.


![2. adımda dört saklı yordamları s veritabanı saklı yordamlar klasöründe bulunabilir.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Şekil 12**: 2. adımda dört saklı yordamları s veritabanı saklı yordamlar klasöründe bulunabilir


> [!NOTE]
> Sunucu Gezgini'ni görmüyorsanız, Görünüm menüsüne gidin ve Sunucu Gezgini seçeneği belirleyin. Adım 2'den eklenen ürün ile ilgili saklı yordamlar görmüyorsanız saklı yordamlar klasörü sağ tıklayıp seçme deneyin yenileyin.


Görüntülemek veya bir saklı yordam değiştirmek için sunucu Gezgini'ndeki kendi adına çift tıklayın veya alternatif olarak, saklı yordam üzerinde sağ tıklayın ve Aç'ı seçin. Şekil 13 gösterir `Products_Delete` saklı yordamı, açıldığında.


[![Saklı yordamlar açılabilir ve Visual Studio içinden gelen değişiklik](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Şekil 13**: saklı yordamları açılabilir ve değiştiren gelen içinde Visual Studio ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


Her ikisi de içeriğini `Products_Delete` ve `Products_Select` saklı yordamlar oldukça basittir. `Products_Insert` Ve `Products_Update` saklı yordamlar, diğer yandan, her ikisi de gerçekleştirdikçe daha yakın bir inceleme garanti bir `SELECT` deyiminden sonra kendi `INSERT` ve `UPDATE` deyimleri. Örneğin, aşağıdaki SQL oluşturur `Products_Insert` saklı yordam:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Saklı yordam giriş parametreleri olarak kabul eder `Products` tarafından döndürülen sütunlar `SELECT` s TableAdapter Sihirbazı'nı ve bu değerleri belirtilen sorgu kullanıldığı bir `INSERT` deyimi. Aşağıdaki `INSERT` deyimi, bir `SELECT` döndürmek için kullanılan sorgu `Products` sütun değerleri (dahil olmak üzere `ProductID`) yeni eklenen kaydın. Yeni eklenen güncelleştirmeleri otomatik olarak toplu güncelleştirme deseni kullanılarak yeni bir kaydı ekleyerek bu yenileme özelliği kullanışlıdır `ProductRow` örnekleri `ProductID` veritabanı tarafından atanan otomatik olarak artan değerleri ile özellikleri.

Aşağıdaki kod, bu özellik gösterir. İçerdiği bir `ProductsTableAdapter` ve `ProductsDataTable` için oluşturulan `NorthwindWithSprocs` türü belirtilmiş veri kümesi. Yeni ürün oluşturarak veritabanına eklenen bir `ProductsRow` değerleri sağlayarak ve TableAdapter s çağırma örneği `Update` tümleştirilmesidir yöntemi `ProductsDataTable`. Dahili olarak, TableAdapter s `Update` yöntemi numaralandırır `ProductsRow` geçilen DataTable örnekleri (Bu örnekte var olan yalnızca bir - bir eklediğimiz yöntemlerin) ve uygun ekleme, güncelleştirme veya silme komutu. Bu durumda, `Products_Insert` saklı yordam yürütüldüğünde, bu yeni kayda ekler `Products` tablo ve yeni eklenen kaydın ayrıntılarını döndürür. `ProductsRow` Örneği s `ProductID` değeri sonra güncelleştirilir. Sonra `Update` yöntemi tamamlandıktan, yeni eklenen kayıt s erişip `ProductID` aracılığıyla değer `ProductsRow` s `ProductID` özelliği.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

`Products_Update` Benzer şekilde, saklı yordam içeren bir `SELECT` deyiminden sonra kendi `UPDATE` deyimi.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Bu saklı yordamı Not iki giriş parametrelerini içeren `ProductID`: `@Original_ProductID` ve `@ProductID`. Bu işlev için birincil anahtarı nerede değiştirilebilir senaryoları sağlar. Örneğin, bir çalışan veritabanında, her bir çalışan kaydı çalışan s sosyal güvenlik numarası, birincil anahtar olarak kullanabilirsiniz. Var olan bir çalışanın s sosyal güvenlik numarasını değiştirmek için hem özgün, hem de yeni sosyal güvenlik numarası sağlanmalıdır. İçin `Products` tablo, bu işlevselliğin gerekli değildir çünkü `ProductID` sütunu bir `IDENTITY` sütun ve hiçbir zaman değiştirilmelidir. Aslında, `UPDATE` deyiminde `Products_Update` saklı yordam eklenmemişse t dahil `ProductID` sütunu, sütun listesinde. Bu nedenle, while `@Original_ProductID` kullanılır `UPDATE` deyimi s `WHERE` yan tümcesi olduğu için gereksiz `Products` tarafından değiştirilebilir ve tablo `@ProductID` parametresi. Bir saklı yordam s parametrelerini değiştirirken Bu saklı yordam kullanmak TableAdapter yöntemleri de güncellenir önemlidir.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>4. adım: bir saklı yordam s parametreleri ve TableAdapter güncelleştiriliyor

Bu yana `@Original_ProductID` parametredir gereksiz, let s öğesinden kaldırın `Products_Update` tamamen saklı yordamı. Açık `Products_Update` saklı yordamı, silme `@Original_ProductID` parametresini hem de `WHERE` yan tümcesi `UPDATE` ifadesi, parametre adı kullanılan değişiklik `@Original_ProductID` için `@ProductID`. Bu değişiklikleri yaptıktan sonra T-SQL saklı yordam içinde aşağıdaki gibi görünmelidir:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Bu değişiklikleri veritabanına kaydetmek için araç çubuğunda Kaydet simgesine tıklayın veya Ctrl + S isabet. Bu noktada, `Products_Update` saklı yordam girmek istemediğiniz bir `@Original_ProductID` giriş parametresi, ancak TableAdapter böyle bir parametre geçirmek için yapılandırılır. TableAdapter gönderecek şekilde parametreleri gördüğünüz `Products_Update` saklı yordamı veri kümesi Tasarımcısı'nda TableAdapter'ı seçerek, Özellikler penceresinde gidip, üç noktaya tıklayarak `UpdateCommand` s `Parameters` koleksiyonu. Bu Şekil 14'te gösterilen parametre koleksiyon Düzenleyicisi iletişim kutusu getirir.


![Parametreler Koleksiyonu Düzenleyicisi listeleri kullanılan parametreler için Products_Update geçirilen depolanan yordamı](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Şekil 14**: geçirilen parametre Koleksiyonu Düzenleyicisi listeleri kullanılan parametreler `Products_Update` depolanan yordamı


Yalnızca'i seçerek bu parametre buradan kaldırabilirsiniz `@Original_ProductID` listesinden üyeleri ve Kaldır düğmesini tıklatarak parametre.

Alternatif olarak, TableAdapter Tasarımcısı'nda sağ tıklayıp Yapılandır'ı seçerek tüm yöntemleri için kullanılan parametreler yenileyebilirsiniz. Bu saklı yordamları kullanılan seçme, ekleme, güncelleştirme, listeleme TableAdapter Yapılandırma Sihirbazı'nı ortaya çıkarır ve silme, parametreleri birlikte almak saklı yordamları beklerler. Güncelleştirme aşağı açılan listede tıklarsanız gördüğünüz `Products_Update` saklı yordamlar, artık artık içeren giriş parametrelerini beklenen `@Original_ProductID` (bkz. Şekil 15). Yalnızca TableAdapter tarafından kullanılan parametre koleksiyonu otomatik olarak güncelleştirmek için Son'u tıklatın.


[![Alternatif olarak yöntem parametre koleksiyonları yenilemek için TableAdapter s Yapılandırma Sihirbazı'nı kullanabilirsiniz](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Şekil 15**: alternatif olarak, TableAdapter s Yenile Its yöntemler parametre koleksiyonlara Yapılandırma Sihirbazı'nı kullanabilirsiniz ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>5. adım: Ek TableAdapter yöntemleri ekleme

Gösterilen 2. adım, yeni bir TableAdapter oluştururken otomatik olarak oluşturulan karşılık gelen saklı yordamlar kolaydır. Aynı Tableadapter'a ek yöntemler ekleme geçerlidir. Bunu açıklamak üzere ekleme s izin bir `GetProductByProductID(productID)` yönteme `ProductsTableAdapter` 2. adımda oluşturmuştunuz. Bu yöntem sürer giriş olarak bir `ProductID` değer ve belirtilen ürün ayrıntılarını döndürür.

Tableadapter'a sağ tıklayıp bağlam menüsünden Sorgu Ekle seçerek başlatın.


![Yeni bir sorgu için TableAdapter Ekle](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Şekil 16**: yeni bir sorgu için TableAdapter Ekle


Bu, TableAdapter veritabanına nasıl erişmeli için ilk ister TableAdapter sorgu Yapılandırma Sihirbazı ' nı başlatır. Oluşturulan yeni bir saklı yordam için oluştur yeni bir saklı yordam seçeneği seçin ve İleri'ye tıklayın.


[![Yeni bir saklı yordam seçeneği Create seçin](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Şekil 17**: oluşturma, yeni bir saklı yordam seçeneği seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


Sonraki ekranda, bir dizi satır veya tek bir skaler değer döndürür veya gerçekleştirmek yürütülecek sorgu türünü tanımlamak için bize soran bir `UPDATE`, `INSERT`, veya `DELETE` deyimi. Bu yana `GetProductByProductID(productID)` yöntemi bir satır döndürür, satır seçeneği seçili ve sonraki isabet döndüren SELECT bırakın.


[![Satır seçeneği döndüren Seç](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Şekil 18**: satır seçeneği döndüren Seç ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


Sonraki ekranda yeni saklı yordamın adını listeler TableAdapter s ana sorgu görüntüler (`dbo.Products_Select`). Saklı yordam adı aşağıdakiyle değiştirin `SELECT` tüm belirtilen bir ürünün ürün alanları döndüren bir ifade:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Saklı yordam adı bir SELECT sorgusu ile değiştirin.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Şekil 19**: saklı yordam adı ile değiştirin. bir `SELECT` sorgu ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


Sonraki ekran oluşturulacak saklı yordam adı ister. Bir ad girin `Products_SelectByProductID` ve İleri'ye tıklayın.


[![Yeni saklı yordam Products_SelectByProductID adı](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Şekil 20**: yeni bir saklı yordam adı `Products_SelectByProductID` ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


Sihirbazın son adım adları oluşturulan yanı sıra dolgu kullanılıp kullanılmayacağını belirtmek yöntemi bir DataTable desenini değiştirin, iade DataTable desen veya her ikisi de olanak tanır. Bu yöntem için iki seçenek de işaretli bırakın, ancak yeniden adlandırmak için yöntemleri `FillByProductID` ve `GetProductByProductID`. Sihirbaz gerçekleştirin ve sonra Sihirbazı tamamlamak için Son'u tıklatın adımları özetini görüntülemek için İleri'ye tıklayın.


[![TableAdapter s yöntemleri FillByProductID ve GetProductByProductID olarak yeniden adlandırın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Şekil 21**: TableAdapter s yöntemlere Yeniden Adlandır `FillByProductID` ve `GetProductByProductID` ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Sihirbazı tamamladıktan sonra yeni bir yöntem kullanılabilir, TableAdapter'in `GetProductByProductID(productID)` , çağrıldığında yürütecek `Products_SelectByProductID` saklı yordamı yalnızca oluşturulmuş. Bu yeni bir saklı yordamı Sunucu Gezgini'nden saklı yordamlar klasörüne araştırıp bulma ve açma görüntülemek için bir dakikanızı ayırın `Products_SelectByProductID` (bunu görmüyorsanız, saklı yordamlar klasörü sağ tıklatın ve Yenile'yi seçin).

Unutmayın `SelectByProductID` depolanan yordam alır `@ProductID` giriş parametresi olarak ve yürüten `SELECT` sihirbazda girdiğimiz deyimi.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>6. adım: bir iş mantığı katmanı sınıfı oluşturma

Öğretici serisinin, sunu katmanı tüm çağrıları, iş mantığı katmanı (BLL) içinde yapılan katmanlı bir mimari korumak biz genişletmeniz. Bu tasarım kararınız uyması için önce ürün verileri sunu katmanı erişmeden önce yeni türü belirtilmiş veri kümesi için bir BLL sınıfı oluşturmak ihtiyacımız var.

Adlı yeni bir sınıf dosyası oluşturma `ProductsBLLWithSprocs.cs` içinde `~/App_Code/BLL` klasörü ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Bu sınıf taklit eden `ProductsBLL` sınıfı kullanır ancak önceki öğreticilerden semantiği `ProductsTableAdapter` ve `ProductsDataTable` nesnelerin `NorthwindWithSprocs` veri kümesi. Örneğin, sahip olmak yerine bir `using NorthwindTableAdapters` deyimi sınıf dosyasının başında `ProductsBLL` mu, `ProductsBLLWithSprocs` sınıfı kullanır `using NorthwindWithSprocsTableAdapters`. Benzer şekilde, `ProductsDataTable` ve `ProductsRow` bu sınıfta kullanılan nesneleri önekiyle `NorthwindWithSprocs` ad alanı. `ProductsBLLWithSprocs` SAX iki veri erişim yöntemlerine, `GetProducts` ve `GetProductByProductID`, yöntemleri eklemek, güncelleştirmek ve tek ürün örneğini silin.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>7. adım: çalışma`NorthwindWithSprocs`veri kümesinden sunu katmanı

Bu noktada saklı yordamlar erişmek ve temel alınan veritabanı verileri değiştirmek için kullandığı bir DAL oluşturduk. Ayrıca bir ilkel BLL tüm ürünler veya ekleme, güncelleştirme için yöntemlerle birlikte belirli bir ürünü ve silme ürünleri almak için yöntemleri ile derledik. Bu öğreticide yuvarlamak için let s oluşturma BLL s kullanan bir ASP.NET sayfasını `ProductsBLLWithSprocs` görüntüleme, güncelleştirme ve silme kayıtlarını için sınıf.

Açık `NewSprocs.aspx` sayfasını `AdvancedDAL` klasörü ve adlandırma Tasarımcısı araç kutusundan sürükleyip GridView `Products`. GridView ' s akıllı etiket seçin adlı yeni bir ObjectDataSource bağlamak `ProductsDataSource`. ObjectDataSource kullanmak için yapılandırma `ProductsBLLWithSprocs` Şekil 22'de gösterildiği gibi sınıfı.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Şekil 22**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


İki seçeneği seçme sekmesinde açılır listede bulunan `GetProducts` ve `GetProductByProductID`. GridView tüm ürünleri görüntülemek istiyoruz beri seçin `GetProducts` yöntemi. UPDATE, INSERT ve DELETE sekmelerdeki her açılan listeler, yalnızca bir yöntem gerekir. Bu açılan listelerin uygun yönteminin seçildiğini olduğundan emin olun ve ardından Son'a tıklayın.

ObjectDataSource Sihirbaz tamamlandıktan sonra Visual Studio BoundFields ve bir CheckBoxField ürün veri alanları için GridView ekleyeceksiniz. GridView s yerleşik düzenleme ve silme özelliklerini düzenlemeyi etkinleştir ve akıllı etiketinde mevcut seçenekler silmeyi etkinleştir'i işaretleyerek etkinleştirin.


[![Sayfa düzenleme ve silme desteği etkin ile GridView içeriyor](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Şekil 23**: GridView düzenleme ve silme desteği etkin sayfa içeriyor ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Ve ObjectDataSource s Sihirbazı'nın tamamlanma önceki öğreticilerde, Visual Studio kümeleri ele almıştık `OldValuesParameterFormatString` özgün özelliğini\_{0}. Bu varsayılan değerine döndürülmesi gereken {0} düzgün çalışması veri değişikliği özellikleri için sırayla verilen bizim BLL yöntemleri tarafından beklenen parametre. Bu nedenle, ayarladığınızdan emin olun `OldValuesParameterFormatString` özelliğini {0} veya özelliği tamamen bildirim temelli söz dizimi kaldırın.

Düzenleme ve silme GridView desteği ve ObjectDataSource s döndüren üzerinde veri kaynağı Yapılandırma Sihirbazı tamamlandıktan sonra `OldValuesParameterFormatString` özelliği varsayılan değerine, sayfa s bildirim temelli biçimlendirme görünmelidir aşağıdakine benzer:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

Bu noktada biz GridView ' doğrulama, dahil etmek için düzenleme arabirimini özelleştirme tarafından tutuyor sahip `CategoryID` ve `SupplierID` sütunlar DropDownList işlemek ve benzeri. Biz de Sil düğmesini için istemci tarafı doğrulama ekleyebilirsiniz ve bu geliştirmeler uygulamak için zaman ayırmanız geçmenizi öneriyoruz. Önceki öğreticilerde, bu konularda ele alınmış olduğundan, ancak biz bunları yeniden buraya kapsamaz.

Bir tarayıcıda sayfa s çekirdek özellikleri olup, GridView veya geliştirme bağımsız olarak, test edin. Şekil 24 gösterildiği gibi sayfa başına düzenleme ve silme özelliklerini satır sağlayan GridView ürünleri listeler.


[![Ürünler görüntülenebilir, düzenlenebilir ve GridView silindi](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Şekil 24**: ürünler görüntülenebilir, düzenlenen ve Silinen GridView gelen ([tam boyutlu görüntüyü görmek için tıklatın](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Özet

TableAdapter bağdaştırıcıları türü belirtilmiş veri kümesi, geçici SQL deyimlerini kullanarak veritabanındaki veya saklı yordamları aracılığıyla verilere erişebilir. TableAdapter Sihirbazı yeni sağlanabilir veya ne zaman ya da mevcut saklı yordamlara kullanılabilir saklı yordamlarla çalışma, temel yordamlar depolanan bir `SELECT` sorgu. Bu öğreticide, biz nasıl bizim için otomatik olarak oluşturulan depolanmış yordamlara sahip incelediniz.

Saklı yordamları otomatik olarak oluşturulan yardımcı zamandan tasarruf yaparken, burada saklı yordamı tarafından Sihirbazı eklenmemişse t hizalama ne bizim kendi oluşturduk ile bazı durumlar vardır. Bir örnek `Products_Update` saklı yordamsa, her ikisi de beklenen `@Original_ProductID` ve `@ProductID` giriş parametreleri olsa bile `@Original_ProductID` parametresi gereksiz.

Birçok senaryoda saklı yordamları zaten oluşturulmuş olabilir ya da biz bunları el ile zahmetli saklı yordam s komutlar üzerinde denetim sahibi olması için oluşturmak isteyebilirsiniz. Her iki durumda da, biz metotlarını için mevcut saklı yordamlara kullanılacak TableAdapter almasını istersiniz. Biz, bunu bir sonraki öğreticide gerçekleştirmek nasıl göreceksiniz.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Saklı yordamları oluşturmak ve sürdürmek](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Bir saklı yordamdan skaler veri alma](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [SQL Server saklı yordamı temelleri](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Saklı yordamlar: Genel bakış](http://www.sqlteam.com/item.asp?ItemID=563)
- [Bir saklı yordam yazma](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Hilton Geisenow oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
