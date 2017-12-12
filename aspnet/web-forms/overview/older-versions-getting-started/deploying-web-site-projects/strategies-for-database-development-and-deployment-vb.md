---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: "Veritabanı geliştirme ve dağıtım (VB) stratejileri | Microsoft Docs"
author: rick-anderson
description: "Veri temelli bir uygulamayı ilk kez dağıtırken, veritabanı geliştirme ortamında üretim ortamına doğrudan kopyalayabilirsiniz. B...."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 8632ed2fe5c1a296747a0206de1c6f5c5bb59dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="strategies-for-database-development-and-deployment-vb"></a>Veritabanı geliştirme ve dağıtım (VB) stratejileri
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Veri temelli bir uygulamayı ilk kez dağıtırken, veritabanı geliştirme ortamında üretim ortamına doğrudan kopyalayabilirsiniz. Ancak bir blind gerçekleştirme sonraki dağıtımlarda kopyalama üretim veritabanına girilir tüm verilerin üzerine yazar. Bunun yerine, bir veritabanı dağıtma üretim veritabanının üzerine son dağıtım itibaren geliştirme veritabanına yapılan değişiklikleri uygulamadan içerir. Bu öğretici, bu zorluklar inceler ve chronicling ve sonrasında son dağıtımı veritabanına yapılan değişiklikleri uygulamadan yardımcı olmak üzere çeşitli stratejileri sunar.


## <a name="introduction"></a>Giriş

Önceki eğitimlerine açıklandığı gibi bir ASP.NET uygulaması dağıtma ilgili içerik geliştirme ortamından üretim ortamına kopyalama kapsar. Dağıtımı bir kerelik değil ancak bunun yerine yazılımı yeni bir sürümünü yayımladı her zaman veya zaman gerçekleşen bir şey hataları veya güvenlik sorunlarının tanımlanır ve ele. ASP.NET sayfaları kopyalarken, görüntüler, JavaScript dosyaları ve diğer tür dosyaları kendiniz nasıl bu dosyayla ilgili gerekmez üretim ortamına son dağıtım bu yana değiştirildi. Dosyayı doğrudan üretim için var olan içeriğin üzerine de kopyalayabilirsiniz. Ne yazık ki, bu Basitlik veritabanı dağıtımına kapsamaz.

Veri temelli bir uygulamayı ilk kez dağıtırken, veritabanı geliştirme ortamında üretim ortamına doğrudan kopyalayabilirsiniz. Ancak bir blind gerçekleştirme sonraki dağıtımlarda kopyalama üretim veritabanına girilir tüm verilerin üzerine yazar. Bunun yerine, bir veritabanı dağıtma uygulama içerir *değişiklikleri* üretim veritabanının üzerine son dağıtım itibaren geliştirme veritabanına yapılır. Bu öğretici, bu zorluklar inceler ve chronicling ve sonrasında son dağıtımı veritabanına yapılan değişiklikleri uygulamadan yardımcı olmak üzere çeşitli stratejileri sunar.

## <a name="the-challenges-of-deploying-a-database"></a>Bir veritabanı dağıtma zorlukları

Veri temelli bir uygulamayı ilk kez dağıtılan önce yalnızca bir veritabanı, yani veri güdümlü bir uygulamayı ilk kez dağıtırken, doğrudan neden olan geliştirme ortamında veritabanını Kopyala veritabanında üretim ortamına geliştirme ortamı. Ancak veritabanı iki kopyasını uygulama dağıtıldıktan sonra vardır: biri geliştirme ve üretim birinde.

Dağıtımlar arasında geliştirme ve üretim veritabanları eşitlenmemiş hale gelebilir. Üretim veritabanı s şeması değişmeden kaldığı sürece yeni özellikler eklendikçe geliştirme veritabanı s şeması değişebilir. Ekleyebilir veya sütunları, tabloları, görünümleri veya saklı yordamlar kaldırabilirsiniz. Geliştirme veritabanına eklenen önemli verileri olabilir. Birçok veri tabanlı uygulamalar kullanıcı tarafından düzenlenebilir olmayan sabit kodlanmış, uygulamaya özgü verilerle doldurmuş arama tabloları içerir. Örneğin, artırma Web sitesi açılan listesini auctioned öğesi koşul açıklamak seçenekleriyle olabilir: yeni, gibi yeni, iyi ve bölgenizdeki. Bu seçenekler aşağı açılan listesinde doğrudan kodlamak yerine bir veritabanı tablosunda yerleştirmek daha iyi olur. Geliştirme sırasında düşük adlı yeni bir koşul tabloya sonra uygulama dağıtırken eklenirse, bu aynı kaydı üretim veritabanını arama tablosunda eklenmesi gerekir.

