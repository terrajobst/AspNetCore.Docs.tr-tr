---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: c058d4314ea0bed87e22c0e6b3fa09b89b2423d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833361"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: kod güncelleştirmesi dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

İlk dağıtımdan sonra Bakım ve web sitenizi geliştirme iş devam eder ve uzun süre önce bir güncelleştirme dağıtmak istersiniz. Bu öğretici, uygulama kodunuz için bir güncelleştirme dağıtma işlemini gösterir. Güncelleştirmeyi uygulamak ve Bu öğreticide dağıtın, veritabanı değişikliği gerektirmez; farklı bir veritabanı değişiklik sonraki öğreticide dağıtma hakkında nedir görürsünüz.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="make-a-code-change"></a>Bir kod değişikliği yaptığınızda

Basit bir örnek, bir güncelleştirme uygulamanıza, için ekleyeceksiniz **Eğitmenler** seçili Eğitmenler tarafından verilen derslerimiz Listesi sayfasında.

Çalıştırırsanız **Eğitmenler** sayfasında seçeneğinde olduğunu **seçin** bağlantıları kılavuzunda, ancak yapma dışında bir satır arka plan Aç gri uygulamayın.

![Eğitmenler seçimi sayfası](deploying-a-code-update/_static/image1.png)

Şimdi ne zaman çalışan kod ekleyeceksiniz **seçin** bağlantı tıklatıldığında ve seçili Eğitmenler tarafından verilen derslerimiz listesini görüntüler.

1. İçinde *Instructors.aspx*, aşağıdaki işaretlemeyi ekleyin hemen sonra **ErrorMessageLabel** `Label` denetimi:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Sayfayı çalıştırın ve bir eğitmen seçin. Bu eğitmen tarafından verilen derslerimiz listesini görürsünüz.

    ![Verilen derslerimiz Eğitmenler sayfası](deploying-a-code-update/_static/image2.png)
3. Tarayıcıyı kapatın.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Kod güncelleştirme test ortamına dağıtın

Test, hazırlık ve üretim için dağıtma, yayımlama profillerini kullanmadan önce veritabanı yayımlama seçeneklerini değiştirmek gerekir. Artık, üyelik veritabanının verin ve verileri dağıtım betiklerini Çalıştır gerekmez.

1. Açık **Web'i Yayımla** ContosoUniversity projeye sağ tıklayıp'ı tıklatarak Sihirbazı **Yayımla**.
2. Tıklayın **Test** içinde profil **profili** aşağı açılan listesi.
3. Tıklayın **ayarları** sekmesi.
4. Altında **DefaultConnection** içinde **veritabanları** bölümünde, NET **veritabanını Güncelleştir** onay kutusu.
5. Tıklayın **profili** sekmesine ve ardından **hazırlama** içinde profil **profili** aşağı açılan listesi.
6. Yaptığınız değişiklikleri kaydetmek isteyip istemediğiniz sorulduğunda **Test** profilini, tıklayın **Evet**.
7. Hazırlama profili aynı değişikliği yapın.
8. Üretim profili aynı değişikliği yapmak için işlemi tekrarlayın.
9. Kapat **Web'i Yayımla** Sihirbazı.

Basit sağlasa da, çalışan tek tıklamayla yayımlama şimdi yeniden test ortamına dağıtma olur. Bu işlem daha hızlı hale getirmek için kullanabileceğiniz **Web tek tık Yayımla** araç çubuğu.

1. İçinde **görünümü** menüsünde seçin **araç çubukları** seçip **Web tek tık Yayımla**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. İçinde **Çözüm Gezgini**, ContosoUniversity projeyi seçin.
3. **Web tek tık Yayımla** araç seçin **Test** yayımlama profili ve ardından **Web'i Yayımla** (sağa ve sola işaret eden oklarla simgesiyle).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfasına otomatik olarak açılır.
5. Eğitmenler çalıştırırsanız ve güncelleştirme başarıyla dağıtıldığını doğrulamak için bir eğitmen seçin.

