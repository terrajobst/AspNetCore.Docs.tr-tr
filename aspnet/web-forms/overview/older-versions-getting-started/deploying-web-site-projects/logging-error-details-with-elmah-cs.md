---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: (C#) ELMAH ile hata ayrıntılarını günlüğe | Microsoft Docs
author: rick-anderson
description: Hata günlüğü modüller ve işleyiciler (ELMAH), bir üretim ortamında çalışma zamanı hatalarını günlüğe kaydetme için başka bir yaklaşım sunar. ELMAH ücretsiz, açık kaynaklı bir hatadır...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 4337500e0da3c6a75737438f3eeed731350847dd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755139"
---
<a name="logging-error-details-with-elmah-c"></a>(C#) ELMAH ile hata ayrıntılarını günlüğe kaydetme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Hata günlüğü modüller ve işleyiciler (ELMAH), bir üretim ortamında çalışma zamanı hatalarını günlüğe kaydetme için başka bir yaklaşım sunar. ELMAH hata filtreleme ve bir RSS akışı olarak bir web sayfasından hata günlüğünü görüntüleyin veya virgülle ayrılmış bir dosya indirmek için özelliği gibi özellikler içeren bir ücretsiz, açık kaynaklı hata günlüğü kitaplıktır. Bu öğreticide, yükleme ve yapılandırma ELMAH aracılığıyla açıklanmaktadır.


## <a name="introduction"></a>Giriş

[Önceki öğretici](logging-error-details-with-asp-net-health-monitoring-cs.md) ASP incelenir. NET'ın sistem durumu bir çeşit Web olaylarını kaydetmek için kutusu kitaplık yetersizliği sağlayan sistem izleme. Geliştiricilerin çoğu, sistem durumu izleme oturumu ve işlenmeyen özel durumların ayrıntılarını e-posta kullanın. Ancak, bazı sorunlu noktaları bu sistemi vardır. Öncelikle olmaması günlüğe kaydedilen olayları hakkında bilgi görüntülemek için kullanıcı arabirimi her türlü ' dir. Son 10 hataları özetini görmek veya geçen hafta gerçekleşen bir hata ayrıntılarını görüntülemek istiyorsanız, veritabanı aracılığıyla ya da depolama gerekir, tarama, aracılığıyla e-posta gelen kutusuna veya derleme bilgileri görüntüleyen bir web sayfası `aspnet_WebEvent_Events` tablo.

Başka bir sorunun noktası sistem durumu izlemenin karmaşıklığı ortalar. Çünkü farklı olaylar deseninizi oluşturmayı kaydetmek için sistem durumu izleme kullanılabilir ve çeşitli seçenekler nasıl ve ne zaman olayları kaydedilir söyleyen için olduğundan, doğru sistem durumu izleme sistemi yapılandırma onerous bir görev olabilir. Son olarak, uyumluluk sorunları vardır. Sistem durumu izleme önce .NET Framework 2.0 sürümünde eklendiğinden, ASP.NET sürümü kullanılarak oluşturulan eski web uygulamaları için kullanılabilir değilse 1.x. Ayrıca, `SqlWebEventProvider` günlükleri hata ayrıntılarını bir veritabanı için önceki öğreticide kullanılan, sınıf, yalnızca Microsoft SQL Server veritabanları ile çalışır. Bir XML dosyasına veya Oracle database'e gibi bir alternatif veri deposuna hataları günlüğe gerektiğinde özel günlük sağlayıcı sınıfı oluşturmanız gerekir.

Sistem durumu izleme sistemi için hata günlüğü modüller ve işleyiciler (ELMAH), bir ücretsiz, açık kaynaklı hata günlüğünü sistem tarafından oluşturulan alternatiftir [Atif Aziz](http://www.raboof.com/). İki sistem arasındaki en dikkat çekici fark hataları ve bir web sayfasından ve belirli bir hata ayrıntılarını bir RSS akışı olarak bir listesini görüntülemek için ELAMH'ın olanağıdır. ELMAH yalnızca hataları kaydeder olduğundan sistem durumu izleme daha yapılandırmak daha kolay olur. Ayrıca, ELMAH ASP.NET 1.x, ASP.NET 2.0 ve ASP.NET 3.5 uygulamaları ve çeşitli günlük kaynak sağlayıcılar ile birlikte gönderilmektedir için destek içerir.

Bu öğreticide ELMAH ASP.NET uygulamasını ekleme adımları açıklanmaktadır. Haydi başlayalım!

> [!NOTE]
> Sistem durumu izleme sistemi ve ELMAH hem kendi avantajları ve dezavantajları vardır. Her iki sistem deneyin ve ne bir en iyi karşılayacak karar gereksinimlerinizi öneriyoruz.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>ELMAH ASP.NET Web uygulamasına ekleme

ELMAH yeni veya mevcut bir ASP.NET uygulamasına tümleştirmek beş dakikadan kısa sürer bir kolayca ve basit işlemdir. Buna koysalar dört basit adımları içerir:

1. ELMAH indir ve Ekle `Elmah.dll` web uygulamanız için derleme
2. ELMAH'ın HTTP modülleri ve işleyicisinde kaydetme `Web.config`,
3. ELMAH'ın yapılandırma seçenekleri belirtin ve
4. Hata günlüğü kaynak altyapı gerekirse oluşturun.

Şimdi her bu dört adımı, bir kerede bir yol.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>1. adım: ELMAH proje dosyalarını indirme ve ekleme`Elmah.dll`Web uygulamanıza

ELMAH 1.0 BETA 3 (derleme 10617), yazma zamanında en son sürümü karşıdan bu öğreticiyle dahildir. Alternatif olarak, ziyaret edebilirsiniz [ELMAH Web sitesi](https://code.google.com/p/elmah/) en son sürümü Al veya kaynak kodunu indirebilir. ELMAH indirme masaüstünüzdeki bir klasöre ayıklayın ve ELMAH derleme dosyası bulunamıyor (`Elmah.dll`).

> [!NOTE]
> `Elmah.dll` Dosya indirme işlemine ait bulunan `Bin` alt sürüm ve hata ayıklama yapıları ve farklı .NET Framework sürümleri için olan klasör. Yayın derlemesi için uygun framework sürümü kullanın. Örneğin, bir ASP.NET 3.5 web uygulaması derliyorsanız, kopyalama `Elmah.dll` dosya `Bin\net-3.5\Release` klasör.


Ardından, Visual Studio'yu açın ve derleme ve Çözüm Gezgini bağlam menüsünden Başvuru Ekle'i seçme içinde Web sitesi adı sağ tıklayarak projenize ekleyin. Bu başvuru Ekle iletişim kutusunu açar. Göz atma sekmesine gidin ve seçin `Elmah.dll` dosya. Bu eylem ekler `Elmah.dll` web uygulamasının dosyasına `Bin` klasör.

> [!NOTE]
> Web uygulama projesi (WAP) türü gösterilmemektedir `Bin` Çözüm Gezgini'nde klasörü. Bunun yerine, bu öğeleri başvurular klasörünün altındaki listeler.


`Elmah.dll` Derleme ELMAH sistem tarafından kullanılan sınıflar içerir. Bu sınıfların tek üç kategoriye ayrılır:

- **HTTP modüllerinden** -bir HTTP modülü için olay işleyicileri tanımlayan bir sınıftır `HttpApplication` olayları gibi `Error` olay. ELMAH içeren birden çok HTTP modülü, üç en başlığıyla ilgili olanları ediliyor: 

    - `ErrorLogModule` -bir günlük kaynağına işlenmeyen özel durumları günlüğe kaydeder.
    - `ErrorMailModule` -İşlenmeyen bir özel durum ayrıntılarını bir e-posta iletisi gönderir.
    - `ErrorFilterModule` -geliştirici tarafından belirtilen filtreler hangi özel durumları günlüğe belirlemek için ve hangi geçerlidir olanları yok sayılır.
- **HTTP işleyicileri** -bir HTTP işleyicisini, belirli bir talep türü için biçimlendirme oluşturmaktan sorumlu bir sınıftır. ELMAH hata ayrıntılarını bir web sayfası olarak, bir RSS akışı olarak veya bir virgülle ayrılmış değerler dosyası (CSV) olarak işleyen HTTP işleyicilerini içerir.
- **Hata günlüğü kaynakları** - ELMAH oturum bellek, Oracle veritabanı, bir Microsoft Access veritabanı için bir Microsoft SQL Server veritabanı hataları için yepyeni bir SQLite veritabanından veya Vista DB veritabanı için bir XML dosyası. Durum sistemini izleme gibi ELMAH'ın mimarisi oluşturma ve gerekirse kendi özel günlük kaynak sağlayıcıları sorunsuzca tümleştirin sağlayıcı modeli kullanılarak oluşturulmuştur.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>2. adım: ELMAH'ın HTTP modülü ve işleyicisi kaydediliyor

Sırada `Elmah.dll` dosyasını içeren HTTP modüllerini ve işleyici gerekli işlenmeyen özel durumları otomatik olarak oturum açın ve bir web sayfasından hata ayrıntılarını görüntülemek için bu web uygulamasının yapılandırmasında açıkça kaydedilmelidir. `ErrorLogModule` HTTP modülü, bir kez kayıtlı aboneliği `HttpApplication`'s `Error` olay. Bu olayı yükseltildiğinde her `ErrorLogModule` belirtilen günlük kaynağına özel durumun ayrıntılarını kaydeder. Sonraki bölümde, günlüğü kaynak sağlayıcısı tanımlama görüyoruz "Yapılandırma ELMAH." `ErrorLogPageFactory` HTTP işleyici üreteci, hata günlüğü bir web sayfasından görüntülerken biçimlendirme oluşturmak için sorumludur.

HTTP modüller ve işleyiciler kaydetmek için belirli bir söz dizimi sitesi destekleyen web sunucusu üzerinde bağlıdır. ASP.NET Geliştirme Sunucusu ve Microsoft'un IIS için sürüm 6.0 ve önceki sürümleri, HTTP modüller ve işleyiciler kayıtlı `<httpModules>` ve `<httpHandlers>` içinde görünen bölümleri `<system.web>` öğesi. IIS 7.0 kullandığınız sonra kaydedilmesi ihtiyaç duydukları `<system.webServer>` öğenin `<modules>` ve `<handlers>` bölümler. Neyse ki, HTTP modüller ve işleyiciler, tanımlayabilirsiniz *hem* kullanılan web sunucusu bağımsız olarak yerleştirir. Geliştirme ve üretim ortamlarında kullanılan web sunucusu bağımsız olarak kullanılmak üzere aynı yapılandırmayı verdiğinden, bu seçenek en taşınabilir bir bileşendir.

Başlangıç kaydederek `ErrorLogModule` HTTP modülü ve `ErrorLogPageFactory` HTTP işleyicisine `<httpModules>` ve `<httpHandlers>` konusundaki `<system.web>`. Yapılandırmanızı zaten bu iki öğenin sonra yalnızca tanımlıyorsa dahil `<add>` ELMAH'ın HTTP modülü ve işleyici için öğesi.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Ardından, YAZMAÇ ELMAH'ın HTTP modülü ve işleyicisinde `<system.webServer>` öğesi. Bu öğe zaten yapılandırmanızda mevcut değilse, önceden olduğu gibi sonra ekleyin.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

Varsayılan olarak, IIS 7 HTTP modüller ve işleyiciler, kayıtlı olmadığını şeklinde hata verir `<system.web>` bölümü. `validateIntegratedModeConfiguration` Özniteliğini `<validation>` gibi hata iletilerinin gösterilmemesini IIS 7 öğe bildirir.

Unutmayın kaydetmek için söz dizimi `ErrorLogPageFactory` HTTP işleyicisini içeren bir `path` ayarlanır özniteliği, `elmah.axd`. Bu öznitelik web uygulaması olmadığını bildirir adlandırılmış bir sayfa için bir istek ulaştığında `elmah.axd` isteğin işlenmesi gereken sonra `ErrorLogPageFactory` HTTP işleyicisi. Görüyoruz `ErrorLogPageFactory` Bu öğreticinin sonraki adımlarında uygulamada HTTP işleyici.

### <a name="step-3-configuring-elmah"></a>3. adım: ELMAH yapılandırma

ELMAH kendi yapılandırma seçenekleri Web sitesinin içinde arar `Web.config` adlı bir özel yapılandırma bölümü dosyasında `<elmah>`. Özel bir bölümde kullanmak için `Web.config` , öncelikle tanımlanmalıdır `<configSections>` öğesi. Açık `Web.config` dosya ve eklemek için aşağıdaki biçimlendirme `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> ELMAH ASP.NET 1.x uygulaması yapılandırıyorsanız kaldırın, ardından `requirePermission="false"` özniteliğini `<section>` yukarıdaki öğeler.


Yukarıdaki sözdizimi özel kaydeder `<elmah>` bölüm ve alt bölümleri: `<security>`, `<errorLog>`, `<errorMail>`, ve `<errorFilter>`.

Ardından, ekleme `<elmah>` bölümünü `Web.config`. Bu bölümde, aynı düzeyde görünmelidir `<system.web>` öğesi. İçinde `<elmah>` bölümü ekleyin `<security>` ve `<errorLog>` bölümlerde şu şekilde:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

`<security>` Bölümün `allowRemoteAccess` öznitelik, uzaktan erişim izin verilip verilmediğini gösterir. Bu değer 0 olarak ayarlanırsa, ardından hata günlüğü web sayfasında yalnızca yerel olarak görüntülenebilir. Bu öznitelik için 1 olarak ayarlanırsa hata günlüğü web sayfasında uzak ve yerel ziyaretçiler için etkin. Şimdilik, uzak ziyaretçiler için hata günlüğü web sayfasında şimdi devre dışı. Bunun yapılması, güvenlik sorunları tartışmak için fırsatına sahip olduğumuz sonra size daha sonra uzaktan erişime izin.

`<errorLog>` Bölüm tanımlar burada hata ayrıntılarını kaydedilir; benzer hangi belirtir hata günlüğü kaynağı `<providers>` durum sistemini izleme bölümü. Yukarıdaki sözdizimi belirtir `SqlErrorLog` sınıfı tarafından belirtilen bir Microsoft SQL Server veritabanı hataları günlüğe kaydeden hata günlüğü kaynağı olarak `connectionStringName` öznitelik değeri.

> [!NOTE]
> ELMAH, bir XML dosyası, bir Microsoft Access veritabanı, Oracle veritabanı ve diğer veri depoları için hataları günlüğe kaydetmek için kullanılan ek hata günlüğü sağlayıcıları ile birlikte gelir. Örneğe bakın `Web.config` ELMAH indirme bu alternatif hata günlüğü sağlayıcılarını kullanma hakkında daha fazla bilgi için bulunan dosya.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>4. adım: hata günlüğü Kaynak Altyapısı oluşturma

ELMAH'ın `SqlErrorLog` sağlayıcısı belirtilen bir Microsoft SQL Server veritabanı için hata ayrıntılarını günlüğe kaydeder. `SqlErrorLog` Sağlayıcısı bekliyor adlı bir tablonuz varsa bu veritabanını `ELMAH_Error` ve üç saklı yordamlar: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, ve `ELMAH_LogError`. ELMAH indirme adlı bir dosya içerir `SQLServer.sql` içinde `db` bu tabloyu ve bunları oluşturmak için T-SQL içeren klasörü saklı yordamlar. Bu deyimler, veritabanında çalıştırmak ihtiyacınız olacak `SqlErrorLog` sağlayıcısı.

**Şekil 1** ve **2** gerekli veritabanı nesnelerini sonra Visual Studio'da veritabanı Gezgini Göster `SqlErrorLog` sağlayıcısı eklenmiştir.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Şekil 1**: `SqlErrorLog` sağlayıcısı hataları günlüğe kaydeder `ELMAH_Error` tablo

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Şekil 2**: `SqlErrorLog` sağlayıcısı üç saklı yordamlar kullanır

## <a name="elmah-in-action"></a>ELMAH sürüyor

ELMAH kayıtlı web uygulaması için bu noktada ekledik `ErrorLogModule` HTTP modülü ve `ErrorLogPageFactory` HTTP işleyicisini ELMAH'ın yapılandırma seçeneklerinde belirtilen `Web.config`ve gerekli veritabanı nesnelerini eklenen `SqlErrorLog` hata günlüğü sağlayıcısı. Biz ELMAH çalıştığını görmek hazırsınız! Kitap incelemeleri Web sitesini ziyaret edin ve bir çalışma zamanı hatası, gibi oluşturan bir sayfasını ziyaret edin `Genre.aspx?ID=foo`, ya da var olmayan sayfa gibi `NoSuchPage.aspx`. Bu sayfaları ziyaret gördüğünüz bağımlı `<customErrors>` yapılandırması ve yerel olarak veya uzaktan ziyaret ettiğiniz. (Kiracıurl [ *özel hata sayfası görüntüleme* öğretici](displaying-a-custom-error-page-cs.md) bu konuda tazelemek.)

ELMAH işlenmeyen bir özel durum oluştuğunda içeriği kullanıcıya gösterilen etkilemez; yalnızca ayrıntılarını günlüğe kaydeder. Web sayfasından aldığınız bu hata günlüğü erişilebilir `elmah.axd` , sitenizin kök gibi `http://localhost/BookReviews/elmah.axd`. (Bu dosya fiziksel olarak projenizde, ancak bir istek için geldiğinde yok `elmah.axd` çalışma zamanı için gönderir `ErrorLogPageFactory` tarayıcıya gönderilen biçimlendirmeyi oluşturur HTTP işleyicisini.)

> [!NOTE]
> Ayrıca `elmah.axd` test hatası oluşturulacak ELMAH istemek için sayfa. Ziyaret `elmah.axd/test` (gibi `http://localhost/BookReviews/elmah.axd/test`) ELMAH türünde bir özel durum oluşturmasına neden `Elmah.TestException`, hata iletisi vardır: "Bu güvenle yoksayılabilir bir test özel durumdur."


**Şekil 3** ziyaret hata günlüğünü gösterir `elmah.axd` geliştirme ortamından.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Şekil 3**: `Elmah.axd` bir Web sayfasından hata günlüğünü görüntüler  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-elmah-cs/_static/image7.png))

Hata oturum **Şekil 3** altı hata girişleri içerir. Her giriş (404 veya bu hatalar, 500) HTTP durum kodu, türü, açıklama, hata oluştuğunda, oturum açan kullanıcının adını ve tarih ve saat içerir. Ayrıntıları bağlantısını görüntüler hata ayrıntılarını sarı ekran, ölüm içinde gösterilen aynı hata iletisini içeren bir sayfa (bkz **Şekil 4**) hatanın zaman sunucu değişkenlerinin değerlerini birlikte (bkz  **Şekil 5**). Ayrıca HTTP POST üst bilgisinde değerleri gibi ek bilgileri içeren ham XML hata ayrıntılarını kaydedilen görüntüleyebilirsiniz.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Şekil 4**: YSOD hata ayrıntılarını görüntüleme  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Şekil 5**: hata, sunucu değişkenleri toplama değerlerini keşfedin  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-elmah-cs/_static/image13.png))

