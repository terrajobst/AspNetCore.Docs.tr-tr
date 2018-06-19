---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Bir ne olursa gerçekleştirme dağıtım | Microsoft Docs
author: jrjlee
description: Bu konuda 'ise' gerçekleştirmeyi açıklar (veya benzetimli) Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) ve V kullanarak dağıtımları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: c1a13f38c8e629bcd615190b00104109e25fb289
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879991"
---
<a name="performing-a-what-if-deployment"></a>"," Dağıtım gerçekleştirme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, "," gerçekleştirmeyi açıklar (veya benzetimli) VSDBCMD ve Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak dağıtımları. Bu gerçekten Uygulamanızı dağıtmadan önce dağıtım mantığınızı etkilerini belirli hedef ortamda belirlemenize olanak tanır.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir uygulama oluşturma yönergeleri içeren. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Web paketleri için bir "," dağıtım gerçekleştirme

Web dağıtımı içeren dağıtımlarda "," gerçekleştirmenize olanak sağlayan işlevselliği (veya deneme) modu. "," Modunda yapıları dağıttığınızda, Web dağıtımı bir günlük dosyası dağıtım gerçekleştirilen, ancak bunu gerçekten hedef sunucuda herhangi bir şeyi değiştirmez olarak oluşturur. Günlük dosyasını gözden dağıtımınızı hedef sunucuda, özellikle olacaktır hangi etkisi anlamanıza yardımcı olabilir:

- Ne eklenir.
- Ne güncelleştirilecektir.
- Ne silinecek.

"," Dağıtımı gerçekte ne zaman yapamayacağı hedef sunucuda herhangi bir şeyi değiştirmez olduğu için bir dağıtım başarılı olup olmadığını tahmin.

Bölümünde açıklandığı gibi [dağıtma Web paketleri](../web-deployment-in-the-enterprise/deploying-web-packages.md), Web dağıtımı kullanarak iki yolla web paketleri dağıtabilirsiniz&#x2014;doğrudan veya çalıştırarak MSDeploy.exe komut satırı yardımcı programını kullanarak *. deploy.cmd* dosyası derleme işlem oluşturur.

MSDeploy.exe doğrudan kullanıyorsanız, ekleyerek bir "," dağıtım çalıştırabilirsiniz **– whatIf** komutunuzu bayrak. Örneğin, bir hazırlama ortamına ContactManager.Mvc.zip paketi dağıtılmışsa, ne olacağını değerlendirmek için MSDeploy komutu şuna benzemelidir:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


"," Dağıtımınızın Sonuçlardan memnun kaldığınızda, kaldırabilirsiniz **– whatIf** dinamik dağıtımını çalıştırmak için bayrak.

