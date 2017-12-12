---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: - 7 / 12 üretim ortamına dağıtma | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: ad44968975b7929f5b0f70334deabc7238797402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: - 7 / 12 üretim ortamına dağıtma
====================
tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide bir barındırma sağlayıcısına sahip bir hesap ayarlama ve dağıtma, ASP.NET web uygulaması tek tıklatmayla Visual Studio'yu kullanarak üretim ortamına Yayımlama özelliği.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Bir barındırma sağlayıcısı seçme

Contoso University uygulama ve Bu öğretici seri için ASP.NET 4 ve Web dağıtımı destekleyen bir sağlayıcı gerekir. Belirli bir barındırma şirketi, böylece öğreticileri canlı Web sitesine dağıtma eksiksiz deneyimi göstermeye seçildi. Her bir barındırma şirketi farklı özellikleri sağlar ve onların sunucuları dağıtma deneyimi biraz değişir. Ancak, bu öğreticide açıklanan işlemi için genel işlem normaldir. Cytanium.com, Bu öğretici için kullanılan barındırma sağlayıcısı kullanılabilir olan birçok biridir ve Bu öğreticide kullanımı bir onay veya öneri oluşturan değil.

Barındırma sağlayıcınız seçmek hazır olduğunuzda, özellikleri ve fiyatlarını karşılaştırabilirsiniz [sağlayıcılarının galeri](https://www.microsoft.com/web/hosting) Microsoft.com/web sitesinde.

## <a name="creating-an-account"></a>Hesap oluşturma

Seçili sağlayıcısındaki bir hesap oluşturun. Tam bir SQL Server veritabanı bir eklenir desteği ek olarak, Bu öğretici için seçin gerekmez, ancak için gerekir [SQL Server'a geçirmeyi](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) bu serideki sonraki öğretici.

Bu öğreticileri için yeni bir etki alanı adını kaydettirerek gerekmez. Sağlayıcı tarafından siteye atanmış geçici URL'yi kullanarak başarılı dağıtımı doğrulamak için test edebilirsiniz.

Hesap oluşturulduktan sonra genellikle dağıtmak ve sitenizi yönetmek için gereken tüm bilgileri içeren bir Hoş Geldiniz e-posta alırsınız. Barındırma sağlayıcınız gönderdiği bilgiler ne burada gösterilen için benzer olacaktır. Yeni hesap sahiplerine gönderilir Cytanium Hoş Geldiniz e-aşağıdaki bilgileri içerir:

- Siteniz için ayarları yönetebileceği, sağlayıcının Denetim Masası'sitesi için URL. Hoş Geldiniz e-posta kolay başvuru için bu bölümü kimliği ve parolası belirttiğiniz dahil edilmiştir. (Her ikisi de bu gösterim demo değerine değiştirildi.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Varsayılan .NET Framework sürümünü ve bunun nasıl değiştirileceği hakkında bilgi. 2.0, 3.0 veya 3.5 .NET Framework hedefleyin ASP.NET uygulamaları ile çalışan birçok barındırma siteler varsayılan 2.0. Ancak, bu ayarı değiştirmek zorunda Contoso University bir .NET Framework 4 uygulaması olduğundan. (Bir ASP.NET 4.5 uygulama için .NET 4.0 ayarı kullanırsınız.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Web sitesine erişmek için kullanabileceğiniz geçici URL'si. Bu hesap oluşturulduğunda "contosouniversity.com" var olan etki alanı adı olarak girildi. Bu nedenle geçici URL'dir `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Veritabanları ve bunlara erişmek için gereken bağlantı dizelerini ayarlama hakkında bilgi:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Araçlar ve sitenizi dağıtmak için ayarları hakkında bilgiler. (E-posta Cytanium adresinden de burada atlanmıştır WebMatrix tanımlamıştır.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>.NET Framework sürüm ayarı

Cytanium Hoş Geldiniz e-posta, .NET Framework sürümünü Değiştir hakkında yönergeler için bir bağlantı içerir. Bu yönergelerde bu Cytanium Denetim Masası'ndan yapılabilir açıklanır. Farklı görünüyor Denetim Masası siteler diğer sağlayıcıları olan veya farklı bir yolla bunu size istemeniz.

Denetim Masası URL'sine gidin. Kullanıcı adı ve parola oturum açtıktan sonra Denetim Masası bakın.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

İçinde **barındırma boşlukları** kutusunda Web simgesinin üzerine işaretçiyi tutun ve seçin **Web siteleri** menüsünde.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

İçinde **Web siteleri** kutusunda, **contosouniversity.com** (hesap oluştururken kullandığınız site adı).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

İçinde **Web sitesi özellikleri** kutusunda **uzantıları** sekmesi.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Değiştirme ASP.NET tarafından **2.0 tümleşik ardışık** için **4.0 (tümleşik ardışık)**ve ardından **güncelleştirme**.

## <a name="publishing-to-the-hosting-provider"></a>Barındırma sağlayıcısına yayımlama

Barındırma sağlayıcısı'ndan Hoş Geldiniz e-posta projeyi yayımlamak için gereken ayarları içerir ve bu bilgileri bir yayımlama profili el ile girebilirsiniz. Ancak sağlayıcısına dağıtım yapılandırmak için bir kolay ve hata potansiyeli daha az yöntemi kullanacaksınız: indirme bir *.publishsettings* dosyası ve bir yayımlama profili aktarın.

Tarayıcınızda Cytanium Denetim Masası'na gidin ve seçin **Web** ve ardından **Web siteleri.**

![Denetim Masası Web siteleri seçme](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Seçin **contosouniversity.com** web sitesi.

![Denetim Masası contosouniversity.com seçme](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Seçin **Web'de Yayımlama** sekmesi.

![Denetim Masası Web'de yayımlama sekmesi](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Web kullanıcı adı ve parola girerek yayımlama için kullanılacak kimlik bilgilerini oluşturun. Denetim Masası'na oturum açmak için kullandığınız aynı kimlik bilgilerini girebilirsiniz. Ardından **etkinleştirmek**.

![Denetim Masası yayımlama kimlik bilgileri oluşturma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Tıklatın **bu web sitesi için yayımlama profili indirme**.

![Denetim Masası indirme yayımlama profili](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Dosyasını açmanız veya kaydetmeniz istendiğinde, dosyayı kaydedin.

![Yayımlama profili dosyasını kaydedin](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

İçinde **Çözüm Gezgini** Visual Studio'da ContosoUniversity projesine sağ tıklatın ve **Yayımla**. **Web'i Yayımla** iletişim kutusu açılır **Önizleme** ile sekme **Test** profili, kullandığınız son profil olduğundan seçilmedi.

Seçin **profil** sekmesini ve sonra **alma**.

![Yayımla Web Sihirbazı'nı içeri aktarma düğmesi](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

İçinde **alma yayımlama ayarları** iletişim kutusunda *.publishsettings* dosyasını karşıdan öğesini tıklatıp **açık**. Sihirbaz, tüm doldurulmuş alanları ile bağlantı sekmesine ilerler.

![Web Sihirbazı bağlantı sekmesi yayımlama](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings dosyasını site için planlanan kalıcı URL hedef URL kutusunda geçirir, ancak henüz bu etki alanı satın almadığınız, değer geçici URL'niz ile değiştirin. Bu örnekte URL'nin,  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Bu kutu yalnızca amacı, tarayıcı otomatik olarak sonra başarıyla dağıtımdan sonra açılacak hangi URL belirtmektir. Boş bırakırsanız, yalnızca sonucu tarayıcı dağıtım sonrasında otomatik olarak başlatılmaz ' dir.

Tıklatın **bağlantıyı doğrula** ayarlarının doğru olduğundan ve sunucuya bağlanabilirsiniz doğrulanamadı. Daha önce gördüğünüz gibi yeşil bir onay işareti bağlantının başarılı olduğunu doğrular.

Doğrulama bağlantısını tıklattığınızda görebileceğiniz bir **sertifika hatası** iletişim kutusu. Bunu yaparsanız, sunucu adını beklediğiniz olduğunu doğrulayın. İse, seçin **bu sertifikayı gelecekteki Visual Studio oturumları için Kaydet** tıklatıp **kabul**. (Bu hata barındırma sağlayıcısı dağıttığınız URL'si için bir SSL sertifikası satın alma gider önlemek seçtiği anlamına gelir. Geçerli bir sertifika kullanarak güvenli bir bağlantı kurmayı tercih ederseniz, barındırma sağlayıcınızla bağlantı kurun.)

![Sertifika hatası](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)


              **İleri**'ye tıklayın.

İçinde **veritabanları** bölümünü **ayarları** sekmesinde, aynı girin, Test için girdiğiniz değerleri yayımlama profili. Gereksinim duyduğunuz bağlantı dizeleri açılan listelerde bulabilirsiniz.

- Bağlantı dizesi kutusunda **SchoolContext,** seçin`Data Source=|DataDirectory|School-Prod.sdf`
- Altında **SchoolContext**seçin **geçerli Code First Migrations**.
- Bağlantı dizesi kutusunda **DefaultConnection**seçin`Data Source=|DataDirectory|aspnet-Prod.sdf`
- Altında **DefaultConnection**, bırakın **güncelleştirme veritabanı** temizlendi.

![Web Sihirbazı ayarları sekmesi yayımlama](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)


              **İleri**'ye tıklayın.

İçinde **Önizleme** sekmesini tıklatın, **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için. Yerel bilgisayarda IIS dağıtıldığında, daha önce gördüğünüzle aynı listesine bakın.

Yayımlamadan önce Web.Production.config dönüştürme dosyanızı uygulanacak şekilde profilin adını değiştirin. Seçin **profil** sekmesinde **profillerini yönetme**.

![Web Sihirbazı profillerini yönetme yayımlama](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

İçinde **Web yayımlama profillerini Düzenle** iletişim kutusunda, üretim profili seçin, **yeniden adlandırma**ve üretim için profil adını değiştirin. Ardından **Kapat**.

![Web yayımlama profillerini iletişim kutusunu Düzenle](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Tıklatın **yayımlama**.

Uygulama barındırma sağlayıcısının yayımlanır. Sonuç gösterir **çıkış** penceresi.

![Dağıtımdan sonra çıktı penceresi](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Girdiğiniz URL tarayıcı otomatik olarak açılır **hedef URL** kutusuna **bağlantı** sekmesinde **Web'i Yayımla** Sihirbazı. Artık dışında hiçbir "(Test)" site Visual Studio'da çalıştırdığınızda aynı giriş sayfasına bakın veya "(Geliştirme)" ortam göstergesi başlık çubuğunda. Bu belirten ortamı göstergesi *Web.config* dönüştürme çalışılan doğru.

> [!NOTE]
> "(Test)" başlığı görmeye devam ederseniz, *obj* klasöründen ContosoUniversity proje ve yeniden dağıtın. Üretim profili kullandığınızı rağmen yazılımı yayın öncesi sürümlerde daha önce uygulanan dönüşüm dosyası (Web.Test.config) yeniden uygulanacağını.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Veritabanı erişimi neden olan bir sayfa çalıştırmadan önce Elmah oluşan hataların oturum açabilecek olacağından emin olun.

## <a name="setting-folder-permissions-for-elmah"></a>Elmah klasör izinlerini ayarlama

Bu serideki önceki öğreticiden unutmayın gibi uygulama Elmah hata günlük dosyalarını depoladığı uygulamanızı klasörü için yazma izinlerine sahip olduğundan emin olmanız gerekir. IIS, bilgisayarınıza yerel olarak dağıtıldığında, bu izinleri el ile ayarlayın. Bu bölümde, izinlerin Cytanium nasıl ayarlanacağı görürsünüz. (Bazı barındırma hizmeti sağlayıcıları, bunu yapmak etkinleştirmemeniz; yazma izinlerine sahip bir veya daha fazla önceden tanımlanmış klasörlere sunabilir. Bu durumda, uygulamanızı belirtilen klasörleri kullanacak şekilde değiştirmek gerekir.)

Cytanium Denetim Masası'nda klasör izinlerini ayarlayabilirsiniz. URL denetimine panel seçin gidip **Dosya Yöneticisi**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

İçinde **Dosya Yöneticisi** kutusunda **contosouniversity.com** ve ardından **wwwrooot** uygulamanın kök klasörünü görmek için. Asma kilit simgesini tıklatın **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

İçinde **dosya**/**klasör izinleri** penceresinde, seçin **okuma** ve **yazma** için onay kutularını  **contosouniversity.com** tıklatıp **izinlere**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Elmah yazma erişimi olduğundan emin olun *Elmah* bir hataya neden olan ve Elmah hata raporu görüntüleyerek klasör. Gibi geçersiz bir URL isteği *Studentsxxx.aspx*. Önce gördüğünüz gibi *GenericErrorPage.aspx* sayfası. Tıklatın **Log Out** bağlamak ve ardından çalıştırın *Elmah.axd*. Aldığınız **oturum** ilk olarak, hangi doğrular sayfa *Web.config* dönüştürme Elmah yetkilendirme başarıyla eklendi. Oturum açtıktan sonra yalnızca neden olduğu hata gösteren rapor görürsünüz.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Üretim ortamında test etme

Çalıştırma **Öğrenciler** sayfası. Veritabanını oluşturmak için Code First Migrations tetikler ilk kez için Okul veritabanına erişmek uygulama çalışır. Sayfa anında gecikmeden sonra görüntülediğinde, hiçbir Öğrenciler olduğunu gösterir.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Çalıştırma **Eğitmen** çekirdek verileri veritabanına başarıyla Eğitmen veri eklenen doğrulamak için sayfa.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Test ortamında yaptığınız gibi üretim ortamında veritabanı güncelleştirmeleri çalışır, ancak genellikle üretim veritabanınıza test verilerini girin istemediğiniz doğrulamak istiyorsunuz. Bu öğretici için testte yaptığınız aynı yöntemi kullanacaksınız. Ancak veritabanı doğrulayan bir yöntem bulmak isteyebilirsiniz gerçek bir uygulamada güncelleştirmeleri üretim veritabanına test verilerini oluşturmaksızın başarılı olur. Bazı uygulamalarda, bir şey ekleyin ve ardından silmek kullanışlı olabilir.

Öğrencinin ekleyin ve girdiğiniz verileri görüntülemek **Öğrenciler** veritabanındaki verileri güncelleştirebilirsiniz doğrulamak için sayfa.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Yetkilendirme kuralları seçerek düzgün çalıştığını doğrulamak **güncelleştirme krediler** gelen **kurslar** menüsü. **Oturum** sayfası görüntülenir. ' I tıklatın, yönetici hesabı kimlik bilgilerinizi girin **oturum**ve **güncelleştirme krediler** sayfası görüntülenir.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Oturum açma başarılıysa **güncelleştirme krediler** sayfası görüntülenir. Bu, ASP.NET üyelik veritabanının (tek bir yönetici hesabıyla) başarılı bir şekilde dağıtıldı gösterir.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Şimdi başarıyla dağıtılan ve sitenizi test ve genel olarak Internet üzerinden kullanılabilir.

## <a name="creating-a-more-reliable-test-environment"></a>Daha güvenilir bir Test Ortamı Oluşturma

İçinde anlatıldığı gibi [Test ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğretici, en güvenilir test ortamında üretim hesabı gibi sahip barındırma sağlayıcısında ikinci bir hesabı olacaktır. Bu, ikinci bir barındırma hesap için kaydolmanız sahip olduğu yerel IIS kendi test ortamınıza kullanmaktan daha pahalı olur. Ancak üretim site hataları veya kesintileri engelliyorsa, maliyetlere olduğunu karar verebilirsiniz.

Çoğu oluşturma ve test hesabına dağıtma işleminin ne üretime dağıtmak için yapmış için benzer:

- Oluşturma bir *Web.config* dönüşüm dosyası.
- Barındırma sağlayıcısındaki bir hesap oluşturun.
- Yeni bir yayımlama profili oluşturun ve test hesabına dağıtın.

### <a name="preventing-public-access-to-the-test-site"></a>Genel erişim Test siteye önleme

Önemli bir sınama hesabı için Internet'te Canlı olacaktır, ancak ortak kullanmak istemediğiniz konudur. Site gizliliğini korumak için bir veya daha fazla aşağıdaki yöntemlerden birini kullanabilirsiniz:

- Test etmek için kullandığınız IP adreslerinden test siteye erişime izin veren güvenlik duvarı kuralları ayarlamak için barındırma sağlayıcısına başvurun.
- Genel site URL'SİNİN benzer olmaması URL gizlenebilir.
- Kullanım bir *robots.txt* dosya arama motorları test site ve rapor bağlantıları için arama sonuçlarında gezinme değil emin olun.

Bu yöntemlerin ilki açıkça en güvenli seçenektir ancak yordamı için her barındırma sağlayıcısına özeldir ve Bu öğreticide kapsamında olmayan. Test hesap URL'si göz atmak yalnızca IP adreslerine izin vermek için barındırma sağlayıcınızda düzenlemek, teorik olarak bu gezinme arama motorları hakkında endişelenmeniz gerekmez. Ancak bu durumda bile dağıtma bir *robots.txt* dosyasıdır iyi bir fikir yedek olarak bu güvenlik duvarı kuralı her zamankinden yanlışlıkla devre dışı durumda.

*Robots.txt* dosya proje klasörünüzdeki gider ve aşağıdaki metni içinde olması gerekir:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Satır belirten kurallar dosyasındaki tüm arama motoru web gezginleri için (robots), geçerli arama motorları ve `Disallow` satırı belirtir sitesinde hiç sayfa gezinme.

Böylece bu dosyayı üretim dağıtımdan hariç gerek üretim sitenizi katalog için arama motorları istemenizdir. Bunu yapmak için bkz: **t belirli dosyaları veya klasörleri dağıtımdan hariç tutabilirsiniz?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/en-us/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Yalnızca üretim yayımlama için profil dışlama belirttiğinizden emin olun.

İkinci bir barındırma hesabı oluşturma, gerekli değildir, ancak eklenen gider olabilir bir test ortamı ile çalışmaya bir yaklaşımdır. Aşağıdaki öğreticilerde, IIS, test ortamı olarak kullanmak üzere devam edeceğiz.

Sonraki öğreticide uygulama kodunu güncelleştirin ve değişikliğinizi test ve üretim ortamlarını dağıtın.

>[!div class="step-by-step"]
[Önceki](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[sonraki](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
