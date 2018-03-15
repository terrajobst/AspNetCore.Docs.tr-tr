---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: "Veritabanı dağıtımları birden çok ortamlar için özelleştirme | Microsoft Docs"
author: jrjlee
description: "Bu konuda, belirli hedef ortamları için bir veritabanı özelliklerini dağıtım işleminin bir parçası uyarlamak açıklar. Not: Konu th varsayar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: f3ca344c2466d9d538f55cd8ff0a5bf5b7bac808
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Birden çok ortamlar için veritabanı dağıtımları özelleştirme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, belirli hedef ortamları için bir veritabanı özelliklerini dağıtım işleminin bir parçası uyarlamak açıklar.
> 
> > [!NOTE]
> > Konu MSBuild.exe ve VSDBCMD.exe kullanarak bir Visual Studio 2010 veritabanı projesi dağıttığınız varsayar. Bu yaklaşım neden seçebilirsiniz daha fazla bilgi için bkz: [Web dağıtımı kuruluştaki](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) ve [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Birden çok varış yeri için bir veritabanı projesi dağıttığınızda, genellikle her hedef ortam için veritabanı dağıtım özellikleri özelleştirmek istersiniz. Hazırlık veya üretim ortamlarında, verilerinizi korumak için artımlı güncelleştirmeler yapmak çok büyük olasılıkla olur ancak örneğin, test ortamlarında, genellikle her dağıtım veritabanını yeniden oluşturmanız.
> 
> Bir Visual Studio 2010 veritabanı projesi dağıtım ayarları bir dağıtım yapılandırma (.sqldeployment) dosyası içinde bulunur. Bu konuda ortama özgü dağıtım yapılandırma dosyaları oluşturup VSDBCMD parametre olarak kullanmak istediğinizi belirtmek nasıl yapacağınızı gösterir.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bu konuda, varsayılır:

- Bölünmüş proje dosyası yaklaşımı Çözüm dağıtımı açıklandığı gibi kullandığınız [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- VSDBCMD açıklandığı gibi veritabanı projenizi dağıtmanın proje dosyasından çağrılmasına [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Veritabanı dağıtım özellikleri hedef ortamlar arasında değişen destekleyen bir dağıtım sistemi oluşturmak için ihtiyacınız:

- Her hedef ortam için bir dağıtım yapılandırma (.sqldeployment) dosyası oluşturun.
- Dağıtım yapılandırma dosyası bir komut satırı anahtarını belirten bir VSDBCMD komutu oluşturun.
- Böylece VSDBCMD seçenekleri hedef ortam için uygun olan bir Microsoft Build Engine (MSBuild) proje dosyası VSDBCMD komutta Parametreleştirme.

Bu konuda her bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Ortama özgü dağıtım yapılandırma dosyaları oluşturma

Varsayılan olarak, bir veritabanı proje adlı bir tek dağıtım yapılandırma dosyası içerir. *Database.sqldeployment*. Visual Studio 2010'da bu dosyayı açarsanız, kullanabileceğiniz farklı dağıtım seçenekleri görebilirsiniz:

- **Dağıtım karşılaştırma harmanlama**. Bu, veritabanı harmanlaması projenizin kullanılıp kullanılmayacağını seçmesine olanak tanır ( *kaynak* harmanlama) veya veritabanı harmanlaması, hedef sunucunuzun ( *hedef* harmanlama). Çoğu durumda, sınama ortamında veya bir geliştirme dağıttığınızda kaynak harmanlaması kullanmak istersiniz. Bir hazırlık veya üretim ortamına dağıttığınızda, genellikle tüm birlikte çalışabilirlik sorunlarını önlemek için değişmeden hedef harmanlama bırakın istersiniz.
- **Veritabanı özellikleri dağıtma**. Bu veritabanı özellikleri uygulanıp uygulanmayacağını tanımlandığı şekilde seçmenizi sağlar *Database.sqlsettings* dosya. Bir veritabanını ilk kez dağıttığınızda, veritabanı özellikleri dağıtmanız gerekir. Varolan bir veritabanını güncelleştiriyorsanız özellikleri zaten yerinde olmalıdır ve onları yeniden dağıtmanız gerekmez.
- **Her zaman veritabanı yeniden oluşturma**. Hedef veritabanı dağıtmak ya da hedef veritabanı getirmek için artımlı şemanızı ile güncel değişiklik her zaman yeniden oluşturulup oluşturulmayacağını seçmek bu sağlar. Veritabanını yeniden oluşturursanız, var olan veritabanındaki tüm verileri kaybedersiniz. Bu nedenle, genellikle bu ayar **false** hazırlık veya üretim ortamları için dağıtımlar için.
- **Veri kaybı, artımlı dağıtım engelleme**. Bu dağıtım veritabanı şeması değişiklik veri kaybına neden olup olmayacağını durması gerektiğini olup olmadığını seçmenize olanak sağlar. Genellikle bu ayar **true** müdahale ve herhangi bir önemli veri koruma olanağı vermek için bir üretim ortamında, dağıtılacak. Ayarlamış olmanız durumunda **her zaman yeniden oluştur veritabanı** için **yanlış**, bu ayar hiçbir etkisi olmaz.
- **Dağıtım tek kullanıcı modunda çalıştır**. Bu genellikle geliştirme veya test ortamlarında bir sorun değildir. Bununla birlikte, genellikle bu ayarlamalıdır **true** hazırlık veya üretim ortamları için dağıtımlar için. Bu, kullanıcıların dağıtım devam ederken veritabanına değişiklikler yapmasını engeller.
- **Dağıtımdan önce veritabanı yedekleme**. Genellikle bu ayar **true** dağıtımını yaparken bir üretim ortamı için veri kaybına karşı önlem olarak. Ayarlamak isteyebilirsiniz **true** dağıtırken hazırlama ortamına hazırlama, veritabanınızı çok miktarda veri içeriyorsa.
- **Hedef veritabanına olan ancak veritabanı projesi içinde olmayan nesneler için bırak deyimleri oluşturmak**. Çoğu durumda, bu bir veritabanı için artımlı değişiklikler, tam sayı ve önemli bir bölümünü oluşturur. Ayarlamış olmanız durumunda **her zaman yeniden oluştur veritabanı** için **yanlış**, bu ayar hiçbir etkisi olmaz.
- **CLR Türleri güncelleştirmek için ALTER ASSEMBLY deyimleri kullanmayın**. Bu ayar, SQL Server ortak dil çalışma zamanı (CLR) türleri için yeni derleme sürümleri nasıl güncelleştirmelidir belirler. Bu ayarlanmalı **false** Çoğu senaryoda.

Bu tablo farklı bir hedef ortamlar için tipik dağıtım ayarlarını gösterir. Ancak, ayarlarınızı tam gereksinimlerinize bağlı olarak farklı olabilir.

|  | Geliştirme ve Test | Hazırlama/tümleştirme | Üretim |
| --- | --- | --- | --- |
| **Dağıtım karşılaştırma harmanlama** | Kaynak | Hedef | Hedef |
| **Veritabanı özellikleri dağıtma** | Doğru | Yalnızca ilk kez | Yalnızca ilk kez |
| **Veritabanı her zaman yeniden oluşturma** | Doğru | False | False |
| **Veri kaybı, artımlı dağıtım engelle** | False | Maybe | Doğru |
| **Dağıtım betiği tek kullanıcı modunda çalıştır** | False | Doğru | Doğru |
| **Dağıtımdan önce veritabanını yedekleyin** | False | Maybe | Doğru |
| **Hedef veritabanındaki nesneler için bırak deyimleri oluşturur, ancak veritabanı projesi içinde olmayan** | False | Doğru | Doğru |
| **CLR Türleri güncelleştirmek için ALTER ASSEMBLY deyimleri kullanmayın** | False | False | False |
  

> [!NOTE]
> Veritabanı dağıtım özellikleri ve ortam konuları hakkında daha fazla bilgi için bkz: [bir genel bakış, veritabanı proje ayarları](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [nasıl yapılır: özelliklerini yapılandırmak için dağıtım ayrıntıları](https://msdn.microsoft.com/library/dd172125.aspx), [ Derleme ve veritabanı bir yalıtılmış geliştirme ortamına dağıtma](https://msdn.microsoft.com/library/dd193409.aspx), ve [derleme ve veritabanları bir hazırlık veya üretim ortamına dağıtma](https://msdn.microsoft.com/library/dd193413.aspx).


Birden çok varış yeri için veritabanı projesi'nın dağıtımını desteklemek için her hedef ortam için bir dağıtım yapılandırma dosyası oluşturmanız gerekir.

**Bir ortama özgü yapılandırma dosyası oluşturmak için**

1. Visual Studio 2010 içinde **Çözüm Gezgini** penceresi, veritabanı projenize sağ tıklayın ve ardından **özellikleri**.
2. Veritabanı Proje özellikleri sayfasında üzerinde **dağıtma** sekmesinde **dağıtım yapılandırma dosyası** satır, tıklatın **yeni**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. İçinde **yeni bir dağıtım yapılandırma dosyası** iletişim kutusunda, dosya anlamlı bir ad verin (örneğin, **TestEnvironment.sqldeployment**) ve ardından **kaydetmek**.
4. Üzerinde *[Filename] *** .sqldeployment** sayfasında, hedef ortamınızın gereksinimlerini karşılamak için dağıtım özellikleri ayarlayın ve ardından dosyayı kaydedin.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Yeni dosya, veritabanı projenizi özellikleri klasöründe eklenir dikkat edin.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Dağıtım yapılandırma dosyası VSDBCMD içinde belirtme

Visual Studio 2010 içinden çözüm yapılandırmaları (örneğin, hata ayıklama ve yayın) kullandığınızda, bir dağıtım yapılandırma dosyası her yapılandırma ile ilişkilendirebilirsiniz. Belirli bir yapılandırmasının derlerken, derleme işlemi özel yapılandırma dağıtım yapılandırma dosyasına işaret özel yapılandırma dağıtım bildirim dosyası oluşturur. Ancak, bu öğreticileri açıklanan dağıtıma yaklaşımın ana amaçlar kişiler dağıtım işlemi Visual Studio 2010 ve çözüm yapılandırmaları kullanmadan denetleme olanağı vermek için biridir. Bu yaklaşımda, çözüm yapılandırması hedef dağıtım ortamı bakılmaksızın aynı olacak. Veritabanı dağıtımınız belirli hedef ortamınıza uyarlamak için dağıtım yapılandırma dosyası belirtmek için VSDBCMD komut satırı seçeneklerini kullanabilirsiniz.

Bir dağıtım yapılandırma dosyası, VSDBCMD belirtmek için kullanın **p:/DeploymentConfigurationFile** geçin ve dosyanızı tam yolunu girin. Bu dağıtım bildirimi tanımlayan dağıtım yapılandırma dosyası geçersiz kılar. Örneğin, dağıtmak için bu VSDBCMD komutu kullanabilirsiniz **ContactManager** veritabanı bir test ortamı için:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Çıktı dizinine dosya kopyalarken yapı işlemi .sqldeployment dosyanızı adlandırın unutmayın.


Dağıtım öncesi veya dağıtım sonrası SQL komut dosyalarınızda SQL komutu değişkenleri kullanıyorsanız bir ortama özgü .sqlcmdvars dosyasını dağıtımınız ile ilişkilendirmek için benzer bir yaklaşım kullanabilirsiniz. Bu durumda, kullandığınız **p:/SqlCommandVariablesFile** .sqlcmdvars dosyanızı tanımlamak için anahtar.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>MSBuild proje dosyasından VSDBCMD komutunu çalıştırma

Kullanarak bir MSBuild proje dosyası VSDBCMD komuttan çağırabileceği bir **Exec** MSBuild hedef içinde görev. An basit haliyle, onu şöyle olabilir:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- Uygulamada, okuyup yeniden, proje dosyalarınıza kolaylaştırmak için çeşitli komut satırı parametreleri depolamak için özellikler oluşturmak istediğiniz. Bu, kullanıcıların bir ortama özgü proje dosyasında özellik değerlerini sağlamak ya da MSBuild komut satırından varsayılan değerleri geçersiz kılmak için kolaylaştırır. Açıklanan bölünmüş proje dosyası yaklaşımı kullanırsanız [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), yapı yönergeler ve iki dosya arasında özellikleri buna göre bölme:
- Dağıtım yapılandırma dosya adı, veritabanı bağlantı dizesi ve hedef veritabanı adı gibi ortama özgü ayarları ortama özgü proje dosyasında gitmeniz gerekir.
- VSDBCMD yürütülebilir dosyasının konumuna gibi Evrensel tüm özellikleri ile birlikte VSDBCMD komutu çalıştırır MSBuild hedef Evrensel proje dosyasında gitmeniz gerekir.

Ayrıca, .deploymanifest dosya oluşturulur ve kullanıma hazır olmasını sağlamak VSDBCMD çağırma önce veritabanı projesi derleme de emin olmalısınız. Bu yaklaşımın konusunda tam bir örnek görebilirsiniz [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), hangi kılavuzluk, proje dosyalarında [Contact Manager örnek çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Sonuç

Bu konuda nasıl MSBuild ve VSDBCMD kullanarak veritabanı projeleri dağıtırken, farklı bir hedef ortamlar için veritabanı özellikleri uyarlayabileceğiniz açıklanmaktadır. Veritabanı projeleri büyük, Kurumsal ölçekte çözümleri bir parçası olarak dağıtmak gerektiğinde bu yararlı bir yaklaşımdır. Bu çözümler genellikle korumalı geliştirme veya test ortamları, hazırlama ya da tümleştirme Platform ve üretim veya dinamik ortamları gibi birden çok varış yeri için dağıtılır. Bu hedef ortamların her birinde, genellikle benzersiz bir veritabanı dağıtım özellikleri kümesi gerektirir.

## <a name="further-reading"></a>Daha Fazla Bilgi

Veritabanı projeleri VSDBCMD.exe kullanarak dağıtma hakkında daha fazla bilgi için bkz: [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md). Dağıtım işlemi denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Bu makaleler MSDN'de veritabanı dağıtımı hakkında daha fazla genel rehberlik sağlar:

- [Veritabanı projesi ayarlarına genel bakış](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Nasıl yapılır: özellikler için dağıtım ayrıntılarını Yapılandır](https://msdn.microsoft.com/library/dd172125.aspx)
- [Derleme ve veritabanları bir yalıtılmış geliştirme ortamına dağıtma](https://msdn.microsoft.com/library/dd193409.aspx)
- [Derleme ve veritabanları bir hazırlık veya üretim ortamına dağıtma](https://msdn.microsoft.com/library/dd193413.aspx)

>[!div class="step-by-step"]
[Önceki](performing-a-what-if-deployment.md)
[sonraki](deploying-database-role-memberships-to-test-environments.md)
