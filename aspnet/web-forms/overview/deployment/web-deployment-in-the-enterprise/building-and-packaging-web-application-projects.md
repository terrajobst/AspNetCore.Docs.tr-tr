---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Derleme ve paketleme Web Uygulama projeleri | Microsoft Docs
author: jrjlee
description: Bir web uygulaması projesi için bir uzak sunucu ortamı dağıtmak istediğinizde, projeyi derleyin ve web dağıtımı paketiDesteklenen oluşturmak için ilk göreviniz olan...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: 406b8e6daf47196eb98700efe41e34c02d5682d3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755111"
---
<a name="building-and-packaging-web-application-projects"></a>Derleme ve Web Uygulama projeleri paketleme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bir web uygulaması projesi için bir uzak sunucu ortamı dağıtmak istediğinizde, projeyi derleyin ve web dağıtım paketi oluşturmak için ilk göreviniz olan. Bu konuda, web uygulama projeleri için derleme işleminin nasıl çalıştığı açıklanmaktadır. Özellikle, açıklar:
> 
> - Nasıl dağıtım işlevselliği eklemek için derleme işlemindeki Web yayımlama işlem hattı (WPP) genişletir.
> - Nasıl Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) web uygulamanıza bir dağıtım paketi bırakır.
> - Derleme ve paketleme işleminin nasıl çalıştığı ve dosyalar oluşturulur.


Visual Studio 2010'da web uygulama projeleri için derleme ve dağıtım işlemi WPP tarafından desteklenir. WPP MSBuild işlevlerini genişletmek ve Web dağıtımı ile tümleştirmek etkinleştiren Microsoft Build Engine (MSBuild) hedefleri sunmaktadır. Visual Studio içinden bu genişletilmiş işlevselliği için web uygulaması projenize özellik sayfalarında görebilirsiniz. **Paketle/Yayımla Web** sayfasında, birlikte **SQL Paketle/Yayımla** sayfası, yapı işlemi tamamlandığında, web uygulaması projenizin dağıtım için nasıl paketlenmiştir yapılandırmanızı sağlar.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>WPP nasıl çalışır?

Bir proje dosyasını bir C# için göz atın,-temel web uygulaması projesi, iki .targets dosyalarına aktarır görebilirsiniz.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


İlk **alma** deyimi, tüm Visual C# projeleri için ortak. Bu dosya *Microsoft.CSharp.targets*, hedefler ve Visual C# için belirli görevler içerir. Örneğin, C# derleyicisi (**Csc**) görev burada çağrılır. *Microsoft.CSharp.targets* sırayla içeri aktarmalar dosyası *Microsoft.Common.targets* dosya. Bu gibi tüm projeler için ortak olan hedefleri tanımlar **derleme**, **yeniden**, **çalıştırma**, **derleme**, ve **Temizle** . İkinci **alma** deyimi web uygulaması projelerine özeldir. *Microsoft.WebApplication.targets* sırayla içeri aktarmalar dosyası *Microsoft.Web.Publishing.targets* dosya. *Microsoft.Web.Publishing.targets* temelde dosya *olduğu* WPP. Hedefler gibi tanımlar **paket** ve **MSDeployPublish**, çeşitli dağıtım görevlerini tamamlamak için Web dağıtımı çağırır.

Bu ek hedefler, kişi yöneticisi örnek çözümde kullanılma oluşturulduklarını *Publish.proj* göz atın ve dosya **BuildProjects** hedef.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Bu hedef kullanan **MSBuild** çeşitli projeleri oluşturmak için görev. Bildirim **DeployOnBuild** ve **DeployTarget** özellikleri:

- **DeployOnBuild = true** özelliği temelde anlamına gelir "İstiyorum ek bir hedef derleme başarıyla tamamlandığında yürütülecek."
- **DeployTarget** özelliği tanımlar, istediğiniz zaman yürütmek için hedef adını **DeployOnBuild** özelliğini eşittir **true**. Bu durumda, MSBuild yürütmek için istediğiniz belirlediniz **paket** projesini oluşturduktan sonra hedef.

**Paket** hedef tanımlanmış *Microsoft.Web.Publishing.targets* dosya. Esas olarak, bu hedef, web uygulaması projenize yapı çıkışını alır ve bir IIS web sunucusuna yayımlanan web dağıtım paketi dönüştürür.

