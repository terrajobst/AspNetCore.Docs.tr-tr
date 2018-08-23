---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Parolalar ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtmak için en iyi yöntemler | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide nasıl kodunuzu güvenli bir şekilde depolayın ve güvenli bilgilere gösterilir. En önemli olan nokta, parolalar veya diğer Gönder hiçbir zaman depolamanız gerekir ediyor...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: eda2277a4baad8f2a63aa2fdf6ab84f57f1eb0e0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755621"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Parolalar ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtmak için en iyi uygulamalar
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğreticide nasıl kodunuzu güvenli bir şekilde depolayın ve güvenli bilgilere gösterilir. En önemli kaynak kodunda hiçbir zaman parolaları ve diğer hassas verileri saklamalısınız ve geliştirme ve test modunda üretim gizli dizileri kullanmamalısınız noktasıdır.
> 
> Örnek kod, basit bir WebJob konsol uygulaması ve bir veritabanı bağlantı dizesi parola, Twilio, Google ve SendGrid güvenli anahtarlar erişmesi gereken bir ASP.NET MVC uygulaması ' dir.
> 
> Şirket içi ayarları ve PHP de bahsedilir.


- [Parolaları geliştirme ortamında çalışma](#pwd)
- [Bağlantı dizeleri geliştirme ortamında çalışma](#con)
- [WebJobs konsol uygulamaları](#wj)
- [Gizli dizileri Azure'a dağıtma](#da)
- [Şirket içi ve PHP için Notlar](#not)
- [Ek Kaynaklar](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Parolaları geliştirme ortamında çalışma

Öğreticiler, kaynak kodundaki Umarım kaynak kodunda hassas verileri hiçbir zaman saklamalısınız bir uyarı ile sık hassas verileri gösterir. Örneğin, Belgelerim [2fa'yı SMS ve e-posta ile ASP.NET MVC 5 uygulaması](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) öğretici aşağıdaki gösterir *web.config* dosyası:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* dosyasıdır kaynak kodu, bu gizli dizileri hiçbir zaman bu dosyada depolanacak şekilde. Neyse ki, `<appSettings>` öğeye sahip bir `file` özniteliği hassas uygulama yapılandırma ayarlarını içeren bir dış dosya belirtmenizi sağlar. Dış dosya, kaynak ağacına işaretlenmediği sürece, harici bir dosyaya tüm gizli dizilerin taşıyabilirsiniz. Örneğin, aşağıdaki biçimlendirme, dosya içinde *AppSettingsSecrets.config* tüm uygulama gizli anahtarlarının içerir:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Dış dosya işaretlemede (*AppSettingsSecrets.config* Bu örnekte), aynı biçimlendirme bulunan *web.config* dosyası:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

ASP.NET çalışma zamanı biçimlendirmeye sahip harici dosyasının içeriğini birleştirir &lt;appSettings&gt; öğesi. Belirtilen dosya bulunamazsa, çalışma zamanı dosya özniteliğini yok sayar.

> [!WARNING]
> Güvenlik - eklemeyin, *gizli dizileri .config* dosyası projenize veya kaynak denetimine dahil etmeyin. Varsayılan olarak, Visual Studio ayarlar `Build Action` için `Content`, yani dosya dağıtılır. Daha fazla bilgi için [neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) İçin uzantıyı kullanabilirsiniz, ancak *gizli dizileri .config* dosyası sağlamak en iyisidir *.config*gibi yapılandırma dosyaları, IIS tarafından sunulan değil. Ayrıca dikkat *AppSettingsSecrets.config* dosyasıdır gelen iki dizin düzeylerini *web.config* tamamen çözüm dizini dışında bu nedenle, dosya. Dosyayı çözüm dizini dışında hareket ettirerek &quot;git ekleyin \* &quot; deponuza eklemeyeceğiz.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Bağlantı dizeleri geliştirme ortamında çalışma

Visual Studio kullanan yeni ASP.NET projeleri oluşturur [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB, özellikle geliştirme ortamı için oluşturuldu. Bir parola gerektirmez, dolayısıyla kaynak kodunuzu iade gizli dizileri engellemek için herhangi bir şey yapmanız gerekmez. Bazı geliştirme ekipleri, bir parola gerektir tam sürümü SQL Server'ı (veya diğer DBMS) kullanın.

Kullanabileceğiniz `configSource` tamamını değiştirmek için özniteliği `<connectionStrings>` biçimlendirme. Farklı `<appSettings>` `file` biçimlendirme birleştirir özniteliği `configSource` özniteliği işaretleme değiştirir. Aşağıdaki biçimlendirme gösterildiği `configSource` özniteliğini *web.config* dosyası:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Kullanırsanız `configSource` harici bir dosyaya bağlantı dizelerinizi taşımak için yukarıda gösterildiği gibi öznitelik ve Visual Studio'nun yeni bir web sitesi oluşturma, bir veritabanını kullandığınızı ve veritabanını yapılandırma seçeneği vermeyeceğiz algılamak göremezsiniz olduğunda, Yayımla Visual Studio'dan mla azure'a. Kullanıyorsanız `configSource` özniteliği oluşturmak ve web sitenizi ve veritabanınızı dağıtmak için PowerShell kullanabilirsiniz veya yayımlamadan önce web sitenizi ve veritabanınızı portalda oluşturabilirsiniz. [Yeni AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) yeni web sitesi ve veritabanı komut dosyası oluşturur.


> [!WARNING]
> Güvenlik - aksine *AppSettingsSecrets.config* dosyası, dış bağlantı dizeleri dosyası kök ile aynı dizinde olmalıdır *web.config* emin olmak için önlemler alması gerekecektir. Bu nedenle, dosya Bu, kaynak deponuza iade etmeyin.


> [!NOTE]
> **Gizli dosya çubuğunda güvenlik uyarısı:** test ve geliştirme üretim gizli dizileri kullanmak için en iyi uygulamadır. Test veya geliştirme üretim parolaları kullanarak bu gizli dizileri yayılır.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>WebJobs konsol uygulamaları

*App.config* bir konsol uygulaması tarafından kullanılan dosya göreli yolları desteklemiyor, ancak mutlak yollar desteklemiyor. Gizli anahtarlarınız proje dizini dışına taşımak için mutlak bir yol kullanabilirsiniz. Aşağıdaki biçimlendirmede gizli gösterir *C:\secrets\AppSettingsSecrets.config* hassas olmayan verilerin yanı sıra dosya *app.config* dosya.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Gizli dizileri Azure'a dağıtma

Web uygulamanızı Azure'a dağıtırken *AppSettingsSecrets.config* (yani, istediğiniz) dosyası dağıtılmaz. Git [Azure Yönetim Portalı](https://azure.microsoft.com/services/management-portal/) ve bunu yapmak için bunları el ile ayarlayın:

1. Git [ https://portal.azure.com ](https://portal.azure.com)ve Azure kimlik bilgilerinizle oturum açın.
2. Tıklayın **Gözat &gt; Web Apps**, ardından web uygulamanızın adına tıklayın.
3. Tıklayın **tüm ayarlar &gt; uygulama ayarları**.

**Uygulama ayarları** ve **bağlantı dizesi** aynı ayarları geçersiz kılma değerleri *web.config* dosya. Bu anahtarlar, olsaydı ancak Örneğimizde, biz bu ayarları Azure'a dağıtılmayan *web.config* dosya, portalda gösterilen ayarları önceliklidir.

İzlemek için en iyi uygulamadır bir [DevOps iş akışı](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) ve [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (veya başka bir framework gibi [Chef](http://www.opscode.com/chef/) veya [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) için Azure'da bu değerleri ayarlama otomatikleştirin. Aşağıdaki PowerShell betiğini kullanır [dışarı aktarma CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) şifrelenmiş gizli dizileri diske dışarı aktarmak için:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Yukarıdaki betik 'Name' gizli anahtar adı olduğu gibi '&quot;FB\_AppSecret&quot; veya "TwitterSecret". Tarayıcınızda komut dosyası tarafından oluşturulan ".credential" dosyasını görüntüleyebilirsiniz. Aşağıdaki kod parçacığında, her kimlik bilgisi dosya test eder ve adlandırılmış bir web uygulaması için gizli dizileri ayarlar:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Güvenlik - parolaları veya diğer parolaları hassas verileri dağıtmak için bir PowerShell betiğini kullanarak amacı bunu defeats yapılması PowerShell betiğini dahil değildir. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i bir parola almak için güvenli bir mekanizma sağlar. Bir kullanıcı Arabirimi istemi kullanarak parola sızıntı engelleyebilirsiniz.


### <a name="deploying-db-connection-strings"></a>DB bağlantı dizeleri dağıtma

DB bağlantı dizeleri, uygulama ayarları için benzer şekilde işlenir. Web uygulamanızı Visual Studio'dan dağıtırsanız, bağlantı dizesini sizin için yapılandırılacak. Bu portalda doğrulayabilirsiniz. PowerShell ile bağlantı dizesini ayarlamak için önerilen yoldur. Bir PowerShell Betiği örneği için bir Web sitesi ve veritabanı oluşturur ve bağlantı dizesini ayarlar indirin ve Web sitesinde [yeni AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) gelen [Azure betik kitaplığını](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>PHP için Notlar

Her ikisi için anahtar-değer çiftleri beri **uygulama ayarları** ve **bağlantı dizeleri** ortam değişkenleri Azure App Service'te bir web uygulama çerçevelerinin (PHP gibi) kolayca kullanabilir geliştiriciler depolanır Bu değerleri alın. Stefan Schackow'ın bkz [Windows Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) blog gönderisi, bir uygulama ayarlarının ve bağlantı dizelerinin okumak için bir PHP kod parçacığı gösterir.

## <a name="notes-for-on-premises-servers"></a>Şirket içi sunucular için Notlar

Şirket içi web sunucularına dağıtıyorsanız, güvenli bir gizli dizi tarafından Yardım [yapılandırma bölümlerini yapılandırma dosyalarının şifreleme](https://msdn.microsoft.com/library/ff647398.aspx). Alternatif olarak, Azure Web siteleri için önerilen aynı yaklaşımı kullanabilirsiniz: yapılandırma dosyalarında geliştirme ayarları tutun ve üretim ayarları için ortam değişkeni değerlerini kullanın. Bu durumda, ancak, Azure Web Siteleri'nde otomatik işlevleri için uygulama kodları yazmak zorunda: ortam değişkenlerinden ayarlarını alma ve yapılandırma dosyası ayarlarının yerine bu değerleri veya yapılandırma dosyası ayarlarının kullanın, ortam değişkenleri bulunamadı.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

Oluşturan bir web uygulaması + veritabanı, betiği bir PowerShell örneği için ayarlar bağlantı dizesi + uygulama ayarları, indirme [yeni AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) gelen [Azure betik kitaplığını](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Stefan Schackow'ın bkz [Windows Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Özel Barry Dorrans sayesinde ( [ @blowdart ](https://twitter.com/blowdart) ) ve gözden geçirme için Carlos Farre.
