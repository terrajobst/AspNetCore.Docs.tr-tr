---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Kaynak denetimine içerik ekleme | Microsoft Docs
author: jrjlee
description: Bu konuda, kaynak denetimi Team Foundation Server (TFS) 2010 için içerik ekleme açıklanmaktadır. Bu projeler ve çözümler için takım proje ekleyeceğinizi açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 2119705a75d0717d05d4a7db69b3f5d38b1cdd45
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754908"
---
<a name="adding-content-to-source-control"></a>Kaynak denetimine içerik ekleme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, kaynak denetimi Team Foundation Server (TFS) 2010 için içerik ekleme açıklanmaktadır. TFS'de bir takım projesine çözümler ve projeler ekleme açıklar ve çerçeveleri veya derlemeleri gibi dış bağımlılıklar kaynak denetimine ekleme açıklanmaktadır.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

## <a name="task-overview"></a>Görev genel bakış

Çoğu durumda, içerik kaynak denetimine ekleyebilmek için geliştirici ekibinin her bir üyesi olmalıdır. TFS'de kaynak denetimine çözüm ekleme için üst düzey adımları tamamlamanız gerekir:

- Bir takım projesine bağlanın.
- Sunucu üzerindeki takım projesi klasör yapısı, yerel bilgisayarınızda bir klasör yapısı eşleyin.
- Çözümün ve içeriklerinin kaynak denetimine ekleyin.
- Herhangi bir dış bağımlılığın kaynak denetimine ekleyin.

Bu konuda, bu yordamları gerçekleştirmek nasıl gösterilmektedir.

