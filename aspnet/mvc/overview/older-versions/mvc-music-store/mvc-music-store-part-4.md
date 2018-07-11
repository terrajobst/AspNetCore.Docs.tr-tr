---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: '4. Bölüm: Modeller ve veri erişimi | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 4. Bölüm modeller ve veri erişimi kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 023350e882afe049ce3800921825b1b2bec8e415
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818959"
---
<a name="part-4-models-and-data-access"></a>4. Bölüm: Modeller ve veri erişimi
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.
> 
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 4. Bölüm modeller ve veri erişimi kapsar.


"Şu ana kadar biz yalnızca işlevsiz veri" bizim denetleyicilerinden görünümü şablonlarımızı geçirme. Şimdi gerçek bir veritabanını kanca hazırız. Bu öğreticide size SQL Server Compact (SQL CE olarak da adlandırılır) Edition kullanmayı veritabanı altyapımız kapsar. SQL CE herhangi bir yükleme veya yerel geliştirme için gerçekten kullanışlı kılan yapılandırma gerektirmeyen bir ücretsiz, katıştırılmış, dosya tabanlı veritabanıdır.

## <a name="database-access-with-entity-framework-code-first"></a>Veritabanı erişimi ile Entity Framework Code-First

Sorgulamak ve veritabanını güncellemek için ASP.NET MVC 3 projeleri dahil Entity Framework (EF) destek kullanacağız. EF bir esnek nesne ilişkisel eşleme (ORM) veri geliştiricilerinin nesne yönelimli bir şekilde bir veritabanında depolanan verileri sorgu ve güncelleştirme API ' dir.

Entity Framework sürüm 4 kod öncelikli olarak adlandırılan bir geliştirme paradigma destekler. İlk kod model nesnesi basit sınıfları (POCO "düz eski" CLR nesne olarak da bilinir) yazarak oluşturmanıza olanak sağlar ve veritabanı, sınıflardan hareket halindeyken bile oluşturabilirsiniz.

### <a name="changes-to-our-model-classes"></a>Bizim Model sınıfları değişiklikler

Biz Entity Framework veritabanı oluşturma özelliği Bu öğreticide yararlanarak. Bunu yapmadan önce birkaç küçük değişiklikler daha sonra kullanacağız bazı şeyler eklemek için sunduğumuz model sınıflarına olalım.

#### <a name="adding-the-artist-model-classes"></a>Sanatçının Model sınıfları ekleme

Bir Sanatçıdan açıklamak için basit bir model sınıfı ekleyeceğiz. Bu nedenle bizim albümleri Sanatçılar ile ilişkilendirilir. Aşağıda gösterilen kodu kullanarak Artist.cs adlı modelleri klasörüne yeni bir sınıf ekleyin.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Bizim Model sınıfları güncelleştiriliyor

Albüm sınıfı, aşağıda gösterildiği gibi güncelleştirin.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Ardından, tarz sınıfına aşağıdaki güncelleştirmeleri yapın.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Uygulama ekleme\_veri klasörü

Bir uygulama ekleyeceğiz\_Projemizin bizim SQL Server Express veritabanı dosyaları için veri dizini. Uygulama\_veritabanı erişimi için doğru güvenlik erişim izinleri olan ASP.NET özel bir dizinde verilerdir. ASP.NET klasör ekleyin ve ardından uygulama proje menüsünden seçin\_veri.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Web.config dosyasında bağlantı dizesi oluşturma

Entity Framework bizim veritabanına bağlanmak nasıl bilebilmesi Web sitesinin yapılandırma dosyası için birkaç satır kod ekleyeceğiz. Proje kök dizininde bulunan Web.config dosyasını çift tıklayın.

![](mvc-music-store-part-4/_static/image2.png)

Bu dosyanın alt kısma kaydırın ve ekleme bir &lt;connectionStrings&gt; aşağıda gösterildiği gibi doğrudan son satırında bölümü.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Bağlam sınıfı ekleme

Modeller klasörü sağ tıklatın ve MusicStoreEntities.cs adlı yeni bir sınıf ekleyin.

![](mvc-music-store-part-4/_static/image3.png)

Bu sınıf Entity Framework veritabanı bağlamı temsil eder ve bizim Oluştur işlemek, okuma, güncelleştirme ve silme işlemleri bizim için. Bu sınıfın kodu, aşağıda gösterilmiştir.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

İşte bu kadar - hiçbir diğer yapılandırma, özel arabirimler vb. yoktur. DbContext temel sınıfını genişleterek, bizim MusicStoreEntities sınıfı bizim için veritabanı işlemlerimiz işleyebilir. Şimdi, ölçekledikçe ayarladığımıza göre bizim model sınıflarına veritabanımızda yer bazı ek bilgiler avantajlarından yararlanmak için birkaç daha fazla özellik ekleyin.

### <a name="adding-our-store-catalog-data"></a>Mağaza Kataloğu verilerimizi ekleme

Biz Entity Framework, yeni oluşturulan veritabanına "seed" veri ekleyen bir özelliğin avantajlarından sürer. Bu depolama kataloğumuzu türleri, sanatçıların ve albümleri listesiyle önceden doldurulur. -Bu öğreticide daha önce kullanılan bizim site tasarımı dosyaları dahil - bir MvcMusicStore Assets.zip indirme kodu adlı bir klasörde bulunan bu çekirdek verilerle bir sınıf dosyası vardır.

