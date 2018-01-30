---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: "Hata ayrıntılarını ELMAH ile (C#) günlüğü | Microsoft Docs"
author: rick-anderson
description: "Hata günlüğü modülleri ve işleyicileri (ELMAH), bir üretim ortamında çalışma zamanı hataları günlüğü başka bir yaklaşım sunmaktadır. ELMAH ücretsiz, açık kaynaklı bir hatadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 26d40d17447b3b03d17265f291b8ac246a449966
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
<a name="logging-error-details-with-elmah-c"></a>Günlük hata ayrıntılarla ELMAH (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> Hata günlüğü modülleri ve işleyicileri (ELMAH), bir üretim ortamında çalışma zamanı hataları günlüğü başka bir yaklaşım sunmaktadır. ELMAH hata filtreleme ve bir RSS bir web sayfasından hata günlüğünü görüntüleyin ya da bir virgülle ayrılmış değerler dosyası olarak karşıdan yüklemek için özelliği gibi özellikler içeren bir ücretsiz, açık kaynaklı hata günlüğü kitaplıktır. Bu öğreticide, yükleme ve yapılandırma ELMAH aracılığıyla açıklanmaktadır.


## <a name="introduction"></a>Giriş

[Önceki öğretici](logging-error-details-with-asp-net-health-monitoring-cs.md) ASP inceledi. NET'in durum çok çeşitli Web olayları kaydı için kutusunu kitaplık dışı sunar sistem izleme. Çoğu geliştiricinin durumunu oturum ve işlenmeyen özel durumlar ayrıntılarını e-posta izleme kullanın. Ancak, bu sistemle birkaç sorun teşkil edecek noktalar vardır. Öncelikle günlüğe kaydedilen olayları hakkında bilgi görüntülemek için kullanıcı arabirimi herhangi bir tür yetersizliğidir. Tarama e-posta adresiniz gelen kutusu veya derleme bilgilerini görüntüleyen bir web sayfası 10 son hataların bir özetini görmek veya geçen hafta gerçekleşen hata ayrıntılarını görüntülemek istiyorsanız, veritabanı aracılığıyla ya da Yerleştir gerekir `aspnet_WebEvent_Events` tablo.

Başka bir sorunun noktası sistem durumu izlemenin karmaşıklık toplanır. Sistem durumu izleme farklı olayları sayısız kaydetmek için kullanılabilir olduğundan ve çeşitli nasıl ve ne zaman olayların günlüğe kaydedileceğini bilgilendirerek için seçenekleri olduğundan, doğru sistem izleme sistem durumu yapılandırma onerous bir görev olabilir. Son olarak, uyumluluk sorunları vardır. Sistem durumu izleme önce .NET Framework sürüm 2.0 eklendi, ASP.NET sürüm kullanılarak oluşturulan eski web uygulamaları için kullanılabilir değildir, çünkü 1.x. Ayrıca, `SqlWebEventProvider` önceki öğreticide bir veritabanına günlükleri hata ayrıntıları için kullanıldığında, sınıf, yalnızca Microsoft SQL Server veritabanları ile çalışır. Bir XML dosyası ya da Oracle veritabanı gibi bir alternatif veri deposuna hataları günlüğe kaydetmek ihtiyacınız olursa özel günlük sağlayıcı sınıfı oluşturmanız gerekir.

Durumunu sistem izleme için hata günlüğü modülleri ve işleyicileri (ELMAH), bir ücretsiz, açık kaynaklı hata günlüğünü sistem tarafından oluşturulan alternatiftir [Atif Aziz](http://www.raboof.com/). İki sistem arasındaki en dikkat çekici fark hataları ve bir web sayfasından ve belirli bir hata ayrıntılarını bir RSS olarak listesini görüntülemek için ELAMH'ın özelliğidir. ELMAH yalnızca hatalarını günlüğe çünkü sistem durumu izleme daha yapılandırmak daha kolay olur. Ayrıca, ELMAH ASP.NET 1.x, ASP.NET 2.0 ve ASP.NET 3.5 uygulamalarını ve günlük kaynak sağlayıcıları çeşitli birlikte verilir için destek içerir.

Bu öğreticide ELMAH ASP.NET uygulamasını ekleme adımları açıklanmaktadır. Haydi başlayalım!

> [!NOTE]
> Durumunu sistem ve ELMAH izleme hem kendi Artıları ve eksileri kümesi sahiptir. I her iki sistemle deneyin ve hangi bir en iyi Setleri karar gereksinimlerinizi öneririz.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Bir ASP.NET Web uygulamasına ELMAH ekleme

Yeni veya mevcut bir ASP.NET uygulamasına ELMAH tümleştirme altında beş dakika sürer bir kolay ve kolay işlemidir. Buna koysalar dört basit adımları içerir:

1. ELMAH indirin ve ekleme `Elmah.dll` web uygulamanız için derleme
2. ELMAH'ın HTTP modülleri ve işleyicisinde kaydını `Web.config`,
3. ELMAH'ın yapılandırma seçenekleri belirtin ve
4. Hata günlüğü kaynak altyapı gerekirse oluşturun.

Şimdi her dört adımları, bir seferde bir yol.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>1. adım: ELMAH proje dosyalarını indirme ve ekleme`Elmah.dll`Web uygulamanız için

ELMAH 1.0 BETA 3 (yapı 10617), en son sürüm yazma zaman Bu öğretici ile indirilebilir dahil edilir. Alternatif olarak, ziyaret edebilirsiniz [ELMAH Web sitesi](https://code.google.com/p/elmah/) en son sürümü almak için veya kaynak kodu indirmek için. ELMAH indirme masaüstünüzdeki bir klasöre ayıklayın ve ELMAH derleme dosyasını bulun (`Elmah.dll`).

> [!NOTE]
> `Elmah.dll` Dosya indirme işlemine ait bulunur `Bin` klasörleri farklı .NET Framework sürümleri ve yayın ve hata ayıklama yapıları için varsa klasör. Yayın derlemesi için uygun framework sürümü kullanın. Örneğin, bir ASP.NET 3.5 web uygulaması oluşturuyorsanız, kopyalama `Elmah.dll` dosya `Bin\net-3.5\Release` klasör.


Ardından, Visual Studio'yu açın ve Web sitesi adı Çözüm Gezgini'nde ve bağlam menüsünden Başvuru Ekle'i seçme sağ tıklayarak derleme projenize ekleyin. Başvuru Ekle iletişim kutusunu açar. Gözat sekmesine gidin ve seçin `Elmah.dll` dosya. Bu eylem ekler `Elmah.dll` web uygulamasının dosyasına `Bin` klasör.

> [!NOTE]
> Web uygulaması projesi (WAP) türü gösterme `Bin` Çözüm Gezgininde klasör. Bunun yerine, bu öğeler başvuruları klasörü altında listelenir.


`Elmah.dll` Derleme ELMAH sistemi tarafından kullanılan sınıfları içerir. Bu sınıfların üç kategoriden ayrılır:

- **HTTP modülleri** -bir HTTP modülü için olay işleyicileri tanımlayan bir sınıftır `HttpApplication` olayları gibi `Error` olay. ELMAH içeren birden fazla HTTP modülü, üç en başlığıyla ilgili olanları oluşturuluyor: 

    - `ErrorLogModule`-İşlenmeyen özel durumlar için günlük kaynağına günlüğe kaydeder.
    - `ErrorMailModule`-e-posta iletisinde işlenmeyen bir özel durum ayrıntıları gönderir.
    - `ErrorFilterModule`-hangi özel durumları günlüğe belirlemek için filtreleri Geliştirici belirtilen ve ne yöneliktir olanları yok sayılır.
- **HTTP işleyicileri** -bir HTTP işleyicisini isteği belirli bir tür için biçimlendirme oluşturmaktan sorumlu bir sınıftır. Hata ayrıntılarını bir web sayfası olarak, bir RSS olarak veya bir virgülle ayrılmış değerler dosyası (CSV) olarak işlemek HTTP işleyicileri ELMAH içerir.
- **Hata günlüğü kaynakları** - ELMAH oturum belleği, bir Microsoft SQL Server veritabanına bir Oracle veritabanına bir Microsoft Access veritabanı hataları için kutunun dışında bir SQLite veritabanı veya Vista DB veritabanı için bir XML dosyası. Durumunu sistem izleme gibi ELMAH'in Mimarisi oluşturmak ve gerekirse, kendi özel günlük kaynak sağlayıcıları sorunsuz şekilde tümleşir anlamına sağlayıcı modeli kullanılarak oluşturuldu.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>2. adım: ELMAH'ın HTTP modülü ve işleyici kaydediliyor

Sırada `Elmah.dll` dosyasını içeren HTTP modülleri ve işleyici gerekli otomatik olarak işlenmeyen özel durumları günlüğe kaydetmek için ve bir web sayfasından hata ayrıntılarını görüntülemek için bu web uygulamasının yapılandırmasında açıkça kaydedilmelidir. `ErrorLogModule` HTTP modülü, bir kez kaydedildi, aboneliği `HttpApplication`'s `Error` olay. Bu olay her oluşturulur `ErrorLogModule` belirtilen günlük kaynağına özel durum ayrıntıları kaydeder. Sonraki bölümde, günlük kaynak sağlayıcısı tanımlamak nasıl göreceğiz "Yapılandırma ELMAH." `ErrorLogPageFactory` HTTP işleyici üreteci biçimlendirme oluşturmak için bir web sayfasından hata günlüğü görüntülerken sorumlu.

HTTP modülleri ve işleyicileri kaydettirmek için özel sözdizimi site destekleyen web sunucusu bağlıdır. ASP.NET Geliştirme Sunucusu ve Microsoft'un IIS için sürüm 6.0 ve önceki sürümleri, HTTP modülleri ve işleyicileri kayıtlı `<httpModules>` ve `<httpHandlers>` içinde görünür bölümleri `<system.web>` öğesi. IIS 7.0 kullanıyorsanız sonra kaydedilmesi ihtiyaç duydukları `<system.webServer>` öğenin `<modules>` ve `<handlers>` bölümler. Neyse ki, HTTP modülleri ve işleyiciler tanımlayabilirsiniz *her ikisi de* kullanılan web sunucusu bağımsız olarak yerleştirir. Bu seçenek çoğu taşınabilir bir aynıdır kullanılan web sunucusu bağımsız olarak geliştirme ve üretim ortamlarında kullanılmak üzere aynı yapılandırmayı sağlar.

Başlangıç kaydederek `ErrorLogModule` HTTP modülü ve `ErrorLogPageFactory` HTTP işleyicisi `<httpModules>` ve `<httpHandlers>` bölümüne `<system.web>`. Yapılandırmanızın zaten bu iki öğenin sonra yalnızca tanımlıyorsa dahil `<add>` ELMAH'ın HTTP modülü ve işleyici için öğesi.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

Ardından, ELMAH'ın HTTP modülü ve işleyicisinde kaydetmek `<system.webServer>` öğesi. Bu öğe zaten yapılandırmanızda mevcut değilse önceki gibi ardından ekleyin.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

HTTP modülleri ve işleyicileri kayıtlı değilse, varsayılan olarak, IIS 7 complains `<system.web>` bölümü. `validateIntegratedModeConfiguration` Özniteliğini `<validation>` öğesi böyle hata iletileri bastırmak için IIS 7 bildirir.

Unutmayın kaydettirmek için sözdizimi `ErrorLogPageFactory` HTTP işleyicisi içeren bir `path` ayarlanır özniteliği `elmah.axd`. Bu öznitelik web uygulaması olması durumunda sizi bilgilendirir adlı bir sayfa için bir istek ulaştığında `elmah.axd` isteği tarafından işlenmesi gerektiğini sonra `ErrorLogPageFactory` HTTP işleyicisi. Göreceğiz `ErrorLogPageFactory` daha sonra Bu öğreticide uygulamada HTTP işleyicisi.

### <a name="step-3-configuring-elmah"></a>3. adım: ELMAH yapılandırma

Kendi Web sitesinin yapılandırma seçeneklerinde arar ELMAH `Web.config` adlı özel yapılandırma bölümü dosyasında `<elmah>`. Özel bir bölümde kullanmak için `Web.config` içinde ilk tanımlanmalıdır `<configSections>` öğesi. Açık `Web.config` dosya ve aşağıdaki biçimlendirmeleri eklemek `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> ELMAH ASP.NET 1.x uygulamaya ait yapılandırıyorsanız, ardından kaldırın `requirePermission="false"` özniteliğini `<section>` yukarıdaki öğeler.


Yukarıdaki söz dizimi özel kaydeder `<elmah>` bölümü ve onun alt bölümleri: `<security>`, `<errorLog>`, `<errorMail>`, ve `<errorFilter>`.

Ardından, eklemek `<elmah>` için bölüm `Web.config`. Bu bölümde aynı düzeyde görünmelidir `<system.web>` öğesi. İçinde `<elmah>` Bölüm Ekle `<security>` ve `<errorLog>` bölümleri sözlüğüdür:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

`<security>` Bölümün `allowRemoteAccess` öznitelik, uzaktan erişim izin verilip verilmediğini gösterir. Bu değer 0 olarak ayarlarsanız, ardından hata günlüğü web sayfası yalnızca yerel olarak görüntülenebilir. Bu öznitelik 1 olarak ayarlanmışsa, hata günlüğü web sayfası uzak ve yerel ziyaretçiler için etkinleştirilir. Şimdilik, şimdi hata günlüğü web sayfası uzaktan ziyaretçiler için devre dışı bırakın. Bunun yapılması, güvenlik sorunlarının ele fırsatına sahip olduğumuz sonra size daha sonra uzaktan erişime izin.

`<errorLog>` Bölümü tanımlar burada hata ayrıntılarını kaydedilir; benzer hangi belirtir hata günlüğü kaynağı `<providers>` durum sistemini izleme bölümü. Yukarıdaki sözdizimini belirtir `SqlErrorLog` sınıfı tarafından belirtilen bir Microsoft SQL Server veritabanına hatalarını günlüğe hata günlüğü kaynağı olarak `connectionStringName` öznitelik değeri.

> [!NOTE]
> ELMAH bir XML dosyası, bir Microsoft Access veritabanı, bir Oracle veritabanına ve diğer veri depolarına hataları günlüğe kaydetmek için kullanılan ek hata günlüğü sağlayıcıları ile birlikte gelir. Örneğe bakın `Web.config` ELMAH indirme bu alternatif hata günlüğü sağlayıcılarını kullanma hakkında bilgi ile birlikte dosya.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>4. adım: hata günlüğü Kaynak Altyapısı oluşturma

ELMAH'ın `SqlErrorLog` sağlayıcı, belirtilen bir Microsoft SQL Server veritabanı hata ayrıntıları kaydeder. `SqlErrorLog` Sağlayıcı bekliyor adlı bir tablo olması için bu veritabanını `ELMAH_Error` ve üç saklı yordamlar: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, ve `ELMAH_LogError`. Adında bir dosya ELMAH indirme içerir `SQLServer.sql` içinde `db` bu tabloyu ve bunları oluşturmak için T-SQL içeren klasörü saklı yordamlar. Bu ifadeler, veritabanını kullanacak şekilde çalıştırmanız gerekir `SqlErrorLog` sağlayıcısı.

**Şekil 1** ve **2** gerekli veritabanı nesnelerini sonra Visual Studio'da Database Explorer Göster `SqlErrorLog` sağlayıcısı eklenmiştir.

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**Şekil 1**: `SqlErrorLog` sağlayıcısı için hataları günlüğe `ELMAH_Error` tablosu

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**Şekil 2**: `SqlErrorLog` sağlayıcısı kullanan üç saklı yordamlar

## <a name="elmah-in-action"></a>Eylem ELMAH

ELMAH kayıtlı web uygulaması için bu noktada ekledik `ErrorLogModule` HTTP modülü ve `ErrorLogPageFactory` HTTP işleyicisi ELMAH'ın yapılandırma seçeneklerinde belirtilen `Web.config`ve gerekli veritabanı nesnelerini eklenen `SqlErrorLog` hata günlüğü sağlayıcısı. Biz şimdi ELMAH eylemini görmek hazırsınız! Kitap incelemeleri Web sitesini ziyaret edin ve bir çalışma zamanı hatası gibi üretir sayfasını ziyaret edin `Genre.aspx?ID=foo`, ya da mevcut olmayan sayfa gibi `NoSuchPage.aspx`. Bu sayfaları ziyaret eden gördüğünüz bağlıdır `<customErrors>` yapılandırması ve yerel olarak veya uzaktan ziyaret ettiğiniz. (Geri başvurmak [ *bir özel hata sayfası görüntüleme* öğretici](displaying-a-custom-error-page-cs.md) bu konuyla ilgili Yenileyicinin.)

ELMAH işlenmeyen bir özel durum oluştuğunda hangi içerik kullanıcıya gösterilen etkilemez; yalnızca ayrıntılarını günlüğe kaydeder. Bu hata günlüğü web sayfasından erişilebilen `elmah.axd` Web sitenizin kök gibi `http://localhost/BookReviews/elmah.axd`. (Bu dosyayı projenize, ancak bir istek için geldiğinde fiziksel olarak yok `elmah.axd` çalışma zamanı için gönderir `ErrorLogPageFactory` tarayıcıya gönderilen biçimlendirmeleri oluşturur HTTP işleyicisi.)

> [!NOTE]
> Aynı zamanda `elmah.axd` sınama hatası oluşturmak için ELMAH istemek üzere sayfa. Ziyaret eden `elmah.axd/test` (gibi `http://localhost/BookReviews/elmah.axd/test`) türünde bir özel durum ELMAH neden `Elmah.TestException`, hata iletisi vardır: "Güvenle yoksayılabilir bir test özel budur."


**Şekil 3** ziyaret ederken hata günlüğü gösterir `elmah.axd` geliştirme ortamı'ndan.

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**Şekil 3**: `Elmah.axd` bir Web sayfasından hata günlüğünü görüntüler  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-elmah-cs/_static/image7.png))

Hata oturum **Şekil 3** altı hata girişleri içerir. Her giriş (404 veya bu hatalar için 500) HTTP durum kodu, türü, açıklama, hata oluştuğu sırada oturum açmış kullanıcının adını ve tarih ve saati içerir. Hata ayrıntıları sarı ekran, ölüm içinde gösterilen aynı hata iletisini içeren bir sayfa görüntüler Ayrıntılar bağlantı tıklatıldığında (bkz **Şekil 4**) hata zaman sunucu değişkenlerin değerleri birlikte (bkz  **Şekil 5**). HTTP POST üstbilgisinde değerleri gibi ek bilgileri içeren hata ayrıntılarını kaydedilen ham XML de görüntüleyebilirsiniz.

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**Şekil 4**: YSOD hata ayrıntılarını görüntüleme  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**Şekil 5**: hata zaman sunucu değişkenleri toplama değerlerini keşfedin  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-elmah-cs/_static/image13.png))

