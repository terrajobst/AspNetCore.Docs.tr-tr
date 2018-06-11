---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Her şeyi (Azure ile gerçek bulut uygulamaları derleme) otomatikleştirmek | Microsoft Docs
author: MikeWasson
description: Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 2e30ab7831a10f215a08f74e61adf2d147e76543
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30875181"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Her şeyi (Azure ile gerçek bulut uygulamaları derleme) otomatikleştirme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap giriş için bkz: [ilk bölüm](introduction.md).


İnceleyeceğiz ilk üç desenleri gerçekte herhangi bir yazılım geliştirme projesinin, ancak özellikle bulut projeler için geçerlidir. Bu düzen geliştirme görevleri otomatikleştirme hakkında yöneliktir. Süreç yavaş ve hatalara eğilimli olduğundan önemli bir konudur; bir hızlı, güvenilir ve Çevik iş akışını Ayarla olası yardımcı olarak birçoğu olarak otomatikleştirme. Güç veya bir şirket içi ortamda otomatik hale getirmek imkansız olan pek çok görev kolayca otomatik hale getirebilirsiniz için bulut geliştirme için benzersiz olarak önemlidir. Örneğin, tüm test ayarlayabilirsiniz ortamları yeni bir web sunucusu ve arka uç VM'ler dahil olmak üzere, veritabanlarını, blob storage (dosya depolama), kuyruklar vb.

## <a name="devops-workflow"></a>DevOps iş akışı

"DevOps." terimi giderek duyduğunuz Terimi yazılım verimli bir şekilde geliştirmek için geliştirme ve işlemleri görevlerini tümleştirmek elinizde bir tanıma dışında geliştirmiştir. Etkinleştirmek istediğiniz iş akışı türü, bir uygulamayı geliştirme, onu dağıtabilir, bunu üretim kullanımdan öğrenin, ne öğrendiğinize yanıt değiştirmek ve döngüsü hızlı ve güvenilir bir şekilde yineleyin biridir.

Bazı başarılı bulut geliştirme ekiplerinin canlı bir ortama günde birden çok kez dağıtın. Bir ana dağıtmak için kullanılan Azure ekibi şimdi ancak 2-3 ayda sürümleri küçük güncelleştirmeler her 2-3 gün ve ana yayımları 2-3 haftada güncelleştirin. Bu tempoyla alma gerçekten müşteri geri bildirimine yanıt verebilir durumda olmanıza yardımcı olur.

Bunu yapmak için yinelenebilir, güvenilir ve tahmin edilebilir ve düşük döngü süresi olan bir geliştirme ve dağıtım döngüsü etkinleştirmek gerekiyor.

![DevOps iş akışı](automate-everything/_static/image1.png)

Diğer bir deyişle, bir özellik için bir fikir olduğunda ve müşterilerin zaman kullanmaya ve geri bildirim sağladıkları arasındaki süre olabildiğinde kısa olması gerekir. İlk üç desenleri – her şeyi kaynak denetimi otomatikleştirmek ve sürekli tümleştirme ve teslim--bu tür bir işlem etkinleştirmek için önerdiğimiz tüm en iyi uygulamalar hakkında.

## <a name="azure-management-scripts"></a>Azure yönetim komut dosyaları

İçinde [bu e-kitap giriş](introduction.md), Azure Yönetim Portalı web tabanlı konsolun gördünüz. Yönetim Portalı, izlemek ve tüm Azure'da dağıtılan kaynakları yönetmenize olanak sağlar. Oluşturma ve web uygulamaları ve VM'ler gibi hizmetleri silmek, bu hizmetleri yapılandırmak, hizmet işlemi izlemek ve benzeri için kolay bir yoludur. Harika bir araçtır, ancak bunu kullanarak el ile bir işlemdir. Herhangi bir boyuttaki bir üretim uygulaması geliştirme oluşturacağız ve bir ekip ortamında özellikle önerilir öğrenin ve Azure keşfetmek için kullanıcı Arabirimi Portalı aracılığıyla gidin ve art arda yapılması işlemleri otomatik hale getirme.

