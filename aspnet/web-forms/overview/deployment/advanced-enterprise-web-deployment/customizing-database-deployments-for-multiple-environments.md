---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Birden çok ortam için veritabanı dağıtımlarını özelleştirme | Microsoft Docs
author: jrjlee
description: 'Bu konu, bir veritabanına belirli bir hedef ortam özelliklerini dağıtım işleminin bir parçası olarak uyarlamak açıklar. Not: Konu th varsayar...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 3a368e5055f4921b68f5c5eb2739728a2f1fd4d2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378079"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Birden çok ortam için veritabanı dağıtımlarını özelleştirme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, bir veritabanına belirli bir hedef ortam özelliklerini dağıtım işleminin bir parçası olarak uyarlamak açıklar.
> 
> > [!NOTE]
> > Bu konuda, MSBuild.exe ve VSDBCMD.exe kullanarak bir Visual Studio 2010 veritabanı projesi dağıtıyorsanız varsayılır. Neden bu yaklaşımı tercih edebileceğiniz daha fazla bilgi için bkz: [Kurumsal Web dağıtımı](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) ve [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Birden çok hedefe bir veritabanı projesi dağıttığınızda, genellikle her hedef ortam için veritabanı dağıtım özellikleri özelleştirmek isteyebilirsiniz. Hazırlık veya üretim ortamlarında, verilerinizi korumak için artımlı güncelleştirmeler yapmak çok daha büyük olasılıkla olacaktır ancak örneğin, test ortamlarında, genellikle her dağıtım veritabanında yeniden.
> 
> Visual Studio 2010 veritabanı projesi, dağıtım ayarları bir dağıtım yapılandırma (.sqldeployment) dosyası içinde yer alır. Bu konuda, ortama özgü dağıtım yapılandırma dosyaları oluşturma ve VSDBCMD parametre olarak kullanmak istediğiniz bir tane belirtmeniz gösterilmektedir.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bu konu başlığı altında olduğunu varsayar:

- Çözüm dağıtımı, bölünmüş proje dosyası yaklaşımı açıklandığı gibi kullandığınız [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Veritabanı projenizi dağıtmak için proje dosyasından VSDBCMD açıklandığı çağrı [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Veritabanı dağıtım özelliklerini hedef ortamlar arasında değişen destekleyen bir dağıtım sistemi oluşturmak için ihtiyacınız:

- Her hedef ortam için dağıtım yapılandırma (.sqldeployment) dosyası oluşturun.
- Dağıtım yapılandırma dosyası bir komut satırı anahtarı belirten bir VSDBCMD komutu oluşturun.
- VSDBCMD seçenekleri hedef ortam için uygun olacak şekilde bir Microsoft Build Engine (MSBuild) proje dosyasında VSDBCMD komut parametreleştirin.

Bu konuda, bu yordamların her biri gerçekleştirme gösterilmektedir.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Ortama özgü dağıtım yapılandırma dosyaları oluşturma

Varsayılan olarak, bir veritabanı projesi adlı tek bir dağıtım yapılandırma dosyasını içeren *Database.sqldeployment*. Visual Studio 2010'da bu dosyayı açmak için kullanabileceğiniz farklı dağıtım seçenekleri görebilirsiniz:

- **Dağıtım karşılaştırma harmanlama**. Bu veritabanı harmanlaması projenizin kullanılıp kullanılmayacağını seçmenize olanak tanır ( *kaynak* harmanlama) ya da hedef veritabanı harmanlaması ( *hedef* harmanlama). Çoğu durumda, bir geliştirme dağıttığınızda veya test ortamı kaynak harmanlaması kullanmak isteyebilirsiniz. Bir hazırlık veya üretim ortamına dağıtma yaparken, genellikle hedef harmanlamaya birlikte çalışabilirlik sorunları önlemek için değiştirilmemiş şekilde bırakmak istersiniz.
- **Veritabanı özellikleri dağıtma**. Bu veritabanı özelliklerini uygulanıp uygulanmayacağını tanımlandığı şekilde seçmenizi sağlar *Database.sqlsettings* dosya. Bir veritabanı ilk kez dağıttığınızda, veritabanı özelliklerini dağıtmanız gerekir. Varolan bir veritabanını güncelleştiriyorsanız özellikleri zaten yerinde olmalıdır ve yeniden dağıtmanız gerekmez.
- **Veritabanı her zaman yeniden oluştur**. Hedef veritabanı dağıtma veya hedef veritabanı getirmek için artımlı şemanızı ile güncel değişiklikler her zaman yeniden oluşturulup oluşturulmayacağını seçin olanak sağlar. Veritabanı yeniden oluşturursanız, tüm veriler var olan veritabanını kaybedersiniz. Bu nedenle, genellikle bu ayar **false** dağıtımları için hazırlık veya üretim ortamları için.
- **Veri kaybı, artımlı dağıtım Block**. Bu dağıtım veritabanı şeması değişiklik veri kaybına neden olacaksa durdurulup durdurulmayacağını seçmenize olanak sağlar. Genellikle bu ayar **true** müdahale ve tüm önemli verileri korumaya fırsatı sunmak için bir üretim ortamında dağıtılacak. Ayarladıysanız **her zaman yeniden oluştur veritabanı** için **false**, bu ayarın hiçbir etkisi olmaz.
- **Tek kullanıcı modunda dağıtım yürütün**. Bu genellikle geliştirme ve test ortamları bir sorun değildir. Ancak, genellikle bu ayar **true** dağıtımları için hazırlık veya üretim ortamları için. Bu, kullanıcıların dağıtım devam ederken veritabanına değişiklikler yapmasını engeller.
- **Dağıtımdan önce veritabanını yedekleme**. Genellikle bu ayar **true** veri kaybına karşı önlem olarak, bir üretim ortamına dağıtırken. Ayarlayın isteyebilirsiniz **true** dağıtırken bir hazırlama ortamına hazırlama veritabanınız çok fazla veri içeriyorsa.
- **Hedef veritabanında olan ancak bir veritabanı projesinde olmayan nesneler için bırak deyimleri oluşturmasını**. Çoğu durumda, bir veritabanına artımlı değişiklikler yapma integral ve önemli bir bölümü budur. Ayarladıysanız **her zaman yeniden oluştur veritabanı** için **false**, bu ayarın hiçbir etkisi olmaz.
- **CLR türlerini güncelleştirmek için ALTER ASSEMBLY deyimleri kullanmayın**. Bu ayar, SQL Server ortak dil çalışma zamanı (CLR) türleri için yeni derleme sürümlerini nasıl güncelleştirmelisiniz belirler. Bu ayarlanmalıdır **false** Çoğu senaryoda.

Bu tabloda, farklı bir hedef ortamları için tipik dağıtım ayarları gösterilmektedir. Ancak, ayarlarınızı tam gereksinimlerinize bağlı olarak farklı olabilir.

|  | Geliştirici/Test | Hazırlama/tümleştirme | Üretim |
| --- | --- | --- | --- |
| **Dağıtım karşılaştırma harmanlama** | Kaynak | Hedef | Hedef |
| **Veritabanı özellikleri dağıtma** | Doğru | Yalnızca ilk kez | Yalnızca ilk kez |
| **Veritabanı her zaman yeniden oluştur** | Doğru | False | False |
| **Veri kaybı, artımlı dağıtım engelle** | False | Belki de | Doğru |
| **Tek kullanıcı modunda dağıtım betiği yürütün** | False | Doğru | Doğru |
| **Dağıtımdan önce veritabanını yedekleyin** | False | Belki de | Doğru |
| **Hedef veritabanında olan ancak bir veritabanı projesinde olmayan nesneler için bırak deyimleri oluştur** | False | Doğru | Doğru |
| **CLR türlerini güncelleştirmek için ALTER ASSEMBLY deyimleri kullanmayın** | False | False | False |
  

> [!NOTE]
> Veritabanı dağıtım özellikleri ve ortam konuları hakkında daha fazla bilgi için bkz. [bir genel bakış, veritabanı projenizin ayarlarının](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [nasıl yapılır: özelliklerini yapılandırmak için dağıtım ayrıntılarını](https://msdn.microsoft.com/library/dd172125.aspx), [ Derleme ve veritabanı bir yalıtılmış geliştirme ortamına dağıtma](https://msdn.microsoft.com/library/dd193409.aspx), ve [derleme ve veritabanları için bir hazırlık veya üretim ortamına dağıtma](https://msdn.microsoft.com/library/dd193413.aspx).


Veritabanı projesi birden fazla hedefe dağıtımını desteklemek için her hedef ortam için dağıtım yapılandırma dosyası oluşturmanız gerekir.

**Bir ortama özgü yapılandırma dosyası oluşturmak için**

1. Visual Studio 2010 içinde **Çözüm Gezgini** penceresinde veritabanı projenize sağ tıklayın ve ardından **özellikleri**.
2. Veritabanı Proje özellikleri sayfasında, üzerinde **Dağıt** sekmesinde **dağıtım yapılandırma dosyası** satır, tıklayın **yeni**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. İçinde **yeni bir dağıtım yapılandırma dosyası** iletişim kutusunda, dosyayı anlamlı bir ad verin (örneğin, **TestEnvironment.sqldeployment**) ve ardından **Kaydet**.
4. Üzerinde *[Filename] *** .sqldeployment** sayfasında, hedef ortamınızın gereksinimlerini karşılamak için dağıtım özellikleri ayarlayın ve ardından dosyayı kaydedin.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Veritabanı projenizde özellikleri klasörüne yeni dosya eklenir dikkat edin.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Dağıtım yapılandırma dosyası içinde VSDBCMD belirtme

Visual Studio 2010 içinden çözüm yapılandırmaları (örneğin, hata ayıklama ve yayın) kullandığınızda, dağıtım yapılandırma dosyası her bir yapılandırma ile ilişkilendirebilirsiniz. Belirli bir yapılandırma derlerken, derleme işlemi yapılandırmaya özgü dağıtım yapılandırma dosyasına işaret eden özel yapılandırma dağıtım bildirimi dosyası oluşturur. Ancak, bu öğreticilerde açıklanan dağıtım yaklaşımı ana amaçları kişiler Visual Studio 2010 ve çözüm yapılandırmaları kullanmadan dağıtım işlemi denetleme olanağı vermek için biridir. Bu yaklaşımda, çözüm yapılandırması hedef dağıtım ortamı bağımsız olarak aynıdır. Belirli bir hedef ortam için veritabanı dağıtımınızı uyarlamak için dağıtım yapılandırma dosyası belirtmek için VSDBCMD komut satırı seçeneklerini kullanabilirsiniz.

İçinde VSDBCMD bir dağıtım yapılandırma dosyası belirtmek için kullanın **p:/DeploymentConfigurationFile** geçin ve dosyanızı tam yolunu belirtin. Bu, dağıtım bildirimini tanımlayan dağıtım yapılandırma dosyası geçersiz kılar. Örneğin, dağıtmak için bu VSDBCMD komut kullanabilirsiniz **ContactManager** veritabanı bir test ortamı için:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Dosyayı çıkış dizinine kopyalarken yapı işlemi .sqldeployment dosyanızı yeniden unutmayın.


Dağıtım öncesi veya dağıtım sonrası SQL betiğinizde SQL komutu değişkenleri kullanırsanız dağıtımınıza bir ortama özgü .sqlcmdvars dosyasını ilişkilendirmek için benzer bir yaklaşım kullanabilirsiniz. Bu durumda, kullandığınız **p:/SqlCommandVariablesFile** .sqlcmdvars dosyanızı tanımlamak için anahtar.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Bir MSBuild proje dosyasından VSDBCMD komutu çalıştırılıyor

Kullanarak bir MSBuild proje dosyası VSDBCMD komuttan çağırabilirsiniz bir **Exec** görev içinde bir MSBuild hedefi. En basit şekliyle, onu şöyle görünebilir:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- Uygulamada, proje dosyalarınızı okuma ve yeniden kullanmak, daha kolay hale getirmek için çeşitli komut satırı parametreleri depolamak için özellikler oluşturmak istersiniz. Bu, kullanıcılar bir ortama özgü proje dosyasında özellik değerlerini belirtin veya MSBuild komut satırından varsayılan değerlerini geçersiz kılması için kolaylaştırır. Açıklanan bölünmüş proje dosyası yaklaşımı kullanırsanız [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), derleme yönergeleri ve iki dosya arasında özellikleri uygun şekilde bölme:
- Dağıtım yapılandırma dosya adı, veritabanı bağlantı dizesi ve hedef veritabanı adı gibi ortama özgü ayarları ortama özgü proje dosyasında gitmeniz gerekir.
- Evrensel bir proje dosyasında VSDBCMD komutu, Evrensel özeliklerini VSDBCMD yürütülebilir dosya konumunu gibi birlikte çalışan MSBuild hedefi gitmeniz gerekir.

Ayrıca, böylece oluşturulmuş ve kullanılmaya hazır .deploymanifest dosyasıdır VSDBCMD çağırma önce veritabanı projesini derleme emin olmalısınız. Bu yaklaşımın konusunda tam bir örnek gördüğünüz [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), yol gösterir proje dosyalarında aracılığıyla [Kişi Yöneticisi örnek çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Sonuç

MSBuild ve VSDBCMD kullanarak veritabanı projeleri dağıtırken farklı hedef ortamlarında veritabanı özelliklerini nasıl uyarlayabileceğiniz bu konuda açıklanan. Bu yaklaşım, daha büyük, kurumsal sınıf çözümler bir parçası olarak veritabanı projeleri dağıtmak istediğinizde yararlıdır. Bu çözümler genellikle korumalı geliştirme ve test ortamları, hazırlama veya tümleştirme platformları ve üretim veya canlı ortamları gibi birden fazla hedefe dağıtılır. Bu hedef ortamların her birinde, genellikle bir dizi benzersiz veritabanı dağıtım özellikleri gerektirir.

## <a name="further-reading"></a>Daha Fazla Bilgi

Veritabanı projeleri VSDBCMD.exe kullanarak dağıtma hakkında daha fazla bilgi için bkz. [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md). Dağıtım işlemini denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz. [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Bu makaleler MSDN'de veritabanı dağıtımı hakkında daha fazla genel rehberlik sağlar:

- [Veritabanı Proje ayarlarına genel bakış](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Nasıl yapılır: dağıtım ayrıntılarını özelliklerini yapılandırın](https://msdn.microsoft.com/library/dd172125.aspx)
- [Derleme ve veritabanlarını bir yalıtılmış geliştirme ortamına dağıtma](https://msdn.microsoft.com/library/dd193409.aspx)
- [Derleme ve veritabanları için bir hazırlık veya üretim ortamına dağıtma](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Önceki](performing-a-what-if-deployment.md)
> [İleri](deploying-database-role-memberships-to-test-environments.md)
