---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
title: Web dağıtımı için doğru yaklaşım seçme | Microsoft Docs
author: jrjlee
description: Internet Information Services (IIS) Web dağıtım aracı ile (Web dağıtımı) 2.0 veya üstü çalışırken almak için kullanabileceğiniz üç ana yaklaşım vardır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 787a53fd-9901-4a11-9d58-61e0509cda45
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 2d690744687af93a69743dc6ce6c853629f61f5d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="choosing-the-right-approach-to-web-deployment"></a>Web dağıtımı için doğru yaklaşım seçme
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Internet Information Services (IIS) Web dağıtım aracı ile (Web dağıtımı) 2.0 veya üstü çalışırken, bir web sunucusuna paketlenmiş web uygulamalarınızın almak için kullanabileceğiniz üç ana yaklaşım vardır. Şunlardan birini yapabilirsiniz:
> 
> - Hedefleyerek uzak bir konumdan uygulama dağıtımı *Web Dağıtım Aracı hizmeti* (aracısı olarak da bilinen "Uzaktan") hedef sunucuda.
> - Web dağıtımı üzerinde isteğe bağlı (aracısı olarak da bilinen "temp") kullanarak uzak bir konumdan uygulamayı dağıtın.
> - Hedefleyerek uzak bir konumdan uygulama dağıtımı *IIS Web dağıtımı işleyicisi* hedef sunucuda.
> - El ile web paketi hedef sunucuya kopyalamaya ve IIS Yöneticisi'yle aktararak uygulamayı dağıtın.
> 
> Hangi yaklaşımın dağıtım için kullanmak istediğiniz üzerinde hedef web sunucularının nasıl yapılandırdığınıza bağlıdır. Bu konu, dağıtıma hangi yaklaşımın size uygun olduğuna karar vermenize yardımcı olur.


Bu tablo ana avantajları ve dezavantajları çoğunlukla her iki yaklaşımın uygun senaryoları ile birlikte her dağıtım yaklaşımın gösterir.

| Yaklaşım | Yararları | Dezavantajlar | Tipik senaryolar |
| --- | --- | --- | --- |
| Uzak Aracı | Ayarlanan kolaydır. Web uygulamaları ve içeriği düzenli güncelleştirmeler için uygundur. | Kullanıcının hedef sunucuda bir yönetici olması gerekir. Kullanıcı, alternatif kimlik bilgileri sağlayamazsınız. | Geliştirme ortamları. Sınama ortamlarında. |
| Geçici aracı | Web dağıtımı hedef bilgisayara yüklemek için gerek yoktur. Web Dağıtımı'nın en son sürümünü otomatik olarak kullanılır. | Kullanıcının hedef sunucuda bir yönetici olması gerekir. Kullanıcı, alternatif kimlik bilgileri sağlayamazsınız. | Geliştirme ortamları. Sınama ortamlarında. |
| Web dağıtımı işleyicisi | Yönetici olmayan kullanıcılar içeriği dağıtabilirsiniz. Web uygulamaları ve içeriği düzenli güncelleştirmeler için uygundur. | Ayarlamak için çok daha karmaşıktır. | Hazırlama ortamlarını. İntranet üretim ortamlarında. Barındırılan ortamlarda. |
| Çevrimdışı dağıtım | Ayarlamak çok kolaydır. Yalıtılmış ortamlar için uygundur. | Sunucu Yöneticisi el ile kopyalayın ve her zaman web paketini içeri gerekir. | Internet'e yönelik üretim ortamlarında. Yalıtılmış ağ ortamları. |
  

## <a name="using-the-remote-agent"></a>Uzak aracı kullanma

Varsayılan ayarları kullanarak bir hedef sunucuda Web dağıtımı yüklediğinizde, Web dağıtım aracı hizmetini ("Uzak Aracı") otomatik olarak yüklenir başlatıldı ve. Varsayılan olarak, bir HTTP uç noktası bu adresindeki uzak aracı sunar:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample1.cmd)]


> [!NOTE]
> Değiştirebileceğiniz [*server*] web sunucunuz makine adı ile web sunucunuz veya bir ana bilgisayar adı için bir IP adresi çözümleyen web sunucunuza.


Sunucu yöneticileri, bu uç noktası adresi belirterek bir geliştirici makine ya da bir yapı sunucusu gibi uzak bir konumdan web paketleri dağıtabilirsiniz. Örneğin, Fabrikam, Inc. adresindeki Matt Hink ContactManager.Mvc web uygulama projesi kendi Geliştirici makinede yerleşik olan varsayalım. Derleme işlemi ile birlikte bir web paketi oluşturur. bir *. deploy.cmd* paketi yüklemek için Web dağıtımı komutları içeren dosya gerekli. Matt TESTWEB1 sunucuda Sunucu Yöneticisi ise, kendisinin kendi Geliştirici makinesinde bu komutu çalıştırarak test web sunucusunda web uygulamasına dağıtabilirsiniz:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample2.cmd)]


