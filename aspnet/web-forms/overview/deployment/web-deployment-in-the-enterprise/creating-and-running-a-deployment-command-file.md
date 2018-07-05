---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Oluşturma ve çalıştırma bir dağıtım komut dosyası | Microsoft Docs
author: jrjlee
description: Bu konuda re bir tek adım olarak Microsoft Build Engine (MSBuild) proje dosyalarını kullanarak dağıtım çalıştırmanıza izin veren bir komut dosyasının nasıl oluşturulacağı açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395379"
---
<a name="creating-and-running-a-deployment-command-file"></a>Oluşturma ve dağıtım komut dosyası çalıştırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda adım tek, tekrarlanabilir bir işlem olarak Microsoft Build Engine (MSBuild) proje dosyalarını kullanarak dağıtım çalıştırmanıza izin veren bir komut dosyası nasıl oluşturulduğu açıklanır.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi](the-contact-manager-solution.md) çözüm&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [derleme işlemini anlama](understanding-the-build-process.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="process-overview"></a>İşleme genel bakış

Bu konu başlığında, oluşturmayı ve tekrarlanabilir bir dağıtım için hedef ortamınız gerçekleştirmek için bu proje dosyalarının kullanan bir komut dosyasını çalıştırmayı öğreneceksiniz. Esas olarak, komut dosyası yalnızca bir MSBuild komut içerecek şekilde erişmesi:

- Ortam bağımsız yürütmek için MSBuild söyler *Publish.proj* dosya.
- Söyler *Publish.proj* hangi dosya nerede bulacağını ve ortama özgü proje ayarları içeren dosya.

## <a name="create-an-msbuild-command"></a>MSBuild komut oluşturma

Bölümünde anlatıldığı gibi [derleme işlemini anlama](understanding-the-build-process.md), ortama özgü proje dosyası&#x2014;gibi *Env Dev.proj*&#x2014;ortam geçişte sorun yaşamaz alınmak üzere tasarlanmıştır *Publish.proj* dosya derleme zamanında. Birlikte, bu iki dosyayı oluşturmak ve çözümünüzü dağıtmak nasıl MSBuild söyleyen yönergeler eksiksiz bir kümesini sağlar.

*Publish.proj* dosyası kullanan bir **alma** ortama özgü proje dosyasını içeri aktarmak için öğesi.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Kişi Yöneticisi çözümü oluşturup dağıtmaya için MSBuild.exe kullandığınızda, bu nedenle, şunları yapmanız gerekir:

- MSBuild.exe çalıştıracağınız *Publish.proj* dosya.
- Adlı bir komut satırı parametresi sağlayarak ortama özgü proje dosyasının konumunu belirtin **TargetEnvPropsFile**.

Bunu yapmak için MSBuild komut şuna benzemelidir:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Buradan, tekrarlanabilir, tek adımlı dağıtımına taşıyın basit adımdır. Tek yapmak için ihtiyacınız olan, MSBuild komut .cmd dosyasına ekleme. Kişi Yöneticisi çözümde yayımlama klasörü adlı bir dosya içerir. *Yayımla Dev.cmd* tam olarak bunu yapar.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl** anahtar bildirir adlandırılmış bir günlük dosyası oluşturmak için MSBuild *msbuild.log* çalışma dizininde MSBuild.exe çağrıldı.


Dağıtma veya kişi yöneticisi çözümü yeniden dağıtmak için tüm yapmanız gereken çalıştırılır *Yayımla Dev.cmd* dosya. MSBuild, dosyanın çalıştırdığınızda, olur:

- Çözümdeki tüm projeleri oluşturun.
- Web uygulama projeleri için dağıtılabilen web paketleri oluşturun.
- Veritabanı projeleri için .dbschema ve .deploymanifest dosyaları oluşturur.
- Web paketleri web sunucusuna dağıtın.
- Veritabanı, veritabanı sunucusuna dağıtın.

## <a name="run-the-deployment"></a>Dağıtımı çalıştırın

Hedef ortamınız için bir komut dosyası oluşturduktan sonra dosyayı çalıştırarak tüm dağıtım tamamlaması mümkün olması gerekir.

**Kişi Yöneticisi çözümü test ortamınıza dağıtmak için**

1. Geliştirici iş istasyonunda, Windows Gezgini'ni açın ve ardından konumuna göz atın *Yayımla Dev.cmd* dosya.
2. Çalıştırmak için dosyaya çift tıklayın.
3. Varsa bir **açık dosya – Güvenlik Uyarısı** iletişim kutusu görüntülendikten sonra **çalıştırma**.
4. Yapılandırma ayarları ve test sunucuları ayarlanıp ayarlanmadığını doğru komut istemi penceresini gösterir bir **oluşturma başarılı oldu** MSBuild proje dosyaları işlemeyi tamamladığında iletisi.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Bu ortama çözüm dağıtmış olduğunuz ilk kez buysa, test web sunucusu makine hesabı eklemeniz gerekecektir **db\_datawriter** ve **db\_datareader**rollerinde **ContactManager** veritabanı. Bu yordamda açıklanan [bir veritabanı sunucusu için Web dağıtımı yayımlama yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Yalnızca veritabanı oluşturduğunuzda, bu izinleri atamanız gerekir. Varsayılan olarak, derleme işleminin her dağıtım veritabanını yeniden oluşturur değil&#x2014;bunun yerine, bu varolan veritabanını en son Şema karşılaştırma ve yalnızca gerekli değişiklikleri yapın. Sonuç olarak, bu veritabanı rolleri çözümü dağıttıktan ilk kez eşlemek yalnızca gerekir.
6. Kişi Yöneticisi uygulama URL'sine göz atın ve Internet Explorer'ı açın (örneğin, `http://testweb1:85/ContactManager/`).
7. Uygulamanın beklendiği gibi çalıştığını doğrulayın ve kişiler ekleyebilirsiniz.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Sonuç

MSBuild talimatları içeren bir komut dosyası oluşturuluyor derleyip bir belirli hedef ortam için çok projeli bir çözüm dağıtmayı hızlı ve kolay bir yol sağlar. Art arda birden çok hedef ortama çözümünüzü dağıtmak gerekiyorsa, birden çok komut dosyaları oluşturabilirsiniz. Her komut dosyası, MSBuild komut aynı Evrensel bir proje dosyası oluşturur, ancak farklı bir ortama özgü proje dosyası belirteceksiniz. Örneğin, bir komut dosyası için bir geliştirici yayımlamak veya test ortamı için bu MSBuild komut içerebilir:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Bir komut dosyası hazırlama ortamına yayımlamak için bu MSBuild komut içerebilir:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Kendi server ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz. [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Özelliklerini geçersiz kılma veya çeşitli diğer anahtarlar, MSBuild komut ayarlayarak, her ortam için yapı işlemini de özelleştirebilirsiniz. Daha fazla bilgi için [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Önceki](deploying-database-projects.md)
> [İleri](manually-installing-web-packages.md)
