---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: MSBuild proje dosyalarından Windows PowerShell betikleri çalıştırma | Microsoft Docs
author: jrjlee
description: Bu konu, bir derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar. Bir betiği yerel olarak çalıştırabilirsiniz (diğer bir deyişle, b'de...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: faedcee480b6c50dc560055206fedbe7af4d5f67
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803155"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>MSBuild proje dosyalarından Windows PowerShell betikleri çalıştırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, bir derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar. (Diğer bir deyişle, yapı sunucusunda) bir betik üzerinde yerel olarak çalıştırmak veya uzaktan bir hedef web sunucusu veya veritabanı sunucusu üzerindeki ister.
> 
> Neden bir dağıtım sonrası Windows PowerShell Betiği çalıştırmak isteyebilirsiniz nedeni çok sayıda vardır. Örneğin, aşağıdakileri yapabilirsiniz:
> 
> - Özel olay kaynağı, kayıt defterine ekleyin.
> - Karşıya yükleme için bir dosya sistemi dizini oluşturun.
> - Derleme dizinler temizleyin.
> - Girişler, özel bir günlük dosyasına yazmak.
> - Yeni sağlanan web uygulamasının kullanıcıları davet e-posta gönderin.
> - Uygun izinlere sahip kullanıcı hesapları oluşturun.
> - SQL Server örnekleri arasında çoğaltmayı yapılandırın.
> 
> Bu konu, Windows PowerShell betiklerini yerel olarak veya uzaktan Microsoft Build Engine (MSBuild) proje dosyasında özel bir hedef nasıl çalıştırılacağı gösterilmektedir.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bir otomatik olarak veya tek adımlı dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak için bu üst düzey görevleri tamamlamanız gerekir:

- Windows PowerShell Betiği, çözümünüze ve kaynak denetimine ekleyin.
- Windows PowerShell komut dosyasını çağıran bir komut oluşturun.
- Komutunuz tüm ayrılmış XML karakterlerini çıkış.
- Özel MSBuild proje dosyanızda hedef oluşturma ve kullanma **Exec** komutunuz çalıştırılacak görev.

Bu konuda, bu yordamları gerçekleştirmek nasıl gösterilmektedir. Görevleri ve bu konudaki yönergeler zaten MSBuild hedefleri ve özellikleri konusunda bilgi sahibi olduğunuz, ve özel bir MSBuild proje dosyası bir derleme ve dağıtım sürecini sürücü için nasıl kullanılacağını anladığınızı varsayar. Daha fazla bilgi için [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Oluşturma ve Windows PowerShell betiklerini ekleme

Bu konu başlığı altındaki görevleri adlandırılmış bir örnek Windows PowerShell betiğini kullanın **LogDeploy.ps1** nden Msbuild'i komut dosyalarını çalıştırmak nasıl göstermek için. **LogDeploy.ps1** betik, tek satırlık giriş bir günlük dosyasına yazan basit bir işlevi içerir:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


**LogDeploy.ps1** betiği iki parametre kabul eder. İlk parametre bir giriş eklemek istediğiniz günlük dosyasına tam yolunu temsil eder ve ikinci parametre günlük dosyasını kaydetmek istediğiniz dağıtım hedefi temsil eder. Betiği çalıştırdığınızda, günlük dosyası şu biçimde bir satır ekler:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Yapmak **LogDeploy.ps1** MSBuild için kullanılabilir komut dosyası, şunları yapmanız gerekir:

- Betik, kaynak denetimine ekleyin.
- Visual Studio 2010'daki çözümünüze betiği ekleyin.

Yapı sunucusunda veya uzak bir bilgisayarda komut dosyasını çalıştırmak planlama bağımsız olarak çözüm içeriğinizi betiğiyle dağıtmak gerekmez. Komut dosyası için bir çözüm klasörü ekleme bir seçenektir. Windows PowerShell komut dosyası dağıtım sürecinin bir parçası olarak kullanmak istediğiniz çünkü Kişi Yöneticisi örnekte yayımlama Çözüm klasörü için komut dosyası eklemek için mantıklıdır.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Çözüm klasörleri içeriğini yapı sunucusu kaynak malzeme kopyalanır. Ancak, herhangi bir proje çıktı işlevinin hiçbir bölümü oluşturur.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Yapı sunucusunda bir Windows PowerShell Betiği yürütülüyor

Bazı senaryolarda projelerinizi oluşturan bilgisayarda Windows PowerShell betiklerini çalıştırmak isteyebilirsiniz. Örneğin, yapı klasörleri temizlemek veya girişleri özel bir günlük dosyasına yazmak için bir Windows PowerShell Betiği kullanabilirsiniz.

Söz dizimi bakımından, bir Windows PowerShell Betiği çalıştıran bir MSBuild proje dosyasından normal bir komut istemi'nden Windows PowerShell Betiği çalıştırıyor ile aynı olur. Yürütülebilir powershell.exe çağırmak ve kullanmak için ihtiyaç duyduğunuz **– komut** Windows PowerShell'i çalıştırmak istediğiniz komutları sağlamak için anahtar. (Windows PowerShell v2'de de kullanabilirsiniz **– dosya** geçiş). Komut şöyle izlemesi gerekir:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Örneğin:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Betik için yolu boşluk içeriyorsa, dosya yolu ve işareti tarafından tek tırnak içine almanız gerekir. Komutu içine almak için zaten kullandığınız için çift tırnak kullanamazsınız:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Bu komuttan MSBuild çağırdığınızda, bazı ek hususlar vardır. İlk olarak, içermelidir **– NonInteractive** bayrak betik sessizce çalıştığından emin olun. Ardından, içermelidir **– ExecutionPolicy** bayrağına sahip bir uygun bağımsız değişken değeri. Bu Windows PowerShell, komut dosyanız için uygulanır ve komut dosyası yürütülmesinin hızlanması engelleyebilir varsayılan yürütme ilkesini geçersiz kılmanıza da olanak tanır yürütme ilkesini belirtir. Bu bağımsız değişken değerler arasından seçim yapabilir:

- Değerini **Kısıtlanmamış** komut dosyası imzalı olup bağımsız olarak betiğinizi çalıştırmak Windows PowerShell izin verir.
- Değerini **RemoteSigned** Windows PowerShell, yerel makine üzerinde oluşturulan imzalanmamış komut dosyalarının çalışmasına izin verir. Ancak, diğer yerlerde oluşturulan komut dosyalarını oturum açmanız gerekir. (Uygulamada, bir Windows PowerShell Betiği yerel olarak bir yapı sunucusunda oluşturduğunuz çok düşüktür).
- Değerini **AllSigned** yalnızca imzalı betiklere yürütmek Windows PowerShell izin verir.

Varsayılan yürütme İlkesi **kısıtlı**, engelleyen Windows PowerShell çalışmasını herhangi komut dosyaları.

Son olarak, Windows PowerShell komutunda oluşan herhangi bir ayrılmış XML karakterleri kaçış yapmanız gerekir:

- Tek tırnak işaretleri yerine  **&amp;apos;**
- Çift tırnak işareti yerine  **&amp;quot;**
- İle kodlarına değiştirin  **&amp;amp;**

- Bu değişiklik yaptığınızda bu komutunuz benzeyecektir:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


Özel MSBuild proje dosyası içinde yeni bir hedef oluşturabilir ve kullanabilirsiniz **Exec** görev bu komutu çalıştırmak için:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


Bu örnekte, dikkat edin:

- Parametre değerlerini ve Windows PowerShell yürütülebiliri konumu gibi herhangi bir değişkeni, MSBuild özellikleri bildirilir.
- Koşullar, komut satırından bu değerleri geçersiz kılmak kullanıcıları etkinleştirmek için dahil edilir.
- **MSDeployComputerName** özelliği başka bir proje dosyasında bildirilir.

Yapı işleminizin bir parçası olarak bu hedef yürüttüğünüzde, Windows PowerShell komutu çalıştırın ve bir günlük girişi, belirtilen dosyaya yaz.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Uzak bir bilgisayarda bir Windows PowerShell Betiği yürütülüyor

Windows PowerShell betikleri uzak bilgisayarlarda çalıştırabilen [Windows Uzaktan Yönetimi](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Bunu yapmak için kullanmanız gerekir [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet'i. Bu, uzak bilgisayarlara betik kopyalamadan betiğinizi bir veya daha fazla uzak bilgisayar karşı yürütmek sağlar. Betiğin çalıştırıldığı yerel bilgisayarda herhangi bir sonuç döndürülür.

> [!NOTE]
> Kullanmadan önce **Invoke-Command** cmdlet'inin Windows PowerShell yürütmek için uzak bir bilgisayarda komutlar, WinRM dinleyicisi uzak iletileri kabul edecek şekilde yapılandırmanız gerekir. Komutunu çalıştırarak bunu yapabilirsiniz **winrm quickconfig** uzak bilgisayarda. Daha fazla bilgi için [yükleme ve yapılandırma için Windows Uzaktan Yönetimi](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Bir Windows PowerShell penceresinden çalıştırmak için şu sözdizimini kullanırsınız **LogDeploy.ps1** uzak bir bilgisayarda komut dosyası:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Kullanmanın çeşitli yolları vardır **Invoke-Command** bir betik dosyası, ancak bu yaklaşım çalıştırmaktır en dolaysız parametre değerlerini sağlayın ve boşluk içeren yolları yönetmek gerektiğinde.


Bu bir komut isteminde çalıştırdığınızda, yürütülebilir bir Windows PowerShell çağırmak ve kullanmak gereken **– komut** parametresi, yönergeler sağlamak için:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Daha önce bazı ek anahtarlar sağlamak ve MSBuild'den komutu çalıştırdığınızda herhangi ayrılmış XML karakterleri kaçış gereksinim duyduğunuz:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Son olarak, önceki örneklerde olduğu gibi kullanabileceğiniz **Exec** komutunuzu yürütmek için özel bir MSBuild hedefi görevi:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Yapı işleminizin bir parçası olarak bu hedef yürüttüğünüzde, Windows PowerShell komut, belirtilen bilgisayarda çalışmayacak **– computername** bağımsız değişken.

## <a name="conclusion"></a>Sonuç

Bu konuda bir MSBuild proje dosyasını bir Windows PowerShell betiğini çalıştırmak nasıl kaydedileceği açıklanır. Bu yaklaşım, yerel veya uzak bir bilgisayarda bir otomatik olarak veya tek adımlı derleme ve dağıtım sürecinin bir parçası olarak bir Windows PowerShell betiğini çalıştırmak için kullanabilirsiniz.

## <a name="further-reading"></a>Daha Fazla Bilgi

Windows PowerShell komut dosyalarını imzalama ve yürütme ilkeleri yönetme ile ilgili yönergeler için bkz: [çalışan Windows PowerShell komut](https://technet.microsoft.com/library/ee176949.aspx). Windows PowerShell komutları çalıştıran bir uzak bilgisayardan ile ilgili yönergeler için bkz. [çalıştıran uzak komutları](https://technet.microsoft.com/library/dd819505.aspx).

Dağıtım işlemini denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz. [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Önceki](taking-web-applications-offline-with-web-deploy.md)
> [İleri](troubleshooting-the-packaging-process.md)
