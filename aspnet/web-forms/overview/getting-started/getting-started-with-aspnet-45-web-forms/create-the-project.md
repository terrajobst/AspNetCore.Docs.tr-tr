---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Projeyi oluşturmak | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 754f085e3e43f7efa155f410d02a0d29d3349612
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912299"
---
<a name="create-the-project"></a>Proje oluşturma
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide oluşturmak, inceleyin ve ASP.NET özellikleriyle öğrenmenize olanak sağlayacak Visual Studio'da varsayılan proje çalıştırın. Ayrıca, Visual Studio ortamını gözden geçirirsiniz.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Yeni bir Web formları projesi oluşturma
- Web formları projesi dosya yapısı.
- Visual Studio'da proje çalıştırmayı öğrenin.
- Varsayılan Web forms uygulaması farklı özellikleri.
- Visual Studio ortamını kullanma hakkında bazı temel bilgileri.

## <a name="creating-the-project"></a>Projeyi Oluşturma

1. Visual Studio'yu açın.
2. Seçin **yeni proje** gelen **dosya** Visual Studio'daki menü. 

    ![Yeni Proje menüsü öğesi - proje oluşturma](create-the-project/_static/image1.png)
3. Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki şablonları grubu.
4. Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.  
 Bu öğretici serisinde, .NET Framework 4.5.2 kullanıyor.
5. Projenizi adlandırın *WingtipToys* ve **Tamam** düğmesi. 

    ![Takım projesi - yeni proje iletişim kutusu oluşturma](create-the-project/_static/image2.png)

    > [!NOTE]
    > Bu öğretici serisine proje adı **WingtipToys**. Bu kullanmanız önerilir *tam* böylece kodu beklendiği gibi işlevleri öğretici serisinin sağlanan proje adı.

6. Tıklayın **kimlik doğrulamayı Değiştir** düğmesi. Seçin **bireysel kullanıcı hesapları** tıklatıp **Tamam** düğmesi.

7. Seçin **Web Forms** şablonu ve tıklatın **Tamam** düğmesi.

    ![Yeni Proje şablonu - proje oluşturma](create-the-project/_static/image3.png)

Projeyi oluşturmak için biraz zaman alabilir. Hazır olduğunda, açma **Default.aspx** sayfası.

![Yeni Proje şablonu - proje oluşturma](create-the-project/_static/image4.png)

Arasında geçiş yapabilirsiniz **tasarım** görünümü ve **kaynak** merkezi pencerenin alt kısmındaki bir seçenek belirleyerek görünümü. **Tasarım** görüntüleyen ASP.NET Web sayfaları, ana sayfalar, içerik sayfaları, HTML sayfaları ve kullanıcı denetimleri WYSIWYG yakın görünümünü kullanarak. **Kaynak** görüntüleyen düzenleyebileceğiniz Web sayfanızın, HTML biçimlendirmesi.

