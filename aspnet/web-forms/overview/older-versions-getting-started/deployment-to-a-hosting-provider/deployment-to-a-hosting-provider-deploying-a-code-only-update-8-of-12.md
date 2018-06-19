---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: Code-Only güncelleştirmesi - 8 12 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886920"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: Code-Only güncelleştirmesi - 8 12 dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

İlk dağıtımdan sonra Bakımı ve web sitenizi geliştirme iş devam eder ve uzun süre önce bir güncelleştirme dağıtmak istersiniz. Bu öğretici bir güncelleştirme uygulama kodunuz dağıtma işlemini gösterir. Bu güncelleştirme, bir veritabanı değişiklik gerektirmez; sonraki öğreticide veritabanı değişikliği dağıtma hakkında farklı nedir görürsünüz.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Bir kod değişikliği yapma

İçin ekleyeceksiniz basit bir örnek bir güncelleştirme olarak uygulamanıza, **Eğitmen** tarafından seçilen Eğitmen öğrettin kurslar listesini sayfa.

Çalıştırırsanız **Eğitmen** sayfasında fark olduğunu **seçin** bağlantılar kılavuzunda, ancak yapma dışında her şey satır arka plan Aç gri yapmayın.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Ne zaman çalışan kod ekleyeceksiniz artık **seçin** bağlantı tıklatıldığında ve seçili Eğitmen tarafından öğrettin kurslar listesini görüntüler.

İçinde *Instructors.aspx*, aşağıdaki biçimlendirmeleri eklemek hemen sonra **ErrorMessageLabel** `Label` denetimi:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Sayfayı çalıştırın ve bir eğitmen seçin. Bu Eğitmeni tarafından öğrettin kurslar listesini bakın.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Kod güncelleştirme Test ortamına dağıtma

Test ortamına dağıtma basit sağlasa da, çalışan tek tıklamayla yayımlama yeniden olur. Bu işlemi daha hızlı hale getirmek için kullanabileceğiniz **Web tek tık Yayımla** araç.

İçinde **Görünüm** menüsünde seçin **araç çubukları** ve ardından **Web tek tık Yayımla**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

İçinde **Çözüm Gezgini**, ContosoUniversity projesini seçin.

**Web tek tık Yayımla** araç seçin **Test** yayımlama profili ve ardından **Web'i Yayımla** (sağa ve sola işaret eden oklarla simgesiyle).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfasına otomatik olarak açar. Eğitmen sayfayı çalıştırın ve güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için bir eğitmen seçin.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalde ayrıca regresyon testi yapmak (diğer bir deyişle, yeni değişiklik herhangi bir varolan işlevsellik yarıda kaydetmedi emin olmak için sitenin geri kalanını test). Ancak bu öğretici için bu adımı atlayın ve güncelleştirmeyi üretime dağıtmaya devam edin.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Üretim için ilk veritabanı durumu çözümünüzün yeniden dağıtımını önleme

Gerçek bir uygulamada kullanıcıların ilk dağıtımınızdan sonra üretim sitenizle etkileşim ve veritabanlarını dinamik verilerle doldurulur. Bu nedenle, üyelik veritabanının tüm canlı verileri silme, ilk durumunda dağıtmanız istemezsiniz. SQL Server Compact veritabanları dosyalarında olduğundan *uygulama\_veri* klasörüne sahip, dosyalar için dağıtım ayarlarını değiştirerek Bunu önlemek *uygulama\_veri* klasörü dağıtılan değil.

Açık **proje özelliklerini** ContosoUniversity proje ve Seç penceresi **Paketle/Yayımla Web** sekmesi. Olduğundan emin olun **yapılandırma** açılan kutusu vardır ya da **etkin (sürüm)** veya **sürüm** seçili seçin **uygulamasındandosyalarıdışlayın\_Veri klasörü**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Hata ayıklama derlemesi gelecekte dağıtmaya karar durumda, hata ayıklama yapı yapılandırması için aynı değişikliği yapmak için iyi bir fikirdir: değiştirme **yapılandırma** için **hata ayıklama** ve ardından **Dışla Uygulama dosyaları\_veri klasörü**.

