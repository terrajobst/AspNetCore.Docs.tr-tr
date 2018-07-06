---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Veritabanı projeleri dağıtma | Microsoft Docs
author: jrjlee
description: 'Not: kurumsal dağıtım senaryolarında çok sayıda, dağıtılmış bir veritabanı için artımlı güncelleştirmeleri yayımlamak için gerçekleştirebilmeleri gerekmektedir. Alternatif yeniden oluşturmaktır...'
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 1e5af29a91f5f432f9241dc3ba0c8fc0bfcf773f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804989"
---
<a name="deploying-database-projects"></a>Veritabanı projeleri dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Kurumsal dağıtım senaryolarında çok sayıda, dağıtılmış bir veritabanı için artımlı güncelleştirmeleri yayımlamak için gerçekleştirebilmeleri gerekmektedir. Alternatif, tüm veriler var olan veritabanını kaybetmeniz anlamına gelir her dağıtım veritabanını yeniden oluşturmaktır. Visual Studio 2010 ile çalışırken, VSDBCMD artımlı veritabanı yayımlama için önerilen yaklaşım kullanmaktır. Ancak, Visual Studio ve Web yayımlama işlem hattı (WPP)'ın sonraki sürümü artımlı yayımlama doğrudan destekleyen araçlar içerir.


Visual Studio 2010'da örnek Kişi Yöneticisi çözümü açarsanız, veritabanı projesini dört dosyaları içeren bir özellikler klasör içerdiğini görürsünüz.

![](deploying-database-projects/_static/image1.png)

Proje dosyası ile birlikte (*ContactManager.Database.dbproj* bu durumda), bu dosyalar derleme ve dağıtım sürecini çeşitli yönlerini denetleme:

- *Database.sqlcmdvars* dosyası proje dağıtırken kullandığınız tüm SQLCMD değişkenler için değerler sağlar. Her çözüm yapılandırması (örneğin, hata ayıklama ve yayın) farklı .sqlcmdvars dosyası belirtebilirsiniz.
- *Database.sqldeployment* dosyası projenize ya da hedef sunucunun harmanlama tanımlanan harmanlama kullanıp kullanmayacağınızı gibi dağıtımına özgü ayarlar sağlar mı hedef veritabanı yeniden oluşturmak her zaman veya yalnızca güncel duruma getirmek ve benzeri için var olan veritabanını Değiştir. Her çözüm yapılandırması farklı .sqldeployment dosyası belirtebilirsiniz.
- *Database.sqlpermissions* hedef veritabanına eklemek istediğiniz tüm izinleri tanımlamak için kullanabileceğiniz bir XML belgesi bir dosyadır. Tüm çözüm yapılandırmaları aynı .sqlpermissions dosyayı paylaşın.
- *Database.sqlsettings* dosyası veritabanı harmanlama, Karşılaştırma işleçleri davranışını ve benzeri gibi oluştururken kullanmak için veritabanı düzeyinde özelliklerini belirtir. Tüm çözüm yapılandırmaları aynı .sqlsettings dosyayı paylaşın.

Bu, bu dosyalar Visual Studio içinde açın ve ile içeriği hakkında bilgilenmeli vakit ayırdığınız değer olur.

Veritabanı projesi derlerken, derleme işlemi iki dosya oluşturulur:

- A *veritabanı şemasını* (.dbschema dosyası). Bu XML biçiminde oluşturmak istediğiniz veritabanının şeması açıklar.
- A *dağıtım bildirimi* (.deploymanifest dosyası). Bu, oluşturmak ve veritabanınızı dağıtmak için gereken tüm bilgileri içerir. Bu, dağıtım yönergeleri (.sqldeployment dosyası) ve tüm dağıtım öncesi veya dağıtım sonrası SQL komut dosyaları gibi diğer kaynakları birlikte .dbschema dosyasına başvuruyor.

Bu, bu kaynakları arasındaki ilişkiyi gösterir:

![](deploying-database-projects/_static/image2.png)

Gördüğünüz gibi .sqlsettings dosyasını ve .sqlpermissions dosyasını oluşturma sürecinde girişlerdir. Veritabanı Proje dosyası ile birlikte, bu dosyalar, veritabanı şema dosyası oluşturmak için kullanılır. .Sqldeployment dosyasını ve .sqlcmdvars dosyasını değiştirmeden sürecinde geçirin. Dağıtım bildirimi veritabanı şeması, .sqldeployment dosya, .sqlcmdvars dosyayı ve tüm dağıtım öncesi veya dağıtım sonrası betiklerinin konumu gösterir.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Veritabanı projesi dağıtmak için neden VSDBCMD kullanmalıyım?

