---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Dosya ve klasörleri dağıtımdan dışlama | Microsoft Docs
author: jrjlee
description: Bu konuda, yapı ve bir web uygulaması projesi paketini nasıl, dosya ve klasörleri web dağıtım paketinden hariç tutabilirsiniz açıklanmaktadır.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: fd7357a94ab09effcec86f3725a37cfb2ef4746a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753943"
---
<a name="excluding-files-and-folders-from-deployment"></a>Dosya ve klasörleri dağıtımdan dışlama
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, yapı ve bir web uygulaması projesi paketini nasıl, dosya ve klasörleri web dağıtım paketinden hariç tutabilirsiniz açıklanmaktadır.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="overview"></a>Genel Bakış

Visual Studio 2010'daki bir web uygulaması projesi oluşturma sırasında Web yayımlama işlem hattı (Usewpp_copywebapplication), bu yapı işlemini derlenmiş web uygulamanıza dağıtılabilir web paketi paketleyerek genişletmenizi sağlar. Bu web paketini dağıtmak için bir uzak bir IIS web sunucusu (Web dağıtımı) Internet Information Services (IIS) Web dağıtım aracı kullanın veya el ile IIS Yöneticisi aracılığıyla web paketini içeri aktarma. Bu paketleme işlemi açıklanan [oluşturma ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Paket nasıl web dahil kontrol edebilirim? Temel Proje dosyasının Visual Studio'da proje ayarlarıyla birçok senaryo için yeterli denetim sağlar. Ancak, bazı durumlarda belirli hedef ortamlara web paketinizi içeriğini uyarlama isteyebilirsiniz. Örneğin, uygulamanızı test ortamına dağıtın, ancak bir hazırlık veya üretim ortamına uygulamanın dağıttığınızda klasörü dışlamak günlük dosyaları için bir klasör eklemek isteyebilirsiniz. Bu konu, bunun nasıl yapılacağı gösterilmektedir.

## <a name="what-gets-included-by-default"></a>Varsayılan olarak neleri?

Visual Studio'da web uygulaması proje özelliklerinizi yapılandırdığınızda **dağıtmak için öğeleri** listesini **Paketle/Yayımla Web** sayfası, web dağıtımınızdaki dahil edilmesini istediğiniz belirtmenize olanak sağlar Paket. Varsayılan olarak, bu ayar **yalnızca bu uygulamayı çalıştırmak için gereken dosyaları**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Seçeneğini belirlediğinizde **yalnızca bu uygulamayı çalıştırmak için gereken dosyaları**, WPP hangi dosyaların web pakete eklenmesi gerektiğini belirlemeyi dener. Şunları içerir:

- Proje için tüm yapı çıkarır.
- Tüm dosyalar olarak işaretlenmiş derleme eylemini **içerik**.

> [!NOTE]
> Dahil etmek için hangi dosyaların belirleyen mantık, bu dosyada yer alır:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Belirli dosyaları ve klasörleri dışarıda

Bazı durumlarda, dosya ve klasörler üzerinde dağıtılan daha ayrıntılı denetim istersiniz. Hangi dosyaların, öncesinde hariç tutmak istediğiniz zaman, bildiğiniz ve dışlama tüm hedef ortamlar için geçerlidir, ayarlayabilirsiniz **derleme eylemi** her dosyanın **hiçbiri**.

**Belirli dosyaları dağıtımından hariç tutmak için**

1. İçinde **Çözüm Gezgini** penceresi, dosyaya sağ tıklayın ve ardından **özellikleri**.
2. İçinde **özellikleri** penceresi, **derleme eylemi** satır, select **hiçbiri**.

Ancak, bu yaklaşım her zaman uygun değildir. Örneğin, hangi dosyaların farklılık isteyebilirsiniz ve klasörleri hedef ortamınız göre ve Visual Studio dışında dahildir. Örneğin, kişi yöneticisi örnek çözümde ContactManager.Mvc projenin içeriğini göz atın:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- İç klasör oluşturma, bırakma ve geliştirme amacıyla yerel veritabanları doldurmak için geliştirici kullanan bazı SQL komut dosyalarını içerir. Bu klasörde hiçbir şey bir hazırlık veya üretim ortamı için dağıtılmalıdır.
- Betikler klasörüne çeşitli JavaScript dosyalarını içerir. Çok sayıda bu dosya yalnızca hata ayıklama desteği ya da Visual Studio IntelliSense sağlamak için dahil edilir. Bu dosyaların bazıları için hazırlık veya üretim ortamlarında dağıtılmamalıdır. Ancak, bunları sorun giderme sürecini kolaylaştırmak için bir geliştirici test ortamına dağıtmak isteyebilirsiniz.

