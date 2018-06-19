---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Derleme ve Web Uygulama projeleri paketleme | Microsoft Docs
author: jrjlee
description: Bir web uygulaması projesi bir uzak sunucu ortamı dağıtmak istediğinizde, projeyi oluşturun ve web dağıtımı paketiDesteklenen üretmek için ilk göreviniz olup...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: d630e1776607bd0bd7c61e1f0f7234ef58c7533b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892315"
---
<a name="building-and-packaging-web-application-projects"></a>Derleme ve Web Uygulama projeleri paketleme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bir web uygulaması projesi bir uzak sunucu ortamı dağıtmak istediğinizde, ilk projeyi oluşturun ve bir web dağıtım paketi oluşturmak için bir görevdir. Bu konuda, web uygulaması projelerinde derleme işleminin nasıl çalıştığı açıklanmaktadır. Özellikle, açıklar:
> 
> - Web yayımlama ardışık düzen (WPP) nasıl dağıtım işlevleri dahil olmak üzere derleme işlem genişletir.
> - Nasıl (Web dağıtımı) Internet Information Services (IIS) Web dağıtım aracı web uygulamanıza bir dağıtım paketi etkinleştirir.
> - Derleme ve paketleme işleminin nasıl çalıştığı ve hangi dosyaların oluşturulur.


Visual Studio 2010'da, web uygulama projeleri için derleme ve dağıtım işlemi tarafından WPP desteklenir. WPP MSBuild işlevselliğini genişletmek ve Web dağıtımı ile tümleştirmek etkinleştiren Microsoft Build Engine (MSBuild) hedefleri kümesi sağlar. Visual Studio içinde genişletilmiş bu işlev özellik sayfalarında web uygulaması projeniz için görebilirsiniz. **Paketle/Yayımla Web** sayfasında, birlikte **SQL Paketle/Yayımla** sayfası, yapılandırmanızı derleme işlemi tamamlandığında, web uygulaması projenizin dağıtım için nasıl paketlenmiştir sağlar.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP nasıl çalışır?

Proje dosyası bir C# için göz atın,-tabanlı web uygulama projesi, iki .targets dosyaları alır görebilirsiniz.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


İlk **alma** açıklamadır tüm Visual C# projeleri için ortak. Bu dosya, *Microsoft.CSharp.targets*, hedefler ve Visual C# için belirli görevler içerir. Örneğin, C# Derleyici (**Csc**) görev burada çağrılır. *Microsoft.CSharp.targets* sırayla içeri aktarmalar dosya *Microsoft.Common.targets* dosya. Bu gibi tüm projeleri için ortak olan hedefleri tanımlar **yapı**, **yeniden**, **çalıştırmak**, **derleme**, ve **Temizle** . İkinci **alma** deyimi web uygulaması projelerine özel. *Microsoft.WebApplication.targets* sırayla içeri aktarmalar dosya *Microsoft.Web.Publishing.targets* dosya. *Microsoft.Web.Publishing.targets* temelde dosya *olan* WPP. Hedefler, gibi tanımlar **paket** ve **MSDeployPublish**, çeşitli dağıtım görevlerini tamamlamak için Web dağıtımı çağırma.

Bu ek hedefleri, kişinin Yöneticisi örnek çözümü kullanılma anlamak için açık *Publish.proj* dosyası ve bir göz atalım **BuildProjects** hedef.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Bu hedef kullanan **MSBuild** çeşitli projeleri oluşturmak üzere görev. Bildirim **DeployOnBuild** ve **DeployTarget** özellikleri:

- **DeployOnBuild = true** özelliği temelde anlamına gelir "İstiyorum yapı başarılı bir şekilde tamamlandığında bir ek hedefi yürütülecek."
- **DeployTarget** özelliği istediğiniz zaman yürütmek için hedef adını tanımlayan **DeployOnBuild** özelliği eşittir **doğru**. Bu durumda, yürütülecek MSBuild istediğiniz belirlediniz **paket** projesini oluşturduktan sonra hedef.

**Paket** hedef tanımlanmış *Microsoft.Web.Publishing.targets* dosya. Esas olarak, bu hedef, web uygulama projesi derleme çıktısını alır ve bir IIS web sunucusuna yayımlanmış bir web dağıtım paketi dönüştürür.

> [!NOTE]
> Proje dosyası görüntülemek için (örneğin, <em>ContactManager.Mvc.csproj</em>) Visual Studio 2010'da önce çözümünüzü projeden kaldırmak gerekir. İçinde <strong>Çözüm Gezgini</strong> penceresinde, proje düğümüne sağ tıklayın ve ardından <strong>projeyi</strong>. Proje düğümüne sağ tıklayın ve ardından <strong>Düzenle</strong><em>[proje dosyası]</em>). Proje dosyası ham XML biçiminde açılır. İşiniz bittiğinde projeyi yeniden yüklemek unutmayın.  
> MSBuild hedefleri, görevleri hakkında daha fazla bilgi ve <strong>alma</strong> deyimleri bkz [proje dosyası anlama](understanding-the-project-file.md). Proje dosyalarını ve WPP daha kapsamlı bir giriş için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Web dağıtım paketi nedir?

