---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
title: Web paketlerini el ile yükleme | Microsoft Docs
author: jrjlee
description: Bu konuda, bir web dağıtım paketi Internet Information Services (IIS) içinde el ile içe aktarmayı açıklar. Konu oluşturma ve paketleme Web uygulama...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f11d22a7-5d32-4ad0-8a9b-276460a61c06
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/manually-installing-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d2b2e4852d01f62feef40f8b15252737327ec4ec
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386633"
---
<a name="manually-installing-web-packages"></a>Web paketlerini el ile yükleme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, bir web dağıtım paketi Internet Information Services (IIS) içinde el ile içe aktarmayı açıklar.
> 
> Konu [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md) açıklanan nasıl IIS Web Dağıtım Aracı (Web dağıtımı) birlikte Microsoft Build Engine (MSBuild) ve Web yayımlama işlem hattı (Usewpp_copywebapplication), izin verir, paket, Web Uygulama projeleri bir tek bir ZIP dosyasına. Web dağıtım paketi (veya sadece bir dağıtım paketi olarak), yaygın olarak bilinen bu dosya, IIS web sunucusunda web uygulamanızı yeniden oluşturmak için gereken tüm içerik ve yapılandırma bilgileri içerir.
> 
> Web dağıtım paketi oluşturduktan sonra çeşitli yollarla IIS sunucusuna yayımlayabilirsiniz. İçinde çok sayıda senaryo, tümleştirme noktaları arasında MSBuild WPP ve Web dağıtımı oluşturmak ve web paketleri uzaktan bir otomatik olarak veya tek adımlı derleme ve dağıtım sürecinin bir parçası olarak yüklemek için yararlanmak isteyebilirsiniz. Bu işlem açıklanan [Web paketleri dağıtma](deploying-web-packages.md). Ancak, bu her zaman mümkün değildir. Internet'e yönelik bir üretim ortamında bir web uygulamasına dağıtmak istediğinizi varsayalım. Güvenlik nedeniyle, bu tür bir üretim ortamına derleme sunucusundan bir çevre ağındaki (DMZ, sivil bölge ve denetimli alt ağ olarak da bilinir) ayrı bir alt ağdaki bir güvenlik duvarının arkasında olması olası çok az altındadır. İçinde çok sayıda servis talepleri, ayrı bir etki alanı veya fiziksel olarak yalıtılmış bir ağda üretim ortamına olacaktır.
> 
> Bu senaryolarda, tek seçeneğiniz, hedef sunucuya web paketi bağlantı noktası ve IIS içinde el ile içeri aktarmak için olabilir. Bu yaklaşım, otomatik dağıtım ışığının rağmen bir web uygulaması yayımlamak için son derece etkili bir teknik hala açıktır&#x2014;yalnızca tek bir ZIP dosyasını web sunucunuza kopyalayın ve içeri aktarma işlemi boyunca size rehberlik için bir sihirbaz kullanın.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

## <a name="task-overview"></a>Görev genel bakış

Web dağıtım paketi, IIS ile içeri aktarmak için bu üst düzey görevleri tamamlamak gerekir:

- MSBuild komut satırı, ekip veya Visual Studio 2010'u kullanarak bir web dağıtım paketi oluşturun.
- Web paketi hedef web sunucusuna kopyalayın.
- Uygulama paketi İçeri Aktarma Sihirbazı'nı IIS Yöneticisi'nde bağlantı dizelerini ve hizmet uç noktaları gibi değişkenler için değerler sağlayın ve web paketi yüklemek için kullanın.

Bu konuda, bu yordamları gerçekleştirmek nasıl gösterilmektedir. Görevler ve bu konudaki yönergeler, zaten web paketleri, Web dağıtımı ve WPP kavramları hakkında bilgi sahibi olduğunuzu varsayar. Daha fazla bilgi için [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).

> [!NOTE]
> Bu konuda en iyi birlikte kullanılan [bir Web sunucusunu Web dağıtımı yayımlama için (çevrimdışı dağıtım) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md), gerekli bileşenleri yüklemeniz ve paketi içeri aktarma için bir IIS Web hazırlamak nasıl açıklar.


