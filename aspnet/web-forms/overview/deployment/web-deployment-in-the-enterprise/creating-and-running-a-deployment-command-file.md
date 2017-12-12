---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "Oluşturma ve çalıştırma bir dağıtım komut dosyası | Microsoft Docs"
author: jrjlee
description: "Bu konuda re bir tek adım olarak Microsoft Build Engine (MSBuild) proje dosyalarını kullanarak bir dağıtımını çalıştırmak olanak tanıyan bir komut dosyasının nasıl oluşturulacağı açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 729b4fa4c461eedbd0447371102010451eb51586
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-running-a-deployment-command-file"></a>Oluşturma ve bir dağıtım komut dosyası çalıştırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda adım tek, tekrarlanabilir bir işlem olarak Microsoft Build Engine (MSBuild) proje dosyalarını kullanarak bir dağıtım çalıştırmadan sağlayacak bir komut dosyasının nasıl oluşturulacağı açıklanmaktadır.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanan [Contact Manager](the-contact-manager-solution.md) çözüm & #x 2014; Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [oluşturma işlemini anlama](understanding-the-build-process.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="process-overview"></a>İşlemine genel bakış

Bu konuda, oluşturma ve bu proje dosyalarını hedef ortamınızı tekrarlanabilir bir dağıtımına gerçekleştirmek için kullandığı bir komut dosyası çalıştırma öğreneceksiniz. Esas olarak, komut dosyasının yalnızca bir MSBuild komut içermesi gerekir:

- Ortam belirsiz yürütmek için MSBuild söyler *Publish.proj* dosya.
- Söyler *Publish.proj* dosya hangi dosya ortama özgü proje ayarlarını ve nerede bulacağını içerir.

## <a name="create-an-msbuild-command"></a>MSBuild komut oluşturma

Bölümünde açıklandığı gibi [oluşturma işlemini anlama](understanding-the-build-process.md), ortama özgü proje dosyası & #x 2014; Örneğin, *Env Dev.proj*& #x 2014; içe için tasarlanmıştır ortam belirsiz *Publish.proj* derleme zamanında dosya. Birlikte, bu iki dosyayı nasıl oluşturulacağı ve çözümünüzü dağıtmak MSBuild söyleyen yönergeler eksiksiz bir kümesini sağlar.

*Publish.proj* dosya kullanan bir **alma** ortama özgü proje dosyasını içeri aktarmak için öğesi.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Bu nedenle, yapı ve Contact Manager çözümü dağıtmak için MSBuild.exe kullandığınızda için gerekir:

- MSBuild.exe çalıştıracağınız *Publish.proj* dosya.
- Adlı bir komut satırı parametresini sağlayarak ortama özgü proje dosyası konumunu belirtin **TargetEnvPropsFile**.

Bunu yapmak için MSBuild komutunuzu şuna benzemelidir:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Buradan, yinelenebilir, tek adımlı dağıtımına taşımanızı basit adımdır. Yapmanız gereken tek şey MSBuild komutunuzu .cmd dosyasına eklemek için. Contact Manager çözümde Yayımla klasör adında bir dosya içerir *Yayımla Dev.cmd* , tam olarak bunu yapar.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> **/Fl** anahtar bildirir adlı bir günlük dosyası oluşturmak için MSBuild *msbuild.log* MSBuild.exe çağrıldığı çalışma dizininde.


Dağıtımı veya Contact Manager çözümü yeniden dağıtmak için tüm yapmanız gereken çalıştırıldığında *Yayımla Dev.cmd* dosya. Dosyayı çalıştırdığınızda MSBuild olur:

- Çözümdeki tüm projeleri oluşturun.
- Web uygulama projeleri için dağıtılabilir web paketleri oluşturun.
- Veritabanı projeleri için .dbschema ve .deploymanifest dosyaları oluşturur.
- Web paketleri web sunucusuna dağıtın.
- Veritabanı için veritabanı sunucusu dağıtın.

## <a name="run-the-deployment"></a>Dağıtımı çalıştırın

Hedef ortamınız için bir komut dosyasını oluşturduğunuz zaman, tüm dağıtım dosyasını çalıştırarak tamamlaması mümkün olması gerekir.

**Test ortamınızı Contact Manager çözümü dağıtmak için**

1. Geliştirici istasyonunuzda Windows Gezgini'ni açın ve dosyanın konumuna göz atın *Yayımla Dev.cmd* dosya.
2. Çalıştırmak için dosyaya çift tıklayın.
3. Varsa bir **açık dosya – Güvenlik Uyarısı** iletişim kutusu görüntülenirse, tıklatın **çalıştırmak**.
4. Yapılandırma ayarlarını ve test sunucularına ayarlanması, komut istemi penceresini doğru gösterecektir bir **yapı başarılı** MSBuild proje dosyalarını işlemeyi tamamladığında iletisi.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Bu ortam için çözüm dağıtmış ilk kez kullanıyorsanız, test web sunucusunun makine hesabı eklemeniz gerekir **db\_datawriter** ve **db\_datareader**rollerinde **ContactManager** veritabanı. Bu yordamda açıklanan [bir veritabanı sunucusu için Web dağıtımı yayımlama yapılandırma](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Yalnızca veritabanı oluşturduğunuzda, bu izinleri atamanız gerekir. Varsayılan olarak, yapı işlemi her dağıtım & #x 2014 veritabanını yeniden değil; bunun yerine, en son şema varolan bir veritabanına karşılaştırın ve yalnızca gerekli değişiklikleri yapın. Sonuç olarak, bu veritabanı rolleri çözümü dağıtmak ilk kez eşlemek yalnızca gerekir.
6. Internet Explorer'ı açın ve ilgili kişi Yöneticisi uygulamasının URL'sine gidin (örneğin, `http://testweb1:85/ContactManager/`).
7. Uygulamanın beklendiği gibi çalıştığını doğrulayın ve kişiler ekleyebilecek.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Sonuç

MSBuild yönelik yönergeler içeren bir komut dosyası oluşturma oluşturma ve belirli bir hedef ortam için birden çok proje çözümü hızlı ve kolay bir yol sağlar. Çözümünüzü birden çok hedef ortamlar için sürekli olarak dağıtmak gerekiyorsa, birden çok komut dosyası oluşturabilirsiniz. Her komut dosyası MSBuild komut aynı Evrensel proje dosyası oluşturur, ancak farklı ortama özgü proje dosyası belirtmeniz gerekecektir. Örneğin, bir geliştirici yayımlamak veya test ortamı için bir komut dosyası bu MSBuild komut içerebilir:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Hazırlama ortamına yayımlamak için bir komut dosyası bu MSBuild komut içerebilir:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Kendi sunucu ortamları için ortama özgü proje dosyalarını özelleştirme konusunda yönergeler için bkz [dağıtım özelliklerini yapılandırmak için bir hedef ortam](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Her ortam için yapı işlemi özelliklerini geçersiz kılma veya diğer çeşitli anahtarları MSBuild komutunuzu ayarlayarak özelleştirebilirsiniz. Daha fazla bilgi için bkz: [MSBuild komut satırı başvurusu](https://msdn.microsoft.com/en-us/library/ms164311.aspx).

>[!div class="step-by-step"]
[Önceki](deploying-database-projects.md)
[sonraki](manually-installing-web-packages.md)
