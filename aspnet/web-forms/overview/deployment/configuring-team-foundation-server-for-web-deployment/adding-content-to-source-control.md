---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: "İçerik kaynak denetimine ekleme | Microsoft Docs"
author: jrjlee
description: "Bu konuda, kaynak denetimine Team Foundation Server (TFS) 2010 içeriği eklemek açıklanmaktadır. Çözümler ve projeler için bir takım proje ekleneceğini açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: a6a90a03674cfe7565da0ed56148186ee9525707
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-content-to-source-control"></a>İçerik kaynak denetimine ekleme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, kaynak denetimine Team Foundation Server (TFS) 2010 içeriği eklemek açıklanmaktadır. Çözümler ve projeler TFS takım projesinde nasıl ekleneceğini açıklar ve çerçeveleri veya derlemeler gibi dış bağımlılıklar kaynak denetimine ekleme konusunda açıklanmaktadır.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.

## <a name="task-overview"></a>Görev genel bakış

Çoğu durumda, her geliştirici ekip üyesi içerik kaynak denetimine ekleyebilirsiniz olmalıdır. TFS kaynak denetiminde bir çözüm eklemek için üst düzey adımları tamamlamanız gerekir:

- Bir takım projesine bağlanma.
- Takım projesi klasör yapısı sunucu üzerindeki bir klasör yapısı, yerel bilgisayarınızda eşleyin.
- Kaynak denetimine çözüm ve içeriği ekleyin.
- Dış bağımlılıkları kaynak denetimine ekleyin.

Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir.