Veritabanı projeleri dağıtma için çeşitli farklı yaklaşım vardır. Ancak, bunları tüm uzak sunuculara kurumsal bir ortamda bir veritabanı projesi dağıtmak için uygun değildir. Bir veritabanı projesi dağıtımından istediğinizi düşünün. Kurumsal dağıtım senaryolarında büyük olasılıkla:

- Uzak bir konumdan veritabanı projesini dağıtma yeteneği.
- Mevcut bir veritabanına artımlı güncelleştirmeler yapma yeteneği.
- Dağıtım öncesi betiklerin veya dağıtım sonrası betikleri dahil etme yeteneği.
- Birden çok hedef ortama dağıtım uyarlama olanağı.
- Veritabanı projesi büyük, parçası olarak dağıtma yeteneği genellikle komut dosyası, tek adımlı Çözüm dağıtımı.

Veritabanı projesi dağıtmak için kullanabileceğiniz üç ana yaklaşım vardır:

- Visual Studio 2010 veritabanı proje türü ile dağıtım işlevlerini kullanabilirsiniz. Dağıtım işlemi dağıtım bildirimi oluşturmak ve Visual Studio 2010 veritabanı projesi dağıtma, derleme yapılandırmasını belirli bir SQL tabanlı bir dağıtım dosyası oluşturmak için kullanır. Zaten mevcut değil veya henüz yoksa veritabanı için gerekli değişiklikleri yapın. Bu veritabanı oluşturur. Bu dosya hedef üzerinde çalıştırılacak SQLCMD.exe kullanabilirsiniz veya oluşturup dosyayı çalıştırmak için Visual Studio ayarlayabilirsiniz. Bu yaklaşımın bir dezavantajı, dağıtım ayarlarını denetime yalnızca sınırlı sahip olur. Genellikle de ortama özgü değişken değerlerini sağlamak için SQL dağıtım dosyasını değiştirmeniz gerekebilir. Yüklü Visual Studio 2010 ile yalnızca bir bilgisayardan bu yaklaşımı kullanabilirsiniz ve geliştirici bilmeniz ve tüm hedef ortamlar için bağlantı dizelerini ve kimlik bilgilerini sağlamanız gerekir.
- Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) için kullanabileceğiniz [bir veritabanı, web uygulama projesinin bir parçası dağıtma](https://msdn.microsoft.com/library/dd465343.aspx). Ancak, bu yaklaşım, bir veritabanı projesi dağıtmak yerine yalnızca bir hedef sunucuda var olan bir yerel veritabanı çoğaltma istiyorsanız çok daha karmaşık olur. Veritabanı projesi oluşturur ve SQL dağıtım betiği çalıştırmak için ancak bunu yapmak için Web dağıtımı yapılandırabilirsiniz, web uygulaması projenize için özel bir WPP hedeflerini dosya oluşturmanız gerekir. Bu dağıtım işlemine karmaşıklığı önemli miktarda ekler. Ayrıca, Web dağıtımı doğrudan mevcut veritabanlarının artımlı güncelleştirmeleri desteklemez. Bu yaklaşımı hakkında daha fazla bilgi için bkz. [paket veritabanı projesi için Web yayımlama ardışık genişletme dağıtılan SQL dosyası](https://go.microsoft.com/?linkid=9805121).
- Veritabanı, veritabanı şemasını veya dağıtım bildirimi kullanarak dağıtmak için VSDBCMD yardımcı programını kullanabilirsiniz. Daha büyük ve komut dosyalı dağıtım işleminin bir parçası veritabanları yayımlamanıza olanak sağlayan bir MSBuild hedefi'den VSDBCMD.exe çağırabilirsiniz. Değişkenleri .sqlcmdvars dosya ve çok sayıda farklı ortamlar için dağıtım, birden çok oluşturma yapılandırmasında oluşturmadan özelleştirmenizi sağlar bir komuttan VSDBCMD diğer veritabanı özellikleri geçersiz kılabilirsiniz. VSDBCMD yalnızca bir hedef veritabanı, veritabanı şeması ile hizalamak için gerekli değişiklikleri yapar yani ayrıştırması işlevselliği sağlar. VSDBCMD ayrıca çok çeşitli dağıtım işlemi üzerinde ayrıntılı denetim verdiğiniz komut satırı seçenekleri sunar.

Bu genel bakış'tan, MSBuild ile VSDBCMD kullanarak bir tipik kurumsal dağıtım senaryosu için en uygun yaklaşımı olduğunu görebilirsiniz:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Uzaktan dağıtım destekler? | Evet | Evet | Evet |
| Artımlı güncelleştirmeler destekler? | Evet | Hayır | Evet |
| Öncesi/sonrası-deployment betikleri destekler? | Evet | Evet | Evet |
| Çok ortamlı dağıtımını destekler? | Sınırlı | Sınırlı | Evet |
| Komut dosyası dağıtım destekler? | Sınırlı | Evet | Evet |

Bu konunun geri kalanı VSDBCMD dağıtmak veritabanı projeleri için MSBuild ile kullanımını açıklar.

## <a name="understanding-the-deployment-process"></a>Dağıtım işlemini anlama

VSDBCMD yardımcı programı bir veritabanı veya veritabanı şeması (.dbschema dosyası), hem de dağıtım bildirimini (.deploymanifest dosyası) kullanarak dağıtmanıza olanak tanır. Uygulamada, dağıtım bildirimini çeşitli dağıtım özellikleri için varsayılan değerler sağlayın ve çalıştırmak istediğiniz tüm dağıtım öncesi veya dağıtım sonrası SQL betikleri belirlemenize olanak tanır şekilde dağıtım bildirimini neredeyse her zaman kullanacaksınız. Örneğin, bu VSDBCMD komut dağıtmak için kullanılan **ContactManager** veritabanına bir test ortamında bir veritabanı sunucusu:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


Bu durumda:

- **/A** (veya **/eylem**) anahtar VSDBCMD yapmak istediğinizi belirtir. Bu ayar **alma** veya **Dağıt**. **Alma** seçeneği, var olan bir veritabanından .dbschema dosyası oluşturmak için kullanılır ve **Dağıt** seçeneği, bir hedef veritabanına .dbschema dosya dağıtmak için kullanılır.
- **/MANIFEST** (veya **/ManifestFile**) anahtar dağıtmak istediğiniz .deploymanifest dosya tanımlar. .Dbschema dosyası kullanmayı istediyseniz, kullanacağınız **/model** (veya **/ModelFile**) geçin.
- **/Cs** (veya **/ConnectionString**) anahtar hedef veritabanı sunucusu için bağlantı dizesini sağlar. Bu veritabanının adı içermeyen Not&#x2014;VSDBCMD gerekiyor; veritabanı sunucusuna bağlanmak tek bir veritabanına bağlanma gerekmez. Bir bağlantı dizesi .deploymanifest dosyanızı içeriyorsa, bu anahtarı atlayabilirsiniz. Anahtarı yine de kullanırsanız, anahtar değeri .deploymanifest değerini geçersiz kılar.
- <strong>/P:TargetDatabase</strong> oluşturma hedef veritabanında atamak istediğiniz ad özelliği sağlar. Bu değeri geçersiz kılar <strong>TargetDatabase</strong> .deploymanifest dosyasındaki özellik. Kullanabileceğiniz <strong>/p:</strong> <em>[özellik adı]</em>sözdizimi çok çeşitli dağıtım özellikleri ayarlamak ve herhangi bir SQLCMD değişkeni geçersiz kılmak için bildirilen .sqlcmdvars dosyanızda.
- **/Dd+** (veya **/DeployToDatabase+**) anahtar bir dağıtımını oluşturun ve hedef ortama dağıtmak istediğinizi belirtir. Belirtirseniz **/dd-**, veya anahtarını atlarsanız, VSDBCMD dağıtım betiği oluşturur, ancak hedef ortama dağıtmaz. Bu anahtar, genellikle karışıklık kaynaktır ve sonraki bölümde daha ayrıntılı açıklanmıştır.
- **/SCRIPT** (veya **/DeploymentScriptFile**) anahtar dağıtım betiği oluşturmak istediğiniz belirtir. Bu değer, dağıtım işlemi etkilemez.

VSDBCMD hakkında daha fazla bilgi için bkz. [VSDBCMD için komut satırı başvurusu. EXE (dağıtım ve şema içeri aktarma)](https://msdn.microsoft.com/library/dd193283.aspx) ve [nasıl yapılır: bir veritabanı VSDBCMD kullanarak dağıtım için bir komut istemi'nden hazırlayın. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

VSDBCMD bir MSBuild proje dosyasından nasıl kullanabileceğinize bir örnek için bkz [derleme işlemini anlama](understanding-the-build-process.md). Birden çok ortam için veritabanı dağıtım ayarlarını yapılandırma örnekleri için bkz: [birden çok ortam için veritabanı dağıtımlarını özelleştirme](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>DeployToDatabase anahtar anlama

Davranışını **/dd** veya **/DeployToDatabase** anahtar olup VSDBCMD bir .dbschema veya .deploymanifest dosyası ile kullanmakta olduğunuz üzerinde bağlıdır. Davranış .dbschema dosyası kullanıyorsanız, oldukça basittir:

- Belirtirseniz **/dd+** veya **/dd**, VSDBCMD bir dağıtım betiği oluşturun ve veritabanı dağıtma.
- Belirtirseniz **/dd-** veya anahtarını atlarsanız, VSDBCMD yalnızca bir dağıtım komut dosyası üretir.

.Deploymanifest dosyası kullanıyorsanız, çok daha karmaşık bir davranıştır. Bir özellik adı .deploymanifest dosyayı içeren olmasıdır **DeployToDatabase** ayrıca belirleyen veritabanı dağıtılır.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Bu özelliğin değeri, veritabanı projesini özelliklerine göre ayarlanır. Ayarlarsanız **eylem dağıtma** için **dağıtım betiği (.sql) oluşturma**, değer olacaktır **False**. Ayarlarsanız **eylem dağıtma** için **dağıtım betiği (.sql) oluşturma ve dağıtma veritabanına**, değer olacaktır **True**.

> [!NOTE]
> Bu ayarlar, belirli bir yapı yapılandırması ve platformu ile ilişkilidir. Örneğin ayarlarını yapılandırırsanız, **hata ayıklama** yapılandırma ve ardından kullanarak yayımlama **yayın** yapılandırması ayarlarınızı kullanılmayacak.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> Bu senaryoda, **eylem dağıtma** her zaman ayarlanmalıdır **dağıtım betiği (.sql) oluşturma**, veritabanınızı dağıtmak için Visual Studio 2010 istemediğinden. Diğer bir deyişle, **DeployToDatabase** özelliği her zaman olmalıdır **False**.


Olduğunda bir **DeployToDatabase** özelliği belirtildi, **/dd** özelliği değer ise anahtar özelliğinin yalnızca geçersiz **false**:

- Varsa **DeployToDatabase** özelliği **False**, ve belirttiğiniz **/dd+** veya **/dd**, VSDBCMD kılar  **DeployToDatabase** özelliği ve veritabanı dağıtma.
- Varsa **DeployToDatabase** özelliği **False**, ve belirttiğiniz **/dd-** veya anahtarını atlarsanız, VSDBCMD olmayan veritabanı dağıtma.
- Varsa **DeployToDatabase** özelliği **True**, VSDBCMD anahtar yoksay ve veritabanı dağıtma.
- Dağıtım betiğini de veritabanı dağıtımı yapıyorsanız bağımsız olarak her durumda oluşturulur.

## <a name="conclusion"></a>Sonuç

Bu konuda, Visual Studio 2010 veritabanı projeleri için derleme ve dağıtım sürecine genel bakış sağlanır. VSDBCMD.exe MSBuild ile Kurumsal ölçekte veritabanı dağıtımı desteklemek üzere nasıl kullanabileceğinizi de açıklanmaktadır.

Bunun uygulamada nasıl çalıştığı hakkında daha fazla bilgi için bkz: [birden çok ortam için veritabanı dağıtımlarını özelleştirme](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Daha Fazla Bilgi

Her ortam için ayrı bir dağıtım yapılandırma dosyası oluşturarak veritabanı dağıtımlarını özelleştirme hakkında daha fazla bilgi için bkz: [birden çok ortam için veritabanı dağıtımlarını özelleştirme](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Veritabanı rol üyelikleri, bir dağıtım sonrası betiği çalıştırarak yapılandırma hakkında yönergeler için bkz. [Test ortamlarına veritabanı rol üyelikleri dağıtma](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Üyeliğin benzersiz sorunlarından bazıları yönetme hakkında rehberlik için veritabanları dayatır, bkz: [Kurumsal ortamlara üyelik veritabanları dağıtma](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Bu MSDN'de daha geniş bir rehberlik ve Visual Studio veritabanı projelerini hakkında arka plan bilgileri ve veritabanı dağıtım işlemi sağlar:

- [Visual Studio 2010 SQL Server veritabanı projeleri](https://msdn.microsoft.com/library/ff678491.aspx)
- [Veritabanı değişikliği yönetme](https://msdn.microsoft.com/library/aa833404.aspx)
- [Nasıl yapılır: bir veritabanı VSDBCMD kullanarak dağıtım için bir komut istemi'nden hazırlayın. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Veritabanı derleme ve dağıtım genel bakış](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Önceki](deploying-web-packages.md)
> [İleri](creating-and-running-a-deployment-command-file.md)
