---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Web dağıtımı için doğru yaklaşımı seçme | Microsoft Docs
author: jrjlee
description: Internet Information Services (IIS) Web dağıtım aracı ile (Web dağıtımı) 2.0 veya sonraki çalışırken almak için kullanabileceğiniz üç ana yaklaşım vardır...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: eb1b7d50e5d7461d760ad7a963cc70369b7a4513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807057"
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Web dağıtımı için doğru yaklaşımı seçme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Internet Information Services (IIS) Web dağıtım aracı ile (Web dağıtımı) 2.0 veya sonraki çalışırken, bir web sunucusuna paketlenmiş web uygulamalarınızı almak için kullanabileceğiniz üç ana yaklaşım vardır. Şunlardan birini yapabilirsiniz:
> 
> - Uygulamayı uzak bir konumdan hedefleyerek dağıtma *Web Dağıtım Aracı hizmeti* (aracısı olarak da bilinen "uzak") hedef sunucuda.
> - Web dağıtımı üzerine (aracısı olarak da bilinen "temp") kullanarak uzak bir konumdan uygulamayı dağıtın.
> - Uygulamayı uzak bir konumdan hedefleyerek dağıtma *IIS Web dağıtımı işleyicisi* hedef sunucuda.
> - El ile web paketi hedef sunucuya kopyalayıp IIS Yöneticisi'yle alma uygulamayı dağıtın.
> 
> Hangi yaklaşımın dağıtım için kullanmak istediğiniz üzerinde hedef web sunucularınızın nasıl yapılandırdığınıza bağlıdır. Bu konuda, dağıtıma hangi yaklaşımın size uygun olduğuna karar vermenize yardımcı olur.


Bu tablo, ana olumlu ve olumsuz en tipik olarak her bir yaklaşıma uyacak senaryoları ile birlikte her dağıtım yaklaşımın gösterir.

| Yaklaşım | Yararları | Dezavantajlar | Tipik senaryoları |
| --- | --- | --- | --- |
| Uzak Aracı | Ayarlamak kolay bir işlemdir. Web uygulamaları ve içeriği düzenli güncelleştirmeler için uygundur. | Hedef sunucuda bir yönetici kullanıcı olması gerekir. Kullanıcı, alternatif kimlik bilgileri sağlayamazsınız. | Geliştirme ortamlarını. Test ortamları. |
| Geçici aracı | Web dağıtımı hedef bilgisayara yüklemek için gerek yoktur. Web Dağıtımı'nın en son sürümünü otomatik olarak kullanılır. | Hedef sunucuda bir yönetici kullanıcı olması gerekir. Kullanıcı, alternatif kimlik bilgileri sağlayamazsınız. | Geliştirme ortamlarını. Test ortamları. |
| Web dağıtımı işleyicisi | Yönetici olmayan kullanıcılar içeriği dağıtabilirsiniz. Web uygulamaları ve içeriği düzenli güncelleştirmeler için uygundur. | Ayarlamak için çok daha karmaşıktır. | Hazırlama ortamları. İntranet üretim ortamları. Barındırılan ortamlarda. |
| Çevrimdışı dağıtım | Ayarlamak çok kolaydır. Ayrık ortamlar için uygundur. | Sunucu Yöneticisi el ile kopyalamanız ve her web paketi içeri gerekir. | Internet'e yönelik üretim ortamları. Ağdan yalıtılmış ortamlara. |
  

## <a name="using-the-remote-agent"></a>Uzak aracı kullanma

Varsayılan ayarları kullanarak bir hedef sunucuda Web dağıtımı yükleme sırasında Web dağıtımı Aracı hizmeti ("Uzak Aracı") otomatik olarak yüklenir çalışmaya ve. Varsayılan olarak, uzak aracı bu adresten bir HTTP uç noktasını kullanıma sunar:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Değiştirebilirsiniz [*sunucu*] web sunucunuzun makine adıyla web sunucunuz ya da bir ana bilgisayar adı için bir IP adresi çözümleyen web sunucunuza.


