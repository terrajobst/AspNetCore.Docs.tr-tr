---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: En iyi uygulamalar parolaları ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtma | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici nasıl kodunuzu güvenli bir şekilde depolayın ve güvenli bilgilere gösterir. Parolaları veya diğer ğ hiçbir zaman saklamalısınız en önemli noktasıdır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28033027"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Parolalar ve diğer hassas verileri ASP.NET ve Azure uygulama hizmeti dağıtmak için en iyi uygulamalar
====================
tarafından [Rick Anderson](https://github.com/Rick-Anderson)

> Bu öğretici nasıl kodunuzu güvenli bir şekilde depolayın ve güvenli bilgilere gösterir. En önemli kaynak kodunda parolalar ve diğer hassas verileri asla saklamalısınız ve geliştirme ve test modunda üretim gizli kullanmamanız noktasıdır.
> 
> Örnek kod, basit bir Web işi konsol uygulaması ve bir veritabanı bağlantı dizesi parola, Twilio, Google ve SendGrid güvenli anahtarlar erişmesi gereken bir ASP.NET MVC uygulaması ' dir.
> 
> Şirket içi ayarlarına ve PHP da belirtiliyor.


- [Geliştirme ortamında parolaları ile çalışma](#pwd)
- [Bağlantı dizeleri geliştirme ortamında ile çalışma](#con)
- [Web işleri konsol uygulamaları](#wj)
- [Gizli Azure'a dağıtma](#da)
- [Şirket içi ve PHP için Notlar](#not)
- [Ek kaynaklar](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Geliştirme ortamında parolaları ile çalışma

Öğreticiler hassas verileri umarız kaynak kodunda hassas verileri asla saklamalısınız bir uyarı ile kaynak kodunda sık gösterir. Örneğin, Belgelerim [SMS ve e-posta 2FA ASP.NET MVC 5 uygulamayla](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) öğretici gösterir aşağıdaki *web.config* dosyası:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

*Web.config* dosyasıdır kaynak kodu, bu Sırları hiçbir zaman bu dosyada saklanan şekilde. Neyse ki, `<appSettings>` öğeye sahip bir `file` özniteliği hassas uygulama yapılandırma ayarlarını içeren bir dış dosyası belirtmenize olanak tanır. Dış dosya kaynak ağacına işaretli olduğu sürece, tüm gizli kod dizeleri harici bir dosyaya taşıyabilirsiniz. Örneğin, aşağıdaki biçimlendirme, dosyanın içinde *AppSettingsSecrets.config* tüm uygulama sırrı içerir:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Dış dosya biçimlendirmede (*AppSettingsSecrets.config* Bu örnekte), aynı biçimlendirme bulunan *web.config* dosya:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

ASP.NET çalışma zamanı sahip harici dosyasının içeriğini birleştirir &lt;appSettings&gt; öğesi. Belirtilen dosya bulunamazsa çalışma zamanı dosya özniteliğini yok sayar.

> [!WARNING]
> Güvenlik - eklemeyin, *gizli .config* dosya projenize ya da kaynak denetimine denetleyin. Varsayılan olarak, Visual Studio ayarlar `Build Action` için `Content`, dosyanın başka bir deyişle, dağıtılır. Daha fazla bilgi için bkz: [neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Uzantıyı için kullanmanız mümkün olmakla birlikte *gizli .config* , onu dosyasıdır kalmasını sağlamak en iyi *.config*yapılandırma dosyaları, IIS tarafından sunulan değil gibi. Ayrıca dikkat *AppSettingsSecrets.config* dosyasıdır iki dizin düzeylerinden yukarı *web.config* tamamen dışında çözüm dizini nedenle dosya. Dosyanın çözüm dizini dışında taşıyarak &quot;git eklemek \* &quot; deponuza ekleyin olmaz.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Bağlantı dizeleri geliştirme ortamında ile çalışma

Visual Studio oluşturur kullanan yeni ASP.NET projeleri [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Yerel veritabanı için özellikle geliştirme ortamı oluşturuldu. Bir parola gerektirmez, bu nedenle kaynak kodunuzu iade gizli engellemek için herhangi bir şey yapmanız gerekmez. Bazı geliştirme ekiplerinin parola gerektiren tam SQL Server (veya sürümlerini diğer DBMS) kullanın.

Kullanabileceğiniz `configSource` tüm değiştirmek için öznitelik `<connectionStrings>` biçimlendirme. Farklı `<appSettings>` `file` biçimlendirme birleştirir özniteliği `configSource` özniteliği işaretleme değiştirir. Aşağıdaki biçimlendirme gösterildiği `configSource` özniteliğini *web.config* dosyası:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Kullanırsanız `configSource` özniteliği harici bir dosyaya bağlantı dizelerinizi taşımak için yukarıda gösterildiği gibi ve Visual Studio'nun yeni bir web sitesi oluşturun, bir veritabanı kullanıyor ve veritabanını yapılandırma seçeneği vermeyecektir algılamak çalıştırılamayacak olduğunda, Yayımla Visual Studio'dan XPS Azure. Kullanıyorsanız `configSource` özniteliği, web sitesi ve veritabanı oluşturmak ve dağıtmak için PowerShell kullanın ya da yayımlamadan önce web siteniz ve veritabanınız portalda oluşturabilirsiniz. [Yeni AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) yeni web sitesi ve veritabanı komut dosyası oluşturur.


> [!WARNING]
> Güvenlik - aksine *AppSettingsSecrets.config* dosya, dış bağlantı dizeleri dosyası kök ile aynı dizinde olmalıdır *web.config* emin olmak için önlem gerekecek şekilde dosya Kaynak depoya denetlemez.


> [!NOTE]
> **Gizli dosya üzerinde güvenlik uyarısı:** test ve geliştirme üretim parolalarında kullanmayacak şekilde en iyi uygulamadır. Test veya geliştirme üretim parolaları kullanarak bu gizli sızdırıyor.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Web işleri konsol uygulamaları

*App.config* bir konsol uygulaması tarafından kullanılan dosya göreli yollar desteklemez, ancak mutlak yollar desteklemiyor. Gizli anahtarlarınız proje dizininiz dışına taşımak için mutlak bir yol kullanabilirsiniz. Aşağıdaki biçimlendirmede gizli anahtarları gösterir *C:\secrets\AppSettingsSecrets.config* dosya ve hassas olmayan verilerde *app.config* dosya.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Gizli Azure'a dağıtma

Web uygulamanızı Azure'a dağıtırken *AppSettingsSecrets.config* (yani istediğinizi) dosyası dağıtılmaz. Git [Azure Yönetim Portalı](https://azure.microsoft.com/services/management-portal/) ve bunu yapmak için bunları el ile ayarlayın:

1. Git [ https://portal.azure.com ](https://portal.azure.com)ve Azure kimlik bilgilerinizle oturum açın.
2. Tıklatın **Gözat &gt; Web uygulamaları**, web uygulamanızın adına tıklayın.
3. Tıklatın **tüm ayarları &gt; uygulama ayarları**.

**Uygulama ayarları** ve **bağlantı dizesi** değerleri geçersiz kılmak için aynı ayarlarında *web.config* dosya. Bu anahtarları içinde olup olmadığını ancak Örneğimizde, biz bu ayarlar, Azure'a dağıtılmayan *web.config* dosya, portalda gösterilen ayarları önceliklidir.

İzlemek için en iyi uygulamadır bir [DevOps iş akışı](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) ve [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (veya başka bir framework gibi [Chef](http://www.opscode.com/chef/) veya [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) için Azure'da bu değerleri ayarlama otomatikleştirin. Aşağıdaki PowerShell betiğini kullanır [verme CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) diske şifrelenmiş parolalar dışa aktarmak için:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

Yukarıdaki betik 'Name' gizli anahtarı gibi adıdır '&quot;FB\_AppSecret&quot; veya "TwitterSecret". Tarayıcınızda komut dosyası tarafından oluşturulan ".credential" dosyasını görüntüleyebilirsiniz. Aşağıdaki kod parçacığında kimlik bilgisi dosyaların her biri sınar ve gizli anahtarları adlandırılmış web uygulaması için ayarlar:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Güvenlik - parolaları veya diğer parolaları bunu hedefini uğratır hassas verileri dağıtmak için bir PowerShell Betiği kullanılarak amacı yapılması PowerShell Betiği dahil etmeyin. [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet'i bir parola almak için güvenli bir mekanizma sağlar. UI İstemi'ni kullanarak bir parola sızmasını engelleyebilir.


### <a name="deploying-db-connection-strings"></a>DB bağlantı dizeleri dağıtma

DB bağlantı dizeleri için uygulama ayarları benzer şekilde işlenir. Web uygulamanızı Visual Studio'dan dağıtırsanız, bağlantı dizesi sizin için yapılandırılır. Bu portalda doğrulayabilirsiniz. PowerShell ile bağlantı dizesini ayarlamak için önerilen yoldur. PowerShell komut dosyası örneği için bir Web sitesi ve veritabanı oluşturur ve bağlantı dizesini ayarlar Web sitesi karşıdan [yeni AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) gelen [Azure Kod Kitaplığı](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>PHP için Notlar

Her ikisi için anahtar-değer çiftleri itibaren **uygulama ayarları** ve **bağlantı dizeleri** ortam değişkenleri Azure App Service'te bir web uygulama çerçevelerinin (PHP gibi) kolayca kullanabilir geliştiriciler depolanır Bu değerleri alır. Stefan Schackow'ın bkz [Windows Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) blog yayını, bir uygulama ayarlarının ve bağlantı dizeleri okumak için PHP parçacık gösterir.

## <a name="notes-for-on-premises-servers"></a>Şirket içi sunucular için Notlar

Şirket içi web sunucularına dağıtıyorsanız, güvenli parolaları tarafından yardımcı olabilir [yapılandırma bölümlerini yapılandırma dosyalarının şifreleme](https://msdn.microsoft.com/library/ff647398.aspx). Alternatif olarak, Azure Web siteleri için önerilen aynı yaklaşımı kullanabilirsiniz: geliştirme ayarlarını yapılandırma dosyalarını tutmak ve üretim ayarları için ortam değişkeni değerlerini kullanın. Bu durumda, ancak Azure Web siteleri otomatik işlevselliği için uygulama kod yazmayı vardır: ortam değişkenlerinin ayarları almak ve yapılandırma dosyası ayarları yerine bu değerleri veya yapılandırma dosyası ayarları kullanın, ortam değişkenleri bulunamadı.

<a id="addRes"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

Bir PowerShell örneği için bir web uygulaması + veritabanı oluşturur komut dosyası ayarlar bağlantı dizesi + uygulama ayarları, indirme [yeni AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) gelen [Azure Kod Kitaplığı](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Stefan Schackow'ın bkz [Windows Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Özel Hakan Dorrans sayesinde ( [ @blowdart ](https://twitter.com/blowdart) ) ve gözden geçirme için Carlos Farre.
