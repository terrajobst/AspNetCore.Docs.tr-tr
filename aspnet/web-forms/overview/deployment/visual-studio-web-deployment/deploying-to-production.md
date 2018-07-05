---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Üretim dağıtımı | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: 5bc39162bc53dc27230f8ccc55266212a51b6380
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369651"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Üretim dağıtımı
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir Microsoft Azure hesabı ayarlama, hazırlama ve üretim ortamları oluşturma ve hazırlama ASP.NET web uygulamanızı dağıtın ve üretim ortamlarında, Visual Studio tek tıklamayla kullanarak özellik yayımlama.

İsterseniz, bir üçüncü taraf barındırma sağlayıcısına dağıtabilirsiniz. Her bir sağlayıcı, kendi hesabı ve web sitesi yönetimi için kullanıcı arabirimi olan Bu öğreticide açıklanan yordamlardan çoğu için bir barındırma sağlayıcısı veya Azure için aynıdır. Bir barındırma sağlayıcısında bulabilirsiniz [sağlayıcı galeri](https://www.microsoft.com/web/hosting) Microsoft.com web sitesinde.

Anımsatıcı:, hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda Bu öğretici serisine sorun giderme sayfasını denetlediğinizden emin olun.

## <a name="get-a-microsoft-azure-account"></a>Microsoft Azure hesabı edinin

Azure hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Bir hazırlık ortamı oluşturma

> [!NOTE]
> Azure App Service, Bu öğretici yazıldığı olduğundan, hazırlama ve üretim ortamları oluşturmak için işlemler birçoğunu otomatik hale getirmek için yeni bir özellik eklemiştir. Bkz: [hazırlık Azure App service'taki web apps için ortamları ayarlama](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Açıklandığı gibi [Test Ortamı öğreticiye Dağıt](deploying-to-iis.md)en güvenilir test etme ortamıdır bir web sitesi barındırma sağlayıcısında üretim web sitesi gibi sahiptir. Birçok barındırma sağlayıcılarının bu karşı ek önemli maliyet avantajları tartmanız gerekir, ancak Azure'da hazırlama uygulamanızla bir ek boş web uygulaması oluşturabilirsiniz. Ayrıca bir veritabanınızın olması gerekir ve söz konusu ek masraf gider üretim veritabanınız üzerinden ya da hiçbiri olacak veya en az. Azure'da kullandığınız veritabanı depolama alanı miktarı yerine her veritabanı için ödeme yaparsınız ve hazırlamada kullanacağınız ek depolama alanı miktarını en az olacaktır.

İçinde anlatıldığı gibi [Dağıt Test Ortamı öğreticiye](deploying-to-iis.md), hazırlama ve üretim ilerlememesi ileride bir veritabanına iki veritabanlarınızı dağıtmak için. Bunları ayrı tutmak istiyorsanız, işlem aynı dışında her ortam için ek bir veritabanı oluşturursunuz ve yayımlama profili oluşturduğunuzda, doğru hedef dize her veritabanı için seçeceğiniz olacaktır.

Öğreticinin bu bölümünde, bir web uygulaması ve hazırlık ortamı için kullanılacak veritabanı oluşturursunuz ve hazırlık ortamına dağıtma ve oluşturmadan ve üretim ortamına dağıtmadan önce test etmek.

> [!NOTE]
> Aşağıdaki adımlar Azure yönetim portalını kullanarak Azure App Service'te bir web uygulaması oluşturma işlemini gösterir. Azure SDK'sının en son sürümü de sunucu Gezgini'ni kullanarak Visual Studio çıkmadan bunu yapabilirsiniz. Visual Studio 2013'te doğrudan Yayımla iletişim kutusundan bir web uygulaması oluşturabilirsiniz. Daha fazla bilgi için [Azure App Service'te ASP.NET web uygulaması oluşturma.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. İçinde [Azure Yönetim Portalı](https://manage.windowsazure.com/), tıklayın **Web siteleri**ve ardından **yeni**.
2. Tıklayın **Web sitesi**ve ardından **özel Oluştur**.

    **Yeni Web sitesi - özel Oluştur** Sihirbazı açılır. **Özel Oluştur** Sihirbazı bir web sitesi ve bir veritabanı aynı anda oluşturmanıza olanak sağlar.
3. İçinde **Web sitesi oluştur** adım Sihirbazı'nın bir dize girin **URL** kutusunu uygulamanız için benzersiz bir URL olarak kullanmak için ortamı hazırlama. Örneğin, ContosoUniversity staging123 (ContosoUniversity hazırlama alınmış durumda benzersiz olacak şekilde sonunda rastgele sayılar dahil) girin.

    Tam URL ne buraya girdiğiniz ve metin kutusunun yanındaki gördüğünüz bir son eke oluşur.
4. İçinde **bölge** aşağı açılan listesinde, size en yakın bölgeyi seçin.

    Bu ayar, web uygulamanızı çalışır veri merkezini belirtir.
5. İçinde **veritabanı** aşağı açılan listesinde **yeni SQL veritabanı oluşturma**.
6. İçinde **DB bağlantı dizesi adı** kutusunda, varsayılan değeri bırakın *DefaultConnection*.
7. İşaret kutusunun altındaki sağ oka tıklayın.

    Aşağıdaki çizimde gösterildiği **Web sitesi oluştur** örnek değerleri ile iletişim. URL ve girdiğiniz bölge farklı olacaktır.

    ![Web sitesi adım oluşturma](deploying-to-production/_static/image1.png)

    Sihirbaz ilerler **veritabanı ayarlarını belirt** adım.
8. İçinde **adı** kutusuna *ContosoUniversity* benzersiz, örneğin sağlamak için rastgele bir sayı artı *ContosoUniversity123*.
9. İçinde **sunucu** kutusunda **yeni SQL veritabanı sunucusu**.
10. Bir yönetici adı ve parola girin.

    Mevcut bir ad ve burada parolayı girmezsiniz. Yeni bir ad ve veritabanına eriştiğinizde daha sonra kullanmak üzere şimdi tanımlayacağınız parolayı girersiniz.
11. İçinde **bölge** kutusunda, web uygulaması için seçtiğiniz aynı bölgeyi seçin.

    Web sunucusu ve veritabanı sunucusu aynı bölgede tutarak en iyi performansı sağlar ve masrafları en aza indirir.
12. Tamamlanmış belirtmek üzere kutusunun altındaki onay işaretine tıklayın.

    Aşağıdaki çizimde gösterildiği **veritabanı ayarlarını belirt** örnek değerleri ile iletişim. Girdiğiniz değerlerden farklı olabilir.

    ![Veritabanı ayarları adım yeni Web sitesi - Veritabanı Sihirbazı](deploying-to-production/_static/image2.png)

    Yönetim Portalı Web siteleri sayfasına döndürür ve **durumu** sütun, web uygulamasının oluşturulduğunu gösterir. (Genellikle bir dakikadan az), bir süre sonra **durumu** sütun, web uygulaması başarıyla oluşturulduğunu gösterir. Sol gezinti çubuğunda, hesabınızda kullandığınız web apps sayısı yanında görünür **Web siteleri** simgesi ve veritabanlarının sayısını yanında görünüyorsa **SQL veritabanları** simgesi.

    ![Web siteleri Yönetim Portalı, web sitesi oluşturuldu sayfası](deploying-to-production/_static/image3.png)

    Web uygulamanızın adına çizimde gösterilen örnek uygulamadan farklı olacaktır.

## <a name="deploy-the-application-to-staging"></a>Hazırlama uygulamayı dağıtma

Bir web uygulaması ve hazırlık ortamı için veritabanı oluşturduktan sonra projeyi dağıtabilirsiniz.

> [!NOTE]
> Bu yönergeler, indirerek bir yayımlama profili oluşturma işlemi gösterilmektedir bir *.publishsettings* dosya, yalnızca Azure için aynı zamanda üçüncü taraf barındırma sağlayıcıları için çalışır. En son Azure SDK'sı Ayrıca, Visual Studio'dan doğrudan Azure'a bağlanın ve Azure hesabınızda sahip web uygulamaları listesinden sağlar. Visual Studio 2013'te, Azure'da oturum açarak **Web Yayımlama** iletişim veya **Sunucu Gezgini** penceresi. Daha fazla bilgi için [Azure App Service'te ASP.NET web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>.Publishsettings dosyasını indirin

1. Yeni oluşturduğunuz web uygulamasını adına tıklayın.

    ![Sitenin panosuna gitmek için tıklayın](deploying-to-production/_static/image4.png)
2. Altında **Hızlı Bakış** içinde **Pano** sekmesinde **yayımlama profili indir**.

    ![Yayımlama profili bağlantısını indirin](deploying-to-production/_static/image5.png)

    Bu adım, web uygulamanızı bir uygulamayı dağıtmak için gereken tüm ayarları içeren bir dosya yükler. Bu bilgileri el ile girmek zorunda kalmamak için bu dosyayı Visual Studio'ya içeri aktaracağız.
3. Kaydet *.publishsettings* dosya bir klasördeki Visual Studio'dan erişebilirsiniz.

    ![.publishsettings dosyasını kaydetme](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Güvenlik - *.publishsettings* dosyası (kodlanmamış), Azure abonelik ve hizmetleri yönetmek için kullanılan kimlik içeriyor. Bu dosya için en iyi güvenlik uygulaması, kaynak dizinleri (örneğin Libraries\Documents klasöründe) dışında geçici olarak depolar ve içeri aktarma tamamlandıktan sonra Sil sağlamaktır. Erişim kazanır kötü niyetli bir kullanıcının *.publishsettings* dosya düzenleme, oluşturabilir ve Azure hizmetlerinizi silin.

### <a name="create-a-publish-profile"></a>Bir yayımlama profili oluşturun

1. Visual Studio'da ContosoUniversity projeye sağ **Çözüm Gezgini** seçip **Yayımla** bağlam menüsünden.

    **Web'i Yayımla** Sihirbazı açılır.
2. Tıklayın **profili** sekmesi.
3. **İçeri aktar**'a tıklayın.
4. Gidin *.publishsettings* dosyasını daha önce indirdiğiniz ve ardından **açık**.

    ![İçeri aktarma yayımlama Ayarları iletişim kutusu](deploying-to-production/_static/image7.png)
5. İçinde **bağlantı** sekmesinde **bağlantıyı doğrula** ayarlarının doğru olduğundan emin olmak için.

    Bağlantı doğrulandı, yeşil bir onay işareti yanında gösterilen **bağlantıyı doğrula** düğmesi.

    ' A tıkladığınızda bazı barındırma sağlayıcıları için **bağlantıyı doğrula**, görebileceğiniz bir **sertifika hatası** iletişim kutusu. Bunu yaparsanız, sunucu adının beklediğiniz olduğunu doğrulayın. Sunucu adı doğruysa seçin **bu sertifikayı gelecekteki Visual Studio oturumları için Kaydet** tıklatıp **kabul**. (Bu hata bir barındırma sağlayıcısına dağıtım yaptığınız URL için bir SSL sertifikası satın alma maliyetinden kaçının seçmiştir anlamına gelir. Geçerli bir sertifika kullanarak güvenli bir bağlantı oluşturmak isterseniz, barındırma sağlayıcınızla bağlantı kurun.)
6. **İleri**'ye tıklayın.

    ![Bağlantı başarılı simgesi ve bağlantı sekmesinde İleri düğmesi](deploying-to-production/_static/image8.png)
7. İçinde **ayarları** sekmesinde, genişletme **dosya yayımlama seçeneği**ve ardından **uygulamadan dosyaları dışlama\_veri klasörü**.

    Diğer seçenekler hakkında bilgi için **dosya yayımlama seçeneği**, bkz: [IIS'ye dağıtma](deploying-to-iis.md) öğretici. Veritabanı yapılandırma adımları sona sonucudur ve bu adımı aşağıdaki veritabanı yapılandırma adımlarını gösteren ekran görüntüsü.
8. Altında **DefaultConnection** içinde **veritabanları** bölümünde, üyelik veritabanının veritabanı dağıtımı yapılandırın.
9. 1. Seçin **veritabanını Güncelleştir**.

        **Uzak bağlantı dizesi** kutuyu doğrudan **DefaultConnection** .publishsettings dosyasından bağlantı dizesini doldurulur. Bağlantı dizesi düz metin olarak depolanır, SQL Server kimlik bilgilerini içeren *.pubxml* dosya. Kalıcı olarak yok depolamaya değil tercih ederseniz, veritabanı dağıtıldıktan sonra yayımlama profilini kaldırın ve bunun yerine Azure'a depolamak. Daha fazla bilgi için [ASP.NET veritabanı bağlantı dizelerini kaynak sunucudan Azure'a dağıtırken güvenliğini nasıl](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) Scott Hanselman'ın blogunda.
      2. Tıklayın **yapılandırma veritabanı güncelleştirmeleri**.
      3. İçinde **veritabanı güncellemelerini yapılandırma** iletişim kutusu, tıklayın **SQL komut dosyası Ekle**.
      4. İçinde **SQL komut dosyası Ekle** kutusunda, gitmek *aspnet veri prod.sql* çözüm klasöründe daha önce kaydedilmiş ve ardından betiği **açık**.
      5. Kapat **veritabanı güncellemelerini yapılandırma** iletişim kutusu.
10. Altında **SchoolContext** içinde **veritabanları** bölümünden **yürütme Code First Migrations (uygulama başlatılırken çalışır)**.

    Visual Studio görüntüler **Code First Migrations yürütme** yerine **veritabanını Güncelleştir** için `DbContext` sınıfları. Kullanarak erişen bir veritabanı dağıtmak için geçişleri yerine dbDacFx sağlayıcısı kullanmak istiyorsanız bir `DbContext` sınıfı [Code First geçişleri veritabanından nasıl dağıtırım?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) Visual Studio Web dağıtımı SSS ve ASP.NET MSDN'de.

    **Ayarları** sekmesinde şimdi aşağıdaki örnekteki gibi görünür:

    ![Hazırlama için sekmesinde ayarları](deploying-to-production/_static/image9.png)
11. Profili kaydedin ve yeniden adlandırın aşağıdaki adımları *hazırlama*:

    1. Tıklayın **profili** sekmesine ve ardından **Spravovat Profily**.
    2. İçeri aktarma, iki yeni profili, bir FTP ve Web dağıtımı için oluşturuldu. Web dağıtımı profili yapılandırılmış: Bu profile Yeniden Adlandır *hazırlama*.

        ![Hazırlık ortamına profili yeniden adlandır](deploying-to-production/_static/image10.png)
    3. Kapat **Web yayımlama profillerini Düzenle** iletişim kutusu.
    4. Kapat **Web'i Yayımla** Sihirbazı.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Yayımlama profili dönüşüm ortam göstergesi için yapılandırma

> [!NOTE]
> Bu bölümde, Web.config dönüşümü ortam göstergesi için ayarlama işlemi gösterilmektedir. Göstergenin olduğundan `<appSettings>` öğesi, Azure App Service'e dağıtırken dönüşümü belirtmek için başka bir seçenek vardır. Daha fazla bilgi için [belirten Web.config ayarları azure'da](web-config-transformations.md#watransforms).


1. İçinde **Çözüm Gezgini**, genişletme **özellikleri**ve ardından **PublishProfiles**.
2. Sağ *Staging.pubxml*ve ardından **ekleme Config dönüştürme**.

    ![Hazırlama için yapılandırma dönüşümü Ekle](deploying-to-production/_static/image11.png)

    Visual Studio oluşturur *Web.Staging.config* dönüşüm dosyasını ve onu açar.
3. İçinde *Web.Staging.config* dönüşüm dosyasında, açıldıktan hemen sonra aşağıdaki kodu ekleyin `configuration` etiketi.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Yayımlama profili hazırlama kullandığınızda, bu dönüşüm "Prod" ortam göstergesini ayarlar. Dağıtılan web uygulamasının herhangi bir sonek gibi "(Geliştirme)" veya "(Test)" sonra "Contoso Üniversitesi" H1 başlığını görmezsiniz.
4. Sağ *Web.Staging.config* tıklayın ve dosya **Önizleme dönüştürme** , kodlanmış dönüştürme beklenen değişiklikleri üretir emin olmak için.

    **Web.config Önizleme** penceresi gösterir hem de sonucu *Web.Release.config* dönüştürür ve *Web.Staging.config* dönüştürür.

### <a name="prevent-public-use-of-the-test-app"></a>Genel test uygulamasının kullanımını engelle

Hazırlama uygulamanın önemli bir konu İnternette canlı, ancak bunu kullanmak için genel istemediğiniz değildir. Kişileri bulun ve kullanın olasılığını en aza indirmek için bir veya daha fazla aşağıdaki yöntemlerden birini kullanabilirsiniz:

- Hazırlama test etmek için kullandığınız IP adreslerinden hazırlama uygulamaya erişime izin veren güvenlik duvarı kurallarını ayarlayın.
- Tahmin imkansız olan karıştırılmış bir URL kullanın.
- Oluşturma bir *robots.txt* arama motorları test uygulama ve rapor bağlantıları için arama sonuçlarında gezinme değil emin olmak için dosya.

Bu yöntemlerin ilki olan en etkili olduğu, ancak bu öğreticide bir Azure App Service yerine Azure bulut Hizmeti'ne dağıtma gerekeceğinden kapsamında değildir. Bulut hizmetleri hakkında daha fazla bilgi ve azure'da IP kısıtlamaları için bkz: [sağlanan işlem barındırma seçenekleri Azure tarafından](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) ve [blok belirli IP adreslerini bir Web rolü erişmesini](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Bir üçüncü taraf barındırma sağlayıcısına dağıttığınız durumlarda IP kısıtlamaları uygulamak nasıl öğrenmek için sağlayıcısına başvurun.

Bu öğreticide, oluşturacağınız bir *robots.txt* dosya.

1. İçinde **Çözüm Gezgini**ContosoUniversity projeye sağ tıklayın ve tıklayın **Yeni Öğe Ekle**.
2. Yeni bir **metin dosyası** adlı *robots.txt*ve aşağıdaki metni yerleştirin:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent` Satırı belirten kurallar dosyasındaki tüm arama motoru web gezginleri için (robotlar), geçerli arama motorları ve `Disallow` satırı yok sayfaları sitesinde gezinilmesi istendiğinde, belirtir.

    Üretim uygulamanızı üretim dağıtımından bu dosyayı hariç tutacak gerek katalog arama motorları istiyor musunuz? Yapılandırırsınız, yapmak için bir üretim ortamında yayımlama profili oluşturduğunuzda.

### <a name="deploy-to-staging"></a>Hazırlık ortamına dağıtma

1. Açık **Web'i Yayımla** Contoso University projeye sağ tıklayıp'ı tıklatarak Sihirbazı **Yayımla**.
2. Emin olun **hazırlama** profili seçilidir.
3. Tıklayın **yayımlama**.

    **Çıkış** penceresi hangi dağıtım eylemlerinin gerçekleştirildiğini gösterir ve dağıtımın başarılı olarak tamamlanmasına bildirir. Varsayılan tarayıcı otomatik olarak dağıtılan web uygulamasının URL'sini açar.

## <a name="test-in-the-staging-environment"></a>Hazırlık ortamında test etme

Ortam göstergesi yok olduğuna dikkat edin ("(test yok)" veya "(Geliştirme)" olduğunu gösterir ve H1 başlığına sonra *Web.config* ortam göstergesi için dönüştürme başarılı oldu.

![Giriş sayfası hazırlama](deploying-to-production/_static/image12.png)

Çalıştırma **Öğrenciler** dağıtılan veritabanı öğrenci olduğunu doğrulamak için sayfa.

Çalıştırma **Eğitmenler** sayfa Code First Eğitmen veri veritabanıyla çekirdek değeri oluşturulmuş olduğunu doğrulamak için:

Seçin **ekleme Öğrenciler** gelen **Öğrenciler** menüsünden bir öğrenci eklemek ve ardından yeni bir öğrenci olarak görüntüleyin **Öğrenciler** sayfasına veritabanına başarıyla yazabildiğinizi doğrulayın .

Gelen **kursları** sayfasında **güncelleştirme KREDİLERİ**. **Güncelleştirme KREDİLERİ** sayfa yönetici izinleri gerektirir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "prodpwd") oluşturduğunuz yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme KREDİLERİ** sayfası görüntülendiğinde, önceki öğreticide oluşturduğunuz yönetici hesabı test ortamı için doğru şekilde dağıtıldığını doğrular.

ELMAH izler ve ardından ELMAH hata raporu istek, bir hataya neden geçersiz bir URL isteği. Bir üçüncü taraf barındırma sağlayıcısına dağıtıyorsanız, büyük olasılıkla raporun önceki öğreticide boş aynı nedenle boş olduğunu bulabilirsiniz. Günlük klasörü için yazma ELMAH etkinleştirmek için klasör izinlerini yapılandırmak için barındırma sağlayıcısının hesabı yönetim araçlarını kullanın gerekecektir.

Oluşturduğunuz uygulama artık yalnızca ne üretim için kullanacağınız gibi olan bir web uygulaması bulutta çalışıyor. Her şeyin düzgün çalıştığından olduğundan, üretim için sonraki adım dağıtmaktır.

## <a name="deploy-to-production"></a>Üretime dağıtma

Dışlanacak ihtiyacınız dışında bir üretim web uygulaması oluşturma ve Üretim dağıtımı işlemi hazırlama, aynıdır *robots.txt* dağıtım. Bunu yapmak için yayımlama profili dosyasını düzenlemeniz.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Üretim ortamı oluşturabilir ve üretim yayımlama profili

1. Hazırlama için kullandığınız yordamın aynısını uygulayarak Azure'da, üretim web uygulaması ve veritabanı oluşturun.

    Veritabanı oluşturduğunuzda, daha önce oluşturduğunuz sunucuya koymak seçin veya yeni bir sunucu oluşturun.
2. İndirme *.publishsettings* dosya.
3. Üretim içeri aktararak yayımlama profilini oluşturma *.publishsettings* hazırlama için kullanılan aynı yordamı dosyası.

    Veri dağıtım betiği altında yapılandırmak unutmayın **DefaultConnection** içinde **veritabanları** bölümünü **ayarları** sekmesi.
4. Yeniden adlandırmak için yayımlama profili *üretim*.
5. Hazırlama için kullandığınız yordamın aynısını uygulayarak ortamı göstergesi için yayımlama profili dönüşüm Yapılandır...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Robots.txt dışlanacak .pubxml dosyayı Düzenle

Yayımlama profilini dosyaları &lt;profilename&gt;*.pubxml* ve bulunan *PublishProfiles* klasör. *PublishProfiles* klasördür altında *özellikleri* altında bir C# web uygulaması bir klasörde proje *Projem* VB web uygulama projesinde veya altında klasör *Uygulama\_veri* klasöründe bir web uygulama projesi. Her *.pubxml* dosyası için geçerli olan ayarları içerir yayımlama profili. Web'i Yayımla Sihirbazı'nda girdiğiniz değerler, bu dosyalarda saklanır ve Visual Studio kullanıcı Arabiriminde sunulan bulunmayan ayarları oluşturmak veya değiştirmek için bunları düzenleyebilirsiniz.

Varsayılan olarak, *.pubxml* bir yayımlama profili oluşturun ancak projeden çıkarın ve Visual Studio yine de bunları kullandığınızda dosyalar projeye eklenir. Visual Studio arar *PublishProfiles* klasör *.pubxml* dosyalar projeye olup olmadığı dahil ne olursa olsun.

Her *.pubxml* dosya var. bir *. pubxml.user* dosya. *. Pubxml.user* seçtiyseniz, dosyası şifrelenen parolayı içerir **Parolayı Kaydet** seçeneği ve varsayılan olarak projeden çıkarılır.

A *.pubxml* dosya belirli bir yayımlama profiline ait ayarlarını içerir. Tüm profiller için geçerli ayarları yapılandırmak istiyorsanız, oluşturabileceğiniz bir *. wpp.targets* dosya. Derleme işlemi bu dosyaya aktarır *.csproj* veya *.vbproj* proje dosyası, böylece proje dosyasında yapılandırabileceğiniz çoğu ayarları içinde bu dosyaları yapılandırılabilir. Hakkında daha fazla bilgi için *.pubxml* dosyaları ve *. wpp.targets* dosyaları görmek [nasıl yapılır: dağıtım ayarlarını düzenle yayımlama profili (.pubxml) dosyalarını ve. wpp.targets dosyasını Visual Studio'da Web projeleri](https://msdn.microsoft.com/library/ff398069.aspx).

1. İçinde **Çözüm Gezgini**, genişletme **özellikleri** genişletin **PublishProfiles**.
2. Sağ *Production.pubxml* tıklatıp **açık**.

    ![.Pubxml dosyasını açın](deploying-to-production/_static/image13.png)
3. Sağ *Production.pubxml* tıklatıp **açık**.
4. Kapatmadan hemen önce aşağıdaki satırları ekleyin `PropertyGroup` öğesi:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    .Pubxml dosyası artık şu örnekteki gibi görünür:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Dosya ve klasörleri dışlama hakkında daha fazla bilgi için bkz. [miyim belirli dosyaları veya klasörleri dağıtım kapsamından çıkarabilirsiniz?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) içinde **Visual Studio ve ASP.NET Web dağıtımı hakkında SSS** MSDN'de.

### <a name="deploy-to-production"></a>Üretime dağıtma

1. Açık **Web'i Yayımla** Sihirbazı emin olun **üretim** yayımlama profilini seçilir ve ardından **önizlemeyi Başlat** üzerinde **Önizleme**doğrulamak için sekmesinde *robots.txt* dosya üretim uygulamasına kopyalanmayacak.

    ![Üretime yayımlanacak dosyaları önizlemesi](deploying-to-production/_static/image14.png)

    Kopyalanacak dosyaların listesini gözden geçirin. Göreceksiniz tüm *.cs* dahil dosyaları *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, ve  *Master.Designer.cs* dosyaları atlanmış. Bu kod tüm derlenmiş içine *ContosoUniversity.dll* ve *ContosUniversity.pdb* içinde bulabilirsiniz dosyaları *bin* klasör. Çünkü yalnızca *.dll* için gerekli çalışma uygulama ve belirtilen daha önce uygulamayı çalıştırmak için gerekli dosyaları dağıtılması gerektiğini, Hayır *.cs* dosyalar hedef konuma kopyalanmıştır ortam. *Obj* klasörü ve *ContosoUniversity.csproj* ve *. csproj.user* dosyaları aynı nedenden dolayı atlanmış.

    Tıklayın **Yayımla** üretim ortamına dağıtmak için.
2. Hazırlama için kullanılan aynı yordamı üretimde test edin.

    Her şeyi hazırlama URL'si dışında ve olmaması için aynı *robots.txt* dosya.

## <a name="summary"></a>Özet

Artık başarıyla dağıtılan ve web uygulamanızı test ve genel Internet üzerinden kullanılabilir.

![Giriş sayfası üretim](deploying-to-production/_static/image15.png)

Sonraki öğreticide uygulama kodunu güncelleştirmesi ve değişiklik test, hazırlık ve üretim ortamlarına dağıtın.

> [!NOTE]
> Uygulamanızı üretim ortamında olarak kullanılırken bir kurtarma planı uygulanması. Diğer bir deyişle, düzenli aralıklarla veritabanlarınızı üretim uygulamadan bir güvenli depolama konumuna yedeklemeyi ve böyle yedeklemeler çeşitli nesil tutulması. Veritabanı güncelleştirdiğinizde, bir yedek kopyadan hemen önce yapmalısınız. Daha sonra bir hata yaparsanız ve üretime dağıttıktan sonra kadar Bul yok, hala onu bozulmasından önceki olduğu duruma veritabanına kurtarmanız mümkün olacaktır. Daha fazla bilgi için [Azure SQL veritabanını yedekleme ve geri yükleme](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> Bu öğreticide SQL Server, dağıtım yaptığınız Azure SQL veritabanı sürümüdür. Dağıtım işlemi için SQL Server'ın başka sürümleri benzer olsa da, gerçek üretimde Uygulama Bazı senaryolarda Azure SQL veritabanı için özel kod gerektirebilir. Daha fazla bilgi için [Azure SQL veritabanı ile çalışmaya](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) ve [SQL Server ile Azure SQL veritabanı arasında seçim yapma](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Önceki](setting-folder-permissions.md)
> [İleri](deploying-a-code-update.md)
