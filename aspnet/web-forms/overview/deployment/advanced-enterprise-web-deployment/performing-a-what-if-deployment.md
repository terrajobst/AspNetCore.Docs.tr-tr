---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Durum, gerçekleştirme dağıtım | Microsoft Docs
author: jrjlee
description: Bu konu ',' yerine getirilmesi anlatılmaktadır (veya sanal) Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) ile V dağıtımlarını...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754909"
---
<a name="performing-a-what-if-deployment"></a>Bir "What If" dağıtımı gerçekleştirme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu başlığı altında "what IF" yerine getirilmesi anlatılmaktadır (veya sanal) VSDBCMD ve Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak dağıtımları. Bu gerçekten Uygulamanızı dağıtmadan önce dağıtım mantığınızı etkilerini belirli hedef ortamda belirlemenize olanak sağlar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli derleme yönergeleri içeren. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Web paketleri için bir "What If" dağıtımı gerçekleştirme

Web dağıtımı içeren dağıtımlarda "what IF" gerçekleştirmenize olanak tanıyan işlevsellik (veya deneme) modu. Yapıtları "what IF" modunda dağıttığınızda, Web dağıtımı bir günlük dosyası dağıtımın gerçekleştiği, ancak gerçekte herhangi bir hedef sunucuda bir değişiklik yapmaz olarak oluşturur. Günlük dosyasını gözden geçirme dağıtımınızı hedef sunucuda, özellikle olacaktır hangi etkilerini anlamanıza yardımcı olabilir:

- Ne eklenir.
- Ne güncelleştirilecektir.
- Ne silinecek.

Bir dağıtım başarılı olur, "what IF" dağıtımı gerçekte ne zaman yapamazsınız, hedef sunucuda bir şeyi değiştirmez olduğundan tahmin edin.

Bölümünde anlatıldığı gibi [Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md), iki yolla Web Dağıtımı'nı kullanarak web paketleri dağıtabilirsiniz&#x2014;doğrudan veya çalıştırarak MSDeploy.exe komut satırı yardımcı programını kullanarak *. deploy.cmd* dosyası Yapı işlemi oluşturur.

MSDeploy.exe doğrudan kullanıyorsanız, ekleyerek "what IF" dağıtımın çalıştırılabileceği **– whatIf** komutunuz için bayrak. Örneğin, bir hazırlama ortamına ContactManager.Mvc.zip paket dağıttıysanız ne olacağını değerlendirmek için MSDeploy komut şuna benzemelidir:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


"What IF" dağıtımınızın Sonuçlardan memnun olduğunuzda, kaldırabilirsiniz **– whatIf** Canlı dağıtım çalıştırmak için bayrak.