ELMAH üretim Web sitesine dağıtma kapsar:

- Kopyalama `Elmah.dll` dosyasını `Bin` üretim klasörü
- ELMAH özgü yapılandırma ayarlarını kopyalama `Web.config` , üretimde kullanılan dosya ve
- Hata günlüğü kaynak altyapı üretim veritabanına ekleniyor.

Biz, önceki öğreticilerdeki üretimde geliştirme dosyaları kopyalamak için teknikleri incelediniz. Belki de en kolay yolu, üretim veritabanında bir hata günlüğü kaynak altyapı elde etmek için SQL Server Management Studio üretim veritabanına bağlanmak ve ardından yürütmek için kullanmaktır `SqlServer.sql` gereken tablo oluşturur ve depolanan betik dosyası yordamları.

### <a name="viewing-the-error-details-page-on-production"></a>Üretimde hata Ayrıntılar sayfasını görüntüleme

Sitenizi Üretim dağıtımı sonra üretim Web sitesini ziyaret edin ve işlenmeyen bir özel durum oluşturur. Geliştirme ortamı, ELMAH işlenmeyen bir özel durum oluştuğunda görüntülenen hata sayfasında herhangi bir etkisi yoktur; Bunun yerine, yalnızca hata günlükleri. Hata günlüğü sayfayı ziyaret etmek denerseniz (`elmah.axd`) üretim ortamından, gösterilen Yasak sayfasıyla greeted **Şekil 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Şekil 6**: varsayılan olarak, uzak ziyaretçiler hata günlüğü Web sayfasını görüntüleyemez  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-elmah-cs/_static/image16.png))