Neredeyse her şeyi Yönetim Portalı'nda veya Visual Studio'dan el ile yapabilirsiniz, geri KALAN yönetim API'sini çağırarak de yapılabilir. Komut dosyalarını kullanarak yazabilirsiniz [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), veya bir açık kaynak framework gibi kullanabilir [Chef](http://www.opscode.com/chef/) veya [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Bu gibi durumlarda, Bash komut satırı aracı ayrıca bir Mac veya Linux ortamında kullanabilirsiniz. Azure API bu farklı ortamlar için komut dosyası ve, varsa bir [.NET Yönetim API'si](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) komut dosyası yerine kodu yazma istemeniz durumunda.

Düzelt uygulaması için bir test ortamı oluşturma ve bu ortama projesini dağıtma işlemlerini otomatikleştirmek bazı Windows PowerShell komut dosyalarını oluşturduk ve biz bu komut dosyalarının içeriğini bazıları gözden geçirin.

## <a name="environment-creation-script"></a>Ortam oluşturma komut dosyası

Biz bakabilir ilk komut dosyası adlı *yeni AzureWebsiteEnv.ps1*. Test Düzelt uygulamaya dağıtabileceğiniz bir Azure ortamı oluşturur. Bu komut dosyası gerçekleştirdiği ana görevler şunlardır:

