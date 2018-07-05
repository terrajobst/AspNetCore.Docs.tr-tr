---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: TFS'de bir takım projesi oluşturma | Microsoft Docs
author: jrjlee
description: Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 2c3b30cac408f47d7d15ae7456f0744219506c85
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397567"
---
<a name="creating-a-team-project-in-tfs"></a>TFS'de bir takım projesi oluşturma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, Team Foundation Server (TFS) 2010 yeni bir takım projesi oluşturmayı açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

## <a name="task-overview"></a>Görev genel bakış

Sağlama ve TFS'de yeni bir takım projesi kullanmak için üst düzey adımları tamamlamanız gerekir:

- Yeni takım projesi oluşturacak kullanıcıya izinleri verin.
- Takım projesi oluşturun.
- Proje üzerinde çalışan takım üyelerine izinler verir.
- Bazı içeriği teslim edin.

Kullanıcılar ve büyük olasılıkla her yordam için sorumlu olan iş roller tanımlayacak ve bu konuda bu yordamları gerçekleştirmek nasıl gösterilir. Kuruluşunuzun yapısına bağlı olarak, bu görevlerin her biri farklı bir kişiyle sorumluluğu olabileceğini unutmayın.

Görevler ve bu konudaki yönergeler yüklemiş ve TFS yapılandırılmış olduğunu ve bir takım projesi koleksiyonu yapılandırma işleminin bir parçası olarak oluşturduğunuz varsayılır. Bu varsayımları hakkında daha fazla bilgi ve senaryo daha genel bilgiler için bkz: [bir TFS derleme sunucusunu Web dağıtımı için yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Takım projesi oluşturucusu için izinler

Yeni takım projesi oluşturmak için bu izinlere ihtiyacınız vardır:

- Olmalıdır **yeni projeler oluştur** TFS uygulama katmanı izni. Genellikle kullanıcılar ekleyerek bu izni verirsiniz **proje koleksiyonu yöneticileri** TFS grubu. **Team Foundation Yöneticileri** genel grubu, bu izni de içerir.
- TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu içindeki yeni ekip siteleri oluşturmak için izniniz olmalıdır. Genellikle kullanıcı bir SharePoint grubuna ekleyerek bu izni verirsiniz **tam denetim** haklarını SharePoint site koleksiyonu.
- SQL Server Reporting Services özelliklerini kullanıyorsanız, bir üyesi olmanız gerekir **Team Foundation İçerik Yöneticisi** Raporlama Hizmetleri'ndeki rol.

### <a name="who-performs-these-procedures"></a>Kimler bu yordamları gerçekleştirir?

Genellikle, kişi veya grup TFS dağıtımını yöneten de bu yordamları gerçekleştirir.

Bu üst düzeyde ayrıcalıklı bir izin kümesi olduğundan, yeni takım projeleri ile TFS dağıtımını yönetme sorumluluğunu kullanıcıların küçük bir kısmı genellikle oluşturulur. Geliştiriciler genellikle yeni takım projeleri oluşturmak için gereken izinleri verilmez.

### <a name="grant-permissions-in-tfs"></a>Tfs'deki izinler

Yeni takım projeleri oluşturmak bir kullanıcı etkinleştirmek istiyorsanız, ilk üst düzey görev kullanıcıya eklemektir **proje koleksiyonu yöneticileri** takım projesi koleksiyonu için Grup.

**Proje Koleksiyonu Yöneticileri grubuna bir kullanıcı eklemek için**

1. TFS sunucusu üzerinde üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft Team Foundation Server 2010**ve ardından **Team Foundation Yönetim Konsolu**.
2. Gezinti ağaç görünümü'nde genişletin **uygulama katmanı**ve ardından **takım projesi koleksiyonları**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. İçinde **takım projesi koleksiyonları** takım projesi koleksiyonunu yönetmek istediğiniz bölmesinde seçin.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. Üzerinde **genel** sekmesinde **grup üyeliği**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. İçinde **genel grup** iletişim kutusunda **proje koleksiyonu yöneticileri** grup ve ardından **özellikleri**.
6. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcı veya grup**ve ardından **Ekle**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna yeni takım projeleri, mümkün olmasını istediğiniz kullanıcının kullanıcı adına tıklayın **Adları Denetle**ve ardından **Tamam** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklayın **Tamam**.
9. İçinde **genel grup** iletişim kutusu, tıklayın **Kapat**.

### <a name="grant-permissions-in-sharepoint-services"></a>SharePoint Services izin ver

Ardından, yeni ekip siteleri TFS takım projesi koleksiyonuna karşılık gelen SharePoint site koleksiyonu oluşturma izni vermeniz gerekir.

**SharePoint site koleksiyonu üzerinde tam denetim izinleri vermek için**

1. Team Foundation Server Yönetim Konsolu içinde üzerinde **takım projesi koleksiyonları** sayfasında, yönetmek istediğiniz takım projesi koleksiyonu seçin.
2. Üzerinde **SharePoint sitesi** sekmesinde, bu değeri Not **varsayılan güncel Site konumu** URL'si.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Internet Explorer'ı açın ve ardından 2. adımda not ettiğiniz URL'sine gidin.

    > [!NOTE]
    > Windows için takım projesi koleksiyonu oluşturan bir kullanıcı oturum değil, SharePoint ile devam etmek için bu kullanıcı olarak oturum açmanız gerekir.
4. Üzerinde **Site eylemleri** menüsünü tıklatın **Site Ayarları**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. Üzerinde **Site Ayarları** sayfasındaki **kullanıcıları ve izinleri**, tıklayın **kişiler ve gruplar**.
6. Sol gezinti panelinde **grupları**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. Üzerinde **kişiler ve gruplar: tüm grupları** sayfasında **grupları bu Site için ayarlanmış yukarı**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Alabileceğiniz bir <strong>HTTP 404 Bulunamadı</strong> çift bir HTTP kodlama hatası nedeniyle hata oluştu. Bu meydana gelirse, URL ile değiştirin:   
   > `[site_collection_URL]/_layouts/permsetup.aspx` Örneğin:  
   > `http://tfs/sites/Fabrikam%20Web%20Projects/_layouts/permsetup.aspx` 
8. Üzerinde **grupları bu Site için ayarlanmış yukarı** sayfasında, takım projelerine oluşturacak kullanıcıyı eklemek **sahipleri** grup ve ardından **Tamam**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Bir takım projesi koleksiyonu içindeki yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonları için Set Administrator Permissions](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Yeni bir takım projesi oluşturabilir ve kullanıcı ekleme

Gerekli izinleri aldıktan sonra kullanabileceğiniz **Takım Gezgini** yeni takım projesi oluşturmak için Visual Studio 2010 penceresi. Bu yaklaşım, gerekli tüm bilgileri toplar ve TFS, SharePoint ve SQL Server Reporting Services gerekli görevleri gerçekleştiren bir sihirbaz sağlar. Yeni takım projesi ekleyin ve içeriğini değiştirmek bunları etkinleştirmek için geliştirici takım üyeleri, için izinlerini gerekecektir.

### <a name="who-performs-these-procedures"></a>Kimler bu yordamları gerçekleştirir?

Genellikle bir TFS Yöneticisi veya bir geliştirici Ekip Lideri, bu yordamları gerçekleştirir.

### <a name="create-a-new-team-project"></a>Yeni takım projesi oluşturma

Sonraki yordamda, TFS 2010'da yeni bir takım projesi oluşturma işlemini açıklar.

**Yeni takım projesi oluşturmak için**

1. Üzerinde **Başlat** menüsünde **tüm programlar**, tıklayın **Microsoft Visual Studio 2010**, sağ **Microsoft Visual Studio 2010**, ve ardından **yönetici olarak çalıştır**.

    > [!NOTE]
    > Yeni Takım Projesi Sihirbazı, Visual Studio 2010'ı yönetici olarak çalıştırmazsanız, son adımda başarısız olur.
2. Varsa **kullanıcı hesabı denetimi** iletişim kutusu görüntülendikten sonra **Evet**.
3. Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.

    > [!NOTE]
    > TFS sunucusuna bir bağlantı yapılandırdıysanız, 4. ve 7. adımları atlayabilirsiniz.
4. İçinde **bağlantı takım projesine** iletişim kutusu, tıklayın **sunucuları**.
5. İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Ekle**.
6. İçinde **Team Foundation Server Ekle** iletişim kutusunda, TFS örneğiniz ayrıntılarını sağlayın ve ardından **Tamam**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Kapat**.
8. İçinde **takım projesine Bağlan** takım seçmek için bağlanmak istediğiniz TFS örneği proje koleksiyonunu ekleyin ve ardından istediğiniz iletişim kutusunda **Connect**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. İçinde **Takım Gezgini** penceresinde, takım projesi koleksiyonu ve ardından sağ tıklama **yeni takım projesi**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. İçinde **yeni takım projesi** iletişim kutusu, bir ad ve takım projesi için bir açıklama girin ve ardından **sonraki**.

    > [!NOTE]
    > Takım projeniz boşluk içeriyorsa, çıkış yolunu paketleri dağıtmak için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanmak için gelen bazı sorunlar karşılaşacağınız. Alanları yolunda, Web dağıtımı komutlarını çalıştırmak çok daha zor zorlaştırabilir.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. Üzerinde **işlem şablonu seçme** sayfasında, geliştirme sürecini yönetir ve ardından kullanmak istediğiniz işlem şablonunu seçin **sonraki**.

    > [!NOTE]
    > TFS işlem şablonları hakkında daha fazla bilgi için bkz. [işlem şablonları ve araçları](https://msdn.microsoft.com/vstudio/aa718795).
12. Üzerinde **takım sitesi ayarları** sayfasında varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.
13. Bu ayarı oluşturur veya TFS takım projesiyle ilişkilendirilmiş bir SharePoint ekip sitesi tanımlar. Geliştirme ekibiniz, bu site, belgeleri yönetmek, içinde tartışma zincirlerini katılmasına, wiki sayfaları oluşturun ve kodla ilişkili olmayan çeşitli görevleri gerçekleştirmek için kullanabilirsiniz. Daha fazla bilgi için [SharePoint ürünleri arasındaki etkileşimler ve Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. Üzerinde **kaynak denetim ayarları belirtmek** sayfasında varsayılan ayarları değiştirmeden bırakın ve ardından **sonraki**.
15. Bu ayar tanımlayan veya içeriğiniz için kök klasör olarak davranacak TFS klasör hiyerarşisindeki konumu oluşturur.
16. Üzerinde **takım projesi ayarlarını onaylayın** sayfasında **son**.
17. Yeni takım projesi başarıyla oluşturulduğunda, üzerinde **takım projesi oluştururken** sayfasında **Kapat**.

### <a name="add-users-to-a-team-project"></a>Bir takım projesine kullanıcı ekleme

Yeni takım projesi oluşturduğunuza göre bunları ekleme ve içerik üzerinde işbirliğine başlamak etkinleştirmek için kullanıcılara izinler verebilirsiniz.

**Kullanıcılar bir takım projesine eklemek için**

1. Visual Studio 2010 içinde **Takım Gezgini** penceresinde takım projesine sağ tıklayın, fareyle **takım projesi ayarları**ve ardından **grup üyeliği**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Bir kullanıcı eklemek, değiştirmek ve kaynak denetimi altında kod kaldırmak için ondan için eklemeniz gerekir **katkıda bulunanlar** grubu.
3. İçinde **proje gruplarını** iletişim kutusunda **katkıda bulunanlar** grup ve ardından **özellikleri**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusunda **Windows kullanıcı veya grup**ve ardından **Ekle**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. İçinde **kullanıcıları, bilgisayarları veya grupları** iletişim kutusuna kullanıcı takım projesine eklemek istediğiniz kullanıcı adına **Adları Denetle**ve ardından **Tamam**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. İçinde **Team Foundation Server Grup Özellikleri** iletişim kutusu, tıklayın **Tamam**.
7. İçinde **proje gruplarını** iletişim kutusu, tıklayın **Kapat**.

## <a name="conclusion"></a>Sonuç

Bu noktada, yeni takım proje'niz kullanılmaya hazırdır ve geliştirme takımınıza içerik ekleme ve geliştirme süreci üzerinde işbirliği yapmaya başlayabilirsiniz.

Bir sonraki konu [kaynak denetimine içerik ekleme](adding-content-to-source-control.md), kaynak denetimine içerik ekleme işlemi açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

TFS'de takım projeleri oluşturmak daha geniş yönergeler için bkz [bir takım projesi oluşturma](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Bir takım projesi koleksiyonu içindeki yeni takım projeleri oluşturmak kullanıcıları etkinleştirme ile ilgili daha fazla bilgi için bkz: [takım projesi koleksiyonları için Set Administrator Permissions](https://msdn.microsoft.com/library/dd547204.aspx). Takım projelerine kullanıcılar ekleme hakkında daha fazla bilgi için bkz. [takım projelerine kullanıcılar ekleme](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Önceki](configuring-team-foundation-server-for-web-deployment.md)
> [İleri](adding-content-to-source-control.md)