İdeal olarak, veritabanı dağıtma veritabanı geliştirme üretime kopyalama içerir. Ancak, dağıtılan uygulama ve geliştirme sürdürüldü sonra gerçek kullanıcıların gerçek verilerle üretim veritabanı doldurulurken aklınızda bulundurun. Bu nedenle, yalnızca veritabanı geliştirme sonraki dağıtım sırasında üretim kopyalamak için olsaydı üretim veritabanının üzerine yaz ve var olan verileri kaybedersiniz. Veritabanı dağıtımı sonrasında son dağıtımı geliştirme veritabanına yapılan değişiklikleri uygulamak için boils olduğunu net sonucudur.

Bir veritabanı dağıtımı sonrasında son dağıtımı şemayı ve büyük olasılıkla, verilerde yapılan değişiklikleri uygulamak içerdiğinden değişikliklerin geçmişini gerekir korunabilir (veya dağıtma sırasında belirlenir) böylece bu değişiklikleri üretimde uygulanabilir. Çeşitli yönetilmesi ve veri modeline değişiklikler uygulanırken teknikler vardır.

### <a name="defining-the-baseline"></a>Taban çizgisi tanımlama

Uygulama s veritabanınız değişiklikleri korumak için bazı başlangıç durumu için yapılan değişikliklerin uygulanması bir taban çizgisi olması gerekir. Bir uçta başlangıç durumu boş bir veritabanı yok tabloları, görünümleri veya saklı yordamlar olabilir. Tüm veritabanı s tabloları, görünümleri ve saklı yordamlar ilk dağıtımdan sonra yapılan değişiklikler birlikte oluşturulmasını eklemeniz gerekir çünkü bu tür, bir taban çizgisi büyük değişiklik günlüğünde sonuçlanır. Spektrumun diğer ucunda, başlangıçta üretim ortamına dağıtılan veritabanı sürümü olarak taban ayarlayabilirsiniz. Yalnızca ilk dağıtımda aşağıdaki veritabanına yapılan değişiklikleri içerdiği için bu seçenek bir kadar küçük değişiklik günlüğünde sonuçlanır. Bu tercih ediyorum yaklaşımdır. Ve Elbette yol yaklaşımın daha fazla Orta taban çizgisi tanımlama veritabanı ilk oluşturma ve veritabanı ilk olarak dağıtıldığında arasında bazı noktası seçebilirsiniz.

Seçilen bir taban çizgisi bulduktan sonra temel sürüm yeniden oluşturmak için yürütülebilir bir SQL betiği oluşturulmadan göz önünde bulundurun. Böyle bir komut dosyası hızlı bir şekilde veritabanı temel sürümünü yeniden mümkün kılar. Bu işlev büyük projelerinde özellikle yararlı olur ve her olabilir burada proje ya da test veya hazırlama, gibi ek ortamlar üzerinde çalışan birden çok geliştiriciler kendi veritabanının kopyasını gerekir.

Temel sürüm bir SQL komut dosyasını oluşturmak için elden araçları çeşitli vardır. Gelen SQL Server Management Studio (veritabanı üzerinde sağ SSMS), görevleri alt menüsüne gidin ve komut dosyaları oluşturma seçeneğini belirleyin. Veritabanınızı s nesneleri oluşturmak için SQL komutlarını içeren bir dosya oluşturmak için söyleyebilirsiniz komut dosyası Sihirbazı başlatır. Veritabanı yayımlama yalnızca veritabanı tablolarında veritabanı şeması, aynı zamanda verileri oluşturmak için SQL komutlarını oluşturabilir Sihirbazı'nı, başka bir seçenektir. Veritabanı Yayımlama Sihirbazı'nı ayrıntılı olarak incelenmesi geri *bir veritabanı dağıtma* Öğreticisi. Kullandığınız hangi aracı bağımsız olarak sahip olmanız gereken sonunda, veritabanınızı temel sürümünü yeniden oluşturmak için kullanabileceğiniz bir komut dosyası gereken gereken ortaya çıkar.

