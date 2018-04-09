---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: üretime dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: f3b3898bd003ace100ba05619f2c45ca808462df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: üretime dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir Microsoft Azure hesabı ayarlamanız, hazırlama ve üretim ortamları oluşturma ve hazırlama, ASP.NET web uygulamasına dağıtmak ve tek tıklatmayla Visual Studio'yu kullanarak üretim ortamlarında özelliği yayımlayın.

İsterseniz, bir üçüncü taraf barındırma sağlayıcısına dağıtabilirsiniz. Her bir sağlayıcı hesabı ve web sitesi yönetimi için kendi kullanıcı arabirimi olan Bu öğreticide açıklanan yordamları çoğu Azure, veya bir barındırma sağlayıcısı için aynıdır. Bir barındırma sağlayıcısına bulabilirsiniz [sağlayıcılarının galeri](https://www.microsoft.com/web/hosting) Microsoft.com web sitesinde.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, Bu öğretici serisinde sorun giderme sayfa denetlediğinizden emin olun.

## <a name="get-a-microsoft-azure-account"></a>Bir Microsoft Azure hesabı edinin

Zaten bir Azure hesabınız yoksa, yalnızca birkaç dakika içinde ücretsiz bir deneme hesabı oluşturabilirsiniz. Ayrıntılar için bkz [Azure ücretsiz deneme sürümü](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Hazırlama ortamını oluşturma

> [!NOTE]
> Bu öğretici yazıldı olduğundan, Azure App Service birçok hazırlama ve üretim ortamları oluşturma işlemleri otomatikleştirmek için yeni bir özellik eklemiştir. Bkz: [hazırlık Azure App Service'deki web uygulamaları için ortamları ayarlama](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


İçinde anlatıldığı gibi [Test Ortamı öğretici Dağıt](deploying-to-iis.md), en güvenilir test ortamıdır üretim web sitesi gibi sahip barındırma sağlayıcısında bir web sitesi. Birçok barındırma sağlayıcılarının avantajları bu önemli ek maliyet karşı tartmanız gerekir, ancak Azure'da hazırlama uygulamanızı ek boş web uygulaması oluşturabilirsiniz. Ayrıca bir veritabanınızın olması gerekir ve söz konusu ek gider üretim veritabanınız gider üzerinden ya da hiçbiri olur veya en az. Azure'da kullandığınız veritabanı depolama miktarı yerine her veritabanı için ödeme ve hazırlamada kullanacağınız ek depolama alanı miktarı en az olur.

İçinde anlatıldığı gibi [Test Ortamı öğretici Dağıt](deploying-to-iis.md), hazırlama ve üretim giderek bir veritabanına iki veritabanlarınızı dağıtmak için. Ayrı tutmak istiyorsanız, işlem aynı her ortam için ek bir veritabanı oluşturacak ve yayımlama profili oluşturduğunuzda, her veritabanı için doğru hedef dizesi seçeceğiniz olacaktır.

Öğreticinin Bu bölümde bir web uygulaması ve veritabanı için hazırlık ortamı kullanılacağını oluşturacaksınız ve hazırlama için dağıtmak ve var. oluşturma ve üretim ortamına dağıtılmadan önce test.

> [!NOTE]
> Aşağıdaki adımlar Azure yönetim portalını kullanarak Azure App Service'te bir web uygulaması oluşturmak nasıl gösterir. Azure SDK'ın en son sürümünde, aynı zamanda Sunucu Gezgini kullanarak Visual Studio ayrılmadan bunu yapabilirsiniz. Visual Studio 2013'te doğrudan Yayımla iletişim kutusundan bir web uygulaması oluşturabilirsiniz. Daha fazla bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. İçinde [Azure Yönetim Portalı](https://manage.windowsazure.com/), tıklatın **Web siteleri**ve ardından **yeni**.
2. Tıklatın **Web sitesi**ve ardından **özel Oluştur**.

    **Yeni Web sitesi - özel Oluştur** Sihirbazı açılır. **Özel Oluştur** Sihirbazı, bir web sitesi ve bir veritabanı aynı anda oluşturmanıza olanak sağlar.
3. İçinde **oluşturma Web sitesi** adım Sihirbazı'nın bir dize girin **URL** kutusunu uygulamanız için benzersiz URL'si olarak kullanmak için ortam hazırlama. Örneğin, ContosoUniversity staging123 (ContosoUniversity hazırlama alınmış durumda benzersiz olması için rastgele sayılar sonunda dahil) girin.

    Tam URL ne buraya girdiğiniz ve metin kutusunun yanında göreceğiniz sonekine oluşur.
4. İçinde **bölge** aşağı açılan listesinde, size en yakın bölgeyi seçin.

    Bu ayar, web uygulamanızı çalıştırmak, hangi veri merkezi belirtir.
5. İçinde **veritabanı** aşağı açılan listesinde, seçin **yeni bir SQL veritabanı oluşturma**.
6. İçinde **DB bağlantı dizesi adı** kutusunda, varsayılan değeri bırakın *DefaultConnection*.
7. Sağ taraftaki kutusunun alt kısmındaki işaret oka tıklayın.

    Aşağıdaki çizimde gösterildiği **oluşturma Web sitesi** örnek değerleri ile iletişim. URL ve girdiğiniz bölge farklı olacaktır.

    ![Web sitesi adım oluşturma](deploying-to-production/_static/image1.png)

    İçin sihirbaz ilerler **veritabanı ayarlarını belirt** adım.
8. İçinde **adı** kutusuna *ContosoUniversity* benzersiz, örneğin olması için rastgele bir sayı artı *ContosoUniversity123*.
9. İçinde **Server** kutusunda **yeni SQL veritabanı sunucusuna**.
10. Bir yönetici adı ve parola girin.

    Mevcut bir ad ve burada parola girmezsiniz. Yeni bir ad ve veritabanına eriştiğinizde daha sonra kullanmak üzere şimdi tanımlayacağınız parolayı girersiniz.
11. İçinde **bölge** kutusunda, web uygulaması için seçtiğiniz aynı bölgeyi seçin.

    Web sunucusu ve veritabanı sunucusu aynı bölgede tutulması en iyi performansı verir ve giderler en aza indirir.
12. Tamamlanmış belirtmek üzere kutusunun altındaki onay işaretine tıklayın.

    Aşağıdaki çizimde gösterildiği **veritabanı ayarlarını belirt** örnek değerleri ile iletişim. Girdiğiniz değerler farklı olabilir.

    ![Veritabanı ayarlarını adım yeni Web sitesi - Veritabanı Sihirbazı](deploying-to-production/_static/image2.png)

    Yönetim Portalı Web siteleri sayfasına döndürür ve **durum** sütun web uygulamasının oluşturulduğunu gösterir. (Genellikle bir dakikadan az), bir süre sonra **durum** sütunda görüntülenir web uygulaması başarıyla oluşturuldu. Sol gezinti çubuğunda hesabınızda sahip web uygulamalarının sayısı görüntülenir **Web siteleri** simgesini ve veritabanı sayısı görünür yanına **SQL veritabanları** simgesi.

    ![Yönetim Portalı, web sitesi oluşturulan Web siteleri sayfası](deploying-to-production/_static/image3.png)

    Web uygulaması adı çizimde örnek uygulamasını farklı olacaktır.

## <a name="deploy-the-application-to-staging"></a>Hazırlama için uygulama dağıtma

Bir web uygulaması ve hazırlık ortamı için veritabanı oluşturduğunuza göre proje dağıtabilirsiniz.

> [!NOTE]
> Bu yönergeleri yükleyerek bir yayımlama profili oluşturmak nasıl Göster bir *.publishsettings* yalnızca Azure için üçüncü taraf barındırma hizmeti sağlayıcıları için aynı zamanda çalışır dosya. En son Azure SDK'sı ayrıca Visual Studio'dan Azure'a doğrudan bağlanmak ve Azure hesabınızda sahip web uygulamalarının listesini arasından seçim sağlar. Visual Studio 2013'te, Azure'dan oturum açarak **Web Yayımlama** iletişim veya **Sunucu Gezgini** penceresi. Daha fazla bilgi için bkz: [Azure App Service'te bir ASP.NET web uygulaması oluşturma](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>.Publishsettings dosyasını indirin

1. Yeni oluşturduğunuz web uygulaması adını tıklatın.

    ![Panoya gitmek için siteyi tıklatın](deploying-to-production/_static/image4.png)
2. Altında **Hızlı Bakış** içinde **Pano** sekmesini tıklatın, **yayım profili indirin**.

    ![Yayımlama profili bağlantı indirin](deploying-to-production/_static/image5.png)

    Bu adım, web uygulamanıza bir uygulama dağıtmak için gereken tüm ayarları içeren bir dosya yükler. Bu bilgileri el ile girmek zorunda kalmamak için bu dosyayı Visual Studio'ya içeri.
3. Kaydet *.publishsettings* Visual Studio'dan erişmek için bir klasör içindeki dosya.

    ![.publishsettings dosyasını kaydetme](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Güvenlik - *.publishsettings* dosyası Azure Abonelikleriniz ve hizmetleri yönetmek için kullanılan (kodlanmamış) kimlik bilgilerinizi içerir. Geçici olarak kaynak dizinlerinizi (örneğin Libraries\Documents klasöründe) dışında depolama ve alma işlemi tamamlandıktan sonra silmek için bu dosya için en iyi güvenlik uygulaması değil. Erişim kazanır kötü niyetli bir kullanıcı *.publishsettings* dosyayı düzenleyebilir, oluşturmak ve Azure hizmetlerinizi silebilirsiniz.

### <a name="create-a-publish-profile"></a>Bir yayımlama profili oluştur

1. Visual Studio'da ContosoUniversity projeye sağ **Çözüm Gezgini** seçip **Yayımla** ve bağlam menüsünden.

    **Web'i Yayımla** Sihirbazı açılır.
2. Tıklatın **profil** sekmesi.
3. **İçeri aktar**'a tıklayın.
4. Gidin *.publishsettings* , daha önce indirilen dosya ve ardından **açık**.

    ![İçeri aktarma yayımlama Ayarları iletişim kutusu](deploying-to-production/_static/image7.png)
5. İçinde **bağlantı** sekmesini tıklatın, **bağlantıyı doğrula** ayarlarının doğru olduğunu doğrulayın.

    Bağlantı doğrulandığında yeşil bir onay işareti yanında gösterilen **bağlantıyı doğrula** düğmesi.

    ' I tıklattığınızda bazı barındırma hizmeti sağlayıcıları için **bağlantıyı doğrula**, görebilirsiniz bir **sertifika hatası** iletişim kutusu. Bunu yaparsanız, sunucu adını beklediğiniz olduğunu doğrulayın. Sunucu adı doğruysa, seçin **bu sertifikayı gelecekteki Visual Studio oturumları için Kaydet** tıklatıp **kabul**. (Bu hata barındırma sağlayıcısı dağıttığınız URL'si için bir SSL sertifikası satın alma gider önlemek seçtiği anlamına gelir. Geçerli bir sertifika kullanarak güvenli bir bağlantı kurmayı tercih ederseniz, barındırma sağlayıcınızla bağlantı kurun.)
6. **İleri**'ye tıklayın.

    ![Bağlantı başarılı simgesi ve bağlantı sekmesinde İleri düğmesi](deploying-to-production/_static/image8.png)
7. İçinde **ayarları** sekmesinde, genişletin **dosya yayımlama seçeneği**ve ardından **uygulamadan dosyaları dışlama\_veri klasörü**.

    Diğer seçenekler hakkında bilgi için **dosya yayımlama seçeneği**, bkz: [IIS dağıtma](deploying-to-iis.md) Öğreticisi. Bu adım ve aşağıdaki veritabanı yapılandırma adımlarını veritabanı yapılandırma adımlarını sonunda sonucudur gösteren ekran görüntüsü.
8. Altında **DefaultConnection** içinde **veritabanları** bölümünde, üyelik veritabanı için veritabanı dağıtımı yapılandırın.
9. 1. Seçin **güncelleştirme veritabanı**.

        **Uzak bağlantı dizesi** kutuyu doğrudan **DefaultConnection** .publishsettings dosyasından bağlantı dizesini doldurulur. Düz metin halinde depolanır SQL Server kimlik bilgilerinin bağlantı dizesini içeren *.pubxml* dosya. Bunları kalıcı olarak var olmayan depolanacağı tercih ederseniz, veritabanı dağıtıldıktan sonra bunları kaldırmak için yayımlama profili ve bunun yerine Microsoft azure'da depoladığınız. Daha fazla bilgi için bkz: [ASP.NET veritabanı bağlantı dizeleri için Azure kaynağından dağıtırken güvenliğini nasıl](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) Scott Hanselman'ın blogunda.
      2. Tıklatın **yapılandırma veritabanı güncelleştirmeleri**.
      3. İçinde **yapılandırma veritabanı güncelleştirmeleri** iletişim kutusu, tıklatın **SQL komut dosyası Ekle**.
      4. İçinde **SQL komut dosyası Ekle** kutusunda, gitmek *aspnet veri prod.sql* çözüm klasöründe daha önce kaydedilmiş ve ardından komut dosyası **açık**.
      5. Kapat **yapılandırma veritabanı güncelleştirmeleri** iletişim kutusu.
10. Altında **SchoolContext** içinde **veritabanları** bölümünde, select **yürütme önce kod uygulamalı geçişler (uygulama başlatılırken çalışır)**.

    Visual Studio görüntüler **Code First Migrations yürütme** yerine **güncelleştirme veritabanı** için `DbContext` sınıfları. DbDacFx sağlayıcısı kullanarak erişen bir veritabanını dağıtmak için geçişler yerine kullanmak istiyorsanız bir `DbContext` sınıfı için bkz: [geçişler olmaksızın Code First bir veritabanına nasıl dağıtırım?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) Visual Studio için Web dağıtımı SSS ve ASP.NET konusuna bakın.

    **Ayarları** sekmesini şimdi aşağıdaki gibi görünür:

    ![Hazırlama için Ayarlar sekmesi](deploying-to-production/_static/image9.png)
11. Profili kaydetmek ve ona yeniden adlandırmak için aşağıdaki adımları gerçekleştirin *hazırlama*:

    1. Tıklatın **profil** sekmesini ve ardından **profillerini yönetme**.
    2. Alma işlemi iki yeni profili, bir FTP ve Web dağıtımı için bir oluşturdu. Web dağıtımı profili yapılandırılmış: Bu profile yeniden adlandırma *hazırlama*.

        ![Profili yeniden adlandırmak için hazırlama](deploying-to-production/_static/image10.png)
    3. Kapat **Web yayımlama profillerini Düzenle** iletişim kutusu.
    4. Kapat **Web'i Yayımla** Sihirbazı.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Bir yayımlama profili dönüşüm ortamı göstergesi için yapılandırma

> [!NOTE]
> Bu bölümde, ortam göstergesi için Web.config dönüştürmesi ayarlama gösterilmektedir. Göstergenin olduğundan `<appSettings>` öğesi, Azure App Service'e dağıtırken dönüştürme belirtmek için başka bir seçenek vardır. Daha fazla bilgi için bkz: [Azure belirten Web.config ayarlarında](web-config-transformations.md#watransforms).


1. İçinde **Çözüm Gezgini**, genişletin **özellikleri**, genişletin ve ardından **PublishProfiles**.
2. Sağ *Staging.pubxml*ve ardından **eklemek Config dönüştürmesi**.

    ![Config dönüştürmesi için Hazırlama Ekle](deploying-to-production/_static/image11.png)

    Visual Studio oluşturur *Web.Staging.config* dönüştürme dosyası ve açar.
3. İçinde *Web.Staging.config* dönüşüm dosyasında, açtıktan hemen sonra aşağıdaki kodu ekleyin `configuration` etiketi.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Hazırlama yayımlama profili kullandığınızda, bu dönüştürme için "Üretim" ortam göstergesi ayarlar. Dağıtılan web uygulamasının "(Geliştirme)" gibi herhangi bir sonek veya "(Test)" sonra "Contoso University" H1 başlığını görmezsiniz.
4. Sağ *Web.Staging.config* dosya ve tıklayın **Önizleme dönüştürme** , kodlanmış dönüştürme beklenen değişiklikleri üreten emin olmak için.

    **Web.config Önizleme** penceresi, her ikisi de uygulanıyor sonucunu gösterir *Web.Release.config* dönüştürür ve *Web.Staging.config* dönüştürür.

### <a name="prevent-public-use-of-the-test-app"></a>Genel test uygulamasının kullanımına engelle

Önemli bir hazırlama uygulaması için Internet'te Canlı olacaktır, ancak ortak kullanmak istemediğiniz konudur. Kişiler bulmak ve kullanmak olasılığını en aza indirmek için bir veya daha fazla aşağıdaki yöntemlerden birini kullanabilirsiniz:

- Hazırlama test etmek için kullandığınız IP adreslerinden hazırlama uygulamasına erişime izin veren güvenlik duvarı kuralları ayarlayın.
- Tahmin imkansız olan karıştırılmış bir URL kullanın.
- Oluşturma bir *robots.txt* dosya arama motorları test uygulama ve rapor bağlantıları için arama sonuçlarında gezinme değil emin olun.

Bu yöntemlerin ilki en etkili olur ancak bu öğreticide Azure uygulama hizmeti yerine bir Azure bulut hizmetine dağıtma gerekeceğinden kapsamında değildir. Bulut hizmetleri hakkında daha fazla bilgi ve Azure IP kısıtlamaları için bkz: [sağlanan işlem barındırma seçenekleri Azure tarafından](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) ve [blok belirli IP adreslerini bir Web rolü erişimini](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Bir üçüncü taraf barındırma sağlayıcısına dağıtıyorsanız, IP kısıtlamaları uygulamak nasıl öğrenmek için sağlayıcısına başvurun.

Bu öğretici için oluşturacağınız bir *robots.txt* dosya.

1. İçinde **Çözüm Gezgini**, ContosoUniversity projeyi sağ tıklatın ve **Yeni Öğe Ekle**.
2. Yeni bir **metin dosyası** adlı *robots.txt*ve aşağıdaki metni yerleştirin:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    `User-agent` Satır belirten kurallar dosyasındaki tüm arama motoru web gezginleri için (robots), geçerli arama motorları ve `Disallow` satırı belirtir sitesinde hiç sayfa gezinme.

    Arama motorları üretim dağıtımdan bu dosyayı hariç gerekmez, üretim uygulamanızın katalog istiyor musunuz? Yapılandırırsınız, yapmak için bir üretim ortamında yayımlama profili oluşturduğunuzda.

### <a name="deploy-to-staging"></a>Hazırlama için dağıtma

1. Açık **Web'i Yayımla** Contoso University projeye sağ tıklayarak ve tıklatarak Sihirbazı **Yayımla**.
2. Olduğundan emin olun **hazırlama** profili seçilidir.
3. Tıklatın **yayımlama**.

    **Çıkış** penceresinde hangi dağıtım eylemlerinin gerçekleştirilen gösterir ve dağıtımın başarılı şekilde tamamlandığını bildirir. Varsayılan tarayıcı otomatik olarak dağıtılan web uygulamasının URL'sini açar.

## <a name="test-in-the-staging-environment"></a>Hazırlama ortamında test

Ortam göstergesi yok olduğuna dikkat edin (yok "(Test)" yok veya "(Geliştirme)" gösterir H1 başlığını sonra *Web.config* dönüştürme ortamı göstergesi için başarılı oldu.

![Giriş sayfası hazırlama](deploying-to-production/_static/image12.png)

Çalıştırma **Öğrenciler** sayfa dağıtılan veritabanı hiçbir Öğrenciler sahip olduğunu doğrulayın.

Çalıştırma **Eğitmen** Code First Eğitmen veri veritabanıyla sağlanmış olduğunu doğrulamak için sayfa:

Seçin **eklemek Öğrenciler** gelen **Öğrenciler** menüsünde öğrencinin ekleyin ve ardından yeni Öğrenci görüntüleyin **Öğrenciler** sayfasını veritabanına başarıyla yazabilirsiniz doğrulamak için .

Gelen **kurslar** sayfasında, **güncelleştirme krediler**. **Güncelleştirme krediler** sayfa yönetici izinleri gerekir böylece **oturum** sayfası görüntülenir. Önceki ("Yönetici" ve "prodpwd") oluşturulan yönetici hesabı kimlik bilgilerini girin. **Güncelleştirme krediler** sayfası görüntülenirse, önceki öğreticide oluşturduğunuz yönetici hesabı için test ortamı doğru bir şekilde dağıtıldı doğrular.

ELMAH izler ve ELMAH hata raporu isteği, bir hataya neden geçersiz bir URL isteği. Bir üçüncü taraf barındırma sağlayıcısına dağıtıyorsanız, büyük olasılıkla raporun önceki öğreticide boş aynı nedenden dolayı boş olduğunu bulabilirsiniz. Günlük klasörüne yazmak ELMAH etkinleştirmek için klasör izinlerini yapılandırmak için barındırma sağlayıcısının hesabı yönetim araçlarını kullanması gerekir.

Oluşturduğunuz uygulama, yalnızca ne üretim için kullanacağınız gibi bir web uygulamasını bulutta şimdi çalışıyor. Her şeyin doğru şekilde çalıştığını olduğundan, sonraki adıma üretime dağıtmaktır.

## <a name="deploy-to-production"></a>Üretime dağıtma

Dışlamak gereken ancak bu işlem bir üretim web uygulaması oluşturma ve Üretim dağıtımı için hazırlama, aynıdır *robots.txt* dağıtımından. Bunu yapmak için yayımlama profili dosyasını düzenleyeceksiniz.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Üretim ortamı oluşturma ve üretim yayımlama profili

1. Hazırlama için kullanılan aynı yordamı izleyerek azure'da üretim web uygulaması ve veritabanı oluşturun.

    Veritabanı oluşturduğunuzda, daha önce oluşturduğunuz aynı sunucuda yerleştirilecek seçin veya yeni bir sunucu oluşturun.
2. Karşıdan *.publishsettings* dosya.
3. Üretim içeri aktararak yayımlama profili oluşturma *.publishsettings* hazırlama için kullanılan aynı yordamı izleyerek dosyasını.

    Altında veri dağıtım komut dosyası yapılandırmak unutmayın **DefaultConnection** içinde **veritabanları** bölümünü **ayarları** sekmesi.
4. Yayımlama profili yeniden adlandırma *üretim*.
5. Hazırlama için kullanılan yordamın aynısını aşağıdaki ortam göstergesi için yayımlama profili dönüşüm Yapılandır...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Robots.txt dışlanacak .pubxml dosyayı düzenleyin.

Yayımlama profili dosyalarını adlı &lt;profilename&gt;*.pubxml* ve bulunan *PublishProfiles* klasör. *PublishProfiles* klasördür altında *özellikleri* altında bir C# web uygulaması klasöründe proje *My proje* VB web uygulama projesi veya altında bir klasör *Uygulama\_veri* bir web uygulaması projesi klasöründe. Her *.pubxml* dosyası için geçerli olan ayarları içeren yayımlama profili. Web'i Yayımla sihirbazda girdiğiniz değerleri bu dosyalarında depolanır ve bunları oluşturmak veya Visual STUDİO'da kullanılabilir hale olmayan ayarlarını değiştirmek için düzenleyebilirsiniz.

Varsayılan olarak, *.pubxml* dosyaları bir yayımlama profili oluştururken ancak projeden çıkarın ve Visual Studio hala kullanacağınız bunları projeye dahil edilir. Visual Studio görünür *PublishProfiles* için klasör *.pubxml* projeye olup olmadığı dahil bağımsız olarak dosyalar.

Her *.pubxml* var. dosya bir *. pubxml.user* dosya. *. Pubxml.user* dosyası seçtiyseniz, şifrelenen parolayı içerir **Parolayı Kaydet** seçeneği ve varsayılan projeden dışlandı.

A *.pubxml* dosya bir belirli yayımlama profili ilgilidir ayarlarını içerir. Tüm profiller için geçerli ayarları yapılandırmak istiyorsanız, oluşturabileceğiniz bir *. wpp.targets* dosya. Yapılandırma işlemi bu dosyaya aktarır *.csproj* veya *.vbproj* proje dosyasında yapılandırabilirsiniz çoğu ayarları bu dosyalarda yapılandırılabilir şekilde proje dosyası. Hakkında daha fazla bilgi için *.pubxml* dosyaları ve *. wpp.targets* dosyaları görmek [nasıl yapılır: dağıtım ayarlarını düzenle yayımlama profili (.pubxml) dosyaları ve. wpp.targets dosyasını Visual Studio'da Web projeleri](https://msdn.microsoft.com/library/ff398069.aspx).

1. İçinde **Çözüm Gezgini**, genişletin **özellikleri** ve genişletin **PublishProfiles**.
2. Sağ *Production.pubxml* tıklatıp **açık**.

    ![.Pubxml dosyasını açın](deploying-to-production/_static/image13.png)
3. Sağ *Production.pubxml* tıklatıp **açık**.
4. Hemen kapatmadan önce aşağıdaki satırları ekleyin `PropertyGroup` öğe:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    .Pubxml dosya şimdi aşağıdaki gibi görünür:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Dosyaları ve klasörleri dışarıda bırak hakkında daha fazla bilgi için bkz: [t belirli dosyaları veya klasörleri dağıtımdan hariç tutabilirsiniz?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) içinde **Visual Studio ve ASP.NET Web dağıtımı hakkında SSS** MSDN'de.

### <a name="deploy-to-production"></a>Üretime dağıtma

1. Açık **Web'i Yayımla** Sihirbazı emin olun **üretim** yayımlama profili seçilir ve ardından **önizlemeyi Başlat** üzerinde **Önizleme**doğrulamak için sekme *robots.txt* dosya üretim uygulamasına kopyalanmayacak.

    ![Üretime yayımlanacak dosyalar önizlemesi](deploying-to-production/_static/image14.png)

    Kopyalanacak dosyaların listesini gözden geçirin. Göreceksiniz tüm *.cs* dahil dosyaları *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, ve  *Master.Designer.cs* dosyaları atlanmış. Bu kod tümüne derlenmiş içine *ContosoUniversity.dll* ve *ContosUniversity.pdb* içinde bulabilirsiniz dosyaları *bin* klasör. Çünkü yalnızca *.dll* için gereken uygulama ve belirtilen daha önce yalnızca uygulamayı çalıştırmak için gerekli dosyaları dağıtılmalıdır Çalıştır, Hayır *.cs* dosya hedefe kopyalanmadı ortam. *Obj* klasör ve *ContosoUniversity.csproj* ve *. csproj.user* dosyaları aynı nedenden dolayı atlanmış.

    Tıklatın **Yayımla** üretim ortamına dağıtma.
2. Hazırlama için kullanılan aynı yordamı izleyerek üretimde test edin.

    Her şeyi URL dışında hazırlama ve olmaması için aynı olan *robots.txt* dosya.

## <a name="summary"></a>Özet

Şimdi başarıyla dağıtılan ve web uygulamanızı test ve genel olarak Internet üzerinden kullanılabilir.

![Giriş sayfası üretim](deploying-to-production/_static/image15.png)

Sonraki öğreticide uygulama kodunu güncelleştirin ve test, hazırlama ve üretim ortamları için değişiklik dağıtın.

> [!NOTE]
> Uygulamanızı üretim ortamında kullanımdayken bir kurtarma planı uygulanması. Diğer bir deyişle, düzenli aralıklarla veritabanlarınızı üretim uygulamadan bir güvenli depolama konumuna yedekleme yapıyorsanız ve böyle yedekleri birkaç nesli tutulması. Veritabanını güncelleştirirken bir yedek kopyadan hemen önce olmanız gerekir. Bir hata yaparsanız ve üretim dağıttıktan sonra kadar Bul yok, daha sonra onu bozulmasından önceki durumla durum veritabanına kurtarabilmek için devam edersiniz. Daha fazla bilgi için bkz: [Azure SQL veritabanını yedekleme ve geri yükleme](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> Bu öğreticide SQL Server, dağıttığınız Azure SQL veritabanı sürümüdür. Dağıtım işlemi SQL Server'ın diğer sürümleri için benzer olmakla birlikte, gerçek üretimde uygulama, bazı senaryolarda Azure SQL veritabanı için özel kod'nı gerektirebilir. Daha fazla bilgi için bkz: [Azure SQL veritabanı ile çalışan](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) ve [SQL Server ve Azure SQL veritabanı arasında seçim yapma](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Önceki](setting-folder-permissions.md)
> [sonraki](deploying-a-code-update.md)
