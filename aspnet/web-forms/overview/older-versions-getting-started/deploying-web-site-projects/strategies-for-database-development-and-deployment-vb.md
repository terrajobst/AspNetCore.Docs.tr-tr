---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
title: Veritabanı geliştirme ve dağıtma (VB) stratejileri | Microsoft Docs
author: rick-anderson
description: Veri temelli bir uygulama ilk kez dağıtırken, veritabanı geliştirme ortamında üretim ortamına körüne kopyalayabilirsiniz. B...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: 07b8905d-78ac-4252-97fb-8675b3fb0bbf
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/strategies-for-database-development-and-deployment-vb
msc.type: authoredcontent
ms.openlocfilehash: 1964a8c482fba39aeb64158c2cb4624980bcff0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838718"
---
<a name="strategies-for-database-development-and-deployment-vb"></a>Veritabanı geliştirme ve dağıtma (VB) stratejileri
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF'yi indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial10_DBDevel_vb.pdf)

> Veri temelli bir uygulama ilk kez dağıtırken, veritabanı geliştirme ortamında üretim ortamına körüne kopyalayabilirsiniz. Ancak bir blind gerçekleştirme sonraki dağıtımlarda kopyalama üretim veritabanına girilir tüm verilerin üzerine yazılır. Bunun yerine, bir veritabanı dağıtma üretim veritabanı üzerine son dağıtım sonrasında geliştirme veritabanına yapılan değişiklikleri uygulamadan içerir. Bu öğretici, bu zorluklar inceler ve chronicling ve son dağıtım veritabanına yapılan değişiklikleri uygulamadan yardımcı olmak üzere çeşitli stratejileri sunar.


## <a name="introduction"></a>Giriş

Önceki öğreticilerde açıklandığı gibi bir ASP.NET uygulaması dağıtma ilgili içerik geliştirme ortamından üretim ortamına kopyalamayı kapsar. Dağıtım bir kerelik değil ancak bunun yerine yeni bir yazılım sürümü her zaman veya olduğunda gerçekleşen bir sorun hataları veya güvenlik kaygıları tanımlanır ve ele. ASP.NET sayfaları kopyalarken, resimler, JavaScript dosyaları ve diğer tür dosyaları kendiniz nasıl bu dosyayla ilgili gerekmez üretim ortamına son dağıtım beri değiştirildi. Varolan içeriğin üzerine üretim için dosyayı doğrudan kopyalayabilirsiniz. Ne yazık ki bu kolaylık veritabanı dağıtımına genişletilmez.

Veri temelli bir uygulama ilk kez dağıtırken, veritabanı geliştirme ortamında üretim ortamına körüne kopyalayabilirsiniz. Ancak bir blind gerçekleştirme sonraki dağıtımlarda kopyalama üretim veritabanına girilir tüm verilerin üzerine yazılır. Bunun yerine, bir veritabanı dağıtma uygulama içerir *değişiklikleri* üretim veritabanı üzerine son dağıtım sonrasında geliştirme veritabanına yapılan. Bu öğretici, bu zorluklar inceler ve chronicling ve son dağıtım veritabanına yapılan değişiklikleri uygulamadan yardımcı olmak üzere çeşitli stratejileri sunar.

## <a name="the-challenges-of-deploying-a-database"></a>Veritabanı dağıtma sorunlarından

Veri temelli bir uygulama ilk kez dağıtıldıktan önce yalnızca bir veritabanı yok, yani veri temelli bir uygulama ilk kez dağıtırken, doğrudan neden olan geliştirme ortamındaki veritabanı kopyalama veritabanında üretim ortamına bir geliştirme ortamı. Uygulama dağıtıldıktan sonra vardır, ancak veritabanı iki kopyasını: biri geliştirme ve üretim birinde.