## <a name="documenting-the-database-changes-in-prose"></a>Veritabanı değişikliklerini yazıda belgeleme

Veri modeline değişikliklerin bir günlüğünü geliştirme aşamasında korumak için en basit prose'u değişiklikleri kaydetmek için yoldur. Örneğin, bir dağıtılmış uygulama geliştirme sırasında yeni bir sütun eklerseniz `Employees` tablo, bir sütundan kaldırmak `Orders` tablo ve yeni bir tablo ekleyin (`ProductCategories`), bir metin dosyası veya Microsoft Word belgesi korumak Aşağıdaki geçmişi ile:

<a id="0.8_table01"></a>


| **Değişikliği tarihi** | **Değişiklik ayrıntıları** |
| --- | --- |
| 2009-02-03: | Eklenen sütun `DepartmentID` (`int`, NOT NULL) için `Employees` tablo. Bir yabancı anahtar kısıtlaması eklenen `Departments.DepartmentID` için `Employees.DepartmentID`. |
| 2009-02-05: | Kaldırılan sütun `TotalWeight` gelen `Orders` tablo. İçinde zaten Yakalanan veriler ilişkili `OrderDetails` kaydeder. |
| 2009-02-12: | Oluşturulan `ProductCategories` tablo. Üç sütun bulunur: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), ve `Active` (`bit`, `NOT NULL`). Bir birincil anahtar kısıtlaması eklenen `ProductCategoryID`ve bir varsayılan değeri 1 `Active`. |


Bu yaklaşımın dezavantajları vardır. Yeni başlayanlar, Otomasyon hiçbir soluk yoktur. Uygulama dağıtılır - Geliştirici el ile uygulamalıdır olduğunda her değiştirmek gibi bir kerede dilediğiniz zaman bu değişiklikleri veritabanına - uygulanması gerekir. Değişiklik günlüğünü kullanarak temel veritabanından belirli bir sürümü yeniden yapılandırılması gerekiyorsa, günlük boyutu büyüdükçe Ayrıca, bunun nedenle giderek daha fazla zaman alır. Bu yöntem için başka bir dezavantajı daha anlaşılır olması ve her değişiklik günlük girişinin ayrıntı düzeyini değişmeden olduğunu değişikliği kaydı kişiye olmasıdır. Birden çok geliştirici ekibiyle içinde bazı diğerlerinden daha ayrıntılı, daha okunabilir ya da daha kesin girişi kalmasına neden olabilir. Ayrıca, yazım hatalarını ve diğer İnsan ilgili veri girişi sırasında oluşan hataları mümkün değildir.

Veritabanı değişikliklerini yazıda belgeleme birincil yararı Basitlik ' dir. T gereksinimi oluşturmak ve veritabanı nesnelerini değiştirme SQL söz dizimi aşina istemiyorsunuz. Bunun yerine, prose'u değişiklikleri kaydetmek ve bunları SQL Server Management Studio s grafik kullanıcı arabirimi aracılığıyla uygulama.

Yazıda, değişiklik günlüğü, kuşkusuz değil çok karmaşık ve kazanılan t iş kapsamda büyük olanları gibi belirli projeleri ile iyi korumak sık değişiklikleri veri modeline sahip veya birden çok geliştirici içerir. Ancak yalnızca arada sırada değişiklikleri veri modeline sahip ve burada solo Geliştirici güçlü bir arka plan oluşturma ve veritabanı nesnelerini değiştirme SQL söz dizimi yok oldukça iyi küçük, one-man projeleri çalışma bu yaklaşım görülen.