Sunucu yöneticileri, bu uç nokta adresi belirterek bir geliştirici makine veya bir yapı sunucusu gibi uzak bir konumdan web paketleri dağıtabilirsiniz. Örneğin, Fabrikam, Inc., Matt Hink ContactManager.Mvc web uygulaması projesi kendi Geliştirici makinesinde yerleşik olan varsayalım. Yapı işlemi ile birlikte bir web paketi oluşturur. bir *. deploy.cmd* paketini yüklemek için gerekli Web dağıtımı komutları içeren dosya. Matt TESTWEB1 sunucuda Sunucu Yöneticisi, kendisinin kendi Geliştirici makinesinde bu komutu çalıştırarak test web sunucusunun web uygulamasına dağıtabilirsiniz:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Matt yalnızca bu tür gerekir makine adı sağlarsanız, gerçekte Web dağıtımı yürütülebilir dosyayı uzak aracı uç nokta adresini çıkarabilir:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Web dağıtımı komut satırı sözdizimi hakkında daha fazla bilgi ve *. deploy.cmd* dosyaları görmek [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).


Uzak Aracı uzak bir konumdan içerik dağıtmak için basit bir yol sunar ve bu yaklaşım tek tıklamayla veya otomatik dağıtım ile de çalışabilir. Ancak, dağıtım komutu çalıştıran kullanıcının, aynı zamanda bir etki alanı yöneticisi veya hedef sunucuda yerel Yöneticiler grubunun bir üyesi olmalıdır. Ayrıca, alternatif kimlik bilgilerini komut satırına geçirilemez için temel kimlik doğrulaması, uzak aracı desteklemiyor.

Uzak Aracı geliştirme veya test senaryoları, burada bir test sunucu ortamı üzerinde tam yönetici denetiminiz geliştiricilerin durumdur ve uygulamalar genellikle yeniden ve çok yeniden dağıtım için yararlı bir yaklaşım sağlar. Sık. Ancak, bu yaklaşım hazırlık veya üretim ortamları için genellikle daha az olabilir.

