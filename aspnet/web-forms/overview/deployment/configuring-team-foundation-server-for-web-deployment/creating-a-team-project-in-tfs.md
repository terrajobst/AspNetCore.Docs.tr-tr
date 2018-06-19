---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: "' De TFS takım projesi oluşturma | Microsoft Docs"
author: jrjlee
description: Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880433"
---
<a name="creating-a-team-project-in-tfs"></a>' De TFS takım projesi oluşturma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

## <a name="task-overview"></a>Görev genel bakış

Sağlama ve TFS'de yeni bir takım projesi kullanmak için üst düzey adımları tamamlamanız gerekir:

- Yeni takım projesi oluşturacak kullanıcıya izinleri verin.
- Takım projesi oluşturun.
- Projede çalışan ekip üyelerine izinleri verin.
- Bazı içeriği denetleyin.

Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir ve onu kullanıcılar ve büyük olasılıkla her yordam için sorumlu iş rolleri tanımlar. Kuruluşunuzun yapısına bağlı olarak, bu görevlerin her birini farklı bir kişiye sorumluluğunda olabileceğini unutmayın.

Görevleri ve bu konudaki yönergeler artık yüklenmiş ve TFS yapılandırılmış olduğunu ve bir takım projesi koleksiyonu yapılandırma işleminin bir parçası olarak oluşturduğunuz varsayalım. Bu varsayımları hakkında daha fazla bilgi ve senaryo hakkında daha fazla genel arka plan bilgileri için bkz: [yapı TFS sunucusu için Web dağıtımı yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Takım projesi Oluşturucusu izinleri

Yeni takım projesi oluşturmak için bu izinler gerekir:

- Sahip olmanız gerekir **yeni projeler oluşturma** TFS uygulama katmanında izni. Genellikle kullanıcılara ekleyerek bu izni vermek **proje koleksiyonu yöneticileri** TFS grubu. **Team Foundation Yöneticileri** genel bir grup da bu izni içerir.
- TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu içinde yeni ekip siteleri oluşturma iznine sahip olmalıdır. Genellikle bu izne sahip bir SharePoint grubuna kullanıcı ekleme verdiğiniz **tam denetim** hakları SharePoint site koleksiyonu.
- SQL Server Reporting Services özelliklerini kullanıyorsanız, bir üyesi olmalıdır **Team Foundation İçerik Yöneticisi** Raporlama Hizmetleri'nde rolü.

### <a name="who-performs-these-procedures"></a>Kim, bu yordamları gerçekleştirir mi?

Genellikle, kişi veya TFS dağıtım yöneten grubu da bu yordamları gerçekleştirir.

Bu düzeyde ayrıcalıklı bir izin kümesi olduğundan, yeni takım projelerine kullanıcılar küçük bir kısmı TFS dağıtımı yönetmek için sorumluluk ile genellikle oluşturulur. Geliştiriciler genellikle yeni takım projeleri oluşturmak için gerekli izinleri verilecektir değil.

### <a name="grant-permissions-in-tfs"></a>TFS izinlerini verin

Yeni takım projeleri oluşturmak bir kullanıcı etkinleştirmek istiyorsanız, ilk üst düzey kullanıcı eklemek için bir görevdir **proje koleksiyonu yöneticileri** takım projesi koleksiyonu için Grup.

**Proje Koleksiyonu Yöneticileri grubuna kullanıcı eklemek için**

1. TFS sunucusunda üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft Team Foundation Server 2010**ve ardından **Team Foundation Yönetim Konsolu**.
2. Gezinti ağaç görünümünde genişletin **uygulama katmanı**ve ardından **takım projesi koleksiyonları**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. İçinde **takım projesi koleksiyonları** takım projesi koleksiyonu yönetmek istediğiniz bölmesinde seçin.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Üzerinde **genel** sekmesini tıklatın, **grup üyeliği**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. İçinde **genel grup** iletişim kutusunda **proje koleksiyonu yöneticileri** grup ve ardından **özellikleri**.
6. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcısı veya grubu**ve ardından **Ekle**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna yeni takım projeleri, oluşturabilecek istediğiniz kullanıcının kullanıcı adını tıklatın **Adları Denetle**ve ardından **Tamam** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklatın **Tamam**.
9. İçinde **genel grup** iletişim kutusu, tıklatın **Kapat**.

### <a name="grant-permissions-in-sharepoint-services"></a>SharePoint Services izinleri

Ardından, yeni ekip siteleri, TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu oluşturmak için kullanıcı izni vermeniz gerekir.

**SharePoint site koleksiyonu üzerinde tam denetim izinleri vermek için**

1. Team Foundation Server Yönetim Konsolu'nda üzerinde **takım projesi koleksiyonları** sayfasında, yönetmek istediğiniz takım projesi koleksiyonu seçin.
2. Üzerinde **SharePoint sitesi** sekmesinde, değeri Not **varsayılan güncel Site konumu** URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Internet Explorer'ı açın ve 2. adımda not ettiğiniz URL'ye gidin.

    > [!NOTE]
    > Windows için takım projesi koleksiyonunu oluşturan kullanıcı oturum açtınız değil, devam etmek için SharePoint için bu kullanıcı olarak oturum açmak gerekir.
4. Üzerinde **Site eylemleri** menüsünde tıklatın **Site Ayarları**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Üzerinde **Site Ayarları** sayfasında **kullanıcılar ve izinler**, tıklatın **kişiler ve gruplar**.
6. Sol gezinti panelinde tıklatın **grupları**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Üzerinde **kişiler ve gruplar: tüm grupları** sayfasında, **Gruplar Kur bu Site için**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Alabileceğiniz bir <strong>HTTP 404 Bulunamadı</strong> çift HTTP kodlama hata nedeniyle hata. Bu gerçekleşirse, URL bu ile değiştirin:   
   > [<em>site koleksiyonu URL'si</em>] /\_layouts/permsetup.aspx  
   > Örneğin:  
   > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. Üzerinde **Gruplar Kur bu Site için** sayfasında, takım projelerine oluşturan kullanıcı ekleme **sahipleri** grup ve ardından **Tamam**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Bir takım projesi koleksiyonu içinde yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonu için yönetici izinleri ayarlama](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Yeni takım projesi oluşturma ve kullanıcıları ekleme

Gerekli izinlere sahip olduğunda, kullanabileceğiniz **Takım Gezgini** yeni takım projesi oluşturmak için Visual Studio 2010 penceresinde. Bu yaklaşım, gerekli tüm bilgileri toplar ve TFS, SharePoint ve SQL Server Reporting Services gerekli görevleri gerçekleştiren bir sihirbaz sağlar. Yeni takım projesi eklemek ve içeriği değiştirmek bunları etkinleştirmek için geliştirici ekibinin üyelerine, izinleri gerekecektir.

### <a name="who-performs-these-procedures"></a>Kim, bu yordamları gerçekleştirir mi?

Genellikle bir TFS Yöneticisi veya bir geliştirici Ekip Lideri bu yordamları gerçekleştirir.

### <a name="create-a-new-team-project"></a>Yeni takım projesi oluşturma

Sonraki yordam TFS 2010'da yeni bir takım projesi oluşturmayı açıklar.

**Yeni takım projesi oluşturmak için**

1. Üzerinde **Başlat** menüsündeki **tüm programlar**, tıklatın **Microsoft Visual Studio 2010**, sağ **Microsoft Visual Studio 2010**, ve ardından **yönetici olarak çalıştır**.

    > [!NOTE]
    > Yeni Takım Projesi Sihirbazı, bir yönetici olarak Visual Studio 2010 çalıştırmazsanız son adımda başarısız olur.
2. Varsa **kullanıcı hesabı denetimi** iletişim kutusu görüntülenirse, tıklatın **Evet**.
3. Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.

    > [!NOTE]
    > TFS sunucusu bağlantısı zaten yapılandırdıysanız, 4-7 adımları atlayabilirsiniz.
4. İçinde **takım projesine bağlantı** iletişim kutusu, tıklatın **sunucuları**.
5. İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Ekle**.
6. İçinde **Team Foundation Server Ekle** iletişim kutusu, TFS örneğinizi ayrıntılarını sağlayın ve ardından **Tamam**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Kapat**.
8. İçinde **takım projesine Bağlan** takım seçmek için bağlanmak istediğiniz TFS örnek proje ekleyin ve ardından istediğiniz koleksiyonu iletişim kutusunda **Bağlan**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. İçinde **Takım Gezgini** penceresinde, takım projesi koleksiyonu ve ardından sağ **yeni takım projesi**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. İçinde **yeni takım projesi** iletişim kutusu, bir ad ve takım projesi için bir açıklama girin ve ardından **sonraki**.

    > [!NOTE]
    > Takım projenizin boşluk içeriyorsa, çıktı yolundan paketlerini dağıtmak için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanmak için geldiğinizde bazı sorunlar karşılaşıyor. Yolunda boşluk Web dağıtımı komutları çalıştırmak çok daha zor yapabilirsiniz.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Üzerinde **işlem şablonu seçin** sayfasında, geliştirme sürecini yönetmenize ve ardından kullanmak istediğiniz işlem şablonunu seçin **sonraki**.

    > [!NOTE]
    > TFS işlem şablonları hakkında daha fazla bilgi için bkz: [işlem şablonları ve araçları](https://msdn.microsoft.com/vstudio/aa718795).
12. Üzerinde **ekip sitesi ayarları** sayfasında, varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.
13. Bu ayar oluşturur veya tanımlar, TFS takım projesi ile ilişkilendirilmiş bir SharePoint ekip sitesi. Geliştirme ekibiniz, bu site, belgeleri yönetmek, tartışma iş parçacıklarında katılmak, wiki sayfaları oluşturmak ve kodu ilgili olmayan diğer çeşitli görevleri gerçekleştirmek için kullanabilirsiniz. Daha fazla bilgi için bkz: [SharePoint ürünleri arasındaki etkileşimler ve Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Üzerinde **kaynak denetim ayarları belirtmek** sayfasında, varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.
15. Bu ayar tanımlayan veya konumu içeriğiniz için kök klasör olarak hareket edecek TFS klasör hiyerarşisi oluşturur.
16. Üzerinde **takım projesi ayarlarını onaylama** sayfasında, **son**.
17. Yeni takım projesi başarıyla oluşturulduğunda, üzerinde **takım projesi yaratıldı** sayfasında, **Kapat**.

### <a name="add-users-to-a-team-project"></a>Bir takım projesine kullanıcılar ekleme

Yeni takım projesi oluşturduğunuza göre bunları ekleme ve içerik işbirliği başlatmak etkinleştirmek için kullanıcılar için izinler verebilirsiniz.

**Bir takım projesine kullanıcıları eklemek için**

1. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesine sağ tıklayın, fareyle **takım projesi ayarları**ve ardından **grup üyeliği**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Bir kullanıcı eklemek, değiştirmek ve kaynak denetimi altında kod kaldırmak etkinleştirmek için ondan için eklemeniz gerekir **katkıda bulunanlar** grubu.
3. İçinde **Proje grupları** iletişim kutusunda **katkıda bulunanlar** grup ve ardından **özellikleri**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcısı veya grubu**ve ardından **Ekle**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna takım projesine eklemek istediğiniz kullanıcının kullanıcı adını tıklatın **Adları Denetle**ve ardından **Tamam**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklatın **Tamam**.
7. İçinde **Proje grupları** iletişim kutusu, tıklatın **Kapat**.

## <a name="conclusion"></a>Sonuç

Bu noktada, yeni takım projesi kullanıma hazır ve Geliştirme ekibiniz içerik ekleme ve geliştirme sürecinin üzerinde işbirliği başlayabilirsiniz.

Sonraki konuyu [ekleme içerik kaynak denetimine](adding-content-to-source-control.md), içerik kaynak denetimine eklemeyi açıklar.

## <a name="further-reading"></a>Daha Fazla Bilgi

TFS'de takım projeleri oluşturma hakkında daha geniş yönergeler için bkz [takım projesi oluşturma](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Bir takım projesi koleksiyonu içinde yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonu için yönetici izinleri ayarlama](https://msdn.microsoft.com/library/dd547204.aspx). Takım projelerine kullanıcılar ekleme hakkında daha fazla bilgi için bkz: [takım projelerine kullanıcılar ekleme](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Önceki](configuring-team-foundation-server-for-web-deployment.md)
> [sonraki](adding-content-to-source-control.md)
