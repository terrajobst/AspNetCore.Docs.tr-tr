---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Dosya ve klasörleri dışarıda dağıtımından | Microsoft Docs
author: jrjlee
description: Bu konu, yapı ve bir web uygulaması projesi paketini nasıl, dosya ve klasörleri web dağıtım paketinden hariç tutabilirsiniz açıklar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: c435448bf057bbef9127d66ffda24a07729f2322
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="excluding-files-and-folders-from-deployment"></a>Dosya ve klasörleri dışarıda dağıtımından
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, yapı ve bir web uygulaması projesi paketini nasıl, dosya ve klasörleri web dağıtım paketinden hariç tutabilirsiniz açıklar.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="overview"></a>Genel Bakış

Visual Studio 2010 bir web uygulaması projesi oluşturduğunuzda, derlenmiş bir web uygulamanıza dağıtılabilir web paketi paketleyerek bu derleme işlemini genişletme Web yayımlama ardışık düzen (WPP) sağlar. Uzaktaki bir IIS web sunucusunda bu web paketini dağıtmak için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanın veya el ile IIS Yöneticisi üzerinden web paketini içeri aktarın. Bu Paketleme işlemini açıklandığı [bina ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Böylece nasıl web içinde yer, kontrol paketini? Visual Studio Proje ayarları temel alınan proje dosyası aracılığıyla çok sayıda senaryoları için yeterli denetim sağlar. Ancak, bazı durumlarda, belirli bir hedef ortamları web paketinin içeriğini uyarlamak isteyebilirsiniz. Örneğin, uygulamanızı bir sınama ortamında dağıtmak ancak bir hazırlık veya üretim ortamı için uygulamayı dağıttığınızda klasörü dışlamak günlük dosyaları için bir klasör eklemek isteyebilirsiniz. Bu konu bunun nasıl yapılacağını gösterir.

## <a name="what-gets-included-by-default"></a>Varsayılan olarak içereceği?

Visual Studio'da web uygulaması proje özelliklerini yapılandırırken **dağıtmak için öğeleri** listesini **Paketle/Yayımla Web** sayfa web dağıtımınızda dahil edilmesini istediğiniz belirtmenize olanak sağlar Paket. Varsayılan olarak, bu ayarlamak **yalnızca bu uygulamayı çalıştırmak için gerekli dosyaları**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Seçtiğinizde **yalnızca bu uygulamayı çalıştırmak için gerekli dosyaları**, hangi dosyaların web pakete eklenmesi gerektiğini belirlemek WPP çalışacaktır. Şunları içerir:

- Tüm yapı projesi için çıkarır.
- Herhangi bir dosya olarak işaretlenmiş bir yapı eylemi ile **içerik**.

> [!NOTE]
> Dahil etmek için hangi dosyaların belirleyen mantık bu dosyada yer alır:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Belirli dosya ve klasörleri dışarıda

Bazı durumlarda, dosyalar ve klasörler üzerinde dağıtılan daha ayrıntılı denetim isteyeceksiniz. Hangi dosyaların ilerisinde hariç tutmak istediğiniz zaman, bildiğiniz ve dışlama tüm hedef ortamlar için geçerlidir, yalnızca ayarlayabilirsiniz **yapı eylemi** her dosyanın **hiçbiri**.

**Belirli dosyaları dağıtımından dışlamak için**

1. İçinde **Çözüm Gezgini** penceresinde, dosyaya sağ tıklayın ve ardından **özellikleri**.
2. İçinde **özellikleri** penceresi, **yapı eylemi** satır, select **hiçbiri**.

Ancak, bu yaklaşım her zaman uygun değil. Örneğin, hangi dosyaların farklılık isteyebilirsiniz ve klasörleri hedef ortamınıza göre ve dış Visual Studio'dan dahil edilir. Örneğin, kişinin Yöneticisi örnek çözümü ContactManager.Mvc proje içeriğini göz atın:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- İç klasör oluşturma, bırakma ve geliştirme amacıyla yerel veritabanlarını doldurmak için geliştirici kullanan bazı SQL komut dosyaları içerir. Bu klasördeki bir şey bir hazırlık veya üretim ortamına dağıtılmalıdır.
- Betikler klasörüne birkaç JavaScript dosyaları içerir. Yalnızca hata ayıklamayı desteklemek ya da Visual Studio IntelliSense sağlamak için bu dosyaların çok eklenmemesi. Bu dosyaların bazıları hazırlık veya üretim ortamları için kullanılmamalıdır. Ancak, sorun giderme kolaylaştırmak için geliştirici test ortamı dağıtmak isteyebilirsiniz.

Belirli dosyaları ve klasörleri dışlamak için proje dosyalarınıza düzenlersiniz rağmen daha kolay bir yolu yoktur. Adlı öğe listelerini oluşturarak dosya ve klasörleri dışlamak için bir mekanizma WPP içeren **ExcludeFromPackageFolders** ve **ExcludeFromPackageFiles**. Bu listelerden kendi öğeleri ekleyerek bu düzenek genişletebilirsiniz. Bunu yapmak için üst düzey adımları tamamlamanız gerekir:

1. Adlı bir özel proje dosyası oluşturma *[Proje adı].wpp.targets* proje dosyası ile aynı klasörde.

    > [!NOTE]
    > *. Wpp.targets* dosyasının gerekir, web uygulaması proje dosyası ile aynı klasörde gitmek&#x2014;Örneğin, *ContactManager.Mvc.csproj*&#x2014;yerine herhangi bir özel ile aynı klasörde Proje dosyalarını derleme ve dağıtım işlemini kontrol eden kullanın.
2. İçinde *. wpp.targets* dosya, ekleme bir **ItemGroup** öğesi.
3. İçinde **ItemGroup** öğesi ekleme **ExcludeFromPackageFolders** ve **ExcludeFromPackageFiles** belirli dosyaları ve klasörleri gerektiği gibi dışlamak için öğeleri.

Bu temel yapısını budur *. wpp.targets* dosyası:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Her öğe adlı bir öğe meta veri öğesi içerdiğini unutmayın **FromTarget**. Derleme işlemi etkilemez isteğe bağlı bir değerdir; yalnızca belirli dosyaları veya klasörleri neden atlanmış belirtmek için sunduğu birisi yapı günlükleri incelemeleri durumunda.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Bir Web paketinden dosyaları ve klasörleri dışarıda

Sonraki yordam nasıl ekleneceğini gösterir bir *. wpp.targets* dosyasına bir web uygulaması projesi ve projenizi yapılandırdığınızda web paketinden belirli dosyaları ve klasörleri dışarıda bırakılacak dosya kullanma.

**Web dağıtım paketinden dosyaları ve klasörleri dışlamak için**

1. Çözümünüzü Visual Studio 2010'da açın.
2. İçinde **Çözüm Gezgini** penceresinde, web uygulama projesi düğümüne sağ tıklayın (örneğin, **ContactManager.Mvc**), işaret **Ekle**ve ardından **Yeni öğe**.
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **XML dosyası** şablonu.
4. İçinde **adı** kutusuna *[Proje adı] ***.wpp.targets** (örneğin, **ContactManager.Mvc.wpp.targets**) ve ardından **Ekle**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Yeni bir öğe bir proje kök düğüme eklerseniz, dosya proje dosyası ile aynı klasörde oluşturulur. Bu klasörü Windows Gezgini'nde açarak doğrulayabilirsiniz.
5. Dosyasına ekleyin bir **proje** öğesi ve bir **ItemGroup** öğe:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Web paketinden klasörleri dışlamak istiyorsanız, eklemeniz bir **ExcludeFromPackageFolders** öğesine **ItemGroup** öğe:

   1. İçinde **INCLUDE** özniteliği, dışlamak istediğiniz klasörleri noktalı virgülle ayrılmış bir listesini sağlayın.
   2. İçinde **FromTarget** meta veri öğesi neden klasörleri, adı gibi dışlanan belirtmek için anlamlı bir değer sağlayın *. wpp.targets* dosya.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Web paketinden dosyaları hariç tutmak istiyorsanız, ekleyin bir **ExcludeFromPackageFiles** öğesine **ItemGroup** öğe:

   1. İçinde **INCLUDE** özniteliği, hariç tutmak istediğiniz dosyaları noktalı virgülle ayrılmış bir listesini sağlayın.
   2. İçinde **FromTarget** meta veri öğesi neden dosyaları, adı gibi dışlanan belirtmek için anlamlı bir değer sağlayın *. wpp.targets* dosya.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. *[Proje adı].wpp.targets* dosya şimdi benzer bu:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Kaydet ve Kapat *[Proje adı].wpp.targets* dosya.

Sonraki başlatışınızda yapı ve paket, web uygulama projesi, WPP otomatik olarak algılar *. wpp.targets* dosya. Tüm dosya ve klasörler, belirttiğiniz web pakete dahil edilmez.

## <a name="conclusion"></a>Sonuç

Özel bir oluşturarak web paketi oluşturduğunuzda belirli dosyaları ve klasörleri dışarıda bırak nasıl bu konuda açıklanan *. wpp.targets* , web uygulaması proje dosyası ile aynı klasörde dosya.

## <a name="further-reading"></a>Daha Fazla Bilgi

Dağıtım işlemi denetlemek için özel Microsoft Build Engine (MSBuild) proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Paketleme ve dağıtım işlemi hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Web paketi dağıtımı için yapılandırma parametreleri](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), ve [ Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Önceki](deploying-membership-databases-to-enterprise-environments.md)
> [sonraki](taking-web-applications-offline-with-web-deploy.md)