Görevler ve bu konudaki yönergeler içeriğinizi yönetmek için yeni bir TFS takım projesi zaten oluşturduğunuzu varsayar. Yeni takım projesi oluşturma hakkında daha fazla bilgi için bkz. [TFS'de takım projesi oluşturma](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Kimler bu yordamları gerçekleştirir?

Çoğu durumda, her takım elemanının, geliştirici ekleme ve belirli takım projelerine içeriği değiştirebilirsiniz olmalıdır.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Bir takım projesine bağlanın ve bir klasörü eşlemesini oluşturma

Herhangi bir içerik kaynak denetimine eklemeden önce bir takım projesine bağlanın ve yerel makinenizde sunucuda klasör yapısını ve dosya sistemi arasında bir eşleme oluşturmak gerekir.

**Bir takım projesine bağlanın ve yerel bir yol eşlemek için**

1. Geliştirici iş istasyonunuza, Visual Studio 2010'u açın.
2. Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.

    > [!NOTE]
    > TFS sunucusuna bir bağlantı yapılandırdıysanız, 3-6. adımları atlayabilirsiniz.
3. İçinde **bağlantı takım projesine** iletişim kutusu, tıklayın **sunucuları**.
4. İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Ekle**.
5. İçinde **Team Foundation Server Ekle** iletişim kutusunda, TFS örneğiniz ayrıntılarını sağlayın ve ardından **Tamam**.

    ![](adding-content-to-source-control/_static/image1.png)
6. İçinde **Team Foundation Server Ekle/Kaldır** iletişim kutusu, tıklayın **Kapat**.
7. İçinde **takım projesine Bağlan** takım seçmek için bağlanmak istediğiniz TFS örneği seçin iletişim kutusunda, proje koleksiyonu, eklemek istediğiniz takım projesini seçin ve ardından **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. İçinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından çift **kaynak denetimi**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Üzerinde **Kaynak Denetim Gezgini** sekmesinde **eşlenmedi**.

    ![](adding-content-to-source-control/_static/image4.png)
10. İçinde **harita** iletişim kutusundaki **yerel klasör** kutusunda göz atın (veya oluşturma) takım projesi için kök klasör olarak davranır ve ardından yerel bir klasöre **harita**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Kaynak dosyalarını indirmek üzere istendiğinde tıklayın **Evet**.

    ![](adding-content-to-source-control/_static/image6.png)

Bu noktada, bir yerel klasör, geliştirici çalışma alanınız için takım projesi için sunucu tarafı klasörü eşlediğiniz. Ayrıca, takım projesinden yerel klasör yapınız varolan içeriği indirdikten. Artık kendi içerik kaynak denetimine eklemeye başlayabilirsiniz.

## <a name="add-projects-and-solutions-to-source-control"></a>Projeleri ve çözümleri kaynak denetimine Ekle

Projeleri ve çözümleri kaynak denetimine eklemek için önce onları takım projesi için eşlenen klasörü yerel makinenizde taşımak gerekir. İçerik ekleme sunucusu ile eşitlemek için kontrol edebilirsiniz.

**Projeler kaynak denetimine eklemek için**

1. Geliştirici iş istasyonunuza, projeler ve çözümler takım projesi için eşlenen klasörü yapısı içinde uygun bir konuma taşıyın.

    > [!NOTE]
    > Çoğu kuruluş, projeleri ve çözümleri kaynak denetimine nasıl düzenlenmelidir tercih edilen bir yaklaşım olacaktır. Yapı klasörlere ilişkin yönergeler için bkz [nasıl yapılır: Yapı uygulamanızın kaynak denetimi klasörleri Team Foundation Server'da](https://msdn.microsoft.com/library/bb668992.aspx).
2. Çözümü Visual Studio 2010'da açın.
3. İçinde **Çözüm Gezgini** penceresi, çözüme sağ tıklayın ve ardından **kaynak denetimine Çözüm Ekle**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > Kuruluşunuz, tfs'deki yapısı içeriği nasıl düşünmeyi bağlı olarak bazı durumlarda projeleri kaynak denetimine ayrı ayrı kaynak kodunuzu nasıl düzenlendiğini üzerinde daha hassas bir denetim sağlamak için eklemeniz gerekebilir.
4. Doğrulayın **Kaynak Denetim Gezgini** sekmesi, takım projesi için sunucu klasörü yapısı içinde eklediğiniz içeriği görüntüler.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **Kaynak Denetim Gezgini** sekmesi, Hayır, daha fazla istemde bulunulmaması ile eşlenmiş bir klasör yerel dosya sisteminde, çözüm eklediği için içeriğinizi görüntüler. Çözümünüzü eşlenmemiş bir konuma olduysa, hem TFS hem de yerel dosya sisteminize klasör konumlarını belirtin istenir.
5. Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** bölmesinde, takım projesine sağ tıklayın (örneğin, **ContactManager**) ve ardından **iade et Bekleyen değişiklikler**.
6. İçinde **iade – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.

    ![](adding-content-to-source-control/_static/image9.png)

Bu noktada çözümünüzün TFS'de kaynak denetimi eklediniz.

## <a name="add-external-dependencies-to-source-control"></a>Dış bağımlılıklar kaynak denetimine Ekle

Tüm dosya ve klasörler, proje veya çözüm içindeki bir proje veya çözüm kaynak denetimi eklediğinizde de eklenir. Ancak çok sayıda durumlarda, projeler ve çözümler aynı zamanda düzgün çalışması için yerel bütünleştirilmiş kod gibi dış bağımlılıkları yararlanmaktadır. Bu tür bir kaynaklar kaynak denetimine Team Build ve diğer geliştirici ekibi üyelerinin izin vermek için kodunuzu başarıyla derleme eklemeniz gerekir.

Örneğin, örnek Kişi Yöneticisi çözümü klasör yapısını paketleri adlı bir klasör içerir. Bu, ADO.NET varlık çerçevesi 4.1 için derleme ve çeşitli destekleyici kaynakları içerir. Packages klasörünü Kişi Yöneticisi çözümünü parçası değil, ancak bu olmadan çözüm başarıyla oluşturmaz. Çözümü derlemek Team Build'ı etkinleştirmek için kaynak denetimine packages klasörünü eklemeniz gerekir.

> [!NOTE]
> Bir paket klasörüne çözümünüze NuGet uzantısı için Visual Studio 2010 kullanarak Entity Framework veya benzer kaynakları eklediğinizde ne olur tipik eklenmesidir.


**Proje-olmayan içeriği kaynak denetimine eklemek için**

1. (Örneğin, packages klasörünü) eklemek istediğiniz öğeleri yerel dosya sisteminize eşlenen bir klasördeki uygun bir konumda olduğundan emin olun.
2. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından çift **kaynak denetimi**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** eklemek istediğiniz öğeyi içeren veya maddeleri klasörü bölmesinde seçin.
4. Tıklayın **öğeleri klasöre Ekle** düğmesi.

    ![](adding-content-to-source-control/_static/image11.png)
5. İçinde **kaynak denetimine Ekle** iletişim kutusunda, klasör veya ekleyin ve ardından istediğiniz öğeleri seçin **sonraki**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Üzerinde **öğeyi hariç Tuttu** sekmesinde, otomatik olarak (örneğin, derlemeleri) dışarıda ve ardından gerekli öğeleri seçin **öğeleri dahil et**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Üzerinde **eklenecek öğeler** sekmesinde, eklemek istediğiniz tüm dosyaları listelenir ve ardından doğrulayın **son**.

    ![](adding-content-to-source-control/_static/image14.png)
8. İçinde **Kaynak Denetim Gezgini** penceresinde tıklayın **iade** düğmesi.

    ![](adding-content-to-source-control/_static/image15.png)
9. İçinde **iade – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.

Bu noktada, çözümünüz için dış bağımlılıklar için kaynak denetimi eklediniz.

## <a name="conclusion"></a>Sonuç

Bu konuda bir takım projesine bağlanın, klasör yapısını ve kaynak denetimine içerik ekleme açıklanmıştır. Kaynak denetimi altındaki öğelerle çalışma konusunda daha fazla bilgi için bkz. [kullanarak sürüm denetimi](https://msdn.microsoft.com/library/ms181368.aspx).

Bir sonraki konu [bir TFS derleme sunucusunu Web dağıtımı için yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md), derlemek ve çözümünüzü dağıtmak için bir TFS takım yapı sunucunuzu hazırlama işlemini açıklamaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Kaynak denetimi, TFS ile çalışma hakkında daha kapsamlı bilgi için bkz. [kullanarak sürüm denetimi](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Önceki](creating-a-team-project-in-tfs.md)
> [İleri](configuring-a-tfs-build-server-for-web-deployment.md)