> [!NOTE]
> MSDeploy.exe komut satırı seçenekleri hakkında daha fazla bilgi için bkz: [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Kullanıyorsanız, *. deploy.cmd* dosyası ekleyerek bir "," dağıtım çalıştırabilirsiniz **/t** (deneme modu) bayrağı yerine bayrak **/y** bayrağı ("Evet" veya güncelleştirme modu) komutu. Örneğin, çalıştırarak ContactManager.Mvc.zip paketi dağıtılmışsa, ne olacağını değerlendirmek için *. deploy.cmd* dosya, komutunuzu şuna benzemelidir:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


"Deneme modu" dağıtımınızın Sonuçlardan memnun kaldığınızda, değiştirebilirsiniz **/t** ile bayrak bir **/y** bayrağı dinamik dağıtımını çalıştırmak için:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> İçin komut satırı seçenekleri hakkında daha fazla bilgi için *. deploy.cmd* dosyaları görmek [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx). Çalıştırırsanız *. deploy.cmd* dosya bayrakları belirtmeden komut istemi kullanılabilir bayrakların bir listesi görüntülenir.


## <a name="performing-a-what-if-deployment-for-databases"></a>Veritabanları için bir "," dağıtım gerçekleştirme

Bu bölümde, artımlı, şema tabanlı veritabanı dağıtım gerçekleştirmek için VSDBCMD yardımcı programı kullanmakta olduğunuz varsayılır. Bu yaklaşım, daha ayrıntılı olarak açıklanan [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md). Burada açıklanan kavramları uygulamadan önce bu konu ile öğrenmeniz olmasını öneririz.

İçinde VSDBCMD kullandığınızda **dağıtma** kullanabileceğiniz modu, **/dd** (veya **/DeployToDatabase**) VSDBCMD yalnızca oluşturur veya gerçekte veritabanı dağıtan olup olmadığını denetlemek için bayrak bir dağıtım komut dosyası. .Dbschema dosya dağıtıyorsanız davranış budur:

- Belirtirseniz **/dd+** veya **/dd**, VSDBCMD bir dağıtım komut dosyası oluşturmanız ve veritabanı dağıtın.
- Belirtirseniz **/dd-** veya anahtarını atlarsanız, VSDBCMD, yalnızca bir dağıtım komut dosyası üretir.

> [!NOTE]
> Bir .deploymanifest dosyası yerine bir .dbschema dosyası davranışını dağıtıyorsanız **/dd** anahtarıdır çok daha karmaşıktır. VSDBCMD değerini esas olarak, göz ardı eder **/dd** .deploymanifest dosya içeriyorsa, anahtar bir **DeployToDatabase** değerini bir öğesiyle **doğru**. [Veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md) tam bu davranışını tanımlar.


Örneğin, bir dağıtım komut dosyası oluşturmak için **ContactManager** gerçekten VSDBCMD komutunuzu veritabanı dağıtma olmadan veritabanı bu benzer:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD bir fark veritabanı dağıtım araçtır ve bu nedenle dağıtım betiği dinamik olarak, belirtilen şemaya varsa, geçerli veritabanında güncelleştirmek için gereken tüm SQL komutları içerecek şekilde oluşturulur. Dağıtım betiğini gözden geçirme ne dağıtımınızı etkileyecek belirlemek için kullanışlı bir yol geçerli veritabanı ve raporda bulunan verilere sahip olur. Örneğin, belirlemek isteyebilirsiniz:

- Mevcut tabloların olup kaldırılacak ve, veri kaybına neden olur.
- Olup bölme ya da tabloları birleştirme işlemleri sırasını Örneğin, veri kaybı riski taşır.

Dağıtım betiği ile memnun kaldıysanız, VSDBCMD ile yineleyebilirsiniz bir **/dd+** değişiklik yapmak için bayrak. Alternatif olarak, gereksinimlerinize ve veritabanı sunucusunda el ile yürütmek için dağıtım betiğini düzenleyebilirsiniz.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Özel proje dosyalarına "," işlevselliği tümleştirme

Daha karmaşık dağıtım senaryolarında açıklandığı gibi derleme ve dağıtım mantığınızı kapsüllemek için özel bir Microsoft Build Engine (MSBuild) proje dosyası kullanmak istersiniz [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Örneğin, [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü *Publish.proj* dosyası:

- Çözüm oluşturur.
- Paket ve ContactManager.Mvc uygulamayı dağıtmak için Web dağıtımı kullanır.
- Paket ve ContactManager.Service uygulamayı dağıtmak için Web dağıtımı kullanır.
- Dağıtır **ContactManager** veritabanı.

Tek adımlı işlemine bu şekilde birden çok web paketleri ve/veya veritabanlarını dağıtımını'i tümleştirdiğinizde, ayrıca, bir "," modunda tam dağıtımını gerçekleştirme seçeneği isteyebilirsiniz.

*Publish.proj* dosyası gösterir nasıl bunu yapabilirsiniz. İlk olarak, "," değeri depolamak üzere bir özelliğe oluşturmanız gerekir:


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


Adlı bir özelliği bu durumda, oluşturduğunuz **whatIf** varsayılan değerini **false**. Kullanıcılar bu değeri özelliğini ayarlayarak kılabilirsiniz **true** komut satırı parametresi göreceksiniz kısa süre içinde.

Web dağıtımı Parametreleştirme için sonraki aşamasıdır ve VSDBCMD komutları bayrakları yansıtmasını **whatIf** özellik değeri. Örneğin, bir sonraki hedef (alınan *Publish.proj* dosya ve Basitleştirilmiş) çalıştıran *. deploy.cmd* web paketini dağıtmak için dosya. Varsayılan olarak, komut içeren bir **/Y** anahtarı ("Evet" veya güncelleştirme modu). Varsa **whatIf** ayarlanır **true**, bu değiştirilir bir **/T** (deneme veya "," modu) anahtarı.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Benzer şekilde, sonraki hedefi VSDBCMD yardımcı programı bir veritabanını dağıtmak için kullanır. Varsayılan olarak, bir **/dd** anahtar dahil değildir. Bu VSDBCMD bir dağıtım komut dosyası oluşturur, ancak veritabanı dağıtmaz anlamına gelir&#x2014;diğer bir deyişle, bir "," senaryo. Varsa **whatIf** özelliği ayarlı değil **true**, **/dd** anahtar eklenir ve VSDBCMD veritabanı dağıtmak.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


İlgili tüm komutlar, proje dosyasında Parametreleştirme için aynı yaklaşımı kullanın. Bir "," dağıtımını çalıştırmak istediğinizde, ardından yalnızca sağlayabilirsiniz bir **whatIf** komut satırından özellik değeri:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Bu şekilde, "," dağıtımı için tüm proje bileşenleri tek bir adımda çalıştırabilirsiniz.

## <a name="conclusion"></a>Sonuç

Bu konuda, "," Web dağıtımı, VSDBCMD ve MSBuild kullanarak dağıtımları açıklanmıştır. "," Dağıtım hedef ortama gerçekte herhangi bir değişiklik yapmadan önce önerilen dağıtım etkisini değerlendirmenize olanak sağlar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Komut satırı sözdizimi Web dağıtımı hakkında daha fazla bilgi için bkz: [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Kullandığınızda komut satırı seçenekleri hakkında yönergeler için *. deploy.cmd* dosya için bkz: [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx). VSDBCMD komut satırı sözdizimi hakkında yönergeler için bkz [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema Al)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Önceki](advanced-enterprise-web-deployment.md)
> [sonraki](customizing-database-deployments-for-multiple-environments.md)
