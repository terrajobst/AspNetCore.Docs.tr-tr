---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Web paketleri el ile yükleme | Microsoft Docs
author: jrjlee
description: Bu konuda, bir web dağıtım paketi Internet Information Services (IIS) içine el ile içe aktarmayı açıklar. Paketleme Web uygulama ve konu oluşturuluyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: 4a28ea92b22e4928e41a39a8a91b62bfa4f08175
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="manually-installing-web-packages"></a>Web paketleri el ile yükleme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, bir web dağıtım paketi Internet Information Services (IIS) içine el ile içe aktarmayı açıklar.
> 
> Konu [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md) açıklanan IIS Web Dağıtım Aracı (Web dağıtımı), Microsoft Build Engine (MSBuild) ve Web yayımlama ardışık düzen (WPP), birlikte sağlar nasıl, paketi, Web uygulaması projelerine tek zip dosyasına. Web dağıtım paketi (veya sadece bir dağıtım paketi) olarak, yaygın olarak bilinen bu dosya, IIS web uygulamanızı bir web sunucusunda yeniden oluşturmak için gereken tüm içerik ve yapılandırma bilgileri içerir.
> 
> Web dağıtım paketini oluşturduktan sonra bir IIS sunucusuna çeşitli şekillerde yayımlayabilirsiniz. İçinde çok sayıda senaryoları, MSBuild, WPP ve Web dağıtımı oluşturmak ve web paketleri uzaktan bir otomatik olarak veya tek adımlı derleme ve dağıtım işleminin parçası olarak yüklemek için arasında tümleştirme noktaları yararlanmak istersiniz. Bu işlem açıklanan [dağıtma Web paketleri](deploying-web-packages.md). Ancak, bu her zaman mümkün değildir. Bir web uygulaması Internet'e üretim ortamında dağıtmak istediğinizi varsayalım. Güvenlik nedeniyle, bu tür bir üretim ortamında çok az olası bir güvenlik duvarının arkasında bulunan yapı sunucusundan bir çevre ağında (DMZ, sivil bölge ve denetimli alt ağ olarak da bilinir) ayrı bir alt ağda olması altındadır. İçinde çok örneklerinin üretim ortamına ayrı bir etki alanı veya fiziksel olarak yalıtılmış ağda olacaktır.
> 
> Bu senaryolarda, hedef sunucuya web paketi bağlantı noktası ve IIS ile el ile içeri aktarmak için tek seçeneğiniz olabilir. Bu yaklaşım otomatik dağıtım önleyen, hala bir web uygulaması yayımlama için son derece verimli bir yöntem olsa da&#x2014;sadece tek ZIP dosyası, web sunucunuza kopyalayın ve alma işleminde size kılavuzluk etmesi için bir Sihirbazı'nı kullanın.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

## <a name="task-overview"></a>Görev genel bakış

Web dağıtım paketi IIS'e içeri aktarmak için bu üst düzey görevleri tamamlamak gerekir:

- MSBuild komut satırında, ekip veya Visual Studio 2010'u kullanarak bir web dağıtım paketi oluşturun.
- Web paketinin hedef web sunucusuna kopyalayın.
- Alma uygulama paketi Sihirbazı'nı IIS Yöneticisi'nde web paketini yüklemek ve bağlantı dizeleri ve hizmet uç noktaları gibi değişkenleri için değerleri sağlamak için kullanın.

Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir. Görevleri ve bu konudaki yönergeler, zaten web paketleri, Web dağıtımı ve WPP kavramları aşina olduğunuzu varsayar. Daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Bu konu en iyi ile birlikte kullanılan [bir Web sunucusu Web dağıtımı yayımlama için (çevrimdışı dağıtımı) yapılandırmak](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), gerekli bileşenleri yüklemeniz ve bir IIS Web paketini içeri aktarmak üzere hazırlamak nasıl açıklar.


