---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Veritabanı (C#) dağıtma | Microsoft Docs
author: rick-anderson
description: Bir ASP.NET web uygulaması dağıtma, gerekli dosyalara ve kaynaklara geliştirme ortamından üretim ortamına alma kapsar. Da için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: fe85e583618cbab772c788f9a53509000aa415c5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386543"
---
<a name="deploying-a-database-c"></a>Veritabanı (C#) dağıtma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Bir ASP.NET web uygulaması dağıtma, gerekli dosyalara ve kaynaklara geliştirme ortamından üretim ortamına alma kapsar. Veri tabanlı web uygulamaları için bu veritabanı şemasını ve verileri içerir. Bu öğretici geliştirme ortamından üretim veritabanına başarıyla dağıtmak için gerekli olan adımları ele bir serinin ilk bölümüdür.


## <a name="introduction"></a>Giriş

Bir ASP.NET web uygulaması dağıtma, gerekli dosyalara ve kaynaklara geliştirme ortamından üretim ortamına alma kapsar. Son altı eğitim kursunda basit bir kitap incelemeleri web uygulaması dağıtmayı incelemiştik. Bu Tanıtım site sunucu tarafı kaynaklar - ASP.NET sayfaları, yapılandırma dosyaları, bir dizi anahtardan oluşan bir `Web.sitemap` dosya ve benzeri - istemci tarafı kaynakları birlikte görüntüleri ve CSS dosyaları ister. Ancak, veri odaklı hakkında web uygulamaları? Bir veritabanı kullanan bir web uygulamasını dağıtmak için hangi adımlar atılmalıdır?

Sonraki birkaç öğreticiler biz bir veri odaklı web uygulamasını dağıtmak için gerekli olan adımları ele alınacaktır. Bu öğreticide, sonraki öğretici gerekli yapılandırma değişikliklerini ararken bir veritabanı s şeması ve içerik geliştirme ortamından üretim ortamına alma inceleyerek başlar. Aşağıdaki uygulama Hizmetleri (üyelik, roller, profili vb.) kullanan bir veritabanı dağıtma sorunlarından keşfedeceksiniz.

## <a name="examining-the-updated-book-reviews-web-application"></a>İnceleme güncelleştirildi Kitap incelemeleri Web uygulaması

Bir veri odaklı web uygulamasını dağıtma göstermek için miyim ve basit, statik bir Web sitesi Kitap incelemeleri web uygulamasından veri odaklı bir güncelleştirildi. Önce Bu öğretici s indirme uygulamada iki sürümü olduğundan: bir Web uygulaması proje modeli ve Web sitesi proje modeli kullanan bir kullanır.

Güncelleştirilmiş Kitap incelemeleri web uygulamasının kullandığı bir [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) s sitede depolanan veritabanı `App_Data` klasörü (`~/App_Data/Reviews.mdf`). SQL Server 2008'in bilgisayarınızda yüklü varsa tanıtım hatasız çalıştırmanız gerekir. Daha eski bir sürümü varsa, SQL Server'ın yapabilirsiniz veya ücretsiz SQL Server 2008 Express Edition yükleyebilir veya kendiniz veritabanını oluşturmak için Bu öğreticide s kullanılabilir betikler indirme veritabanını kullanabilirsiniz.

`Reviews.mdf` Veritabanı dört tablo içerir:

- `Genres` -İş teknoloji ve kurgu gibi her bir tür için bir kayıt içerir.
- `Books` -gibi sütunlarla her İnceleme için bir kayıt içerir `Title`, `GenreId`, `ReviewDate`, ve `Review`, diğerlerinin yanı sıra.
- `Authors` -için gözden geçirilmiş bir kitap katkılarıyla her yazar hakkında bilgi içerir.
- `BooksAuthors` -ne yazarların hangi kitapları yazmış belirten bir çoktan çoğa birleştirme tablo.
  

Şekil 1, bu dört tabloların bir ER diyagramda gösterilmektedir.


[![Kitap incelemeleri Web uygulamasının s veritabanı oluşan, dört tablo.](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Şekil 1**: Kitap incelemeleri Web uygulamasının s veritabanı oluşan, dört tablo olduğundan ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image3.jpg))


Önceki sürümü Kitap incelemeleri Web sitesinin her kitap için ayrı bir ASP.NET sayfası vardı. Örneğin, adında bir sayfa oluştu `~/Tech/TYASP35.aspx` gözden geçirmeyi bulunan *öğretin kendiniz ASP.NET 3.5 24 saat içindeki*. Bu yeni veri tabanlı Web sitesinin veritabanı ve tek bir ASP.NET sayfası Review.aspx?ID= depolanan incelemeleri sürümde*Bookıd*, belirtilen kitap için gözden geçirmesi görüntüler. Benzer şekilde, bir Genre.aspx?ID= yoktur*genreId* sayfasını, belirtilen türe gözden geçirilmiş kitaplarda listeler.

Şekil 2 ve 3 show `Genre.aspx` ve `Review.aspx` sayfaları sürüyor. Her sayfa için adres çubuğundaki URL'yi not alın. Şekil 2 BT s Genre.aspx? kimliği 85d164ba 1123 4 c 47 82a0-c8ec75de7e0e =. 85d164ba-1123-4c47-82a0-c8ec75de7e0e olduğundan `GenreId` teknoloji Tarz, "Teknoloji incelemeleri" s sayfa başlığı okuma ve madde işaretli liste değeri bu tarz altında kalan bu incelemeleri sitesinde numaralandırır.


[![Teknoloji tarzı sayfası](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Şekil 2**: teknoloji Tarz sayfa ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image6.jpg))


[![Gözden geçirme için kendiniz ASP.NET 3.5 24 saatte öğretin](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Şekil 3**: gözden geçirilmek üzere *öğretin kendiniz ASP.NET 3.5 24 saat içindeki* ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image9.jpg))


Kitap incelemeleri web uygulaması, ayrıca burada yöneticileri eklemek, düzenlemek ve türleri, incelemeler, silebilir ve bilgi Yazar yönetim bölümündeki içerir. Şu anda, her ziyaretçi yönetim bölümündeki erişebilirsiniz. Bir sonraki öğreticide biz kullanıcı hesapları için destek ekleyecek ve yalnızca yetkili kullanıcılar yönetim sayfalarına izin verir.

Kitap incelemeleri uygulamanın veri temelli bir uygulama dağıtımı göstermek üzere amacı olan etkilenebileceğini Lütfen yüklerseniz. En iyi uygulama tasarımı sunulan ürünün kendinde gerçekleştirilmez. Örneğin, hiçbir ayrı bir veri erişim katmanı (DAL) yoktur; ASP.NET sayfaları SqlDataSource denetimi ya da kendi arka plan kod sınıflardaki ADO.NET kod aracılığıyla veritabanıyla doğrudan iletişim kurar. Katmanlı bir mimari kullanarak veri odaklı uygulamaları oluşturmaya daha derinlemesine bakış için bkz my [ *verilerle çalışma* öğreticiler](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Geliştirme ve üretim veritabanları

Üzerinde veri temelli web uygulaması geliştirme başlattığınızda, veritabanına bağlanmak nasıl uygulama ayrıntıları sağlar. bir veritabanı bağlantı dizesi belirtmeniz gerekir. Başka şeyler, veritabanı sunucusu, veritabanı adı ve güvenlik bilgileri bu bağlantı dizesini belirtir. Çoğunlukla, geliştirme sırasında uygulama tarafından kullanılan veritabanı yüklendiğinde bir veritabanı farklı Bu üretimde s. Üretim ve geliştirme için farklı veritabanlarındaki kullanmanın birçok avantajı vardır. Farklı geliştirme anlamına gelir, veritabanı rsquo sahip yanlışlıkla değiştirmekten veya silmekten canlı verileri hakkında endişelenmeye gerek. Ayrıca, sahte test verilerinizden veya uygulamayı üretim sırasında üzerindeki etkileri hakkında endişelenmenize gerek kalmadan bozucu değişiklikleri veri modeline olun sağlar. Uygulama olduğunda, farklı bir veritabanı olan geliştirme ve üretim ortamlarında sahip dezavantajı veritabanını ve tüm ilgili değişiklikler s veritabanı şemasına dağıtılan veya verileri de dağıtılması gerekir.

İlk dağıtımdan önce veritabanının tek örneğini yoktur ve bu örnek bir geliştirme ortamıdır. Uygulamayı üretim ortamına ilk kez dağıtırken biz yalnızca gerekli sunucu tarafı ve istemci tarafı dosyaları kopyalayın, kalmayıp ayrıca veritabanı geliştirme ortamından üretim ortamına kopyalayın. Bu Kitap incelemeleri web uygulamasıyla - veritabanı bulunan şimdi burada size doğru bekleme, `App_Data` geliştirme ortamımız klasöründe ancak henüz üretim ortamına itilmiş henüz.

Uygulama dağıtıldıktan sonra iki kopyasını veritabanı vardır. Uygulama geliştikçe yeni özellikler eklenebilir, veri modeli için bir değişiklik araya (yeni sütunlar mevcut tablolarına ekleme gibi mevcut sütunları yeni tablo, ekleme, değişiklik yapmak ve benzeri). Sonraki web uygulamasına dağıtıldığında, son dağıtım üretim veritabanı için uygulanması gereken bu yana değişiklikler geliştirme ortamındaki veritabanı uygulanan. Bu işlem yönetmek için bazı stratejiler bir sonraki öğreticide ele alınmıştır. Bu öğreticide, tüm veritabanı geliştirme ortamından üretime dağıtmaya odaklanır.

## <a name="deploying-the-database-to-the-production-environment"></a>Üretim ortamına dağıtma

Bu öğreticinin geri kalanında, veritabanı geliştirme ortamından üretim ortamına dağıtmayı bakar. İzliyorsanız web ana bilgisayar sağlayıcınız hesabınızla Microsoft SQL Server içerdiğinden emin olmak için veritabanı desteği. Ayrıca bazı bilgiler eldeki, yani veritabanı sunucu adı, veritabanı adı ve kullanıcı adı ve veritabanına bağlanmak için kullanılan parola gerekir.

Bu öğreticide daha önce belirtildiği gibi depolanan bir SQL Server 2008 Express Edition veritabanı Kitap incelemeleri s Web sitesi veritabanı olan `App_Data` klasör. Neden böyle bir veritabanı dağıtma kopyalamak kadar basit olduğu bekleme `App_Data` klasör geliştirme ortamından üretim ortamına. Ancak, çoğu web ana bilgisayar sağlayıcıları barındırma veritabanlarında desteklemeyen `App_Data` güvenlik nedeniyle nedeniyle klasörü. Bunun yerine, web ana bilgisayarları, ortam içinde bir SQL Server veritabanı sunucusunda bir hesap sağlayın. Geliştirme ortamınızı veritabanından üretim ortamına dağıtma veritabanınızı web ana bilgisayar s veritabanı sunucusunda kayıtlı alma gerektirir.

Peki, veritabanınızı geliştirme ortamından üretim ortamına elde ederim? Web ana bilgisayar tekliflerinizi Hizmetleri bağlı olarak bunu yapmanın birkaç yolu vardır. DiscountASP.NET gibi bazı ana bilgisayarlar ile veritabanı ya da gerçek bir yedeğini FTP `.mdf` dosya sitenize ve daha sonra Denetim Masası'ndan yedekleme dosyasını geri yüklemek veya eklemek `.mdf` SQL Server veritabanı sunucusu için dosya. Veritabanı dağıtma gibi araçlarla kopyalamak kadar basit `App_Data` klasörüne üretim ortamına ve ardından Denetim Masası ekleniyor. Belki de ilk kez veritabanınızı yayımlamak için kolay ve hızlı bir yolu budur.

Başka bir yaklaşım, veritabanı Yayımlama Sihirbazı'nı kullanmaktır. Veritabanı Yayımlama Sihirbazı'nı kendi tablolarında - tabloları, saklı yordamları, görünümleri, kullanıcı tanımlı işlevler ve diğerleri - veritabanı s şemanızı ve isteğe bağlı olarak, verileri oluşturmak için SQL komutları oluşturan bir Windows Masaüstü uygulamasıdır. SQL Server Management Studio aracılığıyla web ana bilgisayar sağlayıcısı s veritabanı sunucusuna bağlanmak ve ardından üretim veritabanını çoğaltmak için bu betiği yürütün. Daha iyi, web ana bilgisayar sağlayıcınız Microsoft s destekliyorsa [Database Publishing Services](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) Database Publishing Wizard'ı veritabanı sunucusunda sizin adınıza otomatik olarak yürütülen tarafından oluşturulan komut dosyası olabilir. Veritabanı Yayımlama Sihirbazı'nı s veritabanı şemasını ve verileri oluşturan bir komut oluşturduğundan, web ana bilgisayar sağlayıcınız karşıya yüklenen bir ekleme gibi özellikler sunan bağımsız olarak çalışır `.mdf` dosya.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Veritabanı Yayımlama Sihirbazı'nı kullanarak verileri ve veritabanı şeması oluşturmak için SQL komutları oluşturma

Veritabanı Yayımlama Sihirbazı'nı kullanarak Kitap incelemeleri veritabanı üretime dağıtma adımları yol s olanak tanır. Veritabanı Yayımlama Sihirbazı'nı ötesinde veya Visual Studio 2008 kullanıyorsanız, zaten yüklü. Visual Studio 2005 kullanıyorsanız sonra ilk ihtiyacınız olacak [yükleyip](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) Sihirbazı.

Visual Studio'yu açın ve gidin `Reviews.mdf` veritabanı. Visual Web Developer kullanıyorsanız, veritabanı Gezgini'ne gidin; Visual Studio kullanıyorsanız, sunucu Gezgini'ni kullanın. Şekil 4'te gösterildiği `Reviews.mdf` veritabanı Gezgini Visual Web Developer veritabanında. Şekil 4'te gösterildiği gibi `Reviews.mdf` veritabanı dört tablo, üç saklı yordamlar ve kullanıcı tanımlı bir işlev oluşur.


[![Veritabanı Gezgini veya Sunucu Gezgini veritabanı bulun](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Şekil 4**: veritabanı Gezgini veya Sunucu Gezgini veritabanı bulun ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image12.jpg))


Veritabanı adına sağ tıklayın ve bağlam menüsünden "Sağlayıcı Yayımla" seçeneğini seçin. Bu veritabanı Yayımlama Sihirbazı'nı başlatır (bkz: Şekil 5). Karşılama ekranında geçmiş öncelikli İleri'yi tıklatın.


[![Veritabanı Yayımlama Sihirbazı Karşılama ekranı](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Şekil 5**: veritabanı Yayımlama Sihirbazı Karşılama ekranında ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image15.jpg))


Sihirbazın ikinci ekranda Database Publishing Wizard erişilebilir veritabanlarını listeler ve seçili veritabanındaki tüm nesneleri betik mi, yoksa betik hangi nesneleri seçmek için seçmenize olanak sağlar. Uygun veritabanını seçin ve "Seçili veritabanında betik tüm nesneleri" seçeneğini işaretli bırakın.

> [!NOTE]
> Hata alırsanız "veritabanında hiçbir nesne olmadığını *databaseName* Bu sihirbaz tarafından kodlanabilir türlerinin" Şekil 6 üzerinde gösterilen ekranda İleri'ye tıklama, veritabanı dosyanızın yolu aşırı uzun olmadığından emin olun. Veritabanı dosyası yolu çok uzunsa bu hata ortaya çıkabilecek bulundu.


[![Veritabanı Yayımlama Sihirbazı Karşılama ekranı](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Şekil 6**: veritabanı Yayımlama Sihirbazı Karşılama ekranında ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image18.jpg))


Sonraki ekranda bir betik dosyası oluştur veya web ana, destekliyorsa, veritabanını doğrudan web ana bilgisayar sağlayıcısı s veritabanı sunucunuza yayımlayın. Şekil 7 gösterildiği gibi betik dosyasına yazılmış yaşıyorum `C:\REVIEWS.MDF.sql`.


[![Bir dosyaya veritabanı komut dosyası veya uygulamanızın Web ana bilgisayar sağlayıcısına doğrudan yayımlama](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Şekil 7**: bir dosyaya veritabanı komut dosyası veya uygulamanızın Web ana bilgisayar sağlayıcısına doğrudan yayımlama ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image21.jpg))


Sonraki ekranda, betik seçenekleri çeşitli ister. Betik bu varolan nesneleri kaldırmak için kaldırma deyimleri içerip içermeyeceğini belirtebilirsiniz. Bu True, bir veritabanı ilk kez dağıtırken ince varsayar. Hedef veritabanı SQL Server 2000, SQL Server 2005 veya SQL Server 2008 olup olmadığını belirtebilirsiniz. Şema ve veri, betik verilip verilmeyeceğini belirten son olarak, yalnızca verileri veya yalnızca şemayı. Şema nesneleri, tabloları, saklı yordamları, görünümleri ve benzeri koleksiyonudur. Verileri tablolarda bulunan bilgilerdir.

Şekil 8 gösterildiği gibi ben ve var olan veritabanı nesnelerini bırakmayı yapılandırılmış Sihirbazı SQL Server 2008 veritabanı için komut dosyası oluşturmayı ve hem şema hem de veri yayımlamak için alındı.


[![Yayımlama belirtin seçenekleri](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Şekil 8**: yayımlama seçeneklerini belirtin ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image24.jpg))


Son iki ekran hakkında alınması ve komut dosyası durumunu görüntülemek için eylemleri özetler. Sihirbazı çalıştırma net üretim veritabanı oluşturma ve geliştirme gibi aynı verilerle doldurmak için gereken SQL komutlarını içeren bir komut dosyası sahibiz sonucudur.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Üretim ortamı veritabanında SQL komutları yürütme

Biz sahip olduğunuza göre kalan tüm veritabanını ve verileri oluşturmak için SQL komutları içeren üretim veritabanı komut dosyası yürütme betiğidir. Bazı web ana bilgisayar sağlayıcıları textbox SQL komutunu veritabanınızda yürütülecek girebileceğiniz, Denetim Masası'ndaki sunar. Çok büyük komut dosyası olması durumunda bu seçenek çalışmayabilir ( `REVIEWS.MDF.sql` komut dosyasıdır üzerinde 425 KB boyutunda örneği için).

SQL Server Management Studio (SSMS) kullanarak doğrudan üretim veritabanı sunucusuna daha iyi bir yaklaşımdır. Bir Express Edition'ın SQL bilgisayarınızda yüklü Server olmayan varsa sonra büyük olasılıkla yüklü SSMS zaten. Aksi takdirde [yükleyip](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) ücretsiz bir kopyasını SQL Server Management Studio Express Edition.

SSMS'yi başlatın ve web ana bilgisayar sağlayıcınız tarafından sağlanan bilgileri kullanarak web ana bilgisayar s veritabanı sunucunuza bağlanın.


[![Web ana bilgisayar sağlayıcısı s veritabanı sunucusuna bağlanma](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Şekil 9**: Web ana bilgisayar sağlayıcınız s veritabanı sunucusu bağlantısı ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image27.jpg))


Veritabanları sekmesini genişletin ve veritabanınızı bulun. Araç çubuğunun sol üst köşesindeki Yeni sorgu düğmesine tıklayın, veritabanı Yayımlama Sihirbazı tarafından oluşturulan komut dosyasından SQL komutlarını yapıştırın ve üretim veritabanı sunucusunda bu komutları çalıştırmak için YÜRÜT düğmesine tıklayın. Komut dosyanızı özellikle büyük ise, komutları yürütmek için birkaç dakika sürebilir.


[![Web ana bilgisayar sağlayıcısı s veritabanı sunucusuna bağlanma](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Şekil 10**: Web ana bilgisayar sağlayıcınız s veritabanı sunucusu bağlantısı ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image30.jpg))


Tüm var. Bu s için İşte bu kadar! Bu noktada geliştirme veritabanı üretime Tekrarlanmış. SSMS veritabanında yenilerseniz yeni veritabanı nesneleri görmeniz gerekir. Şekil 11 üretim veritabanı s tabloları, saklı yordamlar ve kullanıcı tanımlı işlevleri geliştirme veritabanı üzerindekiler yansıtan gösterir. Ve üretim veritabanı s tabloları biz verileri yayımlamak için veritabanı Yayımlama Sihirbazı belirtildiği için sihirbazı yeniden yürütülmesi zaman geliştirme veritabanı s tablolar aynı verilere sahip. Şekil 12 gösterir verilerde `Books` üretim veritabanında tablo.


[![Üretim veritabanında veritabanı nesnelerini üzere Yinelediniz](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Şekil 11**: veritabanı nesneleri sahip olan yinelenen üretim veritabanında ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image33.jpg))


[![Üretim veritabanı geliştirme veritabanında aynı verileri içerir.](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Şekil 12**: üretim veritabanı geliştirme veritabanı olarak aynı verileri içerir ([tam boyutlu görüntüyü görmek için tıklatın](deploying-a-database-cs/_static/image36.jpg))


Bu noktada biz yalnızca geliştirme veritabanı üretime dağıttım. Biz henüz web uygulaması dağıtma sırasında aranan veya üretim uygulamayı üretim veritabanını kullanmak için gereken yapılandırma değişiklikleri incelenir. Sonraki Öğreticide bu sorunları ele alacağız ediyoruz!

## <a name="summary"></a>Özet

Veri odaklı web uygulamasını dağıtma, üretim ortamına geliştirme sırasında kullanılan veritabanı kopyalanmasını gerektirir. Birçok web ana bilgisayar sağlayıcıları dağıtma işlemini basitleştirmek için araçlar sağlar. Örneğin, DiscountASP.NET ile veritabanınızı FTP `.mdf` dosya (veya bir yedekleme) ve ardından veritabanı Denetim Masası'ndan veritabanı sunucusuna ekleyin. Hangi özelliklerin bağımsız olarak çalışan başka bir seçenek web ana bilgisayar sağlayıcısı tekliflerinizi, bir komut dosyası geliştirme veritabanı s şemasını ve verileri oluşturmak için SQL komutları oluşturur Microsoft s Database Publishing Wizard aracı. Bu komut dosyası oluşturulduktan sonra üretim veritabanında yürütün.

Kitap incelemeleri web uygulama s veritabanı üretimde olduğuna göre biz uygulama dağıtabilirsiniz. Ancak, web uygulama s yapılandırma bilgileri veritabanına bağlantı dizesi belirtir ve geliştirme veritabanı bağlantı dizesini başvuruyor. Site üretime dağıtırken Bu bağlantı dizesi bilgilerini güncelleştirmeniz gerekir. Sonraki öğreticiye bu yapılandırma farklılıkları arar ve üretim için veri odaklı Kitap incelemeleri site yayımlamak için gereken adımlar anlatılmaktadır.

Mutlu programlama!

#### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Microsoft SQL Server veritabanı Yayımlama Sihirbazı'nı 1.1 indirin](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Microsoft SQL Server Management Studio Express Edition'ı indirin](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Önceki](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [İleri](configuring-the-production-web-application-to-use-the-production-database-cs.md)