> [!NOTE]
> MSDeploy.exe komut satırı seçenekleri hakkında daha fazla bilgi için bkz. [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Kullanıyorsanız *. deploy.cmd* dosyası dahil ederek bir "what IF" dağıtım çalıştırabilirsiniz **/t** (deneme modu) bayrağını yerine bayrak **/y** bayrağı ("Evet" veya güncelleştirme modu) komutunuz. Örneğin, çalıştırarak ContactManager.Mvc.zip paket dağıttıysanız ne olacağını değerlendirmek için *. deploy.cmd* dosyası, komut şuna benzemelidir:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


"Deneme modu" dağıtımınızın Sonuçlardan memnun olduğunuzda, değiştirebileceğiniz **/t** ile bayrak bir **/y** bayrağı Canlı dağıtım çalıştırmak için:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> İçin komut satırı seçenekleri hakkında daha fazla bilgi için *. deploy.cmd* dosyaları görmek [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx). Çalıştırırsanız *. deploy.cmd* dosya herhangi bir bayrağı belirtmeden komut istemini kullanılabilir bayrakların listesi görüntülenir.


## <a name="performing-a-what-if-deployment-for-databases"></a>Veritabanları için bir "What If" dağıtımı gerçekleştirme

Bu bölümde, VSDBCMD yardımcı programı artımlı, şema tabanlı veritabanı dağıtımı gerçekleştirmek için kullandığınız varsayılır. Bu yaklaşım, daha ayrıntılı olarak açıklanan [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md). Burada açıklanan kavramlar uygulamadan önce bu konu ile planladığınızdan öneririz.

İçinde VSDBCMD kullandığınızda **Dağıt** modunu kullanabilir **/dd** (veya **/DeployToDatabase**) VSDBCMD aslında veritabanı dağıtır ya da yalnızca oluşturur denetimine bayrak bir Dağıtım betiği. .Dbschema dosya dağıtıyorsanız, bu durum geçerlidir:

- Belirtirseniz **/dd+** veya **/dd**, VSDBCMD bir dağıtım betiği oluşturun ve veritabanı dağıtma.
- Belirtirseniz **/dd-** veya anahtarını atlarsanız, VSDBCMD yalnızca bir dağıtım komut dosyası üretir.

> [!NOTE]
> Davranışını bir .dbschema dosyası yerine .deploymanifest dosya dağıtıyorsanız **/dd** anahtarıdır çok daha karmaşıktır. Aslında, VSDBCMD değerini yoksayar **/dd** .deploymanifest dosya içeriyorsa, geçiş bir **DeployToDatabase** öğe değerini **True**. [Veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md) tam bu davranışını tanımlar.


Örneğin, bir dağıtım betiğini oluşturmak için **ContactManager** veritabanı veritabanı VSDBCMD komutunuz gerçekten dağıtmak zorunda kalmadan bu benzemesi gerekir:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD Türevsel bir veritabanı dağıtım araçtır ve bu nedenle dağıtım betiği dinamik olarak, belirtilen şemaya varsa, geçerli veritabanında güncelleştirmek için gereken tüm SQL komutları içerecek şekilde oluşturulur. Dağıtım betiği gözden geçirme ne dağıtımınıza etkisi belirlemek için kullanışlı bir yol geçerli veritabanı ve içerdiği verilere sahip olur. Örneğin, belirlemek isteyebilirsiniz:

- Mevcut tabloların olup kaldırılacak ve bu veri kaybına yol açar.
- Olup bölme ya da tabloları birleştirme gibi işlemlerin sırası veri kaybı riskini taşır.

Dağıtım betiği içeren memnun kaldıysanız, ile VSDBCMD yineleyebilirsiniz bir **/dd+** değişiklik yapmak için bayrak. Alternatif olarak, gereksinimlerinizi karşılayacak ve veritabanı sunucusunda el ile çalıştırmak için dağıtım betiğini düzenleyebilirsiniz.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Özel proje dosyalarına "What If" işlevselliği tümleştirme

Daha karmaşık dağıtım senaryolarında açıklandığı gibi derleme ve dağıtım mantığınızı yalıtılacak özel Microsoft Build Engine (MSBuild) proje dosyası kullanmak istiyorsanız [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Örneğin, [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözüm, *Publish.proj* dosyası:

- Çözüm oluşturur.
- Web dağıtımı ContactManager.Mvc uygulaması paketleme ve dağıtma için kullanır.
- Web dağıtımı ContactManager.Service uygulaması paketleme ve dağıtma için kullanır.
- Dağıtır **ContactManager** veritabanı.

Bu şekilde tek adımlı bir işlemin içinde birden çok web paketleri ve/veya veritabanları dağıtımını tümleştirdiğinizde, bir "what IF" modunda tüm dağıtımı gerçekleştirme seçeneği de isteyebilirsiniz.

*Publish.proj* dosyası, bunu yapmak nasıl gösterir. İlk olarak, "what IF" değerini depolamak için bir özellik oluşturmak gerekir:


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


Bu durumda, adlı bir özellik oluşturduğunuz **WhatIf** varsayılan değerini **false**. Kullanıcılar bu değeri özelliğini ayarlayarak kılabilir **true** komut satırı parametresi göreceksiniz kısa bir süre.

Web dağıtımı parametre haline getirmek için sonraki aşamasıdır ve VSDBCMD komutları bayrakları yansıtmasını **WhatIf** özellik değeri. Örneğin, sonraki hedefe (alınan *Publish.proj* dosya ve Basitleştirilmiş) çalıştıran *. deploy.cmd* web paketini dağıtmak için dosya. Varsayılan olarak, komut içeren bir **/Y** anahtarı ("Evet" veya güncelleştirme modu). Varsa **WhatIf** ayarlanır **true**, bu değiştirilir bir **/T** anahtarı (deneme veya "what IF" modu).


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Benzer şekilde, sonraki hedefi, bir veritabanı dağıtmak için VSDBCMD yardımcı programını kullanır. Varsayılan olarak, bir **/dd** anahtar dahil değildir. Yani VSDBCMD dağıtım betiği oluşturur ancak veritabanı dağıtmaz&#x2014;başka bir deyişle, "what IF" senaryo. Varsa **WhatIf** özelliği ayarlanmamış **true**, **/dd** anahtar eklenir ve VSDBCMD, veritabanı dağıtacağınız.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Aynı yaklaşımı, ilgili tüm komutlar, proje dosyasında parametre haline getirmek için kullanabilirsiniz. Bir "what IF" dağıtım çalıştırmak istediğiniz zaman, ardından basitçe sağlayabilirsiniz bir **WhatIf** komut satırından özellik değeri:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Bu şekilde, "what IF" dağıtımı için tüm proje bileşenleri tek bir adımda çalıştırabilirsiniz.

## <a name="conclusion"></a>Sonuç

Bu konuda, Web dağıtımı, VSDBCMD ve MSBuild kullanarak "what IF" dağıtımları açıklanmıştır. "What IF" dağıtımı için hedef ortam gerçekte herhangi bir değişiklik yapmadan önce önerilen dağıtım etkisini değerlendirmenize olanak tanır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Web dağıtımı komut satırı sözdizimi hakkında daha fazla bilgi için bkz. [Web dağıtma işlemi ayarları](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Kullandığınız komut satırı seçenekleri hakkında rehberlik için *. deploy.cmd* bkz [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx). VSDBCMD komut satırı söz dizimi hakkında yönergeler için bkz. [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema içeri aktarma)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Önceki](advanced-enterprise-web-deployment.md)
> [İleri](customizing-database-deployments-for-multiple-environments.md)
