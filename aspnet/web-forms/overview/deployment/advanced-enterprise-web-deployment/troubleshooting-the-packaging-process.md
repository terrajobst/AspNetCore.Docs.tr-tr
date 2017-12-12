---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: "Paketleme işlemini sorunlarını giderme | Microsoft Docs"
author: jrjlee
description: "Bu konu, M EnablePackageProcessLoggingAndAssert özelliğini kullanarak paketleme işlemi hakkında ayrıntılı bilgi nasıl toplayabilirsiniz açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 977077357eb5774193a40c55fabee9733dd5ab2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="troubleshooting-the-packaging-process"></a>Paketleme işlemini sorunlarını giderme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Nasıl kullanarak paketleme işlemi hakkında ayrıntılı bilgi toplamak bu konuda açıklanmaktadır **EnablePackageProcessLoggingAndAssert** Microsoft Build Engine (MSBuild) özelliği.
> 
> Ayarladığınızda **EnablePackageProcessLoggingAndAssert** özelliğine **doğru**, MSBuild olur:
> 
> - Paketleme işlemini hakkında ek bilgi için yapı günlükleri ekleyin.
> - Yinelenen dosyalar paketleme listede bulunursa, örneğin, belirli koşullar altında hatalarını günlüğe.
> - Bir günlük dizini oluşturma *ProjectName*\_paketini klasörü ve paketleme dosyalar hakkındaki bilgileri kaydetmek için kullanın.
> 
> Paket oluşturma işlemi başarısız oluyor ya da web dağıtımı paketleri beklediğiniz dosyaları içermeyen, burada şeyler yanlış kalacaklarını işlemi ve pinpoint gidermek için bu bilgileri kullanabilirsiniz.
> 
> > [!NOTE]
> > **EnablePackageProcessLoggingAndAssert** özelliği yalnızca çalışır kullanarak projesi oluşturursanız **hata ayıklama** yapılandırma. Özelliği, diğer yapılandırmaları göz ardı edilir.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>EnablePackageProcessLoggingAndAssert özelliğinin anlama

[Yapı ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) Web yayımlama ardışık düzen (WPP) MSBuild işlevselliğini genişleten ve Internet Information Services (IIS) Web ile tümleştirmek etkinleştiren MSBuild hedefleri kümesi nasıl sağladığını açıklanan Dağıtım Aracı (Web dağıtımı). Bir web uygulaması projesi paketini oluşturduğunuzda WPP hedefleri çağırma.

Bu WPP hedeflerde çok sayıda dahil ek bilgileri günlüğe kaydeder koşullu mantık zaman **EnablePackageProcessLoggingAndAssert** özelliği ayarlanmış **doğru**. Örneğin, gözden geçirin, **paket** hedef, ek günlük dizini oluşturur ve dosyaların bir listesini, bir metin dosyasına yazar görebilirsiniz **EnablePackageProcessLoggingAndAssert** içineşittir**doğru**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> WPP hedefleri tanımlanan *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasörü içinde dosya. Bu dosyayı açın ve Visual Studio 2010 veya herhangi bir XML Düzenleyicisi hedefleri gözden geçirin. Dosya içeriğini değiştirmek için dikkatli olun.


## <a name="enabling-the-additional-logging"></a>Ek günlük kaydını etkinleştirme

İçin bir değer sağlamanız **EnablePackageProcessLoggingAndAssert** projenizi nasıl oluşturulacağına bağlı olarak çeşitli şekillerde özelliği.