Görevleri ve bu konudaki yönergeler içeriğinizi yönetmek için yeni bir TFS ekip projesi zaten oluşturduğunuzu varsayalım. Yeni takım projesi oluşturma hakkında daha fazla bilgi için bkz: [TFS'de takım projesi oluşturma](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>Kim, bu yordamları gerçekleştirir mi?

Çoğu durumda, her geliştirici ekip üyesi eklemek ve belirli bir takım projeleri içinde içerik değiştirmek mümkün olmalıdır.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Bir takım projesine bağlama ve klasör eşlemesi oluşturma

Kaynak denetimi için herhangi bir içerik eklemeden önce bir takım projesine bağlanma ve yerel makinenizde sunucudaki klasör yapısını ve dosya sistemi arasında bir eşleme oluşturmanız gerekir.

**Bir takım projesine bağlanma ve bir yerel yol eşlemek için**

1. Geliştirici iş istasyonunuza, Visual Studio 2010'u açın.
2. Visual Studio'da üzerinde **takım** menüsünde tıklatın **Team Foundation Server'a Bağlan**.

    > [!NOTE]
    > TFS sunucusu bağlantısı zaten yapılandırdıysanız, 3-6 adımları atlayabilirsiniz.
3. İçinde **takım projesine bağlantı** iletişim kutusu, tıklatın **sunucuları**.
4. İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Ekle**.
5. İçinde **Team Foundation Server Ekle** iletişim kutusu, TFS örneğinizi ayrıntılarını sağlayın ve ardından **Tamam**.

    ![](adding-content-to-source-control/_static/image1.png)
6. İçinde **Ekle/Kaldır Team Foundation Server** iletişim kutusu, tıklatın **Kapat**.
7. İçinde **takım projesine Bağlan** iletişim kutusu, select takım seçmek için bağlanmak istediğiniz TFS örnek proje koleksiyonu, eklemek istediğiniz takım projesini seçin ve ardından **Bağlan**.

    ![](adding-content-to-source-control/_static/image2.png)
8. İçinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından **kaynak denetimi**.

    ![](adding-content-to-source-control/_static/image3.png)
9. Üzerinde **Kaynak Denetim Gezgini** sekmesini tıklatın, **eşlenmedi**.

    ![](adding-content-to-source-control/_static/image4.png)
10. İçinde **harita** iletişim kutusunda **yerel klasör** kutusunda, göz atın (veya oluşturma) takım projesi için kök klasör olarak davranmasına ve ardından yerel bir klasöre **harita**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Kaynak dosyalarını indirmek üzere istendiğinde tıklatın **Evet**.

    ![](adding-content-to-source-control/_static/image6.png)

Bu noktada, yerel bir klasöre Geliştirici istasyonunuzda takım projesi için sunucu tarafı klasörünün eşledikten. Ayrıca, varolan içeriğin takım projesi yerel klasör yapınız indirdiğiniz. Kaynak denetimi için kendi içerik eklemek şimdi başlayabilirsiniz.

## <a name="add-projects-and-solutions-to-source-control"></a>Projeler ve çözümler kaynak denetimine ekleme

Projeler ve çözümler için kaynak denetimi eklemek için önce bunları takım projesine eşlenmiş klasörünü yerel makinenizde taşımak gerekir. Ardından, eklemeler sunucusu ile eşitlemek için içerik kontrol edebilirsiniz.

**Kaynak denetimi projeleri eklemek için**

1. Geliştirici iş istasyonunuza, projeler ve çözümler takım projesine eşlenmiş klasörü yapısı içinde uygun bir konuma taşıyın.

    > [!NOTE]
    > Birçok kuruluş, projeler ve çözümler kaynak denetiminde nasıl düzenlenmelidir bir tercih edilen yaklaşım sahip olur. Yapı klasörlere konusunda yönergeler için bkz [nasıl yapılır: Yapı bilgisayarınızı kaynak denetim klasörlerine Team Foundation Server'da](https://msdn.microsoft.com/en-us/library/bb668992.aspx).
2. Çözümü Visual Studio 2010'da açın.
3. İçinde **Çözüm Gezgini** penceresinde, çözüme sağ tıklayın ve ardından **kaynak denetimine Çözüm Ekle**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > Kuruluşunuz TFS, yapısı içeriği nasıl istemeyebilir bağlı olarak bazı durumlarda tek tek kaynak kodunuzu nasıl düzenlendiğini üzerinde daha hassas bir denetim sağlamak için kaynak denetimi projeleri eklemeniz gerekebilir.
4. Doğrulayın **Kaynak Denetim Gezgini** sekmesi, takım projesi için sunucu klasörü yapısı içinde eklediğiniz içeriği görüntüler.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > **Kaynak Denetim Gezgini** sekmesi eşlenmiş bir klasör yerel dosya sisteminde çözümünüzü eklenmiş olduğundan başka isteyen ile içeriğinizi görüntüler. Çözümünüzü eşlenmemiş bir konumda varsa, TFS hem yerel dosya sisteminizi klasör konumlarını belirtin istenir.
5. Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** bölmesinde, takım projesine sağ tıklayın (örneğin, **ContactManager**) ve ardından **iade et Bekleyen değişiklikler**.
6. İçinde **iade et – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.

    ![](adding-content-to-source-control/_static/image9.png)

Bu noktada çözümünüzün TFS kaynak denetiminde eklediniz.

## <a name="add-external-dependencies-to-source-control"></a>Dış bağımlılıkları kaynak denetimine ekleme

Kaynak denetimi için bir proje ya da çözüm eklediğinizde, tüm dosya ve klasörler proje ya da çözüm içinde de eklenir. Bununla birlikte, içinde çok sayıda durumlarda, projeler ve çözümler de düzgün çalışması için yerel derlemeler gibi dış bağımlılıklar kullanır. Bu tür bir kaynaklar ekip ve diğer geliştirici ekibi üyelerinin izin vermek için kaynak denetimi için kodunuzu başarıyla yapı eklemeniz gerekir.

Örneğin, kişinin Yöneticisi örnek çözümü için klasör yapısı paketleri adlı bir klasör içerir. Bu seçenek, derleme ve çeşitli destekleyici kaynakları için ADO.NET Entity Framework 4.1 içerir. Paketler klasörü Contact Manager çözümün bir parçası değil, ancak olmadan çözüm başarıyla oluşturmaz. Çözümü derlemek ekip etkinleştirmek için kaynak denetimine paketler klasörü eklemeniz gerekir.

> [!NOTE]
> Paketler klasörü Visual Studio 2010 için NuGet uzantısını kullanarak, çözümünüz için Entity Framework veya benzer kaynakları eklediğinizde ne olur tipik eklenmesidir.


**Kaynak denetimi proje olmayan içerik eklemek için**

1. (Örneğin, paketler klasörü) eklemek istediğiniz öğeleri yerel dosya sistemine eşlenen bir klasördeki uygun bir konumda olduğundan emin olun.
2. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projenizi genişletin ve ardından **kaynak denetimi**.

    ![](adding-content-to-source-control/_static/image10.png)
3. Üzerinde **Kaynak Denetim Gezgini** sekmesinde **klasörleri** eklemek istediğiniz öğeyi içeren veya öğeleri klasörü bölmesinde seçin.
4. Tıklatın **klasöre öğe ekleme** düğmesi.

    ![](adding-content-to-source-control/_static/image11.png)
5. İçinde **eklemek için kaynak denetimi** iletişim kutusunda, klasör veya ekleyin ve ardından istediğiniz öğeleri seçin **sonraki**.

    ![](adding-content-to-source-control/_static/image12.png)
6. Üzerinde **öğeleri dışarıda** sekmesinde, otomatik olarak (örneğin, derlemeler) dışarıda ve ardından gereken öğeleri seçin **öğeleri dahil**.

    ![](adding-content-to-source-control/_static/image13.png)
7. Üzerinde **eklemek için öğeleri** sekmesinde, dahil etmek istediğiniz tüm dosyaları listelenir ve ardından doğrulayın **son**.

    ![](adding-content-to-source-control/_static/image14.png)
8. İçinde **Kaynak Denetim Gezgini** penceresinde tıklatın **iade** düğmesi.

    ![](adding-content-to-source-control/_static/image15.png)
9. İçinde **iade et – kaynak dosyaları** iletişim kutusu, bir açıklama yazın ve ardından **iade**.

Bu noktada, çözümünüz için kaynak denetimi için dış bağımlılıklar eklediniz.

## <a name="conclusion"></a>Sonuç

Bu konuda bir takım projesine bağlanma, bir klasör yapısı harita ve içerik kaynak denetimine ekleme açıklanmıştır. Kaynak denetimindeki öğeleri ile çalışma konusunda daha fazla bilgi için bkz: [kullanarak sürüm denetimi](https://msdn.microsoft.com/en-us/library/ms181368.aspx).

Sonraki konuyu [TFS yapı bir sunucu için Web dağıtımı yapılandırma](configuring-a-tfs-build-server-for-web-deployment.md), bir TFS ekip sunucusu oluşturmak ve çözümünüzü dağıtmak için hazırlamayı açıklar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Kaynak denetimi TFS ile çalışma hakkında daha kapsamlı bilgi için bkz: [kullanarak sürüm denetimi](https://msdn.microsoft.com/en-us/library/ms181368.aspx).

>[!div class="step-by-step"]
[Önceki](creating-a-team-project-in-tfs.md)
[sonraki](configuring-a-tfs-build-server-for-web-deployment.md)
