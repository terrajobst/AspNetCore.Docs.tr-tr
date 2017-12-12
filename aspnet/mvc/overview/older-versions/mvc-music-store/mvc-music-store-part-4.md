---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: "4. Kısım: Modelleri ve veri erişimi | Microsoft Docs"
author: jongalloway
description: "Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 4 modelleri ve veri erişimi kapsar."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 727336344493b439130b2cf0ec6e7b5925dd5637
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-4-models-and-data-access"></a>4. Kısım: Modelleri ve veri erişimi
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.
> 
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 4 modelleri ve veri erişimi kapsar.


"Şu ana kadar biz yalnızca kukla veri" bizim denetleyicilerinden görünüm şablonlarımız geçirme. Şimdi biz kanca gerçek bir veritabanını yedeklemek hazırsınız. Bu öğreticide şu SQL Server Compact (SQL CE olarak da adlandırılır) Edition kullanmayı bizim veritabanı altyapısı olarak kapsayan. SQL CE herhangi bir yükleme veya yerel geliştirme için gerçekten kolay hale getirir yapılandırma gerektirmeyen bir ücretsiz, katıştırılmış, dosya tabanlı veritabanıdır.

## <a name="database-access-with-entity-framework-code-first"></a>Veritabanı erişimi ile Entity Framework kod-ilk

Sorgulamak ve veritabanını güncelleştirmek için ASP.NET MVC 3 projelerinde dahil Entity Framework (EF) destek kullanacağız. EF bir esnek (ORM) veri nesne yönelimli bir yolla bir veritabanında depolanan verileri sorgulamak veya güncelleştirme geliştiricilerinin API eşleme ilişkisel nesnesidir.

Entity Framework sürüm 4 kod ilk olarak adlandırılan bir geliştirme standardı destekler. Kod ilk basit sınıfları (POCO "düz-eski" CLR nesnelerinden olarak da bilinir) yazarak model nesnesi oluşturmanıza olanak tanır ve veritabanı, sınıflardan kolay bir şekilde bile oluşturabilirsiniz.

### <a name="changes-to-our-model-classes"></a>Bizim modeli sınıfları yapılan değişiklikler

Biz Entity Framework veritabanı oluşturma özelliği Bu öğreticide yararlanarak. Bunu önce yine de birkaç küçük değişiklikler biz daha sonra kullanacağız bazı şeyleri eklemek için bizim model sınıflarına olalım.

#### <a name="adding-the-artist-model-classes"></a>Sanatçı Model sınıfları ekleme

Sanatçı açıklamak için basit model sınıfı ekleyeceğiz şekilde bizim albümleri Sanatçılar ile ilişkilendirilir. Aşağıda gösterilen kodu kullanarak Artist.cs adlı modeller klasörü için yeni bir sınıf ekleyin.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Bizim modeli sınıfları güncelleştiriliyor

Albüm sınıfı, aşağıda gösterildiği gibi güncelleştirin.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

Ardından, aşağıdaki güncelleştirmeleri Tarz sınıfına olun.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Uygulama ekleme\_veri klasörü

Bir uygulama ekleyeceğiz\_Projemizin bizim SQL Server Express veritabanı dosyalarını tutmak için veri dizinine. Uygulama\_zaten veritabanı erişimi için doğru güvenlik erişim izinlerine sahip ASP.NET özel bir dizinde verilerdir. ASP.NET klasör ekleyin ve ardından uygulama proje menüsünden seçin\_veri.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Web.config dosyasında bir bağlantı dizesi oluşturma

Entity Framework bizim veritabanına bağlanmak bildiği böylece Web sitesinin yapılandırma dosyası için birkaç satır ekleyeceğiz. Proje kök dizininde bulunan Web.config dosyasını çift tıklatın.

![](mvc-music-store-part-4/_static/image2.png)

Bu dosyanın sonuna kaydırın ve ekleme bir &lt;connectionStrings&gt; aşağıda gösterildiği gibi doğrudan son satırında bölüm.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Bağlam sınıfı ekleme

Modeller klasörü sağ tıklatın ve MusicStoreEntities.cs adlı yeni bir sınıf ekleyin.

![](mvc-music-store-part-4/_static/image3.png)

Bu sınıf Entity Framework veritabanı bağlamı temsil eder ve bizim oluşturma işlemek, okuma, güncelleştirme ve silme işlemleri için bize. Bu sınıf için kod aşağıda verilmiştir.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

İşte bu kadar - hiçbir diğer yapılandırma, özel arabirimleri, vb. vardır. DbContext temel sınıf genişleterek, bizim MusicStoreEntities bizim veritabanı işlemleri için bize işleyebilen sınıftır. Biz, sayfaya olduğuna göre birkaç daha fazla özellik bazı ek bilgiler bizim veritabanında yararlanmak için bizim model sınıflarına ekleyelim.

### <a name="adding-our-store-catalog-data"></a>Bizim mağazası Kataloğu veri ekleme

Biz bir özellik için yeni oluşturulan bir veritabanı "seed" veri ekleyen Entity Framework içinde yararlanır. Bu bizim mağazası kataloğu ve türler, sanatçılar, Albümler listesiyle önceden doldurur. -, Bu öğreticide daha önce kullanılan bizim site tasarımı dosyaları dahil - MvcMusicStore Assets.zip indirme kodu adlı bir klasörde bulunan bu çekirdek verilerle bir sınıf dosyası vardır.

Kod içinde / modeller klasörü SampleData.cs dosyasını bulun ve aşağıda gösterildiği gibi bizim proje modelleri klasörüne bırakın.

