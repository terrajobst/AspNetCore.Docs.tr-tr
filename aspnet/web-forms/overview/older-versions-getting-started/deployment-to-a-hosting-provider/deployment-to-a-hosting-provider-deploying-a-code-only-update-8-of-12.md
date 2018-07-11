---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Code-Only güncelleştirmesi - 12 8 dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16a9e48034536964d526717ecde2a9b813818963
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816090"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: 12 8 - Code-Only güncelleştirmesi dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

İlk dağıtımdan sonra Bakım ve web sitenizi geliştirme iş devam eder ve uzun süre önce bir güncelleştirme dağıtmak istersiniz. Bu öğretici, uygulama kodunuz için bir güncelleştirme dağıtma işlemini gösterir. Bu güncelleştirme, veritabanı değişikliği gerektirmez; farklı bir veritabanı değişiklik sonraki öğreticide dağıtma hakkında nedir görürsünüz.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Bir kod değişikliği yapmadan

Basit bir örnek, bir güncelleştirme uygulamanıza, için ekleyeceksiniz **Eğitmenler** seçili Eğitmenler tarafından verilen derslerimiz Listesi sayfasında.

Çalıştırırsanız **Eğitmenler** sayfasında seçeneğinde olduğunu **seçin** bağlantıları kılavuzunda, ancak yapma dışında bir satır arka plan Aç gri uygulamayın.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Şimdi ne zaman çalışan kod ekleyeceksiniz **seçin** bağlantı tıklatıldığında ve seçili Eğitmenler tarafından verilen derslerimiz listesini görüntüler.

İçinde *Instructors.aspx*, aşağıdaki işaretlemeyi ekleyin hemen sonra **ErrorMessageLabel** `Label` denetimi:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Sayfayı çalıştırın ve bir eğitmen seçin. Bu eğitmen tarafından verilen derslerimiz listesini görürsünüz.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Test ortamı için kod güncelleştirmesi dağıtma

Test ortamına dağıtma ibarettir çalıştırmanın tek tıklamayla yayımlama yeniden olur. Bu işlem daha hızlı hale getirmek için kullanabileceğiniz **Web tek tık Yayımla** araç çubuğu.

İçinde **görünümü** menüsünde seçin **araç çubukları** seçip **Web tek tık Yayımla**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

İçinde **Çözüm Gezgini**, ContosoUniversity projeyi seçin.

**Web tek tık Yayımla** araç seçin **Test** yayımlama profili ve ardından **Web'i Yayımla** (sağa ve sola işaret eden oklarla simgesiyle).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

Visual Studio güncelleştirilmiş uygulamayı dağıtır ve tarayıcı giriş sayfasına otomatik olarak açılır. Eğitmenler çalıştırırsanız ve güncelleştirme başarıyla dağıtıldığını doğrulamak için bir eğitmen seçin.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalde ayrıca gerileme sınaması işlemi (diğer bir deyişle, yeni değişiklik herhangi bir mevcut işlevsellik yarıda yaramadı emin olmak için sitenin geri kalanını test). Ancak bu öğretici için bu adımı atlayın ve güncelleştirmeyi üretime dağıtmak için devam edin.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Üretim için ilk veritabanı durumu çözümünüzün yeniden dağıtımını önleme

Gerçek bir uygulama kullanıcıların ilk dağıtımınızdan sonra üretim sitenizi etkileşim ve veritabanlarını Canlı verilerle doldurulur. Bu nedenle, tüm canlı verileri silme, başlangıç durumunda üyelik veritabanında yeniden istemezsiniz. SQL Server Compact veritabanları dosyalarında olduğundan *uygulama\_veri* klasörüne sahip dosyalar, bu nedenle dağıtım ayarlarını değiştirerek Bunu önlemek *uygulama\_veri* klasörü dağıtılan değildir.

Açık **proje özellikleri** ContosoUniversity proje ve Seç penceresi **Paketle/Yayımla Web** sekmesi. Emin olun **yapılandırma** açılan kutu ya da sahip **etkin (sürüm)** veya **yayın** seçili seçin **uygulamadandosyalarıdışlama\_Veri klasörü**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