Derleme ve Visual Studio 2010 kullanarak veya doğrudan MSBuild kullanarak bir web uygulaması projesi dağıtma, nihai sonucu genellikle olduğunda bir *web dağıtım paketi*. Web dağıtım paketi bir .zip dosyasıdır. IIS'nin her şeyi içerir ve Web dağıtımı gerekir, web uygulamanızı yeniden oluşturmak için de dahil olmak üzere:

- İçerik, kaynak dosyaları, yapılandırma dosyalarını, JavaScript ve geçişli stil sayfaları (CSS) kaynaklar vb. dahil olmak üzere, web uygulamanızın derlenmiş çıkışı.
- Projeleri, çözümünüz içinde başvurulan derlemeler herhangi ve web uygulama projesi için.
- Web uygulamanızı dağıtıyorsunuz veritabanları oluşturmak için SQL komut dosyaları.

Web dağıtım paketi oluşturulduktan sonra bir IIS web sunucusuna çeşitli şekillerde yayımlayabilirsiniz. Örneğin, onu uzaktan Web dağıtmak uzak Aracısı hizmeti veya Web dağıtımı işleyicisi hedef web sunucusunda hedefleyerek dağıtabilirsiniz veya paket hedef web sunucusunda el ile almak için IIS Yöneticisi'ni kullanabilirsiniz. Bu yaklaşım dağıtımı hakkında daha fazla bilgi için bkz: [Web dağıtımı sağ yaklaşımı seçme](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Derleme işlemi nasıl çalışır?

Bu yapı ve bir web uygulaması projesi paket ne olacağını gösterir:

![](building-and-packaging-web-application-projects/_static/image2.png)

Bir web uygulaması projesi derlerken, derleme işlemi adlı bir dosya oluşturur *[Proje adı]. SourceManifest.xml*. Proje dosyası ve yapı çıktı yanı sıra bu *. SourceManifest.xml* dosya söyler Web dağıtımı, web dağıtımı paketi içermesi gerekir. Bu girişleri kullanarak, Web dağıtımı oluşturur adlı bir web dağıtım paketi *[Proje adı] .zip*.

Web dağıtım paketi paketini kullanmak için yardımcı olabilecek iki dosya oluşturma işlemi oluşturur:

- *. Deploy.cmd* dosyası, web dağıtımı paketi uzak bir IIS web sunucusuna yayımlama, Web dağıtımı (MSDeploy.exe) parametreli komutlar kümesi içerir. Çalışan *. deploy.cmd* uygun parametrelerle dosya genellikle bir daha hızlı sağlar ve daha kolay alternatif MSDeploy.exe el ile oluşturmak için kendiniz komutları.
- *SetParameters.xml* dosya parametre değerlerini MSDeploy.exe komut kümesi sağlar. Bu değerler, Hizmeti uç nokta değerleri paketini dağıtmak istediğiniz ve bağlantı dizeleri tanımlanan IIS web uygulaması adı gibi özellikleri dahil *web.config* dosya ve herhangi bir dağıtım özelliği Proje Özellikleri sayfalarında tanımlı değerler.

*SetParameters.xml* dağıtım işlemini yönetmek için anahtar bir dosyadır. Bu dosya, web uygulama projesi içeriğine göre dinamik olarak oluşturulur. Örneğin, bir bağlantı dizesi eklerseniz, *web.config* dosya, yapılandırma işlemi otomatik olarak bağlantı dizesini algılar, dağıtım uygun şekilde Parametreleştirme ve girdi oluşturma  *SetParameters.xml* dağıtım işleminin bir parçası bağlantı dizesini değiştirin olanak tanımak için dosya. Sonraki konuyu [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md), bu dosya daha ayrıntılı rolünü açıklar ve hangi değiştirebilirsiniz derleme ve dağıtım sırasında farklı yolları açıklanmaktadır.

> [!NOTE]
> Visual Studio 2010'da, bir web uygulaması paketleme önce sayfalarında önceden derleme WPP desteklemez. Visual Studio ve WPP sonraki sürümü paketleme seçeneği olarak bir web uygulaması derleneceği özelliği içerir.


## <a name="conclusion"></a>Sonuç

Bu konu yapı ve paketleme işlemi genel bakış web uygulama projeleri Visual Studio 2010 için sağlanan. Nasıl WPP MSBuild Web dağıtımı komutlarından çağırma olanak sağlayan açıklanan ve yapı ve paketleme işleminin nasıl çalıştığı açıklanmıştır.

Web dağıtım paketini oluşturduktan sonra sonraki adımınız, dağıtmaktır. Bunun hakkında daha fazla bilgi için bkz: [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md) ve [dağıtma Web paketleri](deploying-web-packages.md).

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide sonraki konuları [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md) ve [dağıtma Web paketleri](deploying-web-packages.md), oluşturduğunuz web paketin nasıl kullanılacağı hakkında rehberlik sağlar. Bu serideki son öğretici [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), özelleştirme ve Paketleme işlemini sorunlarını gidermek yönergeler verilmektedir.

Proje dosyalarını ve WPP daha kapsamlı bir giriş için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Önceki](understanding-the-build-process.md)
> [sonraki](configuring-parameters-for-web-package-deployment.md)
