---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirmesi dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: dd02b5c627fbfbb0034030f4c21207d24f6aabce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881340"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: bir kod güncelleştirme dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

İlk dağıtımdan sonra Bakımı ve web sitenizi geliştirme iş devam eder ve uzun süre önce bir güncelleştirme dağıtmak istersiniz. Bu öğretici bir güncelleştirme uygulama kodunuz dağıtma işlemini gösterir. Veritabanı değişikliği uygulamak ve Bu öğreticide dağıtan güncelleştirme gerektirmez; sonraki öğreticide veritabanı değişikliği dağıtma hakkında farklı nedir görürsünüz.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="make-a-code-change"></a>Bir kod değişikliği yapın

İçin ekleyeceksiniz basit bir örnek bir güncelleştirme olarak uygulamanıza, **Eğitmen** tarafından seçilen Eğitmen öğrettin kurslar listesini sayfa.

Çalıştırırsanız **Eğitmen** sayfasında fark olduğunu **seçin** bağlantılar kılavuzunda, ancak yapma dışında her şey satır arka plan Aç gri yapmayın.

![Eğitmen seçimi sayfası](deploying-a-code-update/_static/image1.png)

Ne zaman çalışan kod ekleyeceksiniz artık **seçin** bağlantı tıklatıldığında ve seçili Eğitmen tarafından öğrettin kurslar listesini görüntüler.

1. İçinde *Instructors.aspx*, aşağıdaki biçimlendirmeleri eklemek hemen sonra **ErrorMessageLabel** `Label` denetimi:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Sayfayı çalıştırın ve bir eğitmen seçin. Bu Eğitmeni tarafından öğrettin kurslar listesini bakın.

    ![Öğrettin kurslar Eğitmen sayfası](deploying-a-code-update/_static/image2.png)
3. Tarayıcıyı kapatın.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Kod güncelleştirmesini test ortamına dağıtma

Test, hazırlama ve üretim dağıtmak için yayımlama profillerini kullanmadan önce veritabanı yayımlama seçeneklerini değiştirmeniz gerekir. Artık, üyelik veritabanının için grant ve veri dağıtım betikleri çalıştırmanız gerekir.

1. Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayarak ve tıklatarak Sihirbazı **Yayımla**.
2. Tıklatın **Test** içinde profil **profil** aşağı açılan liste.
3. Tıklatın **ayarları** sekmesi.
4. Altında **DefaultConnection** içinde **veritabanları** bölümünde, Temizle **güncelleştirme veritabanı** onay kutusu.
5. Tıklatın **profil** sekmesini ve ardından **hazırlama** içinde profil **profil** aşağı açılan liste.
6. Yaptığınız değişiklikleri kaydetmek isteyip istemediğinizi sorulduğunda **Test** profil, tıklatın **Evet**.
7. Hazırlama profili aynı değişiklik.
8. Üretim profilde aynı değişikliği yapmak için işlemi yineleyin.
9. Kapat **Web'i Yayımla** Sihirbazı.

Basit sağlasa da, çalışan tek tıklamayla yayımlama şimdi yeniden test ortamına dağıtma olur. Bu işlemi daha hızlı hale getirmek için kullanabileceğiniz **Web tek tık Yayımla** araç.

1. İçinde **Görünüm** menüsünde seçin **araç çubukları** ve ardından **Web tek tık Yayımla**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. İçinde **Çözüm Gezgini**, ContosoUniversity projesini seçin.
3. **Web tek tık Yayımla** araç seçin **Test** yayımlama profili ve ardından **Web'i Yayımla** (sağa ve sola işaret eden oklarla simgesiyle).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfasına otomatik olarak açar.
5. Eğitmen sayfayı çalıştırın ve güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için bir eğitmen seçin.

Normalde ayrıca regresyon testi yapmak (diğer bir deyişle, yeni değişiklik herhangi bir varolan işlevsellik yarıda kaydetmedi emin olmak için sitenin geri kalanını test). Ancak bu Öğreticide bu adımı atlayın ve hazırlama ve üretim için güncelleştirmeyi dağıtmak için devam edin.