Matt yalnızca bu tür gerekir böylece makine adı sağlarsanız, gerçekte, Web dağıtımı yürütülebilir uzak aracı uç nokta adresi çıkarımını:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample3.cmd)]


> [!NOTE]
> Komut satırı sözdizimi Web dağıtımı hakkında daha fazla bilgi ve *. deploy.cmd* dosyaları görmek [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd yüklemek](https://msdn.microsoft.com/library/ff356104.aspx).


Uzak Aracı içerik uzak bir konumdan dağıtmak için basit bir yol sunar ve bu yaklaşım da tek tıklatmayla veya otomatik dağıtım ile çalışabilirsiniz. Ancak, dağıtım komutu çalıştıran kullanıcının, aynı zamanda bir etki alanı yöneticisi veya hedef sunucuda yerel Yöneticiler grubunun bir üyesi olmalıdır. Ayrıca, komut satırında alternatif kimlik bilgileri geçiremezsiniz şekilde uzak aracı temel kimlik doğrulaması, desteklemiyor.

Uzak Aracı geliştirme veya test senaryoları, burada bir test sunucu ortamı üzerinde tam yönetici denetime sahip olmasını geliştiriciler için seyrek değil ve uygulamalar genellikle yeniden ve çok imzalanmasını dağıtımında yararlı bir yaklaşım sağlar Sık. Ancak, bu yaklaşım hazırlık veya üretim ortamları için genellikle daha az olabilir.

Uzak Aracı yaklaşım kullanan bir senaryo uçtan uca örneği için bkz: [senaryo: bir Test ortamı için Web dağıtımı yapılandırma](scenario-configuring-a-test-environment-for-web-deployment.md).

## <a name="using-the-temp-agent"></a>Geçici aracı kullanma

Temp Aracısı dağıtımı için Uzak Aracı yaklaşımı benzer bir yaklaşımdır. Ancak, bir uzak aracı yaklaşım aksine, Web dağıtımı hedef web sunucusuna yüklemeniz gerekmez. Bunun yerine, dağıtım gerçekleştirdiğinizde, Web dağıtımı web dağıtım aracı hizmetini geçici bir sürümünü hedef sunucuya yüklenir ve bu IIS içeriğinizi dağıtmak için kullanır. Dağıtım tamamlandıktan sonra tüm geçici dosyalar silinir.

Temp Aracısı sağlayıcısı ayarı kullanmak istiyorsanız eklemek **/g** dağıtım komutunuzu bayrak:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample4.cmd)]


> [!NOTE]
> Web Dağıtım Aracı hizmeti hedef bilgisayarda yüklüyse, hizmet çalışmıyorsa bile temp Aracısı'nı kullanamazsınız.


Bu yaklaşımın avantajı, hedef sunuculara Web dağıtımı yüklemelerinde korumak gerekmemesidir. Ayrıca, kaynak ve hedef bilgisayarlar aynı Web dağıtımı sürümünü çalıştırdığından emin olmak gerekmez. Ancak, bu yaklaşım uzak aracı yaklaşım aynı asıl sınırlamalara gelen öğesine içerik dağıtmak için hedef sunucuda yerel yönetici olması gerekir ve yalnızca NTLM kimlik doğrulaması desteklenen düşer. Temp Aracısı yaklaşım, aynı zamanda hedef ortam çok daha fazla başlangıç yapılandırmasını gerektirir.

Geçici aracı kullanma hakkında daha fazla bilgi için bkz: [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx) ve [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

## <a name="using-the-web-deploy-handler"></a>Web dağıtmanızı işleyicisi

IIS 7'de veya sonraki sürümleri, Web dağıtımı, IIS Web dağıtımı işleyicisi aracılığıyla bir alternatif dağıtım yaklaşımı sunar. Web dağıtımı işleyicisi yakından IIS Web yönetimi kullanıcıların uzak konumlardan IIS Web siteleri yönetmesine izin vermek için tasarlanmış hizmeti (WMSvc) tümleşiktir.

Varsayılan olarak, bir HTTP uç noktası bu adresindeki uzak aracı sunar:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample5.cmd)]


> [!NOTE]
> Değiştirebileceğiniz [*server*] web sunucunuz makine adı ile web sunucunuz veya bir ana bilgisayar adı için bir IP adresi çözümleyen web sunucunuza.


Web dağıtımı işleyicisi büyük avantajlarından uzak aracı ve geçici aracı üzerinden, uygulamalar ve içerik için belirli IIS Web sitelerini dağıtmak yönetici olmayan kullanıcıların izin vermek için IIS yapılandırabilirsiniz ' dir. Alternatif kimlik bilgileri olarak parametreler, Web dağıtımı komutlarda sağlayabilmesi için Web dağıtımı işleyicisi temel kimlik doğrulaması da destekler. Önemli dezavantajı, Web dağıtımı işleyicisi ayarlama ve yapılandırma başlangıçta çok daha karmaşık olmasıdır.