Dağıtımlar arasında geliştirme ve üretim veritabanları eşitlenmemiş hale gelebilir. Üretim veritabanı s şeması değişmeden kaldığı sürece, yeni özellikler eklendikçe geliştirme veritabanı s şeması değişebilir. Ekleme veya sütunların, tabloları, görünümleri ve saklı yordamlar'ı kaldırma. Geliştirme veritabanına eklenen önemli verileri olabilir. Birçok veri odaklı uygulamalar kullanıcı tarafından düzenlenebilen olmayan sabit kodlanmış ve uygulamaya özgü verilerle doldurmuş arama tabloları içerir. Örneğin, bir artırma Web sitesi açılan listesini auctioned öğenin durumunu açıklayan seçenek olabilir: yeni, gibi yeni, sağlam ve serginin. Bu seçenekler aşağı açılan listede doğrudan kodlamak yerine bunları bir veritabanı tablosu, yerleştirmek daha iyi olur. Geliştirme sırasında kötü adlı yeni bir koşul tabloya sonra uygulama dağıtılırken eklenirse, bu aynı kaydın üretim veritabanı arama tablosunda eklenmesi gerekir.

İdeal olarak, veritabanı dağıtma veritabanı geliştirme üretime kopyalama içerir. Ancak, dağıtılan uygulama ve geliştirme sürdürüldü sonra gerçek kullanıcılardan gerçek verileri içeren üretim veritabanı doldurulurken aklınızda bulundurun. Bu nedenle, yalnızca sonraki dağıtım üretimde geliştirme veritabanını kopyalamak için olsaydı üretim veritabanının üzerine yaz ve kendi mevcut veri kaybına neden. Veritabanı dağıtma beri son dağıtım geliştirme veritabanına yapılan değişiklikleri uygulamak için boils, net sonucudur.

Veritabanı dağıtma beri son dağıtım şema ve büyük olasılıkla, veri değişiklikleri uygulamadan gerektirdiğinden, değişikliklerin geçmişini gerekir korunabilir (veya dağıtım sırasında belirlenir) ve böylece bu değişiklikleri üretimde uygulanabilir. Çeşitli teknikler yönetmek ve veri modeline değişiklikleri uygulamak için vardır.

### <a name="defining-the-baseline"></a>Taban çizgisi tanımlama

Değişiklikleri uygulama s veritabanınıza korumak için bazı başlangıç durumu, bir taban çizgisi için değişiklikleri uygulandığı olması gerekir. Başlangıç durumu, bir uçta yok tabloları, görünümleri ve saklı yordamlar ile boş bir veritabanı olabilir. Tüm veritabanı s tablolar, görünümler ve saklı yordamlar ilk dağıtımdan sonra yapılan değişiklikler ile birlikte oluşturulmasını içermesi gerekir çünkü bu tür bir temel büyük değişiklik günlüğünde sonuçlanır. Spektrumun diğer ucunda, temel başlangıçta üretim ortamına dağıtılan bir veritabanı sürümü olarak ayarlayabilirsiniz. Yalnızca ilk dağıtımda aşağıdaki veritabanına yapılan değişiklikleri içerdiği için bu seçeneği bir çok daha küçük değişiklik günlüğünde sonuçlanır. Bu tercih ediyorum yaklaşımdır. Ve Elbette bir daha fazla Orta yol yaklaşımın temel tanımlama gibi belirli bir noktada veritabanının ilk oluşturma arasındaki veritabanı ilk olarak dağıtıldığında seçebilirsiniz.

Seçilen temel sonra temel sürümünü yeniden oluşturmak için yürütülen bir SQL komut dosyası oluşturmayı göz önünde bulundurun. Böyle bir komut dosyası, veritabanını temel sürümünü hızla yeniden oluşturmak mümkün kılar. Bu işlev büyük projelerinde, özellikle yararlıdır ve her proje ya da test ya da hazırlık, gibi başka ortamlarda çalışan birden çok geliştiriciler olabilir burada kendi veritabanının kopyasını gerekir.

Çeşitli araçlar elinizin altında bir temel sürüm, SQL betiği oluşturmak için vardır. Gelen SQL Server Management Studio (veritabanı üzerinde sağ tıklayabilirsiniz SSMS), görevleri menüye gidin ve betikleri oluştur seçeneğini belirleyin. Bu, veritabanınızı s nesneleri oluşturmak için SQL komutları içeren bir dosya oluşturmak için bildirebilirsiniz betiği Sihirbazı başlatır. Veritabanı yayımlama, yalnızca veritabanı şeması, aynı zamanda veri veritabanı tablolarına oluşturmak için SQL komutları oluşturabilirsiniz Sihirbazı, başka bir seçenektir. Veritabanı Yayımlama Sihirbazı'nı ayrıntılı olarak incelenir geri *dağıtma* öğretici. Kullandığınız hangi aracı ne olursa olsun, sonunda olması gerektiğini veritabanınızın temel sürümünü yeniden oluşturmak için kullanabileceğiniz bir komut dosyası gereken gereksinimini ortaya çıkar.

