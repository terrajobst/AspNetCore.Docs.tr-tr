---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: Saklı yordamları ve kullanıcı tanımlı işlevleri ile oluşturma, yönetilen kod (VB) | Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 .NET ortak dil nesneleri yönetilen kod aracılığıyla geliştiricilerin izin vermek için çalışma zamanı ile tümleştirilir. Bu öğreticide...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 438ebfa474ab510d90738c4a3ee40e172d838dcb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752543"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Saklı yordamlar ve yönetilen kod (VB) ile kullanıcı tanımlı işlevler oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) veya [PDF olarak indirin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 .NET ortak dil nesneleri yönetilen kod aracılığıyla geliştiricilerin izin vermek için çalışma zamanı ile tümleştirilir. Bu öğretici, yönetilen saklı yordamlar oluşturma işlemini gösterir ve yönetilen kullanıcı tanımlı işlevler, Visual Basic veya C# koduna sahip. Visual Studio'nun bu sürümleri gibi yönetilen veritabanı nesneleri hata ayıklamak için nasıl izin görüyoruz.


## <a name="introduction"></a>Giriş

Veritabanları gibi Microsoft s SQL Server 2005 [Transact-Structured sorgu dili (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) ekleme, değiştirme ve veri alma. Çoğu veritabanı sistemi tek, yeniden kullanılabilir bir birim olarak yürütülebilen SQL deyimleri bir dizi gruplandırma yapıları içerir. Saklı yordamlar, bir örnektir. Başka bir özelliktir *kullanıcı tanımlı işlevler*(UDF), bir yapı daha ayrıntılı olarak 9. adım inceleyeceğiz.

Özünde, SQL veri kümeleriyle çalışmak için tasarlanmıştır. `SELECT`, `UPDATE`, Ve `DELETE` deyimleri kendiliğinden karşılık gelen tablosundaki tüm kayıtlar için geçerlidir ve yalnızca sınırlıdır, `WHERE` yan tümceleri. Henüz bir kayıtla aynı anda çalışmak için ve skaler verileri işlemek için tasarlanmış birçok dil özellikleri de vardır. [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553) bir kayıt kümesi için teker teker aracılığıyla sokulması izin verir. Dize işleme işlevleri gibi `LEFT`, `CHARINDEX`, ve `PATINDEX` skaler verilerle çalışın. SQL denetim akış deyimlerindeki gibi yöntemlerine `IF` ve `WHILE`.

Microsoft SQL Server 2005'ten önce saklı yordamlar ve UDF'ler yalnızca T-SQL deyimlerinin koleksiyonu olarak tanımlanabilir. SQL Server 2005, ancak ile tümleştirme sağlamak üzere tasarlanmış [ortak dil çalışma zamanı (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), tüm .NET derlemeleri tarafından kullanılan çalışma zamanı olduğu. Sonuç olarak, saklı yordamlar ve UDF'ler bir SQL Server 2005 veritabanı'nda, yönetilen kod kullanılarak oluşturulabilir. Diğer bir deyişle, bir Visual Basic sınıfta bir yöntem olarak bir saklı yordam veya UDF oluşturabilirsiniz. Bu saklı yordamlar ve UDF'ler .NET Framework ve kendi özel sınıflar işlevselliğinden yararlanmasına olanak tanır.

İnceleyeceğiz Bu öğreticide yönetilen oluşturma saklı yordamlar ve kullanıcı tanımlı işlevler ve bunları bizim Northwind veritabanına tümleştirmeyi öğreneceksiniz. Let s başlayın!

> [!NOTE]
> Yönetilen veritabanı nesnelerini SQL karşılıkları bazı avantajları sunar. Ana avantajları şunlardır: dilini zenginliği ve bilgisi ve mevcut kodunuzu ve mantığını yeniden kullanma olanağı. Ancak, yönetilen nesneleri kadar yordamsal mantığını içermeyen veri kümeleriyle çalışırken daha az verimli olması muhtemeldir. T-SQL ve yönetilen kod kullanarak bir daha kapsamlı açıklaması için avantajları hakkında kullanıma [avantajları, kullanarak yönetilen kod için veritabanı nesneleri oluşturma](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>1. adım: Northwind veritabanını taşıma`App_Data`

Bir web uygulaması s Microsoft SQL Server 2005 Express Edition veritabanı dosyasında tüm öğreticilerimizden şimdiye kadarki kullanmış `App_Data` klasör. Veritabanında yerleştirme `App_Data` Basitleştirilmiş dağıtma ve tüm dosyaları bir dizin içinde bulunan ve öğretici test etmek için hiçbir ek yapılandırma adımları gerekli olarak bu öğreticileri çalıştırmak.

Bu öğreticide, ancak let s Taşı / Northwind veritabanı `App_Data` ve açıkça ile SQL Server 2005 Express Edition veritabanı örneği kaydedin. Veritabanı Bu öğretici için şu adımları gerçekleştirebilmenize karşın `App_Data` klasör, bir dizi adımı yapılan çok daha kolay veritabanı ile SQL Server 2005 Express Edition veritabanı örneği açıkça kaydederek.

Bu öğretici için indirme sahip iki veritabanı dosyalarını - `NORTHWND.MDF` ve `NORTHWND_log.LDF` - adlı bir klasöre yerleştirilen `DataFiles`. Öğreticiler kendi uygulaması ile birlikte izliyorsanız, Visual Studio'yu kapatın ve taşıma `NORTHWND.MDF` ve `NORTHWND_log.LDF` dosyaları s Web sitesinden `App_Data` Web sitesi dışında bir klasöre klasörü. Veritabanı dosyalarını başka bir klasöre taşındıktan sonra size Northwind veritabanı ile SQL Server 2005 Express Edition veritabanı örneği kaydetmeniz gerekir. Bu SQL Server Management Studio'dan yapılabilir. Bir olmayan - Express sürümü, SQL Server yüklü 2005 varsa, daha sonra büyük olasılıkla Management Studio zaten yüklüyse. Yalnızca SQL Server 2005 Express Edition'ın bilgisayarınızda yüklü sonra indirmek ve yüklemek için birkaç dakikanızı [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

SQL Server Management Studio'yu başlatın. Şekil 1 gösterildiği gibi bağlanmak için hangi sunucu sorarak Management Studio başlatılır. Localhost\SQLExpress için sunucu adını girin, Windows kimlik doğrulaması kimlik doğrulaması aşağı açılan listeden seçin ve Bağlan'a tıklayın.


![Uygun veritabanına bağlanın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Şekil 1**: uygun veritabanı örneğine bağlanın


Nesne Gezgini penceresi, önceden bağlandıktan sonra veritabanları, güvenlik bilgileri, yönetim seçenekleri ve diğerleri dahil olmak üzere SQL Server 2005 Express Edition veritabanı örneğiyle ilgili bilgiler listelenir.

Northwind veritabanına eklemek ihtiyacımız `DataFiles` klasöre (veya her yerde, onu taşınmış olabilir) SQL Server 2005 Express Edition veritabanı örneği. Veritabanı klasörü sağ tıklatın ve bağlam menüsünden iliştirme seçeneği seçin. Bu veritabanlarını Ayır iletişim kutusunu getirir. Ekle düğmesine tıklayın, uygun aşağı ayrıntıya `NORTHWND.MDF` dosya ve Tamam'a tıklayın. Bu noktada, ekran Şekil 2'ye benzer görünmelidir.


[![Uygun veritabanına bağlanın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Şekil 2**: uygun veritabanı örneğine bağlanın ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Management Studio aracılığıyla SQL Server 2005 Express Edition örneğine bağlanırken veritabanları ekleme iletişim kutusu Belgelerim gibi kullanıcı profili dizinleri detayına izin vermez. Bu nedenle, yerleştirdiğinizden emin olun `NORTHWND.MDF` ve `NORTHWND_log.LDF` Kullanıcısız profil dizindeki dosyaları.


Veritabanı eklemek için Tamam düğmesine tıklayın. Veritabanlarını Ayır iletişim kutusu kapanır ve Nesne Gezgini artık yalnızca bağlı veritabanı listelemelisiniz. Büyük olasılıkla, Northwind veritabanı gibi bir ada sahip olan `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Veritabanı için Northwind veritabanına sağ tıklayıp Yeniden Adlandır'i seçerek yeniden adlandırın.


![İçin Northwind veritabanı yeniden adlandır](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Şekil 3**: Northwind için veritabanını yeniden adlandır


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>2. adım: Visual Studio'da yeni bir çözüm ve SQL Server projesi oluşturma

SQL Server 2005'te yönetilen saklı yordamları ve UDF'ler oluşturmak için biz UDF mantığı ve saklı yordamı bir sınıf içindeki Visual Basic kod yazacaksınız. Kod yazıldıktan sonra bu sınıf bir birleştirme dosyasına derlemek ihtiyacımız (bir `.dll` dosyası), SQL Server veritabanı ile derleme kaydedin ve ardından karşılık gelen yöntemin işaret veritabanında bir saklı yordam veya UDF nesne oluşturma derleme. Bu adımları tüm el ile gerçekleştirilebilir. Biz kodu herhangi bir metin düzenleyicisi oluşturun, Visual Basic Derleyicisi kullanarak komut satırından derleme (`vbc.exe`), veritabanı kullanarak kaydetmek [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx) Management Studio'yu komutunu veya saklı ekleyin yordam veya UDF nesne benzer bulunamazsınız. Neyse ki, Visual Studio Professional ve takım sistemleri sürümleri, bu görevleri otomatik hale getiren bir SQL Server Proje türü içerir. Bu öğreticide size yönetilen bir saklı yordam ve UDF oluşturmak için SQL Server Proje türü kullanarak size yol gösterir.

> [!NOTE]
> Visual Web Developer veya Visual Studio standart sürümünü kullanıyorsanız, el ile bir yaklaşım kullanmanız gerekir. 13. adım, bu adımları el ile gerçekleştirmek için ayrıntılı yönergeler sağlar. Adım 2 12-13. adım, kullandığınız Visual Studio'nun hangi sürümünün bağımsız olarak uygulanması gereken önemli SQL Server yapılandırma yönergeleri Bu adımlar bu yana okumadan önce okumak için öneriyoruz.


Visual Studio açarak işleme başlayın. Yeni Proje iletişim kutusu görüntülemek için yeni proje Dosya menüsünden seçin (bkz: Şekil 4) kutusunda. Veritabanı proje türüne eşleniyorsa detayına gidin ve ardından yeni bir SQL Server projesi oluşturmak sağ tarafta listelenen şablonlardan birini seçin. Bu proje adı seçmiş olduğunuz `ManagedDatabaseConstructs` ve adlı bir çözüm içinde yerleştirilen `Tutorial75`.


[![Yeni bir SQL sunucusu projesi oluşturma](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Şekil 4**: yeni bir SQL Server projesi oluşturun ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


SQL Server Proje ve çözüm oluşturmak için yeni proje iletişim kutusunda Tamam düğmesine tıklayın.

Bir SQL Server Proje belirli bir veritabanına bağlanır. Sonuç olarak, yeni SQL Server projesi oluşturduktan sonra size hemen bu bilgileri belirtmeniz istenir. Şekil 5, adım 1'deki SQL Server 2005 Express Edition veritabanı örneğinde kaydettiğimiz Northwind veritabanına işaret edecek şekilde doldurulduktan yeni veritabanı başvurusu iletişim kutusunu gösterir.


![Northwind veritabanı ile SQL Server projesi ilişkilendirmek](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Şekil 5**: Northwind veritabanı ile SQL Server projesi ilişkilendirmek


Yönetilen saklı yordamlar ve bu projede oluşturacağız UDF'ler hata ayıklamak için SQL/CLR hata ayıklama bağlantı için desteği etkinleştirmek ihtiyacımız var. (Şekil 5'te yaptığımız gibi) ile yeni bir veritabanı bir SQL Server projesi ilişkilendirme olduğunda, Visual Studio ister bize şu bağlantıda SQL/CLR hata ayıklamayı etkinleştirmek istiyorsanız (bkz. Şekil 6). Evet'e tıklayın.


![SQL/CLR hata ayıklamayı etkinleştir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Şekil 6**: SQL/CLR hata ayıklamayı etkinleştir


Bu noktada yeni SQL Server projesi çözüme eklendi. Adlı bir klasör içerir `Test Scripts` adlı bir dosya ile `Test.sql`, projede oluşturulan yönetilen nesneleri hata ayıklamak için kullanılır. Biz, adım 12'de hata ayıklama sırasında görünür.

Biz artık yeni yönetilen saklı yordamlar ve UDF'ler bu projeye ekleyebilirsiniz, ancak biz yapmasına izin vermeden önce s ilk içerir mevcut web uygulamamıza çözümde. Dosya menüsünden Ekle seçeneğini belirleyin ve mevcut Web sitesini seçin. Uygun bir Web sitesi klasörüne gözatın ve Tamam'a tıklayın. Şekil 7 gösterildiği gibi bu iki proje eklemek için çözüm güncelleştirir: Web sitesi ve `ManagedDatabaseConstructs` SQL Server projesi.


![Çözüm Gezgini, artık iki proje içerir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Şekil 7**: Çözüm Gezgini'nde artık iki proje içerir


`NORTHWNDConnectionString` Değerini `Web.config` şu anda başvuran `NORTHWND.MDF` dosyası `App_Data` klasör. Biz bu veritabanından kaldırdığımızdan `App_Data` ve açıkça kayıtlı gelenlere güncelleştirmek ihtiyacımız SQL Server 2005 Express Edition veritabanı örneğinde `NORTHWNDConnectionString` değeri. Açık `Web.config` Web sitesi ve değişiklik dosyasında `NORTHWNDConnectionString` bağlantı dizesini okur, değer: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Bu değişiklikten sonra `<connectionStrings>` konusundaki `Web.config` aşağıdakine benzer görünmelidir:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Bölümünde açıklandığı gibi [önceki öğretici](debugging-stored-procedures-vb.md), bir SQL Server nesnesine bir istemci uygulamasında hata ayıklama sırasında bir ASP.NET Web sitesi gibi biz bağlantı havuzu devre dışı bırakmanız gerekir. Yukarıda gösterilen bağlantı dizesinde bağlantı havuzu devre dışı bırakır ( `Pooling=false` ). Yönetilen saklı yordamları ve UDF'ler ASP.NET Web sitesinden hata ayıklamayı planlamıyorsanız bağlantı havuzu etkinleştirin.


## <a name="step-3-creating-a-managed-stored-procedure"></a>3. adım: saklı yordamı bir yönetilen oluşturuluyor

Yönetilen bir saklı yordam Northwind veritabanına eklemek için ilk olarak SQL Server projesi bir yöntemde saklı yordam oluşturmak ihtiyacımız var. Çözüm Gezgini'nden sağ `ManagedDatabaseConstructs` proje adı ve yeni bir öğe eklemek seçin. Bu projeye eklenen bir yönetilen veritabanı nesne türlerini listeleyen yeni öğe Ekle iletişim kutusu görüntüler. Şekil 8 gösterildiği gibi bu saklı yordamlar ve kullanıcı tanımlı işlevler, diğerlerinin yanı sıra içerir.

Yalnızca tüm yarıda kesildi ürünleri döndüren bir saklı yordamı ekleyerek başlayın s olanak tanır. Yeni saklı yordam dosya adı `GetDiscontinuedProducts.vb`.


[![GetDiscontinuedProducts.vb adlı yeni bir saklı yordam Ekle](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Şekil 8**: Yeni saklı yordam adlandırılmış ekleme `GetDiscontinuedProducts.vb` ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Aşağıdaki içeriğe bu yeni bir Visual Basic sınıf dosyası oluşturur:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Saklı yordam olarak uygulanan Not bir `Shared` yöntemi içinde bir `Partial` adlı bir sınıf dosyası `StoredProcedures`. Ayrıca, `GetDiscontinuedProducts` yöntemi ile donatılmış [ `SqlProcedure` özniteliği](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), yöntemi bir saklı yordam olarak işaretler.

Aşağıdaki kod oluşturur bir `SqlCommand` nesne ve ayarlar, `CommandText` için bir `SELECT` tüm sütunları döndüren sorgu `Products` ürünleri için ayarlanmış tablo `Discontinued` alan eşittir 1. Ardından, komutu yürütür ve sonuçları istemci uygulamasına geri gönderir. Bu kodu ekleyin `GetDiscontinuedProducts` yöntemi.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Tüm yönetilen nesneleri erişimi bir [ `SqlContext` nesne](https://msdn.microsoft.com/library/ms131108.aspx) , çağıran bağlamını temsil eder. `SqlContext` Erişim sağlayan bir [ `SqlPipe` nesne](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx) aracılığıyla kendi [ `Pipe` özelliği](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Bu `SqlPipe` nesnesi, SQL Server veritabanı ve çağıran uygulama bilgileri ferry için kullanılır. Adından da anlaşılacağı gibi [ `ExecuteAndSend` yöntemi](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) bir geçirilen içinde yürütür `SqlCommand` nesne ve sonuçlarını istemci uygulamaya gönderir.

> [!NOTE]
> Yönetilen veritabanı nesneleri saklı yordamlar ve set tabanlı mantıksal yerine yordam mantığı kullanmak UDF için uygundur. Satır temelinde veri kümeleriyle çalışmak veya skaler verilerle çalışma yordamsal mantığını içerir. `GetDiscontinuedProducts` Yöntemi biraz önce oluşturduğumuz, ancak hiçbir yordam mantığı içerir. Bu nedenle, bu ideal bir T-SQL saklı yordam uygulanması. Saklı yordamlar oluşturma ve dağıtma için gerekli adımları göstermek için yönetilen bir saklı yordam yönetilen olarak uygulanır.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>4. adım: saklı yordamı yönetilen dağıtma

Bu kod tamamlandı, Northwind veritabanına dağıtmak hazırız. Bir SQL Server projesi dağıtma bütünleştirilmiş kodu derler, derleme ile veritabanına kaydeder ve derlemedeki uygun yöntemleri bağlamak veritabanında ilgili nesneleri oluşturur. Daha kesin 13. adım dağıtma seçeneği tarafından gerçekleştirilen görevleri kümesinin sözcüklerle. Sağ `ManagedDatabaseConstructs` proje adı Çözüm Gezgini'nde ve dağıtım seçeneği belirleyin. Ancak, dağıtım şu hatayla başarısız olur: 'Dış' yakınındaki sözdizimi yanlış. Geçerli veritabanı uyumluluk düzeyi bu özelliği etkinleştirmek için daha yüksek bir değere ayarlamanız gerekebilir. Bkz. Yardım için saklı yordam `sp_dbcmptlevel`.

Bu hata iletisi Northwind veritabanı ile derleme kaydolmaya çalışılırken oluşur. Bir derlemeyi bir SQL Server 2005 veritabanı ile kaydetmek için veritabanı s uyumluluk düzeyi 90 olarak ayarlanmalıdır. Varsayılan olarak, bir uyumluluk düzeyi 90'ın yeni SQL Server 2005 veritabanlarını sahiptir. Ancak, Microsoft SQL Server 2000 kullanılarak oluşturulan veritabanları varsayılan uyumluluk düzeyi 80 ' var. Northwind veritabanı Microsoft SQL Server 2000 veritabanındaki başlangıçta olduğundan uyumluluk düzeyi 80 şu anda ayarlandı ve bu nedenle yönetilen nesneleri kaydetmek için 90'a yükseltilmiştir gerekiyor.

Veritabanı s uyumluluk düzeyini güncelleştirmek için Management Studio'da yeni bir sorgu penceresi açın ve girin:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Yukarıdaki sorguyu çalıştırmak için araç çubuğunda yürütme simgesine tıklayın.


[![Northwind veritabanı s uyumluluk düzeyini güncelleştirme](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Şekil 9**: Northwind veritabanı uyumluluk düzeyinde. s güncelleştirin ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Uyumluluk düzeyini güncelleştirdikten sonra SQL Server projeyi yeniden dağıtın. Bu süre, hatasız dağıtım tamamlamanız gerekir.

SQL Server Management Studio'ya geri dönün, nesne Gezgini'nde Northwind veritabanına sağ tıklayın ve Yenile'yi seçin. Ardından, programlama klasörüne detaya gitme ve sonra derlemeleri klasörünü genişletin. Şekil 10 gösterildiği gibi Northwind veritabanı artık tarafından oluşturulan derleme içerir `ManagedDatabaseConstructs` proje.


![Artık kayıtlı Northwind veritabanı ile ManagedDatabaseConstructs derlemesidir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Şekil 10**: `ManagedDatabaseConstructs` derlemesidir Northwind veritabanı artık kaydedildi


Ayrıca saklı yordamlar klasörünü genişletin. Adlı bir saklı yordam yok göreceğiniz `GetDiscontinuedProducts`. Bu saklı yordamı noktalarına ve dağıtım işlemi tarafından oluşturulmuş `GetDiscontinuedProducts` yönteminde `ManagedDatabaseConstructs` derleme. Zaman `GetDiscontinuedProducts` saklı yordam yürütüldüğünde, sırayla yürütür `GetDiscontinuedProducts` yöntemi. Bu yönetilen bir saklı yordam olduğundan Management Studio düzenlenemez (Bu nedenle saklı yordam adının yanında kilit simgesi).


![GetDiscontinuedProducts saklı yordam depolanan yordamları klasörde listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Şekil 11**: `GetDiscontinuedProducts` saklı yordam saklı yordamlar klasöründe listelenen


Yine de sahibiz yönetilen saklı yordamı diyoruz önce aşmak için bir daha fazla hurdle yoktur: veritabanı, yönetilen kodun yürütülmesini önlemek için yapılandırılır. Bu yeni bir sorgu penceresi açıp ve yürütme doğrulayın `GetDiscontinuedProducts` saklı yordamı. Aşağıdaki hata iletisini alırsınız: .NET Framework içinde kullanıcı kodu yürütme devre dışı bırakıldı. Clr etkin yapılandırma seçeneğini etkinleştirin.

Girin Northwind veritabanı s yapılandırma bilgilerini inceleyin ve komutu yürütmek için `exec sp_configure` sorgu penceresinde. Bu ayar etkin clr şu anda 0 olarak ayarlanırsa gösterir.


[![Clr etkin ayarı 0'dır şu anda ayarlamak için](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Şekil 12**: clr enabled ayardır şu anda ayarlamak için 0 ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Şekil 12 her bir yapılandırma ayarı ile listelenen dört değer olduğuna dikkat edin: en düşük ve en yüksek değerleri ve yapılandırma ve çalıştırma değerleri. Clr enabled'ayarı için yapılandırma değeri güncelleştirmek için aşağıdaki komutu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Yeniden çalıştırırsanız `exec sp_configure` yukarıdaki ifadeyi clr etkin ayarı s yapılandırma değeri 1 olarak güncelleştirildi, ancak çalışma değeri 0 olarak hala ayarlanmış durumda görürsünüz. Yürütülecek ihtiyacımız etkili olması bu yapılandırma değişikliğini [ `RECONFIGURE` komut](https://msdn.microsoft.com/library/ms176069.aspx), hangi ayarlar çalışma değeri için geçerli yapılandırma değeri. Girmeniz yeterlidir `RECONFIGURE` sorgu penceresinde ve araç çubuğunda yürütme simgesine tıklayın. Çalıştırırsanız `exec sp_configure` artık etkin clr ayarı s yapılandırma için 1 değerini görmek ve değerleri çalıştırın.

Clr enabled yapılandırması tamamlandı, biz yönetilen çalıştırılmaya hazır durumda `GetDiscontinuedProducts` saklı yordamı. Sorgu penceresinde girin ve bağlamını `exec` `GetDiscontinuedProducts`. Saklı yordam çağırma karşılık gelen yönetilen koda neden `GetDiscontinuedProducts` yürütmek için yöntemi. Bu kod sorunları bir `SELECT` üretilmeyen ve bu örnekte SQL Server Management Studio olan çağıran uygulama için bu verileri döndüren tüm ürünleri döndürmek için sorgu. Management Studio bu sonuçları alır ve bunları Sonuçları penceresinde görüntüler.


[![Saklı yordam tüm döndürür GetDiscontinuedProducts ürünler kullanımdan kaldırıldı](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Şekil 13**: `GetDiscontinuedProducts` depolanan yordam döndürür tüm kullanımdan ürünleri ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>5. adım: Saklı yordamları giriş parametrelerini kabul eden yönetilen oluşturma

Birçok sorguları ve saklı yordamlar oluşturduğumuz Bu öğretici kullanılan *parametreleri*. Örneğin, [oluşturma yeni saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) adlı bir saklı yordam oluşturduğumuz öğretici `GetProductsByCategoryID` adlı giriş parametresi kabul `@CategoryID`. Saklı yordam sonra tüm ürünleri döndürülen olan `CategoryID` alanı, sağlanan değeri eşleşen `@CategoryID` parametresi.

Giriş parametrelerini kabul eden yönetilen bir saklı yordam oluşturmak için bu parametreleri s yöntem tanımında belirtin. Let s Bunu açıklamak üzere yönetilen başka bir saklı yordama ekleme `ManagedDatabaseConstructs` adlı proje `GetProductsWithPriceLessThan`. Yönetilen Bu saklı yordamı bir fiyat belirten bir giriş parametresi kabul eder ve tüm ürünleri döndürür, `UnitPrice` alan parametresi s değerinden daha küçük olur.

Yeni bir saklı yordam projeye eklemek için sağ `ManagedDatabaseConstructs` proje adı ve yeni bir saklı yordam eklemek seçin. Dosya adı `GetProductsWithPriceLessThan.vb`. Adım 3'te gördüğümüz gibi bu adlı bir yöntem ile yeni bir Visual Basic sınıf dosyası oluşturur `GetProductsWithPriceLessThan` içinde yerleştirilen `Partial` sınıfı `StoredProcedures`.

Güncelleştirme `GetProductsWithPriceLessThan` kabul edecek şekilde s yöntemi tanımı bir [ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx) adlı giriş parametresi `price` ve yürütün ve sorgu sonuçlarını döndürmek için kodu yazın:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

`GetProductsWithPriceLessThan` s yöntem tanımını ve kod çok benzeyen tanımı ve kodu `GetDiscontinuedProducts` 3. adımda oluşturulan yöntemi. Yalnızca farklar şunlardır `GetProductsWithPriceLessThan` yöntemi giriş parametresi olarak kabul eder (`price`), `SqlCommand` s sorgu içeren bir parametre (`@MaxPrice`), ve bir parametre eklendiğinden `SqlCommand` s `Parameters` koleksiyonudur ve değerini atanan `price` değişkeni.

Bu kodu ekledikten sonra SQL Server projeyi yeniden dağıtın. Ardından, SQL Server Management Studio'ya geri dönün ve saklı yordamlar klasör yenilenemedi. Yeni bir giriş görmeniz gerekir `GetProductsWithPriceLessThan`. Bir sorgu penceresi girin ve bağlamını `exec GetProductsWithPriceLessThan 25`, Şekil 14 gösterildiği gibi 25 ABD Doları, küçüktür tüm ürünler listesi olur.


[![25 ABD Doları ürünleri altında görüntülenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Şekil 14**: 25 ABD Doları ürünleri altında görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>6. adım: veri erişim katmanından yönetilen saklı yordamı çağırma

Bu noktada ekledik `GetDiscontinuedProducts` ve `GetProductsWithPriceLessThan` saklı yordamlar için yönetilen `ManagedDatabaseConstructs` proje ve bunları Northwind SQL Server veritabanıyla kaydettiniz. Biz de bu yönetilen saklı yordamlar SQL Server Management Studio'dan çağrılır (Şekil 13 ve 14 bakın). Bizim ASP.NET sırada saklı yordamlar yönetilen uygulama bunları kullanmak için ancak biz bunları veri erişimi ve iş mantığı katman mimarisinde eklemeniz gerekir. Bu adımda için iki yeni yöntemler ekleyeceğiz `ProductsTableAdapter` içinde `NorthwindWithSprocs` başlangıçta oluşturulan türü belirtilmiş DataSet [oluşturma yeni saklı yordamlar için türü belirtilmiş veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) öğretici. Adım 7'de BLL için karşılık gelen yöntemlere ekleyeceğiz.

Açık `NorthwindWithSprocs` Visual Studio ve yeni bir yöntemine ekleyerek başlangıç türü belirtilmiş veri kümesi `ProductsTableAdapter` adlı `GetDiscontinuedProducts`. Bir TableAdapter bağdaştırıcısına yeni bir yöntem eklemek, Tasarımcısı'nda TableAdapter s adına sağ tıklayın ve bağlam menüsünden Sorgu Ekle seçeneğini seçin.

> [!NOTE]
> Northwind veritabanından geçtiğimizi beri `App_Data` klasörü SQL Server 2005 Express Edition veritabanı örneği için karşılık gelen bağlantı dizesini Web.config dosyasında bu değişikliği yansıtacak şekilde güncelleştirilmesi gerekir. 2. adımda güncelleştirme ele almıştık `NORTHWNDConnectionString` değerini `Web.config`. Bu güncelleştirme yapmak unuttuysanız, hata iletisi sorgu ekleme başarısız görürsünüz. Bağlantı bulunamıyor `NORTHWNDConnectionString` nesne için `Web.config` TableAdapter bağdaştırıcısına yeni bir yöntem eklemeye çalışırken bir iletişim kutusunda. Bu hatayı gidermek için Tamam'a tıklayın ve ardından Git `Web.config` ve güncelleştirme `NORTHWNDConnectionString` değerini 2. adımda açıklandığı gibi. Ardından yöntem için TableAdapter'ı yeniden eklemeyi deneyin. Bu süre hatasız çalışmalıdır.


Yeni bir yöntem eklemeyi, birden çok kez son öğreticilerde kullandık TableAdapter sorgu Yapılandırma Sihirbazı başlatılır. İlk adım bize TableAdapter veritabanına nasıl erişmeli belirtmenizi ister: yeni veya mevcut bir saklı yordam aracılığıyla veya geçici SQL deyimi. Biz önceden oluşturduğunuz kayıtlı ve bu yana `GetDiscontinuedProducts` veritabanı ile yönetilen bir saklı yordam seçin saklı yordamı seçeneği ve sonraki isabet var olanı kullan.


[![Saklı yordam seçeneği mevcut kullanımı seçin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Şekil 15**: saklı yordamı seçeneği var olanı Kullan'ı seçin ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


Sonraki ekranda, bize yöntemi çağırır saklı yordam için ister. Seçin `GetDiscontinuedProducts` aşağı açılan listeden saklı yordam yönetilen ve sonraki basın.


[![GetDiscontinuedProducts yönetilen saklı yordam seçin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Şekil 16**: seçin `GetDiscontinuedProducts` saklı yordam yönetilen ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


Biz ardından saklı yordam satır, tek bir değer veya hiçbir şey döndürüp döndürmediğini belirtmeniz istenir. Bu yana `GetDiscontinuedProducts` kümesi döndürür artık sağlanmayan ürün satırların, ilk (Tablosal veri) seçeneğini belirleyin ve İleri'ye tıklayın.


[![Tablosal veri seçeneğini belirleyin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Şekil 17**: Tablosal veri seçeneğini belirleyin ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


Son sihirbaz ekranda kullanılan veri erişim desenlerini ve sonuçta elde edilen yöntemlerin adlarını belirtmek sağlıyor. Hem işaretli hem de ad yöntemleri bırakın `FillByDiscontinued` ve `GetDiscontinuedProducts`. Sihirbazı tamamlamak için Son'u tıklatın.


[![Ad yöntemleri FillByDiscontinued ve GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Şekil 18**: yöntemleri ad `FillByDiscontinued` ve `GetDiscontinuedProducts` ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Adlı bir yöntem oluşturmak için bu adımları yineleyin `FillByPriceLessThan` ve `GetProductsWithPriceLessThan` içinde `ProductsTableAdapter` için `GetProductsWithPriceLessThan` saklı yordam yönetilir.

Şekil 19 yöntemlere ekledikten sonra veri kümesi Tasarımcısı'nın ekran gösterir `ProductsTableAdapter` için `GetDiscontinuedProducts` ve `GetProductsWithPriceLessThan` saklı yordamlar yönetilen.


[![Bu adımda eklenen yeni yöntemler düzenleyen içerir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Şekil 19**: `ProductsTableAdapter` Bu adımda eklenen yeni yöntemler içerir ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>7. adım: İş mantığı katmanı için karşılık gelen yöntemlere ekleme

Adım 4 ve 5'te eklenmiş yönetilen saklı yordamları çağırma yöntemleri eklemek için veri erişim katmanı güncelleştirdik, biz iş mantığı katmanı için karşılık gelen yöntemlere eklemeniz gerekir. Aşağıdaki iki yöntemi ekleme `ProductsBLLWithSprocs` sınıfı:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Her iki yöntem de yalnızca karşılık gelen DAL yöntemini çağırın ve dönüş `ProductsDataTable` örneği. `DataObjectMethodAttribute` Her yöntem yukarıda biçimlendirme ObjectDataSource s Confgure veri kaynağı Sihirbazı'nı seçin sekmesindeki açılan liste dahil edilmesi için bu yöntemleri neden olur.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>8. adım: saklı yordamları sunu katmanını gelen yönetilen çağırma

Arama için destek eklemek için iş mantığı ve veri erişim katmanları ile Genişletilmiş `GetDiscontinuedProducts` ve `GetProductsWithPriceLessThan` saklı yordamlar, yönetilen biz artık bu görüntüleyebilirsiniz depolanan yordamları sonuçları ASP.NET sayfası aracılığıyla.

Açık `ManagedFunctionsAndSprocs.aspx` sayfasını `AdvancedDAL` klasörü ve araç kutusundan GridView tasarımcının üzerine sürükleyin. GridView s ayarlamak `ID` özelliğini `DiscontinuedProducts` ve isteğe bağlı olarak, akıllı etiketten adlı yeni bir ObjectDataSource bağlama `DiscontinuedProductsDataSource`. ObjectDataSource kendi verileri çekmek için yapılandırma `ProductsBLLWithSprocs` s sınıfı `GetDiscontinuedProducts` yöntemi.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Şekil 20**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Aşağı açılan listeden seçme sekmesinde GetDiscontinuedProducts yöntemini seçin.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Şekil 21**: seçin `GetDiscontinuedProducts` yöntemi seçin sekmeyi aşağı açılan listeden ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Bu kılavuz yalnızca görünen ürün bilgileri için kullanılacağından, güncelleştirme, ekleme, açılan listeler ayarlayın ve sekmeleri (hiçbiri) SİLİN ve ardından Son'a tıklayın.

Sihirbazı tamamladığınızda, Visual Studio otomatik olarak BoundField veya CheckBoxField her veri alanı için ekler `ProductsDataTable`. Tüm dışında bu alanlardan kaldırmak için bir dakikanızı ayırın `ProductName` ve `Discontinued`, hangi, GridView gelin ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Sayfa, ziyaret edilen, ObjectDataSource çağrıları `ProductsBLLWithSprocs` s sınıfı `GetDiscontinuedProducts` yöntemi. Adım 7'de gördüğümüz gibi bu yöntem için DAL s çağırır `ProductsDataTable` s sınıfı `GetDiscontinuedProducts` çağıran yöntem `GetDiscontinuedProducts` saklı yordamı. Bu saklı yordam, yönetilen bir saklı yordam ve artık üretilmeyen ürünler döndüren adım 3'te oluşturduğumuz kodu yürütür ' dir.

Yönetilen bir saklı yordam tarafından döndürülen sonuçları halinde paketlenmiş bir `ProductsDataTable` DAL tarafından ve ardından bunları sunu nerede bunlar GridView'a bağlı ve görüntülenen katmana döndürür BLL döndürülen. Beklendiği gibi kılavuz yarıda kesildi Bu ürünleri listeler.


[![Kullanımdan ürünler listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Şekil 22**: kullanımdan ürünleri listelenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Daha fazla uygulama için bir metin kutusu ve başka bir GridView sayfaya ekleyin. Ürünleri miktarından daha küçük çağırarak aşağıdaki metin kutusuna girilen görüntüler bu GridView sahip `ProductsBLLWithSprocs` s sınıfı `GetProductsWithPriceLessThan` yöntemi.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>9. adım: Oluşturma ve T-SQL UDF'ler çağırma

Kullanıcı tanımlı işlevleri veya UDF'ler, programlama dillerindeki işlevleri semantiği yakından mimic veritabanı nesnelerini olur. Visual Basic'te bir işlev gibi UDF'ler değişken sayıda giriş parametrelerini içerir ve belirli bir tür değeri döndürür. Bir UDF skaler verileri - bir dize, bir tamsayı ve benzeri - veya tablo veri döndürebilir. Her iki UDF'ler skalar veri türü döndüren bir UDF ile başlayarak, tür hızlı göz atın s olanak tanır.

Aşağıdaki UDF belirli bir ürün için Stok tahmini değerini hesaplar. Üç giriş parametreleri - alma olarak bunu yapar `UnitPrice`, `UnitsInStock`, ve `Discontinued` türünde bir değer döndürür ve değerleri için belirli bir ürün - `money`. Çarpılarak Envanter tahmini değerini hesaplar `UnitPrice` tarafından `UnitsInStock`. Kullanımdan kaldırılan öğeler için bu değer miktarı yarıya iner.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Bu UDF veritabanına eklendikten sonra şu Management Studio programlama klasör sonra işlevleri ve skaler değerli işlevleri genişleterek bulunabilir. İçinde kullanılabilir bir `SELECT` sorgu şu şekilde:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Eklenen `udf_ComputeInventoryValue` Northwind veritabanına; UDF Şekil 23 yukarıdaki çıktısını gösterir `SELECT` Management Studio görüntülendiğinde sorgulayın. Ayrıca, UDF Nesne Gezgini skaler değerli işlevleri klasöründe listelendiğine dikkat edin.


[![Her bir ürün stok değerlerini s listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Şekil 23**: her ürün s Envanter değerleri listelenmektedir ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


UDF ayrıca tablo veri döndürebilir. Örneğin, belirli bir kategoriye ait ürünleri döndüren bir UDF oluşturabiliriz:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF kabul eden bir `@CategoryID` giriş parametresi ve belirtilen sonuçları döndürür `SELECT` sorgu. İçinde bir kez oluşturduktan sonra bu UDF başvurulabilir `FROM` (veya `JOIN`) yan tümcesi bir `SELECT` sorgu. Aşağıdaki örnek döndürecekti `ProductID`, `ProductName`, ve `CategoryID` her İçecekler için değerler.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Eklenen `udf_GetProductsByCategoryID` Northwind veritabanına; UDF Şekil 24 yukarıdaki çıktısını gösterir `SELECT` Management Studio görüntülendiğinde sorgulayın. Tablo verisi döndüren UDF'ler Nesne Gezgini s tablo değerli işlevler klasöründe bulunabilir.


[![ProductID, ProductName ve CategoryID için her içecek listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Şekil 24**: `ProductID`, `ProductName`, ve `CategoryID` için her içecek listelenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Oluşturma ve UDF'ler kullanma hakkında daha fazla bilgi için kullanıma [kullanıcı tanımlı işlevlere giriş](http://www.sqlteam.com/item.asp?ItemID=1955). Ayrıca kullanıma [avantajları ve Drawbacks of User-Defined işlevleri](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>10. adım: yönetilen bir UDF oluşturma

`udf_ComputeInventoryValue` Ve `udf_GetProductsByCategoryID` Yukarıdaki örneklerde oluşturulan UDF'ler olan T-SQL veritabanı nesneleri. SQL Server 2005 da eklenebilir yönetilen UDF'leri destekler `ManagedDatabaseConstructs` yalnızca yönetilen adımları 3 saklı yordamlar ve 5 gibi proje. Bu adım için uygulama s izin `udf_ComputeInventoryValue` yönetilen kodda UDF.

Yönetilen bir UDF için eklenecek `ManagedDatabaseConstructs` proje, Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve yeni bir öğe eklemek seçin. Yeni Öğe Ekle iletişim kutusundan kullanıcı tanımlı şablonu seçin ve yeni UDF dosya adı `udf_ComputeInventoryValue_Managed.vb`.


[![Yeni bir yönetilen UDF ManagedDatabaseConstructs projeye ekleyin.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Şekil 25**: yeni bir yönetilen UDF ekleme `ManagedDatabaseConstructs` proje ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


Kullanıcı tanımlı işlev şablonu oluşturur bir `Partial` adlı sınıfı `UserDefinedFunctions` sınıfı s dosyası adıyla aynı adı olan bir yöntem (`udf_ComputeInventoryValue_Managed`, bu örnekte). Bu yöntemi kullanarak düzenlenmiş [ `SqlFunction` özniteliği](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), yöntemi bir yönetilen UDF olarak işaretler.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

`udf_ComputeInventoryValue` Yöntem şu anda döndürür bir [ `SqlString` nesne](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx) ve herhangi bir giriş parametreleri kabul etmez. Üç giriş parametreleri - kabul ettiği yöntem tanımını güncelleştirmeye ihtiyacımız `UnitPrice`, `UnitsInStock`, ve `Discontinued` - ve döndüren bir `SqlMoney` nesne. T-SQL ile aynı stok değerini hesaplamak için mantığı `udf_ComputeInventoryValue` UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

UDF s yöntemi giriş parametrelerini kendi karşılık gelen SQL türleri olduğunu unutmayın: `SqlMoney` için `UnitPrice` alanı [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx) için `UnitsInStock`, ve [ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx) için `Discontinued`. Bu veri türleri içinde tanımlanan türleri yansıtacak `Products` tablosu: `UnitPrice` sütun türüdür `money`, `UnitsInStock` sütun türü `smallint`ve `Discontinued` sütun türü `bit`.

Kod oluşturarak başlıyor bir `SqlMoney` adlı örnek `inventoryValue` 0 değeri atanır. `Products` Tablosu için veritabanı verir `NULL` değerler `UnitsInPrice` ve `UnitsInStock` sütun. Bu nedenle, ilk denetlemek için bu değerleri içerip içermediğini görmek üzere ihtiyacımız `NULL` aracılığıyla yaptığımız s, `SqlMoney` s nesnesi [ `IsNull` özelliği](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx). Her iki `UnitPrice` ve `UnitsInStock` içeren olmayan`NULL` değerleri daha sonra şu işlem `inventoryValue` iki ürün olacak. Ardından, eğer `Discontinued` biz değeri edilmesiyle yarıya sonra true olur.

> [!NOTE]
> `SqlMoney` Nesne yalnızca sağlayan iki `SqlMoney` birlikte çarpılmasına örnekleri. Project'in izin vermediği bir `SqlMoney` değişmez bir kayan noktalı sayı ile çarpılmasına örneği. Bu nedenle, edilmesiyle yarıya için `inventoryValue` biz tarafından yeni bir Çarp `SqlMoney` 0,5 değeri olan bir örneği.


## <a name="step-11-deploying-the-managed-udf"></a>11. adım: yönetilen UDF dağıtma

Artık yönetilen UDF oluşturulduktan sonra size Northwind veritabanına dağıtmaya hazırsınız. Adım 4'te gördüğümüz gibi yönetilen nesneler bir SQL Server projesindeki Çözüm Gezgini'nde proje adının üzerine sağ tıklayın ve bağlam menüsünden dağıtma seçeneği seçerek dağıtılır.

Proje dağıttıktan sonra SQL Server Management Studio'ya geri dönün ve skaler değerli işlevleri klasör yenilenemedi. Şimdi iki girişleri görmeniz gerekir:

- `dbo.udf_ComputeInventoryValue` -T-SQL UDF adım 9'da oluşturulan ve
- `dbo.udf ComputeInventoryValue_Managed` -Adım 10'yalnızca dağıtılmış yönetilen UDF oluşturulur.

Bu yönetilen UDF test etmek için Management Studio içinden gelen aşağıdaki sorguyu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Bu komut, yönetilen kullanır `udf ComputeInventoryValue_Managed` UDF T-SQL yerine `udf_ComputeInventoryValue` UDF, ancak çıkış aynıdır. Geri UDF s çıktının bir ekran görüntüsünü görmek için Şekil 23 bakın.

## <a name="step-12-debugging-the-managed-database-objects"></a>12. adım: yönetilen bir veritabanı nesnelerinde hata ayıklama

İçinde [saklı yordamlar hata ayıklama](debugging-stored-procedures-vb.md) ele aldığımız üç öğretici, Visual Studio aracılığıyla SQL Server hata ayıklama için seçenekleri: doğrudan veritabanı hata ayıklama, uygulamada hata ayıklama ve hata ayıklama bir SQL Server projeden. Bir istemci uygulaması ve SQL Server projesi doğrudan yönetilen veritabanı nesneleri doğrudan veritabanı hata ayıklama ile hata ayıklaması yapılamaz, ancak hata ayıklaması yapılabilir. Çalışmak için hata ayıklama için sırada ancak SQL Server 2005 veritabanını SQL/CLR hata ayıklama izin vermeniz gerekir. İlk oluşturduğunuz sırada, geri çağırma `ManagedDatabaseConstructs` proje Visual Studio sorulan bize SQL/CLR (bkz. Şekil 6 2. adım) hata ayıklama sağlamak istedik. Sunucu Gezgini penceresini veritabanından sağ tıklayarak, bu ayar değiştirilebilir.


![Veritabanı'nın SQL/CLR hata ayıklama izin verdiğinden emin olun](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Şekil 26**: Veritabanı'nın SQL/CLR hata ayıklama izin verdiğinden emin olun


Hata ayıklamak istedik Imagine `GetProductsWithPriceLessThan` saklı yordam yönetilir. Biz kod içinde bir kesme noktası ayarlayarak başlar `GetProductsWithPriceLessThan` yöntemi.


[![GetProductsWithPriceLessThan yöntemde bir kesme noktası ayarlayın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Şekil 27**: bir kesim noktası `GetProductsWithPriceLessThan` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


Yönetilen veritabanı nesnelerini SQL Server projenin hata ayıklama sırasında ilk bakış s olanak tanır. İki proje - Çözümümüzü içerdiğinden `ManagedDatabaseConstructs` ihtiyacımız başlatmak için Visual Studio istemek için SQL Server projesinde hata ayıklama için Web sitemizi - yanı sıra SQL Server projesi `ManagedDatabaseConstructs` SQL Server hata ayıklaması başlattığınızda, proje. Sağ `ManagedDatabaseConstructs` Çözüm Gezgini'nde proje ve bağlam menüsünden seçeneği başlangıç projesi olarak Ayarla'ı seçin.

Zaman `ManagedDatabaseConstructs` SQL deyimlerinde yürütülmeden Hata Ayıklayıcı'daki proje başlatıldığında `Test.sql` bulunan dosya `Test Scripts` klasör. Örneğin, test etmek için `GetProductsWithPriceLessThan` saklı yordam, yönetilen varolan `Test.sql` dosya içeriği çağıran aşağıdaki deyimi `GetProductsWithPriceLessThan` öğesinde geçen bir saklı yordam yönetilen `@CategoryID` 14.95 değeri:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Yukarıdaki betiğe önceden girilen sonra `Test.sql`, hata ayıklama menüsüne giderek ve hata ayıklamayı Başlat'ı seçerek veya F5'e basarak hata ayıklamaya başlayın veya araç çubuğundaki yeşil play simgesi. Bu çözüm içindeki projeleri derlemek, yönetilen nesneleri Northwind veritabanına dağıtın ve ardından yürütme `Test.sql` betiği. Bu noktada, kesme noktasına isabet ve size adım adım `GetProductsWithPriceLessThan` yöntemi, giriş parametrelerinin değerlerini inceleyin ve benzeri.


[![GetProductsWithPriceLessThan yönteminde kesme noktalarına isabet Ettirilmedi](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Şekil 28**: kesme `GetProductsWithPriceLessThan` yöntemi isabet aldı ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Bir SQL veritabanı nesnesi bir istemci uygulaması hata ayıklaması için sırada veritabanı uygulamasında hata ayıklama desteklemek için yapılandırılması zorunludur. Sunucu Gezgini veritabanı üzerinde sağ tıklayın ve uygulama hata ayıklama seçeneğinin işaretlendiğinden emin olun. Ayrıca, SQL hata ayıklayıcısı ile tümleştirin ve bağlantı havuzu devre dışı bırakmak için ASP.NET uygulaması gerekir. Bu adımları ayrıntılı adım 2'de bahsedilen [saklı yordamlar hata ayıklama](debugging-stored-procedures-vb.md) öğretici.

ASP.NET uygulaması ve veritabanı yapılandırdıktan sonra ASP.NET Web sitesi başlangıç projesi olarak ayarlayın ve hata ayıklamaya başlayın. Bir kesme noktası olan yönetilen nesneleri birini çağıran bir sayfayı ziyaret ederse, uygulamanın durdurulur ve denetimi için hata ayıklayıcı, burada şekil 28'de gösterildiği gibi kod boyunca gezinebilirsiniz kapatılacak.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>13. adım: Derleme ve dağıtma el ile yönetilen veritabanı nesneleri

SQL Server projeleri oluşturmak, derlemek ve yönetilen veritabanı nesneleri dağıtmak kolaylaştırır. Ne yazık ki, SQL Server projeleri yalnızca Visual Studio Professional ve takım sistemleri sürümlerinde kullanılabilir. Visual Web Developer ya da Standard Edition, Visual Studio kullanarak ve yönetilen veritabanı nesnelerini kullanmak istiyorsanız, el ile oluşturmanız ve dağıtmanız gerekir. Bu dört adımdan oluşur:

1. Yönetilen veritabanı nesnesi için kaynak kodu içeren bir dosya oluşturun
2. Nesneyi bir araya derlemek,
3. SQL Server 2005 veritabanı ile bütünleştirilmiş kodunu kaydetme ve
4. Derlemedeki uygun yöntemi işaret eden bir SQL Server veritabanı nesnesi oluşturun.

Bu görevleri göstermek, yeni bir s izin vermek için bu ürünlerin döndüren saklı yordamı yönetilen olan `UnitPrice` belirtilen değerden büyüktür. Bilgisayarınızda adlı yeni bir dosya oluşturun `GetProductsWithPriceGreaterThan.vb` ve dosyaya aşağıdaki kodu girin (Visual Studio, Not Defteri'ni veya metin düzenleyicilere bunu gerçekleştirmek için kullanabilirsiniz):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Bu kod olarak neredeyse aynı `GetProductsWithPriceLessThan` 5. adımda oluşturulan yöntemi. Yöntem adları, tek fark vardır `WHERE` yan tümcesi ve sorguda kullanılan parametre adı. Geri `GetProductsWithPriceLessThan` yöntemi `WHERE` yan tümcesi okuyun: `WHERE UnitPrice < @MaxPrice`. Burada, `GetProductsWithPriceGreaterThan`, kullanıyoruz: `WHERE UnitPrice > @MinPrice` .

Artık bu sınıf bir birleştirme dosyasına derlemek ihtiyacımız var. Komut satırından kaydettiğiniz dizine gidin `GetProductsWithPriceGreaterThan.vb` dosya ve C# derleyicisi kullanın (`csc.exe`) sınıfı dosyanın bir birleştirme dosyasına derlemek için:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Varsa v içeren klasör `bc.exe` içinde sistemde s `PATH`, tam yolunu da başvurmak zorunda `%WINDOWS%\Microsoft.NET\Framework\version\`, şu şekilde:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![Bir derleme içine GetProductsWithPriceGreaterThan.vb derleyin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Şekil 29**: derleme `GetProductsWithPriceGreaterThan.vb` içine bir derleme ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


`/t` Bayrak, Visual Basic sınıf dosyasının bir DLL (bir yürütülebilir dosya yerine) derlenmesi gerektiğini belirtir. `/out` Bayrak, sonuçta elde edilen derlemenin adını belirtir.

> [!NOTE]
> Derleme yerine `GetProductsWithPriceGreaterThan.vb` sınıf dosyası alternatif olarak kullanabilirsiniz komut satırından [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) veya Visual Studio standart sürüm ayrı bir sınıf kitaplığı projesi oluşturun. S ren Jacob Lauritsen böyle bir Visual Basic Express Edition projesi için kod ile genişliğinizin sağlanan `GetProductsWithPriceGreaterThan` saklı yordam ve iki yönetilen saklı yordamlar ve UDF oluşturulan adım 3, 5 ve 10. S ren s proje karşılık gelen veritabanı nesneleri eklemek için gereken T-SQL komutlarını da içerir.


Bir bütünleştirilmiş kod içine derlenmiş kod ile derleme içinde SQL Server 2005 veritabanını kaydetmek hazırız. Bu T-komutunu kullanarak SQL gerçekleştirilebilir `CREATE ASSEMBLY`, ya da SQL Server Management Studio aracılığıyla. S odak Management Studio'yu kullanarak odaklanmasına imkan sağlar.

Management Studio'da, Northwind veritabanındaki programlama klasörünü genişletin. Alt klasörünü derlemeleri biridir. Yeni bir derleme için veritabanını el ile eklemek için derlemeleri klasörü sağ tıklatın ve bağlam menüsünden Yeni bir derleme seçin. Yeni derleme iletişim kutusu (bkz. Şekil 30) bu görüntüler. Select göz at düğmesine tıklayarak `ManuallyCreatedDBObjects.dll` derleme biz yalnızca derlenmiş ve derleme eklemek için Tamam'a tıklayın. Değil görmelisiniz `ManuallyCreatedDBObjects.dll` nesne Gezgini'nde derleme.


[![Veritabanına ManuallyCreatedDBObjects.dll derleme ekleme](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Şekil 30**: ekleme `ManuallyCreatedDBObjects.dll` veritabanı derlemesine ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![Nesne Gezgini'nde ManuallyCreatedDBObjects.dll listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Şekil 31**: `ManuallyCreatedDBObjects.dll` nesne Gezgini'nde listelenir


Derleme Northwind veritabanına eklenmiş olsa da bir saklı yordamı ile ilişkilendirmek henüz `GetProductsWithPriceGreaterThan` derlemesindeki yöntemi. Bunu gerçekleştirmek için yeni bir sorgu penceresi açın ve aşağıdaki komutu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Bu Northwind veritabanında adlı yeni bir saklı yordam oluşturur `GetProductsWithPriceGreaterThan` ve yönetilen yöntemi ile ilişkilendirir `GetProductsWithPriceGreaterThan` (sınıf içinde `StoredProcedures`, derleme içinde olduğu `ManuallyCreatedDBObjects`).

Yukarıdaki betik yürütüldükten sonra nesne Gezgini'ndeki saklı yordamlar klasörü yenileyin. Yeni bir saklı yordam giriş - görmelisiniz `GetProductsWithPriceGreaterThan` -yanında kilit simgesi vardır. Bu saklı yordamı test etmek için girin ve sorgu penceresinde aşağıdaki komutu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Şekil 32 gösterildiği gibi yukarıdaki komutu ile bu ürünlere yönelik bilgileri görüntüler. bir `UnitPrice` $24.95 büyüktür.


[![Nesne Gezgini'nde ManuallyCreatedDBObjects.dll listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Şekil 32**: `ManuallyCreatedDBObjects.dll` nesne Gezgini'nde listelenir ([tam boyutlu görüntüyü görmek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Özet

Microsoft SQL Server 2005 ile ortak dil çalışma zamanı (veritabanı nesnelerinin yönetilen kod kullanarak oluşturulmasına olanak tanıyan CLR), tümleştirme sağlar. Daha önce bu nesneleri yalnızca T-SQL kullanarak oluşturulabilir, ancak artık Visual Basic gibi programlama dillerinde .NET kullanarak bu nesneleri oluşturabiliriz. Oluşturduğumuz Bu öğreticide iki saklı yordamlar ve yönetilen bir kullanıcı tanımlı işlev yönetilen.

Visual Studio s SQL Server Proje türü, oluşturma, derleme ve yönetilen veritabanı nesneleri dağıtımı kolaylaştırır. Ayrıca, zengin hata ayıklama desteği sunar. Ancak, SQL Server Proje türleri yalnızca Visual Studio Professional ve takım sistemleri sürümlerinde kullanılabilir. 13. adım gördüğümüz gibi olanlar için Visual Web Developer veya Standard Edition, Visual Studio, oluşturma, derleme ve dağıtım adımlarını kullanarak el ile gerçekleştirilmesi gerekir.

Mutlu programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Artıları ve eksileri kullanıcı tanımlı işlevler](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Yönetilen kodda SQL Server 2005 nesneleri oluşturma](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [SQL Server 2005'te yönetilen kod kullanarak Tetikleyicileri oluşturma](http://www.15seconds.com/issue/041006.htm)
- [Nasıl yapılır: Oluşturma ve çalıştırma CLR SQL Server saklı yordam](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [Nasıl yapılır: Oluşturma ve kullanıcı tanımlı bir CLR SQL Server işlevi çalıştırın](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [Nasıl yapılır: Düzenle `Test.sql` SQL nesneleri çalıştırılacak betik](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [Giriş kullanıcı tanımlı işlevler](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Yönetilen kod ve SQL Server 2005 (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Transact-SQL Başvurusu](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [İzlenecek yol: yönetilen kodda bir saklı yordam oluşturma](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme S ren Jacob Lauritsen oluştu. Bu makalede, S olan ren da gözden yanı sıra yönetilen veritabanı nesnelerini el ile derlemek için bu makalede s indirme dahil Visual C# Express Edition projesi oluşturdunuz. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](debugging-stored-procedures-vb.md)