Kod içinde / modeller klasörü SampleData.cs dosyasını bulun ve aşağıda gösterildiği gibi proje modelleri klasörüne bırakın.

![](mvc-music-store-part-4/_static/image4.png)

Şimdi biz bir Entity Framework, SampleData sınıfı hakkında bilgi için kod satırı eklemeniz gerekir. Global.asax dosyası açın ve aşağıdaki uygulama üstüne satır eklemek için proje kökündeki çift\_yöntemi başlatın.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

Bu noktada, biz Projemizin için Entity Framework yapılandırmak gerekli iş tamamladınız.

## <a name="querying-the-database"></a>Veritabanını sorgulama

Artık "veri kukla" kullanmak yerine bunun yerine, bilgilerin tümünü sorgulamak için sunduğumuz veritabanına çağırır, böylece müşterilerimize StoreController güncelleştirelim. Bir alan üzerinde bildirerek başlayacağız **StoreController** storeDB adlı MusicStoreEntities sınıfının bir örneğini tutmak için:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Veritabanını sorgulamak için Store dizini güncelleştiriliyor

MusicStoreEntities sınıf, Entity Framework tarafından korunur ve veritabanımızdaki her tablo için bir koleksiyon özelliği sunar. Tüm türleri veritabanımızdaki almak için dizin eylem bizim StoreController'ın güncelleştirelim. Daha önce bu kodlama sabit dize verileri tarafından yaptık. Şimdi bunun yerine yalnızca Entity Framework bağlamına Generes koleksiyon kullanabiliriz:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Hiçbir değişiklik biz yine de biz yalnızca canlı verileri bizim veritabanından artık döndürürken önce - biz döndürülen aynı StoreIndexViewModel döndürürken beri bizim şablonu görüntüle gerçekleşmesi gerekir.

Projeyi yeniden çalıştırın ve "/ Store" URL'sini ziyaret edin, artık tüm türleri listesini veritabanımızda yer görüyoruz:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Store göz atın ve ayrıntıları canlı verileri güncelleştirme

İle/Store/göz atma? Tarz =*[Tarz bazı]* eylem yöntemi, biz aramakta olduğunuz bir türe için ada göre. Yalnızca tek bir sonuç şu iki giriş aynı türe adı için hiç olmadığı kadar sahip olmamalıdır ve kullanabiliriz şekilde bekliyoruz. LINQ Sorgu için şunun gibi uygun türe nesne Single() uzantısı (Bu henüz yazmayın):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Tek bir yöntem sağlayacak şekilde tanımlamış olduğunuz değer adı ile eşleşen tek bir türe nesne istediğimizi belirten bir parametre olarak bir Lambda ifadesi alır. Yukarıdaki durumda biz DISCO eşleşen bir ad değer ile tek bir türe nesne yükleniyor.

Biz, tarz nesne alındığında de yüklenen istiyoruz diğer ilgili varlıkları belirtmek olanak sağlayan bir Entity Framework özelliğin avantajlarından yararlanmak. Bu özellik, sorgu sonucu biçimlendirmeye ek olarak adlandırılır ve ihtiyacımız olan tüm bilgileri almak için veritabanına erişmek için ihtiyacımız sayısını azaltmak sağlıyor. Sorgumuzu ilgili Albümler istediğimizi belirten Genres.Include("Albums") içerecek şekilde güncelleştireceğiz. böylece albümleri alıyoruz, tarz için önceden getirme istiyoruz. Tek veritabanı isteği bizim tarz ve albüm verileri alacak olduğundan daha verimli budur.

Açıklamalar ile ortada, işte güncelleştirilmiş Gözat denetleyicisi eylemimiz nasıl göründüğünü:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Biz, her bir tür içinde kullanılabilir olan albümleri görüntülenecek Store Gözat görünümü şimdi güncelleştirebilirsiniz. Görünüm şablonu (öğe içinde bulunan /Views/Store/Browse.cshtml) açın ve bir madde işaretli liste albümleri, aşağıda gösterildiği gibi ekleyin.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Uygulamamızı çalıştırma ve tarama/Store/dizinine göz atma? Tarz = ettiğimiz sonuçların artık tüm Albümler bizim seçili tarzında görüntüleme veritabanından alınır Caz gösterir.

![](mvc-music-store-part-4/_static/image2.jpg)

Aynı bizim/Store/Ayrıntılar / [ID] URL'sini değiştirin ve sahte verilerimizi albüm kimliği parametre değeri ile eşleşen yükleyen bir veritabanı sorguyla değiştirin oluşturacağız.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Uygulamamızı çalıştırmak ve /Store/Details/1 için tarama sonuçları veritabanından artık çekilen olduğunu gösterir.

![](mvc-music-store-part-4/_static/image5.png)

Albüm albüm Kimliğine göre görüntülemek için Store ayrıntıları sayfamıza ayarlayın, güncelleştirelim **Gözat** ayrıntıları görünüme bağlantılandırmak için Görünüm. Tam olarak Store dizinden Store gözatmak için önceki bölümde sonunda bağlamak için yaptığımız gibi Html.ActionLink kullanacağız. Göz atma görünümü için tam kaynak altında görünür.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Artık Store sayfamızı kullanılabilir albümleri listeleyen bir türe sayfasına göz atın sunabiliyoruz ve bir albümü tıklayarak Biz bu Albüm ayrıntılarını görüntüleyebilirsiniz.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-3.md)
> [İleri](mvc-music-store-part-5.md)
