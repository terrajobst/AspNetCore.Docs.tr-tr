---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: MSBuild proje dosyalarından Windows PowerShell komut dosyaları çalıştırmak | Microsoft Docs
author: jrjlee
description: Bu konu, derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar. Bir komut dosyası yerel olarak çalıştırabilirsiniz (diğer bir deyişle, B'deki...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890365"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>MSBuild proje dosyalarından Windows PowerShell betikleri çalıştırma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, derleme ve dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak açıklar. (Diğer bir deyişle, yapı sunucuya) yerel olarak bir komut dosyası çalıştırma veya uzaktan bir hedef web sunucusunda veya veritabanı sunucusunda ister.
> 
> Pek çok neden bir dağıtım sonrası Windows PowerShell betiğini çalıştırmak isteyebilirsiniz nedenleri vardır. Örneğin, aşağıdakileri yapabilirsiniz:
> 
> - Özel olay kaynağı kayıt defterine ekleyin.
> - Yüklemeleri için bir dosya sistemi dizin oluşturur.
> - Yapı dizinler temizleyin.
> - Özel bir günlük dosyası girişlerini yazma.
> - Kullanıcılar yeni sağlanan web uygulaması için davet e-postalar gönderin.
> - Uygun izinlere sahip kullanıcı hesapları oluşturun.
> - SQL Server örnekleri arasında çoğaltmayı yapılandırın.
> 
> Bu konuda Microsoft Build Engine (MSBuild) proje dosyasında özel bir hedeften hem yerel hem de uzaktan Windows PowerShell betikleri çalıştırmak nasıl yapacağınızı gösterir.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bir otomatik olarak veya tek adımlı dağıtım işleminin bir parçası bir Windows PowerShell betiğini çalıştırmak için bu üst düzey görevleri tamamlamanız gerekir:

- Çözümünüzü ve kaynak denetimi için Windows PowerShell komut dosyası ekleyin.
- Windows PowerShell komut dosyasını çağıran bir komut oluşturun.
- Tüm ayrılmış XML karakterlerini Komutunuzda kaçış.
- Bir hedef, özel MSBuild proje dosyası oluşturma ve kullanma **Exec** , komutu çalıştırmak için görev.

Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir. Görevleri ve bu konudaki yönergeler, zaten MSBuild hedefleri ve özellikleri ile tanıdık olduğunuz ve bir özel MSBuild proje dosyası oluşturma ve dağıtma işlemi sürücü için nasıl kullanılacağını anlamak varsayalım. Daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Oluşturma ve Windows PowerShell komut dosyaları ekleme

Bu konu başlığı altındaki görevleri adlandırılmış bir örnek Windows PowerShell Betiği kullanmak **LogDeploy.ps1** MSBuild komut dosyalarını çalıştırmak nasıl göstermek üzere. **LogDeploy.ps1** betik, bir tek satır girişi bir günlük dosyasına yazar basit bir işlev içerir:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


**LogDeploy.ps1** komut dosyası iki parametre kabul eder. İlk parametre tam yolu bir giriş eklemek istediğiniz günlük dosyasına ve ikinci parametre günlük dosyasını kaydetmek istediğiniz dağıtım hedef temsil eder. Komut dosyasını çalıştırdığınızda, günlük dosyası bu biçimde bir satır ekler:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Yapmak için **LogDeploy.ps1** MSBuild için kullanılabilir komut dosyası, yapmanız gerekir:

- Komut dosyası için kaynak denetimi ekleyin.
- Çözümünüzü Visual Studio 2010 için komut dosyası ekleyin.

Komut dosyası derleme sunucusundaki veya uzak bir bilgisayarda komut dosyasını çalıştırmayı planladığınız bağımsız olarak çözüm içeriğinizi ile dağıtmanız gerekmez. Komut dosyası bir çözüm klasörüne eklemek bir seçenektir. Dağıtım işleminin bir parçası Windows PowerShell Betiği kullanmak istediğiniz olduğundan Contact Manager örnekte Yayımla çözüm klasöre komut dosyası eklemek için mantıklıdır.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Çözüm klasörlerinin içeriğini sunucular kaynak malzeme olarak oluşturmak için kopyalanır. Ancak, herhangi bir proje çıktı hiçbir bölümü oluşturur.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Yapı sunucuda bir Windows PowerShell betiğini yürütme

Bazı senaryolarda projelerinizi derlemeler bilgisayarda Windows PowerShell betikleri çalıştırmak isteyebilirsiniz. Örneğin, yapı klasörleri temizlemek veya özel bir günlük dosyası girişlerini yazmak için bir Windows PowerShell betiğini kullanabilirsiniz.

Sözdizimi açısından bir MSBuild proje dosyasından bir Windows PowerShell Betiği çalıştıran bir Windows PowerShell Betiği normal bir komut isteminden çalıştırma ile aynı değil. Yürütülebilir powershell.exe çağırma ve kullanması gereken **– komutu** Windows PowerShell'i çalıştırmak istediğiniz komutları sağlamak için anahtar. (Windows PowerShell v2'de kullanabilirsiniz **– dosya** geçiş). Komut şöyle izlemesi gerekir:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Örneğin:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Komut dosyanızın yolu boşluk içeriyorsa, dosya yolunu ve işareti tarafından öncesinde tek tırnak içine alın gerekir. Komut kapsamak için zaten kullandıysanız, çift tırnak işareti kullanamazsınız:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


