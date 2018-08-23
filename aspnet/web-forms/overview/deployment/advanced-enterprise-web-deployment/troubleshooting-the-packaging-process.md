---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: Paketleme işleminin sorunlarını giderme | Microsoft Docs
author: jrjlee
description: Bu konu, M sürümünde EnablePackageProcessLoggingAndAssert özelliğini kullanarak paketleme işlemi hakkında ayrıntılı bilgi nasıl Toplayabileceğiniz açıklar...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22be1ccc5a1ec52d143bedffd79264918c1a45e1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757266"
---
<a name="troubleshooting-the-packaging-process"></a>Paketleme işleminin sorunlarını giderme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Nasıl kullanarak paketleme işlemi hakkında ayrıntılı bilgi toplamak bu konuda açıklanmaktadır **EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) özelliği.
> 
> Ayarladığınızda **EnablePackageProcessLoggingAndAssert** özelliğini **true**, MSBuild olur:
> 
> - Paketleme işlemi hakkında daha fazla bilgi için derleme günlüklerine ekleyin.
> - Yinelenen dosyalar paketleme listede bulunması durumunda hataları belirli koşullar altında günlüğe kaydeder.
> - Bir günlük dizini oluşturma *ProjectName*\_paketi klasörü ve paketleme dosyaları hakkındaki bilgileri kaydetmek için kullanın.
> 
> Paketleme işlemi başarısız oluyor ya da beklediğiniz dosyaları, web dağıtımı paketleri içermeyen, burada şeyleri yanlış giden pinpoint ve işlem sorunlarını gidermek için bu bilgileri kullanabilirsiniz.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** özelliği yalnızca çalışır kullanarak projenize yapı **hata ayıklama** yapılandırma. Özelliği diğer yapılandırmaları göz ardı edilir.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>EnablePackageProcessLoggingAndAssert özelliği anlama

[Oluşturma ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) nasıl Web yayımlama işlem hattı (WPP) MSBuild işlevselliğini genişleten ve Internet Information Services (IIS) Web ile tümleştirmek etkinleştirmek MSBuild hedefleri takımına açıklanan Dağıtım Aracı (Web dağıtımı). Bir web uygulaması projesi paketlediğinizde WPP hedefleri çağırma.

Bu WPP hedefler çok sayıda ek bilgileri günlüğe kaydeder, koşullu mantık dahil olduğunda **EnablePackageProcessLoggingAndAssert** özelliği **true**. Örneğin, gözden **paket** hedef, ek günlük dizinini oluşturur ve dosyaların listesini bir metin dosyasına yazar görebilirsiniz **EnablePackageProcessLoggingAndAssert** içineşittir**true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> WPP hedefleri tanımlanan *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasöründeki dosya. Bu dosyayı açın ve Visual Studio 2010 veya herhangi bir XML Düzenleyicisi hedeflerin gözden geçirin. Dosyanın içeriğini değiştirmek için dikkatli olun.


## <a name="enabling-the-additional-logging"></a>Ek günlük kaydını etkinleştirme

İçin bir değer sağlayabilirsiniz **EnablePackageProcessLoggingAndAssert** nasıl projenizi bağlı olarak çeşitli şekillerde özelliği.

Komut satırından projenizi, bir değer sağlayabilirsiniz **EnablePackageProcessLoggingAndAssert** komut satırı bağımsız değişkeni olarak özelliği:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Projelerinizi oluşturmak için özel proje dosyası kullanıyorsanız, dahil edebileceğiniz **EnablePackageProcessLoggingAndAssert** değerini **özellikleri** özniteliği **MSBuild**görevi:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Projelerinizi derlemek için Team Foundation Server (TFS) derleme tanımını kullanıyorsanız için bir değer sağlayabilirsiniz **EnablePackageProcessLoggingAndAssert** özelliğinde **MSBuild bağımsız değişkenleri** satır:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Oluşturma ve derleme tanımı yapılandırma hakkında daha fazla bilgi için bkz. [bir derleme tanımı, destekleyen dağıtım oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Alternatif olarak, her derlemede paket dahil etmek istiyorsanız, ayarlamak, web uygulaması projesi için proje dosyasını değiştirebilir **EnablePackageProcessLoggingAndAssert** özelliğini **true**. Özelliği ilk eklemelisiniz **PropertyGroup** .csproj veya .vbproj dosyanızı içindeki öğe.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Günlük dosyalarını gözden geçirme

Ne zaman oluşturun ve bir web uygulaması projesi ile paket **EnablePackageProcessLoggingAndAssert** kümesine **true**, MSBuild günlüğünde adlı ek bir klasör oluşturur *ProjectName* \_Paket klasörüne. Günlük klasörü çeşitli dosyaları içerir:

![](troubleshooting-the-packaging-process/_static/image2.png)

Gördüğünüz dosyaların listesi, projenizin ve yapı işleminizin şeyler göre değişir. Ancak, bu dosyalar, genellikle WPP toplama işleminin çeşitli aşamasında paketleme dosyaları listesini kaydetmek için kullanılır:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* dosya dışlama için belirtilen tüm dosyaları kaldırılmadan önce paketleme MSBuild tarafından toplanan dosyaları listeler.
- *AfterExcludeFilesFilesList.txt* dosya dışlama için belirtilen tüm dosyaları kaldırıldıktan sonra değiştirilen dosya listesini içerir.

    > [!NOTE]
    > Dosya ve klasörleri paketleme işleminin dışında tutarak daha fazla bilgi için bkz: [hariç dosya ve klasörleri dağıtımdan](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* dosyası için paketleme herhangi sonra toplanan dosyaları listeler *Web.config* dönüşümleri gerçekleştirilir. Bu listedeki herhangi bir yapılandırma belirli *Web.config* dosyaları gibi dönüştürme *Web.Debug.config* ve *Web.Release.config*, dosyaları listesinden dışlanmaz paketleme. Dönüştürülen tek bir *Web.config* bunun yerine dahildir.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* dosya bağlantı dizelerini sonra dosyaların listesini içeren *Web.config* dosya parametreli. Bu, paketi dağıtırken, hedef ortamınız için doğru ayarlarla bağlantı dizelerinizi değiştirmenizi sağlayan işlemidir.
- *Prepackage.txt* dosyanın pakete dahil edilecek dosyalar sonlandırılmış derleme öncesi listesini içerir.

> [!NOTE]
> Ek günlük dosyalarının adlarını genellikle WPP hedeflerini karşılık gelir. Bu hedefler inceleyerek inceleyebilirsiniz *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasöründeki dosya.


Web paketinin içeriği beklediğiniz değilseniz, bu dosyaları gözden geçirme işlemi şeyler hangi noktasında sorun oluştu tanımlamak için kullanışlı bir yol olabilir.

## <a name="conclusion"></a>Sonuç

Bu konu nasıl kullanabileceğinizi açıklanan **EnablePackageProcessLoggingAndAssert** paketleme işleminin sorunlarını gidermek için MSBuild özelliği. İçinde sağlayabilirler oluşturma sürecinde özellik değeri farklı yolları açıklanmıştır ve özellik kümesine olduğunda, kaydedilen ek bilgileri açıklanan **true**.

## <a name="further-reading"></a>Daha Fazla Bilgi

Dağıtım işlemini denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz. [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md). WPP ve paketleme işlemi nasıl yönettiği hakkında daha fazla bilgi için bkz. [oluşturma ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Web dağıtımı paketleri, belirli dosyaları ve klasörleri dışarıda konusunda yönergeler için bkz. [hariç dosya ve klasörleri dağıtımdan](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Önceki](running-windows-powershell-scripts-from-msbuild-project-files.md)