> [!NOTE]
> Değişiklik günlüğünde, teknik olarak, yalnızca dağıtma-zamana kadar gerekli bilgileriyse ı değişikliklerin geçmişini tutmanızı önerir. Ancak, tek bir koruma yerine değişim günlük dosyası, sürekli büyüyen düşünün her veritabanı sürümü için bir farklı değişiklik günlük dosyasına sahip. Genellikle, dağıtılan her zaman veritabanı sürümüne isteyeceksiniz. Değişikliği günlükleri günlüğünü tutarak, taban çizgisinden başlangıç herhangi bir veritabanı sürümü sürüm 1 ' başlayarak değişiklik günlüğü komut dosyaları çalıştırarak yeniden oluşturabilirsiniz ve sürüm ulaşana kadar devam etmeden yeniden oluşturmanız gerekir.


## <a name="recording-the-sql-change-statements"></a>SQL değişikliği deyimleri kaydetme

Değişiklik günlüğü yazıda sürdürme birincil dezavantajı Otomasyon yetersizliğidir. İdeal olarak, üretim veritabanını dağıtmak zaman veritabanı değişiklikleri uygulama bir betik yürütmek için bir düğmeye tıklanması yerine olarak yönergeleri listesini el ile yapmak zorunda kadar kolay olacaktır. Bu tür Otomasyon veri modeli değiştirmek için kullanılan SQL komutları içeren bir değişiklik günlüğü tutarak mümkündür.