## <a name="create-a-web-deployment-package"></a>Web dağıtım paketi oluştur

İlk görev bir web dağıtım paketi dağıtmak istediğiniz web uygulaması projesi için oluşturmaktır. Web paketleri çeşitli yollarla oluşturabilirsiniz.

**Yaklaşım 1: Visual Studio ile derleme işleminin bir parçası olarak bir paket oluşturma**

Web uygulaması projenize her derlemeden sonra bir web dağıtım paketi oluşturmak için yapılandırabileceğiniz **Paketle/Yayımla Web** proje özelliği sayfalarından sekmesinde. Bu işlem açıklanan [oluşturma ve paketleme Web Uygulama projeleri](building-and-packaging-web-application-projects.md).

**Yaklaşım 2: MSBuild ile derleme işleminin bir parçası olarak bir paket oluşturma**

MSBuild, doğrudan komut satırından veya özel bir MSBuild proje dosyası aracılığıyla kullanarak web uygulaması projenize yapı web dağıtım paketi oluşturma işleminin bir parçası dahil ederek oluşturabileceğiniz **DeployOnBuild = true** ve **DeployTarget paket =** özelliklerinde komutu. Bu işlem açıklanan [derleme işlemini anlama](understanding-the-build-process.md).

**Yaklaşım 3: Visual Studio'da isteğe bağlı bir paket oluşturun.**

Visual Studio 2010'daki herhangi bir zamanda bir web uygulaması projesi için web dağıtım paketi oluşturabilirsiniz. Bunu yapmak için **Çözüm Gezgini** penceresinde web uygulaması projenize sağ tıklayın ve ardından **derleme dağıtım paketi**.

![](manually-installing-web-packages/_static/image1.png)

**Yaklaşım 4: komut satırından isteğe bağlı bir paket oluşturun.**

Web dağıtım paketi komut satırından çağırarak oluşturabileceğiniz **paket** MSBuild'ı kullanarak web uygulaması projenize hedefte. Komut şuna benzemelidir:


[!code-console[Main](manually-installing-web-packages/samples/sample1.cmd)]


Hangisi yaklaşımını kullanmak, sonuç aynıdır. WPP web dağıtım paketi, web uygulaması projesi için çıkış klasöründe çeşitli destekleyici kaynakları birlikte bir zip dosyası olarak oluşturur.

![](manually-installing-web-packages/_static/image2.png)

Web paketi el ile içeri aktarma planlamanız durumunda, yalnızca zip dosyası gerektirir. Bu dosya hedef web sunucusuna kopyalayın ve içeri aktarma işlemi başlayabilirsiniz.

## <a name="import-a-web-package-into-iis"></a>IIS içinde Web paketi içeri aktarma

Sonraki yordam, web dağıtım paketi, yerel dosya sisteminden bir IIS Web içeri aktarmak için kullanabilirsiniz. Bu yordamı gerçekleştirmeden önce sahip olduğunuzdan emin olun:

- Web dağıtım paketi, web sunucusuna kopyalanır.
- Uygulamanızı barındırmak için IIS web sunucusu yapılandırıldı.

Bir IIS web sunucusunu web dağıtımı paketleri destekleyecek şekilde yapılandırma hakkında daha fazla bilgi için bkz: [bir Web sunucusunu Web dağıtımı yayımlama için (çevrimdışı dağıtım) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).

**IIS Yöneticisi'ni kullanarak bir web dağıtımı paketini içeri aktarmak için**

1. IIS Yöneticisi'nde, **bağlantıları** bölmesinde, IIS Web sitesini sağ tıklatın, **Dağıt**ve ardından **alma uygulama**.

    ![](manually-installing-web-packages/_static/image3.png)
