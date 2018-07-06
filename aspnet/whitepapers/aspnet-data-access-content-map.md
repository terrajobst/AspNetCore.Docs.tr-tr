---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET veri erişimi - önerilen kaynaklar | Microsoft Docs
author: rick-anderson
description: Bu konuda, öncelikli olarak SQL Se ve Entity Framework kullanarak ASP.NET web uygulamalarında veri erişim hakkında belge kaynakları bağlantılar sağlar...
ms.author: aspnetcontent
ms.date: 07/25/2013
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: fb0cea94d82cc8f59ec56a5445ee84d38325995e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832506"
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET veri erişimi - önerilen kaynaklar
====================
> Bu konuda, öncelikli olarak SQL Server ve Entity Framework kullanarak ASP.NET web uygulamalarında veri erişim hakkında belge kaynakları için bağlantılar sağlar.
> 
> Harika bir blog gönderisi, biliyorsanız [stackoverflow](http://stackoverflow.com) iş parçacığı veya kullanışlı olabilecek diğer bir bağlantı [bize bir e-posta gönderin](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) bağlantı.
> 
> Son güncelleştirilen 4/3/2014


Bu konuda aşağıdaki bölümleri içerir:

- [ASP.NET veri erişimi ile çalışmaya başlama](#gettingstarted)
- [Entity Framework kullanma](#ef)

    - [Varlık çerçevesi kodu ilk kez kullanma](#cf)
    - [Entity Framework Code First geçişleri kullanma](#efcfmigrations)
    - [Entity Framework veritabanı ilk kullanarak veya Model ilk (EF Tasarımcısı)](#efdbf)
    - [Entity Framework (yavaş yükleniyor, istekli yükleme ve açık yükleniyor) ilgili verileri yükleniyor](#efrelateddata)
    - [Entity Framework performansı iyileştirme](#optimizingef)
    - [Bir Entity Framework uygulamasında eşzamanlılık işleme](#efconcurrency)
    - [Entity Framework kitapları](#efbooks)
    - [Ek Entity Framework kaynakları](#otherefresources)
- [Veri bağlama ASP.NET Web Forms uygulamaları](#wfdatabinding)

    - [Kullanarak Web Forms Model bağlama](#wfmodelbinding)
    - [Veri kaynağı denetimleri kullanarak Web formları](#wfdsc)
    - [Verilere bağlı denetimler ve veri bağlama ifadeleri kullanarak Web formları](#wfdbc)
- [SQL Server veritabanları ile çalışma](#sqlserver)

    - [SQL Server Express LocalDB veritabanları ile çalışma](#sslocaldb)
    - [SQL Server Express veritabanları ile çalışma](#sse)
    - [Windows Azure SQL veritabanı ile çalışma](#ssdb)
    - [SQL Server ve Windows Azure SQL veritabanı arasında seçim yapma](#ssdbchoosing)
- [NoSQL veritabanı yönetim sistemleri ile çalışma](#nosql)
- [ASP.NET uygulamalarında LINQ sorguları kullanma](#linq)
- [Dinamik veri yapı iskelesini kullanmaya](#dd)
- [Veri erişiminin güvenliğini sağlama](#securing)
- [Veri erişim performansı iyileştirme](#optimizingdataaccess)
- [Veritabanı dağıtma](#deploying)
- [Bir Web hizmeti aracılığıyla verilere erişme](#webservice)
- [Ek kaynaklar](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>ASP.NET veri erişimi ile çalışmaya başlama

- [Veri depolama seçenekleri (Windows Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). -Bulut için geliştirme hakkında kitap bölümü. Alternatif olarak bilinen ilişkisel veritabanları ile birçok geliştiricinin kolayca gözden kaçabilir eğilimindedir NoSQL veritabanları sunar. Ne seçerken ilişkisel düşünün veya NoSQL veya belirli bir platform belirleyerek yönergeleri sunar.
- [ASP.NET veri erişim seçenekleri](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). ASP.NET için ilişkisel veritabanları için veri erişim seçenekleri ve yönergeleri platformlarını seçin ve senaryonuz için uygun olan yöntemlere erişmek için bir giriş.
- [İlişkisel veritabanı](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). İlişkisel veritabanlarıyla çalışmadıysanız bu sayfayı giriş ilişkisel veritabanı terimler ve kavramlar için bkz. SQL Server için giriş özellikle bkz [SQL Server veritabanları ile çalışma](#sqlserver) bu konuda.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Entity Framework kullanma

- [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Bir Entity Framework geliştirme yaklaşımını ilk veritabanı modeli ilk ya da Code First hakkında yönergeler.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Varlık çerçevesi kodu ilk kez kullanma
  

Aşağıdaki öğreticilerde indirilebilir örnek uygulamalarını sunar:

- [MVC 5 kullanarak EF 6 ile çalışmaya başlama](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Arka planda Entity Framework Code First senaryoları, geçişleri ve EF 6 dahil olmak üzere çeşitli bağlantı dayanıklılığı, komut durdurma ve zaman uyumsuz gibi özellikleri. Bu güncelleştirilmiş bir sürümü olduğu [EF 5 / MVC 4 serisi](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Önceki serisi, yeni dizide yer almayan bir iş birimi desenleri ve depo bir öğretici içerir.
- [ASP.NET MVC 5 giriş](../mvc/overview/getting-started/introduction/getting-started.md). Dar bir varlık çerçevesi kodu aralığı ilk senaryoları ele alınmaktadır ancak MVC özellikleri ile tanışın, daha kapsamlı bir iş yapar.
- [Model bağlama ve Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Code First, Web Forms uygulaması'nda kullanır.
- [ASP.NET 4.5 Web Forms ile çalışmaya başlama](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Giriş Web Forms bazı kapsamı ile kod ilk. Model bağlama kullanır.
- [MVC müzik Store](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Ayrıca, üyelik ve yetkilendirme uygulayan bir e-ticaret MVC 3 uygulamasında Code First kullanır. MVC sürüm ve burada kullanılan ASP.NET üyelik (kimlik doğrulaması ve yetkilendirme) sistemi büyük/küçük harf geçmiştir; ASP.NET üyelik hakkında daha fazla güncel bilgi için bkz: [ https://asp.net/identity ](https://asp.net/identity).

Diğer kaynaklar:

- [Entity Framework - mevcut bir veritabanına ilk kod](https://msdn.microsoft.com/data/jj200620). MSDN. Video ve nasıl kullanılacağını gösteren izlenecek yol var olan bir veritabanına ilk kod.
- [Veri Geliştirici Merkezi - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Oluşturulan ve Entity Framework ekibi tarafından korunan Entity Framework belgelerine kılavuzu için bkz: [Başlarken](https://msdn.microsoft.com/data/ee712907) bağlantı.

Ayrıca bkz: [Entity Framework hakkındaki Books](#efbooks) ve [ek Entity Framework kaynakları](#otherefresources) bu konuda.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Entity Framework Code First geçişleri kullanma
  

Kapak geçişleri listelenen Code First öğreticiler çoğunu. Ayrıca aşağıdaki kaynaklara bakın.

- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). bir veritabanını dağıtmak için Code First Migrations'ı kullanma işlemini gösterir 2 bölümden öğretici serisi.
- [Bir Windows Azure Web sitesine bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC 5 uygulaması dağıtın](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Üyeliği ve uygulama verilerini Azure'da dağıtmak için geçişleri kullanma
- [Visual Studio ve ASP.NET için Web dağıtımı genel bakış](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Bkz: **Visual Studio'daki yapılandırma veritabanı dağıtımı** Code First Migrations ile Visual Studio web dağıtımı özellikleri nasıl tümleştirildiği hakkında bir açıklama için bölüm.
- [Veri Geliştirici Merkezi - Code First geçişleri](https://msdn.microsoft.com/data/jj591621) (MSDN). Entity Framework takımın geçişleri belgeleri.
- [Geçişleri yayını serisi](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF blog). Code First Migrations Gelişmiş konularında üç videosu.
- [Code First Migrations ile ASP.NET Web sayfaları sitelerinde](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting blog). Code First migrations ile bir ASP.NET Web sayfaları sitesinde bir Visual Studio sınıf kitaplığı projesinde veri bağlamı koyarak kullanmayı gösterir.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Entity Framework veritabanı ilk kullanarak veya Model ilk (EF Tasarımcısı)

- [Entity Framework 6 veritabanı MVC 5 kullanarak First ile çalışmaya başlama](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Bir veritabanı oluşturmak için sunucu Gezgini'nde bir betik çalıştırın ve ardından Entity Framework designer veri modeli oluşturun. Sayfaları basit CRUD web oluşturmak için nasıl ve diğer veri işlevleri tüm EF iş akışları aynı DbContext API kullandığından Code First öğreticilerden birine izleyebilirsiniz işleme gösterir.

Aşağıdaki kaynaklar daha eski. Bunlar, Entity Framework 4.0 sürümünü kullanmak istiyorsanız ve bir Web Forms uygulaması veri bağlama için bir veri kaynağı denetimi kullanmak istediğiniz kullanışlıdır.

- [Entity Framework 4.0 ile çalışmaya başlama](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Nasıl kullanılacağını gösterir **EntityDataSource** denetimi.
- [Entity Framework ile devam etmeden](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(nasıl kullanılacağını gösterir **ObjectDataSource** denetimi. Bir öğretici eşzamanlılık işleme, EF performansı hakkında bir öğretici ve EF 4. 0'yenilikler hakkında bir öğretici içerir.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>İşleme ilgili verileri Entity Framework (yavaş yükleniyor, istekli yükleme ve açık yükleniyor)

- [Bir ASP.NET MVC uygulamasındaki Entity Framework ile ilgili verileri okuma](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). İlk olarak, MVC örnek uygulamanın kodu. Gösterilen yöntemleri, Web Forms model bağlama ve veritabanı ilk iş akışı için de geçerlidir.
- [Veri Geliştirici Merkezi - ilgili varlıkları yükleme](https://msdn.microsoft.com/data/jj574232) (MSDN). İlgili verileri yükleme hakkında Entity Framework takımın belgeler.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Entity Framework performansı iyileştirme

- [Bir ASP.NET uygulaması için Entity Framework senaryoları Gelişmiş](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Değişiklikler kaydedilirken doğrulama devre dışı bırakma, değişiklik algılama devre dışı bırakma ve kendi SQL deyimlerini yürütmek ya da saklı yordamlar çağırmak nasıl gösterir.
- [Entity Framework 5 için performans konuları](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Performansla ilgili önemli noktalar (varlık çerçevesi)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Varlık Çerçevesi'nde bir ASP.NET Web uygulaması performansını en üst düzeye](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Entity Framework 4.0 için geçerlidir.
- Ayrıca bkz: [en iyi duruma getirme ASP.NET veri erişimi](#optimizingdataaccess) bu konuda.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Bir Entity Framework uygulamasında eşzamanlılık işleme

- [Bir ASP.NET MVC uygulamasındaki Entity Framework ile eşzamanlılığı işleme](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). İlk olarak, bir MVC örnek uygulaması kullanarak DbContext API kod.
- [Veri Geliştirici Merkezi'nde – iyimser eşzamanlılık desenlerinin](https://msdn.microsoft.cus/data/jj592904) (MSDN). Entity Framework takımın eşzamanlılık belgeleri.
- [Bir ASP.NET Web uygulamasında Entity Framework ile eşzamanlılığı işleme](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Entity Framework 4.0 için geçerlidir. İlk olarak, Web Forms örnek uygulaması kullanarak ObjectContext API veritabanı.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Entity Framework kitapları

- [Programlama varlık çerçevesi: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman ve Rowan Miller.
- [Programlama varlık çerçevesi: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman ve Rowan Miller.

Bu kitap hem de geçerli önerilen teknikleri ile güncel durumda. Internet'te bir Entity Framework daha kapsamlı henüz kolay anlatımlı giriş kullanılabilir herhangi bir şey daha sağlarlar. Başka bir kitap [Entity Framework programlama](http://shop.oreilly.com/product/9780596807252.do) Julie Lerman tarafından daha büyük ve daha kapsamlı ancak eskidir ve kapsar teknikleri birçoğu artık Entity Framework kullanmak için önerilen yoldur. Ayrıca Entity Framework ekibi tarafından önerilen kitaplar listesini görmek [veri Geliştirici Merkezi - kitaplar](https://msdn.microsoft.com/data/aa937716) MSDN sitesinden.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Diğer Entity Framework kaynakları

- [Entity Framework (ADO.NET) ekibi blogu](https://blogs.msdn.com/b/adonet/). En güncel bilgileri ve duyuruları yeni geliştirmeleri için en iyi kaynaklardan biri. EF ile ilgili diğer günlükleri için bkz. blog kaydı sırasında [Entity Framework ile çalışmaya başlama](https://msdn.microsoft.com/data/ee712907).
- [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx). Bkz: **veri noktaları** Entity Framework ile ilgili konular hakkında sık olan sütun.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Veri bağlama ASP.NET Web Forms uygulamaları

- [ASP.NET Web Forms veri erişim seçenekleri](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Kullanarak Web Forms Model bağlama

- [Model bağlama ve Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Öğretici serisinin EF Code First kullanarak.
- [Web Forms Model bağlama 1. Bölüm: Veri seçme](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Scott Guthrie'nin blog). Eski bu blog gönderileri, şu anda Itemtype adlı özellik ModelType olarak adlandırılmıştı, ancak Aksi takdirde içerdikleri bilgileri geçerli.
- [Web Forms Model bağlama 2. Bölüm: Verileri filtreleme](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Scott Guthrie'nin blog).
- [Web Forms Model bağlama 3. Bölüm: Güncelleştirme ve doğrulama](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Scott Guthrie'nin blog).
- [ASP.NET 4.5 Web Forms Model bağlama](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Model bağlama bölüm 1 - verileri seçme](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Model bağlama bölüm 2 - filtreleme](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Veri alma görüntüleme ASP.NET 4.5 Web Forms ile - başlangıç öğelerini ve ayrıntıları](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Veri kaynağı denetimleri kullanarak Web formları

- [Veri kaynağı Web sunucusu denetimleri](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Dinamik veri sağlayıcısı ve EntityDataSource denetimi bu sürüm için Entity Framework 6 ile tanışın](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (Microsoft Web geliştirme blogu).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Verilere bağlı denetimler ve veri bağlama ifadeleri kullanarak Web formları

- [Model bağlama ve Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). EF Code First kullanan öğretici serisi.
- [Veri alma görüntüleme ASP.NET 4.5 Web Forms ile - başlangıç öğelerini ve ayrıntıları](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Kesin türü belirtilmiş veri denetimleri](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Scott Guthrie'nin blog).
- [Kesin türü belirtilmiş veri denetimleri](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 Web Forms tanımlayıcı türü belirtilmiş veri denetimleri](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Veriye bağlı Web sunucusu denetimleri](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Veri bağlama ifadeleri genel bakış](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Bu sayfa yalnızca kapsar **Eval** ve **bağlama**; içerecek şekilde güncelleştirilmemiş **öğesi** ve **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>SQL Server veritabanları ile çalışma

- [SQL Server veritabanı özellikleri](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Genel bir giriş için çok çeşitli SQL Server konuları için bu bir içindekiler altındaki girişleri bakın.
- [SQL Server sürümleri](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Bir özeti, her biri hakkında daha fazla bilgi için bağlantılarla birlikte kullanılabilir SQL Server sürümlerinde.)
- [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [ASP.NET Web uygulamaları için SQL Server Compact kullanarak](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Veritabanı ürün örnekleri](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Veritabanları AdventureWorks örnek.
- [Örnek veritabanları yükleme](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Burada gösterilen yöntemlere ek olarak, ayrıca bir örnek .mdf dosyaları indirip uygulamaya yükleyebilirsiniz\_veri klasörü bir web projesinin LocalDB veritabanına dönüştürmek ve bir LocalDB bağlantı dizesi oluşturun. Bunu nasıl yapacağınız hakkında daha fazla bilgi için bkz. [nasıl yapılır: yerel veritabanına yükseltme](https://msdn.microsoft.com/library/hh873188.aspx).

SQL Server Express ve LocalDB ile çalışma ve SQL Server ve SQL veritabanı arasında seçim yapma, ayrıca aşağıdaki bölümlere bakın.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>SQL Server Express LocalDB veritabanları ile çalışma

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). LocalDB resmi MSDN giriş.
- [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Nasıl yapılır: yerel veritabanına yükseltme](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). .Mdf dosyası bir sürümündense SQL Server Express LocalDB geçiş yapma. Aşağıdakilerden birini indirmeniz halinde bu süreçte gitmek de [SQL Server 2012 örnek veritabanları](https://go.microsoft.com/fwlink/?linkid=117483).
- [LocalDB, Gelişmiş bir SQL Express ile tanışın](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express blog). MSDN dahil edilmiş miktardan LocalDB neden oluşturulduğu daha fazla arka plan sahiptir.
- [LocalDB: Veritabanım nerede?](https://go.microsoft.com/fwlink/?LinkId=234376) (Blog) SQL Server Express. LocalDB veritabanı dosyaları oluşturulduğu hakkında bilgiler.
- [LocalDB kullanımıyla tam IIS ile bölüm 1: kullanıcı profili](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express blog). LocalDB, IIS ile çalışmak üzere tasarlanmamıştır. Bu bir dizi blog gönderileri, sorunları ve bazı geçici çözümleri açıklar.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>SQL Server Express veritabanları ile çalışma

- [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). SQL Server Express ile AttachDBFileName bağlantı dizesi ayarı kullanıyorsanız, bu sayfanın özellikle kullanıcı örneği bölümüne bakın.
- [Yerel SQL Server Express 2008 sahipliğini nasıl](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express blog). Yaygın bir sorun SQL Server Express örneği üzerinde bir yönetici olmadığınızdan SQL Server Express veritabanlarıyla çalışmak oluşturamıyor. Varsayılan olarak, SQL Server Express yükleyen kişinin bir yöneticidir. Bu blog bilgisayarda yöneticiyseniz kendiniz bir SQL Server Express Yöneticisi olun açıklanmaktadır.
- [ASP.NET web uygulamamı bir SQL Server Express veritabanı üretimde kullanabilir miyim?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Windows Azure SQL veritabanı ile çalışma

- [Bir Windows Azure Web sitesine bir üyelik, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (Microsoft Azure site).
- [SQL veritabanları](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure site). Başlangıç öğreticileri ve nasıl yapılır kılavuzları alınıyor.
- [Windows Azure SQL veritabanı](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). SQL veritabanı'nda MSDN içindekiler tablosunun üst düzey düğüm.
- [Windows Azure SQL veritabanı TechNet Wiki makaleleri dizini](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (Microsoft TechNet sitesinden).
- [Geçici hata işleme uygulama bloğu](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Azaltma gelen sonucunda ortaya çıkan geçici ağ hataları ve bağlantı hatalarını işlemek için sağlayan bir çerçeve. Bir NuGet paketi olarak kullanılabilir: [Kurumsal kitaplığı 5.0 - geçici hata işleme uygulama bloğu](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [SQL veritabanı ve Entity Framework ile çalışmaya başlama](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure eğitim Seti](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft İndirme Merkezi). SQL veritabanı için uygulamalı laboratuvarlar içerir.
- [Windows Azure SQL veritabanı Topluluk Forumu](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Windows Azure SQL veritabanı'na taşıma](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Microsoft Patterns ve uygulamalar ekibi tarafından kapsamlı bir uçtan uca senaryonun bir bölüm. Neden geçirmek isteyebileceğiniz kapsar ve SQL Server'dan SQL veritabanı'na geçirme.
- [SQL Server veritabanları Windows Azure SQL veritabanı'na geçirme](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL veritabanı Geçiş Sihirbazı'nı](http://sqlazuremw.codeplex.com/). Geçiş için bir açık kaynak aracı için ve SQL veritabanı'ndan veritabanları.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>SQL Server ve Windows Azure SQL veritabanı arasında seçim yapma

- [Windows Azure SQL veritabanı ile SQL Server'ı karşılaştırma](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (Microsoft TechNet sitesinden).
- [Windows Azure SQL veritabanı'na veri geçişi: araçlar ve teknikler](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). SQL veritabanı için SQL Server karşılaştırın ve ne zaman SQL Server'dan SQL veritabanı'na geçirme hakkında rehberlik bölümler içerir.
- [Windows Azure SQL veritabanı teslim Kılavuzu](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (Microsoft TechNet sitesinden).
- [SQL Server özellik kısıtlamaları (Windows Azure SQL veritabanı)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure tablo depolama ve Windows Azure SQL veritabanı - benzerlikler ve karşıtlıklar](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Windows Azure'a dağıttığınız bir uygulamanın Windows Azure tablo depolama, Windows Azure SQL veritabanı için bir alternatif olabilir. Bu konu, bu alternatifleri arasında karar vermenize yardımcı olur.
- [Windows Azure SQL veritabanı](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Kılavuzlar ve sınırlamalar (Windows Azure SQL veritabanı)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>NoSQL veritabanı yönetim sistemleri ile çalışma

- [Windows Azure Veri Hizmetleri](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure site). Bkz: [tablo hizmeti özellik Kılavuzu](https://docs.microsoft.com/azure/) ve **büyük veri** sayfasının bölümünde.
- [ASP.NET çok katmanlı depolama tabloları, kuyrukları ve Blobları](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). Windows Azure depolama NoSQL tablo kullanan indirilebilir örnek uygulaması ile uçtan uca öğretici.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>ASP.NET uygulamalarında LINQ sorguları kullanma

- [ASP.NET veri erişim seçenekleri](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). Lınq'e giriş içerir.
- [LINQ eğitim videoları](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (ALi Stagner'ın blogu).
- [Dinamik LINQ kaynaklarına bağlantılar ile ASP.NET Forum dizisinin](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Dinamik veri yapı iskelesini kullanmaya

- [Dinamik veri proje şablonları](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Dinamik veri projeleri kullanıldığı durumlar hakkında yönergeler.
- [ASP.NET dinamik veri](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Veri erişiminin güvenliğini sağlama

- [ASP.NET veri erişimi güvenli hale getirme](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Güvenlik konuları (varlık çerçevesi)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Nasıl yapılır: Güvenli bağlantı dizelerini veri kaynağı denetimleri kullanırken](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Veri erişim performansı iyileştirme

- [ASP.NET performansa genel bakış](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET önbelleğe alma](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [ASP.NET performansını iyileştirme](https://msdn.microsoft.com/library/ff647787) (MSDN). Bu sayfanın üstündeki "Kullanımdan kaldırılan içerik" bir uyarı yoktur, ancak yine de ilgili bilgilerin çoğunu ve karşılaştırılabilir güncelleştirilmiş kaynak yok.
- [SQL Server performansı artırma](https://msdn.microsoft.com/library/ff647793) (MSDN). Önceki bağlantı olarak aynı açıklama.

Ayrıca bkz: [en iyi duruma getirmek Entity Framework performans](#optimizingef) bu konuda daha önce.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Veritabanı dağıtma

- [ASP.NET Web dağıtımı - önerilen kaynaklar](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Bir Web hizmeti aracılığıyla verilere erişme

- [Bir Web hizmeti aracılığıyla verilere](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). Ne zaman Web API'si ve WCF kullanılacağı hakkında yönergeler.
- [ASP.NET Web API'si ile çalışmaya başlama](../web-api/index.md).
- [WCF Veri Hizmetleri](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET veri erişimi SSS](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms öğreticiler - veri](../web-forms/overview/data-access/index.md). Bu öğreticiler görece eski çoğu; okuduğunuzdan emin olun [ASP.NET Data Access Options](https://msdn.microsoft.com/library/ms178359.aspx) ve [veri depolama seçenekleri (gerçek hayatta kullanılan bulut uygulamaları Windows Azure ile oluşturma)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) ilk böylece doğru olmayan çok bir veri erişim yönteme uygulanmaz Senaryonuz için.
- [ASP.NET MVC içerik haritası](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web sayfaları öğreticiler - veri](../web-pages/overview/data/index.md).
- [Visual Studio'da verilere erişme](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Benzer bağlantıların listesini sağlar. Bu içerik haritası ancak ASP.NET yerine Visual Studio odaklanan.