> [!TIP] 
> 
> **ASP.NET Framework anlama**
> 
> ASP.NET Web Forms, tanıdık bir Sürükle ve bırak, olay odaklı modeli kullanarak dinamik build Web siteleri olanak tanır. Bir tasarım yüzeyi ve denetimleri ve bileşenleri yüzlerce Gelişmiş, güçlü kullanıcı Arabirimi denetimli siteleri veri erişimi ile hızlı bir şekilde oluşturmanızı sağlar. Wingtip çocuğunun Store, ASP.NET Web formlarını temel, ancak bu öğretici serisinde şunların kavramlardan bir çoğunu tüm ASP.NET için geçerlidir.
> 
> ASP.NET dört birincil geliştirme çerçeveleri sunar:
> 
> - [ASP.NET Web formları](../../../index.md)  
>  Microsoft Windows Forms (WinForms) ve XAML/WPF/Silverlight gibi bildirim temelli ve denetim tabanlı programlama tercih eden geliştiriciler Web formları framework hedefler. Popüler bir web geliştirme için hızlı uygulama geliştirme (RAD) ortamı aranıyor geliştiricilerle, bu nedenle bir WYSIWYG Tasarımcısı odaklı geliştirme modeli sunar. Web programlama için yenidir ve geleneksel Microsoft RAD istemci geliştirme araçları (örneğin, Visual Basic ve Visual C#) ile ilgili bilgi sahibi olduğunuz, HTML ve JavaScript deneyimi gerek kalmadan hızla bir web uygulaması oluşturabilirsiniz.
> - [ASP.NET MVC](../../../../mvc/index.md)  
>  ASP.NET MVC, desenleri ve teste dayalı geliştirme, görev ayrımı nettir, tersine çevirme (IOC) denetim ve bağımlılık ekleme (dı) gibi ilkeler ilgilendiğiniz geliştiriciler hedefler. Bu çerçeve, iş mantığı katmanı kendi sunu katmanına bir web uygulamasından ayırarak teşvik eder.
> - [ASP.NET Web Sayfaları](../../../../web-pages/index.md)  
>  ASP.NET Web sayfaları, basit bir web geliştirme Öykü PHP satırları boyunca isteyen geliştiriciler hedefler. Web sayfaları modelinde, HTML sayfaları oluşturmak ve ardından sunucu tabanlı kodu dinamik olarak bu işaretleme nasıl oluşturulacağını denetlemek için sayfaya ekleyin. Web sayfaları özellikle basit bir çerçeve olacak şekilde tasarlanmıştır ve ASP.NET kolay giriş noktası kişilerin HTML biliyorsanız ancak geniş kapsamlı bir programlama deneyimi - örneğin olmayabilir, Öğrenciler veya amatörler adıdır. Ayrıca, PHP veya benzer çerçeveler ASP.NET kullanmaya başlamak için bilen web geliştiricileri için en iyi yolu olan.
> - [ASP.NET tek sayfalı uygulama](../../../../single-page-application/index.md)  
>  ASP.NET tek sayfalı uygulama (SPA) HTML 5, CSS 3 ve JavaScript kullanarak önemli istemci tarafı etkileşimler içeren uygulamaları oluşturmanıza yardımcı olur. ASP.NET ve Web Araçları 2012.2 güncelleştirme knockout.js ve ASP.NET Web API'sini kullanarak tek sayfalı uygulamalar oluşturmaya yönelik yeni bir şablon birlikte gelir. Ek olarak yeni SPA şablonu, topluluk tarafından oluşturulan yeni SPA şablonları indirmek için mevcuttur.
> 
> Dört ana geliştirme çerçeveleri ek olarak, ASP.NET farkında ve aşina olmanız gerekir, ancak bu öğretici serisinde kapsamında olmayan ek teknolojilerin de sunar:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP Hizmetleri oluşturmaya yönelik bir çerçeve.
> - [ASP.NET SignalR](../../../../signalr/index.md) -geliştirme gerçek zamanlı web işlevselliği sağlayan bir kitaplık.


### <a name="reviewing-the-project"></a>Projeyi gözden geçirme

Visual Studio'da **Çözüm Gezgini** penceresi projenin dosyalarını yönetmenize olanak tanır. Uygulamanızda eklenmiş klasörleri bir göz atalım **Çözüm Gezgini**. Web uygulaması şablonu temel klasör yapısı ekler:

![Çözüm Gezgini - proje oluşturma](create-the-project/_static/image5.png)

Visual Studio, bazı Başlangıç klasörleri ve projeniz için dosyaları oluşturur. İle bu öğreticinin ilerleyen bölümlerinde çalışacak ilk dosyaları şunlardır:

| **Dosya** | **Amaç** |
| --- | --- |
| *Default.aspx* | Bir tarayıcıda uygulama çalıştırıldığında görüntülenen genellikle ilk sayfa. |
| *Site.Master* | Uygulamanızda sayfalar için tutarlı bir düzen ve kullanım standart davranışı oluşturmak izin veren bir sayfa. |
| *Global.asax* | ASP.NET veya HTTP modülleri tarafından oluşturulan uygulama ve oturum düzeyinde olaylara yanıt vermek için kod içeren isteğe bağlı bir dosya. |
| *Web.config* | Bir uygulama yapılandırma verileri. |

### <a name="running-the-default-web-application"></a>Varsayılan Web uygulamasını çalıştırma

Varsayılan Web uygulaması, yerleşik işlevleri ve Destek dayalı zengin bir deneyim sunar. Varsayılan Web formları projesi için hiçbir değişiklik yapmadan uygulama yerel Web tarayıcınız üzerinde çalıştırmak hazırdır.

1. Tuşuna ***F5*** tuşunu Visual Studio'da.   
 Uygulama oluşturun ve Web tarayıcınızda görüntüleme.  

    ![Proje oluştur - varsayılan sayfa](create-the-project/_static/image6.png)
2. Tamamlanan gözden geçirme çalışan uygulama oluşturduktan sonra tarayıcı penceresini kapatın.

Bu varsayılan Web uygulamasında üç ana sayfası vardır: *Default.aspx* (giriş), *About.aspx*, ve *Contact.aspx*. Bu sayfaların her biri üst gezinti çubuğundan erişilebilir. De vardır hesap klasöründe yer alan iki ek sayfalar, Register.aspx sayfası ve Login.aspx sayfasına. Bu iki sayfa oluşturulacağı, depolanacağı ve kullanıcı kimlik bilgilerini doğrulamak için ASP.NET üyelik yeteneklerini kullanmanıza olanak sağlar.

## <a name="aspnet-web-forms-background"></a>Arka plan ASP.NET Web formları

ASP.NET Web Forms, sunucu üzerinde dinamik olarak çalışan kod tarayıcı veya istemci cihaza bir Web sayfası çıktı üretir, Microsoft ASP.NET teknolojisini temel alan sayfalarıdır. Bir ASP.NET Web Forms sayfası doğru tarayıcı uyumlu HTML stillerini, Düzen ve benzeri gibi özellikleri için otomatik olarak işler. Web Forms .NET ortak dil çalışma zamanı, Microsoft Visual Basic ve Microsoft Visual C# gibi tarafından desteklenen herhangi bir dil ile uyumludur. Web Forms ayrıca yerleşik [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), yönetilen ortamı, tür güvenliği ve devralma gibi avantajlar sağlar.

Bir ASP.NET Web Forms sayfası çalıştığında, bir dizi işlem gerçekleştiren bir yaşam döngüsü boyunca sayfasına gider. Bu adımlar, denetimleri örnekleme, geri yükleme ve durum koruma, olay işleyici kodu çalıştıran ve işleme başlatma içerir. ASP.NET Web Forms gücüyle daha fazla bilgi sahibi oldukça sağlayacağınızı kavramanız önemlidir [ASP.NET sayfa yaşam döngüsü](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) böylece için istediğinize etkisi uygun yaşam döngüsü aşamasında kod yazabilirsiniz.

Bir Web sunucusu bir sayfa için bir istek aldığında, bu sayfanın bulur, işler, tarayıcıya gönderir ve sonra tüm sayfa bilgileri atar. Kullanıcı aynı sayfayı yeniden isterse, sunucunun sayfanın sıfırdan yeniden işlemeyerek dizinin tamamıyla tekrarlar. Bir sunucu işlenen sayfaları olduğunu bellek sayfalarının sahip başka bir deyişle, durum bilgisiz olduğundan. ASP.NET sayfası framework sayfanız ve denetimlerinin durumunu bakım görevini otomatik olarak işler ve uygulamaya özgü bilgilerin durumunu korumak üzere açık yol sağlar.

> [!TIP] 
> 
> **Web Forms uygulaması şablonu, Web uygulaması özellikleri**
> 
> ASP.NET Web Forms uygulaması şablonu, Zengin yerleşik işlevleri sağlar. Yalnızca size sağladığı bir *Home.aspx* sayfasında, bir *About.aspx* sayfasında, bir *Contact.aspx* sayfasında, ancak aynı zamanda kullanıcıların kaydeder ve kaydeden üyelik işlevselliğini içerir kimlik bilgilerini ve böylece kullanıcılar Web sitenize oturum açabilirsiniz. Bu genel bakışta ASP.NET Web Forms uygulaması şablonu ve Wingtip Toys uygulamada nasıl kullanılacağını bulunan özelliklerinden bazıları hakkında daha fazla bilgi sağlar.
> 
> **Üyelik**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) kimlik kullanıcılarınızın kimlik bilgileri uygulama tarafından oluşturulan bir veritabanında depolar. Kullanıcılarınız oturum açtığında, uygulama veritabanını okuyarak, kimlik bilgilerini doğrular. Projenizin *hesabı* klasörü üyelik çeşitli bölümlerini uygulama dosyalarını içerir: kaydetme, oturum açma, parola değiştirme ve erişimi yetkilendirme. Ayrıca, ASP.NET Web Forms, OAuth ve Openıd destekler. Bu kimlik doğrulama geliştirmeler, sitenizi Facebook, Twitter, Windows Live ve Google gibi hesaplarından var olan kimlik bilgilerini kullanarak oturum açmasına imkan tanıyın.
> 
> ![Çözüm Gezgini (ASP.NET Identity) - proje oluşturma](create-the-project/_static/image7.png)
> 
> Varsayılan olarak, şablon, varsayılan veritabanı adı bir SQL Server Express LocalDB, Visual Studio Express 2013 Web ile birlikte gelen geliştirme veritabanı sunucusunu örneğini kullanarak bir üyelik veritabanı oluşturur.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) bir SQL Server veritabanının birçok programlama özellikleri bulunan SQL Server'ın basit bir sürümüdür. SQL Server Express LocalDB kullanıcı modunda çalışır ve yükleme önkoşulları kısa bir listesi olan hızlı, sıfır yapılandırmalı bir yüklemesi. Microsoft SQL Server, bir veritabanı veya Transact-SQL kodunu SQL Server Express Localdb'den SQL Server ve SQL Azure için herhangi bir yükseltme adımı taşınabilir. Bu nedenle, SQL Server Express LocalDB, SQL Server'ın tüm sürümlerini hedefleyen uygulamalar için bir geliştirme ortamı olarak kullanılabilir. SQL Server Express LocalDB, SQL Server Compact'ta kullanılabilir olmayan saklı yordamlar, kullanıcı tanımlı işlevler ve toplamlar, .NET Framework tümleştirmesi, uzamsal türler ve diğerleri gibi özellikler sağlar.
> 
> **Ana Sayfalar**
> 
> Bir [ASP.NET ana sayfası](https://msdn.microsoft.com/library/wtxbf3hh.aspx) tutarlı bir görünüm ve davranış tüm sayfalar için uygulamanızda tanımlar. İçeriği kullanıcının gördüğü son sayfa üretmek için tek bir içerik sayfasından ana sayfa düzenini birleştirir. Wingtip Toys uygulamasında, değişiklik *Site.master* Wingtip Toys Web sitesi tüm sayfalarında aynı farklı logo ve gezinti çubuğu paylaşmak için ana sayfa.
> 
> **HTML5**
> 
> ASP.NET Web Forms uygulaması şablonu destekler [HTML5](http://www.w3schools.com/html/html5_intro.asp), HTML biçimlendirme dili en son sürümünü olduğu. HTML5, yeni öğeler ve Web siteleri oluşturmak kolay hale getirmek işlevselliği destekler.
> 
> **Modernizr**
> 
> HTML5 desteklemeyen tarayıcılar için kullanabileceğiniz [Modernizr](http://www.modernizr.com/). Modernizr bir tarayıcı HTML5 özellikleri destekler ve mevcut değilse bunları etkinleştirmek isteyip algılayabilen açık kaynaklı JavaScript kitaplığıdır. ASP.NET Web Forms uygulaması şablonunda Modernizr bir NuGet paketi olarak yüklenir.
> 
> **Önyükleme**
> 
> Visual Studio 2013 proje şablonlarını kullanma [önyükleme](http://getbootstrap.com/), Twitter tarafından oluşturulan bir düzen ve Tema oluşturma çerçevesi. Önyükleme CSS3 düzenleri farklı bir tarayıcı penceresi boyutları için dinamik olarak uyum sağlayabilen anlamına gelir esnek tasarım sağlamak için kullanır. Bir değişiklik uygulama görünümü sunmalarına kolayca etkilemek için önyükleme'nın Tema oluşturma özelliğini de kullanabilirsiniz. Varsayılan olarak, Visual Studio 2013'te ASP.NET Web uygulaması şablonu, bir NuGet paketi olarak önyükleme içerir.
> 
> **NuGet paketleri**
> 
> ASP.NET Web Forms uygulaması şablonu, bir dizi içerir [NuGet](http://www.nuget.org/) paketleri. Bu paketler, açık kaynak kitaplıkları ve araçları biçiminde bileşenlerden işlevselliği sağlar. Çok çeşitli oluşturmanıza ve uygulamalarınızı test etmek amacıyla paketleri yoktur. Visual Studio eklemek, kaldırmak ve NuGet paketlerini güncelleştirme daha kolay hale getirir. Geliştiriciler, oluşturabilir ve paketler için NuGet de ekleyin.
> 
> ![Takım projesi - NuGet iletişim kutusu oluşturma](create-the-project/_static/image8.png)
> 
> Bir paketi yüklediğinizde, NuGet çözümünüze dosyaları kopyalar ve değişiklikleri başvurular ekleme ve Web uygulamanızla ilişkili yapılandırmasını değiştirme gibi gereklidir otomatik olarak yapar. Kitaplık kaldırmaya karar verirseniz, NuGet dosyaları kaldırır ve böylece hiçbir dağınıklığı sol projenizde yapılan değişiklikleri geri alır. NuGet kullanılabilir **Araçları** Visual Studio'daki menü.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) bir hızlı ve anlaşılır JavaScript, HTML belge geçiş yapma, olay işleme, animasyon ekleme ve hızlı web geliştirme için Ajax etkileşimleri basitleştiren kitaplığıdır. JQuery JavaScript kitaplığı NuGet paketi olarak ASP.NET Web Forms uygulaması şablonuna dahil edilir.
> 
> **Örtük doğrulama**
> 
> Yerleşik doğrulama denetimleri örtük JavaScript için istemci tarafı doğrulama mantığını kullanmak için yapılandırılmış. Bu önemli ölçüde satır sayfa biçimlendirmesi içinde oluşturulan JavaScript azaltır ve genel sayfa boyutunu azaltır. Örtük doğrulama eklenen genel ayar temel ASP.NET Web Forms uygulaması şablonu &lt;appSettings&gt; öğesinin *Web.config* uygulamanın kök dosya.
> 
> **Entity Framework Code First**
> 
> ASP.NET Web Forms uygulaması şablondaki özellikleri yanı sıra Wingtip Toys uygulamanın kullandığı [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), kod odaklı geliştirme verilerle çalışırken sağlayan bir NuGet kitaplığı olduğu. Kısacası, uygulamanız için yazdığınız koda göre veritabanı bölümünü oluşturur. Almak Entity Framework kullanarak ve kesin türü belirtilmiş nesneler olarak veri işleme. Uygulamanızı nasıl verilere erişilebilir ayrıntılarını yerine iş mantığına odaklanabilir olanak sağlar.
> 
> Yüklü kitaplıkları ve ASP.NET Web Forms şablonu ile birlikte gelen paketler hakkında ek bilgi için yüklü NuGet paketleri listesi bakın. Bunu yapmak için Visual Studio'da yeni bir Web Forms proje oluşturun, select **Araçları** > **NuGet Paket Yöneticisi** > **ÇözümiçinNuGetpaketleriniYönet**seçip **yüklü paketleri** içinde **NuGet paketlerini Yönet** iletişim kutusu.

### <a name="touring-visual-studio"></a>Visual Studio hiç yapmadığı bir şey

Visual Studio'da birincil windows dahil **Çözüm Gezgini**, **Sunucu Gezgini** (**veritabanı Gezgini** Express'te), **özellikleri Pencere**, **araç kutusu**, **araç**ve **belge penceresini**.

![Takım projesi - NuGet iletişim kutusu oluşturma](create-the-project/_static/image9.png)

Visual Studio hakkında daha fazla bilgi için bkz. [Visual Web Developer için görsel kılavuz](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Özet

Bu öğreticide, oluşturulan, gözden geçirdikten ve varsayılan Web Forms uygulaması çalıştırın. Varsayılan Web forms uygulaması farklı özelliklerini gözden geçirdikten ve Visual Studio ortamını kullanma hakkında bazı temel bilgileri öğrendiniz. Aşağıdaki öğreticilerde, veri erişim katmanı oluşturursunuz.

## <a name="additional-resources"></a>Ek Kaynaklar

[Doğru programlama modelini seçme](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web Application Projects versus Web sitesi projeleri](https://msdn.microsoft.com/library/dd547590.aspx)   
[ASP.NET Web formları sayfaları genel bakış](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Önceki](introduction-and-overview.md)
> [İleri](create_the_data_access_layer.md)