## <a name="documenting-the-database-changes-in-prose"></a>Prose veritabanı değişiklikleri belgeleme

Prose değişiklikleri kaydetmek için geliştirme aşamasında değişiklikleri veri modeline günlüğünü korumak için en kolay yolu olan. Örneğin, dağıtılmış bir uygulamanın geliştirilmesi sırasında yeni bir sütun ekleyip `Employees` tablo, bir sütunu kaldırmak `Orders` tablosuna sağ tıklayıp yeni bir tablo ekleyin (`ProductCategories`), bir metin dosyası veya Microsoft Word belgesi korumak Aşağıdaki geçmişi ile:

<a id="0.8_table01"></a>


| **Değiştirme tarihi** | **Değişiklik ayrıntıları** |
| --- | --- |
| 2009-02-03: | Eklenen sütun `DepartmentID` (`int`, NOT NULL) için `Employees` tablo. Bir yabancı anahtar kısıtlaması eklenen `Departments.DepartmentID` için `Employees.DepartmentID`. |
| 2009-02-05: | Kaldırılan sütun `TotalWeight` gelen `Orders` tablo. İlişkili önceden yakalanan verileri `OrderDetails` kaydeder. |
| 2009-02-12: | Oluşturulan `ProductCategories` tablo. Üç sütun vardır: `ProductCategoryID` (`int`, `IDENTITY`, `NOT NULL`), `CategoryName` (`nvarchar(50)`, `NOT NULL`), ve `Active` (`bit`, `NOT NULL`). Bir birincil anahtar kısıtlaması eklenen `ProductCategoryID`, varsayılan değeri 1 olarak `Active`. |


Bu yaklaşımın bir dezavantajı vardır. Yeni başlayanlar için Otomasyon için umut yoktur. Bir geliştirici el ile uygulamanız gerekir - uygulama dağıtılır, her değiştirmek gibi bir kerede dilediğiniz zaman bu değişiklikleri veritabanına - uygulanması gerekir. Değişiklik günlüğünü kullanarak temel veritabanından belirli bir sürümü yeniden yapılandırılması gerekiyorsa, günlük boyutu büyüdükçe Ayrıca, bunun yapılması kadar giderek daha fazla zaman alabilir. Bu yöntem için başka bir dezavantajı, netlik ve her değişiklik günlüğü girdisi ayrıntı düzeyi bırakılır, değişiklik kaydı kişiye olmasıdır. Birden fazla Geliştirici ile ekip bazıları diğerlerinden daha ayrıntılı, daha okunabilir ve daha kesin girişleri kalmasına neden olabilir. Ayrıca, yazım hatalarını ve diğer İnsan ilgili veri girişi sırasında oluşan hataları mümkündür.

Prose veritabanı değişiklikleri belgelemek birincil avantajı, Basitlik olur. T gereksinimi oluşturmak ve veritabanı nesnelerini değiştirme SQL söz dizimi konusunda istemiyorsunuz. Bunun yerine, prose değişiklikleri kaydetmek ve bunları SQL Server Management Studio s grafik kullanıcı arabirimi aracılığıyla uygulama.

Prose, değişiklik günlüğünde, kuşkusuz, kapsam içinde büyük gibi bazı projeler ile çok karmaşık ve kazanılan t iş değil koruma sık sık değişiklik veri modeline sahip ya da birden fazla Geliştirici içerir. Ancak, bu yaklaşım yalnızca zaman değişiklikleri veri modeline sahip olan ve burada bireysel Geliştirici güçlü bir arka plan oluşturma ve veritabanı nesnelerini değiştirme SQL söz dizimi yok oldukça iyi küçük, one-man projelerinde çalışma düştüğünü gözlemledik.

> [!NOTE]
> Değişiklik günlüğünde bilgi, teknik olarak dağıtma zamanı kadar yalnızca gerekli olsa da miyim değişikliklerinin geçmişini çalıştırılmasını önerir. Ancak tek bir koruma yerine, değişiklik günlüğü dosyası, sürekli büyüyen göz önünde bulundurun farklı değişiklik günlük dosyası her bir veritabanı sürümü için sahip. Genellikle veritabanı sürümüne dağıtıldığı her defasında istersiniz. Günlüğünü değişikliği günlükleri tutarak, temelinden başlayarak herhangi bir veritabanı sürümü 1 sürümünden başlayarak değişiklik günlüğü betikleri çalıştırarak yeniden oluşturabilirsiniz ve sürüm ulaşana kadar devam etmeden yeniden oluşturmanız gerekir.