Normalde ayrıca gerileme sınaması işlemi (diğer bir deyişle, yeni değişiklik herhangi bir mevcut işlevsellik yarıda yaramadı emin olmak için sitenin geri kalanını test). Ancak bu öğretici için bu adımı atlayın ve güncelleştirmeyi hazırlama ve üretime dağıtmak için devam edin.

Yeniden dağıtırken, Web dağıtımı otomatik olarak hangi dosyaların değiştiğini belirler ve yalnızca kopya dosya sunucusuna değişti. Varsayılan olarak, Web dağıtımı son değiştirildiği tarihleri dosyalarda hangilerinin değiştiğini belirlemek için kullanır. Dosya içeriğini değiştirmeyin, bazı kaynak denetimi sistemlerini tarihleri bile dosyasını değiştirin. Bu durumda, hangi dosyaların değiştiğini belirlemek için dosya sağlama toplamı kullanmak için Web dağıtımı yapılandırmak isteyebilirsiniz. Daha fazla bilgi için [neden tüm dosyalarımı imzalanmasını bunları değişmedi rağmen?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) ASP.NET dağıtım SSS.

## <a name="take-the-application-offline-during-deployment"></a>Nastavit aplikaci dağıtımı sırasında çevrimdışı

Dağıtım yapıyorsanız artık bir tek sayfalı basit bir değişiklik değişikliktir. Ancak bazı durumlarda, daha büyük değişiklikler dağıtmadan veya hem kod hem de veritabanı değişiklikleri dağıtın ve site, yanlış bir kullanıcı dağıtımı tamamlanmadan önce bir sayfa istediğinde davranış gösterebilir. Kullanıcıların, dağıtım işlemi devam ederken siteye erişmesini engellemek için kullanabileceğiniz bir *uygulama\_offline.htm* dosya. Adlı bir dosya yerleştirdiğinizde *uygulama\_offline.htm* uygulamanızın kök klasöründe, IIS uygulamanızı çalıştırmak yerine bu dosyayı otomatik olarak görüntüler. Dağıtım sırasında erişimi engellemek için bu nedenle *uygulama\_offline.htm* kök klasöründe dağıtım işlemini çalıştırın ve Kaldır'ı *uygulama\_offline.htm* sonra başarılı Dağıtım.

Otomatik olarak bir varsayılan koymak için Web dağıtımı yapılandırabileceğiniz *uygulama\_offline.htm* dosya sunucusunda dağıtma başlatıldığında ve tamamlandığında kaldırın. Tüm yapmanız gereken, aşağıdaki XML öğesi yayımlama profili (.pubxml) dosyanıza ekleyin yapmaktır:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Bu öğretici için nasıl özel bir oluşturup göreceğiniz *uygulama\_offline.htm* dosya.

Kullanarak *uygulama\_offline.htm* hazırlama sitesi erişen kullanıcılar sahip olmadığınızdan hazırlama sitesinde gerekli değildir. Ancak, her şeyi üretimde dağıtmayı planladığınız şekilde test etmek için hazırlama kullanmak iyi bir uygulamadır.

### <a name="create-appofflinehtm"></a>Uygulama oluşturma\_offline.htm

1. İçinde **Çözüm Gezgini**, çözüme sağ tıklayın ve tıklayın **Ekle**ve ardından **yeni öğe**.
2. Oluşturma bir **HTML sayfası** adlı *uygulama\_offline.htm* ("m" son sildiğiniz *.html* varsayılan olarak Visual Studio oluşturan uzantısı).
3. Şablon biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Dosyayı kaydedin ve kapatın.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Kopya uygulama\_offline.htm web sitesinin kök klasörüne