Yönetici olmayan kullanıcılar söz konusu olduğunda, Web Yönetimi Hizmeti (WMSvc) yalnızca IIS bağlanmak kullanıcının sunucu düzeyinde yapılan bağlantı yerine bir site düzeyinde bağlantı kullanarak izin verir. Belirli bir siteye erişmek için uç nokta adresi bir siteye sorgu dizesi içerebilir:


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample6.cmd)]


Örneğin, bir yapı işlemi otomatik olarak bir hazırlama ortamında her başarılı yapı sonra bir web uygulamasına dağıtmak için yapılandırıldığını varsayın. Uzak Aracı yaklaşım kullandıysanız, hedef sunucularda yönetici derleme işlem kimliği yapmanız gerekir. Buna karşılık, Web dağıtımı işleyicisi yaklaşımı kullanarak, yönetici olmayan bir kullanıcı verebilirsiniz&#x2014;**FABRIKAM\stagingdeployer** bu durumda&#x2014;yalnızca belirli bir IIS Web ve yapılandırma işlemi bu sağlayabilir web paketini dağıtmak için kimlik bilgileri.


[!code-console[Main](choosing-the-right-approach-to-web-deployment/samples/sample7.cmd)]


> [!NOTE]
> Komut satırı işlemleri Web dağıtımı ve sözdizimi hakkında daha fazla bilgi için bkz: [Web dağıtmak komut satırı başvurusu](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Kullanma hakkında daha fazla bilgi için *. deploy.cmd* dosya için bkz: [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).


Web dağıtımı işleyicisi hazırlama ortamları, barındırılan ortamlar ve sunucuya uzaktan erişim'in kullanılabilir ancak yönetici kimlik bilgileri burada intranet tabanlı üretim ortamlarında, dağıtım için yararlı bir yaklaşım sağlar.

Web dağıtımı işleyicisi yaklaşım kullanan bir senaryo uçtan uca örneği için bkz: [senaryo: Web dağıtımı için hazırlama ortamını yapılandırma](scenario-configuring-a-staging-environment-for-web-deployment.md).

## <a name="using-offline-deployment"></a>Kullanarak çevrimdışı dağıtımı

Bazı durumlarda, olası veya pratik uygulamalar ve içerik uzak bir konumdan bir IIS Web sitesine dağıtmak değil. Örneğin, kaynak ve hedef bilgisayarlar yalıtılmış ağlarda veya ağ kesimlerini olabilir veya güvenlik duvarı ilkesi, uzaktan erişim izin vermeyebilir.

Bu gibi senaryolarda paketleme ve Web dağıtımı yeteneklerini yayımlama kullanmaya devam edebilirsiniz; Yalnızca bunları uzak bir konumdan kullanamazsınız. Bunun yerine, hedef sunucuda bir yönetici web paketini sunucuya kopyalayın ve bu IIS Yöneticisi'yle içeri gerekir.

![](choosing-the-right-approach-to-web-deployment/_static/image1.png)

Çevrimdışı dağıtım yaklaşımının Internet'e üretim ortamlarında, çevre ağındaki sunucuları iç ağdaki bilgisayarlar bağlanabilirlik burada sınırlanmış olabilir genelde yararlıdır.

Uçtan uca örneği çevrimdışı dağıtım yaklaşımının kullanan bir senaryo için bkz: [senaryo: bir üretim ortamında Web dağıtım için yapılandırma](scenario-configuring-a-production-environment-for-web-deployment.md).

## <a name="further-reading"></a>Daha Fazla Bilgi

Komut satırı işlemleri Web dağıtımı ve sözdizimi hakkında daha fazla bilgi için bkz: [Web dağıtmak komut satırı başvurusu](https://technet.microsoft.com/library/dd568991(v=ws.10).aspx). Kullanma hakkında daha fazla bilgi için *. deploy.cmd* dosya için bkz: [nasıl yapılır: dağıtım paketi kullanarak bir dosya deploy.cmd Yükleme](https://msdn.microsoft.com/library/ff356104.aspx).

Web paketleri uzak bir bilgisayardan, dağıtabileceğiniz çeşitli yollar hakkında daha fazla genel yönergeler için bkz [kullanarak Web dağıtımı uzaktan](https://technet.microsoft.com/library/ee461175(WS.10).aspx). Web dağıtımı isteğe bağlı kullanma hakkında daha fazla bilgi için bkz: [Web dağıtımı isteğe bağlı](https://technet.microsoft.com/library/ee517345(WS.10).aspx).

> [!div class="step-by-step"]
> [Önceki](configuring-server-environments-for-web-deployment.md)
> [sonraki](scenario-configuring-a-test-environment-for-web-deployment.md)