Gelecekte bir hata ayıklama derlemesi dağıtmak karar durumda, hata ayıklama derleme yapılandırması için aynı değişikliği yapmanız için iyi bir fikirdir: değiştirmek **yapılandırma** için **hata ayıklama** seçip **Dışla Uygulama dosyaları\_veri klasörü**.

Kaydet ve Kapat **Paketle/Yayımla Web** sekmesi.

> [!NOTE] 
> 
> [!IMPORTANT]
> Yoksa emin **hedefteki ek dosyaları Kaldır** , yayımlama profillerine seçildi. Bu seçeneği seçerseniz, dağıtım işlemi uygulamada olan veritabanlarını silecek\_sitesi dağıtıldı ve bu verileri, uygulama silinecek\_veri klasörü kendisi.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Kullanıcı erişimini üretim sitesini güncelleştirme sırasında engelliyor.

Dağıtım yapıyorsanız artık bir tek sayfalı basit bir değişiklik değişikliktir. Ancak bazen daha büyük değişiklikler dağıttığınızda ve dağıtım tamamlanmadan önce bir kullanıcı bir sayfa istediğinde bu durumda site garip davranabilir. Bunu önlemek için kullanabileceğiniz bir *uygulama\_offline.htm* dosya. Adlı bir dosya yerleştirdiğinizde *uygulama\_offline.htm* uygulamanızın kök klasöründe, IIS uygulamanızı çalıştırmak yerine bu dosyayı otomatik olarak görüntüler. Dağıtım sırasında erişimi engellemek için bu nedenle *uygulama\_offline.htm* kök klasöründe dağıtım işlemini çalıştırın ve Kaldır'ı *uygulama\_offline.htm*.

İçinde **Çözüm Gezgini**, çözüm (değil projelerden birine) sağ tıklayıp **yeni çözüm klasörü**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Klasör adı *SolutionFiles*.

Yeni klasöre adlı bir HTML sayfası oluşturma *uygulama\_offline.htm*. Aşağıdaki biçimlendirme mevcut içeriğini değiştirin:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Kopyalayabilirsiniz *uygulama\_offline.htm* site bir FTP bağlantısı kullanarak bir dosyaya veya **Dosya Yöneticisi** barındırma sağlayıcısının Denetim Masası'ndaki yardımcı programı. Bu öğretici için kullanacağınız **Dosya Yöneticisi**.

Denetim Masası'nı açın ve seçin **Dosya Yöneticisi** yaptığınız gibi [üretim ortamına dağıtma](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) öğretici. Seçin **contosouniversity.com** ardından **wwwroot** uygulamanızın kök klasörüne almak ve ardından **karşıya**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

İçinde **dosyasını karşıya yükle** iletişim kutusunda *uygulama\_offline.htm* dosya ve ardından **karşıya**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Sitenizin URL'sine gidin. Gördüğünüz *uygulama\_offline.htm* sayfası, giriş sayfanızın yerine artık görüntülenir.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Artık üretime dağıtmaya hazır olursunuz.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Kod güncelleştirmesi üretim ortamına dağıtma

İçinde **Web tek tık Yayımla** araç seçin **üretim** yayımlama profili ve ardından **Web'i Yayımla**.

Visual Studio, güncelleştirilmiş uygulamayı dağıtır ve tarayıcı sitenin ana sayfasını açar. *Uygulama\_offline.htm* dosyası görüntülenir. Başarılı dağıtımı doğrulamak için test edebilmek için önce kaldırmanız gerekir *uygulama\_offline.htm* dosya.

Geri dönüp **Dosya Yöneticisi** Denetim Masası'nda uygulama. Seçin **contosouniversity.com** ve **wwwroot**seçin **uygulama\_offline.htm**ve ardından **Sil**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

Tarayıcıda genel sitede Eğitmenler sayfasını açın ve bir eğitmen güncelleştirme başarıyla dağıtıldığını doğrulamak için seçin.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Artık veritabanı değişikliği ilgili olmayan bir uygulama güncelleştirmesi dağıttınız. Sonraki öğreticiye nasıl veritabanı değişikliği dağıtılacağı gösterir.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [İleri](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