- Bir web uygulaması oluşturun.
- Bir depolama hesabı oluşturun. (Sonraki bölümde göreceğiniz gibi BLOB'ları ve kuyrukları, için gereklidir.)
- Bir SQL veritabanı sunucusu ve iki veritabanı oluşturun: uygulama veritabanına ve bir üyelik veritabanı.
- Ayarları uygulama veritabanları ve depolama hesabına erişmek için kullanacağı Azure'de depolayın.
- Dağıtımı otomatik hale getirmek için kullanılan ayarları dosyaları oluşturun.

### <a name="run-the-script"></a>Komut dosyasını çalıştır


> [!NOTE]
> Bu bölümde bu kısmı betikler ve bunları çalıştırmak için girdiğiniz komutlar örnekleri gösterir. Bu demo ve komut dosyalarını çalıştırmak için bilmeniz gereken her şeyi sağlamaz. How-to--BT yönergeler için bkz: [ek: düzeltme bu örnek uygulama](the-fix-it-sample-application.md#deploybase).


Azure hizmetlerini yöneten bir PowerShell betiğini çalıştırmak için Azure PowerShell konsolunu yükleme ve Azure aboneliğiniz ile çalışacak şekilde yapılandırmanız gerekir. Bunları kurduktan sonra bunun gibi bir komutla Düzelt ortamı oluşturma komut dosyası çalıştırabilirsiniz:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` Parametresi, veritabanını ve depolama hesabı oluştururken kullanılacak adı belirtir ve `SqlDatabasePassword` parametresi, SQL veritabanı için oluşturulan yönetici hesabının parolasını belirtir. Daha sonra inceleyeceğiz olduğunu kullanabileceğiniz diğer parametreler bulunur.

![PowerShell penceresi](automate-everything/_static/image2.png)

Komut bittikten sonra Yönetim Portalı'nda oluşturulan öğeleri görebilirsiniz. İki veritabanı bulabilirsiniz:

![Veritabanları](automate-everything/_static/image3.png)

Bir depolama hesabı:

![Depolama hesabı](automate-everything/_static/image4.png)

Ve bir web uygulaması:

![Web sitesi](automate-everything/_static/image5.png)

Üzerinde **yapılandırma** sekmesi web uygulaması için depolama hesabı ayarlarını sahiptir ve SQL veritabanı bağlantı dizelerini ayarlamak için düzeltmeyi, uygulama görebilirsiniz.

![appSettings ve connectionStrings](automate-everything/_static/image6.png)

*Otomasyon* klasörü şimdi de içeren bir  *&lt;websitename&gt;.pubxml* dosya. Bu dosya MSBuild yeni oluşturduğunuz Azure ortamı uygulamayı dağıtmak için kullanacağınız ayarlarını depolar. Örneğin:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Gördüğünüz gibi komut dosyasını bir tam test ortamı oluşturdu ve tüm işlem yaklaşık 90 saniye içinde yapılır.

Takımınızda başka birinin bir test ortamı oluşturmak isterse, yalnızca komut dosyasını çalıştırabilirsiniz. Yalnızca hızlıdır, ancak aynı zamanda bir ortamda bir kullandığınız aynı kullandıkları emin olabilirler. Herkes el ile Yönetim Portalı kullanıcı arabirimini kullanarak ayarlamaları ise emin o olarak oldukça silinemedi.

### <a name="a-look-at-the-scripts"></a>Komut dosyaları bakma

Bu iş yapmak aslında üç betikleri vardır. Bir komut satırından çağırma ve bazı görevleri yapmak için diğer iki otomatik olarak kullanır:

- *AzureWebSiteEnv.ps1 yeni* ana komut dosyasıdır.

    - *AzureStorage.ps1 yeni* depolama hesabı oluşturur.
    - *AzureSql.ps1 yeni* veritabanları oluşturur.

### <a name="parameters-in-the-main-script"></a>Ana komut dosyası parametreleri

Ana komut dosyası *yeni AzureWebSiteEnv.ps1*, çeşitli parametreleri tanımlar:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

İki parametreler gereklidir:

- Komut dosyası oluşturur web uygulamasının adı. (Bu da URL'si kullanılır: `<name>.azurewebsites.net`.)
- Komut dosyası oluşturur veritabanı sunucunuzun yeni yönetici kullanıcı parolası.

İsteğe bağlı parametreler, veri merkezi konumu ("Batı ABD" varsayılan değeri), veritabanı sunucusu yönetici adı ("dbuser" varsayılanlara) ve veritabanı sunucusu için bir güvenlik duvarı kuralı belirtmenize olanak verir.

### <a name="create-the-web-app"></a>Web uygulaması oluşturma

Komut dosyası yapacağı ilk şey web uygulaması çağırarak oluşturmaktır `New-AzureWebsite` için web uygulaması adı ve konumu parametre değerlerini tümleştirilmesidir cmdlet:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Depolama hesabı oluşturma

Ana komut dosyasını çalıştırır sonra <em>yeni AzureStorage.ps1</em> komut dosyası, belirtme "<em>&lt;websitename&gt;</em>depolama" depolama hesabı adı için ve aynı veri merkezi konumu olarak web uygulaması.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*AzureStorage.ps1 yeni* çağrıları `New-AzureStorageAccount` depolama hesabı ve oluşturmak için cmdlet'i hesap adı ve erişim anahtarı değerleri döndürür. Uygulama, BLOB'lar ve kuyruklarla depolama hesabındaki erişim için bu değerleri gerekir.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Her zaman yeni bir depolama hesabı oluşturma istemeyebilirsiniz; İsteğe bağlı olarak mevcut bir depolama hesabını kullanmak üzere yönlendiren bir parametresini ekleyerek betik artırabilecek.

### <a name="create-the-databases"></a>Veritabanı oluşturma

Veritabanı oluşturma komut dosyasında ardından ana komut dosyası çalıştırır *yeni AzureSql.ps1*, sonra varsayılan veritabanı kurma ve güvenlik duvarı kuralı adları:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Veritabanı oluşturma komut dosyasında geliştirme makinenin IP adresini alır ve geliştirme makine bağlanmak ve sunucuyu yönetmek için bir güvenlik duvarı kuralı ayarlar. Veritabanı oluşturma komut dosyasında sonra veritabanlarını ayarlamak için birkaç adım geçer:

- Sunucu kullanarak oluşturur `New-AzureSqlDatabaseServer` cmdlet'i.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Sunucu yönetmek ve buna bağlanmak web uygulamasını etkinleştirmek için geliştirme makine etkinleştirmek için güvenlik duvarı kuralları oluşturur. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Kullanarak sunucu adını ve kimlik bilgilerini içeren bir veritabanı bağlamı oluşturan `New-AzureSqlDatabaseServerContext` cmdlet'i.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` çağıran komut dosyası işlevinde `ConvertTo-SecureString` döndürür ve parola şifrelemek için cmdlet bir `PSCredential` nesnesi, aynı tür `Get-Credential` cmdlet'i döndürür.
- Uygulama veritabanı ve üyelik veritabanı kullanarak oluşturur `New-AzureSqlDatabase` cmdlet'i.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Yerel olarak tanımlanmış işlevi tocreates her veritabanı için bir bağlantı dizesi çağırır. Uygulamanın bu bağlantı dizeleri veritabanlarına erişmek için kullanır. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString bağlantı dizesi için sağlanan parametre değerleri oluşturur komut dosyasında tanımlanan bir işlev değil.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Veritabanı Sunucu adı ve bağlantı dizeleri bir karma tablosu döndürür.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Düzelt uygulaması ayrı üyeliği ve uygulama veritabanlarını kullanır. Üyelik ve uygulama verilerini tek bir veritabanında yerleştirme mümkündür.

### <a name="store-app-settings-and-connection-strings"></a>Mağaza uygulaması ayarlarını ve bağlantı dizeleri

Azure, ayarları ve otomatik olarak okumaya çalıştığında ne uygulamaya döndürülür geçersiz kılma bağlantı dizeleri depolamanıza olanak sağlayan bir özellik olan `appSettings` veya `connectionStrings` Web.config dosyasındaki koleksiyon. Bu uygulama için bir alternatifidir [Web.config dönüşümleri](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) dağıttığınızda. Daha fazla bilgi için bkz: [hassas verileri Azure depolama](source-control.md#appsettings) bu e-kitap daha sonra.

Ortam oluşturma komut dosyasında depolar Azure tümü `appSettings` ve `connectionStrings` uygulama Azure içinde çalıştığında, veritabanları ve depolama hesabı erişmesi gereken değerleri.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) biz gösterme telemetri çerçevesi [izleme ve Telemetri](monitoring-and-telemetry.md) bölüm. Ortam oluşturma komut dosyasında da New Relic ayarlarını Çekmeleri emin olmak için web uygulaması yeniden başlatır.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Dağıtım için hazırlama

İşlemin sonunda, ortamı oluşturma komut dosyasında dağıtım betiği tarafından kullanılacak dosyaları oluşturmak için iki işlevi çağırır.

Bu işlevler birini bir yayımlama profili oluşturur *(&lt;websitename&gt;.pubxml* dosyası). Kod yayımlama ayarlarını almak için Azure REST API'sini çağırır ve bilgileri kaydeder bir *.publishsettings* dosya. Bir şablon dosyası ile birlikte bu dosyadan bilgi kullanıyorsa (*pubxml.template*) oluşturmak için *.pubxml* yayımlama profilini içeren dosya. Visual Studio'da ne bu iki adımlı işlem taklit eder: indirin bir *.publishsettings* dosyası ve bir yayımlama profili oluşturmak için içeri aktarın.

Diğer işlevi oluşturmak için başka bir şablon dosyası (Web sitesi-environment.template) kullanan bir *Web sitesi environment.xml* dağıtım komut dosyası ile birlikte kullanacağınız ayarlarını içeren dosyası *.pubxml*dosya.

### <a name="troubleshooting-and-error-handling"></a>Sorun giderme ve hata işleme

Komut dosyaları gibi programlardır: başarısız olabilir ve bunu yaptıklarında, hata ve neyin neden olduğunu hakkında olabildiğince bilmek isteriz. Bu nedenle, ortam oluşturma komut dosyasında değerini değiştirir `VerbosePreference` değişkeni `SilentlyContinue` için `Continue` böylece tüm ayrıntılı iletileri görüntülenir. Ayrıca değerini değiştirir `ErrorActionPreference` değişkeni `Continue` için `Stop`, böylece Sonlandırıcı olmayan hatayla karşılaştığında zaman bile komut dosyasını durdurur:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Herhangi bir iş yapmadan önce komut başlangıç saati saklar, böylece doldurduktan geçen süre hesaplayabilirsiniz:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Kendi iş tamamlandıktan sonra komut geçen süreyi görüntüler:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Ve örneğin ayrıntılı iletiler anahtar her işlem için betik Yazar:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Dağıtım betiği

Ne *yeni AzureWebsiteEnv.ps1* komut dosyası ortamı oluşturma için gerçekleştirir *Yayımla AzureWebsite.ps1* komut dosyası için uygulama dağıtımı gerçekleştirir.

Dağıtım betiğini web uygulamasının adını alır *Web sitesi environment.xml* ortamı oluşturma komut dosyası tarafından oluşturulan dosya.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Dağıtım kullanıcı parolasından alır *.publishsettings* dosyası:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Yürütülmeden [MSBuild](http://msbuildbook.com/) oluşturan ve projesini dağıtır komutu:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Ve belirlediğiniz `Launch` parametresi komut satırında çağırır `Show-AzureWebsite` cmdlet'ini Web sitesi URL'si için varsayılan tarayıcınızı açın.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Bunun gibi bir komutla dağıtım betiğini çalıştırın:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Ve yapıldığında, tarayıcı bulutta çalışan site açılır `<websitename>.azurewebsites.net` URL.

![Bir Azure projesi uygulama Düzelt](automate-everything/_static/image7.png)

## <a name="summary"></a>Özet

Bu komut dosyaları ile aynı adımları her zaman aynı seçenekleri kullanarak aynı sırada yürütülür emin olabilir. Bu, her geliştirici ekipteki değil bir şey eksik veya bir şey uğraşmanız veya aynı şekilde başka bir ekip üyesinin ortamında veya üretim gerçekte çalışmaz kendi makinede özel bir şey dağıtmak emin olun yardımcı olur.

Benzer şekilde, REST API, Windows PowerShell komut dosyaları, bir .NET dili API veya Linux veya Mac üzerinde çalışabilecek bir Bash yardımcı programını kullanarak Yönetim Portalı'nda yapabilirsiniz çoğu Azure yönetim işlevlerini otomatik hale getirebilirsiniz

İçinde [sonraki bölümde](source-control.md) biz kaynak koduna bakmanız ve neden komut içinde kaynak kod deponuza dahil etmek önemli olduğunu açıklar.

## <a name="resources"></a>Kaynaklar

- [Windows PowerShell için Azure yükleyip](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Azure PowerShell cmdlet'lerini yükleme ve Azure yönetmek için bilgisayarınızda hesap sertifikanın nasıl yükleneceği açıklanmaktadır. Bu, ayrıca PowerShell kendisini öğrenme için kaynaklarına bağlantılar içerdiğinden başlamak için harika bir yerdir.
- [Azure Komut Merkezi](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). WindowsAzure.com portalına başlatılan öğreticileri, cmdlet başvuru belgeleri ve kaynak kodu ve örnek komut dosyalarını almak için bağlantılarla Azure hizmetlerini yöneten betikleri geliştirmek için kaynaklar
- [Hafta sonu komut dosyası yazarları: Azure PowerShell ile çalışmaya başlama](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). Windows PowerShell için adanmış bir blog içinde bu post PowerShell kullanarak Azure yönetim işlevleri için harika bir giriş sağlar.
- [Yükleme ve Azure platformlar arası komut satırı arabirimi yapılandırma](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Mac ve Linux aynı zamanda Windows sistemleri çalışan Azure bir komut dosyası framework için başlama Öğreticisi.
- [Komut satırı araçları bölümüne karşıdan Azure SDK'lar ve Araçlar](https://azure.microsoft.com/downloads/). Belgeler ve yüklemeler için Azure komut satırı araçları ilgili için portal sayfası.
- [Her şeyi Azure yönetim kitaplıklarını ve .NET ile otomatikleştirme](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman Azure .NET Yönetim API'si tanıtır.
- [Geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betiklerini kullanarak](https://msdn.microsoft.com/library/azure/dn642480.aspx). Visual Studio web projeleri için otomatik olarak oluşturan komut dosyaları nasıl kullanılacağı açıklanmaktadır MSDN belgelerine yayımlayın.
- [Visual Studio 2013 için PowerShell Araçları](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio'da Windows PowerShell için dil desteği ekler visual Studio uzantısı.

> [!div class="step-by-step"]
> [Önceki](introduction.md)
> [sonraki](source-control.md)
