---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: - 12 7 üretim ortamına dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383642"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: - 12 7 üretim ortamına dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, bir barındırma sağlayıcısına sahip bir hesap ayarlamak ve dağıtmak, ASP.NET web uygulamasını Visual Studio tek tıklamayla kullanarak üretim ortamına Yayımlama özelliği.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Bir barındırma sağlayıcısı seçme

Contoso University uygulama ve Bu öğretici serisinde, ASP.NET 4 ve Web dağıtımı destekleyen sağlayıcı gerekir. Öğreticiler, canlı bir Web sitesine dağıtma eksiksiz deneyimi göstermek böylece belirli bir barındırma şirketiyle seçildi. Her bir barındırma şirketi farklı özellikleri sağlar ve kendi sunucularına dağıtma deneyimi biraz farklılık gösterir. Ancak, bu öğreticide açıklanan işlemi için genel işlem normaldir. Bu öğreticide, Cytanium.com, kullanılan barındırma sağlayıcısı kullanılabilir olan birçok biridir ve kullanımını Bu öğreticide, bir onay ya da öneri oluşturmadığına.

Barındırma sağlayıcınızı seçin hazır olduğunuzda, özellikler ve fiyatına karşılaştırabilirsiniz [sağlayıcı galeri](https://www.microsoft.com/web/hosting) Microsoft.com/web sitesinde.

## <a name="creating-an-account"></a>Hesap oluşturma

Seçilen sağlayıcısında bir hesap oluşturun. Tam SQL Server veritabanını bir eklenir desteği ek olarak, Bu öğretici için seçmek gerekmez, ancak için buna ihtiyacınız olacak [SQL Server'a geçirmeyi](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) bu serideki sonraki öğretici.

Bu öğreticiler için yeni bir etki alanı adı kaydetmeniz gerekmez. Sağlayıcı tarafından siteye atanmış geçici URL'yi kullanarak başarılı dağıtımı doğrulamak için test edebilirsiniz.

Hesap oluşturulduktan sonra genellikle dağıtmak ve sitenizi yönetmek için gereken tüm bilgileri içeren bir Hoş Geldiniz e-posta alırsınız. Barındırma sağlayıcınız, gönderdiği bilgiler aşağıda gösterilene için benzer olacaktır. Yeni hesap sahipleri için gönderilen Cytanium Hoş Geldiniz e-posta, aşağıdaki bilgileri içerir:

- Siteniz için ayarları yönetebileceğiniz, sağlayıcının Denetim Masası site URL'si. Bu parçası bir kolayca başvurmak için Hoş Geldiniz e-posta kimliği ve parolası, belirttiğiniz dahil edilmiştir. (Her ikisi de bu gösterim bir tanıtım değerine değiştirildi.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Varsayılan .NET Framework sürümünü ve bunun nasıl değiştirileceği hakkında bilgi. 2.0, 3.0 veya 3.5, .NET Framework'ü hedefleyen ASP.NET uygulamaları ile çalışan birçok barındırma siteleri varsayılana 2.0. Ancak, bu ayarı değiştirmek zorunda Contoso University bir .NET Framework 4 uygulaması olduğundan. (Bir ASP.NET 4.5 uygulama için .NET 4.0 ayarı kullanırsınız.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Web sitesine erişmek için kullanabileceğiniz geçici URL. Bu hesap oluşturulduğunda, mevcut etki alanı adı olarak "contosouniversity.com" girildi. Bu nedenle geçici URL'sidir `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Veritabanları ve bunlara erişmek için gereken bağlantı dizelerini ayarlama hakkında bilgi:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Araçlar ve sitenizi dağıtmak için ayarlar hakkında bilgi. (E-postadan Cytanium de burada atlanmıştır WebMatrix, bahseder.)

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>.NET Framework sürümünü ayarlama

Cytanium Hoş Geldiniz e-posta bağlantısını .NET Framework sürümünü değiştirmek yönergeler içerir. Bu yönergeler, bu Cytanium Denetim Masası'ndan yapılabilir açıklanmaktadır. Farklı Denetim Masası siteler diğer sağlayıcılar yok veya farklı bir yolla bunu size isteyin.

Denetim Masası URL'sine gidin. Kullanıcı adı ve parolayla oturum açtıktan sonra Denetim Masası'nı görürsünüz.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

İçinde **barındırma alanları** kutusunda işaretçiyi Web simgenin üzerinde tutun ve seçin **Web siteleri** menüsünde.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

İçinde **Web siteleri** kutusunun **contosouniversity.com** (hesabını oluştururken kullandığınız site adı).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

İçinde **Web sitesi özellikleri** kutusunda **uzantıları** sekmesi.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

ASP.NET tarafından değiştirme **2.0 tümleşik ardışık** için **4.0 (tümleşik ardışık)** ve ardından **güncelleştirme**.

## <a name="publishing-to-the-hosting-provider"></a>Barındırma sağlayıcısı yayımlama

Hoş Geldiniz e-posta barındırma sağlayıcısına ait proje yayımlamak için gereken tüm ayarları içerir ve bu bilgileri bir yayımlama profiline el ile girebilirsiniz. Ancak sağlayıcısına dağıtımını yapılandırmak için bir daha kolay ve daha az hata yapmaya açık yöntemi kullanacaksınız: indirme bir *.publishsettings* dosyası ve bir yayımlama profiline aktarma.

Tarayıcınızda Cytanium Denetim Masası'na gidin ve seçin **Web** seçip **Web siteleri.**

![Denetim Masası'nı seçerek Web siteleri](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Seçin **contosouniversity.com** web sitesi.

![Denetim Masası contosouniversity.com seçme](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Seçin **Web'de Yayımlama** sekmesi.

![Denetim Masası Web'de yayımlama sekmesi](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Web kullanıcı adı ve parola girerek yayımlama için kullanılacak kimlik bilgilerini oluşturun. Denetim Masası'na oturum açmak için kullandığınız kimlik bilgilerini girebilirsiniz. Ardından **etkinleştirme**.

![Denetim Masası'yayımlama kimlik bilgileri oluştur](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Tıklayın **bu web sitesi için yayımlama profilini indirin**.

![Yayımlama profili Denetim Masası indir](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Dosyasını açmanız veya kaydetmeniz istendiğinde, dosyayı kaydedin.

![Yayımlama profili dosyasını Kaydet](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

İçinde **Çözüm Gezgini** Visual Studio'da ContosoUniversity projeye sağ tıklayıp seçin **Yayımla**. **Web'i Yayımla** iletişim kutusu açılır üzerinde **Önizleme** ile sekmesindeki **Test** profili, kullanılan son profil olduğundan seçilmedi.

Seçin **profili** sekmesine ve ardından **alma**.

![Yayımla Web Sihirbazı İçeri Aktar düğmesi](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

İçinde **alma yayımlama ayarları** iletişim kutusunda *.publishsettings* dosyasını indirdiğiniz ve tıklayın **açık**. Sihirbaz ile doldurulmuş alanların tümünü bağlantı sekmesine ilerler.

![Web Sihirbazı bağlantı sekmesi yayımlama](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

.Publishsettings dosyasını hedef URL kutusuna planlı kalıcı site URL'sini geçirir, ancak henüz bu etki alanı satın yapmadıysanız, değeri geçici URL ile değiştirin. Bu örnekte, URL  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Bu kutu yalnızca amacı, tarayıcı için otomatik olarak sonra başarılı dağıtımdan sonra açılır hangi URL belirtmektir. Bunu boş bırakırsanız, yalnızca sonucu tarayıcı dağıtımdan sonra otomatik olarak başlatılmaz olduğu.

Tıklayın **bağlantıyı doğrula** ayarlarının doğru olduğundan ve sunucuya bağlanabilirsiniz doğrulayın. Daha önce bahsettiğim gibi yeşil bir onay işareti, bağlantının başarılı olduğunu doğrular.

Bağlantıyı Doğrula'ye tıkladığınızda görebileceğiniz bir **sertifika hatası** iletişim kutusu. Bunu yaparsanız, sunucu adının beklediğiniz olduğunu doğrulayın. İse, seçin **bu sertifikayı gelecekteki Visual Studio oturumları için Kaydet** tıklatıp **kabul**. (Bu hata bir barındırma sağlayıcısına dağıtım yaptığınız URL için bir SSL sertifikası satın alma maliyetinden kaçının seçmiştir anlamına gelir. Geçerli bir sertifika kullanarak güvenli bir bağlantı oluşturmak isterseniz, barındırma sağlayıcınızla bağlantı kurun.)

![Sertifika hatası](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

**İleri**'ye tıklayın.

İçinde **veritabanları** bölümünü **ayarları** sekmesinde, aynı girin Test için girdiğiniz değerleri yayımlama profili. Aşağı açılan listesinde, gereken bağlantı dizelerine bulabilirsiniz.

- Bağlantı dizesi kutusunda **SchoolContext,** seçin `Data Source=|DataDirectory|School-Prod.sdf`
- Altında **SchoolContext**seçin **uygulamak Code First Migrations**.
- Bağlantı dizesi kutusunda **DefaultConnection**seçin `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Altında **DefaultConnection**, bırakın **veritabanını Güncelleştir** temizlenir.

![Web Sihirbazı Ayarlar sekmesinde Yayımla](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

**İleri**'ye tıklayın.

İçinde **Önizleme** sekmesinde **önizlemeyi Başlat** kopyalanacak dosyaların bir listesini görmek için. Yerel bilgisayarda IIS dağıtıldığında, daha önce gördüğünüzle aynı listesini görürsünüz.

Yayımlamadan önce Web.Production.config dönüştürme dosyanızı uygulanacak şekilde profilinin adını değiştirin. Seçin **profili** sekmesine **Spravovat Profily**.

![Web Sihirbazı Spravovat Profily yayımlama](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

İçinde **Web yayımlama profillerini Düzenle** iletişim kutusunda, üretim profili seçin, **Yeniden Adlandır**ve üretim için profil adını değiştirin. Ardından **Kapat**.

![Web yayımlama profillerini iletişim kutusunu Düzenle](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Tıklayın **yayımlama**.

Uygulama barındırma sağlayıcısının yayınlanır. Sonucu gösterir **çıkış** penceresi.

![Çıkış penceresi dağıtımdan sonra](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Tarayıcı için girdiğiniz URL'yi otomatik olarak açılır **hedef URL** kutusuna **bağlantı** sekmesinde **Web'i Yayımla** Sihirbazı. Artık yok "(Test)" haricinde sitesi Visual Studio'da çalıştırdığınızda aynı giriş sayfasına bakın veya başlık çubuğundaki "(Geliştirme)" ortam göstergesi. Gösterir ortam göstergesi *Web.config* dönüştürme çalışılan doğru.

> [!NOTE]
> "(Test)" başlığı görmeye devam ediyorsanız Sil *obj* klasöründen ContosoUniversity proje ve yeniden dağıtın. Üretim profili kullanıyor olsanız da yazılımın yayın öncesi sürümlerinde, daha önce uygulanan dönüşümü dosyası (Web.Test.config) yeniden uygulandığından.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Veritabanı erişimi neden olan bir sayfa çalıştırmadan önce Elmah oluşan hataları günlüğe mümkün olacağını emin olun.

## <a name="setting-folder-permissions-for-elmah"></a>Elmah klasör izinlerini ayarlama

Bu serideki önceki öğreticiden unutmayın gibi uygulama, uygulama klasöründe Elmah hata günlük dosyalarını depoladığı için yazma izinlerine sahip olduğunu emin olmanız gerekir. IIS için bilgisayarınızda yerel olarak dağıtıldığında, söz konusu izinleri el ile ayarlayın. Bu bölümde Cytanium izinleri ayarlama konusunda görürsünüz. (Bazı barındırma sağlayıcıları, bunu yapmak mümkün kılmayan; yazma izinlerine sahip bir veya daha fazla önceden tanımlanmış klasörlere sağlamıyor olabilir. Bu durumda, belirtilen klasörlere kullanmak için uygulamanızı değiştirmeniz gerekir.)

Cytanium Denetim Masası'ndaki klasör izinlerini ayarlayabilirsiniz. Seçin ve URL denetimine panel **Dosya Yöneticisi**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

İçinde **Dosya Yöneticisi** kutusunda **contosouniversity.com** ardından **wwwrooot** uygulamanın kök klasörü görmek için. Asma kilit simgesini tıklayın **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

İçinde **dosya**/**klasör izinleri** penceresinde **okuma** ve **yazma** onay kutularını  **contosouniversity.com** tıklatıp **izinlere**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Elmah yazma erişimi olduğundan emin olun *Elmah* bir hataya yol açan ve ardından Elmah hata raporunu görüntüleyerek klasörü. Gibi geçersiz bir URL isteği *Studentsxxx.aspx*. Önce gördüğünüz gibi *GenericErrorPage.aspx* sayfası. Tıklayın **Log Out** bağlantısını ve ardından çalıştırın *Elmah.axd*. Size **oturum** ilk olarak, doğrulama sayfasında *Web.config* dönüştürme Elmah yetkilendirme başarıyla eklendi. Oturum açtıktan sonra yalnızca neden olduğu hata gösteren bir rapor görürsünüz.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Üretim ortamında test etme

Çalıştırma **Öğrenciler** sayfası. Uygulama, veritabanını oluşturmak için Code First Migrations tetikler ilk kez için School veritabanını erişmeye çalışır. Sayfanın bir biçimde gecikme süresi dolunca görüntülediğinde, öğrenci olduğunu gösterir.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Çalıştırma **Eğitmenler** çekirdek verileri veritabanına başarıyla Eğitmen veri eklenen doğrulamak için sayfa.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Veritabanı güncelleştirmelerini üretim ortamında çalışır, ancak genellikle test verileri, üretim veritabanına girmek istemiyorsanız doğrulamak istediğiniz test ortamında olduğu gibi. Bu öğretici için testte yaptığınız aynı yöntemi kullanacaksınız. Ancak, veritabanı doğrulayan bir yöntem bulmak isteyebilirsiniz, gerçek bir uygulamada güncelleştirmeleri üretim veritabanına test verilerini oluşturmaksızın başarılı olur. Bazı uygulamalarda, bir şey ekleyin ve ardından silmek mümkün olabilir.

Bir öğrenci eklemek ve ardından, girdiğiniz verileri görüntülemek **Öğrenciler** sayfasını kullanarak veritabanındaki verileri güncelleştirebilirsiniz doğrulayın.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Yetkilendirme kuralları seçerek düzgün çalıştığını doğrulamak **güncelleştirme KREDİLERİ** gelen **kursları** menüsü. **Oturum** sayfası görüntülenir. Yönetici hesabı kimlik bilgilerinizi girin, tıklayın **oturum**ve **güncelleştirme KREDİLERİ** sayfası görüntülenir.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Oturum açma başarılı olursa **güncelleştirme KREDİLERİ** sayfası görüntülenir. Bu, ASP.NET üyelik veritabanına (tek yönetici hesabıyla) başarıyla dağıtıldığını gösterir.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Artık başarıyla dağıtılan ve siteniz test ve genel Internet üzerinden kullanılabilir.

## <a name="creating-a-more-reliable-test-environment"></a>Daha güvenilir bir Test Ortamı Oluşturma

İçinde anlatıldığı gibi [Test ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğreticide en güvenilir test ortamında üretim hesabı gibi sahip barındırma sağlayıcısında ikinci bir hesabı olacaktır. Bu, ikinci bir barındırma hesap için kaydolmanız gerekir bu yana, test ortamı olarak yerel IIS kullanmaktan daha pahalı olabilir. Ancak üretim sitesini hataları veya kesintiler engelliyorsa, maliyeti karşılıyor mu karar verebilirsiniz.

Çoğu oluşturmak ve dağıtmak için bir test hesabı işleminin ne daha önce üretim ortamına dağıtmak için yaptığınız için benzer:

- Oluşturma bir *Web.config* dönüşüm dosyası.
- Barındırma sağlayıcısında bir hesap oluşturun.
- Yeni bir yayımlama profili oluşturun ve test hesabına dağıtın.

### <a name="preventing-public-access-to-the-test-site"></a>Test siteye genel erişimi engelliyor

Önemli bir konu için test hesabının Internet'te Canlı olacaktır, ancak bunu kullanmak için genel istemediğiniz ' dir. Siteye özel olarak saklamak için bir veya daha fazla aşağıdaki yöntemlerden birini kullanabilirsiniz:

- Test etmek için kullandığınız IP adreslerinden gelen test sitesine erişime izin veren güvenlik duvarı kurallarını ayarlamak için barındırma sağlayıcısına başvurun.
- URL, genel site URL'SİNİN benzer olmaması gizlenebilir.
- Kullanım bir *robots.txt* arama motorları test site ve rapor bağlantıları için arama sonuçlarında gezinme değil emin olmak için dosya.

Bu yöntemlerin ilki olan açıkça en güvenli ayardır, ancak yordamı için her bir barındırma sağlayıcısına özeldir ve Bu öğreticide yer almaz. Test hesap URL'sine göz atmak yalnızca IP adreslerine izin verecek şekilde barındırma sağlayıcınızla düzenlemek, teorik olarak, gezinme arama motorları hakkında endişelenmeniz gerekmez. Ancak bu durumda bile dağıtma bir *robots.txt* dosyasıdır fikir yedek olarak bu güvenlik duvarı kuralı her zamankinden yanlışlıkla devre dışı durumda.

*Robots.txt* dosya proje klasörünüzdeki gider ve içine aşağıdaki metni olması gerekir:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

`User-agent` Satırı belirten kurallar dosyasındaki tüm arama motoru web gezginleri için (robotlar), geçerli arama motorları ve `Disallow` satırı yok sayfaları sitesinde gezinilmesi istendiğinde, belirtir.

Arama motorları, üretim sitenizi üretim dağıtımından bu dosyayı hariç tutacak gerek katalog muhtemelen istersiniz. Bunu yapmak için bkz: **miyim belirli dosyaları veya klasörleri dağıtım kapsamından çıkarabilirsiniz?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Yalnızca üretim yayımlamak için profil dışlama belirttiğinizden emin olun.

İkinci bir barındırma hesabı oluşturma, gerekli değildir, ancak ek harcama olabilir bir test ortamı ile çalışmak için bir yaklaşımdır. Aşağıdaki öğreticilerde, IIS'yi, test ortamı olarak kullanmayı devam edeceğiz.

Sonraki öğreticide uygulama kodunu güncelleştirmesi ve değişikliğinizi test ve üretim ortamlarına dağıtın.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [İleri](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