## <a name="recording-the-sql-change-statements"></a>SQL değişiklik deyimleri kaydetme

Prose değişiklik günlüğünde koruma birincil dezavantajı, Otomasyon eksikliği olmasıdır. İdeal olarak, uygulama dağıtma zaman üretim veritabanı için veritabanı değişikliklerini bir betik yürütmek için bir düğmeye tıklatmak yerine olarak yönergeleri listesini el ile yapmak zorunda kolay olurdu. Bu otomasyon, veri modeli değiştirmek için kullanılan SQL komutları içeren bir değişiklik günlüğü tutarak mümkündür.

SQL söz dizimi oluşturmak ve çeşitli veritabanı nesneleri değiştirmek için ifadeleri içerir. Örneğin, [ *CREATE TABLE deyimi*](https://msdn.microsoft.com/library/ms174979.aspx), çalıştırıldığında, belirtilen sütunları ve kısıtlamaları ile yeni bir tablo oluşturur. [ *ALTER TABLE deyimini* ](https://msdn.microsoft.com/library/ms190273.aspx) ekleme, kaldırma veya değiştirme, sütunları veya kısıtlamaları var olan bir tabloyu değiştirmez. Deyimlerini oluşturmak, değiştirmek ve dizinleri, görünümleri, kullanıcı tarafından tanımlanan İşlevler, saklı yordamlar, tetikleyiciler ve diğer veritabanı nesnelerini doğrudan vardır.

Önceki Örneğimizdeki döndüren görüntü, bir dağıtılmış uygulama için yeni bir sütun ekleyin, geliştirme sırasında `Employees` tablo, bir sütunu kaldırmak `Orders` tablosuna sağ tıklayıp yeni bir tablo ekleyin (`ProductCategories`). Tür eylemleri aşağıdaki SQL komutlarını değişiklik günlük dosyasıyla sonuçlanır:

[!code-sql[Main](strategies-for-database-development-and-deployment-vb/samples/sample1.sql)]

Üretim veritabanına dağıtmak zaman bu değişiklikleri gönderme olan bir tek tıklamayla çalışan işlemi: SQL Server Management Studio'yu açın, üretim veritabanınıza bağlanmak, yeni bir sorgu penceresi açın, değişiklik günlüğünün içeriğini yapıştırın ve betiği çalıştırmak için Çalıştır'a tıklayın.

## <a name="using-a-comparison-tool-to-synchronize-the-data-models"></a>Veri modellerini eşitlemek için bir karşılaştırma aracını kullanma

Prose veritabanı değişiklikleri belgelemek kolaydır ancak değişiklikleri uygulamak her değişiklik aynı anda üretim veritabanı bir geliştirici gerektirir; değişiklik SQL komutlarını belgeleme kolay ve hızlı bir düğmeye tıklanması olarak üretim veritabanında bu değişiklikleri uygulamadan yapar, ancak öğrenme ve oluşturmak ve veritabanı nesnelerini değiştirme için sözdizimi ve SQL deyimleri Uzmanlaşma gerekir. Veritabanı karşılaştırma araçları, her iki yaklaşım gelen en iyi şekilde yararlanın ve en kötü atın.

Bir veritabanı karşılaştırma aracı, önceden şema veya iki veritabanı verilerini karşılaştırır ve veritabanlarını farkı gösteren Özet bir raporunu görüntüler. Ardından, bir düğmeye tıklayarak ile bir veya daha fazla veritabanı nesneleri eşitlemek için SQL komutlarını oluşturabilirsiniz. İçinde koysalar geliştirme karşılaştırmak için bir veritabanı karşılaştırma aracı kullanabilirsiniz ve üretim veritabanları dağıtma SQL içeren bir dosyayı oluşturma zamanında, komutları, yürütülen, değişiklikler üretim veritabanı s şemasına kadar uygulanacak olan Geliştirme s veritabanı şeması yansıtır.

Veritabanı üçüncü taraf karşılaştırma araçları birçok farklı satıcılar tarafından sunulan çeşitli vardır. Böyle bir örnektir [ *SQL Compare*](http://www.red-gate.com/products/SQL_Compare/)tarafından [ *kırmızı kapısı yazılım*](http://www.red-gate.com/). S karşılaştırın ve geliştirme ve üretim veritabanları şemaları eşitlemek için SQL Compare kullanma işleminde size kılavuzluk sağlar.

> [!NOTE]
> Bu makalenin yazıldığı sırada SQL Compare geçerli sürümü, sürüm 7.1, Standard Edition $395 maliyetlendirme ile oluştu. 14 günlük ücretsiz deneme indirerek takip edebilirsiniz.


SQL Compare başladığında karşılaştırma projeleri iletişim kutusu açılır ve kaydedilen SQL Compare proje gösteriliyor. Yeni bir proje oluşturun. Bu karşılaştırmak için veritabanları hakkında bilgi isteyen olan proje Yapılandırması Sihirbazı ' nı başlatır (bkz. Şekil 1). Geliştirme ve üretim ortamında veritabanları için bilgileri girin.


[![Geliştirme ve üretim veritabanları karşılaştırın](strategies-for-database-development-and-deployment-vb/_static/image2.jpg)](strategies-for-database-development-and-deployment-vb/_static/image1.jpg)

**Şekil 1**: geliştirme ve üretim veritabanları Karşılaştır ([tam boyutlu görüntüyü görmek için tıklatın](strategies-for-database-development-and-deployment-vb/_static/image3.jpg))


> [!NOTE]
> Geliştirme ortamı veritabanınızı SQL Express Edition veritabanı dosyası içinde olup olmadığını `App_Data` klasör sitenizin Şekil 1'de gösterilen iletişim kutusundan seçmek için SQL Server Express veritabanı sunucusunda veritabanı kaydetmeniz gerekir. Bunu yapmanın en kolay yolu, SQL Server Management Studio (SSMS) açın, SQL Server Express veritabanı sunucusuna bağlanmak ve veritabanını sağlamaktır. SSMS bilgisayarınızda yüklü değilse, indirip ücretsiz yükleyebilirsiniz [ *SQL Server 2008 Management Studio temel sürümü*](https://www.microsoft.com/downloads/details.aspx?FamilyId=7522A683-4CB2-454E-B908-E805E9BD4E28&amp;displaylang=en).


Karşılaştırılacak veritabanlarını seçmenin yanı sıra, çeşitli karşılaştırma ayarları Seçenekler sekmesinde de belirtebilirsiniz. Etkinleştirmek istediğiniz bir seçenek olan "Yoksay kısıtlaması ve dizin adları." Önceki öğreticide uygulama hizmetleri geliştirme ve üretim veritabanları için veritabanı nesneleri ekledik olduğunu hatırlayın. Kullandıysanız `aspnet_regsql.exe` benzersiz kısıtlama adları ve birincil anahtar geliştirme ve üretim veritabanları arasında farklılık bulabilirsiniz sonra bu nesneler üzerinde üretim veritabanını oluşturmak için aracı. Sonuç olarak, SQL Compare tüm uygulama hizmetleri tablolardan farklı olarak bayrak eklenir. "Yoksay kısıtlaması ve dizin adları" ya da bırakabilirsiniz denetlenmeyen kısıtlama adları eşitlemek ve bu farkların yok saymak için SQL Compare isteyin.

Veritabanlarını seçtikten sonra Karşılaştır (ve karşılaştırma seçeneklerini gözden geçirme), karşılaştırma başlamak için şimdi karşılaştırma düğmesine tıklayın. Sonraki birkaç saniye içinde SQL Compare iki veritabanı şemalarını inceler ve bunların nasıl değişiklik gösterdiği bir rapor oluşturur. Ben ve kullanılamıyor.%n%nÇözüm yapılan bazı değişiklikler geliştirme veritabanında tutarsızlıklara SQL Compare arabiriminde nasıl belirtilmiştir göstermek için. Şekil 2 gösterildiği gibi miyim ekledim bir `BirthDate` sütuna `Authors` kaldırıldı tablo `ISBN` sütundan `Books` tablosuna sağ tıklayıp yeni bir tablo eklenen `Ratings`, site hızı gözden geçirilmiş books ziyaret ettiği izin vermek için tasarlanmıştır.

> [!NOTE]
> Bu öğreticide yapılan veri modeli değişikliklerini veritabanı karşılaştırma aracını kullanarak göstermek için yapıldığını. Veritabanında durum sonraki öğreticilerde bu değişiklikleri bulmaz.


[![SQL Compare geliştirme ve üretim veritabanları arasındaki farklar listelenmektedir:](strategies-for-database-development-and-deployment-vb/_static/image5.jpg)](strategies-for-database-development-and-deployment-vb/_static/image4.jpg)

**Şekil 2**: SQL Compare üretim veritabanları ve geliştirme arasındaki farklar listelenmektedir ([tam boyutlu görüntüyü görmek için tıklatın](strategies-for-database-development-and-deployment-vb/_static/image6.jpg))


SQL Compare veritabanı nesneleri gruplar halinde ayırır. hızlı bir şekilde, hangi nesnelerin gösteren içinde her iki veritabanı var, ancak farklı bir veritabanı ancak diğer nesneleri var ve hangi nesnelerin aynıdır. Gördüğünüz gibi her iki veritabanı mevcut, ancak farklı olan iki nesne vardır: `Authors` eklenen bir sütun içeren tablo ve `Books` bir kaldırılmış olan tablo. Yalnızca geliştirme veritabanında, yani yeni oluşturulan mevcut bir nesnesi `Ratings` tablo. Ve her iki veritabanlarında aynı 117 nesneleri vardır.

Bir veritabanı nesnesi seçildiğinde, bu nesnelerin nasıl farklılık gösterir SQL farklılıkları penceresinde görüntülenir. Şekil 2'de, alttaki görüntülenen SQL farklılıkları pencerenin, vurgular `Authors` geliştirme veritabanı tablosundaki `BirthDate` içinde bulunamadı sütun `Authors` üretim veritabanı tablosunda.

Farklar gözden geçirme ve hangi nesnelerin eşitlenmesini istediğinizi seçerek sonra sonraki adıma geliştirme veritabanıyla eşleşmesi için üretim veritabanı s şemasını güncelleştirmek için gereken SQL komutları oluşturmaktır. Bu eşitleme Sihirbazı gerçekleştirilir. Eşitleme sihirbaz hangi eşitlenecek nesne ve eylem özetler onaylar (bkz: Şekil 3) planlayın. Veritabanlarını hemen eşitleme veya zamanınızda çalıştırılabilir SQL komutları ile bir komut dosyası oluştur.


[![Veritabanları, şemalar eşitlemek için Eşitleme Sihirbazı'nı kullanın](strategies-for-database-development-and-deployment-vb/_static/image8.jpg)](strategies-for-database-development-and-deployment-vb/_static/image7.jpg)

**Şekil 3**: uygulamanızın veritabanları şemaları eşitlemek için Eşitleme Sihirbazı'nı ([tam boyutlu görüntüyü görmek için tıklatın](strategies-for-database-development-and-deployment-vb/_static/image9.jpg))


Veritabanı karşılaştırma araçları kırmızı kapısı yazılım s SQL Compare olun üretim veritabanına üzerine gelin ve tıklayın gibi kolay geliştirme veritabanı şema değişiklikleri uygulamak ister.

> [!NOTE]
> SQL Compare karşılaştırır ve iki veritabanı eşitler *şemaları*. Ne yazık ki, olmayan karşılaştırır ve iki veritabanı tabloları içindeki verilerin eşitlemeniz. Kırmızı kapısı yazılım adlı bir ürün teklifi [ *veri SQL Compare* ](http://www.red-gate.com/products/SQL_Data_Compare/) karşılaştırır ve iki veritabanı arasında veri eşitlemeyi gerçekleştirir, ancak SQL Compare tarafından ayrı bir ürün olduğundan ve başka bir $395 maliyetlerini.


## <a name="taking-the-application-offline-during-deployment"></a>Uygulama dağıtımı sırasında çevrimdışı alma

Biz olarak Bu öğretici görülen ve dağıtım birden çok adımdan oluşan bir işlemdir: ASP.NET sayfaları, ana sayfalar, CSS dosyaları, JavaScript dosyaları, görüntüler ve diğer gerekli içerik geliştirme ortamından üretim kopyalama ortam; üretim ortamına özel yapılandırma bilgilerini, gerekirse kopyalama; ve değişiklikleri veri modeline beri son dağıtım uygulanıyor. Dosya sayısı ve veritabanı değişikliklerinizi karmaşıklığına bağlı olarak, bu adımları herhangi bir yere birkaç saniyeden birkaç için tamamlamak için dakikayı bulabilir. Bu pencere sırasında flux içinde web uygulamasıdır ve siteyi ziyaret ettiği hatalar ya da beklenmeyen davranışlarla karşılaşabilirsiniz.

Bir Web sitesi dağıtırken dağıtım tamamlanana kadar web uygulaması "Çevrimdışı" duruma en iyisidir. Uygulama çevrimdışı duruma (ve dağıtım işlemi tamamlandıktan sonra çerçevesinde yedekleme) bir dosyayı karşıya yüklemeyi ve ardından silme oldukça kolaydır. Adlı bir dosya yalnızca varlığını ASP.NET 2.0 ile başlayarak `app_offline.htm` "Çevrimdışı". tüm Web sitesi uygulama s'te kök dizinini alır Bu siteyi ASP.NET sayfasında yapılan tüm istekleri otomatik olarak içeriğiyle yanıt `app_offline.htm` dosya. Bu dosyayı kaldırıldıktan sonra uygulamayı yeniden çevrimiçine döner.

Bir uygulamayı çevrimdışı sonra dağıtım sırasında alma karşıya yükleme olarak basit bir `app_offline.htm` üretim ortamına s dosyası kök dizini dağıtım işlemine başlamadan ve silme (veya başka bir yeniden adlandırma) önce bir kez dağıtım tamamlanmış olur. Bu yöntem hakkında daha fazla bilgi için John Peterson s makalesine bakın. alma bir [ *ASP.NET uygulama çevrimdışı*](http://www.15seconds.com/issue/061207.htm).

## <a name="summary"></a>Özet

Veri temelli bir uygulama dağıtma konusunda temel zorluk, veritabanı dağıtma etrafında ortalar. -Bir geliştirme ortamında - ve bir üretim ortamında veritabanı iki sürümü olduğundan geliştirme yeni özellikler eklendikçe bu iki veritabanı şemaları eşitlenmemiş olabilir. Daha fazla, dosyaları (ASP.NET sayfaları, uygulamayı dağıtırken gibi gerçek kullanıcıların gerçek verilerle doldurulmasını olarak üretim veritabanı, üretim veritabanı ile değiştirilmiş geliştirme veritabanı üzerine yazılamaz çünkü hangi s görüntü dosyaları vb.). Bunun yerine, bir veritabanı dağıtma son dağıtım üretim veritabanı geliştirme veritabanında yapılan değişiklikleri tam kümesini uygulama kapsar.

Bu öğreticide, bakımını yapma ve veritabanı değişikliklerinin günlük uygulamak için üç teknikleri attınız. Prose değişiklikleri kaydetmek için en basit yaklaşımdır bakın. Üretim veritabanında el ile bu değişiklikleri uygulamadan bu Taktik olmasını sağlarken, Bilgi Bankası SQL komutlarının oluşturmak ve veritabanı nesnelerini değiştirme için gerektirmez. Daha karmaşık bir yaklaşım ve birden fazla Geliştirici ile büyük projeleri veya projelerde daha palatable olanı, SQL komutlarını bir dizi olarak değişiklikleri kaydetmek için. Bu, büyük ölçüde bu değişiklikleri hedef veritabanına çalışırken hastens. Her iki yaklaşım en iyi şekilde kırmızı kapısı yazılım s SQL Compare gibi bir veritabanı karşılaştırma aracını kullanarak gerçekleştirilebilir.

Bu öğreticide, veri odaklı bir uygulama dağıtma kazanmasının sona eriyor. Sonraki öğreticiler kümesini nasıl üretim ortamında hatalara yanıt arar. Bunun yerine, sarı ekran ölüm yerine bir kolay hata sayfasını görüntülemek nasıl inceleyeceğiz. Ve hata s ayrıntılarını ve bu tür hatalar oluştuğunda sizi uyarmak görüyoruz.

Mutlu programlama!

> [!div class="step-by-step"]
> [Önceki](configuring-a-website-that-uses-application-services-vb.md)
> [İleri](displaying-a-custom-error-page-vb.md)