Üretim Web sitesine ELMAH dağıtma kapsar:

- Kopyalama `Elmah.dll` dosya `Bin` üretim klasörü
- ELMAH özgü yapılandırma ayarlarını kopyalama `Web.config` üretim üzerinde kullanılan dosya ve
- Hata günlüğü kaynak altyapı üretim veritabanına ekleniyor.

Biz dosyaları geliştirme önceki öğreticileri üretimde kopyalamak için teknikleri incelediniz. Belki de en kolay yolu hata günlüğü kaynak altyapı üretim veritabanında almak için SQL Server Management Studio üretim veritabanına bağlanın ve sonra yürütmek için kullanmaktır `SqlServer.sql` gerekli tablo oluşturur ve depolanan komut dosyası yordamlar.

### <a name="viewing-the-error-details-page-on-production"></a>Hata Ayrıntıları sayfası üretimde görüntüleme

Sitenizi üretime dağıtma sonra üretim Web sitesini ziyaret edin ve işlenmeyen bir özel durum oluşturur. Geliştirme ortamı olduğu gibi ELMAH işlenmeyen bir özel durum oluştuğunda görüntülenen hata sayfasında herhangi bir etkisi yoktur; Bunun yerine, yalnızca hata günlükleri. Hata günlüğü sayfasını ziyaret edin çalışırsanız (`elmah.axd`) üretim ortamından, gösterilen Yasak sayfasıyla greeted **Şekil 6**.

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**Şekil 6**: varsayılan olarak, Uzaktaki ziyaretçiler hata günlüğü Web sayfasını görüntüleyemez  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-elmah-cs/_static/image16.png))