Yeniden dağıtırken, Web dağıtımı otomatik olarak hangi dosyalar değiştirilmiş belirler ve değişen dosyaları sunucuya yalnızca kopyalar. Varsayılan olarak, Web dağıtımı son değiştirilen tarihleri dosyalarda hangilerinin değişmiş belirlemek için kullanır. Dosya içeriğini değiştirmeyin olduğunda bazı kaynak denetim sistemleri tarihleri bile dosyasını değiştirin. Bu durumda, hangi dosyalar değiştirilmiş belirlemek için dosya sağlama toplamlarını kullanmak için Web dağıtımı yapılandırmak isteyebilirsiniz. Daha fazla bilgi için bkz: [neden dosyalarımın tümünü imzalanmasını bunları değişmedi rağmen?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET dağıtım SSS.

## <a name="take-the-application-offline-during-deployment"></a>Uygulamayı dağıtımı sırasında çevrimdışı yap

Dağıtmakta değişiklik tek bir sayfa için basit bir değişiklik sunulmuştur. Ancak bazen büyük değişiklikler, dağıtmak veya kod ve veritabanı değişikliklerini dağıtmak ve dağıtım bitmeden önce kullanıcı bir sayfa isterse, siteyi yanlış davranabilir. Kullanıcıların dağıtım işlemi devam ederken siteye erişmesini önlemek için kullanabileceğiniz bir *uygulama\_offline.htm* dosya. Adlı bir dosya yerleştirdiğinizde *uygulama\_offline.htm* , uygulamanızın kök klasöründe IIS uygulamanızı çalıştırmak yerine bu dosyayı otomatik olarak görüntüler. Dağıtım sırasında erişimi engellemek için yerleştirdiğiniz şekilde *uygulama\_offline.htm* kök klasöründe dağıtım işlemini çalıştırın ve Kaldır'ı *uygulama\_offline.htm* sonra başarılı dağıtımı.

Otomatik olarak varsayılan koymak için Web dağıtımı yapılandırabileceğiniz *uygulama\_offline.htm* dağıtma başladığında sunucu üzerinde dosya ve tamamlandığında kaldırın. Tüm yapmanız gereken, yapmak için aşağıdaki XML öğesi yayımlama profili (.pubxml) dosyanıza ekleyin şöyledir:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Bu öğretici için nasıl oluşturulacağı ve bir özel kullanmanın görürsünüz *uygulama\_offline.htm* dosya.

Kullanarak *uygulama\_offline.htm* hazırlama sitesi erişen kullanıcılar olmadığından hazırlama sitede gerekli değildir. Ancak her şeyi üretimde dağıtmayı planladığınız şekilde test etmek için hazırlama kullanmak iyi bir uygulamadır.

### <a name="create-appofflinehtm"></a>Uygulama oluşturma\_offline.htm

1. İçinde **Çözüm Gezgini**, çözümü sağ tıklatın ve **Ekle**ve ardından **yeni öğe**.
2. Oluşturma bir **HTML sayfası** adlı *uygulama\_offline.htm* (en son "m" silmek *.html* varsayılan olarak Visual Studio oluşturur uzantısı).
3. Şablon biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Dosyayı kaydedin ve kapatın.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopya uygulama\_web sitesinin kök klasöre offline.htm

Web sitesine dosyaları kopyalamak için herhangi bir FTP aracını kullanabilirsiniz. [FileZilla](http://filezilla-project.org/) popüler bir FTP aracıdır ve ekran görüntüleri gösterilir.

Bir FTP aracı kullanmak için üç şey gerekir: FTP URL'si, kullanıcı adı ve parola.

URL web sitesinin Pano sayfası Azure yönetim portalında gösterilir ve kullanıcı adı ve parola FTP bulunabilir *.publishsettings* daha önce indirilen dosya. Aşağıdaki adımlar bu değerleri almak nasıl gösterir.

1. Azure Yönetim Portalı'nda tıklatın **Web siteleri** sekmesinde ve hazırlama web sitesi'ye tıklayın.
2. Üzerinde **Pano** sayfası, FTP ana bilgisayar adı Bul kaydırın **Hızlı Bakış** bölümü.

    ![FTP konak adı](deploying-a-code-update/_static/image5.png)
3. Hazırlama açmak *.publishsettings* dosyasını Not Defteri'nde veya başka bir metin düzenleyici.
4. Bul `publishProfile` FTP profili için öğesi.
5. Kopya `userName` ve `userPWD` değerleri.

    ![FTP kimlik bilgileri](deploying-a-code-update/_static/image6.png)
6. FTP aracı ve FTP URL'SİNİN oturum açın.
7. Kopya *uygulama\_offline.htm* için çözüm klasöründen */site/wwwroot* hazırlama sitesi klasöründe.

    ![App_offline kopyalama](deploying-a-code-update/_static/image7.png)
8. Hazırlama sitenizin URL'ye gidin. Gördüğünüz *uygulama\_offline.htm* sayfası artık giriş sayfanız yerine görüntülenir.

    ![tarayıcı penceresinde app_offline.htm](deploying-a-code-update/_static/image8.png)

Şimdi hazırlık dağıtmaya hazır olursunuz.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Hazırlama ve üretim kodu güncelleştirmesini dağıtma

1. İçinde **Web tek tık Yayımla** araç seçin **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.

    Visual Studio güncelleştirilmiş uygulamayı dağıtır ve sitenin giriş sayfasını tarayıcıda açılır. *Uygulama\_offline.htm* dosya görüntülenir. Başarılı dağıtım doğrulamak için test edebilirsiniz önce kaldırmalısınız *uygulama\_offline.htm* dosya.
2. FTP Aracı'na dönün ve silme **uygulama\_offline.htm** hazırlama sitesinden.
3. Tarayıcı hazırlama sitede Eğitmen sayfasını açın ve güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için bir eğitmen seçin.
4. Hazırlama gibi üretim için aynı yordamı izleyin.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Değişiklikleri gözden geçirmeyi ve belirli dosyaları dağıtma

Visual Studio 2012 ayrıca tek dosyalar dağıtma yeteneği sağlar. Seçilen dosya için yerel sürüm ve dağıtılan sürümü arasındaki farklılıklar görüntüleme, dosya hedef ortamını dağıtmak veya dosyasını hedef ortamından yerel projeye kopyalayın. Öğreticinin bu bölümünde, bu özellikleri kullanmak nasıl görürsünüz.

### <a name="make-a-change-to-deploy"></a>Dağıtmak için bir değişiklik yapın

1. Açık *Content/Site.css*, bloğunu bulun `body` etiketi.
2. Değeri değiştirme `background-color` gelen `#fff` için `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Değişiklik yayımlama Önizleme penceresinde görüntüle

Kullandığınızda **Web'i Yayımla** projeyi yayımlamak için Sihirbazı değişiklikleri dosyasına çift tıklayarak dosyayı yayımlanmasını kalacaklarını görebilirsiniz **Önizleme** penceresi.

1. ContosoUniversity projeyi sağ tıklatın ve **Yayımla**.
2. Gelen **profil** aşağı açılan listesinden, **Test** yayımlama profili.
3. Tıklatın **Önizleme**ve ardından **önizlemeyi Başlat**.
4. İçinde **Önizleme** bölmesinde çift **Site.css**.

    ![Site.css çift tıklatın](deploying-a-code-update/_static/image9.png)

    **Önizleme değişiklikleri** iletişim dağıtılacak değişikliklerin önizlemesini gösterir.

    ![Site.css Önizleme değişiklikler](deploying-a-code-update/_static/image10.png)

    Çift ise *Web.config* dosyası **Önizleme değişiklikleri** iletişim yapılandırma dönüşümleri yapınızın etkisini gösterir ve profil dönüşümleri yayımlayın. Bu noktada, neden olabilecek herhangi bir şey yapmadıysanız *Web.config* dosya sunucusunda hiçbir değişiklik görmeyi beklediğiniz şekilde değiştirmek için. Ancak, **Önizleme değişiklikleri** penceresi yanlış iki değişiklik gösterir. İki XML öğeleri kaldırılacak gibi görünüyor. Bu öğeler seçtiğinizde yayımlama işlemi tarafından eklenen **Code First geçişleri yürütmek, uygulama başlatılırken** Code First bağlamı sınıf. Bunlar kaldırılmaz rağmen bunlar kaldırılmakta olan gibi görünüyor. Bu öğeler yayımlama işlemi ekler önce karşılaştırma gerçekleştirilir. Bu hata, bir sonraki sürümde düzeltilecektir.
5. **Kapat**'ı tıklatın.
6. Tıklatın **yayımlama**.
7. Tarayıcı sınama sitesi giriş sayfasına açıldığında, CSS değişikliğin etkisini görmek için sabit bir yenileme neden için CTRL + F5 tuşuna basın.

    ![CSS Değiştir etkisi](deploying-a-code-update/_static/image11.png)
8. Tarayıcıyı kapatın.

### <a name="publish-specific-files-from-solution-explorer"></a>Çözüm Gezgini'nden belirli dosyaları yayımlama

Mavi arka plan gibi yoktur ve özgün rengini geri çevirmek istediğinizden varsayalım. Bu bölümde, özgün ayarlarına doğrudan belirli bir dosya yayımlayarak geri yüklersiniz **Çözüm Gezgini**.

1. Açık *Content/Site.css* ve geri yükleme `background-color` ayarını `#fff`.
2. İçinde **Çözüm Gezgini**, sağ *Content/Site.css* dosya.

    Bağlam menüsünden üç Yayınlama seçenekleri gösterir.

    ![Çözüm Gezgini'nden seçenekleri Yayımla](deploying-a-code-update/_static/image12.png)
3. Tıklatın **önizlemesi değişiklikleri için Site.css**.

    Yerel dosya sürümü arasındaki farkları hedef ortamda göstermek için bir pencere açılır.

    ![Diff-Content/Site.css](deploying-a-code-update/_static/image13.png)
4. İçinde **Çözüm Gezgini**, sağ **Site.css** yeniden tıklatıp **yayımlama Site.css**.

    **Web yayımlama etkinliğini** penceresi gösterir dosya yayımlandı.

    ![Web yayımlama etkinlik penceresi](deploying-a-code-update/_static/image14.png)
5. Bir tarayıcıda `http://localhost/contosouniversity` URL yazıp ENTER tuşuna değiştirmek CSS etkisini görmek için yenileme bir sabit neden için CTRL + F5'e.

    ![Normal CSS ile giriş sayfası](deploying-a-code-update/_static/image15.png)
6. Tarayıcıyı kapatın.

## <a name="summary"></a>Özet

Veritabanı değişikliği içermeyen bir uygulama güncelleştirmesi dağıtmak için çeşitli yollar şimdi gördünüz ve nelerin güncelleştirildiğinin beklediğiniz olduğunu doğrulamak için değişiklikleri önizlemek öğrendiniz. Eğitmen sayfası artık olduğu bir **kurslar öğrettin** bölümü.

![Öğrettin kurslar Eğitmen sayfası](deploying-a-code-update/_static/image16.png)

Sonraki öğretici veritabanı değişikliği dağıtılacağı gösterilmiştir: Doğum Tarihi alanı veritabanına ve Eğitmen sayfasına ekleyeceksiniz.

> [!div class="step-by-step"]
> [Önceki](deploying-to-production.md)
> [sonraki](deploying-a-database-update.md)