Projenizi komut satırından yapılandırdıysanız, için bir değer sağlayabilir **EnablePackageProcessLoggingAndAssert** özelliği bir komut satırı bağımsız değişkeni olarak:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Projelerinizi oluşturmak için bir özel proje dosyası kullanıyorsanız, içerebilir **EnablePackageProcessLoggingAndAssert** değeri **özellikleri** özniteliği **MSBuild**görevi:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Team Foundation Server (TFS) derleme tanımını, projeler derlemek için kullanıyorsanız, için bir değer sağlayabilir **EnablePackageProcessLoggingAndAssert** özelliğinde **MSBuild bağımsız değişkenleri** satır:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Oluşturma ve derleme tanımları yapılandırması hakkında daha fazla bilgi için bkz: [bir yapı tanımı olduğunu destekleyen dağıtım oluşturma](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Alternatif olarak, her bir yapı içinde paket dahil etmek istiyorsanız, ayarlamak, web uygulama projesi için proje dosyası değiştirebilirsiniz **EnablePackageProcessLoggingAndAssert** özelliğine **doğru**. Özelliği ilk eklemelisiniz **PropertyGroup** .csproj veya .vbproj dosyanız içindeki öğesi.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Günlük dosyalarını gözden geçirme

Ne zaman oluşturmak ve bir web uygulaması projesi ile paket **EnablePackageProcessLoggingAndAssert** kümesine **true**, MSBuild günlüğüne adlı ek bir klasör oluşturur *ProjectName* \_Paket klasörü. Günlük klasörü çeşitli dosyaları içerir:

![](troubleshooting-the-packaging-process/_static/image2.png)

Gördüğünüz dosyaların listesini projeniz ve yapı işleminizin şeyler göre değişir. Ancak, bu dosyalar, genellikle WPP toplama işleminin çeşitli aşamaları sırasında paketleme için dosyalarının listesini kaydetmek için kullanılır:

- *PreExcludePipelineCollectFilesPhaseFileList.txt* dosyası dışlama için belirtilen tüm dosyaları kaldırılmadan önce paketlemek için MSBuild toplar dosyaları listeler.
- *AfterExcludeFilesFilesList.txt* dosyası dışlama için belirtilen tüm dosyaları kaldırıldıktan sonra değiştirilen dosya listesini içerir.

    > [!NOTE]
    > Dosya ve klasörleri paketleme işleminden hariç olmak üzere daha fazla bilgi için bkz: [hariç dosya ve klasörleri dağıtımından](excluding-files-and-folders-from-deployment.md).
- *AfterTransformWebConfig.txt* dosyası listeler herhangi sonra paketleme için toplanan dosyaları *Web.config* dönüşümler gerçekleştirilmiş. Bu listedeki tüm yapılandırma özgü *Web.config* dosyaları gibi dönüştürme *Web.Debug.config* ve *Web.Release.config*, dosyaları listesinden hariç paketleme. Dönüştürülen tek bir *Web.config* kendi yerde bulunur.
- *PostAutoParameterizationWebConfigConnectionStrings.txt* dosyasını bağlantı dizeleri sonra dosyaların listesini içeren *Web.config* dosya parametreli. Bu paketi dağıttığınızda, hedef ortamınız için doğru ayarlarla bağlantı dizelerinizi değiştirmenizi sağlayan işlemdir.
- *Prepackage.txt* dosyası pakete eklenecek dosyaların sonlandırılmış ön derleme listesini içerir.

> [!NOTE]
> Ek günlük dosyaları adları genellikle WPP hedefe karşılık gelir. İnceleyerek bu hedeflerde gözden geçirebilirsiniz *Microsoft.Web.Publishing.targets* % PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web klasörü içinde dosya.


Web paketinin içeriğini beklediğiniz değilseniz, bu dosyaları gözden geçirme işlemi şeyler hangi noktasında yanlış gittiğini adresindeki tanımlamak için kullanışlı bir yol olabilir.

## <a name="conclusion"></a>Sonuç

Bu konuda açıklanan nasıl kullanacağınızı **EnablePackageProcessLoggingAndAssert** Paketleme işlemini giderilir MSBuild özelliği. İçinde sağladığınız oluşturma işlemi için özellik değeri farklı yolları açıklanmıştır ve özellik kümesine olduğunda, kaydedilen ek bilgi açıklanan **doğru**.

## <a name="further-reading"></a>Daha Fazla Bilgi

Dağıtım işlemi denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md). WPP ve Paketleme işlemini nasıl yönettiği hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Web dağıtım paketi belirli dosyaları ve klasörleri dışarıda bırak konusunda yönergeler için bkz [hariç dosya ve klasörleri dağıtımından](excluding-files-and-folders-from-deployment.md).

>[!div class="step-by-step"]
[Önceki](running-windows-powershell-scripts-from-msbuild-project-files.md)
