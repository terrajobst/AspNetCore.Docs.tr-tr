---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Veritabanı projeleri dağıtma | Microsoft Docs
author: jrjlee
description: 'Not: kurumsal dağıtım senaryoları çok sayıda içinde dağıtılmış bir veritabanı için artımlı güncelleştirmeler yayımlamak özelliği gerekir. Yeniden oluşturmak için kullanılan alternatiftir...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: a0b3871ea098b549271bce2b9d5f0c24f9ca8a9c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882507"
---
<a name="deploying-database-projects"></a>Veritabanı projeleri dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Kurumsal dağıtım senaryoları çok sayıda içinde dağıtılmış bir veritabanı için artımlı güncelleştirmeler yayımlamak özelliği gerekir. Var olan veritabanındaki tüm verileri kaybedersiniz anlamına gelir her dağıtım veritabanını yeniden oluşturmak için kullanılan alternatiftir. Visual Studio 2010 ile çalışırken, VSDBCMD artımlı veritabanı yayımlama için önerilen yaklaşım kullanmaktır. Ancak, Visual Studio Web yayımlama ardışık düzen (WPP) ve sonraki sürüm artımlı doğrudan yayımlama destekleyen araçları dahil edilir.


Visual Studio 2010'da Contact Manager örnek çözümü açarsanız, veritabanı projesi dört dosyaları içeren bir özellikler klasörünü içeren görürsünüz.

![](deploying-database-projects/_static/image1.png)

Proje dosyası ile birlikte (*ContactManager.Database.dbproj* bu durumda), bu dosyaları derleme ve dağıtım işleminin çeşitli yönlerini denetleme:

- *Database.sqlcmdvars* dosyası, projenin dağıtırken kullandığınız herhangi bir SQLCMD değişkeni değerleri sağlar. Her çözüm yapılandırması (örneğin, hata ayıklama ve yayın) farklı .sqlcmdvars dosyasını belirtebilirsiniz.
- *Database.sqldeployment* dosyayı projenize veya hedef sunucu harmanlama tanımlı harmanlama kullanıp kullanmayacağınızı gibi dağıtım özgü ayarları sağlar mı hedef veritabanı yeniden oluşturmak her saat veya yalnızca güncel duruma getirmek ve benzeri için varolan bir veritabanını düzeltmek. Her çözüm yapılandırması farklı .sqldeployment dosyasını belirtebilirsiniz.
- *Database.sqlpermissions* hedef veritabanına eklemek istediğiniz tüm izinleri tanımlamak için kullanabileceğiniz bir XML belgesi bir dosyadır. Tüm çözüm yapılandırmaları aynı .sqlpermissions dosya paylaşımı.
- *Database.sqlsettings* dosyasını, Karşılaştırma işleçleri davranışını kullanın ve benzeri için harmanlama gibi veritabanı oluşturulurken kullanılacak veritabanı düzeyi özellikleri belirtir. Tüm çözüm yapılandırmaları aynı .sqlsettings dosya paylaşımı.

Bu, bu dosyalar Visual Studio'da açın ve içeriği ile tanımak için bir dakikanızı değecek olur.

Veritabanı projesi derlerken, derleme işlemi iki dosyaları oluşturur:

- A *veritabanı şeması* (.dbschema dosyası). Bu, şemanın XML biçiminde oluşturmak istediğiniz veritabanının açıklar.
- A *dağıtım bildirimi* (.deploymanifest dosyası). Bu oluşturmak ve veritabanınızı dağıtmak için gerekli tüm bilgileri içerir. Dağıtım yönergeleri (.sqldeployment dosyası) ve tüm dağıtım öncesi veya dağıtım sonrası SQL komut dosyaları gibi diğer kaynaklar birlikte .dbschema dosya başvurur.

Bu, bu kaynakları arasındaki ilişkiyi gösterir:

![](deploying-database-projects/_static/image2.png)

Gördüğünüz gibi .sqlsettings dosyasını ve .sqlpermissions dosyasını oluşturma işlemi için girdileridir. Veritabanı Proje dosyası ile birlikte, bu dosyalar, veritabanı şema dosyası oluşturmak için kullanılır. .Sqldeployment dosyasını ve .sqlcmdvars dosyasını değişmeden derleme sürecinde geçirin. Dağıtım bildirimi veritabanı şeması, .sqldeployment dosya, .sqlcmdvars dosya ve tüm dağıtım öncesi veya dağıtım sonrası SQL komut dosyası konumunu belirtir.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Veritabanı projesi dağıtmak için neden VSDBCMD kullanacak?

