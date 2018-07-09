---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Her şeyi (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) otomatik hale getirin | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839641"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>(Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) her şeyi otomatikleştirin
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı giriş için bkz: [ilk bölüm](introduction.md).


İnceleyeceğiz ilk üç desenler gerçekte herhangi bir yazılım geliştirme projesine, ancak özellikle bulut projeleri için geçerlidir. Geliştirme görevlerini otomatikleştirme hakkında bu modelidir. El ile gerçekleştirilen işlemleri yavaş ve hataya açık olduğundan önemli bir konudur; bir hızlı, güvenilir ve Çevik iş akışını Ayarla olası yardımcı olarak sayıda otomatikleştirme. Zor veya imkansız bir şirket içi ortamda otomatik hale getirmek olan birçok görevleri kolayca otomatikleştirebilirsiniz için bulut geliştirme için benzersiz bir şekilde önemlidir. Örneğin, tüm test ayarlayabilirsiniz. yeni bir web sunucusu ve arka uç sanal makineleri dahil olmak üzere ortamlarında, veritabanlarını, blob depolama (dosya depolama), kuyruklar vb.

## <a name="devops-workflow"></a>DevOps iş akışı

"DevOps." terimi giderek işittiğiniz Terimi yazılım verimli bir şekilde geliştirmek için geliştirme ve operasyon görevlerini tümleştirmek zorunda bir tanıma dışında geliştirilmiştir. Etkinleştirmek istediğiniz bir iş akışı türü, uygulama geliştirme, onu dağıtabilir, bunu üretim ortamından öğrenin, öğrendiklerinizi yanıt değiştirin ve hızla ve güvenle döngüyü tekrarlayın olan biridir.

Bazı başarılı bulut geliştirme ekipleri, canlı bir ortama günde birden çok kez dağıtın. Önemli dağıtmak için kullanılan Azure ekibi, ancak şimdi 2-3 ayda yayınlar küçük güncelleştirmeler her 2 ila 3 gün ve temel sürümleri 2-3 haftada güncelleştirin. Bu tempomuzu alma gerçekten, müşteri geri bildirimlerini hızlı yanıt yardımcı olur.

Bunu yapabilmek için yinelenebilir, öngörülebilir, güvenilir ve düşük döngü süresi olan bir geliştirme ve dağıtım döngüsü etkinleştirmek zorunda.

![DevOps iş akışı](automate-everything/_static/image1.png)

Diğer bir deyişle, bir özellik için bir fikriniz varsa ve müşterilerin kullandığı ve geri bildirim sağlamaya zaman arasındaki süre olabildiğinde kısa olması gerekir. İlk üç desenleri – her şeyi kaynak denetimi, otomatikleştirin ve sürekli tümleştirme ve teslim--olan bu tür bir işlemi etkinleştirmek için önerdiğimiz tüm en iyi uygulamalar hakkında.

## <a name="azure-management-scripts"></a>Azure yönetim komut dosyaları

İçinde [bu e-kitap giriş](introduction.md), web tabanlı konsoldan, Azure Yönetim Portalı gördünüz. Yönetim Portalı, tüm Azure'da dağıttığınız kaynakları yönetmek ve izlemek sağlar. Oluşturma ve web apps ve VM'ler gibi hizmetleri silme, bu hizmetlerin yapılandırma, hizmet işlemi izlemek ve VS kolay bir yoludur. Harika bir araçtır, ancak bunu kullanarak el ile yapılan bir işlemdir. Bir üretim uygulaması her boyuttaki geliştirme oluşturacağız ve bir ekip ortamında özellikle öneririz öğrenin ve Azure keşfetmek için kullanıcı Arabirimi portal üzerinden gidin ve yapacağınız art arda işlemleri işlemleri otomatik hale getirin.