Web sitesine dosyaları kopyalamak için herhangi bir FTP aracını kullanabilirsiniz. [FileZilla](http://filezilla-project.org/) popüler bir FTP aracıdır ve ekran görüntüleri, gösterilir.

Bir FTP aracını kullanmak için üç şeyi gerekir: FTP URL'si, kullanıcı adı ve parola.

Azure Yönetim Portalı'nda web sitesinin Pano sayfasında URL gösterilir ve kullanıcı adını ve parolasını FTP için bulunabilir *.publishsettings* daha önce indirdiğiniz dosyayı. Aşağıdaki adımlar, bu değerleri almak nasıl gösterir.

1. Azure Yönetim Portalı'nda tıklatın **Web siteleri** sekmesini ve sonra hazırlama web sitesi.
2. Üzerinde **Pano** sayfası, FTP konak adı Bul kaydırın **Hızlı Bakış** bölümü.

    ![FTP konak adı](deploying-a-code-update/_static/image5.png)
3. Hazırlama açın *.publishsettings* dosyasını Not Defteri'nde veya başka bir metin düzenleyicisi.
4. Bulma `publishProfile` FTP profili için öğesi.
5. Kopyalama `userName` ve `userPWD` değerleri.

    ![FTP kimlik bilgileri](deploying-a-code-update/_static/image6.png)
6. FTP aracını ve FTP URL'si oturum açın.
7. Kopyalama *uygulama\_offline.htm* için çözüm klasöründen */site/wwwroot* hazırlama sitesi klasöründe.

    ![App_offline kopyalayın](deploying-a-code-update/_static/image7.png)
8. Hazırlama sitenizin URL'sine gidin. Gördüğünüz *uygulama\_offline.htm* sayfası, giriş sayfanızın yerine artık görüntülenir.

    ![tarayıcı penceresinde app_offline.htm](deploying-a-code-update/_static/image8.png)

Hazırlık ortamına dağıtma artık hazırsınız.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Hazırlama ve üretim için kod güncelleştirmesi dağıtma

1. İçinde **Web tek tık Yayımla** araç seçin **hazırlama** yayımlama profili ve ardından **Web'i Yayımla**.

    Visual Studio, güncelleştirilmiş uygulamayı dağıtır ve tarayıcı sitenin ana sayfasını açar. *Uygulama\_offline.htm* dosyası görüntülenir. Başarılı dağıtımı doğrulamak için test edebilmek için önce kaldırmanız gerekir *uygulama\_offline.htm* dosya.
2. FTP aracınızın döndürür ve silme **uygulama\_offline.htm** hazırlama sitesinde.
3. Tarayıcıda, hazırlama sitesinde Eğitmenler sayfasını açın ve bir eğitmen güncelleştirme başarıyla dağıtıldığını doğrulamak için seçin.
4. Hazırlama aynılarını üretim için aynı yordamı izleyin.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Değişiklikleri gözden geçirme ve belirli dosyaları dağıtma

Visual Studio 2012'de tek tek dosyaları dağıtabilme özelliği sağlar. Seçili bir dosya için yerel sürüm dağıtılan sürümü arasındaki farkları görüntülemek, dosyayı hedef ortama dağıtmak veya dosyayı yerel proje için hedef ortam kopyalayın. Öğreticinin bu bölümünde, bu özelliklerin nasıl kullanılacağını bakın.

### <a name="make-a-change-to-deploy"></a>Dağıtmak için bir değişiklik yapın

1. Açık *Content/Site.css*, bloğunu bulun `body` etiketi.
2. Değerini `background-color` gelen `#fff` için `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Yayımlama Önizlemesi penceresinde değişiklik görüntüleme

Kullanırken **Web'i Yayımla** projeyi yayımlamak için Sihirbazı değişiklikleri dosyasına çift tıklayarak yayımlanacak neler gördüğünüz **Önizleme** penceresi.

1. ContosoUniversity projeyi sağ tıklatıp **Yayımla**.
2. Gelen **profili** aşağı açılan listesinden **Test** yayımlama profili.
3. Tıklayın **Önizleme**ve ardından **önizlemeyi Başlat**.
4. İçinde **Önizleme** bölmesinde çift **Site.css**.

    ![Site.css çift tıklayın](deploying-a-code-update/_static/image9.png)

    **Değişiklik önizlemesi** iletişim dağıtılacak değişikliklerin önizlemesi görüntülenir.

    ![Site.css için Değişiklikleri Önizle](deploying-a-code-update/_static/image10.png)

    Çift tıkladığınızda, *Web.config* dosyası **değişiklik önizlemesi** iletişim yayımlama profili dönüşümleri ve yapılandırma dönüşümleri derleme etkisini gösterir. Bu noktada, yol açacak hiçbir şey yapmadıysanız *Web.config* dosyası sunucuda hiçbir değişiklik görmeyi beklediğiniz şekilde değiştirin. Ancak, **değişiklik önizlemesi** penceresi yanlış iki değişiklik gösterir. İki XML öğeleri kaldırılacak gibi görünüyor. Bu öğeler seçtiğinizde yayımlama işlemi tarafından eklenen **Code First Migrations yürütme uygulama başlatılırken** Code First bağlam sınıf. Bunlar kaldırılmaz, ancak bunlar kaldırılıyor gibi görünüyor için yayımlama işlemi bu öğeleri eklemeden önce karşılaştırma gerçekleştirilir. Bu hata, gelecekteki bir sürümde düzeltilecektir.
5. **Kapat**'ı tıklatın.
6. Tıklayın **yayımlama**.
7. Tarayıcı Test site için ana sayfası açıldığında, CSS değişikliğin etkilerini görmek için sabit bir yenileme neden olmak için CTRL + F5 tuşlarına basın.

    ![Değişikliğin etkilerini CSS](deploying-a-code-update/_static/image11.png)
8. Tarayıcıyı kapatın.

### <a name="publish-specific-files-from-solution-explorer"></a>Belirli dosyaları Çözüm Gezgini'nden yayımlama

Yok ve mavi arka plan gibi özgün renge geri dönmek istediğiniz varsayalım. Bu bölümde, özgün ayarlarına doğrudan belirli bir dosyayı yayımlayarak geri yüklersiniz **Çözüm Gezgini**.

1. Açık *Content/Site.css* ve geri yükleme `background-color` ayarını `#fff`.
2. İçinde **Çözüm Gezgini**, sağ *Content/Site.css* dosya.

    Bağlam menüsünden üç Yayımlama seçenekleri gösterir.

    ![Seçenekleri Çözüm Gezgini'nden yayımlama](deploying-a-code-update/_static/image12.png)
3. Tıklayın **önizlemesi değişiklikleri için Site.css**.

    Yerel dosyanın sürümü arasındaki farkları hedef ortamda göstermek için bir pencere açılır.

    ![Fark-içerik/Site.css](deploying-a-code-update/_static/image13.png)
4. İçinde **Çözüm Gezgini**, sağ **Site.css** yeniden tıklatıp **yayımlama Site.css**.

    **Web yayımlama etkinliği** penceresi gösterilmektedir dosyanın yayımlandı.

    ![Web yayımlama etkinlik penceresi](deploying-a-code-update/_static/image14.png)
5. Bir tarayıcıda `http://localhost/contosouniversity` URL ve değiştirme CSS etkisini görmek için yenileyin. sabit bir neden için CTRL + F5 tuşuna basın.

    ![Normal CSS ile giriş sayfası](deploying-a-code-update/_static/image15.png)
6. Tarayıcıyı kapatın.

## <a name="summary"></a>Özet

Şimdi bir veritabanı değişiklik içermeyen bir uygulama güncelleştirmesi dağıtmanın birkaç yolunu gördünüz ve hangi güncelleştirilecek beklediğiniz olduğunu doğrulamak için Değişiklikleri Önizleme kullanmayı gördünüz. Eğitmenler sayfanın artık sahip bir **kursları verilen** bölümü.

![Verilen derslerimiz Eğitmenler sayfası](deploying-a-code-update/_static/image16.png)

Sonraki öğreticiye veritabanı değişikliği dağıtma işlemi gösterilmektedir: veritabanına ve Eğitmenler sayfasına bir doğum tarihi alan ekleyeceksiniz.

> [!div class="step-by-step"]
> [Önceki](deploying-to-production.md)
> [İleri](deploying-a-database-update.md)
