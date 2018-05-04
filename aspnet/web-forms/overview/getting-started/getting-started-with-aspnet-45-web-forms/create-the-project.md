---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Proje oluşturma | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 7cfceb38204b6cfd3589a082761273e54ac122ca
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
<a name="create-the-project"></a>Proje oluşturma
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğreticide oluşturmak, gözden geçirin ve Visual Studio'da ASP.NET özellikleriyle aşina olanak tanıyan, varsayılan proje çalıştırın. Ayrıca, Visual Studio ortamı incelenecektir.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Yeni bir Web Forms projesi oluşturma
- Web Forms projenin dosya yapısı.
- Visual Studio'da projeyi çalıştırmak nasıl.
- Varsayılan Web forms uygulamanın farklı özelliklerini.
- Visual Studio ortamında kullanma hakkında bazı temel bilgileri.

## <a name="creating-the-project"></a>Projeyi Oluşturma

1. Visual Studio'yu açın.
2. Seçin **yeni proje** gelen **dosya** Visual Studio menüsünde. 

    ![Proje - yeni proje menü öğesi oluşturma](create-the-project/_static/image1.png)
3. Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki templates grubu.
4. Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.  
 Bu öğretici seri .NET Framework 4.5.2 kullanıyor.
5. Projenizin adı *WingtipToys* ve **Tamam** düğmesi. 

    ![Proje - yeni proje iletişim kutusu oluşturma](create-the-project/_static/image2.png)

    > [!NOTE]
    > Bu öğretici serisinde proje adı **WingtipToys**. Bu kullanmanız önerilir *tam* böylece kodu öğretici serisi beklendiği gibi işlevler sağlanan proje adı.

6. Tıklatın **kimlik doğrulamayı Değiştir** düğmesi. Seçin **tek tek kullanıcı hesaplarını** tıklatıp **Tamam** düğmesi.

7. Seçin **Web Forms** şablonu ve tıklatın **Tamam** düğmesi.

    ![Proje - yeni proje şablonu oluşturma](create-the-project/_static/image3.png)

Projeyi oluşturmak için biraz zaman alır. Hazır olduğunda açmak **Default.aspx** sayfası.

![Proje - yeni proje şablonu oluşturma](create-the-project/_static/image4.png)

Arasında geçiş yapabilirsiniz **tasarım** Görünüm ve **kaynak** merkezi pencerenin altındaki bir seçenek seçerek görünümü. **Tasarım** görüntüleyen ASP.NET Web sayfaları, ana sayfalar, içerik sayfaları, HTML sayfaları ve kullanıcı denetimleri WYSIWYG yakın görünümünü kullanma. **Kaynak** görüntüleyen düzenleyebileceğiniz, Web sayfası, HTML biçimlendirmesi.