ELMAH yapılandırma sözcüğünün `<security>` ayarladığımız bölüm `allowRemoteAccess` 0 olarak uzak kullanıcılar için hata günlüğünü görüntülemesini önleyen özniteliği. Hata ayrıntılarını güvenlik açıklarını ya da diğer hassas bilgileri açığa çıkarabileceği için hata günlüğünü görüntüleme anonim ziyaretçilerin önlemek önemlidir. Bu öznitelik değerini 1 yapın ve hata günlüğünü uzaktan erişimi etkinleştirmek karar sonra kilitleme önemlidir `elmah.axd` ziyaretçiler, yalnızca yetkili şekilde yolu erişebilirsiniz. Bu ekleyerek gerçekleştirilebilir bir `<location>` öğesine `Web.config` dosya.

Aşağıdaki yapılandırma, hata günlüğü web sayfasına erişmek için yönetici rolünde yalnızca kullanıcılar izin verir:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Yönetici rolü ve sistemdeki - Scott, Jisun ve Alice - üç kullanıcı eklenmiştir [ *bir Web sitesi, kullandığı uygulama hizmetleri yapılandırma* öğretici](configuring-a-website-that-uses-application-services-cs.md). Kullanıcılar Scott ve Jisun yönetici rolünün üyesidir. Kimlik doğrulama ve yetkilendirme ile ilgili daha fazla bilgi için benim [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Hata günlüğünü üretim ortamında uzak kullanıcılar tarafından artık görüntülenebilen; kiracıurl **Şekil 3**, **4**, ve **5** için hata günlüğü web sayfasının ekran görüntüsü. Ancak, bunlar anonim veya yönetici olmayan bir kullanıcı hata günlüğü sayfasında görüntülemeye çalışırsa oturum açma sayfasına otomatik olarak yönlendirilir (`Login.aspx`), olarak **Şekil 7** gösterir.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Şekil 7**: yetkisiz kullanıcıların olduğundan otomatik olarak yeniden yönlendirilen oturum açma sayfası  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Program aracılığıyla hatalarını günlüğe kaydetme

ELMAH'ın `ErrorLogModule` HTTP modülü belirtilen günlük kaynağına işlenmeyen özel durumları otomatik olarak günlüğe kaydeder. Alternatif olarak, işlenmeyen bir özel durum kullanarak yükseltmek zorunda kalmadan hatayla oturum açabilir `ErrorSignal` sınıf ve onun `Raise` yöntemi. `Raise` Yöntemi geçirilir bir `Exception` nesne ve bu özel durum ve ASP.NET çalışma zamanı işlenen olmadan tamamladı alacağı günlüğe kaydeder. Ancak istek'ın normalde sonra yürütmeye devam eder, fark `Raise` yönteminin çağrılıp çağrılmadığını, oluşturulan işlenmeyen bir özel durum isteğin normal yürütmenin keser ve yapılandırılmış görüntülemek ASP.NET çalışma zamanı neden olur hata sayfası.

`ErrorSignal` Sınıfı, burada başarısız olabilir bir eylem yoktur, ancak kendi hata gerçekleştirilmekte olan genel işlem yıkıcı değil durumlarda kullanışlıdır. Örneğin, bir Web sitesi kullanıcının girdi alır, bir veritabanında saklar ve ardından kullanıcı bildiren bir e-posta gönderir, bir form içerebilir, bunlar bilgi işlendiği. Bilgileri veritabanına başarıyla kaydedildi, ancak e-posta iletisi gönderilirken bir hata olduğunu durumunda gerçekleşmesi gereken? Bir özel durum ve kullanıcıya hata sayfasına göndermek için bir seçenek olabilir. Ancak, bu kullanıcının girdiği bilgileri kaydedilmedi düşünmeye kafasını karıştırabilir. Başka bir yaklaşım için e-posta ile ilgili hata günlüğüne, ancak kullanıcının deneyimini herhangi bir şekilde değişiklik olacaktır. Burada `ErrorSignal` sınıfı, kullanışlıdır.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>E-posta yoluyla bildirim hatası

Bir veritabanı için günlük kaydı hataları birlikte ELMAH belirtilen alıcıya hata ayrıntılarının gönderileceği de yapılandırılabilir. Bu işlev tarafından sağlanan `ErrorMailModule` HTTP modülü; bu nedenle, bu HTTP modülü, kaydetmelisiniz `Web.config` hata ayrıntıları e-posta ile göndermek için.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Ardından, hata e-postayla ilgili bilgileri belirtin `<elmah>` öğenin `<errorMail>` bölümünde, e-postanın gönderen ve alıcı, konu gösteren ve e-posta olup olmadığını zaman uyumsuz olarak gönderilir.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Yerinde yukarıdaki ayarlara sahip bir çalışma zamanı hatası oluşturduğunda ELMAH bir e-posta gönderir support@example.com ile hata ayrıntılarını. ELMAH'ın hata e-posta hata ayrıntıları web sayfasında, yani hata iletisi, yığın izlemesi ve sunucu değişkenlerine aynı bilgileri içerir (kiracıurl **rakamlar 4** ve **5**). Hata e-posta eki olarak özel durum ayrıntıları sarı ekran, ölüm içeriği de içerir. (`YSOD.html`).

**Şekil 8** ELMAH'ın hata e-posta adresini ziyaret ederek oluşturulan gösterir `Genre.aspx?ID=foo`. Sırada **Şekil 8** yalnızca hata iletisi ve yığın izlemesi, sunucu değişkenleri daha da aşağı e-postanın gövdesinde dahil olduğunu gösterir.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Şekil 8**: hata ayrıntıları e-posta yoluyla göndermeyi ELMAH yapılandırabilirsiniz.  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Yalnızca ilgilendiğiniz hataları günlüğe kaydetme

Varsayılan olarak, ELMAH 404 ve diğer HTTP hataları dahil olmak üzere her işlenmeyen özel durum ayrıntıları kaydeder. Bunlar ya da diğer hata filtreleme kullanarak hataları türleri yok saymak için ELMAH bildirebilirsiniz. Filtreleme mantığı ELMAH tarafından 's gerçekleştirilir `ErrorFilterModule` kaydetmek için ihtiyacınız olacak HTTP modülü `Web.config` filtreleme mantığı kullanmak için. Filtreleme kurallarını belirtilen `<errorFilter>` bölümü.

Aşağıdaki biçimlendirmede 404 hataları günlüğe kaydetmemeyi ELMAH bildirir.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Hata kaydetmeniz gerekir filtreleme kullanmak için unutmayın `ErrorFilterModule` HTTP modülü.


`<equal>` Öğe içinde `<test>` bölümü bir onaylama olarak adlandırılır. Onaylama true olarak değerlendirilirse hata ELMAH'ın günlüğünden filtre uygulanır. Diğer bir onayları dahil olmak üzere kullanılabilir: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`ve benzeri. Ayrıca, kullanarak Onaylamalar birleştirebilirsiniz `<and>` ve `<or>` Boole işleçleri. Daha da basit bir JavaScript ifadesi onaylama olarak dahil edebilir, veya kendi onaylar C# veya Visual Basic'te yazma.

ELMAH'ın hata filtreleme özellikleri hakkında daha fazla bilgi için [hata filtreleme bölümü](https://code.google.com/p/elmah/wiki/ErrorFiltering) içinde [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Özet

ELMAH ASP.NET web uygulamasında günlük kaydı hataları için basit ama güçlü bir mekanizma sağlar. Microsoft'un sistem durumu izleme sistemi gibi ELMAH hataları veritabanına oturum açabilir ve hata ayrıntılarını bir geliştirici e-posta ile gönderebilirsiniz. Sistem izleme sistem, geniş bir hata günlüğü veri depolarına dahil olmak üzere, hazır desteği dışında ELMAH içerir: Microsoft SQL Server, Microsoft Access, Oracle, XML dosyalarını ve diğer birçok. Ayrıca, ELMAH için hata günlüğünü ve bir web sayfasından belirli bir hatayla ilgili ayrıntıları görüntülemek için yerleşik bir mekanizma sunar `elmah.axd`. `elmah.axd` Sayfası hata bilgilerini bir RSS akışı veya Microsoft Excel kullanarak okuyabilen bir virgülle ayrılmış değer dosyasına (CSV) olarak da işleyebilirsiniz. Ayrıca, filtre hataları için bildirim temelli veya programlı bir onayları kullanarak günlüğünden ELMAH söyleyebilirsiniz. Ve ELMAH ASP.NET sürüm 1.x uygulamaları ile kullanılabilir.

Dağıtılan her bir uygulama otomatik olarak işlenmeyen özel durumları günlüğe kaydetme ve Geliştirme takımına bildirim göndermek için bazı mekanizma olması gerekir. Bu sistem durumu izleme veya ELMAH kullanarak olup gerçekleştirilir ikincil olur. Diğer bir deyişle, gerçekten çok sistem durumu izleme veya ELMAH kullanıp önemi yoktur; Her iki sistem değerlendirir ve ardından ihtiyaçlarınıza en uygun olanı seçin. Temelde önemli bazı mekanizması üretim ortamında işlenmeyen özel durumları günlüğe yerinde koymak olduğu.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ELMAH - hata günlüğü modüller ve işleyiciler](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH proje sayfası](https://code.google.com/p/elmah/) (kaynak kodu, örnekler, wiki)
- [ELMAH ile bir Web uygulaması işlenmeyen özel durumları yakalamak için takma](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Güvenlik hata günlüğü sayfaları](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [HTTP modüller ve işleyiciler kullanarak takılabilir ASP.NET bileşenleri oluşturma](https://msdn.microsoft.com/library/aa479332.aspx)
- [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Önceki](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [İleri](precompiling-your-website-cs.md)
