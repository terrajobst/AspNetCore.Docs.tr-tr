---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: "Yönetilen kod (VB) saklı yordamları ve kullanıcı tanımlı işlevler ile oluşturma | Microsoft Docs"
author: rick-anderson
description: "Microsoft SQL Server 2005 .NET ortak dil veritabanı nesnelerini aracılığıyla yönetilen kod geliştiricilerin izin vermek için çalışma zamanı ile tümleştirir. Bu öğretici..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: efec52c4085c24b1d6227a86f7c435ca657e493c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Saklı yordamlar ve yönetilen kod (VB) ile kullanıcı tanımlı işlevler oluşturma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) veya [PDF indirin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 .NET ortak dil veritabanı nesnelerini aracılığıyla yönetilen kod geliştiricilerin izin vermek için çalışma zamanı ile tümleştirir. Bu öğretici, yönetilen saklı yordamlar oluşturulacağını gösterir ve kullanıcı tanımlı işlevler, Visual Basic veya C# kod ile yönetilir. Visual Studio'nun bu sürümleri, bu tür yönetilen veritabanı nesneleri hata ayıklamak nasıl izin Ayrıca bkz.


## <a name="introduction"></a>Giriş

Veritabanları Microsoft s SQL Server 2005 gibi kullanmak [Transact-Structured sorgu dili (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) ekleme, değiştirme ve veriler alınıyor. Çoğu veritabanı sistemi daha sonra yeniden kullanılabilir, tek bir birim olarak yürütülen SQL deyimlerini bir dizi gruplandırmak için yapılar içerir. Saklı yordamlar bir örnektir. Başka bir *kullanıcı tanımlı işlevler*(UDF'ler) daha ayrıntılı adım 9'daki inceleyeceğiz bir yapı.

Özünde, SQL veri kümeleriyle çalışmak üzere tasarlanmıştır. `SELECT`, `UPDATE`, Ve `DELETE` deyimleri kendiliğinden karşılık gelen tablosundaki tüm kayıtlar için geçerlidir ve tarafından yalnızca sınırlı kendi `WHERE` yan tümceleri. Henüz bir kayıtla aynı anda çalışmak ve skaler verileri işlemek için tasarlanmış birçok dil özellikleri vardır. [`CURSOR`s](http://www.sqlteam.com/item.asp?ItemID=553) bir kayıt kümesi için birer birer aracılığıyla Döngüdeki olmasına izin vermektedir. Dize işleme işlevleri gibi `LEFT`, `CHARINDEX`, ve `PATINDEX` skaler verileri ile çalışabilir. SQL de içeren denetim akışı ifadeleri gibi `IF` ve `WHILE`.

Microsoft SQL Server 2005 önce saklı yordamları ve UDF'leri yalnızca T-SQL deyimlerini koleksiyonu olarak tanımlanabilir. SQL Server 2005, ancak ile tümleştirme sağlamak üzere tasarlanmış [ortak dil çalışma zamanı (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx), tüm .NET derlemelerini tarafından kullanılan çalışma zamanı olduğu. Sonuç olarak, bir SQL Server 2005 veritabanında UDF'ler ve saklı yordamlar, yönetilen kod kullanılarak oluşturulabilir. Diğer bir deyişle, bir Visual Basic sınıfında bir yöntem olarak bir saklı yordam veya UDF oluşturabilirsiniz. Bu, bu saklı yordamları ve .NET Framework ve kendi özel sınıflardan faydalanmak için UDF'ler sağlar.

İnceleyeceğiz Bu öğreticide yönetilen oluşturma, yordamlar ve kullanıcı tanımlı işlevler ve Northwind Veritabanımıza tümleştirmek nasıl depolanır. Let s başlayın!

> [!NOTE]
> Yönetilen veritabanı nesneleri SQL dekiler bazı avantajları sunar. Dil zenginliğini ve benzerlik ve var olan kodu ve mantığını yeniden kullanma olanağı ana avantajları şunlardır. Ancak yönetilen veritabanı nesnelerini kadar yordamsal mantığını içermeyen veri kümeleriyle çalışırken daha az verimli olması muhtemeldir. Yönetilen kod T-SQL ile kullanma hakkında daha kapsamlı tartışma için avantajları hakkında kullanıma [avantajları birini kullanarak yönetilen kod için veritabanı nesneleri oluşturma](https://msdn.microsoft.com/en-us/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>1. adım: Northwind veritabanını taşıma`App_Data`

Tüm öğreticilerimizi bugüne kadarki web uygulama s Microsoft SQL Server 2005 Express Edition veritabanı dosyasında kullanmış `App_Data` klasör. Veritabanında yerleştirme `App_Data` Basitleştirilmiş dağıtma ve tüm dosyaların bir dizin içinde bulunduğu ve öğretici test etmek için hiçbir ek yapılandırma adımları gerekli gibi bu öğreticileri çalışıyor.

Bu öğreticide, ancak let s taşıma dışı Northwind veritabanı `App_Data` ve açıkça SQL Server 2005 Express Edition veritabanı örneği ile kaydedin. Bu öğretici veritabanı için şu adımları gerçekleştirebilirsiniz sırada `App_Data` klasörü, çeşitli adımları yapılan daha kolay veritabanı SQL Server 2005 Express Edition veritabanı örneğiyle açıkça kaydederek.

Bu öğretici için indirme iki veritabanı dosyalarını - sahip `NORTHWND.MDF` ve `NORTHWND_log.LDF` - adlı bir klasöre yerleştirilen `DataFiles`. Öğreticiler kendi uyarlamasını birlikte izliyorsanız, Visual Studio'yu kapatın ve taşıma `NORTHWND.MDF` ve `NORTHWND_log.LDF` s Web sitesinden dosyaları `App_Data` klasöründen Web sitesi dışında bir klasöre. Veritabanı dosyalarını başka bir klasöre taşındıktan sonra Northwind veritabanı SQL Server 2005 Express Edition veritabanı örneği ile kaydetmeniz gerekir. Bu SQL Server Management Studio'dan yapılabilir. Bir olmayan - Express sürümü, SQL Server bilgisayarınızda yüklü 2005 varsa sonra büyük olasılıkla Management Studio'yu yüklemiş olduğunuz. Yalnızca SQL Server 2005 Express Edition yüklü yeniden indirmek ve yüklemek için bir dakikanızı ayırın [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

SQL Server Management Studio'yu başlatın. Şekil 1'de görüldüğü gibi Management Studio bağlanmak için hangi sunucu isteyerek başlar. Localhost\SQLExpress için sunucu adını girin, Windows kimlik doğrulaması kimlik doğrulama aşağı açılan listesinde seçin ve Bağlan'ı tıklatın.


![Uygun veritabanına bağlanın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Şekil 1**: uygun veritabanına bağlanın


Önceden bağlandıktan sonra Nesne Gezgini penceresi veritabanları, güvenlik bilgileri, yönetim seçenekleri ve diğerleri de dahil olmak üzere SQL Server 2005 Express Edition veritabanı örneğiyle ilgili bilgiler listeler.

Northwind veritabanına eklemek ihtiyacımız `DataFiles` klasörü (veya yerlerde, onu taşınmış olabilir) SQL Server 2005 Express Edition veritabanı örneği. Veritabanları klasörünü sağ tıklatın ve bağlam menüsünden Ekle seçeneğini belirleyin. Bu veritabanları Ekle iletişim kutusunu getirir. Ekle düğmesini tıklatın, uygun aşağıya doğru ayrıntıya `NORTHWND.MDF` dosya ve Tamam'ı tıklatın. Bu noktada, ekranınızın Şekil 2'ye benzer görünmelidir.


[![Uygun veritabanına bağlanın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Şekil 2**: uygun veritabanına bağlanın ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Management Studio aracılığıyla SQL Server 2005 Express Edition örneğine bağlanırken veritabanları ekleme iletişim kutusu, Belgelerim gibi kullanıcı profili dizinlerde detaya izin vermiyor. Bu nedenle, yerleştirdiğinizden emin olun `NORTHWND.MDF` ve `NORTHWND_log.LDF` bir kullanıcı olmayan profili dizindeki dosyaları.


Veritabanı eklemek için Tamam düğmesini tıklatın. Veritabanları ekleme iletişim kutusu kapanır ve Nesne Gezgini şimdi henüz bağlı veritabanı listelenmelidir. Büyük olasılıkla Northwind veritabanı gibi bir ada sahip olan `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Veritabanı için Northwind veritabanı üzerinde sağ tıklayıp yeniden adlandırma seçerek yeniden adlandırın.


![Northwind için veritabanını yeniden adlandırın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Şekil 3**: Northwind için veritabanını yeniden adlandırın


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>2. adım: Visual Studio'da yeni bir çözüm ve SQL Server projesi oluşturma

Yönetilen saklı yordamları veya UDF'ler SQL Server 2005'te oluşturmak için şu saklı yordam ve UDF mantığı Visual Basic kodu bir sınıf olarak yazar. Kod yazıldıktan sonra bu sınıf bir derlemeye derlemek ihtiyacımız (bir `.dll` dosyası), derleme SQL Server veritabanı ile kaydetme ve ardından karşılık gelen yönteminde işaret veritabanında bir saklı yordam veya UDF nesne oluşturma derleme. Bu adımları tüm el ile gerçekleştirilebilir. Biz kodu herhangi bir metin düzenleyicisi oluşturun, Visual Basic derleyici kullanarak komut satırından derleme (`vbc.exe`), veritabanı kullanarak kaydetmek [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/en-us/library/ms189524.aspx) komutun veya Management Studio'dan ve saklı ekleyin yordam veya UDF nesnesi aracılığıyla benzer anlamına gelir. Neyse ki, Visual Studio Professional ve takım sistemleri sürümleri, bu görevleri otomatikleştiren bir SQL Server Proje türü içerir. Bu öğreticide biz yönetilen bir saklı yordam ve UDF oluşturmak için SQL Server Proje türü kullanılarak size yol gösterir.

> [!NOTE]
> Visual Web Developer veya Visual Studio standart sürümünü kullanıyorsanız, el ile yaklaşımını kullanmanız gerekir. 13. adım, bu adımları el ile gerçekleştirmeye yönelik ayrıntılı yönergeler sağlar. Kullandığınız Visual Studio sürümünü bağımsız olarak uygulanması gereken önemli SQL Server yapılandırma yönergeleri Bu adımları dahil olduğundan adım 13 okumadan önce adımları 2 ile 12 okumanızı öneririz.


Visual Studio açarak başlayın. Dosya menüsünden Yeni Proje iletişim kutusu görüntülemek için yeni proje kutusunda (Şekil 4'e bakın). Aşağıya doğru veritabanı proje türü ayrıntıya gidin ve ardından, yeni bir SQL Server projesi oluşturmak sağ tarafta listelenen şablonlardan seçin. Bu proje adı seçtiniz `ManagedDatabaseConstructs` ve adlı bir çözüm içinde yerleştirilmiş `Tutorial75`.


[![Yeni bir SQL Server projesi oluşturma](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Şekil 4**: yeni bir SQL Server projesi oluşturun ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


SQL Server Proje ve çözüm oluşturmak için yeni proje iletişim kutusunda Tamam düğmesine tıklayın.

Bir SQL Server projesi belirli bir veritabanına bağlı. Sonuç olarak, yeni SQL Server projesi oluşturduktan sonra biz hemen bu bilgileri belirtmeniz istenir. Şekil 5 biz adım 1'de SQL Server 2005 Express Edition veritabanı örneğinde kayıtlı Northwind veritabanına işaret edecek şekilde doldurulduktan yeni veritabanı başvuru iletişim kutusu gösterir.


![SQL Server projesi Northwind veritabanı ile ilişkilendirme](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Şekil 5**: SQL Server projesi Northwind veritabanı ile ilişkilendirme


Yönetilen saklı yordamları ve UDF'leri oluşturacağız bu projede hataları ayıklamak için şu SQL/CLR hata ayıklama desteği bağlantı için etkinleştirmeniz gerekir. (Şekil 5'te yaptığımız gibi) ile yeni bir veritabanı bir SQL Server projesi ilişkilendirme olduğunda, Visual Studio ister bize biz SQL/CLR bağlantı üzerinde hata ayıklamayı etkinleştirmek isteyip istemediğinizi (bkz. Şekil 6). Evet'i tıklatın.


![SQL/CLR hata ayıklamayı etkinleştir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Şekil 6**: SQL/CLR hata ayıklamayı etkinleştir


Bu noktada yeni SQL Server proje çözüme eklendi. Adlı bir klasörün içerdiği `Test Scripts` adlı bir dosya ile `Test.sql`, projede oluşturulan yönetilen veritabanı nesnelerini hata ayıklama için kullanılan. Adım 12'de hata ayıklama sırasında ele alacağız.

Biz artık yeni yönetilen saklı yordamları ve UDF'leri bu projeye ekleyebilirsiniz, ancak biz yapmasına izin vermeden önce s ilk içerir varolan web uygulamamız çözümde. Dosya menüsünde Ekle seçeneğini seçin ve varolan Web sitesi seçin. Uygun Web sitesi klasörüne gözatın ve Tamam'ı tıklatın. Şekil 7'de görüldüğü gibi bu iki projelerini dahil etmek için çözüm güncelleştirir: Web sitesi ve `ManagedDatabaseConstructs` SQL Sunucu projesi.


![Çözüm Gezgini artık iki proje içerir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Şekil 7**: Solution Explorer şimdi iki proje içerir


`NORTHWNDConnectionString` Değeri `Web.config` şu anda başvuran `NORTHWND.MDF` dosyasını `App_Data` klasör. Biz bu veritabanından kaldırılmış olduğundan `App_Data` ve açıkça kaydedilmiş gelenlere güncelleştirmek ihtiyacımız SQL Server 2005 Express Edition veritabanı örneğinde `NORTHWNDConnectionString` değeri. Açık `Web.config` Web sitesi ve değişiklik dosyasında `NORTHWNDConnectionString` bağlantı dizesi okur, değer: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Bu değişiklikten sonra `<connectionStrings>` bölümüne `Web.config` aşağıdakine benzer görünmelidir:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> ' Da anlatıldığı gibi [önceki öğretici](debugging-stored-procedures-vb.md), SQL Server nesne bir istemci uygulamasında hata ayıklama sırasında bağlantı havuzu devre dışı bırakmak ihtiyacımız bir ASP.NET Web sitesi gibi. Yukarıda gösterilen bağlantı dizesi bağlantı havuzu devre dışı bırakır ( `Pooling=false` ). Yönetilen saklı yordamlar ve UDF'ler ASP.NET Web sitesinden hata ayıklamayı düşünmüyorsanız bağlantı havuzu etkinleştirin.


## <a name="step-3-creating-a-managed-stored-procedure"></a>3. adım: saklı yordamı bir yönetilen oluşturuluyor

Yönetilen bir saklı yordam Northwind veritabanına eklemek için şu ilk saklı yordamı SQL Server projesindeki bir yöntem olarak oluşturmanız gerekir. Çözüm Gezgini'nden sağ `ManagedDatabaseConstructs` proje adı ve yeni bir öğe eklemek için seçin. Bu projeye eklenen yönetilen veritabanı nesnelerini türlerini listeler Yeni Öğe Ekle iletişim kutusu görüntüler. Şekil 8'de görüldüğü gibi bu saklı yordamları ve kullanıcı tanımlı işlevler, diğerleri içerir.

Yalnızca yarıda kesildi ürünlerin tamamı döndüren bir saklı yordam ekleyerek başlayın s olanak tanır. Yeni saklı yordam dosya adı `GetDiscontinuedProducts.vb`.


[![GetDiscontinuedProducts.vb adlı yeni bir saklı yordam ekleyin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Şekil 8**: yeni depolanan yordamı adlandırılmış eklemek `GetDiscontinuedProducts.vb` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Bu, yeni bir Visual Basic sınıfı dosyası aşağıdaki içerik ile oluşturur:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Saklı yordam olarak uygulanan Not bir `Shared` yöntemi içinde bir `Partial` adlı sınıfı dosya `StoredProcedures`. Ayrıca, `GetDiscontinuedProducts` yöntemi donatılmış ile [ `SqlProcedure` özniteliği](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), yöntemi bir saklı yordam olarak işaretler.

Aşağıdaki kod oluşturur bir `SqlCommand` nesne ve kümeleri kendi `CommandText` için bir `SELECT` tüm sütunlardan döndüren sorgu `Products` ürünler için tablo `Discontinued` alan eşittir 1. Komutu çalıştırır ve sonuçları istemci uygulamasına gönderir. Bu kodu ekleyin `GetDiscontinuedProducts` yöntemi.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Tüm yönetilen veritabanı nesnelerini erişimi bir [ `SqlContext` nesne](https://msdn.microsoft.com/en-us/library/ms131108.aspx) , çağıran bağlamını temsil eder. `SqlContext` Erişim sağlayan bir [ `SqlPipe` nesne](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.aspx) aracılığıyla kendi [ `Pipe` özelliği](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Bu `SqlPipe` nesnesi SQL Server veritabanı ve çağıran uygulamada arasında bilgi ferry için kullanılır. Adından da anlaşılacağı gibi [ `ExecuteAndSend` yöntemi](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) bir geçilen yürütür `SqlCommand` nesne ve sonuçları istemci uygulamasına gönderir.

> [!NOTE]
> Yönetilen veritabanı nesnelerini saklı yordamları ve kümesi tabanlı mantığı yerine yordamsal mantığını kullanın UDF'leri için uygundur. Bir satır temelinde temelinde veri kümeleriyle çalışmak veya skaler verilerle çalışma yordamsal mantığını içerir. `GetDiscontinuedProducts` Yöntemi yeni oluşturduğumuz, ancak hiçbir yordamsal mantığını içerir. Bu nedenle, bu ideal bir T-SQL saklı yordamı uygulanması. Saklı yordamlar oluşturmak ve dağıtmak için gereken adımları göstermek için yönetilen bir saklı yordam yönetilen olarak uygulanır.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>4. adım: saklı yordamı yönetilen dağıtma

Bu kod tamamlandı, biz Northwind veritabanına dağıtmaya hazır olursunuz. Bir SQL Server projesi dağıtma bütünleştirilmiş kodu derler, derleme veritabanı ile kaydeder ve uygun yöntemleri derlemesindeki bağlantılandırma veritabanında karşılık gelen nesneleri oluşturur. Dağıtma seçeneği tarafından gerçekleştirilen görevleri setini daha kesin olarak adım 13'te yazılmış. Sağ `ManagedDatabaseConstructs` proje Çözüm Gezgini'nde adına ve dağıtma seçeneğini belirleyin. Ancak, şu hata nedeniyle dağıtım başarısız olur: 'Dış' yakınındaki sözdizimi yanlış. Geçerli veritabanı uyumluluk düzeyi bu özelliği etkinleştirmek için daha yüksek bir değere ayarlamanız gerekebilir. Bkz. saklı yordam için Yardım `sp_dbcmptlevel`.

Bu hata iletisi Northwind veritabanı ile derleme kaydolmaya çalışılırken oluşur. SQL Server 2005 veritabanı ile bir derlemeyi kaydetmeye için veritabanı s uyumluluk düzeyi 90'a ayarlanmalıdır. Varsayılan olarak, yeni SQL Server 2005 veritabanlarını 90 uyumluluk düzeyine sahip. Ancak, Microsoft SQL Server 2000 kullanılarak oluşturulmuş veritabanları 80 varsayılan uyumluluk düzeyine sahip. Northwind veritabanı başlangıçta bir Microsoft SQL Server 2000 veritabanı olduğundan, uyumluluk düzeyi 80 ayarlanmış ve bu nedenle 90 olarak yönetilen veritabanı nesnelerini kaydetmek için artırılacak gerekiyor.

Veritabanı s uyumluluk düzeyini güncelleştirmek için Management Studio'da yeni bir sorgu penceresi açın ve girin:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Yukarıdaki sorguyu çalıştırmak için araç çubuğunda yürütme simgesine tıklayın.


[![Northwind veritabanı s uyumluluk düzeyini güncelleştirmek](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Şekil 9**: Northwind veritabanı uyumluluk düzeyi s güncelleştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Uyumluluk düzeyini güncelleştirdikten sonra SQL Server projeyi yeniden dağıtın. Bu süre dağıtım hatasız tamamlamanız gerekir.

SQL Server Management Studio'ya geri dönün, Nesne Gezgini Northwind veritabanına sağ tıklayın ve Yenile seçeneğini seçin. Ardından, programlama klasörüne detaya ve sonra derlemeler klasörünü genişletin. Şekil 10 gösterildiği gibi Northwind veritabanı artık tarafından oluşturulan derleme içerir `ManagedDatabaseConstructs` projesi.


![Northwind veritabanı artık kayıtlı ManagedDatabaseConstructs derlemedir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Şekil 10**: `ManagedDatabaseConstructs` derlemedir Northwind veritabanı artık kayıtlı


Ayrıca saklı yordamlar klasörünü genişletin. Adlı bir saklı yordam var. göreceğiniz `GetDiscontinuedProducts`. Bu saklı yordam noktalarına ve dağıtım işlemi tarafından oluşturulan `GetDiscontinuedProducts` yönteminde `ManagedDatabaseConstructs` derleme. Zaman `GetDiscontinuedProducts` saklı yordam yürütüldüğünde, sırayla yürütür `GetDiscontinuedProducts` yöntemi. Bu yönetilen bir saklı yordam olduğundan Management Studio düzenlenemez (Bu nedenle kilit adının yanındaki simge saklı yordam).


![GetDiscontinuedProducts saklı yordam saklı yordamları klasörde listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Şekil 11**: `GetDiscontinuedProducts` saklı yordam saklı yordamları klasöründe listelenen


Hala yönetilen saklı yordam diyoruz önce aşmak için sahip olduğumuz bir daha fazla hurdle olduğundan: veritabanına yönetilen kodun yürütülmesini engellemek için yapılandırılmış. Bu yeni bir sorgu penceresi açmak ve yürütme doğrulayın `GetDiscontinuedProducts` saklı yordamı. Aşağıdaki hata iletisini alırsınız: .NET Framework içinde kullanıcı kodu yürütme devre dışı bırakıldı. Clr etkin yapılandırma seçeneğini etkinleştirin.

Northwind veritabanı s yapılandırma bilgilerini inceleyin, girin ve komutu yürütün `exec sp_configure` sorgu penceresinde. Bu ayarı etkin clr şu anda 0 olarak ayarlanırsa gösterir.


[![Clr etkin ayarı 0'dır şu anda ayarlamak için](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Şekil 12**: clr etkin ayardır ayarlanmış 0 ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Şekil 12 her yapılandırma ayarında ile listelenen dört değerleri sahip olduğuna dikkat edin: en düşük ve en yüksek değerleri ve yapılandırma ve çalıştırma değerleri. Clr etkin ayarı için yapılandırma değeri güncelleştirmek için aşağıdaki komutu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Yeniden çalıştırırsanız `exec sp_configure` yukarıdaki ifadeyi clr etkin ayarı s yapılandırma değeri 1 olarak güncelleştirildi, ancak çalışma değeri 0'dır görürsünüz. Yürütülecek ihtiyacımız bu yapılandırma değişikliğin etkili olması için [ `RECONFIGURE` komutu](https://msdn.microsoft.com/en-us/library/ms176069.aspx), hangi ayarlayacak çalışma değeri geçerli yapılandırma değeri. Girmeniz yeterlidir `RECONFIGURE` sorgu penceresinde ve araç çubuğundaki yürütme simgesine tıklayın. Çalıştırırsanız `exec sp_configure` artık clr etkin ayarı s yapılandırma için 1 değerini görmek ve değerleri çalışması gerekir.

Clr etkin yapılandırması tamamlandı, biz yönetilen çalıştırılmaya hazır durumda `GetDiscontinuedProducts` saklı yordamı. Sorgu penceresinde girin ve komutu yürütün `exec` `GetDiscontinuedProducts`. Saklı yordamı çağırma neden karşılık gelen yönetilen kodda `GetDiscontinuedProducts` yürütülecek yöntem. Bu kod sorunları bir `SELECT` üretilmeyen ve SQL Server Management Studio bu örnekte yer alması çağıran uygulama için bu veri döndüren tüm ürünleri döndürülecek sorgu. Management Studio bu sonuçları alır ve sonuçları penceresinde görüntüler.


[![Saklı yordam tüm döndürür GetDiscontinuedProducts ürünleri devam etmez.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Şekil 13**: `GetDiscontinuedProducts` depolanan yordamı döndürür tüm dönüştürülmesine ürünleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>5. adım: Yönetilen oluşturma saklı giriş parametreleri kabul yordamları

Sorguları ve saklı yordamlar oluşturduğumuz bu öğreticileri çoğu kullanmış *parametreleri*. Örneğin, [oluşturma yeni saklı yordamlar için yazılan veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) adlı bir saklı yordam oluşturduğumuz öğretici `GetProductsByCategoryID` adlı bir giriş parametresi kabul `@CategoryID`. Saklı yordam sonra tüm ürünleri döndürülen olan `CategoryID` alan eşleşen sağlanan değeri `@CategoryID` parametresi.

Giriş parametreleri kabul yönetilen bir saklı yordam oluşturmak için bu parametreler s yöntemi tanımında belirtin. Let s bunu göstermek için başka bir yönetilen saklı yordama ekleme `ManagedDatabaseConstructs` adlı projesi `GetProductsWithPriceLessThan`. Yönetilen Bu saklı yordamı bir fiyat belirten bir giriş parametresi kabul eder ve tüm ürünleri döndürülecek olan `UnitPrice` alandır parametre s değerden daha küçük.

Yeni bir saklı yordam projeye eklemek için sağ `ManagedDatabaseConstructs` proje adı ve yeni bir saklı yordam eklemek için seçin. Dosya adı `GetProductsWithPriceLessThan.vb`. Adım 3'te gördüğümüz gibi bu yeni bir Visual Basic sınıfı dosyası adlı bir yöntemle oluşturacak `GetProductsWithPriceLessThan` içinde yerleştirilen `Partial` sınıfı `StoredProcedures`.

Güncelleştirme `GetProductsWithPriceLessThan` kabul edecek şekilde s yöntemi tanımı bir [ `SqlMoney` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.aspx) adlı giriş parametresi `price` ve yürütün ve sorgu sonuçları döndürmek için kod yazın:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

`GetProductsWithPriceLessThan` s yöntemi tanımı ve kod çok benzeyen kodunu ve tanımı `GetDiscontinuedProducts` adım 3'te oluşturulan yöntemi. Yalnızca farklar olan `GetProductsWithPriceLessThan` yöntemi giriş parametresi olarak kabul eder (`price`), `SqlCommand` s sorgu içeren bir parametre (`@MaxPrice`), ve bir parametre eklenir `SqlCommand` s `Parameters` koleksiyonudur ve değeri atanan `price` değişkeni.

Bu kod eklendikten sonra SQL Server projeyi yeniden dağıtın. Ardından, SQL Server Management Studio'ya geri dönün ve saklı yordamlar klasörünü yenileyin. Yeni bir giriş görürsünüz `GetProductsWithPriceLessThan`. Bir sorgu penceresi girin ve komutu yürütün `exec GetProductsWithPriceLessThan 25`, hangi Şekil 14 gösterildiği gibi $25'ten tüm ürünleri listesi olur.


[![Ürünler altında $25 görüntülenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Şekil 14**: $25 ürünleri altında görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>6. adım: veri erişim katmanı yönetilen saklı yordamı çağırma

Bu noktada ekledik `GetDiscontinuedProducts` ve `GetProductsWithPriceLessThan` saklı yordamlar yönetilen `ManagedDatabaseConstructs` proje ve bunları Northwind SQL Server veritabanı ile kaydettiniz. Biz de SQL Server Management Studio'dan yönetilen Bu saklı yordamları çağrılan (Şekil 13 ve 14 bakın). Bizim ASP.NET sırayla bu kullanmak için saklı yordamlar yönetilen bir uygulamanın, ancak biz bunları veri erişimini ve iş mantığı Katmanlar mimarisinde eklemeniz gerekir. Bu adımda iki yeni yöntemlere ekleyeceğiz `ProductsTableAdapter` içinde `NorthwindWithSprocs` başlangıçta oluşturulan yazılan DataSet [oluşturma yeni saklı yordamlar için yazılan veri kümesi s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) Öğreticisi. Adım 7'de BLL karşılık gelen yöntemleri ekleyeceğiz.

Açık `NorthwindWithSprocs` yazılan veri kümesi Visual Studio ve yeni bir yönteme ekleyerek başlangıç `ProductsTableAdapter` adlı `GetDiscontinuedProducts`. Yeni bir yöntem bir TableAdapter eklemek için Tasarımcısı'nda TableAdapter s adına sağ tıklayın ve bağlam menüsünden Sorgu Ekle seçeneğini seçin.

> [!NOTE]
> Biz Northwind veritabanının taşınır beri `App_Data` SQL Server 2005 Express Edition veritabanı örneği için onu klasördür karşılık gelen bağlantı dizesi Web.config dosyasında bu değişikliği yansıtacak şekilde güncelleştirilmesi gerekir. 2. adımda size güncelleştirme ele alınan `NORTHWNDConnectionString` değeri `Web.config`. Daha sonra bu güncelleştirmeyi yapmak unuttuysanız, sorgu eklemek için hata iletisini başarısız görürsünüz. Bağlantı bulunamıyor `NORTHWNDConnectionString` nesnesinin `Web.config` TableAdapter için yeni bir yöntem ekleme girişimi sırasında bir iletişim kutusunda. Bu hatayı gidermek için Tamam'ı tıklatın ve ardından gidin `Web.config` ve güncelleştirme `NORTHWNDConnectionString` değeri 2. adımda açıklandığı gibi. Ardından yöntemi TableAdapter yeniden eklemeyi deneyin. Bu süre hatasız çalışması gerekir.


Yeni bir yöntem ekleme birçok kez geçmiş eğitimlerine kullandık TableAdapter sorgu Yapılandırma Sihirbazı başlatır. İlk adım bize nasıl TableAdapter veritabanına erişim belirtmek için ister: geçici SQL deyimi veya yeni veya varolan bir saklı yordam aracılığıyla. Biz zaten oluşturulmuş kayıtlı ve olduğundan `GetDiscontinuedProducts` depolanan yordamı seçeneği ve sonraki isabet kullanım varolan veritabanı ile yönetilen saklı yordam seçin.


[![Saklı yordam seçeneği mevcut Kullan'ı seçin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Şekil 15**: kullanım varolan saklı yordamı seçeneği seçin ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


Sonraki ekranda, bize yöntemini çağırmak için saklı yordam ister. Seçin `GetDiscontinuedProducts` saklı yordam aşağı açılan listeden yönetilen ve sonraki isabet.


[![GetDiscontinuedProducts yönetilen saklı yordam seçin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Şekil 16**: seçin `GetDiscontinuedProducts` saklı yordam yönetilen ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


Biz ardından saklı yordamı satır, tek bir değer veya hiçbir şey döndürüp döndürmediğini belirtmeniz istenir. Bu yana `GetDiscontinuedProducts` kümesi döndürür devam etmeyen ürüne satırların, ilk seçeneği (tablo veri) ve İleri'yi tıklatın.


[![Tablo verisi seçeneğini belirleyin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Şekil 17**: Tablo veri seçeneğini ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


Son Sihirbazı ekran bize kullanılan veri erişimi desenleri ve sonuçta elde edilen yöntemlerin adlarını belirtmenizi sağlar. İşaretli onay kutularını ve ad yöntemleri bırakın `FillByDiscontinued` ve `GetDiscontinuedProducts`. Sihirbazı tamamlamak için Son'u tıklatın.


[![Ad yöntemleri FillByDiscontinued ve GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Şekil 18**: yöntemler ad `FillByDiscontinued` ve `GetDiscontinuedProducts` ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Adlı yöntemleri oluşturmak için bu adımları yineleyin `FillByPriceLessThan` ve `GetProductsWithPriceLessThan` içinde `ProductsTableAdapter` için `GetProductsWithPriceLessThan` saklı yordam yönetilen.

Şekil 19 yöntemlere ekledikten sonra veri kümesi Tasarımcısı'nın ekran görüntüsü gösterilmektedir `ProductsTableAdapter` için `GetDiscontinuedProducts` ve `GetProductsWithPriceLessThan` saklı yordamlar yönetilen.


[![Bu adımda eklenen yeni yöntemler düzenleyen içerir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Şekil 19**: `ProductsTableAdapter` yöntemleri yeni eklenen bu adımda içerir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>7. adım: İlgili yöntemleri için iş mantığı katmanı ekleme

Adım 4 ve 5'te eklenen yönetilen saklı yordamlar çağırma yöntemleri dahil etmek için veri erişim katmanı güncelleştirilmiş olan, biz iş mantığı katmanı için karşılık gelen yöntemleri eklemeniz gerekir. Aşağıdaki iki yöntem ekleme `ProductsBLLWithSprocs` sınıfı:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Her iki yöntem yalnızca karşılık gelen DAL yöntemini çağırın ve dönüş `ProductsDataTable` örneği. `DataObjectMethodAttribute` Her yöntemi yukarıda biçimlendirme ObjectDataSource s Confgure veri kaynağı Sihirbazı'nı seçin sekmesinde aşağı açılan listesinde dahil edilecek bu yöntemleri neden olur.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>8. adım: saklı yordamları sunu katmandan yönetilen çağırma

İş mantığı ve verileri erişim katmanları engagement'ta arama için destek içerecek şekilde `GetDiscontinuedProducts` ve `GetProductsWithPriceLessThan` saklı yordamlar, yönetilen biz şimdi bu görüntüleyebilirsiniz saklı yordamları sonuçları ASP.NET sayfası aracılığıyla.

Açık `ManagedFunctionsAndSprocs.aspx` sayfasındaki `AdvancedDAL` klasörü ve araç kutusu'ndan GridView tasarımcısına sürükleyin. GridView s ayarlamak `ID` özelliğine `DiscontinuedProducts` ve akıllı etiketten adlı yeni bir ObjectDataSource bağlayın `DiscontinuedProductsDataSource`. ObjectDataSource kendi verisinden isteyecek şekilde yapılandırma `ProductsBLLWithSprocs` s sınıfı `GetDiscontinuedProducts` yöntemi.


[![ObjectDataSource ProductsBLLWithSprocs sınıfını kullanmak için yapılandırma](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Şekil 20**: ObjectDataSource kullanılacak yapılandırma `ProductsBLLWithSprocs` sınıfı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Aşağı açılan listeden seçme sekmesinde GetDiscontinuedProducts yöntemini seçin.](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Şekil 21**: seçin `GetDiscontinuedProducts` yöntemi seçin sekmesini aşağı açılan listeden ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Bu kılavuz yalnızca görünen ürün bilgileri için kullanılacağından, güncelleştirme, ekleme, açılan listeleri ayarlamak ve sekmeleri (hiçbiri) SİLİN ve Son'u tıklatın.

Sihirbazı tamamladığınızda, Visual Studio otomatik olarak BoundField veya CheckBoxField her veri alanı ekleyecektir `ProductsDataTable`. Dışında bu alanların tümünü kaldırmak için bir dakikanızı ayırın `ProductName` ve `Discontinued`, hangi, GridView gelin ve ObjectDataSource s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Sayfa, ziyaret edilen, ObjectDataSource çağrıları `ProductsBLLWithSprocs` s sınıfı `GetDiscontinuedProducts` yöntemi. Adım 7'de gördüğümüz gibi bu yöntem için DAL s çağırır `ProductsDataTable` s sınıfı `GetDiscontinuedProducts` çağırır yöntemi `GetDiscontinuedProducts` saklı yordamı. Bu saklı yordam, yönetilen bir saklı yordam ve devam etmeyen ürünleri döndürme adım 3'te oluşturduğumuz kodu yürütür ' dir.

Yönetilen saklı yordamı tarafından döndürülen sonuçlar içine paketlenir bir `ProductsDataTable` DAL tarafından ve bunları sunu burada bunlar GridView bağlı ve görüntülenen katmana döndürür BLL için döndürdü. Beklendiği gibi kılavuz yarıda kesildi Bu ürünleri listeler.


[![Kullanımdan ürünler listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**Şekil 22**: dönüştürülmesine ürünleri listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Daha fazla uygulama için bir metin kutusu ve başka bir GridView sayfasına ekleyin. Ürünleri miktarından daha küçük çağrılarak metin kutusuna girilen görüntülemek bu GridView sahip `ProductsBLLWithSprocs` s sınıfı `GetProductsWithPriceLessThan` yöntemi.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>9. adım: Oluşturma ve T-SQL UDF'ler çağırma

Kullanıcı tanımlı işlevler veya UDF'ler, veritabanı programlama dilleri içinde işlevler semantiği yakından mimic nesnelerini markalarıdır. Visual Basic'te bir işlev gibi UDF'ler giriş parametreleri değişken sayıda içerir ve belirli bir tür değeri döndürür. Bir UDF skaler ya da veri - bir dize, tamsayı ve benzeri - ya da tablo veri döndürebilir. S UDF'ler skaler veri türü döndüren bir UDF ile başlayarak, her iki tür hızlı bir göz atalım olanak tanır.

Aşağıdaki UDF belirli bir ürün için Stok tahmini değerini hesaplar. Üç giriş parametrelerinde - alma olarak bunu yapar `UnitPrice`, `UnitsInStock`, ve `Discontinued` değerleri için belirli bir ürün - ve türünde bir değer döndürür `money`. Çarparak Stok tahmini değerini hesaplar `UnitPrice` tarafından `UnitsInStock`. Artık kullanımda olmayan öğeler için bu değer yarıya.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Bu UDF veritabanına eklendikten sonra Management Studio programlamasına klasörü sonra işlevleri ve skaler değerli işlevler genişleterek bulunabilir. İçinde kullanılabilir bir `SELECT` sorgu şu şekilde:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Eklemiş olduğunuz `udf_ComputeInventoryValue` UDF Northwind veritabanına; Şekil 23 yukarıdaki çıktısını gösterir `SELECT` Management Studio görüntülendiğinde sorgu. Ayrıca, UDF nesne Gezgini'nde skaler değerli işlevler klasörü altında listelenmiş olduğunu unutmayın.


[![Her ürünün s stok değerlerini listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Şekil 23**: stok değerlerini listelendiğini her ürün s ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


UDF'ler ayrıca tablo veri döndürebilir. Örneğin, belirli bir kategoriye ait ürünleri döndüren bir UDF oluşturabilir:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF kabul eden bir `@CategoryID` giriş parametresi ve belirtilen sonuçları döndürür `SELECT` sorgu. İçinde bir kez oluşturduktan sonra bu UDF başvurulabilir `FROM` (veya `JOIN`) yan tümcesi bir `SELECT` sorgu. Aşağıdaki örnek döndürecekti `ProductID`, `ProductName`, ve `CategoryID` her Meşrubat için değerler.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Eklemiş olduğunuz `udf_GetProductsByCategoryID` UDF Northwind veritabanına; Şekil 24 yukarıdaki çıktısını gösterir `SELECT` Management Studio görüntülendiğinde sorgu. Tablo verisi döndüren UDF'ler Nesne Gezgini s tablo değerli işlevler klasöründe bulunabilir.


[![ProductID, ProductName ve adlı kullanıcı, Categoryıd'si için her içecek listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Şekil 24**: `ProductID`, `ProductName`, ve `CategoryID` için her içecek listelenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Oluşturma ve UDF'ler kullanma hakkında daha fazla bilgi için kullanıma [kullanıcı tanımlı işlevler giriş](http://www.sqlteam.com/item.asp?ItemID=1955). Ayrıca kullanıma [avantajları ve Drawbacks of User-Defined işlevleri](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>10. adım: yönetilen bir UDF oluşturma

`udf_ComputeInventoryValue` Ve `udf_GetProductsByCategoryID` Yukarıdaki örneklerde oluşturulan UDF'ler olduğunuz T-SQL veritabanı nesneleri. SQL Server 2005 ayrıca destekleyen eklenebilir yönetilen UDF'ler `ManagedDatabaseConstructs` yalnızca yönetilen yordamları adımları 3 ve 5 depolanan gibi proje. Bu adım için uygulama s izin `udf_ComputeInventoryValue` UDF yönetilen kod.

Yönetilen bir UDF eklemek için `ManagedDatabaseConstructs` proje, Çözüm Gezgini'nde proje adına sağ tıklayın ve yeni bir öğe eklemek için seçin. Yeni Öğe Ekle iletişim kutusu kullanıcı tanımlı şablonu seçin ve yeni UDF dosya adı `udf_ComputeInventoryValue_Managed.vb`.


[![Yeni bir yönetilen UDF ManagedDatabaseConstructs projeye ekleyin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Şekil 25**: yeni bir yönetilen UDF eklemek `ManagedDatabaseConstructs` proje ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


Kullanıcı tanımlı işlev şablon oluşturur bir `Partial` adlı sınıf `UserDefinedFunctions` adı sınıfı s dosyası adı ile aynı olan bir yöntem (`udf_ComputeInventoryValue_Managed`, bu örnekte). Bu yöntemi kullanarak donatılmış [ `SqlFunction` özniteliği](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), yöntemi bir yönetilen UDF olarak işaretler.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

`udf_ComputeInventoryValue` Yöntemi şu anda döndüren bir [ `SqlString` nesne](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlstring.aspx) ve herhangi bir giriş parametreleri kabul etmez. Üç giriş parametreleri - kabul ettiği yöntem tanımı güncelleştirmeye ihtiyacımız `UnitPrice`, `UnitsInStock`, ve `Discontinued` - ve döndüren bir `SqlMoney` nesnesi. Stok değerini hesaplamak için mantığı T-SQL ile aynı `udf_ComputeInventoryValue` UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Bunların karşılık gelen SQL türlerini UDF s yöntemi giriş parametreleri olduğunu unutmayın: `SqlMoney` için `UnitPrice` alanı [ `SqlInt16` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlint16.aspx) için `UnitsInStock`, ve [ `SqlBoolean` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlboolean.aspx) için `Discontinued`. Bu veri türlerini tanımlanan türleri yansıtacak `Products` tablosu: `UnitPrice` sütundur türü `money`, `UnitsInStock` sütun türü `smallint`ve `Discontinued` sütun türü `bit`.

Kod oluşturmaktır bir `SqlMoney` adlı örneği `inventoryValue` 0 değeri atanır. `Products` Tablosu için veritabanı verir `NULL` değerler `UnitsInPrice` ve `UnitsInStock` sütun. Bu nedenle, ilk denetlemek için bu değerleri içerip içermediğini görmek için ihtiyacımız `NULL` biz aracılığıyla yapmak s `SqlMoney` s nesnesi [ `IsNull` özelliği](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.isnull.aspx). Her iki `UnitPrice` ve `UnitsInStock` içeren olmayan`NULL` biz işlem sonra değerleri `inventoryValue` iki ürün olmalıdır. Ardından, eğer `Discontinued` biz değeri edilmesiyle yarıya sonra true olur.

> [!NOTE]
> `SqlMoney` Nesne yalnızca sağlar iki `SqlMoney` birlikte çarpılacağı örnekleri. İzin vermiyorsa bir `SqlMoney` değişmez değer bir kayan noktalı sayı çarpılacağı örneği. Bu nedenle, edilmesiyle yarıya için `inventoryValue` Biz yeni tarafından Çarp `SqlMoney` 0,5 değerine sahip örnek.


## <a name="step-11-deploying-the-managed-udf"></a>11. adım: yönetilen UDF dağıtma

Şimdi yönetilen UDF oluşturulduktan sonra biz Northwind veritabanına dağıtmaya hazır olursunuz. Adım 4'te gördüğümüz gibi yönetilen nesneler bir SQL Server projesindeki Çözüm Gezgini'nde proje adına sağ tıklayıp bağlam menüsünden dağıtma seçeneği seçerek dağıtılır.

Proje dağıtıldığında, SQL Server Management Studio'ya geri dönün ve skaler değerli işlevler klasörünü yenileyin. Şimdi iki girdiler görmeniz gerekir:

- `dbo.udf_ComputeInventoryValue`-T-SQL UDF adım 9'da oluşturulan ve
- `dbo.udf ComputeInventoryValue_Managed`-Yönetilen UDF yalnızca dağıtılan adım 10 oluşturuldu.

Bu yönetilen UDF sınamak için aşağıdaki sorgudan Management Studio içinde yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Bu komut, yönetilen kullanır `udf ComputeInventoryValue_Managed` T-SQL yerine UDF `udf_ComputeInventoryValue` UDF ancak çıktı aynıdır. Geri 23 UDF s çıktının bir ekran görüntüsünü görmek için Şekil bakın.

## <a name="step-12-debugging-the-managed-database-objects"></a>12. adım: yönetilen veritabanı nesnelerini hata ayıklama

İçinde [saklı yordamlar hata ayıklama](debugging-stored-procedures-vb.md) biz açıklanan üç öğretici seçenekleri Visual Studio ile SQL Server hata ayıklama için: veritabanı doğrudan hata ayıklama, uygulamada hata ayıklama ve hata ayıklama bir SQL Server projeden. Veritabanı nesneleri veritabanı doğrudan hata ayıklama hata ayıklaması olamaz, ancak hata ayıklaması yapılabilir bir istemci uygulaması ve SQL Server projeden doğrudan yönetilen. Çalışmak için hata ayıklama için sırayla ancak, SQL Server 2005 veritabanını SQL/CLR hata ayıklama izin vermeniz gerekir. Biz öncelikle oluşturduğunuzda, geri çağırma `ManagedDatabaseConstructs` proje Visual Studio sorulan bize olup olmadığını biz SQL/CLR (bkz. Şekil 6 2. adım) hata ayıklamayı etkinleştirmek istedik. Sunucu Gezgini penceresini veritabanından üzerinde sağ tıklayarak bu ayar değiştirilebilir.


![Veritabanı'nın SQL/CLR hata ayıklama izin verdiğinden emin olun](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Şekil 26**: Veritabanı'nın SQL/CLR hata ayıklama izin verdiğinden emin olun


Biz hata ayıklama istediğinizi düşünelim `GetProductsWithPriceLessThan` saklı yordam yönetilen. Biz kodunu içinde bir kesme noktası ayarlayarak başlarsınız `GetProductsWithPriceLessThan` yöntemi.


[![GetProductsWithPriceLessThan yönteminde kesme noktası ayarlama](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Şekil 27**: bir kesme noktası kümesinde `GetProductsWithPriceLessThan` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


SQL Server projeden yönetilen veritabanı nesnelerini hata ayıklama sırasında ilk bakış s olanak tanır. İki proje - Çözümümüzdür içeren bu yana `ManagedDatabaseConstructs` SQL Sunucu projesi ihtiyacımız başlatmak için Visual Studio istemek üzere SQL Server projesinde hata ayıklama için bizim Web sitesi - birlikte `ManagedDatabaseConstructs` SQL Server hata ayıklama başlattığınızda, projesi. Sağ `ManagedDatabaseConstructs` Çözüm Gezgini'nde proje ve bağlam menüsünden başlangıç projesi seçeneği olarak kümesi seçin.

Zaman `ManagedDatabaseConstructs` proje gelen SQL deyimlerinde yürütülmeden hata ayıklayıcı başlatıldığında `Test.sql` bulunan dosya `Test Scripts` klasör. Örneğin, test için `GetProductsWithPriceLessThan` saklı yordamı, yönetilen varolan `Test.sql` dosya çağırır aşağıdaki ifadeyi içerikle `GetProductsWithPriceLessThan` tümleştirilmesidir saklı yordam yönetilen `@CategoryID` 14.95 değeri:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Yukarıdaki komut dosyasına önceden girilen sonra `Test.sql`, hata ayıklama menüsüne giderek ve hata ayıklamayı Başlat seçerek veya F5 tuşuna tarafından hata ayıklamayı Başlat veya yeşil, araç çubuğunda simge Yürüt. Bu çözüm içinde projeleri oluşturmak, yönetilen veritabanı nesnelerini Northwind veritabanına dağıtır ve ardından yürütmek `Test.sql` komut dosyası. Bu noktada, kesme noktası isabet ve size adım adım `GetProductsWithPriceLessThan` yöntemi, giriş parametrelerinin değerleri inceleyin ve benzeri.


[![GetProductsWithPriceLessThan yönteminde kesme noktası isabet](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Şekil 28**: kesme `GetProductsWithPriceLessThan` yöntemi isabet aldı ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Bir istemci uygulaması ayıklanacak SQL veritabanı nesnesi için sırayla veritabanı uygulamasında hata ayıklama destekleyecek şekilde yapılandırılması zorunludur. Sunucu Gezgini veritabanına sağ tıklayın ve uygulamada hata ayıklama seçeneğinin işaretli olduğundan emin olun. Ayrıca, biz SQL hata ayıklayıcısı ile tümleştirmek için ve bağlantı havuzu devre dışı bırakmak için ASP.NET uygulama yapılandırmanız gerekir. 2. adım ayrıntılı adımları bahsedilen [saklı yordamlar hata ayıklama](debugging-stored-procedures-vb.md) Öğreticisi.

ASP.NET uygulaması ve veritabanı yapılandırdıktan sonra ASP.NET Web sitesi başlangıç projesi olarak ayarlayın ve hata ayıklamayı Başlat. Bir kesme noktası olan yönetilen nesnelerden çağıran sayfasını ziyaret edin, uygulama durdurulur ve denetimi için hata ayıklayıcı, burada şekil 28'de gösterildiği gibi kod boyunca geçebilirsiniz kapatılacak.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>13. adım: Veritabanı nesneleri derleme ve dağıtma el yönetilen

SQL Server projeleri oluşturma, derleme ve yönetilen veritabanı nesnelerini dağıtma kolaylaştırır. Ne yazık ki, SQL Server projeleri yalnızca Visual Studio Professional ve takım sistemleri sürümlerinde kullanılabilir. Visual Web Developer veya Standard Edition, Visual Studio kullanarak ve yönetilen veritabanı nesnelerini kullanmak istiyorsanız, el ile oluşturmak ve dağıtmak gerekir. Bu dört adımdan oluşur:

1. Yönetilen veritabanı nesnesi için kaynak kodunu içeren bir dosya oluşturun
2. Nesne bir derleme içine derleyin,
3. Derleme SQL Server 2005 veritabanı ile kaydetme ve
4. SQL Server'daki noktalarını derlemesindeki uygun yöntemi için bir veritabanı nesnesi oluşturur.

Bu görevleri göstermek, yeni bir s izin vermek için bu ürünleri döndüren saklı yordamı yönetilen, `UnitPrice` belirtilen bir değerden büyük. Bilgisayarınızda adlı yeni bir dosya oluşturun `GetProductsWithPriceGreaterThan.vb` ve dosyasına aşağıdaki kodu girin (Visual Studio, Not Defteri veya herhangi bir metin düzenleyicisi bunu gerçekleştirmek için kullanabilirsiniz):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Bu kod olarak neredeyse aynıdır `GetProductsWithPriceLessThan` 5. adımda oluşturulan yöntemi. Yalnızca farklar yöntemi, adlardır `WHERE` yan tümcesi ve sorguda kullanılan parametre adı. Geri `GetProductsWithPriceLessThan` yöntemi, `WHERE` okuma yan tümcesi: `WHERE UnitPrice < @MaxPrice`. Burada, `GetProductsWithPriceGreaterThan`, kullanırız: `WHERE UnitPrice > @MinPrice` .

Şimdi bu sınıf bir derlemeye derlemek ihtiyacımız. Komut satırından kaydettiğiniz dizine gidin `GetProductsWithPriceGreaterThan.vb` dosya ve C# Derleyici kullanın (`csc.exe`) bir derlemeye sınıf dosyası derlemek için:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Varsa v içeren klasör `bc.exe` içinde sistemde s `PATH`, tam yolunu başvuru gerekecek `%WINDOWS%\Microsoft.NET\Framework\version\`, şu şekilde:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![Bir derlemeye GetProductsWithPriceGreaterThan.vb derleme](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Şekil 29**: derleme `GetProductsWithPriceGreaterThan.vb` içine bir derleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


`/t` Bayrak, Visual Basic sınıfı dosyası DLL (yürütülebilir bir dosya yerine) derlenmesi gerektiğini belirtir. `/out` Bayrak, sonuçta elde edilen bütünleştirilmiş kodun adını belirtir.

> [!NOTE]
> Derleme yerine `GetProductsWithPriceGreaterThan.vb` sınıf dosyası alternatif olarak kullanabileceğiniz komut satırından [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) veya Visual Studio Standard Edition'da ayrı bir sınıf kitaplığı proje oluşturun. S ren Jacob Lauritsen böyle bir Visual Basic Express Edition projesi için kod ile Lütfen sağlanan `GetProductsWithPriceGreaterThan` saklı yordam ve iki yönetilen saklı yordamları ve UDF oluşturulan adım 3, 5 ve 10. S ren s proje ilgili veritabanı nesnelerini eklemek için gereken T-SQL komutlarını da içerir.


Bütünleştirilmiş koda derlenmemiş koduyla biz SQL Server 2005 veritabanı derlemede kaydetmek hazır olursunuz. Bu T-komutunu kullanarak SQL gerçekleştirilebilir `CREATE ASSEMBLY`, veya SQL Server Management Studio aracılığıyla. Management Studio'yu kullanarak s odak izin.

Management Studio'dan Northwind veritabanındaki programlamasına klasörünü genişletin. Alt dosyasının derlemeler biridir. El ile yeni bir derleme veritabanına eklemek için derlemeleri klasörü sağ tıklatın ve bağlam menüsünden Yeni bir derleme seçin. Yeni derleme iletişim kutusu (bkz. Şekil 30) bu görüntüler. Select Gözat düğmesine tıklayarak `ManuallyCreatedDBObjects.dll` biz yalnızca derlenmiş ve derleme veritabanına eklemek için Tamam'ı derleme. Değil görmelisiniz `ManuallyCreatedDBObjects.dll` nesne Gezgini'nde derleme.


[![Veritabanına ManuallyCreatedDBObjects.dll derleme ekleyin](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Şekil 30**: eklemek `ManuallyCreatedDBObjects.dll` veritabanına derleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![Nesne Gezgini'nde ManuallyCreatedDBObjects.dll listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Şekil 31**: `ManuallyCreatedDBObjects.dll` nesne Gezgini'nde listelenir


Northwind veritabanına derleme ekledik olsa da, bir saklı yordam ile ilişkilendirmek henüz `GetProductsWithPriceGreaterThan` derlemesindeki yöntemi. Bunu gerçekleştirmek için yeni bir sorgu penceresi açın ve aşağıdaki komutu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Bu yeni bir saklı yordam adlı Northwind veritabanına oluşturur `GetProductsWithPriceGreaterThan` ve yönetilen yöntemi ile ilişkilendirir `GetProductsWithPriceGreaterThan` (sınıfında olduğu `StoredProcedures`, derlemede olduğu `ManuallyCreatedDBObjects`).

Yukarıdaki komut dosyasını yürüttükten sonra Nesne Gezgini saklı yordamlar klasöründe yenileyin. Yeni bir saklı yordam giriş - görmelisiniz `GetProductsWithPriceGreaterThan` -yanında bir kilit simgesi vardır. Bu saklı yordam test etmek için girin ve sorgu penceresinde aşağıdaki komutu yürütün:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Şekil 32 gösterildiği gibi yukarıdaki komut, bu ürünlerle bilgilerini görüntüler. bir `UnitPrice` $24.95 büyüktür.


[![Nesne Gezgini'nde ManuallyCreatedDBObjects.dll listelenir](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**Şekil 32**: `ManuallyCreatedDBObjects.dll` nesne Gezgini'nde listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Özet

Microsoft SQL Server 2005 ile ortak dil çalışma zamanı (yönetilen kod kullanarak oluşturulması için veritabanı nesneleri sağlayan CLR), tümleştirme sağlar. Daha önce bu veritabanı nesneleri yalnızca T-SQL kullanarak oluşturulabilir, ancak şimdi Visual Basic gibi programlama dillerinde .NET kullanarak bu nesneleri oluşturabilir. Oluşturduğumuz Bu öğreticide iki saklı yordamlar ve yönetilen bir kullanıcı tanımlı işlev yönetilir.

Visual Studio s SQL Server Proje türü oluşturma, derleme ve yönetilen veritabanı nesnelerini dağıtma kolaylaştırır. Ayrıca, zengin hata ayıklama desteği sunar. Ancak, SQL Server Proje türleri yalnızca Visual Studio Professional ve takım sistemleri sürümlerinde kullanılabilir. Adım 13'te gördüğümüz gibi olanlar için Visual Web Developer veya Standard Edition, Visual Studio, oluşturma, derleme ve dağıtım adımları kullanarak el ile yapılmalıdır.

Mutluluk programlama!

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Artıları ve eksileri kullanıcı tanımlı işlevler](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Yönetilen kodda SQL Server 2005'te nesneleri oluşturma](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [SQL Server 2005'te tetiklemeleri yönetilen kod oluşturma](http://www.15seconds.com/issue/041006.htm)
- [Nasıl yapılır: Oluşturup çalıştırmak bir CLR SQL Server saklı yordamı](https://msdn.microsoft.com/en-us/library/5czye81z(VS.80).aspx)
- [Nasıl yapılır: Oluşturun ve kullanıcı tanımlı bir CLR SQL Server işlevini çalıştıramadı](https://msdn.microsoft.com/en-us/library/w2kae45k(VS.80).aspx)
- [Nasıl yapılır: Düzenle `Test.sql` SQL nesnelerini çalıştırmak için komut dosyası](https://msdn.microsoft.com/en-us/library/ms233682(VS.80).aspx)
- [Giriş kullanıcı tanımlı işlevler](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Yönetilen kod ve SQL Server 2005'te (Video)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Transact-SQL Başvurusu](https://msdn.microsoft.com/en-us/library/aa299742(SQL.80).aspx)
- [İzlenecek yol: yönetilen kodda bir saklı yordam oluşturma](https://msdn.microsoft.com/en-us/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme S ren Jacob Lauritsen oluştu. Bu makale, S ren de gözden yanı sıra el ile yönetilen veritabanı nesnelerini derlemek için bu makalede s indirme dahil Visual C# Express Edition projesi oluşturulur. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](debugging-stored-procedures-vb.md)