Neredeyse her şeyi el ile Yönetim Portalı'nda veya Visual Studio'dan yapabilirsiniz, REST yönetim API'si çağırarak de yapılabilir. Kullanarak betikleri yazabileceğiniz [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), veya gibi açık kaynak bir çerçeve kullanın [Chef](http://www.opscode.com/chef/) veya [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Ayrıca bir Mac veya Linux ortamında Bash komut satırı aracını kullanabilirsiniz. Azure, bu farklı ortamlar için betik yazma API'leri varsa ve bunu bir [.NET Yönetim API'si](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) betik yerine kod yazmak istediğiniz durumunda.

Düzelt uygulama için bir test ortamı oluşturma ve söz konusu ortama projesini dağıtma işlemlerini otomatikleştirmek bazı Windows PowerShell betikleri oluşturduk ve biz bu komut dosyası içeriği gözden geçireceğiz.

## <a name="environment-creation-script"></a>Ortam oluşturma betiği

Baktığımızda, ilk betik adlı *yeni AzureWebsiteEnv.ps1*. Bunu test etmek için bu düzeltme uygulamaya dağıtabileceğiniz bir Azure ortamı oluşturur. Bu betik gerçekleştirdiği ana görevler aşağıda verilmiştir:

- Bir web uygulaması oluşturun.
- Bir depolama hesabı oluşturun. (Sonraki bölümde göreceğiniz gibi BLOB'lar ve Kuyruklar için gereklidir.)
- SQL veritabanı sunucusu iki veritabanı oluşturup: uygulama veritabanına ve bir üyelik veritabanı.
- Azure'da uygulama veritabanları ve depolama hesabına erişmek için kullanacağı ayarları Store.
- Dağıtımı otomatik hale getirmek için kullanılacak ayarları dosyaları oluşturur.

### <a name="run-the-script"></a>Betiği çalıştırın


> [!NOTE]
> Bu bölümde parçası betikleri ve bunları çalıştırmak için girdiğiniz komutlar örneklerini gösterir. Bu Tanıtım ve komut dosyaları çalıştırmak için bilmeniz gereken her şeyi sağlamaz. Yardım-How-to--BT yönergeler için bkz. [ek: Düzelt, örnek uygulama](the-fix-it-sample-application.md#deploybase).


Azure hizmetlerini yöneten bir PowerShell betiğini çalıştırmak için Azure PowerShell konsolunu yükleme ve, Azure aboneliğiniz ile çalışacak şekilde yapılandırmanız gerekir. Bunları kurduktan sonra bunun gibi bir komutla Düzelt ortam oluşturma betiği çalıştırabilirsiniz:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

`Name` Parametresi veritabanı ve depolama hesabı oluştururken kullanılacak adını belirtir ve `SqlDatabasePassword` parametresi, SQL veritabanı için oluşturulacak yönetici hesabının parolasını belirtir. Daha sonra inceleyeceğiz, kullanabileceğiniz diğer parametreler vardır.

![PowerShell penceresi](automate-everything/_static/image2.png)

Betik bittikten sonra Yönetim Portalı'nda oluşturulan öğeleri görebilirsiniz. İki veritabanı bulabilirsiniz:

![Veritabanları](automate-everything/_static/image3.png)

Bir depolama hesabı:

![Depolama hesabı](automate-everything/_static/image4.png)

Ve bir web uygulaması:

![Web sitesi](automate-everything/_static/image5.png)

Üzerinde **yapılandırma** sekmesi web uygulaması için depolama hesabı ayarlarını sahiptir ve SQL veritabanı bağlantı dizelerini ayarlama için düzeltme, uygulama görebilirsiniz.

![appSettings ve connectionStrings](automate-everything/_static/image6.png)

*Otomasyon* klasör şimdi de içeren bir  *&lt;websitename&gt;.pubxml* dosya. Bu dosya, yeni oluşturduğunuz Azure ortamı uygulamayı dağıtmak için Msbuild'i kullanma ayarları depolar. Örneğin:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Gördüğünüz gibi betik, bir tam test ortamı oluşturduğu ve bu işlem yaklaşık 90 saniye içinde gerçekleştirilir.

Takımınızdaki birinin bir test ortamı oluşturmak isterse, bunlar yalnızca komut dosyasını çalıştırabilirsiniz. Yalnızca hızlıdır, ancak aynı zamanda bir kullanmakta olduğunuz özdeş bir ortamı kullanıyorsanız emin olabilirler. Oldukça, söz konusu başarılara herkesin şeyler el ile Yönetim Portalı kullanıcı arabirimini kullanarak ayarlama, olarak silinemedi.

### <a name="a-look-at-the-scripts"></a>Betikleri bakma

Bu işi aslında üç betik vardır. Bir komut satırından çağırma ve bazı görevleri yapmak için diğer iki otomatik olarak kullanır:

- *Yeni AzureWebSiteEnv.ps1* ana betiğidir.

    - *Yeni AzureStorage.ps1* depolama hesabı oluşturur.
    - *Yeni AzureSql.ps1* veritabanlarını oluşturur.

### <a name="parameters-in-the-main-script"></a>Ana betik parametreleri

Ana kod *yeni AzureWebSiteEnv.ps1*, birkaç parametre tanımlar:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

İki parametre gereklidir:

- Betik web uygulamasının adı. (Bu URL için de kullanılır: `<name>.azurewebsites.net`.)
- Komut dosyası oluşturur veritabanı sunucusunun yeni yönetici kullanıcı parolası.

İsteğe bağlı parametreler veri merkezi konumu (varsayılan olarak "Batı ABD"), veritabanı sunucusu Yönetici adını (varsayılan olarak "dbuser") ve veritabanı sunucusu için bir güvenlik duvarı kuralı belirtmenize olanak verir.

### <a name="create-the-web-app"></a>Web uygulaması oluşturma

Çağırarak web uygulaması oluşturma betiği yapacağı ilk şey olan `New-AzureWebsite` için web uygulaması adı ve konumu parametre değerlerini tümleştirilmesidir cmdlet'ini:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Depolama hesabı oluşturma

Ana komut dosyası çalıştırıldıktan sonra <em>yeni AzureStorage.ps1</em> belirterek komut dosyası "<em>&lt;websitename&gt;</em>depolama" depolama hesabı adı için ve aynı veri merkezi konumu olarak web uygulaması.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Yeni AzureStorage.ps1* çağrıları `New-AzureStorageAccount` ve depolama hesabı oluşturmak için cmdlet'i hesap adını ve erişim anahtarı değerleri döndürür. Uygulama, BLOB'lar ve Kuyruklar depolama hesabında erişmek için bu değerlere ihtiyacınız olur.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Her zaman yeni bir depolama hesabı oluşturma istemeyebilirsiniz; İsteğe bağlı olarak mevcut bir depolama hesabını kullanmak üzere yönlendiren bir parametre ekleyerek betik artırabilecek.

### <a name="create-the-databases"></a>Veritabanları oluşturma

Ana komut dosyası, ardından veritabanı oluşturma betiği çalıştırır *yeni AzureSql.ps1*, sonra varsayılan veritabanı kurma ve güvenlik duvarı kural adları:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Veritabanı oluşturma betiği geliştirme makinenin IP adresini alır ve geliştirme makinesini bağlanmak ve sunucuyu yönetmek için bir güvenlik duvarı kuralı ayarlar. Veritabanı oluşturma betiği ardından veritabanlarını ayarlamak için birkaç adım geçer:

- Sunucu kullanarak oluşturur `New-AzureSqlDatabaseServer` cmdlet'i.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Sunucuyu yönetmek için ve buna bağlanmak web uygulamasını etkinleştirmek için geliştirme makinesini etkinleştirmek için güvenlik duvarı kuralları oluşturur. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Kullanarak sunucu adını ve kimlik bilgilerini içeren bir veritabanı bağlamı oluşturur `New-AzureSqlDatabaseServerContext` cmdlet'i.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` bir işlev çağıran kodun `ConvertTo-SecureString` döndürür ve parola ile şifrelemek için cmdlet'i bir `PSCredential` nesnesi aynı türü `Get-Credential` cmdlet döndürür.
- Kullanarak, uygulama veritabanını ve üyelik veritabanı oluşturur `New-AzureSqlDatabase` cmdlet'i.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Yerel olarak tanımlanan işlevi tocreates her veritabanı için bir bağlantı dizesi çağırır. Uygulama, veritabanlarına erişim için bu bağlantı dizeleri kullanır. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString bağlantı dizesi için sağlanan parametre değerlerinden oluşturan komut dosyasında tanımlanan bir işlevdir.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Veritabanı sunucusu adı ve bağlantı dizeleri bir karma tablo döndürür.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Düzeltme uygulama ayrı üyeliği ve uygulama veritabanlarını kullanır. Tek bir veritabanında hem üyeliği ve uygulama verilerini yerleştirmek mümkündür.

### <a name="store-app-settings-and-connection-strings"></a>Uygulama ayarlarının ve bağlantı dizelerinin Store

Azure, ayarları ve otomatik olarak okumaya çalıştığında uygulamaya döndürülür geçersiz kılan bağlantı dizeleri depolamanıza olanak sağlayan bir özellik olan `appSettings` veya `connectionStrings` Web.config dosyasında koleksiyonları. Bu uygulama bir alternatifidir [Web.config dönüşümlerini](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) dağıttığınızda. Daha fazla bilgi için [hassas verileri Azure'da Store](source-control.md#appsettings) ileride bu e-kitabı.

Ortam oluşturma betiği Azure tümünü depolayan `appSettings` ve `connectionStrings` uygulamanız için Azure içinde çalıştığında, veritabanları ve depolama hesabına erişmek için gereken değerleri.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[Yeni Relic](http://newrelic.com/) biz de gösteren bir telemetri çerçevedir [izleme ve Telemetri](monitoring-and-telemetry.md) bölüm. Ortam oluşturma komut ayrıca New Relic ayarlarını seçer emin olmak için web uygulaması başlatır.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Dağıtımı için hazırlama

İşlemin sonunda, ortam oluşturma komut dağıtım betiği tarafından kullanılacak dosyaları oluşturmak için iki işlevleri çağırır.

Bu işlevlerden biri bir yayımlama profili oluşturur *(&lt;websitename&gt;.pubxml* dosyası). Kod yayımlama ayarlarını almak için Azure REST API'sini çağırır ve bilgileri kaydeder bir *.publishsettings* dosya. Bir şablon dosyasıyla birlikte bu dosyadan bilgi kullanıyorsa (*pubxml.template*) oluşturmak için *.pubxml* yayımlama profilini içeren dosya. Bu iki adımlı işlem Visual Studio'da neler benzetimini yapar: indirme bir *.publishsettings* dosyası ve bir yayımlama profili oluşturmak için içeri aktarın.

Diğer işlevi oluşturmak için başka bir şablon dosyası (environment.template Web sitesi) kullanan bir *Web sitesi environment.xml* dağıtım betiği ile birlikte kullanacağınız ayarlarını içeren dosyayı *.pubxml*dosya.

### <a name="troubleshooting-and-error-handling"></a>Sorun giderme ve hata işleme

Betikler gibi programlardır: başarısız olabilir ve bunu yaptıklarında hatası ve ona neyin hakkında mümkün olduğunca bilmek istiyorsunuz. Bu nedenle, ortam oluşturma komut değerini değiştirir. `VerbosePreference` değişkeni `SilentlyContinue` için `Continue` tüm ayrıntılı iletileri görüntülenir. Ayrıca değerini değiştirir `ErrorActionPreference` değişkeni `Continue` için `Stop`, böylece bile Sonlandırıcı olmayan hatalara karşılaştığında komut dosyasını durdurur:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Herhangi bir iş yapmadan önce betiği bittiğinde geçen süreyi hesaplamak başlangıç saati depolar:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Kendi iş tamamlandıktan sonra komut geçen süreyi görüntüler:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Ve örneğin ayrıntılı iletiler anahtar her işlem için betik Yazar:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Dağıtım betiği

Hangi *yeni AzureWebsiteEnv.ps1* betik mu ortam oluşturmak için *Yayımla AzureWebsite.ps1* betik uygulama dağıtımı için yapar.

Dağıtım betiği web uygulamasından adını alır *Web sitesi environment.xml* ortam oluşturma betiği tarafından oluşturulan dosya.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Dağıtım kullanıcı parolasından alır *.publishsettings* dosyası:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Yürütülmeden [MSBuild](http://msbuildbook.com/) oluşturan ve dağıtan proje komutu:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Ve belirttiğiniz `Launch` parametresi komut satırında çağırdığı `Show-AzureWebsite` cmdlet'ini Web sitesi URL'si için varsayılan tarayıcınızı açın.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Bunun gibi bir komut ile dağıtım betiği çalıştırabilirsiniz:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Ve işlem tamamlandığında, tarayıcıyı bulutta çalışan site açılır `<websitename>.azurewebsites.net` URL'si.

![Windows Azure'a dağıtılan uygulama Düzelt](automate-everything/_static/image7.png)

## <a name="summary"></a>Özet

Bu komut dosyaları ile aynı adımları her zaman aynı sırada aynı seçenekleri kullanarak yürütülecek emin olabilirsiniz. Bu, takımın her geliştirici olmayan bir şey kaçırmayın veya bir şey uğraşmanız veya aynı şekilde başka bir takım üyesinin ortamda hem de üretim ortamlarında gerçekte çalışmaz kendi makineye özel dağıtım olduğundan emin olun yardımcı olur.

Benzer şekilde, REST API, Windows PowerShell betikleri, bir .NET dil API veya Linux veya Mac üzerinde çalıştırabileceğiniz bir Bash yardımcı programını kullanarak Yönetim Portalı'nda gerçekleştirebileceğiniz birçok Azure yönetim işlevlerini otomatik hale getirebilirsiniz

İçinde [sonraki bölümde](source-control.md) biz kaynak koda göz atmak ve neden betiklerinizi kaynak kod deponuza dahil etmek önemli olduğunu açıklayan.

## <a name="resources"></a>Kaynaklar

- [Yükleme ve Windows PowerShell için Azure yapılandırma](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Azure PowerShell cmdlet'lerini yükleme ve Azure'ı yönetmek için bilgisayarınızda hesap sertifikanın nasıl yükleneceği açıklanmaktadır. Bu, kendi PowerShell öğrenmek için kaynaklara bağlantılar da olduğundan kullanmaya başlamak için harika bir yerdir.
- [Azure betik Merkezi](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). WindowsAzure.com portalına alma başlangıç öğreticileri, cmdlet başvurusu belgeleri ve kaynak kodu ve örnek betikler bağlantılarını içeren Azure hizmetlerini yöneten betikleri geliştirme kaynakları
- [Hafta sonu ASP'den: Azure ve PowerShell ile çalışmaya başlama](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). Windows PowerShell için adanmış bir blog içinde bu gönderi, PowerShell kullanarak Azure yönetim işlevleri için harika bir giriş sağlar.
- [Yükleme ve yapılandırma Azure platformlar arası komut satırı arabirimi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Mac ve Linux yanı sıra üzerinde Windows sistemleri çalışan bir Azure betik çerçevesi için kullanmaya başlama Öğreticisi.
- [Komut satırı araçlarını indirin ve Azure SDK'ları ve araçları konu bölümünü](https://azure.microsoft.com/downloads/). Belgeler ve indirmeler için Azure komut satırı araçlarını ilgili için portal sayfası.
- [Her şey Azure yönetim kitaplıkları ve .NET ile otomatikleştirme](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman, Azure için .NET Yönetim API'si ortaya çıkarır.
- [Geliştirme ve Test ortamları için yayımlamak için Windows PowerShell betiklerini kullanarak](https://msdn.microsoft.com/library/azure/dn642480.aspx). Nasıl kullanılacağını açıklayan MSDN belgelerine Visual Studio web projeleri için otomatik olarak oluşturduğu betikleri yayımlayın.
- [Visual Studio 2013 için PowerShell Araçları](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio'da Windows PowerShell için dil desteği ekleyen visual Studio uzantısı.

> [!div class="step-by-step"]
> [Önceki](introduction.md)
> [İleri](source-control.md)