Uzak Aracı yaklaşım kullanan bir senaryoyu uçtan uca örneği için bkz: [senaryo: Web dağıtımı için bir Test Ortamı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Geçici aracı kullanma

Dağıtım geçici aracı yaklaşımı, uzak aracı yaklaşımı benzer. Ancak, uzak aracı yaklaşımın aksine, Web dağıtımı hedef web sunucusuna yüklemeniz gerekmez. Bunun yerine, dağıtım gerçekleştirdiğinizde, Web dağıtımı hedef sunucuda geçici bir web dağıtımı Aracı hizmeti sürümü yükler ve bu içeriğinizi IIS'ye dağıtmayı kullanır. Dağıtım tamamlandığında, tüm geçici dosyaları kaldırılır.

Geçici aracı sağlayıcısı ayarı kullanmak istiyorsanız ekleme **/g** bayrağı, dağıtım komut:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Web Dağıtım Aracı hizmeti hedef bilgisayarda yüklü değilse hizmet çalışmıyor olsa bile, geçici aracı kullanamazsınız.


Bu yaklaşımın avantajı, Web dağıtımı yüklemeleri, hedef sunuculara sürdürmeniz gerekmez ' dir. Ayrıca, kaynak ve hedef bilgisayarlar aynı Web dağıtımı sürümünü çalıştırdığından emin olun gerek yoktur. Ancak, bu yaklaşım gelen uzak aracı yaklaşım olarak aynı asıl sınırlamaları yani içerik dağıtmak için hedef sunucuda yerel yönetici olmalıdır ve yalnızca NTLM kimlik doğrulaması desteklenir düşer. Geçici aracı yaklaşım, aynı zamanda hedef ortamın çok fazla ilk yapılandırması gerektirir.

Geçici aracı kullanma hakkında daha fazla bilgi için bkz. [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx) ve [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Kullanarak Web dağıtımı işleyicisi

IIS 7'de ve sonraki sürümlerde, Web dağıtımı, IIS Web dağıtımı işleyicisi aracılığıyla bir alternatif dağıtım yaklaşımı sunar. Web dağıtımı işleyicisi yakından IIS Web yönetimi uzak konumlardan IIS Web sitelerini yönetmek kullanıcılara izin vermek için tasarlanmış olan hizmeti (WMSvc) tümleşiktir.

Varsayılan olarak, uzak aracı bu adresten bir HTTP uç noktasını kullanıma sunar:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Değiştirebilirsiniz [*sunucu*] web sunucunuzun makine adıyla web sunucunuz ya da bir ana bilgisayar adı için bir IP adresi çözümleyen web sunucunuza.


Uzak Aracı ve geçici bir aracı üzerinde büyük avantajı, Web dağıtımı işleyicisi, yönetici olmayan kullanıcıların belirli IIS Web siteleri, uygulamalar ve içerik dağıtmak IIS yapılandırma ' dir. Parametre olarak alternatif kimlik bilgileri, Web dağıtımı komutlarında sağlayabilmesi için Web dağıtımı işleyicisi temel kimlik doğrulaması da destekler. Önemli dezavantajı, Web dağıtımı işleyicisi ayarlamak ve yapılandırmak başlangıçta çok daha karmaşık olmasıdır.

Yönetici olmayan kullanıcılar söz konusu olduğunda, Web Yönetimi Hizmeti (WMSvc) yalnızca IIS bağlanmak kullanıcının sunucu düzeyinde bağlantı yerine bir site düzeyinde bağlantı kullanarak izin verir. Belirli bir siteye erişmek için uç nokta adresi bir siteye sorgu dizesi içerebilir:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Örneğin, bir yapı işlemi otomatik olarak bir hazırlama ortamına her başarılı derlemeden sonra bir web uygulamasına dağıtmak için yapılandırılmış olduğunu varsayalım. Uzak Aracı yaklaşım kullandıysanız, yapı işlem kimliği, hedef sunucularda yönetici yapmak gerekir. Buna karşılık, Web dağıtımı işleyicisi yaklaşımı kullanarak, yönetici olmayan kullanıcı verebilirsiniz&#x2014;**FABRIKAM\stagingdeployer** bu durumda&#x2014;izni yalnızca belirli bir IIS Web ve yapı işlemi bunları sağlayabilir web paketini dağıtmak için kimlik bilgileri.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Komut satırı işlemlerinin Web dağıtımı ve söz dizimi hakkında daha fazla bilgi için bkz. [dağıtma komut satırı başvuru Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Kullanma hakkında daha fazla bilgi için *. deploy.cmd* bkz [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).


Web dağıtımı işleyicisi dağıtım ortamı, barındırılan ortamlar ve burada, uzaktan erişim sunucusu için kullanılabilir ancak yönetici kimlik bilgileri intranet tabanlı üretim ortamı hazırlama için kullanışlı bir yaklaşım sağlar.

Web dağıtımı işleyicisi yaklaşım kullanan bir senaryoyu uçtan uca örneği için bkz: [senaryo: Web dağıtımı için hazırlama ortamı yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Çevrimdışı dağıtımı kullanma

Bazı durumlarda, bunu mümkün ve pratik uygulamalar ve içerik, uzak bir konumdan bir IIS Web sitesine dağıtmak değil. Örneğin, kaynak ve hedef bilgisayarlar yalıtılmış ağlarda veya ağ kesimlerini olabilir ya da güvenlik duvarı ilkesi uzaktan erişime izin verilmez.

Bu gibi senaryolarda, paketleme ve Web dağıtımı yeteneklerini yayımlama kullanmaya devam edebilirsiniz; Yalnızca bunları uzak bir konumdan kullanamazsınız. Bunun yerine, hedef sunucuda bir yönetici web paketini sunucuya kopyalayın ve bu IIS Yöneticisi'yle içeri aktarılması gerekir.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Çevrimdışı dağıtım yaklaşım genellikle çevre ağındaki sunucuları iç ağdaki bilgisayarlar ile bağlantı burada sınırlanmış olabilir, Internet'e yönelik üretim ortamlarında yararlıdır.

Uçtan uca çevrimdışı dağıtım yaklaşımını kullanan bir senaryo örneği için bkz [senaryo: Web dağıtımı için bir üretim ortamı yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Daha Fazla Bilgi

Komut satırı işlemlerinin Web dağıtımı ve söz dizimi hakkında daha fazla bilgi için bkz. [dağıtma komut satırı başvuru Web](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Kullanma hakkında daha fazla bilgi için *. deploy.cmd* bkz [nasıl yapılır: bir dağıtım paketini kullanarak dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).

Web paketleri uzak bir bilgisayardan, dağıtabileceğiniz çeşitli yollar hakkında daha fazla genel yönergeler için bkz. [kullanarak Web dağıtma uzaktan](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Web dağıtımı isteğe bağlı kullanma hakkında daha fazla bilgi için bkz. [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Önceki](configuring-server-environments-for-web-deployment.md)
> [İleri](scenario-configuring-a-test-environment-for-web-deployment.md)