Kaydet ve Kapat **Paketle/Yayımla Web** sekmesi.

> [!NOTE] 
> 
> [!IMPORTANT]
> Yok olduğundan emin olun **hedefteki ek dosyaları Kaldır** Yayımla profillerinizi seçili. Bu seçeneği belirlerseniz, dağıtım işlemi, uygulamanız veritabanlarını silecek\_sitesi dağıtıldı ve bu verileri, uygulama silecek\_veri klasörü kendisi.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Güncelleştirme sırasında üretim siteye kullanıcı erişimini engelleme

Dağıtmakta değişiklik tek bir sayfa için basit bir değişiklik sunulmuştur. Ancak bazen büyük değişiklikler dağıtın ve dağıtım bitmeden önce kullanıcı bir sayfa isterse, bu durumda site garip davranabilir. Bunu önlemek için kullanabileceğiniz bir *uygulama\_offline.htm* dosya. Adlı bir dosya yerleştirdiğinizde *uygulama\_offline.htm* , uygulamanızın kök klasöründe IIS uygulamanızı çalıştırmak yerine bu dosyayı otomatik olarak görüntüler. Dağıtım sırasında erişimi engellemek için yerleştirdiğiniz şekilde *uygulama\_offline.htm* kök klasöründe dağıtım işlemini çalıştırın ve Kaldır'ı *uygulama\_offline.htm*.

İçinde **Çözüm Gezgini**, çözüm (değil projelerden biri) sağ tıklatın ve seçin **yeni çözüm klasörü**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Klasör adı *SolutionFiles*.

Yeni klasör içinde adlı bir HTML sayfası oluşturun *uygulama\_offline.htm*. Varolan içeriğini aşağıdaki biçimlendirme ile değiştirin:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Kopyalayabilirsiniz *uygulama\_offline.htm* site bir FTP bağlantısı kullanarak bir dosyaya veya **Dosya Yöneticisi** yardımcı programı barındırma sağlayıcısının Denetim Masası'nda. Bu öğretici için kullanacağınız **Dosya Yöneticisi**.

Denetim Masası'nı açın ve seçin **Dosya Yöneticisi** de yaptığınız gibi [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Öğreticisi. Seçin **contosouniversity.com** ve ardından **wwwroot** uygulamanızın kök klasörüne alın ve ardından **karşıya**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

İçinde **dosyasını karşıya yükle** iletişim kutusunda *uygulama\_offline.htm* dosya ve ardından **karşıya**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Sitenizin URL'ye gidin. Gördüğünüz *uygulama\_offline.htm* sayfası artık giriş sayfanız yerine görüntülenir.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Şimdi üretime dağıtmaya hazır olursunuz.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Kodu güncelleştirmesi üretim ortamına dağıtma

İçinde **Web tek tık Yayımla** araç seçin **üretim** yayımlama profili ve ardından **Web'i Yayımla**.

Visual Studio güncelleştirilmiş uygulamayı dağıtır ve sitenin giriş sayfasını tarayıcıda açılır. *Uygulama\_offline.htm* dosya görüntülenir. Başarılı dağıtım doğrulamak için test edebilirsiniz önce kaldırmalısınız *uygulama\_offline.htm* dosya.

Geri dönüp **Dosya Yöneticisi** Denetim Masası'nda uygulama. Seçin **contosouniversity.com** ve **wwwroot**seçin **uygulama\_offline.htm**ve ardından **silmek**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Tarayıcı ortak sitede Eğitmen sayfasını açın ve güncelleştirme başarılı bir şekilde dağıtıldı doğrulamak için bir eğitmen seçin.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Şimdi veritabanı değişikliği ilgili olmayan bir uygulama güncelleştirmesi dağıttıktan sonra. Sonraki öğretici veritabanı değişikliği dağıtma gösterir.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [sonraki](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