## <a name="create-a-web-deployment-package"></a>Web dağıtım paketi oluştur

İlk dağıtmak istediğiniz web uygulama projesi için bir web dağıtım paketi oluşturmak için bir görevdir. Web paketleri çeşitli yollarla oluşturabilirsiniz.

**Yaklaşım 1: Visual Studio ile yapılandırma işleminin bir parçası olarak bir paket oluşturun**

Her yapısı üzerinden sonra bir web dağıtım paketi oluşturmak için web uygulama projesi yapılandırabilirsiniz **Paketle/Yayımla Web** proje özellik sayfalarını sekmesinde. Bu işlem açıklanan [bina ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).

**Yaklaşım 2: MSBuild ile derleme işleminin parçası olarak bir paket oluşturun**

MSBuild doğrudan komut satırından veya bir özel MSBuild proje dosyası aracılığıyla kullanarak, web uygulama projesi oluşturursanız, bir web dağıtım paketi oluşturma işleminin bir parçası olarak dahil ederek oluşturabilirsiniz **DeployOnBuild = true** ve **DeployTarget paket =** komutunuzu özelliklerinde. Bu işlem açıklanan [oluşturma işlemini anlama](understanding-the-build-process.md).

**Yaklaşım 3: Visual Studio'da isteğe bağlı bir paket oluşturun**

Visual Studio 2010 herhangi bir zamanda bir web uygulaması projesi için bir web dağıtım paketi oluşturabilirsiniz. Bunu yapmak için **Çözüm Gezgini** penceresinde, web uygulaması projenize sağ tıklayın ve ardından **yapı dağıtım paketi**.

![](manually-installing-web-packages/_static/image1.png)

**Yaklaşım 4: komut satırından isteğe bağlı bir paket oluşturun**

Çağırarak komut satırından web dağıtım paketi oluşturabilir **paket** MSBuild kullanarak web uygulaması projenizin hedef. Komut şuna benzemelidir:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Hangisi yaklaşımını kullanın, sonuç aynıdır. WPP web dağıtım paketi, web uygulama projesi için çıkış klasöründe çeşitli destekleyici kaynakları ile birlikte bir zip dosyası olarak oluşturur.

![](manually-installing-web-packages/_static/image2.png)

Web paketini el ile içeri aktarmak planlama yaparken yalnızca zip dosyası gerektirir. Bu dosyayı, hedef web sunucusuna kopyalayın ve içeri aktarma işlemi başlayabilir.

## <a name="import-a-web-package-into-iis"></a>IIS ile bir Web paketi Al

Web dağıtım paketi yerel dosya sisteminden bir IIS Web sitesinde oturum almak için bir sonraki yordamı kullanın. Bu yordamı gerçekleştirmeden önce şunları yapın:

- Web dağıtım paketi, web sunucusuna kopyalar.
- Uygulamanızı barındırmak için IIS web sunucusu yapılandırılmış.

