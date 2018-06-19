---
uid: whitepapers/aspnet-data-access-content-map
title: ASP.NET Data Access - önerilen kaynakları | Microsoft Docs
author: rick-anderson
description: Bu konu öncelikle SQL Se ve Entity Framework kullanarak ASP.NET web uygulamalarında veri erişme hakkında belge kaynaklarına bağlantılar sağlar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/25/2013
ms.topic: article
ms.assetid: f8157be1-4ab9-469e-ad3a-0ccc80b56c00
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-data-access-content-map
msc.type: content
ms.openlocfilehash: 16364951544dff33c9cdb8c8e330cc93de192c47
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28048265"
---
<a name="aspnet-data-access---recommended-resources"></a>ASP.NET Data Access - önerilen kaynakları
====================
> Bu konu öncelikle SQL Server ve Entity Framework kullanarak ASP.NET web uygulamalarında veri erişme hakkında belge kaynaklarına bağlantılar sağlar.
> 
> Harika bir blog gönderisi, biliyorsanız [stackoverflow](http://stackoverflow.com) iş parçacığı veya yararlı olacak herhangi bir bağlantıyı [bize bir e-posta gönderin](mailto:aspnetue@microsoft.com?subject=Data Access Content Map) bağlantı.
> 
> Son güncelleştirilen 4/3/2014


Konu aşağıdaki bölümleri içerir:

- [ASP.NET veri Erişimindeki ile çalışmaya başlama](#gettingstarted)
- [Entity Framework kullanarak](#ef)

    - [Entity Framework kod ilk kullanma](#cf)
    - [Entity Framework Code First geçişleri kullanma](#efcfmigrations)
    - [Entity Framework veritabanı ilk kullanarak veya Model First (EF Designer)](#efdbf)
    - [Entity Framework (yavaş yükleniyor, istekli yükleniyor ve açık yükleme) içinde ilgili veri yükleme](#efrelateddata)
    - [Entity Framework performansı en iyi duruma getirme](#optimizingef)
    - [Entity Framework uygulamada eşzamanlılık işleme](#efconcurrency)
    - [Entity Framework hakkındaki kitaplar](#efbooks)
    - [Ek Entity Framework kaynaklar](#otherefresources)
- [Veri bağlama ASP.NET Web Forms uygulamaları](#wfdatabinding)

    - [Model bağlama Forms Web kullanma](#wfmodelbinding)
    - [Veri kaynağı denetimleri Forms Web kullanma](#wfdsc)
    - [Forms veri bağlama denetimleri ve veri bağlama ifadeleri Web kullanma](#wfdbc)
- [SQL Server veritabanları ile çalışma](#sqlserver)

    - [SQL Server Express LocalDB veritabanları ile çalışma](#sslocaldb)
    - [SQL Server Express veritabanları ile çalışma](#sse)
    - [Windows Azure SQL veritabanı ile çalışma](#ssdb)
    - [SQL Server ve Windows Azure SQL veritabanı arasında seçim yapma](#ssdbchoosing)
- [NoSQL veritabanı yönetim sistemleri ile çalışma](#nosql)
- [LINQ sorgularını ASP.NET uygulamalarında kullanma](#linq)
- [Dinamik veri iskele kullanma](#dd)
- [Veri erişimi güvenli hale getirme](#securing)
- [Veri erişim performansını en iyi duruma getirme](#optimizingdataaccess)
- [Bir veritabanı dağıtma](#deploying)
- [Web hizmeti aracılığıyla verilere erişme](#webservice)
- [Ek kaynaklar](#additional)

<a id="gettingstarted"></a>

## <a name="getting-started-with-data-access-in-aspnet"></a>ASP.NET veri Erişimindeki ile çalışmaya başlama

- [Veri depolama seçenekleri (Windows Azure ile gerçek bulut uygulamaları derleme)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md). Bulut için geliştirme hakkında bir e kitap bölüm. NoSQL veritabanları ile ilişkisel veritabanları hakkında bilgi sahibi birçok geliştiriciler overlook eğilimindedir alternatif olarak tanıtır. Ne hakkında seçerken ilişkisel düşünmek NoSQL ya da belirli bir platform seçme yönergeleri sunar.
- [ASP.NET veri erişim seçenekleri](https://msdn.microsoft.com/library/ms178359.aspx) (MSDN). ASP.NET için ilişkisel veritabanları için veri erişim seçenekleri ve kılavuzların platformlarını seçin ve senaryonuz için uygun olan yöntemler erişim hakkında giriş.
- [İlişkisel veritabanı](http://en.wikipedia.org/wiki/Relational_database). Wikipedia). İlişkisel veritabanları ile çalışan yapmadıysanız, ilişkisel veritabanı terminolojisi ve kavramları giriş için bu sayfayı görürsünüz. Giriş SQL Server için özellikle bkz [SQL Server veritabanları ile çalışma](#sqlserver) bu konuda daha sonra.

<a id="ef"></a>

## <a name="using-the-entity-framework"></a>Entity Framework kullanarak

- [Entity Framework Geliştirme yaklaşımları](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf) (MSDN). Bir Entity Framework geliştirme yaklaşım veritabanı ilk olarak, Model First veya Code First seçme hakkında yönergeler.

<a id="cf"></a>

### <a name="using-entity-framework-code-first"></a>Entity Framework kod ilk kullanma
  

Aşağıdaki öğreticiler indirilebilir örnek uygulamalar sağlar:

- [MVC 5 kullanarak EF 6 ile çalışmaya başlama](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kapak Entity Framework Code First senaryoları geçişler ve EF 6 dahil olmak üzere, çeşitli bağlantı dayanıklılığı, komut kişiler tarafından ele ve zaman uyumsuz gibi özellikleri. Bu güncelleştirilmiş bir sürümü olan [EF 5 / MVC 4 serisi](../mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Önceki serisi bir öğretici depo ve yeni serisinde dahil edilmeyen iş birimi düzenleri içerir.
- [ASP.NET MVC 5 giriş](../mvc/overview/getting-started/introduction/getting-started.md). Daha dar bir Entity Framework kod aralığı ilk senaryolar için geçerlidir, ancak MVC özellikler sunarak, daha kapsamlı bir işi yapar.
- [Model bağlama ve Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Code First, Web Forms uygulamasında kullanır.
- [ASP.NET 4.5 Web formları ile çalışmaya başlama](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Giriş Web Forms bazı kapsamı ile kod ilk. Model bağlama kullanır.
- [MVC müzik deposu](../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1.md). Code First de üyelik ve yetkilendirme uygulayan bir e-ticaret MVC 3 uygulamada kullanır. MVC sürümü ve burada kullanılan ASP.NET üyelik (kimlik doğrulama ve yetkilendirme) sistemi eski; ASP.NET üyelik hakkında daha fazla güncel bilgi için bkz: [https://asp.net/identity](https://asp.net/identity).

Diğer kaynaklar:

- [Entity Framework - mevcut bir veritabanına ilk kod](https://msdn.microsoft.com/data/jj200620). MSDN. Video ve nasıl kullanılacağını gösterir izlenecek önce var olan bir veritabanı kodu.
- [Veri Geliştirici Merkezi - Entity Framework](https://msdn.microsoft.com/data/ef). MSDN. Oluşturulan ve Entity Framework ekibi tarafından korunan Entity Framework belgelerine kılavuzu için bkz: [Get Started](https://msdn.microsoft.com/data/ee712907) bağlantı.

Ayrıca bkz. [Entity Framework hakkındaki Books](#efbooks) ve [ek Entity Framework kaynaklar](#otherefresources) bu konuda daha sonra.

<a id="efcfmigrations"></a>

### <a name="using-entity-framework-code-first-migrations"></a>Entity Framework Code First geçişleri kullanma
  

Kapak geçişler listelenen Code First öğreticileri çoğunu. Ayrıca aşağıdaki kaynaklara bakın.

- [Visual Studio kullanarak ASP.NET Web dağıtımı](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). bir veritabanını dağıtmak için Code First Migrations kullanmayı gösterir 2 bölümlü öğretici serisi.
- [Windows Azure Web sitesine üyeliği, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC 5 uygulaması dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Microsoft Azure). Geçişler üyelik ve uygulama verileri Azure'a dağıtmak için nasıl kullanılacağını.
- [Visual Studio ve ASP.NET Web dağıtımına genel bakış](https://msdn.microsoft.com/library/dd394698.aspx#dbdeployment). Bkz: **yapılandırma veritabanı dağıtımı Visual Studio'da** Code First Migrations Visual Studio web dağıtım özellikleri nasıl tümleştirildiği hakkında ayrıntılı açıklamalar için bölüm.
- [Veri Geliştirici Merkezi - Code First geçişleri](https://msdn.microsoft.com/data/jj591621) (MSDN). Entity Framework ekibin geçişler belgeleri.
- [Geçişler ekran kaydı serisi](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx). EF blog). Üç videolar Code First Migrations Gelişmiş konularında.
- [Code First Migrations ASP.NET Web sayfaları sitelerle](http://www.mikesdotnetting.com/Article/217/Code-First-Migrations-With-ASP.NET-Web-Pages-Sites). Mikesdotnetting blog). Code First geçişleri bir ASP.NET Web Pages sitesinde bir Visual Studio sınıf kitaplığı projesinde veri bağlamı koyarak kullanmayı gösterir.

<a id="efdbf"></a>

### <a name="using-entity-framework-database-first-or-model-first-the-ef-designer"></a>Entity Framework veritabanı ilk kullanarak veya Model First (EF Designer)

- [Entity Framework 6 veritabanı MVC 5 kullanarak ilk ile çalışmaya başlama](../mvc/overview/getting-started/database-first-development/setting-up-database.md). Bir veritabanı oluşturmak için sunucu Gezgini'nde bir komut dosyasını çalıştırın ve ardından veri modeli oluşturmak için Entity Framework Tasarımcısı'nı kullanın. Sayfaları basit CRUD web oluşturmak için nasıl ve diğer veri işleme tüm EF iş akışları aynı DbContext API kullandığından Code First öğreticileri birini izleyebilirsiniz işlevlerini gösterir.

Aşağıdaki kaynaklar, daha eski. Bunlar, Entity Framework 4.0 sürümünü kullanmak istediğiniz ve Web Forms uygulamasında veri bağlama için bir veri kaynağı denetimi kullanmak istiyorsanız yararlıdır.

- [Entity Framework 4.0 ile çalışmaya başlama](../web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). Nasıl kullanılacağını gösterir **EntityDataSource** denetim.
- [Entity Framework ile devam etmeden](../web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)(nasıl kullanılacağını gösterir **ObjectDataSource** denetim. Eşzamanlılık işleme, bir öğretici EF performansı ve EF 4. 0'yenilikler hakkında bir öğretici bir öğretici içerir.

<a id="efrelateddata"></a>

### <a name="handling-related-data-in-entity-framework-lazy-loading-eager-loading-and-explicit-loading"></a>İşleme ilgili verileri Entity Framework (yavaş yükleniyor, istekli yükleniyor ve açık yükleme)

- [ASP.NET MVC uygulamasındaki Entity Framework ile ilgili verileri okuma](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md). İlk olarak, MVC örnek uygulama kodu. Gösterilen yöntemleri, Web Forms modeli bağlama ve veritabanı ilk iş akışı için de geçerlidir.
- [Veri Geliştirici Merkezi ilgili varlıklar Yükleniyor -](https://msdn.microsoft.com/data/jj574232) (MSDN). Entity Framework ekibin belge yükleme hakkında ilgili verileri.

<a id="optimizingef"></a>

### <a name="optimizing-entity-framework-performance"></a>Entity Framework performansı en iyi duruma getirme

- [Gelişmiş bir ASP.NET uygulaması için Entity Framework senaryoları](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application.md). Doğrulama değişiklikler kaydedilirken devre dışı bırakma, değişikliği algılama devre dışı bırakma ve kendi SQL deyimlerini yürütmek veya saklı yordamlar çağırmak nasıl gösterir.
- [Entity Framework 5 için başarım düşünceleri](https://msdn.microsoft.com/data/hh949853) (MSDN).
- [Başarım düşünceleri (Entity Framework)](https://msdn.microsoft.com/library/cc853327) (MSDN).
- [Bir ASP.NET Web uygulamasında Entity Framework performansı en üst düzeye](../web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md). Entity Framework 4.0 için geçerlidir.
- Ayrıca bkz. [en iyi duruma getirme ASP.NET veri erişimi](#optimizingdataaccess) bu konuda daha sonra.

<a id="efconcurrency"></a>

### <a name="handling-concurrency-in-an-entity-framework-application"></a>Entity Framework uygulamada eşzamanlılık işleme

- [ASP.NET MVC uygulamasındaki Entity Framework eşzamanlılık işleme](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md). İlk olarak, bir MVC örnek uygulaması kullanarak DbContext API kodu.
- [Veri Geliştirici Merkezi – iyimser eşzamanlılık desenleri](https://msdn.microsoft.cus/data/jj592904) (MSDN). Entity Framework ekibin eşzamanlılık belgeleri.
- [Bir ASP.NET Web uygulamasında Entity Framework eşzamanlılık işleme](../web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md). Entity Framework 4.0 için geçerlidir. İlk olarak, ObjectContext API, Web Forms örnek bir uygulama kullanarak veritabanı.

<a id="efrepository"></a><a id="efbooks"></a>

### <a name="books-about-the-entity-framework"></a>Entity Framework hakkındaki kitaplar

- [Programlama Entity Framework: DbContext](http://shop.oreilly.com/product/0636920022237.do) Julie Lerman ve Rowan Mert.
- [Programlama Entity Framework: Kod ilk](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman ve Rowan Mert.

Bu books her ikisi de geçerli önerilen teknikleri ile güncel değil. Internet'te bir Entity Framework daha kapsamlı henüz izleyin kolay giriş kullanılabilir herhangi bir şey daha sağlarlar. Başka bir kitap [programlama Entity Framework](http://shop.oreilly.com/product/9780596807252.do) Julie Lerman tarafından daha büyük ve daha kapsamlı ancak eskidir ve çoğu kapsar teknikleri artık Entity Framework kullanmak için önerilen yoldur. Ayrıca bkz. books Entity Framework ekibi tarafından önerilen listesi [veri Geliştirici Merkezi - Books](https://msdn.microsoft.com/data/aa937716) MSDN sitesinden.

<a id="otherefresources"></a>

### <a name="other-entity-framework-resources"></a>Diğer Entity Framework kaynakları

- [Entity Framework (ADO.NET) ekibi blogu](https://blogs.msdn.com/b/adonet/). En güncel bilgiler ve yenilikleri duyuruları için en iyi kaynaklardan biri. EF ile ilgili diğer bloglar, blog kaydı sırasında bkz [Entity Framework ile çalışmaya başlama](https://msdn.microsoft.com/data/ee712907).
- [MSDN dergisi](https://msdn.microsoft.com/magazine/default.aspx). Bkz: **veri noktaları** Entity Framework ilgili konular hakkında sık olan sütun.

<a id="wfdatabinding"></a>

## <a name="data-binding-in-aspnet-web-forms-applications"></a>Veri bağlama ASP.NET Web Forms uygulamaları

- [ASP.NET Web formları veri erişim seçenekleri](https://msdn.microsoft.com/library/jj822927.aspx) (MSDN)<a id="wfmodelbinding"></a>.

<a id="wfmodelbinding"></a>

### <a name="using-web-forms-model-binding"></a>Model bağlama Forms Web kullanma

- [Model bağlama ve Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). Eğitmen serisi EF Code First kullanarak.
- [Web Forms modeli bağlama bölüm 1: Verileri seçme](https://weblogs.asp.net/scottgu/archive/2011/09/06/web-forms-model-binding-part-1-selecting-data-asp-net-vnext-series.aspx) (Scott Guthrie'nın blogu). Eski bu Web günlüğü postaları ItemType şu anda adlı özellik ModelType adlı ancak içerdikleri bilgi Aksi durumda geçerlidir.
- [Web Forms modeli bağlama bölüm 2: Verileri filtreleme](https://weblogs.asp.net/scottgu/archive/2011/09/12/web-forms-model-binding-part-2-filtering-data-asp-net-vnext-series.aspx) (Scott Guthrie'nın blogu).
- [Web Forms modeli bağlama bölüm 3: Güncelleştirme ve doğrulama](https://weblogs.asp.net/scottgu/archive/2011/10/30/web-forms-model-binding-part-3-updating-and-validation-asp-net-4-5-series.aspx) (Scott Guthrie'nın blogu).
- [ASP.NET 4.5 Web formları Model bağlama](../web-forms/videos/aspnet-web-forms-vnext/aspnet-45-web-forms-model-binding.md). (video).
- [Bağlama bölüm 1 - seçerek veri modeli](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-1-selecting-data.md) (video).
- [Model bağlama bölüm 2 - filtreleme](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-model-binding-part-2-filtering.md) (video).
- [Veri alma görüntülemek ASP.NET 4.5 Web Forms - başlatılan öğeleri ve ayrıntıları](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).

<a id="wfdsc"></a>

### <a name="using-web-forms-data-source-controls"></a>Veri kaynağı denetimleri Forms Web kullanma

- [Veri kaynağı Web sunucusu denetimleri](https://msdn.microsoft.com/library/ms247258.aspx) (MSDN).
- [Dinamik veri sağlayıcı ve EntityDataSource denetimi bu sürüm için Entity Framework 6 Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/02/28/announcing-the-release-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx) (Microsoft Web geliştirme blog).

<a id="wfdbc"></a>

### <a name="using-web-forms-data-bound-controls-and-data-binding-expressions"></a>Forms veri bağlama denetimleri ve veri bağlama ifadeleri Web kullanma

- [Model bağlama ve Web Forms](https://go.microsoft.com/fwlink/?LinkId=286117). EF Code First kullandığı öğretici serisinin.
- [Veri alma görüntülemek ASP.NET 4.5 Web Forms - başlatılan öğeleri ve ayrıntıları](../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details.md).
- [Kesin türü belirtilmiş veri denetimleri](https://weblogs.asp.net/scottgu/archive/2011/09/02/strongly-typed-data-controls-asp-net-vnext-series.aspx) (Scott Guthrie'nın blogu).
- [Kesin türü belirtilmiş veri denetimleri](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [ASP.NET 4.5 Web formları güçlü yazılan veri denetimleri](../web-forms/videos/aspnet-web-forms-vnext/aspnet-vnext-videos-strongly-typed-data-controls.md) (video).
- [Veri bağlama Web sunucusu denetimleri](https://msdn.microsoft.com/library/ms228214.aspx) (MSDN).
- [Veri bağlama ifadeleri genel bakış](https://msdn.microsoft.com/library/ms178366.aspx) (MSDN). Bu sayfa yalnızca kapsar **Eval** ve **bağlamak**; eklenecek güncelleştirilmemiş **öğesi** ve **BindItem**.

<a id="sqlserver"></a>

## <a name="working-with-sql-server-databases"></a>SQL Server veritabanları ile çalışma

- [SQL Server veritabanı özellikleri](https://msdn.microsoft.com/library/hh230827.aspx) (MSDN). Çok çeşitli SQL Server konuları genel bir giriş için bunu İçindekiler altındaki girişleri bakın.
- [SQL Server sürümleri](https://msdn.microsoft.com/library/ms178359.aspx#sqlserver) (MSDN). Bir özeti, her biri hakkında daha fazla bilgi için bağlantılar ile birlikte kullanılabilir SQL Server sürümlerinde.)
- [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [ASP.NET Web uygulamaları için SQL Server Compact kullanarak](https://msdn.microsoft.com/library/ms247257.aspx) (MSDN).
- [Microsoft SQL Server: Veritabanı ürün örnekleri](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Örnek AdventureWorks veritabanları.
- [Örnek veritabanları yükleme](https://github.com/Microsoft/sql-server-samples/blob/master/samples/databases/adventure-works/README.md). Burada gösterilen yöntemlerine ek olarak, ayrıca örnek .mdf dosyaları birini uygulamaya indirebilirsiniz\_bir web projesi veri klasörü veritabanını yerel veritabanına dönüştürmek ve bir yerel veritabanı bağlantı dizesi oluşturun. Bunun hakkında daha fazla bilgi için bkz: [nasıl yapılır: yerel veritabanına yükseltme](https://msdn.microsoft.com/library/hh873188.aspx).

Ayrıca SQL Server Express ve LocalDB çalışma ve SQL Server ve SQL veritabanı arasında seçim yapma aşağıdaki bölümlere bakın.

<a id="sslocaldb"></a>

### <a name="working-with-sql-server-express-localdb-databases"></a>SQL Server Express LocalDB veritabanları ile çalışma

- [SQL Server Express 2012 LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) (MSDN). Resmi MSDN giriş LocalDB.
- [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN).
- [Nasıl yapılır: yerel veritabanına yükseltme](https://msdn.microsoft.com/library/hh873188.aspx) (MSDN). .Mdf dosyasını bir önceki sürümünden SQL Server Express LocalDB için geçiş yapma. Aşağıdakilerden birini karşıdan yüklemeniz halinde bu süreçte gitmek de [SQL Server 2012 örnek veritabanları](https://go.microsoft.com/fwlink/?linkid=117483).
- [Yerel veritabanı, geliştirilmiş SQL Express Tanıtımı](https://go.microsoft.com/fwlink/?LinkId=234375) (SQL Server Express blog). MSDN dahil daha LocalDB neden oluşturulmuş hakkında daha fazla arka plana sahiptir.
- [LocalDB: My veritabanı nerede?](https://go.microsoft.com/fwlink/?LinkId=234376) (SQL Server Express blog). Yerel veritabanı veritabanı dosyaları oluşturulduğu hakkında bilgi.
- [Tam IIS ile 1. Bölüm LocalDB kullanımıyla: kullanıcı profili](https://blogs.msdn.com/b/sqlexpress/archive/2011/12/09/using-localdb-with-full-iis-part-1-user-profile.aspx) (SQL Server Express blog). Yerel veritabanı, IIS ile çalışmak üzere tasarlanmamıştır. Bu blog gönderileri dizi sorunları ve bazı geçici çözümler açıklanmaktadır.

<a id="sse"></a>

### <a name="working-with-sql-server-express-databases"></a>SQL Server Express veritabanları ile çalışma

- [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx) (MSDN). SQL Server Express ile AttachDBFileName bağlantı dizesi ayarı kullanırsanız, özellikle bu sayfa kullanıcı örneği bölümüne bakın.
- [Yerel SQL Server Express 2008 sahipliğini almak nasıl](https://blogs.msdn.com/b/sqlexpress/archive/2010/02/23/how-to-take-ownership-of-your-local-sql-server-2008-express.aspx) (SQL Server Express blog). Ortak bir sorun SQL Server Express örneği üzerinde yönetici olmadığınız için SQL Server Express veritabanları ile çalışabilmek için alınmıyor. Varsayılan olarak, SQL Server Express yükleyen kişi bir yöneticidir. Bu blog bilgisayarda bir yönetici değilseniz kendiniz bir SQL Server Express Yöneticisi nasıl yapılacağını açıklar.
- [My ASP.NET web uygulaması bir SQL Server Express veritabanı üretimde kullanabilir miyim?](https://msdn.microsoft.com/library/jj653753.aspx#sql_express_in_production) (MSDN).

<a id="ssdb"></a>

### <a name="working-with-windows-azure-sql-database"></a>Windows Azure SQL veritabanı ile çalışma

- [Windows Azure Web sitesine üyeliği, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data) (Microsoft Azure site).
- [SQL veritabanları](https://docs.microsoft.com/azure/sql-database/) (Microsoft Azure site). Başlatılan öğreticiler ve nasıl yapılır kılavuzları alınıyor.
- [Windows Azure SQL veritabanı](https://msdn.microsoft.com/library/windowsazure/ee336279.aspx) (MSDN). MSDN SQL veritabanında İçindekiler en üst düzey düğüm.
- [Windows Azure SQL veritabanı TechNet Wiki makaleleri dizini](https://social.technet.microsoft.com/wiki/contents/articles/2267.windows-azure-sql-database-technet-wiki-articles-index-en-us.aspx) (Microsoft TechNet sitesini).
- [Geçici hata işleme uygulama blok](https://msdn.microsoft.com/library/hh680934(v=PandP.50).aspx). Azaltma gelen sonucunda ortaya çıkan geçici ağ hatalarını ve bağlantı hatalarını işlemek için sağlayan bir çerçeve. Bir NuGet paketi olarak kullanılabilir: [Kurumsal kitaplığı 5.0 - geçici hata işleme uygulama blok](http://nuget.org/packages/EnterpriseLibrary.WindowsAzure.TransientFaultHandling).
- [SQL veritabanı ve Entity Framework ile çalışmaya başlama](https://msdn.microsoft.com/data/jj556244) (MSDN).
- [Windows Azure eğitim Seti](https://www.microsoft.com/download/details.aspx?id=8396) (Microsoft İndirme Merkezi). SQL veritabanı için uygulamalı labs içerir.
- [Windows Azure SQL veritabanı Topluluk Forumu](https://social.msdn.microsoft.com/Forums/ssdsgetstarted/threads).
- [Windows Azure SQL veritabanına taşıma](https://msdn.microsoft.com/library/ff803375.aspx) (MSDN). Microsoft Patterns and Practices ekibi tarafından kapsamlı bir uçtan uca senaryonun bir bölüm. Neden geçirmek isteyebileceğiniz kapsar ve SQL Server'dan SQL veritabanına geçirme.
- [SQL Server veritabanları Windows Azure SQL veritabanına geçirme](https://msdn.microsoft.com/library/windowsazure/jj156160.aspx) (MSDN).
- [SQL veritabanı Geçiş Sihirbazı'nı](http://sqlazuremw.codeplex.com/). Geçiş için bir açık kaynak aracı için ve SQL veritabanından veritabanları.

<a id="ssdbchoosing"></a>

### <a name="choosing-between-sql-server-and-windows-azure-sql-database"></a>SQL Server ve Windows Azure SQL veritabanı arasında seçim yapma

- [SQL Server Windows Azure SQL veritabanı ile karşılaştırmak](https://social.technet.microsoft.com/wiki/contents/articles/996.compare-sql-server-with-windows-azure-sql-database-en-us.aspx) (Microsoft TechNet sitesini).
- [Windows Azure SQL veritabanına veri geçişi: araçları ve teknikleri](https://msdn.microsoft.com/library/windowsazure/hh694043.aspx) (MSDN). SQL veritabanı için SQL Server karşılaştırın ve ne zaman SQL Server'dan SQL veritabanına geçirme hakkında kılavuzluk bölümleri içerir.
- [Windows Azure SQL veritabanı teslim Kılavuzu](https://social.technet.microsoft.com/wiki/contents/articles/3398.windows-azure-sql-database-delivery-guide-en-us.aspx) (Microsoft TechNet sitesini).
- [SQL Server özellik kısıtlamaları (Windows Azure SQL veritabanı)](https://msdn.microsoft.com/library/windowsazure/ff394115.aspx) (MSDN).
- [Windows Azure tablo depolaması ve Windows Azure SQL veritabanı - karşılaştırılan ve Contrasted](https://msdn.microsoft.com/library/jj553018.aspx) (MSDN). Windows Azure dağıtımı bir uygulama için Windows Azure Table depolama Windows Azure SQL veritabanı için bir alternatif olabilir. Bu konu, aşağıdaki alternatifleri arasında karar vermenize yardımcı olur.
- [Windows Azure SQL veritabanı](https://msdn.microsoft.com/library/windowsazure/ee336279) (MSDN).
- [Kuralları ve sınırlamaları (Windows Azure SQL veritabanı)](https://msdn.microsoft.com/library/windowsazure/ff394102.aspx)

<a id="nosql"></a>

## <a name="working-with-nosql-database-management-systems"></a>NoSQL veritabanı yönetim sistemleri ile çalışma

- [Windows Azure Data Services](https://www.windowsazure.com/develop/net/data/) (Microsoft Azure site). Bkz: [tablo hizmeti özellik Kılavuzu](https://docs.microsoft.com/azure/) ve **büyük veri** sayfasının bölümünde.
- [ASP.NET çok katmanlı uygulama kullanarak depolama tabloları, kuyrukları ve Blobları](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (Microsoft Azure site). Windows Azure depolama NoSQL tabloları kullanan indirilebilir örnek uygulama ile uçtan uca öğretici.

<a id="linq"></a>

## <a name="using-linq-queries-in-aspnet-applications"></a>LINQ sorgularını ASP.NET uygulamalarında kullanma

- [ASP.NET veri erişim seçenekleri](https://msdn.microsoft.com/library/ms178359.aspx#linq) (MSDN). LINQ bir giriş içerir.
- [LINQ eğitim videoları](http://www.misfitgeek.com/windows-client-linq-training-videos-20/) (Joe Stagner'ın blogu).
- [ASP.NET Forumu iş parçacığı dinamik LINQ kaynaklarına bağlantılar ile](https://forums.asp.net/p/1961037/5601994.aspx?Please+suggest+good+books+so+that+one+can+write+and+understand+dynamic+linq).

<a id="dd"></a>

## <a name="using-dynamic-data-scaffolding"></a>Using Dynamic Data Scaffolding

- [Dinamik veri proje şablonlarını](https://msdn.microsoft.com/library/jj822927.aspx#dynamicdata) (MSDN). Dinamik veri projeleri kullanmak ne zaman hakkında yönergeler.
- [ASP.NET dinamik veri](https://msdn.microsoft.com/library/ee845452.aspx) (MSDN).

<a id="securing"></a>

## <a name="securing-data-access"></a>Veri erişimi güvenli hale getirme

- [ASP.NET veri erişimi güvenli hale getirme](https://msdn.microsoft.com/library/ms178375.aspx) (MSDN).
- [Güvenlik konuları (Entity Framework)](https://msdn.microsoft.com/library/cc716760.aspx) (MSDN).
- [Nasıl yapılır: Güvenli bağlantı dizeleri veri kaynağı denetimleri kullanırken](https://msdn.microsoft.com/library/ms178372.aspx) (MSDN).

<a id="optimizingdataaccess"></a>

## <a name="optimizing-data-access-performance"></a>Veri erişim performansını en iyi duruma getirme

- [ASP.NET Performans genel bakış](https://msdn.microsoft.com/library/cc668225.aspx) (MSDN).
- [ASP.NET önbelleğe alma](https://msdn.microsoft.com/library/xsbfdd8c.aspx) (MSDN).
- [ASP.NET performansı iyileştirme](https://msdn.microsoft.com/library/ff647787) (MSDN). Bu sayfanın üstündeki "Devre dışı bırakılan içerik" bir uyarı var. ancak bilgilerin çoğu hala ilgili olup olmadığını karşılaştırılabilir güncelleştirilmiş kaynak yok.
- [SQL Server performansını artırma](https://msdn.microsoft.com/library/ff647793) (MSDN). Önceki bağlantı olarak aynı açıklama.

Ayrıca bkz. [en iyi duruma getirme Entity Framework performans](#optimizingef) bu konuda daha önce.

<a id="deploying"></a>

## <a name="deploying-a-database"></a>Bir veritabanı dağıtma

- [ASP.NET Web dağıtımı - önerilen kaynakları](aspnet-web-deployment-content-map.md) (MSDN).

<a id="webservice"></a>

## <a name="accessing-data-through-a-web-service"></a>Web hizmeti aracılığıyla verilere erişme

- [Web hizmeti aracılığıyla verilere](https://msdn.microsoft.com/library/ms178359.aspx#webservice) (MSDN). WCF ile Web API kullanma zamanı hakkında yönergeler.
- [ASP.NET Web API ile çalışmaya başlama](../web-api/index.md).
- [WCF Data Services](https://msdn.microsoft.com/data/bb931106) (MSDN).

<a id="additional"></a>

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET veri erişimi hakkında SSS](https://msdn.microsoft.com/library/jj653753.aspx) (MSDN).
- [ASP.NET Web Forms öğreticileri - veri](../web-forms/overview/data-access/index.md). Bu öğreticiler görece eski çoğu; Okuma emin olun [ASP.NET veri erişimi seçenekleri](https://msdn.microsoft.com/library/ms178359.aspx) ve [veri depolama seçenekleri (yapı gerçek bulut uygulamaları Windows Azure ile)](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options.md) ilk böylece doğru olmayan çok veri erişim yöntemi alma Senaryonuz için.
- [ASP.NET MVC içerik haritası](../mvc/overview/getting-started/recommended-resources-for-mvc.md).
- [ASP.NET Web Pages öğreticileri - veri](../web-pages/overview/data/index.md).
- [Visual Studio'da veri erişimi](https://msdn.microsoft.com/library/wzabh8c4.aspx) (MSDN). Bağlantılar benzer bir listesini sağlar bu içerik haritası ancak ASP.NET yerine Visual Studio odaklanır.