Veritabanı projeleri dağıtmak için çeşitli farklı yaklaşım vardır. Ancak, bunların hepsi bir kuruluş ortamında uzak sunuculara veritabanı projesi dağıtmak için uygun değildir. Bir veritabanı proje dağıtımından istediğinize karar verin. Kurumsal dağıtım senaryolarında büyük bir olasılıkla istiyor:

- Uzak bir konumdan veritabanı projesi dağıtma yeteneği.
- Varolan bir veritabanına artımlı güncelleştirmeler yapma yeteneği.
- Dağıtım öncesi komut dosyaları veya dağıtım sonrası komut dosyaları dahil etme yeteneği.
- Birden çok hedef ortamı dağıtımına uyarlama yeteneği.
- Büyük, parçası olarak veritabanı projesi dağıtma yeteneği genellikle komut dosyası, tek adımlı Çözüm dağıtımı.

Veritabanı projesi dağıtmak için kullanabileceğiniz üç ana yaklaşım vardır:

- Visual Studio 2010 veritabanı proje türü ile dağıtım işlevini kullanabilirsiniz. Derleme ve Visual Studio 2010 veritabanı projesi dağıtma, dağıtım işlemi dağıtım bildirimi derleme yapılandırmasını belirli bir SQL tabanlı bir dağıtım dosyası oluşturmak için kullanır. Zaten mevcut değil veya zaten mevcut değilse veritabanına gerekli değişiklikleri yapın, bu veritabanı oluşturur. Bu dosya, hedef sunucuda çalıştırmak için SQLCMD.exe kullanabilirsiniz veya oluşturmak ve dosyayı çalıştırmak için Visual Studio ayarlayabilirsiniz. Bu yaklaşımın bir dezavantajı, dağıtım ayarlarını denetime yalnızca sınırlı sahip olur. Genellikle de ortama özgü değişken değerlerini sağlamak için SQL dağıtım dosyasını değiştirmeniz gerekebilir. Visual Studio 2010 yüklü bir bilgisayardan bu yaklaşım yalnızca kullanabilirsiniz ve geliştirici bilmeniz ve tüm hedef ortamlar için bağlantı dizelerini ve kimlik bilgilerini sağlayın gerekir.
- (Web dağıtımı) Internet Information Services (IIS) Web dağıtım aracı için kullanabileceğiniz [bir veritabanı, web uygulaması projesinin bir parçası dağıtma](https://msdn.microsoft.com/library/dd465343.aspx). Ancak, bu yaklaşım, veritabanı projesi dağıtmak yerine yalnızca bir hedef sunucuda var olan bir yerel veritabanı çoğaltmak istiyorsanız çok daha karmaşıktır. Web dağıtımı veritabanı projesi oluşturur SQL dağıtım komut dosyasını çalıştırmak için ancak bunu yapmak için yapılandırabilirsiniz, web uygulaması projeniz için özel bir WPP hedefleri dosyası oluşturmanız gerekir. Bu dağıtım işlemiyle karmaşıklığını önemli miktarda ekler. Ayrıca, Web dağıtımı doğrudan varolan veritabanları artımlı güncelleştirmeleri desteklemiyor. Bu yaklaşımı hakkında daha fazla bilgi için bkz: [dağıtılan SQL dosya Web yayımlama ardışık paket veritabanı projesi genişletme](https://go.microsoft.com/?linkid=9805121).
- Veritabanı, veritabanı şeması ya da dağıtım bildirimi kullanarak dağıtmak için VSDBCMD yardımcı programını kullanabilirsiniz. Daha büyük ve komut dosyalı dağıtım işleminin bir parçası veritabanlarını yayımlama olanak sağlayan bir MSBuild hedefin VSDBCMD.exe çağırabilirsiniz. .Sqlcmdvars dosya ve diğer veritabanı özelliklerini komutundan dağıtımınız farklı ortamlar için birden çok derleme yapılandırmaları oluşturmadan özelleştirmenize olanak sağlayan bir VSDBCMD, çok sayıda değişkenler geçersiz kılabilirsiniz. VSDBCMD yalnızca veritabanı şemanızı hedef veritabanıyla hizalamak için gerekli değişiklikleri yapar anlamına gelir ayrım işlevselliği sağlar. VSDBCMD çok çeşitli dağıtım işlemi üzerinde hassas denetime komut satırı seçenekleri de sunar.

Bu genel bakış'tan, VSDBCMD MSBuild ile kullanarak en iyi tipik kurumsal dağıtım senaryosu için uygun bir yaklaşım olduğunu görebilirsiniz:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Uzaktan dağıtımı destekler? | Evet | Evet | Evet |
| Artımlı güncelleştirmeler destekler? | Evet | Hayır | Evet |
| Öncesi/sonrası-deployment komut dosyalarını destekler? | Evet | Evet | Evet |
| Çoklu ortam dağıtımını destekler? | Sınırlı | Sınırlı | Evet |
| Komut dosyalı dağıtım destekler? | Sınırlı | Evet | Evet |

Bu konunun geri kalanında VSDBCMD veritabanı projeleri dağıtmak için MSBuild ile kullanımını açıklar.

## <a name="understanding-the-deployment-process"></a>Dağıtım sürecini anlama

VSDBCMD yardımcı programı veritabanı şeması (.dbschema dosyası) veya dağıtım bildirimi (.deploymanifest dosyası) kullanarak bir veritabanı dağıtmanıza olanak tanır. Uygulamada, dağıtım bildirimi çeşitli dağıtım özellikleri için varsayılan değerler sağlayın ve çalıştırmak istediğiniz tüm dağıtım öncesi veya dağıtım sonrası SQL betikleri belirlemenize olanak tanır gibi dağıtım bildirimi neredeyse her zaman kullanacaksınız. Örneğin, bu VSDBCMD komut dağıtmak için kullanılan **ContactManager** bir sınama ortamında bir veritabanı sunucusu veritabanına:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


Bu durumda:

- **/A** (veya **/eylem**) anahtarı belirtir yapmak için VSDBCMD istiyor. Bu ayar **alma** veya **dağıtma**. **Alma** seçeneği, var olan bir veritabanından .dbschema dosyası oluşturmak için kullanılır ve **dağıtma** seçeneği .dbschema dosya hedef veritabanına dağıtmak için kullanılır.
- **/Bildirimi** (veya **/ManifestFile**) anahtar dağıtmak istediğiniz .deploymanifest dosya tanımlar. .Dbschema dosyası kullanmayı istediyseniz, kullanacağınız **/model** (veya **/ModelFile**) geçin.
- **/Cs** (veya **/ConnectionString**) anahtarı, hedef veritabanı sunucusu için bağlantı dizesini sağlar. Bu veritabanının adı içermeyen Not&#x2014;VSDBCMD; veritabanı oluşturmak için sunucuya bağlanmanız gerekiyor tek bir veritabanına bağlanmak gerekmez. Bir bağlantı dizesi .deploymanifest dosyanızı içeriyorsa, bu anahtarı atlayabilirsiniz. Anahtarı yine de kullanırsanız, anahtar değer .deploymanifest değerini geçersiz kılar.
- <strong>/P:TargetDatabase</strong> özelliği, oluşturma sırasında hedef veritabanına atamak istediğiniz adı sağlar. Bu değerini geçersiz kılar <strong>TargetDatabase</strong> .deploymanifest dosyasında özellik. Kullanabileceğiniz <strong>p:</strong> <em>[özellik adı]</em>sözdizimi çok çeşitli dağıtım özelliklerini ayarlamak ve herhangi bir SQLCMD değişkeni geçersiz kılmak için .sqlcmdvars dosyasında bildirildi.
- **/Dd+** (veya **/DeployToDatabase+**) anahtar gösteren bir dağıtımını oluşturun ve hedef ortam dağıtmak istediğiniz. Belirtirseniz **/dd-**, veya anahtarını atlarsanız, VSDBCMD bir dağıtım komut dosyası oluşturur, ancak hedef ortam dağıtmaz. Bu anahtar, Karışıklığı önlemek için kaynak görülür ve sonraki bölümde daha ayrıntılı açıklanmıştır.
- **/Script** (veya **/DeploymentScriptFile**) anahtar dağıtım betiği oluşturmak istediğiniz belirtir. Bu değer, dağıtım işlemi etkilemez.

VSDBCMD hakkında daha fazla bilgi için bkz: [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema Al)](https://msdn.microsoft.com/library/dd193283.aspx) ve [nasıl yapılır: bir veritabanı VSDBCMD kullanarak dağıtım için bir komut isteminden hazırlayın. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

VSDBCMD bir MSBuild proje dosyasından nasıl kullanabileceğinize bir örnek için bkz: [oluşturma işlemini anlama](understanding-the-build-process.md). Birden çok ortamlar için veritabanı dağıtım ayarlarını yapılandırmak nasıl örnekleri için bkz: [veritabanı dağıtımlarını özelleştirme birden çok ortamları](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>DeployToDatabase anahtar anlama

Davranışını **/dd** veya **/DeployToDatabase** anahtar bağlı olup olmadığını VSDBCMD bir .dbschema veya .deploymanifest dosyası ile kullanmakta olduğunuz üzerinde. Bir .dbschema dosyası kullanıyorsanız, davranışı oldukça basittir:

- Belirtirseniz **/dd+** veya **/dd**, VSDBCMD bir dağıtım komut dosyası oluşturmanız ve veritabanı dağıtın.
- Belirtirseniz **/dd-** veya anahtarını atlarsanız, VSDBCMD, yalnızca bir dağıtım komut dosyası üretir.

Bir .deploymanifest dosyası kullanıyorsanız, çok daha karmaşık bir davranıştır. .Deploymanifest dosyası bir özellik adı içerdiğinden budur **DeployToDatabase** de belirleyen veritabanı dağıtılır.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Bu özelliğin değeri, veritabanı projesi özellikleri göre ayarlanır. Ayarlarsanız **eylem dağıtmak** için **bir dağıtım komut dosyası (.sql) oluşturma**, değer olacaktır **False**. Ayarlarsanız **eylem dağıtmak** için **dağıtım komut dosyası (.sql) oluşturabilir ve veritabanına dağıtım**, değer olacaktır **doğru**.

> [!NOTE]
> Bu ayarlar, belirli yapı yapılandırması ve platformu ile ilişkilendirilir. Ayarlarını yapılandırırsanız, örneğin, **hata ayıklama** yapılandırma ve kullanma yayımlama **sürüm** yapılandırma, ayarlarınızı kullanılmayacak.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> Bu senaryoda, **eylem dağıtmak** her zaman ayarlanması gerektiğini **bir dağıtım komut dosyası (.sql) oluşturmak**, veritabanınızı dağıtmak için Visual Studio 2010 istemediğiniz. Diğer bir deyişle, **DeployToDatabase** özelliği uymanız gereken **False**.


Zaman bir **DeployToDatabase** özellik belirtilmemişse, **/dd** anahtar yalnızca kılar özelliği özellik değeri ise **false**:

- Varsa **DeployToDatabase** özelliği **False**, ve belirttiğiniz **/dd+** veya **/dd**, VSDBCMD geçersiz kılar  **DeployToDatabase** özelliği ve veritabanı dağıtın.
- Varsa **DeployToDatabase** özelliği **False**, ve belirttiğiniz **/dd-** veya anahtarını atlarsanız, VSDBCMD olmayan veritabanı dağıtın.
- Varsa **DeployToDatabase** özelliği **doğru**, VSDBCMD anahtar yoksay ve veritabanı dağıtın.
- Veritabanı da dağıtmakta bağımsız olarak her durumda, bir dağıtım komut dosyası oluşturulur.

## <a name="conclusion"></a>Sonuç

Bu konu, Visual Studio 2010 veritabanı projeleri için derleme ve dağıtım işlemine genel bakış sağlanan. Ayrıca, VSDBCMD.exe MSBuild ile Kurumsal ölçekte veritabanı dağıtımını desteklemek için nasıl kullanabileceğinizi açıklanmaktadır.

Bu uygulamada nasıl çalıştığı hakkında daha fazla bilgi için bkz: [veritabanı dağıtımlarını özelleştirme birden çok ortamları](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Daha Fazla Bilgi

Her ortam için ayrı bir dağıtım yapılandırma dosyası oluşturarak veritabanı dağıtımlarını özelleştirme hakkında daha fazla bilgi için bkz: [veritabanı dağıtımlarını özelleştirme birden çok ortamları](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Bir dağıtım sonrası komut dosyası çalıştırarak veritabanı rol üyeliklerini yapılandırma hakkında yönergeler için bkz [Test ortamları için veritabanı rol üyeliklerini dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Benzersiz zorluklarından bazıları, üyelik yönetme konusunda yönergeler için veritabanları zorunlu tuttukları'ı, bkz: [üyelik veritabanına dağıtma kurumsal ortamlarda](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Bu konular MSDN'de daha geniş yönerge ve Visual Studio veritabanı projelerini hakkında arka plan bilgileri ve veritabanı dağıtım işlemi sağlar:

- [Visual Studio 2010 SQL Server veritabanı projeleri](https://msdn.microsoft.com/library/ff678491.aspx)
- [Veritabanı değişikliği yönetme](https://msdn.microsoft.com/library/aa833404.aspx)
- [Nasıl yapılır: bir veritabanı VSDBCMD kullanarak dağıtım için bir komut isteminden hazırlayın. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Veritabanı derleme ve dağıtım genel bakış](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Önceki](deploying-web-packages.md)
> [sonraki](creating-and-running-a-deployment-command-file.md)
