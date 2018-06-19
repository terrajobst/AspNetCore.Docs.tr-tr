---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Bir veritabanı (C#) dağıtma | Microsoft Docs
author: rick-anderson
description: ASP.NET web uygulaması dağıtma gerekli dosyalara ve kaynaklara geliştirme ortamından üretim ortamına alma kapsar. Da için...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 203bf64da887f31e5f0727fc57173d6a573095da
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888032"
---
<a name="deploying-a-database-c"></a>Bir veritabanı (C#) dağıtma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> ASP.NET web uygulaması dağıtma gerekli dosyalara ve kaynaklara geliştirme ortamından üretim ortamına alma kapsar. Veri tabanlı web uygulamaları için bu veritabanı şemasını ve verileri içerir. Bu öğretici geliştirme ortamından üretim veritabanına başarıyla dağıtmak için gerekli olan adımları araştırır serideki ilk ' dir.


## <a name="introduction"></a>Giriş

ASP.NET web uygulaması dağıtma gerekli dosyalara ve kaynaklara geliştirme ortamından üretim ortamına alma kapsar. Son altı öğreticileri süresince basit bir kitap incelemeleri web uygulaması dağıtma sırasında arama. Bu demo site sunucu tarafı kaynaklar - ASP.NET sayfaları, yapılandırma dosyaları, çeşitli oluşan bir `Web.sitemap` dosya ve benzeri - istemci tarafı kaynaklar ile birlikte görüntüler ve CSS dosyaları gibi. Ancak, verilere ne web uygulamaları? Bir veritabanı kullanan bir web uygulamasını dağıtmak için hangi ek adımlar izlenmelidir?

Sonraki birkaç öğreticileri biz bir veri tabanlı web uygulamasını dağıtmak için gerekli olan adımları ele alınacaktır. Bu öğreticinin sonraki öğretici gereken yapılandırma değişikliklerini ararken bir veritabanı s şeması ve içeriği geliştirme ortamından üretim ortamına alma inceleyerek başlar. Aşağıdaki uygulama hizmetlerini (üyelik, roller, profili vb.) kullanan bir veritabanı dağıtma zorluklar incelenecektir.

## <a name="examining-the-updated-book-reviews-web-application"></a>Güncelleştirilmiş rehberi incelemeler Web uygulaması inceleniyor

Veri tabanlı web uygulaması dağıtma göstermek için t ve güncelleştirilmiş basit, statik bir Web sitesi Kitap incelemeleri web uygulamasından veri güdümlü bir. Önce Bu öğretici s indirme uygulamada iki sürümü olduğundan: bir Web uygulaması projesi modeli ve Web sitesi projesini modelini kullanan bir kullanır.

Güncelleştirilmiş Kitap incelemeleri web uygulamasının kullandığı bir [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) s sitede depolanan veritabanı `App_Data` klasörü (`~/App_Data/Reviews.mdf`). SQL Server 2008'in bilgisayarınızda yüklü varsa demo hatasız çalıştırmanız gerekir. Daha eski bir sürümü varsa seçebilir ya da SQL Server ücretsiz SQL Server 2008 Express Edition yükleyin veya veritabanını kendiniz oluşturmak için Bu öğreticide s kullanılabilir komut dosyalarını indirme veritabanını kullanabilirsiniz.

`Reviews.mdf` Veritabanı dört tablonun içerir:

- `Genres` -Teknoloji, kurgu ve iş gibi her bir tarzını için bir kayıt içerir.
- `Books` -gibi sütunlarla her gözden geçirme için bir kayıt içerir `Title`, `GenreId`, `ReviewDate`, ve `Review`, diğerlerinin yanı sıra.
- `Authors` -geçirilmiş defterine katıldığını her yazar hakkında bilgi içerir.
- `BooksAuthors` -hangi yazarların hangi books yazmıştır belirtir bir çok-çok birleştirme tablo.
  

Şekil 1 dört bu tablolar bir ER diyagramını gösterir.


[![Kitap incelemeleri Web uygulaması veritabanı oluşur, dört tablonun s'dir](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Şekil 1**: Kitap incelemeleri Web uygulama veritabanı s'dir oluşur, dört tablonun ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image3.jpg))


Kitap incelemeleri Web sitesi'nın önceki sürümünü her defteri için ayrı bir ASP.NET sayfa vardı. Örneğin, adında bir sayfa vardı `~/Tech/TYASP35.aspx` gözden geçirmeyi bulunan *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*. Bu yeni veri tabanlı Web sitesinin veritabanı ve tek bir ASP.NET sayfası Review.aspx?ID= depolanan incelemeler sürümde*bookId*, belirtilen kitap gözden görüntüler. Benzer şekilde, bir Genre.aspx?ID= yoktur*genreId* belirtilen Tarz geçirilmiş rehberinde listeleyen sayfası.

Şekil 2 ve 3 Göster `Genre.aspx` ve `Review.aspx` eylem sayfalarında. Her bir sayfa için adres çubuğundaki URL'yi unutmayın. Şekil 2 BT s Genre.aspx? kimliği 85d164ba 1123 4 c 47 82a0-c8ec75de7e0e =. 85d164ba-1123-4c47-82a0-c8ec75de7e0e olduğundan `GenreId` teknolojisi Tarz, sayfa s başlık okumaları "Teknoloji incelemeleri" ve madde işaretli liste değeri bu tarz altında kalan bu incelemeler sitesinde numaralandırır.


[![Teknoloji tarzı sayfası](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Şekil 2**: teknolojisi tarzı sayfası ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image6.jpg))


[![Gözden geçirme için öğretmek kendiniz ASP.NET 3.5 24 saat](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Şekil 3**: gözden geçirme için *öğretmek kendiniz ASP.NET 3.5 24 saat içindeki* ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image9.jpg))


Kitap incelemeleri web uygulaması, burada yöneticileri eklemek, düzenlemek ve türler, incelemeleri, silebilir ve bilgi Yazar Yönetim bölümünde de içerir. Şu anda hiçbir ziyaretçi Yönetim bölümünde erişebilir. Sonraki öğreticide biz kullanıcı hesapları için destek ekleyin ve yalnızca yetkili kullanıcılar yönetim sayfalarına izin.

Kitap incelemeleri uygulama amacı veri güdümlü bir uygulama dağıtımı göstermektir unutmayın Lütfen yüklerseniz. Uygulama tasarımı durum en iyi yöntemler gerçekleştirilmez. Örneğin, ayrı hiçbir veri erişim düzeyi (DAL) yoktur; ASP.NET sayfaları SqlDataSource denetimi veya kendi arka plan kodu sınıfları ADO.NET kodu aracılığıyla veritabanı ile doğrudan iletişim kurar. Katmanlı bir mimari kullanarak veri güdümlü uygulamaları oluşturma bir daha derinlemesine bakış için bkz my [ *verilerle çalışma* öğreticileri](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Geliştirme ve üretim veritabanları

Üzerinde veri tabanlı web uygulaması geliştirme başlattığınızda, veritabanına bağlanmak nasıl uygulama ayrıntıları sağlayan bir veritabanı bağlantı dizesi belirtmeniz gerekir. Bu bağlantı dizesi, başka şeylerin, veritabanı sunucusu, veritabanı adı ve güvenlik bilgileri arasında belirtir. Genellikle, geliştirme sırasında uygulama tarafından kullanılan veritabanı kullanılır farklı veritabanıdır, üretimdeki s. Farklı veritabanlarına üretim karşı geliştirme için çok sayıda yararları vardır. Farklı bir geliştirme anlamına gelir veritabanı sahip tan t sahip yanlışlıkla değiştirme veya canlı verileri silme hakkında endişelenmeye gerek. Ayrıca, sahte test verileri put veya ürün uygulamada üzerindeki etkileri hakkında endişelenmeye gerek kalmadan veri modeline önemli değişiklikler yapmak sağlar. Uygulama olduğunda bir olan farklı bir veritabanı geliştirme ve üretim ortamlarında kullanılmasının dezavantajı veritabanını ve ilgili değişiklikleri veritabanı s şemasında dağıtılan veya veri da dağıtılması gerekir.

İlk dağıtımdan veritabanının yalnızca bir örneği yoktur ve bu örnek bir geliştirme ortamıdır. Size gerekir üretime ilk kez uygulama dağıtırken yalnızca gerekli sunucu tarafı ve istemci tarafı dosyaları kopyalamak ancak aynı zamanda veritabanı geliştirme ortamından üretim ortamına kopyalayın. Veritabanı bulunan Kitap incelemeleri web uygulamasıyla - şimdi burada biz sağ göze budur `App_Data` bizim geliştirme ortamı klasöründe ancak henüz üretim ortamına gönderilen henüz.

Uygulama dağıtıldıktan sonra veritabanı iki kopyasını vardır. Uygulama olgunlaşmasına gibi yeni özellikler eklenebilir, veri modeli için bir değişiklik araya (var olan tablolara yeni sütunlar ekleme gibi yeni tablolar ekleme varolan sütunlar için değişiklik ve benzeri). Web uygulaması sonraki dağıtıldığında, son dağıtım üretim veritabanına uygulanması gereken bu yana değişiklikleri veritabanı geliştirme ortamında uygulanır. Bu işlem yönetmek için bazı stratejiler gelecekteki bir öğreticide ele alınmıştır. Bu öğretici geliştirme ortamı tüm veritabanından üretime dağıtma odaklanır.

## <a name="deploying-the-database-to-the-production-environment"></a>Veritabanı üretim ortamına dağıtma

Bu öğreticinin geri kalanında geliştirme ortamı veritabanından üretim ortamına dağıtma bakar. İzliyorsanız hesabınızı web ana bilgisayar sağlayıcınız ile Microsoft SQL Server içerdiğinden emin olun için veritabanı desteklemesi. Ayrıca bazı bilgileri elinizdeki öğesine veritabanı sunucusu adı, veritabanı adını, kullanıcı adı ve veritabanına bağlanmak için kullanılan parola gerekir.

Bu öğreticide daha önce belirtildiği gibi Kitap incelemeleri Web sitesi s veritabanı depolanan bir SQL Server 2008 Express Edition veritabanıdır `App_Data` klasör. Böyle bir veritabanı dağıtma kopyalama gibi basit olacağını neden bekleme `App_Data` geliştirme ortamı klasöründen üretim ortamına. Bununla birlikte, çoğu web ana bilgisayar sağlayıcıları barındırma veritabanlarında desteklemez `App_Data` güvenlik nedenlerle klasör. Bunun yerine, web ana bilgisayarları kendi ortamında SQL Server veritabanı sunucusunda bir hesap sağlayın. Geliştirme ortamınızı veritabanından üretim ortamına dağıtma web ana bilgisayar s veritabanı sunucusunda kayıtlı veritabanınızı alma gerektirir.

Bu nedenle nasıl veritabanınızı geliştirme ortamından üretim ortamına alıyorsunuz? Web ana bilgisayar Teklifleriniz Hizmetleri bağlı olarak bunu yapmanın birkaç yolu vardır. DiscountASP.NET gibi bazı ana veritabanı ya da gerçek bir yedeğini FTP `.mdf` dosyasını Web sitenize ve daha sonra Denetim Masası'ndan yedekleme dosyasını geri yüklemek veya eklemek `.mdf` SQL Server veritabanı sunucusunun dosyasına. Veritabanı dağıtımı gibi araçlarla kopyalama olarak kadar basit hale `App_Data` klasörü üretim ortamına ve Denetim Masası üzerinden ekleme. Bu belki de ilk kez veritabanınızı yayımlamak için kolay ve hızlı bir yoludur.

Veritabanı Yayımlama Sihirbazı'nı kullanan başka bir yaklaşımdır. Veritabanı Yayımlama Sihirbazı'nı kendi tablolarda - tabloları, saklı yordamlar, görünümler, kullanıcı tanımlı işlevler ve benzeri - veritabanı s şemanızı ve isteğe bağlı olarak, verileri oluşturmak için SQL komutlarını oluşturacak olan bir Windows Masaüstü uygulamasıdır. Web sunucunuza konak sağlayıcısı s veritabanı SQL Server Management Studio aracılığıyla bağlanmak ve üretim veritabanını çoğaltmak için bu betiği yürütün. Daha iyi, web ana bilgisayar sağlayıcınız Microsoft s destekliyorsa [veritabanı yayımlama Hizmetleri](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) veritabanı yayımlama, sizin adınıza veritabanı sunucusunda otomatik olarak yürütülen Sihirbazı tarafından oluşturulan komut dosyası olabilir. Veritabanı Yayımlama Sihirbazı'nı veritabanı s şeması ve verisi oluşturan bir komut dosyası oluşturduğundan, web ana bilgisayar sağlayıcınıza karşıya yüklenen bir ekleme gibi özellikler sunar bağımsız olarak çalışır `.mdf` dosya.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Veritabanı Yayımlama Sihirbazı'nı kullanarak verileri ve veritabanı şeması oluşturmak için SQL komutlarını oluşturma

S Kitap incelemeleri veritabanını üretime dağıtmak için veritabanı Yayımlama Sihirbazı'nı kullanarak aracılığıyla yol sağlar. Visual Studio 2008 kullanıyorsanız veya ötesinde veritabanı Yayımlama Sihirbazı'nı zaten yüklü. Visual Studio 2005'i kullanarak sonra ilk gerekir [yükleyip](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) Sihirbazı.

Visual Studio'yu açın ve gidin `Reviews.mdf` veritabanı. Visual Web Developer kullanıyorsanız, veritabanı Gezgini'ne gidin; Visual Studio kullanıyorsanız, sunucu Gezgini'ni kullanın. Şekil 4 gösterir `Reviews.mdf` Visual Web Developer veritabanı Gezgininde veritabanı. Şekil 4'te gösterildiği gibi `Reviews.mdf` veritabanı dört tablonun, üç saklı yordamları ve kullanıcı tanımlı bir işlev oluşur.


[![Veritabanı Explorer ya da Sunucu Gezgini veritabanını bulun](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Şekil 4**: veritabanı Explorer ya da Sunucu Gezgini veritabanını bulun ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image12.jpg))


Veritabanı adını sağ tıklatın ve bağlam menüsünden "Sağlayıcı Yayımla" seçeneğini belirleyin. Bu veritabanı Yayımlama Sihirbazı'nı başlatır (bkz. Şekil 5). Giriş ekranı geçmiş öncelikli İleri'yi tıklatın.


[![Yayımlama Sihirbazı Karşılama ekranı veritabanı](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Şekil 5**: veritabanı Yayımlama Sihirbazı Karşılama ekranında ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image15.jpg))


Sihirbazın ikinci ekranında veritabanı Yayımlama Sihirbazı erişilebilir veritabanlarını listeler ve seçili veritabanındaki tüm nesneleri komut dosyası mi, yoksa komut dosyası için hangi nesnelerin seçmek için seçmenize olanak sağlar. Uygun veritabanını seçin ve "Komut dosyası tüm seçili veritabanındaki nesneler" seçeneğini işaretli bırakın.

> [!NOTE]
> Hata alırsanız "veritabanında nesne yok *databaseName* Bu sihirbaz tarafından kodlanabilir türlerinin" sonraki Şekil 6'da gösterilen ekranında tıklandığında, veritabanı dosyanızın yolu aşırı uzun olmadığından emin olun. Veritabanı dosya yolu çok uzun olduğunda bu hata oluşabilir bulundu.


[![Yayımlama Sihirbazı Karşılama ekranı veritabanı](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Şekil 6**: veritabanı Yayımlama Sihirbazı Karşılama ekranında ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image18.jpg))


Sonraki ekranından bir komut dosyası oluştur veya web ana bilgisayarınız destekliyorsa, veritabanını doğrudan web ana bilgisayar sağlayıcısı s veritabanı sunucunuza yayımlayın. Şekil 7'de görüldüğü gibi bir dosyaya yazılır betik yaşıyorum `C:\REVIEWS.MDF.sql`.


[![Veritabanını bir dosyaya komut dosyası veya doğrudan bilgisayarınızı Web ana makine sağlayıcısı için yayımlama](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Şekil 7**: veritabanını bir dosyaya komut dosyası veya doğrudan bilgisayarınızı Web ana makine sağlayıcısı için yayımlayın ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image21.jpg))


Sonraki ekranda, komut dosyası seçenekleri çeşitli ister. Komut dosyası bu varolan nesneleri kaldırmak için açılan deyimleri içerip içermeyeceğini belirtebilirsiniz. Bu True, bir veritabanına ilk kez dağıtırken ince varsayılan olarak ayarlanır. Hedef veritabanı SQL Server 2000, SQL Server 2005 veya SQL Server 2008 olduğunu da belirtebilirsiniz. Veri ve şema komut dosyası atlanmayacağını belirtmek son olarak, yalnızca verileri veya yalnızca şema. Şema nesneleri, tablolar, saklı yordamlar, görünümler ve benzeri koleksiyonudur. Verileri tablolarda bulunan bilgilerdir.

Şekil 8 gösterildiği gibi t ve var olan veritabanı nesnelerini bırakmak için yapılandırılmış Sihirbazı'nı bir SQL Server 2008 veritabanı için komut dosyası oluşturmanız ve hem şema hem de veri yayımlamak için aldı.


[![Yayımlama belirtin seçenekleri](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Şekil 8**: yayımlama seçeneklerini belirtin ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image24.jpg))


Son iki ekranda, yaklaşık alınması ve komut dosyası durumunu görüntülemek için eylemleri özetler. Sihirbazı çalıştırmadan net üretim veritabanını oluşturmak ve geliştirme gibi üzerinde aynı verilerle doldurmak için gereken SQL komutlarını içeren bir komut dosyası sahibiz sonucudur.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Üretim ortamı veritabanında SQL komutlarını çalıştırma

Biz sahip olduğunuza göre kalan tüm veritabanı ve onun verileri oluşturmak için SQL komutlarını içeren üretim veritabanında betik yürütmek için betiğidir. Bazı web ana bilgisayar sağlayıcıları veritabanınızda yürütmek için SQL komutlarını girebileceğiniz, Denetim Masası'ndaki textbox sunar. Çok büyük komut dosyanız varsa bu seçeneği çalışmayabilir ( `REVIEWS.MDF.sql` komut dosyasıdır üzerinde 425 KB boyutunda örneği için).

SQL Server Management Studio (SSMS) kullanarak doğrudan üretim veritabanı sunucusuna bağlanmak daha iyi bir yaklaşımdır. Bir Express sürümü, SQL, bilgisayarınızda yüklü sunucu olmayan varsa sonra büyük olasılıkla zaten yüklü SSMS var. Aksi takdirde, şunları yapabilirsiniz [yükleyip](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) ücretsiz bir SQL Server Management Studio Express Edition kopyasını.

SSMS başlatın ve web ana bilgisayar sağlayıcınız tarafından sağlanan bilgileri kullanarak, web ana bilgisayar s veritabanı sunucusuna bağlanın.


[![Web ana bilgisayar sağlayıcısı s veritabanı sunucunuza bağlanma](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Şekil 9**: bilgisayarınızı Web ana makine sağlayıcısı s veritabanı sunucusu bağlantısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image27.jpg))


Veritabanları sekmesini genişletin ve veritabanınızı bulun. Araç çubuğu sol üst köşesindeki yeni bir sorgu düğmesini tıklatın, veritabanı Yayımlama Sihirbazı tarafından oluşturulan komut dosyasından SQL komutlarını yapıştırın ve üretim veritabanı sunucusunda bu komutları çalıştırmak için Yürüt düğmesini tıklatın. Komut dosyanız özellikle büyükse komutları yürütmek için birkaç dakika sürebilir.


[![Web ana bilgisayar sağlayıcısı s veritabanı sunucunuza bağlanma](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Şekil 10**: bilgisayarınızı Web ana makine sağlayıcısı s veritabanı sunucusu bağlantısı ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image30.jpg))


Bu s tüm var olan üzere! Bu noktada geliştirme veritabanını üretime yinelenmiş. SSMS veritabanında yenilerseniz yeni veritabanı nesneleri görmeniz gerekir. Şekil 11 üretim veritabanı s tabloları, saklı yordamları ve geliştirme veritabanını üzerindekiler yansıtma kullanıcı tanımlı işlevler gösterir. Ve biz veri yayımlama için veritabanı Yayımlama Sihirbazı belirtildiği için üretim veritabanı s tabloları sihirbaz yürütüldüğü zaman geliştirme veritabanı s tabloları aynı verilere sahip. Şekil 12 gösterir verilerde `Books` üretim veritabanı tablosunda.


[![Üretim veritabanında veritabanı nesnelerini ikinci kez](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Şekil 11**: veritabanı nesneleri sahip olan yinelenen üretim veritabanında ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image33.jpg))


[![Üretim veritabanı geliştirme veritabanında aynı verilere içerir](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Şekil 12**: üretim veritabanı geliştirme veritabanında gibi aynı veri içeriyor ([tam boyutlu görüntüyü görüntülemek için tıklatın](deploying-a-database-cs/_static/image36.jpg))


Bu noktada biz yalnızca geliştirme veritabanını üretime dağıtmış. Biz henüz web uygulaması dağıtma sırasında arama veya üretim uygulamayı üretim veritabanını kullanmak için yapılandırma değişiklikleri gereken inceledi. Sonraki öğreticide Biz bu sorunları ele alacağız!

## <a name="summary"></a>Özet

Veri tabanlı web uygulaması dağıtma üretim ortamına geliştirme sırasında kullanılan veritabanı kopyalama gerektirir. Birçok web ana bilgisayar sağlayıcıları bir veritabanı dağıtma işlemini basitleştirmek için araçlar sunar. Örneğin, DiscountASP.NET ile veritabanınızı FTP `.mdf` dosya (veya bir yedekleme) ve ardından veritabanını Denetim Masası'ndan veritabanı sunucusuna ekleyin. Hangi özelliklerin bağımsız olarak çalışan başka bir web ana bilgisayar sağlayıcısı Teklifleriniz seçenektir geliştirme veritabanı s şeması ve verisi oluşturmak için SQL komutlarını bir komut dosyasını oluşturur Microsoft s veritabanı Yayımlama Sihirbazı'nı aracı. Bu komut dosyası oluşturulduktan sonra üretim veritabanında yürütebilir.

Kitap incelemeleri web uygulama s üretimde veritabanıdır şimdi biz uygulamasını dağıtabilirsiniz. Ancak, web uygulaması s yapılandırma bilgilerini veritabanına bağlantı dizesi belirtir ve geliştirme veritabanı bağlantı dizesini başvuruyor. Site için Üretim dağıtımı sırasında bu bağlantı dizesi bilgilerini güncelleştirmek gerekir. Sonraki öğretici bu yapılandırma farklılıkları arar ve veri temelli Kitap incelemeleri site üretime yayımlamak için gereken adımlar anlatılmaktadır.

Mutluluk programlama!

#### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Yayımlama Sihirbazı'nı 1.1 Microsoft SQL Server veritabanı indirin](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Microsoft SQL Server Management Studio Express Edition'ı karşıdan yükle](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Önceki](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [sonraki](configuring-the-production-web-application-to-use-the-production-database-cs.md)