> [!NOTE]
> Bir proje dosyasını görüntülemek için (örneğin, <em>ContactManager.Mvc.csproj</em>) Visual Studio 2010'da ilk çözümünüzü projeden kaldırmak gerekir. İçinde <strong>Çözüm Gezgini</strong> penceresinde proje düğümünü sağ tıklatın ve ardından <strong>projeyi</strong>. Proje düğümüne sağ tıklayın ve ardından <strong>Düzenle</strong><em>[proje dosyası]</em>). Proje dosyası, ham XML biçiminde açılır. İşiniz bittiğinde, projeyi yeniden yüklemenizi unutmayın.  
> MSBuild hedefleri, görevleri hakkında daha fazla bilgi ve <strong>alma</strong> deyimleri bkz [proje dosyasını anlama](understanding-the-project-file.md). Proje dosyalarını ve WPP daha ayrıntılı bir giriş için bkz. [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation derlemesi](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>Web dağıtım paketi nedir?

Yapı ve Visual Studio 2010 kullanılarak veya doğrudan MSBuild kullanarak bir web uygulaması projesi dağıtma, sonuç genellikle olduğu bir *web dağıtım paketi*. Web dağıtım paketi bir .zip dosyasıdır. IIS'nin her şeyi içerir ve web uygulamanızı yeniden oluşturmak için gereken Web dağıtımı dahil olmak üzere:

- İçerik, kaynak dosyaları, yapılandırma dosyalarını, JavaScript ve geçişli stil sayfaları (CSS) kaynakları vb. dahil olmak üzere, web uygulamanızın derlenmiş çıkışı.
- Çözümünüzdeki projeleri başvurulan derlemeler için web uygulaması projenize ve tüm.
- SQL betikleri, web uygulamanızla dağıtıyorsanız herhangi bir veritabanına oluşturulacak.

Web dağıtım paketi oluşturulduktan sonra bir IIS web sunucusuna çeşitli şekillerde yayımlayabilirsiniz. Örneğin, bunu uzaktan Web dağıtma Uzak Aracı hizmeti veya Web dağıtımı işleyicisi hedef web sunucusunda hedefleyerek dağıtabilirsiniz veya paket hedef web sunucusunda el ile içeri aktarmak için IIS Yöneticisi'ni kullanabilirsiniz. Dağıtım için bu yaklaşımlar hakkında daha fazla bilgi için bkz. [Web dağıtımı için doğru yaklaşımı seçme](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Derleme işlemi nasıl çalışır?

Başka bir gösterilmektedir yapı ve bir web uygulaması projesi paketini ne olur:

![](building-and-packaging-web-application-projects/_static/image2.png)

Bir web uygulaması projesi oluşturduğunuzda, yapı işlemi adlı bir dosya oluşturur. *[Proje adı]. SourceManifest.xml*. Proje dosyası ve yapı çıkışını yanı sıra bu *. SourceManifest.xml* dosya söyler Web dağıtımı, web dağıtım paketinin içermesi gerekir. Bu giriş kullanarak, Web dağıtımı oluşturur adlı bir web dağıtım paketi *[Proje adı] .zip*.

Web dağıtım paketi yanı sıra, yapı işlemi paketini kullanmanıza yardımcı olacak iki dosya oluşturur:

- *. Deploy.cmd* dosyası için uzak bir IIS web sunucusu, web dağıtım paketi yayımlama, Web dağıtımı (MSDeploy.exe) parametreli komutlar kümesi içerir. Çalışan *. deploy.cmd* dosya, uygun parametrelerle genellikle bir daha hızlı sağlar ve daha kolay bir alternatif MSDeploy.exe el ile oluşturmak için kendiniz komutları.
- *SetParameters.xml* dosya MSDeploy.exe komut için parametre değerleri kümesi sağlar. Bu değerler dahil herhangi bir hizmet uç noktaları değerlerini paketini dağıtmak istediğiniz ve bağlantı dizeleri tanımlanan IIS web uygulamasının adı gibi özellikleri *web.config* dosya ve herhangi bir dağıtım özelliği Proje Özellikleri sayfalarında tanımlanan değerleri.

*SetParameters.xml* dağıtım işlemini yönetmek için bir tuşa dosyasıdır. Bu dosya, web uygulaması projenize içeriğini göre dinamik olarak oluşturulur. Örneğin, bir bağlantı dizesi eklerseniz, *web.config* dosya, derleme işlemi otomatik olarak bağlantı dizesini algılar, dağıtımı buna göre Parametreleştirme ve girdi oluşturma  *SetParameters.xml* bağlantı dizesi dağıtım işleminin bir parçası olarak değiştirmenize izin vermek için dosya. Bir sonraki konu [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md), bu dosya daha ayrıntılı rolünü açıklar ve hangi değiştirebilirsiniz, derleme ve dağıtım sırasında farklı yollarını açıklar.

> [!NOTE]
> Visual Studio 2010'da, bir web uygulaması paketleme önce sayfalarında önceden derleme WPP desteklemez. Visual Studio ve WPP'ın sonraki sürümü, bir web uygulaması olarak paketleme seçeneği önceden derleme olanağı dahil edilir.


## <a name="conclusion"></a>Sonuç

Bu konuda, web uygulama projeleri Visual Studio 2010 için derleme ve paketleme işlemi genel bir bakış sağlanır. Nasıl WPP MSBuild Web dağıtımı komutlarından çağırma sağladığını açıklanan ve derleme ve paketleme işleminin nasıl çalıştığı açıklanmıştır.

Web dağıtım paketi oluşturduktan sonra sonraki adımınız onu dağıtmaktır. Bunun hakkında daha fazla bilgi için bkz. [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md) ve [Web paketleri dağıtma](deploying-web-packages.md).

## <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide, sonraki konuları [Web paketi dağıtımı için yapılandırma parametreleri](configuring-parameters-for-web-package-deployment.md) ve [Web paketleri dağıtma](deploying-web-packages.md), oluşturduğunuz web paketin nasıl kullanılacağı hakkında rehberlik sağlar. Bu serinin son Öğreticisi [Gelişmiş Kurumsal Web dağıtımı](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), özelleştirme ve paketleme işleminin sorunlarını giderme konusunda rehberlik sağlar.

Proje dosyalarını ve WPP daha ayrıntılı bir giriş için bkz. [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation derlemesi](http://amzn.com/0735645248) Sayed Ibrahim Hashimi ve William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Önceki](understanding-the-build-process.md)
> [İleri](configuring-parameters-for-web-package-deployment.md)