Belirli dosyaları ve klasörleri dışlamak için proje dosyalarınızı düzenlersiniz olsa da, daha kolay bir yolu yoktur. WPP adlı öğesi listeleri oluşturarak, dosya ve klasörleri dışlamak için bir mekanizma içerir **ExcludeFromPackageFolders** ve **ExcludeFromPackageFiles**. Bu listelerden kendi öğeleri ekleyerek, bu mekanizma genişletebilirsiniz. Bunu yapmak için üst düzey adımları tamamlamanız gerekir:

1. Adlı bir özel proje dosyası oluşturmayı *[Proje adı].wpp.targets* , proje dosyası ile aynı klasörde.

    > [!NOTE]
    > *. Wpp.targets* dosyasına gereken web uygulaması proje dosyanız ile aynı klasörde Git&#x2014;gibi *ContactManager.Mvc.csproj*&#x2014;yerine aynı klasöre herhangi bir özel Proje dosyaları derleme ve dağıtım işlemini denetlemek için kullanın.
2. İçinde *. wpp.targets* ekleyin bir **ItemGroup** öğesi.
3. İçinde **ItemGroup** öğe, Ekle **ExcludeFromPackageFolders** ve **ExcludeFromPackageFiles** belirli dosyaları ve klasörleri gerektiği gibi çıkarılacak öğeleri.

Bu temel yapısını budur *. wpp.targets* dosyası:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Her öğe adlı bir öğe meta verileri öğe içerdiğini unutmayın **FromTarget**. Bu, yapı işlemi etkilemez isteğe bağlı bir değerdir; yalnızca belirli dosyaları veya klasörleri neden atlanmış belirtmek için kullanılır, birisi Derleme günlüklerini inceler.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Bir Web paketinden dosya ve klasörleri dışarıda

Sonraki yordam nasıl ekleneceğini gösterir bir *. wpp.targets* dosyasını bir web uygulaması projesi ve nasıl projenizi yapılandırdığınızda web paketinden belirli dosyaları ve klasörleri dışlamak için bu dosyayı kullanın.

**Web dağıtım paketinden dosyaları ve klasörleri dışlamak için**

1. Visual Studio 2010'da çözümünüzü açın.
2. İçinde **Çözüm Gezgini** penceresinde, web uygulaması proje düğümüne sağ tıklayın (örneğin, **ContactManager.Mvc**), işaret **Ekle**ve ardından**Yeni öğe**.
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **XML dosyası** şablonu.
4. İçinde **adı** kutusuna *[Proje adı] ***.wpp.targets** (örneğin, **ContactManager.Mvc.wpp.targets**) ve ardından **Ekle**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Bir proje kök düğümü için yeni bir öğe eklerseniz, dosyayı proje dosyası ile aynı klasörde oluşturulur. Bu, Windows Gezgini'nde klasörü açarak doğrulayabilirsiniz.
5. Dosyasına ekleyin bir **proje** öğesi ve bir **ItemGroup** öğesi:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Web paketinden klasörleri dışlamak istiyorsanız, ekleme bir **ExcludeFromPackageFolders** öğesine **ItemGroup** öğesi:

   1. İçinde **INCLUDE** özniteliği, hariç tutmak istediğiniz klasörleri noktalı virgülle ayrılmış bir listesini sağlayın.
   2. İçinde **FromTarget** meta veri öğesi neden klasörleri, adı gibi dışlanan belirtmek için anlamlı bir değer sağlayın *. wpp.targets* dosya.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Web paketinden dosyaları dışlamak istiyorsanız, ekleme bir **ExcludeFromPackageFiles** öğesine **ItemGroup** öğesi:

   1. İçinde **INCLUDE** özniteliği, hariç tutmak istediğiniz dosyaların noktalı virgülle ayrılmış bir listesini sağlayın.
   2. İçinde **FromTarget** meta veri öğesi neden dosyaları, adı gibi dışlanan belirtmek için anlamlı bir değer sağlayın *. wpp.targets* dosya.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[Proje adı].wpp.targets* dosya artık benzemelidir bu:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Kaydet ve Kapat *[Proje adı].wpp.targets* dosya.

Sonraki kez, derleme ve paketleme, web uygulaması projenizin WPP otomatik olarak algılar *. wpp.targets* dosya. Tüm dosya ve klasörler, belirttiğiniz web pakete dahil edilmez.

## <a name="conclusion"></a>Sonuç

Bu konuda açıklanan özel bir oluşturarak bir web paketi oluşturduğunuzda, belirli dosyaları ve klasörleri dışlama *. wpp.targets* , web uygulama projesi dosyası ile aynı klasörde bir dosya.

## <a name="further-reading"></a>Daha Fazla Bilgi

Dağıtım işlemini denetlemek için özel Microsoft Build Engine (MSBuild) proje dosyalarını kullanma hakkında daha fazla bilgi için bkz. [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Paketleme ve dağıtım işlemi hakkında daha fazla bilgi için bkz. [oluşturma ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Web paketi dağıtımı için yapılandırma parametreleri](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), ve [ Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Önceki](deploying-membership-databases-to-enterprise-environments.md)
> [İleri](taking-web-applications-offline-with-web-deploy.md)