ELMAH yapılandırma sözcüğünün `<security>` ayarlarız bölüm `allowRemoteAccess` özniteliği 0'dır, ama uzak kullanıcılar için hata günlüğünü görüntüleme yasaklar. Hata ayrıntılarını Güvenlik Açıkları ve diğer hassas bilgiler ortaya çıkarabilir gibi anonim ziyaretçilerin hata günlüğünü görüntüleme önlemek önemlidir. Bu öznitelik 1 olarak ayarlayın ve hata günlüğünü uzaktan erişimi etkinleştirmek karar sonra kilitleme önemlidir `elmah.axd` ziyaretçileri, yalnızca yetkili şekilde yolu erişebilirsiniz. Bu ekleyerek sağlanabilir bir `<location>` öğesine `Web.config` dosya.

Aşağıdaki yapılandırma, yalnızca kullanıcıların hata günlüğü web sayfasına erişmek için yönetici rolünü verir:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> Yönetici rolü ve üç kullanıcı - Scott, Jisun ve Alice - sistemindeki eklenmiştir [ *bir Web sitesi, kullanan uygulama hizmetlerini yapılandırma* Öğreticisi](configuring-a-website-that-uses-application-services-cs.md). Kullanıcıların Scott ve Jisun yönetici rolünün bir üyesi. Kimlik doğrulama ve yetkilendirme hakkında daha fazla bilgi için başvurmak my [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Üretim ortamında hata günlüğüne, artık uzak kullanıcılar tarafından görüntülenebilir; geri başvurmak **rakamları 3**, **4**, ve **5** hata günlüğü web sayfasının ekran görüntüleri için. Ancak, bunlar anonim veya yönetici olmayan bir kullanıcı hata günlüğü sayfasını görüntülemek çalışırsa oturum açma sayfasına otomatik olarak yönlendirilir (`Login.aspx`), olarak **Şekil 7** gösterir.

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**Şekil 7**: yetkisiz kullanıcıların olan otomatik olarak yeniden yönlendirilen oturum açma sayfası  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Program aracılığıyla hatalarını günlüğe kaydetme

ELMAH'ın `ErrorLogModule` HTTP modülü belirtilen günlük kaynağına işlenmeyen özel durumlar otomatik olarak günlüğe kaydeder. Alternatif olarak, bir hata işlenmeyen bir özel durum kullanarak yükseltme yapmak zorunda kalmadan oturum `ErrorSignal` sınıfı ve onun `Raise` yöntemi. `Raise` Yöntemi geçirilen bir `Exception` nesne ve başka bir özel durum atılmış ve ASP.NET çalışma zamanı işlenmiş olmadan ulaşmıştı sanki günlüğe kaydeder. Ancak istek'ın normalde sonra yürütmeye devam eder, farktır `Raise` yönteminin çağrılıp çağrılmadığını, atılmış işlenmeyen bir özel durum isteğin normal çalışmasını kesintiye uğratır ve yapılandırılmış görüntülemek ASP.NET çalışma zamanı neden olurken hata sayfası.

`ErrorSignal` Sınıfı, burada başlatılamayabilir bazı eylem yoktur, ancak kendi hatası gerçekleştirilen ve genel işlemi yıkıcı değil durumlarda yararlıdır. Bir Web sitesi, kullanıcının giriş alır, bir veritabanında depolar ve kullanıcı bildiren bir e-postası gönderir form örneği için içerebilir, bunlar bilgi işlendiği. Bilgileri veritabanına başarıyla kaydedildi, ancak e-posta iletisi gönderirken bir hata olup olmadığını ne? Bir özel durum ve hata sayfası kullanıcı göndermek için bir seçenek olabilir. Ancak, bu kullanıcının girdiği bilgileri kaydedilmedi düşünmeye karıştırır. Başka bir yaklaşım için e-posta ile ilgili hata günlüğüne, ancak herhangi bir şekilde kullanıcı deneyimini değiştirmeyin olacaktır. Bu yerdir `ErrorSignal` sınıf kullanışlıdır.

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>E-posta üzerinden hata bildirimi

Bir veritabanına günlük kaydı hataları birlikte ELMAH hata ayrıntıları için belirtilen bir alıcı e-posta ile de yapılandırılabilir. Bu işlev tarafından sağlanan `ErrorMailModule` HTTP modülü; bu nedenle, bu HTTP modülü, kaydetmelisiniz `Web.config` hata ayrıntıları e-postayla göndermek için.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

Ardından, hata e-posta ile ilgili bilgileri belirtin `<elmah>` öğenin `<errorMail>` bölümünde, e-postanın göndereni ve alıcısı, konu belirten ve e-posta olup olmadığını zaman uyumsuz olarak gönderilir.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

Yerinde yukarıdaki ayarlara sahip her bir çalışma zamanı hatası oluşur ELMAH e-posta gönderir support@example.com hata ayrıntıları ile. ELMAH'ın hata e-posta hata ayrıntıları web sayfasında, yani hata iletisi, yığın izleme ve sunucu değişkenlerini aynı bilgileri içerir (geri başvurmak **rakamları 4** ve **5**). Hata e-posta eki olarak özel durum ayrıntıları sarı ekran, ölüm içeriği de içerir (`YSOD.html`).

**Şekil 8** ELMAH'ın hata e-posta adresini ziyaret ederek oluşturulan gösterir `Genre.aspx?ID=foo`. Sırada **Şekil 8** yalnızca hata iletisi ve yığın izlemesi, sunucu değişkenlerini daha aşağı e-postanın gövdesinde birlikte gösterilir.

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**Şekil 8**: hata ayrıntıları e-postayla göndermek için ELMAH yapılandırabilirsiniz  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Yalnızca ilgilendiğiniz hatalarını günlüğe kaydetme

Varsayılan olarak, ELMAH 404 ve diğer HTTP hataları gibi her işlenmeyen özel durum ayrıntılarını kaydeder. Bu veya başka tür hata filtreleme kullanarak hataları yoksaymak için ELMAH söyleyebilirsiniz. Filtreleme mantığı ELMAH tarafından 's gerçekleştirilen `ErrorFilterModule` kaydetmek için gereken HTTP modülü `Web.config` filtreleme mantığı kullanmak için. Filtreleme kuralları belirtilen `<errorFilter>` bölümü.

Aşağıdaki biçimlendirmede 404 hatalarını günlüğe değil ELMAH bildirir.

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> Hata kaydetmeniz gerekir filtreleme kullanmak için unutmayın `ErrorFilterModule` HTTP modülü.


`<equal>` Öğesi içinde `<test>` bölüm onayı ifade adlandırılır. Onaylama işlemi true hesaplanırsa hata ELMAH'ın günlüğünden filtrelenir. Diğer onaylar dahil olmak üzere kullanılabilir: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`ve benzeri. Ayrıca, kullanarak onaylar birleştirebilirsiniz `<and>` ve `<or>` Boole işleçleri. Daha da basit bir JavaScript ifadesi bir onaylama olarak dahil edebilir, veya kendi onaylar C# veya Visual Basic'te yazma.

Filtreleme özellikleri ELMAH'ın hata hakkında daha fazla bilgi için başvurmak [hata filtreleme bölümü](https://code.google.com/p/elmah/wiki/ErrorFiltering) içinde [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Özet

ELMAH ASP.NET web uygulaması hataları günlüğe kaydetme için basit ancak güçlü bir mekanizma sağlar. Microsoft'un sistem durumu izleme sistemi gibi ELMAH hataları bir veritabanına oturum açabilir ve hata ayrıntılarını bir geliştirici e-posta yoluyla gönderebilirsiniz. Sistem sistem durumu izleme, hata günlüğü veri depolarına dahil olmak üzere, geniş bir yelpazedeki kutusunu desteği dışında ELMAH içerir: Microsoft SQL Server, Microsoft Access, Oracle, XML dosyalarını ve diğer birçok. Ayrıca, ELMAH hata günlüğünü ve bir web sayfasından belirli bir hata ayrıntılarını görüntülemek için yerleşik bir mekanizma sunar `elmah.axd`. `elmah.axd` Sayfasını da hata bilgilerini veya Microsoft Excel kullanarak okuyabilirsiniz virgülle ayrılmış değer dosyası (CSV), bir RSS olarak işlemek. Ayrıca, filtre hataları için bildirim temelli veya programlama onaylar kullanarak günlüğünden ELMAH söyleyebilirsiniz. Ve ELMAH ASP.NET sürüm 1.x uygulamaları ile kullanılabilir.

Dağıtılan her uygulama bazı mekanizması otomatik olarak işlenmeyen özel durumları günlüğe kaydetme ve geliştirme ekibi bildirim göndermek için sahip olması gerekir. Bu sistem durumu izleme veya ELMAH kullanılarak mı gerçekleştirilir ikincil olur. Diğer bir deyişle, çok sistem durumu izleme veya ELMAH kullansanız gerçekten önemli değildir; Her iki sistemle değerlendirin ve gereksinimlerine en uygun olanı seçin. Temelde önemli olan bazı mekanizması işlenmeyen özel durumlar üretim ortamında oturum yerinde konması ' dir.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ELMAH - hata günlük modülleri ve işleyicileri](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH proje sayfası](https://code.google.com/p/elmah/) (kaynak kodu, örnekleri, wiki)
- [ELMAH içine bir Web uygulaması işlenmeyen özel durumları yakalamak için takma](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (video)
- [Güvenlik hata günlüğü sayfaları](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [HTTP modülleri ve işleyicileri takılabilir ASP.NET bileşenleri oluşturmak için kullanma](https://msdn.microsoft.com/library/aa479332.aspx)
- [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[Önceki](logging-error-details-with-asp-net-health-monitoring-cs.md)
[sonraki](precompiling-your-website-cs.md)