MSBuild Bu komuttan çağırdığınızda birkaç ek noktalar vardır. İlk olarak, içermelidir **– NonInteractive** bayrağı betik sessizce çalıştığından emin olun. Ardından, içermelidir **– ExecutionPolicy** bayrağı ile ilgili bağımsız değişken değeri. Bu Windows PowerShell komut dosyanıza uygulanır ve, betik yürütme işlemi engelleyebilir varsayılan yürütme ilkesi geçersiz kılmanıza olanak tanır yürütme ilkesi belirtir. Bu bağımsız değişken değerden birini seçebilirsiniz:

- Değerini **Kısıtlanmamış** Windows PowerShell komut dosyası imzalı olup bağımsız olarak, komut dosyası yürütme izin verir.
- Değerini **RemoteSigned** yerel makine üzerinde oluşturulan imzalanmamış komut dosyalarını çalıştırmak Windows PowerShell izin verir. Ancak, başka bir yerde oluşturulan komut dosyalarını oturum açmanız gerekir. (Pratikte, bir Windows PowerShell komut dosyası yerel olarak bir yapı sunucuda oluşturduğunuz çok düşüktür).
- Değerini **AllSigned** yalnızca imzalı komut dosyalarını çalıştırmak Windows PowerShell izin verir.

Varsayılan yürütme İlkesi **kısıtlı**, önleyen Windows PowerShell komut dosyalarını çalıştırma.

Son olarak, Windows PowerShell komutunda oluşan ayrılmış XML karakterler atlamanız gerekir:

- Tek tırnak işaretleriyle değiştirme  **&amp;apos;**
- Çift tırnak işaretleriyle değiştirme  **&amp;quot;**
- Ve işaretlerini ile Değiştir  **&amp;amp;**

- Bu değişiklikler yaptığınızda, komutunuzu bu benzeyecektir:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


Özel MSBuild proje dosyası içinde yeni bir hedef oluşturma ve kullanma **Exec** görev bu komutu çalıştırın:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


Bu örnekte, dikkat edin:

- Parametre değerleri ve Windows PowerShell yürütülebilir dosyasının konumuna gibi herhangi bir değişkeni MSBuild özellikleri olarak bildirilir.
- Koşullar kullanıcıların komut satırından bu değerleri geçersiz kılmak dahil edilir.
- **MSDeployComputerName** özelliği başka bir yerde proje dosyasında bildirilen.

Bu hedef yapı işleminizin bir parçası olarak çalıştırdığınızda, Windows PowerShell komutu çalıştırın ve bir günlük girişi, belirtilen dosyaya yazma.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Bir uzak bilgisayarda bir Windows PowerShell betiğini yürütme

Windows PowerShell komut dosyaları uzak bilgisayarlarda çalıştırabilen [Windows Uzaktan Yönetim](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Bunu yapmak için kullanmanız gerekir [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet'i. Bu, uzak bilgisayarlara betik kopyalama olmadan bir veya daha fazla uzak bilgisayarlarda komut yürütme sağlar. Komut dosyasını çalıştıran yerel bilgisayara herhangi bir sonuç döndürülür.

> [!NOTE]
> Kullanmadan önce **Invoke-Command** Windows PowerShell yürütmek için cmdlet'i uzak bir bilgisayarda komut dosyaları, uzak iletileri kabul etmek için bir WinRM dinleyicisi yapılandırmanız gerekir. Bu komutu çalıştırarak yapmak **winrm quickconfig** uzak bilgisayarda. Daha fazla bilgi için bkz: [yükleme ve yapılandırma için Windows Uzaktan Yönetim](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Bir Windows PowerShell penceresinden çalıştırmak için şu sözdizimini kullanırsınız **LogDeploy.ps1** uzak bir bilgisayarda komut dosyası:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Kullanmanın çeşitli yolları vardır **Invoke-Command** bir komut dosyası, ancak bu yaklaşım çalıştırmaktır en kolay parametre değerlerini sağlayın ve boşluk içeren yollar yönetmek gerektiğinde.


Bu bir komut isteminden çalıştırdığınızda, Windows PowerShell yürütülebilir çağırma ve kullanmak gereken **– komutu** , yönergeler sağlamak için parametre:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Daha önce MSBuild komutu çalıştırdığınızda, tüm ayrılmış XML karakterlerinden Çık ve bazı ek anahtarlar sağlamak gerek duyduğunuz:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Son olarak, önceki gibi kullanabileceğiniz **Exec** görev, komut yürütmek için özel bir MSBuild hedef içinde:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Bu hedef yapı işleminizin bir parçası olarak çalıştırdığınızda, Windows PowerShell komut, belirtilen bilgisayarda çalışacak **– computername** bağımsız değişkeni.

## <a name="conclusion"></a>Sonuç

Bu konuda bir MSBuild proje dosyasından bir Windows PowerShell betiğini çalıştırmak açıklanmıştır. Bir otomatik olarak veya tek adımlı derleme ve dağıtım işleminin parçası olarak yerel veya uzak bir bilgisayarda bir Windows PowerShell betiğini çalıştırmak için bu yaklaşımı kullanın.

## <a name="further-reading"></a>Daha Fazla Bilgi

Windows PowerShell komut dosyalarını imzalama ve yürütme ilkelerini yönetme konusunda yönergeler için bkz: [çalışan Windows PowerShell komut](https://technet.microsoft.com/library/ee176949.aspx). Windows PowerShell komutları çalıştıran uzak bir bilgisayardan ile ilgili yönergeler için bkz: [çalıştıran uzak komutları](https://technet.microsoft.com/library/dd819505.aspx).

Dağıtım işlemi denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Önceki](taking-web-applications-offline-with-web-deploy.md)
> [sonraki](troubleshooting-the-packaging-process.md)