SQL söz dizimi deyimleri oluşturmak ve çeşitli veritabanı nesneleri değiştirmek için bir dizi içerir. Örneğin, [ *CREATE TABLE deyimi*](https://msdn.microsoft.com/en-us/library/ms174979.aspx), çalıştırıldığında, belirtilen sütunları ve kısıtlamalar ile yeni bir tablo oluşturur. [ *ALTER TABLE deyimi* ](https://msdn.microsoft.com/en-us/library/ms190273.aspx) var olan tablo ekleme, kaldırma veya sütunları veya kısıtlamaları değiştirme değiştirir. Ayrıca oluşturmak, değiştirmek ve dizinleri, görünümler, kullanıcı tanımlı işlevler, saklı yordamlar, tetikleyiciler ve diğer veritabanı nesnelerini bırakma için deyimleri vardır.

Önceki örneğimizde döndürme görüntü, yeni bir sütun ekleyin dağıtılmış bir uygulamanın geliştirilmesi sırasında `Employees` tablo, bir sütundan kaldırmak `Orders` tablo ve yeni bir tablo ekleyin (`ProductCategories`). Gibi eylemleri aşağıdaki SQL komutlarını değişiklik günlük dosyasıyla sonuçlanır:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Bu değişiklikleri dağıtmak zaman üretim veritabanına Ftp'den olan tek tıklatmayla işlemi: SQL Server Management Studio'yu açın, üretim veritabanınıza bağlanmak, yeni bir sorgu penceresi açın, değişiklik günlüğünün içindekileri yapıştırın ve betiği çalıştırmak için Çalıştır'ı tıklatın.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Bir karşılaştırma aracı kullanarak veri modelleri eşitleme

Veritabanı değişikliklerini yazıda belgeleme kolaydır, ancak değişiklikleri uygulayan her değişiklik aynı anda üretim veritabanını bir geliştirici gerektirir; değişiklik SQL komutlarını belgeleme kolay ve hızlı bir düğmeye tıklanması olarak olarak üretim veritabanı üzerinde bu değişiklikleri uygulayarak yapar, ancak öğrenme ve SQL deyimlerini ve oluşturmak ve veritabanı nesnelerini değiştirme sözdizimi katılımınıza gerektirir. Veritabanı karşılaştırma araçları her iki yaklaşımın gelen en iyi alın ve en kötü atın.

Bir veritabanı karşılaştırma aracı şema veya iki veritabanlarının veri karşılaştırır ve veritabanlarının nasıl farklılık gösteren bir Özet rapor görüntüler. Ardından, bir düğmeyi tıklatarak ile bir veya daha fazla veritabanı nesneleri eşitlemek için SQL komutlarını oluşturabilir. Buna koysalar geliştirme karşılaştırmak için bir veritabanı karşılaştırma aracı kullanabilirsiniz ve üretim veritabanları dağıtmak SQL içeren bir dosya oluşturma zamanında, komutları,, yürütülen, değişiklikler üretim veritabanı s şemasında böylece uygulanacak şekilde Geliştirme veritabanı s şeması yansıtır.

Çeşitli birçok farklı satıcılar tarafından sunulan üçüncü taraf veritabanı karşılaştırma araçları vardır. Böyle bir örnektir [ *SQL karşılaştırmak*](http://www.red-gate.com/products/SQL_Compare/), göre [ *kırmızı kapısı yazılım*](http://www.red-gate.com/). S karşılaştırın ve geliştirme ve üretim veritabanları şemaları eşitlemek için SQL karşılaştırmak kullanarak işleminde size kılavuzluk sağlar.

> [!NOTE]
> Bu makalenin yazıldığı sırada SQL karşılaştırmak geçerli sürümü sürüm 7.1, Standard Edition $395 maliyetlendirme ile oluştu. 14 günlük ücretsiz deneme yükleyerek izleyebilirsiniz.


SQL karşılaştırmak başladığında kaydedilmiş SQL karşılaştırmak projeleri gösteren karşılaştırma projeleri iletişim kutusunu açar. Yeni bir proje oluşturun. Bu veritabanları hakkında bilgi Karşılaştırılacak ister proje Yapılandırma Sihirbazı başlatır (bkz: Şekil 1). Geliştirme ve üretim ortamı veritabanları için bilgileri girin.


[![Geliştirme ve üretim veritabanları ile karşılaştırın](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Şekil 1**: geliştirme ve üretim veritabanları karşılaştırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> Geliştirme ortamı veritabanınızı SQL Express Edition veritabanı dosyasında olup olmadığını `App_Data` klasörü sitenizin Şekil 1'de gösterilen iletişim kutusundan seçmek için SQL Server Express veritabanı sunucusunda veritabanı kaydolmanız gerekir. Bunu yapmanın en kolay yolu, SQL Server Management Studio (SSMS) açın, SQL Server Express veritabanı sunucusuna bağlanın ve veritabanını olmaktır. Bilgisayarınızda yüklü SSMS yoksa indirin ve ücretsiz yükleme [ *SQL Server 2008 Management Studio temel sürüm*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Karşılaştırılacak veritabanlarını seçerek ek olarak çeşitli Seçenekler sekmesinde ndan karşılaştırma ayarları belirtebilirsiniz. Etkinleştirmek istediğiniz bir "Yoksay kısıtlaması ve dizin adı." seçenektir Önceki öğreticide uygulama geliştirme ve üretim veritabanları için veritabanı nesneleri Hizmetleri eklediğimiz, geri çağırma. Kullandıysanız `aspnet_regsql.exe` kısıtlama adları ve birincil anahtar geliştirme ve üretim veritabanları arasında farklılık bulursunuz üretim veritabanında bu nesneleri oluşturmak için aracı. Sonuç olarak, SQL karşılaştırmak tüm uygulama hizmetleri tablolarını farklı olarak işaretleyecektir. "Yoksay kısıtlaması ve dizin adları" ya da bırakabilirsiniz denetlenmeyen ve kısıtlama adları eşitleyin veya SQL Bu farklılıklar yoksay Karşılaştırılacak isteyin.

Veritabanlarını seçtikten sonra karşılaştırma (ve karşılaştırma seçeneklerini gözden geçirme), karşılaştırma başlamak için şimdi karşılaştırmak düğmesini tıklatın. Sonraki birkaç saniye içinde SQL karşılaştırın iki veritabanı şemalarını inceler ve bunların nasıl farklı bir rapor oluşturur. I ullanıcı var yapılan bazı değişiklikler bu tutarsızlıkları SQL karşılaştırmak arabiriminde nasıl belirtilmiştir göstermek için geliştirme veritabanına. Şekil 2'de görüldüğü gibi ı ekledim bir `BirthDate` sütuna `Authors` tablo, kaldırılan `ISBN` sütundan `Books` tablo ve yeni bir tablo eklenir `Ratings`, site hızı geçirilmiş books ziyaret eden kullanıcıların olanak amacı.

> [!NOTE]
> Bu öğreticide yapılan veri modeli değişikliklerini veritabanı karşılaştırma aracıyla göstermeye yapıldığını. Veritabanında durum sonraki öğreticilerde bu değişiklikleri bulmaz.


[![Geliştirme ve üretim veritabanları arasındaki farklar SQL karşılaştırma listeler](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Şekil 2**: SQL karşılaştırma listeler üretim veritabanları ve geliştirme arasındaki farklar ([tam boyutlu görüntüyü görüntülemek için tıklatın](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


Veritabanı nesneleri gruplar halinde sekme SQL karşılaştırmak ayırır, hangi nesneleri gösteren hızla her iki veritabanı var, ancak farklıysa, hangi nesnelerin bir veritabanı ancak diğer mevcut ve hangi nesnelerin aynıdır. Gördüğünüz gibi her iki veritabanı var, ancak farklı olan iki nesne vardır: `Authors` eklenen bir sütunu içeriyor, tablo ve `Books` bir kaldırılmış olan tablo. Yalnızca geliştirme veritabanında, yani yeni oluşturulan mevcut bir nesnesi `Ratings` tablo. Ve her iki veritabanı aynı 117 nesneleri vardır.

Bir veritabanı nesnesi seçerek, bu nesnelerin nasıl farklılık gösterir SQL farkları penceresi görüntülenir. Şekil 2'de, alttaki görüntülenen SQL farkları pencere vurgular `Authors` geliştirme veritabanı tablosunda `BirthDate` içinde bulunamadı sütun `Authors` üretim veritabanı tablosunda.

Farkları gözden geçirme ve eşitlemek istediğiniz hangi nesneleri seçtikten sonra sonraki adıma geliştirme veritabanını eşleşecek şekilde üretim veritabanı s şemasını güncelleştirmek için gereken SQL komutlarını oluşturmaktır. Bu eşitleme Sihirbazı gerçekleştirilir. Eşitleme Sihirbazı'nı hangi eşitlemek için nesne ve eylem özetler onaylar (bkz: Şekil 3) planlayın. Veritabanlarını hemen eşitleme ya da zamanınızda çalıştırılabilir SQL komutları ile bir komut dosyası oluşturma.


[![Veritabanı şemaları eşitlemek için Eşitleme Sihirbazı'nı kullanın](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Şekil 3**: bilgisayarınızı veritabanları şemaları eşitlemek için Eşitleme Sihirbazı'nı ([tam boyutlu görüntüyü görüntülemek için tıklatın](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


Veritabanı karşılaştırma araçları kırmızı kapısı yazılım s SQL karşılaştırma yapmak üretim veritabanını seçip kadar kolay geliştirme veritabanı şemasında değişiklikleri uygulamak istiyor.

> [!NOTE]
> SQL karşılaştırmak karşılaştırır ve iki veritabanı eşitler *şemaları*. Ne yazık ki, onu değil karşılaştırın ve iki veritabanı tabloları içinde verileri eşitleyin. Kırmızı kapısı yazılım adlı bir ürün teklif [ *verileri SQL karşılaştırmak* ](http://www.red-gate.com/products/SQL_Data_Compare/) karşılaştırır ve iki veritabanı arasında verileri eşitler ancak SQL karşılaştırmak ayrı üründen olduğundan ve başka bir $395 maliyetleri.


## <a name="taking-the-application-offline-during-deployment"></a>Uygulama dağıtımı sırasında çevrimdışı alma

Biz olarak bu öğreticileri görülen ullanıcı dağıtımıdır birden çok adımdan bir işlem: ASP.NET sayfaları, ana sayfalar, CSS dosyaları, JavaScript dosyaları, görüntüler ve diğer gerekli içerik geliştirme ortamından üretim kopyalama ortam; Üretim ortamında özel yapılandırma bilgilerini yedeklemenin gerekirse kopyalama; ve değişiklikleri veri modeline sonrasında son dağıtımı uygulama. Dosya sayısı ve veritabanı değişikliklerinizi karmaşıklığına bağlı olarak, bu adımları herhangi bir yere birkaç saniyeden birkaç dakika tamamlamak için sürebilir. Bu penceresi sırasında web uygulaması flux ve siteyi ziyaret eden kullanıcıların hataları ya da beklenmeyen davranışlarla karşılaşabilirsiniz.

Bir Web sitesi dağıtırken dağıtım tamamlanana kadar web uygulaması "" çevrimdışına en iyisidir. Uygulama çevrimdışı duruma getirme (ve dağıtım işlemi tamamlandıktan sonra hale getirme yedekleme) bir dosyayı karşıya yüklemeyi ve ardından silme olarak kadar kolaydır. Adlı bir dosya yalnızca varlığını ASP.NET 2.0 ile başlayan `app_offline.htm` tüm Web sitesi kök dizini uygulama s "Çevrimdışı". alır Bu sitedeki bir ASP.NET sayfası için herhangi bir istek içeriğini otomatik olarak yanıt `app_offline.htm` dosya. Bu dosyayı kaldırıldıktan sonra uygulamayı yeniden çevrimiçi olur.

Uygulama dağıtımı sırasında daha sonra çevrimdışı duruma getirmeden karşıya olarak basit bir `app_offline.htm` dosya s üretim ortamı için kök dizini dağıtım işlemine başlamadan ve silme (veya başka bir şey için yeniden adlandırma) önce bir kez dağıtım tamamlanmış olur. Bu teknik hakkında daha fazla bilgi için John Peterson s makalesine başvurun çıkarmadan bir [ *ASP.NET uygulama çevrimdışı*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Özet

Veri tabanlı bir uygulama dağıtmanın en önemli güçlük veritabanı dağıtma çevresinde toplanır. -Bir geliştirme ortamında - ve bir üretim ortamında veritabanı iki sürümü olduğundan yeni özellikler geliştirmeye eklendikçe bu iki veritabanı şemaları eşitlenmemiş hale gelebilir. Daha fazla, (ASP.NET sayfaları, uygulamayı oluşturan dosyaları dağıtırken yapabildiğiniz gibi gerçek kullanıcıların gerçek verilerle doldurulur olarak üretim veritabanını, üretim veritabanını değiştirilmiş geliştirme veritabanıyla üzerine yazılamıyor çünkü hangi s görüntü dosyaları ve benzeri). Bunun yerine, bir veritabanı dağıtımı sonrasında son dağıtımı üretim veritabanı geliştirme veritabanında yapılan değişiklikleri kesin kümesi uygulama kapsar.

Bu öğretici koruma ve bir günlük veritabanı değişiklikleri uygulamak için üç teknikler, aranan. En kolay yaklaşım, prose'u değişikliklerin kaydetmektir. Üretim veritabanını el ile bu değişiklikleri uygulamadan bu Taktik Bunun olmasını sağlarken, SQL komutlarını bilgisi oluşturmak ve veritabanı nesnelerini değiştirme için gerekli değildir. Daha karmaşık bir yaklaşım, birden çok geliştiricilere büyük projeler veya projeleri çok daha palatable olan bir ise SQL komutlarını bir dizi olarak değişiklikleri kaydetmek için. Bu, büyük ölçüde bu değişiklikler gerçekleştirdiği hedef veritabanına çalışırken hastens. Her iki yaklaşımın en iyi kırmızı kapısı yazılım s SQL karşılaştırmak gibi bir veritabanı karşılaştırma aracı kullanılarak elde edilebilir.

Bu öğreticide, veri güdümlü bir uygulama dağıtımı bizim odaklanmasına sonlanır. Öğreticiler bir sonraki kümesini nasıl üretim ortamında hataları yanıt bakar. Bunun yerine, sarı ekran ölüm yerine kolay hata sayfasını görüntülemek nasıl inceleyeceğiz. Ve nasıl hata s ayrıntıları oturum ve bu hata oluştuğunda sizi uyarmak nasıl göreceğiz.

Mutluluk programlama!

>[!div class="step-by-step"]
[Önceki](configuring-a-website-that-uses-application-services-vb.md)
[sonraki](displaying-a-custom-error-page-vb.md)