![](mvc-music-store-part-4/_static/image4.png)

Şimdi biz bir Entity Framework o SampleData sınıfı hakkında söylemeniz kod satırı eklemeniz gerekir. Dosyayı açın ve aşağıdaki uygulama en çok satır eklemek için projenin kök Global.asax dosyasına çift tıklayın\_Başlat yöntemi.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

Bu noktada, sizi Projemizin için Entity Framework yapılandırmak için gerekli iş tamamladınız.

## <a name="querying-the-database"></a>Veritabanı sorgulama

Şimdi şimdi "veri kukla" kullanmak yerine, bunun yerine, bilgilerin tümünü sorgulamak için Veritabanımıza çağırır bizim StoreController güncelleştirin. Üzerinde bir alan bildirerek başlayacağız **StoreController** storeDB adlı MusicStoreEntities sınıfının bir örneği tutmak için:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Veritabanını sorgulamak için depolama dizini güncelleştirme

MusicStoreEntities sınıf Entity Framework tarafından korunur ve Veritabanımıza her tablo için bir koleksiyon özelliği sunar. Şimdi, veritabanındaki tüm türler almak için bizim StoreController'ın dizin eylem güncelleştirin. Daha önce bu kodlama sabit dize verilerini tarafından yaptığımız. Şimdi biz yalnızca Entity Framework bağlamına Generes koleksiyonu kullanabilirsiniz:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Biz yine biz yalnızca dinamik bizim veritabanından şimdi veriyor önce - biz döndürülen aynı StoreIndexViewModel döndürme bu yana bizim görünüm şablonu olmasını hiçbir değişiklik gerekir.

Projeyi tekrar çalıştırın ve "/ depolamak" URL'yi ziyaret şimdi tüm türler listesini bizim veritabanında göreceğiz:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Deposu gözatmak ve ayrıntıları canlı verileri kullanmak için güncelleştirme

/ Deposu/Gözat ile? Tarz =*[Tarz bazı]* eylem yöntemi, biz aramakta olduğunuz bir tarzını için ada göre. Yalnızca bir sonuç, biz şimdiye kadar aynı Tarz ad için iki giriş olmamalıdır beri ve biz kullanabilmeniz için bekliyoruz. LINQ Sorgu şöyle uygun türe nesne için Single() uzantısı (Bu henüz yazmayın):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

Tek yöntem bir Lambda ifadesi sağlayacak şekilde, adı biz tanımladığınız değeri ile eşleşen tek bir tarzını nesne istediğimizi belirten bir parametre olarak alır. Yukarıdaki durumda biz DISCO eşleşen bir ad değeri ile tek bir tarzını nesne yüklüyorsunuz.

Biz, bize Tarz nesnesi alınırken de yüklenen istiyoruz ilgili diğer varlıklar belirtmek izin veren bir Entity Framework özelliğinin avantajlarından yararlanmak. Bu özellik, sorgu sonucu şekillendirme olarak adlandırılır ve bize tüm ihtiyacımız bilgileri almak için veritabanına erişmek için ihtiyacımız sayısını azaltmak sağlar. İlgili Albümler istiyoruz belirtmek için Genres.Include("Albums") içerecek şekilde sorgumuzu güncelleştiriyoruz böylece alıyoruz, tarz albümleri önceden getirme istiyoruz. Tek veritabanı isteği bizim tarz ve albüm verileri alacak beri bu daha verimli olur.

Açıklamaları ile göz önünden, İşte bizim güncelleştirilmiş Gözat denetleyici eylemi nasıl göründüğünü:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Biz her bir tarzını kullanılabilir olan albümleri görüntülemek için deposu Gözat görünüm şimdi güncelleştirebilirsiniz. Görünüm şablonu (bulunan /Views/Store/Browse.cshtml) açın ve madde işaretli albümleri listesini aşağıda gösterildiği gibi ekleyin.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Uygulamamız çalıştırma ve tarama/deposu/için Gözat? Tarz = bizim sonuçları şimdi tüm albümleri bizim seçili Tarz görüntüleme veritabanından alınır Jazz gösterir.

![](mvc-music-store-part-4/_static/image2.jpg)

Bizim/Store/Ayrıntılar / [kimlik] URL'yi değiştirin ve sahte verilerimizi Kimliğine parametre değeri ile eşleşen bir albümü yükleyen bir veritabanı sorgusu ile Değiştir'de aynı vermiyoruz.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Uygulamamızı çalıştırmak ve /Store/Details/1 için tarama sonuçları veritabanından şimdi çekilen olduğunu gösterir.

![](mvc-music-store-part-4/_static/image5.png)

Albüm albüm Kimliğine göre görüntülemek için deposu ayrıntıları sayfamızı ayarlayın, şimdi güncelleştirme **Gözat** Ayrıntılar görünümü bağlamak için görünümü. Tam olarak deposu dizinden deposu gözatmak için önceki bölümde sonunda bağlamak için yaptığımız gibi Html.ActionLink, kullanacağız. Gözat görünüm için tam kaynak aşağıda yer almaktadır.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Biz şimdi deposu sayfamızı kullanılabilir albümleri listeleyen bir tarzını sayfasına göz ve bir albümü tıklayarak Biz bu Albüm ayrıntılarını görüntüleyebilirsiniz.

![](mvc-music-store-part-4/_static/image6.png)

>[!div class="step-by-step"]
[Önceki](mvc-music-store-part-3.md)
[sonraki](mvc-music-store-part-5.md)