2. İçeri aktarma uygulama paketi Sihirbazı'nda üzerinde **paketi seçin** sayfasında web dağıtım paketinin konumuna göz atın ve ardından **sonraki**.
3. Üzerinde **paket içeriğini seçin** sayfasında, gerektiren ve ardından olmayan herhangi bir içeriği temizleyin **sonraki**.

    ![](manually-installing-web-packages/_static/image4.png)

    > [!NOTE]
    > İçinde çok sayıda durumlarda, bir web dağıtım paketi ile birlikte gelen her şeyi almak istemeyebilirsiniz. Örneğin, ilişkili veritabanını değiştirmek Web dağıtımı izin istemeyebilirsiniz.  
    > **İzinleri verin** girişleri, uygulama havuzu kimliğini Web sitesi içeriğini saklayan fiziksel klasör erişebildiğinden emin olmak için hedef dosya sistemi izinleri ayarlayın. Ayrıca, anonim kimlik doğrulaması kullanıcı klasörü çok amaçlı Internet Posta Uzantıları (MIME) türü dosyaları işleme izin vermek için izin verilen okunur. Tercih ederseniz, bu girişleri kaldırın ve izinlerini el ile yapılandırın.
4. Üzerinde **uygulama paketi bilgilerini girin** sayfasında, istenen bilgileri sağlayın.

    ![](manually-installing-web-packages/_static/image5.png)
5. Web paketi oluşturduğunuzda, WPP uygulamanız için yapılandırma dosyasını analiz eder ve bağlantı dizeleri ve hizmet uç noktaları gibi herhangi bir değişkeni algılar. Bu durumda:

    1. **Uygulama yolu** uygulamanızı yüklemek istediğiniz IIS yoludur. Bu ayar, WPP oluşturan tüm dağıtım paketleri yaygındır.
    2. **Hizmet uç noktası adresi ContactService** uygulamanın dağıtılan WCF Hizmeti ile iletişim kurmak için kullanması gereken adresidir. Bu ayar bir girişe karşılık gelen *web.config* dosya.
    3. İlk **bağlantı dizesi** Web dağıtımı (Bu durumda bir veritabanındaki ASP.NET üyeliği) uygulama ile ilişkili veritabanı dağıtmak için kullanması gereken bağlantı dizesini bir ayardır. Bu ayar, ayarları karşılık gelen **SQL Paketle/Yayımla** Visual Studio'da sekmesi.
    4. İkinci **bağlantı dizesi** gerçekten uygulamanız hazır ve çalışır durumda olduğunda veritabanı ile iletişim kurmak için kullanacağı bağlantı dizesini bir ayardır. Bu bir bağlantı dizesi giriş karşılık gelen *web.config* dosya.

        > [!NOTE]
        > Bu parametreleri nereden geldiğini daha fazla bilgi için bkz: [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md).
6. **İleri**'ye tıklayın.
7. Bu Web uygulamasına dağıttınız ilk kez bu değilse, yükleme öncesinde tüm mevcut içeriğini silmek isteyip istemediğinizi belirtmek için istenir. Gereksinimlerinize uygun seçeneği seçin ve ardından **sonraki**.

    ![](manually-installing-web-packages/_static/image6.png)
8. IIS paketinin yüklenmesi tamamlandığında, tıklayın **son**.

    ![](manually-installing-web-packages/_static/image7.png)

Bu noktada, web uygulamanızı IIS'e başarıyla oluşturdunuz.

## <a name="conclusion"></a>Sonuç

Bu konuda, IIS Yöneticisi'ni kullanarak bir IIS Web sitesine bir web dağıtım paketi içeri aktarma açıklanmaktadır. Web uygulaması yayımlamaya bu yaklaşım, güvenlik veya altyapı kısıtlamaları, mümkün olmayan veya istenmeyen Uzaktan dağıtım yaptığınızda uygundur.

## <a name="further-reading"></a>Daha Fazla Bilgi

El ile bir web paketi içe aktarma işlemlerini desteklemesi için IIS web sunucusu yapılandırma hakkında yönergeler için bkz. [bir Web sunucusunu Web dağıtımı yayımlama için (çevrimdışı dağıtım) yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md). Web paketleri dağıtma konusunda daha fazla genel yönergeler için bkz: [izlenecek yol: bir Web uygulaması projesi kullanarak bir Web dağıtımı paketi (Kısım 1 / 4) dağıtma](https://msdn.microsoft.com/library/dd483479.aspx).

> [!div class="step-by-step"]
> [Önceki](creating-and-running-a-deployment-command-file.md)