> [!TIP] 
> 
> **ASP.NET çerçeveleri anlama**
> 
> ASP.NET Web Forms, bilinen sürükle ve bırak, olay denetimli modeli kullanarak derleme dinamik Web siteleri olanak tanır. Tasarım yüzeyi ile yüzlerce denetim ve bileşen Gelişmiş, güçlü kullanıcı Arabirimi denetimli siteleri veri erişimi olan hızlı bir şekilde oluşturmanızı sağlar. Wingtip Oyuncak deposu üzerinde ASP.NET Web Forms tabanlı, ancak bu öğretici serisinde öğrenin kavramlardan birçoğunu tüm ASP.NET uygulanabilir.
> 
> ASP.NET dört birincil geliştirme çerçeveleri sunar:
> 
> - [ASP.NET Web formları](../../../index.md)  
>  Web Forms framework Microsoft Windows Forms (WinForms) ve XAML/WPF/Silverlight gibi bildirim temelli ve denetim tabanlı programlama tercih geliştiriciler hedefler. Popüler web geliştirme için hızlı uygulama geliştirme (RAD) ortamın isteyen geliştiriciler sahip olacak şekilde bir WYSIWYG Tasarımcısı güdümlü geliştirme modeli sunar. Web programlama için yenidir ve tanıdık araçlarıyla geleneksel Microsoft RAD istemci geliştirme (örneğin, Visual Basic ve Visual C#), HTML ve JavaScript deneyimi yaşamadan hızlı bir şekilde bir web uygulaması oluşturabilirsiniz.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC desenleri ve teste dayalı geliştirme, sorunları ayrılması, tersine çevirme (IOC) denetim ve bağımlılık ekleme (dı) gibi ilkeler ilgilendiğiniz geliştiriciler hedefler. Bu çerçeve, sunu katmanı web uygulamasından iş mantığı katmanı ayırarak önerir.
> - [ASP.NET Web Sayfaları](../../../../web-pages/index.md)  
>  ASP.NET Web sayfaları PHP satırları boyunca bir basit bir web geliştirme Öykü isteyen geliştiriciler hedefler. Web sayfaları modelinde HTML sayfaları oluşturmak ve ardından bu biçimlendirme nasıl işlendiğine dinamik olarak denetlemek için sayfasına sunucu tabanlı kodu ekleyin. Web sayfaları basit bir çerçeve olacak şekilde özel olarak tasarlanmıştır ve ASP.NET kolay giriş noktası HTML bilmeniz ancak geniş bir programlama deneyimi - örneğin olmayabilir kişiler, Öğrenciler veya deneyimli olmayanlar için olur. Ayrıca, PHP veya benzer çerçeveleri ASP.NET kullanmaya başlamak için bilen web geliştiricileri için en iyi yolu değil.
> - [ASP.NET tek sayfa uygulaması](../../../../single-page-application/index.md)  
>  ASP.NET tek sayfa uygulama (SPA) HTML 5, CSS 3 ve JavaScript kullanarak önemli istemci tarafı etkileşimler içeren uygulamalar oluşturmanıza yardımcı olur. ASP.NET ve Web Araçları 2012.2 güncelleştirmesi knockout.js ve ASP.NET Web API kullanarak tek sayfa uygulamaları geliştirmek için yeni bir şablon gelir. Yeni SPA şablonu ek olarak, yeni Topluluk tarafından oluşturulan SPA şablonları indirme için kullanılabilir.
> 
> Dört ana geliştirme çerçeveleri ek olarak, ASP.NET farkında ve aşina olmanız gerekir, ancak bu öğretici serisinde kapsamında olmayan ek teknolojileri de sunar:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP Hizmetleri oluşturmaya yönelik bir çerçeve.
> - [ASP.NET SignalR](../../../../signalr/index.md) -kitaplığa geliştirme gerçek zamanlı web işlevselliği kolaylaştırır.


### <a name="reviewing-the-project"></a>Projeyi gözden geçirme

Visual Studio'da **Çözüm Gezgini** penceresi projenin dosyalarını yönetmenize olanak sağlar. Uygulamanızda eklenmiş klasörleri bir göz atalım **Çözüm Gezgini**. Web uygulaması şablonu temel klasör yapısı ekler:

![Çözüm Gezgini - proje oluşturma](create-the-project/_static/image5.png)

Bazı ilk klasörleri ve dosyaları projeniz için Visual Studio oluşturur. İle daha sonra Bu öğreticide çalışacaksınız ilk dosyaları şunlardır:

| **Dosya** | **Amaç** |
| --- | --- |
| *Default.aspx* | Bir tarayıcıda uygulama çalıştırıldığında görüntülenen genellikle ilk sayfa. |
| *Site.Master* | Sayfalar için tutarlı bir yerleşim ve kullanım standart davranışını uygulamanızda oluşturmanıza olanak tanıyan bir sayfa. |
| *Global.asax* | ASP.NET veya HTTP modülleri tarafından oluşturulan uygulama ve oturum düzeyi olaylara yanıt vermek için kod içeren isteğe bağlı bir dosya. |
| *Web.config* | Bir uygulama için yapılandırma verileri. |

### <a name="running-the-default-web-application"></a>Varsayılan Web uygulaması çalıştıran

Varsayılan Web uygulaması yerleşik işlevsellik ve Destek göre zengin bir deneyim sunar. Varsayılan Web forms projesi için hiçbir değişiklik yapmadan uygulama yerel Web tarayıcınızda çalışmak hazırdır.

1. Tuşuna ***F5*** tuşunu Visual Studio'da.   
 Uygulama oluşturma ve Web tarayıcınızda görüntüleyin.  

    ![Proje oluşturma - varsayılan sayfa](create-the-project/_static/image6.png)
2. Tamamlanan gözden geçirme çalışan uygulama olduktan sonra tarayıcı penceresini kapatın.

Bu varsayılan Web uygulamasında üç ana sayfa vardır: *Default.aspx* (giriş), *About.aspx*, ve *Contact.aspx*. Bu sayfaların her biri üst gezinti çubuğunda erişilebilir. Ayrıca hesap klasöründe bulunan iki ek sayfalar, Register.aspx sayfası ve vardır Login.aspx sayfasına. Bu iki sayfaları oluşturmak, depolamak ve kullanıcı kimlik bilgilerini doğrulamak için ASP.NET üyelik özelliklerini kullanmanızı sağlar.

## <a name="aspnet-web-forms-background"></a>Arka plan ASP.NET Web formları

ASP.NET Web Forms sunucu üzerinde dinamik olarak çalışan bir kod tarayıcı veya istemci aygıt Web sayfası çıkışı oluşturur, Microsoft ASP.NET teknolojisine dayalıdır sayfalarıdır. Bir ASP.NET Web Forms sayfası doğru tarayıcı uyumlu HTML stil, Düzen ve benzeri gibi özellikleri için otomatik olarak oluşturur. Web Forms .NET ortak dil çalışma zamanı, Microsoft Visual Basic ve Microsoft Visual C# gibi tarafından desteklenen herhangi bir dil ile uyumludur. Ayrıca, Web Forms yerleşik olan [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), Yönetilen ortamlarda, tür güvenliği ve devralma gibi fayda sağlar.

Bir ASP.NET Web Forms sayfası çalıştığında, sayfa, bir dizi işleme adımları gerçekleştirir bir yaşam döngüsü boyunca geçer. Bu adımlar denetimlerini örnekleme, geri yükleme ve durumu bakımı, olay işleyici kodu çalıştıran ve işleme başlatma içerir. ASP.NET Web Forms gücünü daha tanıdık olurken, sizin için anlamak önemli olan [ASP.NET sayfası yaşam döngüsü](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) böylece düşündüğünüz etkisi için uygun yaşam döngüsü aşamada kod yazabilirsiniz.

Bir Web sunucusu bir sayfa için bir istek aldığında, bu sayfanın bulur, işler, tarayıcıya gönderir ve tüm sayfa bilgilerini atar. Kullanıcı aynı sayfa yeniden isterse, sunucunun sayfanın en baştan yeniden işlemeyerek tüm dizisi tekrarlar. Bir sunucu işlenen sayfaları sahip bellek sayfalarının sahip başka bir deyişle, durum bilgisiz şunlardır. ASP.NET sayfa çerçevesi sayfanız ve denetimlerinin durumunu sürdürme görevini otomatik olarak yönetir ve uygulamaya özgü bilgileri durumunu korumak için açık yollar sağlar.

> [!TIP] 
> 
> **Web Forms uygulaması şablonu Web uygulama özellikleri**
> 
> ASP.NET Web Forms uygulama şablonu zengin bir yerleşik işlevsellik sağlar. Yalnızca size sağladığı bir *Home.aspx* sayfasında, bir *About.aspx* sayfasında, bir *Contact.aspx* sayfasında, ancak Ayrıca kullanıcıların kaydeder ve kaydeden üyelik işlevselliğini içerir kimlik bilgilerini böylece kullanıcılar Web sitenize oturum açabilirsiniz. Bu genel bakışta ASP.NET Web Forms uygulaması şablonu ve bunların Wingtip Toys uygulamada nasıl kullanıldığı yer alan özelliklerin bazıları hakkında daha fazla bilgi sağlar.
> 
> **Üyelik**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) kimlik uygulama tarafından oluşturulan bir veritabanında, kullanıcıların kimlik bilgilerini depolar. Kullanıcılarınız oturum açtığında uygulama veritabanı okuyarak kimlik bilgilerini doğrular. Projenizin *hesap* klasörü üyelik çeşitli kısımlarını uygulamak dosyaları içerir: kaydetme, oturum açma, parola değiştirme ve erişim yetkisi verme. Ayrıca, ASP.NET Web Forms, OAuth ve Openıd destekler. Bu kimlik doğrulaması geliştirmeleri kullanıcıların, sitenizi Facebook, Twitter, Windows Live ve Google olarak böyle hesaplarından varolan kimlik bilgilerini kullanarak oturum açın izin verin.
> 
> ![Çözüm Gezgini (ASP.NET Identity) - proje oluşturma](create-the-project/_static/image7.png)
> 
> Varsayılan olarak, SQL Server Express LocalDB, Visual Studio Express 2013 Web birlikte geliştirme veritabanı sunucusu örneği üzerinde bir varsayılan veritabanı adını kullanarak bir üyelik veritabanı şablonu oluşturur.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) bir SQL Server veritabanının pek çok programlama özelliklerine sahip SQL Server, hafif bir sürümüdür. SQL Server Express LocalDB kullanıcı modunda çalışır ve yükleme önkoşulları kısa listesine sahip hızlı, sıfır yapılandırmalı bir yüklemesi gerekir. Microsoft SQL Server, tüm veritabanı veya Transact-SQL kodunu SQL Server Express LocalDB SQL Server ve SQL Azure yükseltme adımlar taşınabilir. Bu nedenle, SQL Server Express LocalDB Geliştirici ortamı olarak SQL Server'ın tüm sürümleri hedefleyen uygulamalar için kullanılabilir. SQL Server Express LocalDB SQL Server Compact içinde kullanılabilir değil saklı yordamlar, kullanıcı tanımlı işlevler ve toplamalar, .NET Framework tümleştirme, uzamsal türler ve diğerleri gibi özellikler sağlar.
> 
> **Ana Sayfalar**
> 
> Bir [ASP.NET ana sayfa](https://msdn.microsoft.com/library/wtxbf3hh.aspx) tutarlı bir görünüm ve davranış tüm sayfalar için uygulamanızda tanımlar. Ana sayfanın düzenini kullanıcının gördüğü son sayfasında üretmek için tek bir içerik sayfasının içeriği ile birleştirir. Wingtip Toys uygulamada değişiklik *Site.master* Wingtip Toys Web sitesindeki tüm sayfaları aynı ayırt edici logo ve gezinti çubuğu paylaşmak için ana sayfa.
> 
> **HTML5**
> 
> ASP.NET Web Forms uygulama şablonu destekler [HTML5](http://www.w3schools.com/html/html5_intro.asp), HTML biçimlendirme dili en son sürümünü olduğu. HTML5 yeni öğeleri ve Web siteleri oluşturmayı kolaylaştırmak işlevselliği destekler.
> 
> **Modernizr**
> 
> HTML5 desteklemeyen tarayıcılar için kullandığınız [Modernizr](http://www.modernizr.com/). Modernizr bir tarayıcı HTML5 özelliklerini destekler ve mevcut değilse bunları etkinleştirmek olup olmadığını algılayan bir açık kaynak JavaScript kitaplığıdır. ASP.NET Web Forms uygulaması şablonunda Modernizr bir NuGet paketi olarak yüklenir.
> 
> **önyükleme**
> 
> Visual Studio 2013 proje şablonlarını kullanın [önyükleme](http://getbootstrap.com/), Twitter tarafından oluşturulan bir düzen ve tema altyapısı. Önyükleme CSS3 düzenleri dinamik olarak farklı bir tarayıcı penceresi boyutlarına uyarlayabilirsiniz anlamına gelir esnek tasarım sağlamak için kullanır. Uygulamanın görünüm değişikliği kolayca etkilemek için önyükleme'nın Tema oluşturma özelliğini de kullanabilirsiniz. Varsayılan olarak, ASP.NET Web uygulaması şablonu Visual Studio 2013'te bir NuGet paketi olarak önyükleme içerir.
> 
> **NuGet paketleri**
> 
> ASP.NET Web Forms uygulama şablonu bir dizi içerir [NuGet](http://www.nuget.org/) paketler. Bu paketleri, açık kaynak kitaplıkları ve araçlar biçiminde bileşenlerden oluşan işlevsellik sağlar. Çok çeşitli oluşturma ve uygulamalarınızı test yardımcı olması için paketler yoktur. Visual Studio eklemek, kaldırmak ve NuGet paketlerini güncelleştirmeyi daha kolay hale getirir. Geliştiriciler oluşturun ve paketler için NuGet de ekleyin.
> 
> ![Proje - NuGet iletişim kutusu oluşturma](create-the-project/_static/image8.png)
> 
> Bir paketi yüklediğinizde, NuGet çözümünüze dosyaları kopyalar ve otomatik olarak hangi değişikliklerin, başvurular ekleme ve Web uygulamasıyla ilişkilendirilmiş yapılandırmasını değiştirme gibi gerekli hale getirir. Kitaplık kaldırmaya karar verirseniz, NuGet dosyaları kaldırır ve böylece hiçbir dağınıklığı sol projenizde yapılan değişiklikleri geri alır. NuGet edinilebilir **Araçları** Visual Studio menüsünde.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) bir hızlı ve kısa JavaScript HTML belge çapraz geçiş yapma, olay işleme, animasyon ve hızlı web geliştirme için Ajax etkileşimler basitleştiren kitaplıktır. JQuery JavaScript kitaplığı ASP.NET Web Forms uygulaması şablonunda bir NuGet paketi olarak dahil edilir.
> 
> **Örtük doğrulama**
> 
> Yerleşik doğrulama denetimleri için istemci tarafı doğrulama mantığını örtük JavaScript kullanacak şekilde yapılandırılmış. Bu önemli ölçüde satır sayfa biçimlendirmesi içinde işlenen JavaScript miktarını azaltır ve genel sayfa boyutunu azaltır. Örtük doğrulama eklenen genel ayar temel ASP.NET Web Forms uygulaması şablonu &lt;appSettings&gt; öğesinin *Web.config* dosya uygulama kökü.
> 
> **Entity Framework Code First**
> 
> ASP.NET Web Forms uygulama şablonu yer alan özelliklerin yanı sıra Wingtip Toys uygulamanın kullandığı [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), kod merkezli geliştirme verileriyle çalışırken sağlayan bir NuGet kitaplığı olduğu. Basitçe, uygulamanız için yazdığınız kodu göre veritabanı bölümünü oluşturur. Entity Framework kullanarak, almak ve veri güçlü şekilde yazılan nesnelerin olarak değiştirmek. Bu verileri nasıl erişilir ayrıntılarını yerine, uygulamanızın iş mantığına odaklanmanıza olanak sağlar.
> 
> Yüklü kitaplıkları ve ASP.NET Web Forms şablonu ile birlikte gelen paketler hakkında ek bilgi için yüklü olan NuGet paketlerin listesini bakın. Bunu yapmak için Visual Studio'da yeni bir Web Forms proje oluşturun, select **Araçları**  - &gt; **kitaplık Paket Yöneticisi**  - &gt; **Yönet Çözüm için NuGet paketlerini**seçip **yüklü paketleri** içinde **NuGet paketlerini Yönet** iletişim kutusu.


### <a name="touring-visual-studio"></a>Visual Studio yarış

Visual Studio'da birincil windows dahil **Çözüm Gezgini**, **Sunucu Gezgini** (**Database Explorer** Express'te), **özellikleri Pencere**, **araç**, **araç**ve **belge penceresini**.

![Proje - NuGet iletişim kutusu oluşturma](create-the-project/_static/image9.png)

Visual Studio hakkında daha fazla bilgi için bkz: [Visual Web Developer için görsel kılavuz](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Özet

Bu öğreticide oluşturduğunuz, gözden ve varsayılan Web Forms uygulamayı çalıştırın. Varsayılan Web forms uygulamanın farklı özelliklerini gözden geçirdikten ve Visual Studio ortamında kullanma hakkında bazı temel bilgileri öğrendiniz. Aşağıdaki öğreticilerde veri erişim katmanı oluşturacaksınız.

## <a name="additional-resources"></a>Ek Kaynaklar

[Programlama modeli sağ seçme](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web uygulaması projelerine Web sitesi projeleri karşılaştırması](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web formları sayfaları genel bakış](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Önceki](introduction-and-overview.md)
> [sonraki](create_the_data_access_layer.md)