Web dağıtımı paketleri desteklemek üzere IIS web sunucusu yapılandırma hakkında daha fazla bilgi için bkz: [bir Web sunucusu Web dağıtımı yayımlama için (çevrimdışı dağıtımı) yapılandırmak](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**IIS Yöneticisi'ni kullanarak bir web dağıtım paketi içeri aktarmak için**

1. IIS Yöneticisi ' nde, **bağlantıları** bölmesinde, IIS Web sitesini sağ tıklayın, fareyle **dağıtma**ve ardından **alma uygulama**.

    ![](manually-installing-web-packages/_static/image3.png)
2. İçeri aktarma uygulama paketi Sihirbazı'nda üzerinde **paketi seçin** sayfa, web dağıtım paketinin konumuna göz atın ve ardından **sonraki**.
3. Üzerinde **paket içeriğini seçin** sayfasında, gerektiren ve ardından olmayan herhangi bir içerik temizleyin **sonraki**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > İçinde çok sayıda durumlarda, bir web dağıtım paketi ile birlikte gelen her şeyi almak istemeyebilirsiniz. Örneğin, ilişkili veritabanını değiştirmek Web dağıtımı izin istemeyebilirsiniz.  
    > **İzinleri verin** girişleri uygulama havuzu kimliğini Web sitesi içeriğini depolayan fiziksel klasör erişebildiğinden emin olmak için hedef dosya sistemi izinleri ayarlayın. Ayrıca, anonim kimlik doğrulaması kullanıcı verilen çok amaçlı Internet Posta Uzantıları (MIME) türü dosyaları işleme izin vermek amacıyla klasöre okuma izni. Tercih ederseniz, bu girişleri kaldırın ve izinlerini el ile yapılandırın.
4. Üzerinde **uygulama paketi bilgilerini girin** sayfasında, istenen bilgileri sağlayın.

    ![](manually-installing-web-packages/_static/image5.png)
5. Bir web paketi oluşturduğunuzda, WPP uygulamanız için yapılandırma dosyası analiz eder ve bağlantı dizeleri ve hizmet uç noktaları gibi herhangi bir değişkeni algılar. Bu durumda:

    1. **Uygulama yolu** uygulamanızı yüklemek istediğiniz IIS yoludur. Bu ayar WPP oluşturan tüm dağıtım paketleri için yaygın bir durumdur.
    2. **ContactService hizmet uç noktası adresi** uygulama dağıtılan WCF Hizmeti ile iletişim kurmak için kullanması gereken adresidir. Bu ayar, bir girişe karşılık gelir *web.config* dosya.
    3. İlk **bağlantı dizesi** Web dağıtımı uygulamada (Bu durumda bir ASP.NET üyelik veritabanından) ile ilişkili veritabanı dağıtmak için kullanması gereken bağlantı dizesi bir ayardır. Bu ayar, ayarları karşılık gelir **SQL Paketle/Yayımla** Visual Studio sekmesindedir.
    4. İkinci **bağlantı dizesi** uygulamanızı gerçekten da çalışır durumda olduğunda veritabanı ile iletişim kurmak için kullanacağı bağlantı dizesini bir ayardır. Bu bir bağlantı dizesi girişi karşılık *web.config* dosya.

        > [!NOTE]
        > Bu parametreler alınacağı yeri daha fazla bilgi için bkz: [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).
6. **İleri**'ye tıklayın.
7. Bu uygulama bu Web sitesine dağıttıktan sonra ilk kez değilse, yüklemeden önce var olan tüm içeriği silin isteyip istemediğinizi belirtmek için istenir. Gereksinimlerinize uygun seçeneği seçin ve ardından **sonraki**.

    ![](manually-installing-web-packages/_static/image6.png)
8. IIS paket yükleme tamamlandığında, tıklatın **son**.

    ![](manually-installing-web-packages/_static/image7.png)

Bu noktada, IIS web uygulamanızı başarıyla oluşturdunuz.

## <a name="conclusion"></a>Sonuç

Bu konu, IIS Yöneticisi'ni kullanarak bir IIS Web bir web dağıtım paketi içe açıklanmıştır. Web uygulaması yayımlama için bu yaklaşım, güvenlik veya altyapı kısıtlamaları imkansız veya istenmeyen Uzaktan dağıtım yaptığınızda uygundur.

## <a name="further-reading"></a>Daha Fazla Bilgi

El ile bir web paketi içe aktarma işlemlerini desteklemesi için bir IIS web sunucusu yapılandırma hakkında yönergeler için bkz [bir Web sunucusu Web dağıtımı yayımlama için (çevrimdışı dağıtımı) yapılandırmak](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Web paketleri dağıtma konusunda daha fazla genel yönergeler için bkz: [izlenecek yol: bir Web uygulaması projesi kullanarak bir Web dağıtım paketi (Kısım 1 / 4) dağıtma](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Önceki](creating-and-running-a-deployment-command-file.md)
