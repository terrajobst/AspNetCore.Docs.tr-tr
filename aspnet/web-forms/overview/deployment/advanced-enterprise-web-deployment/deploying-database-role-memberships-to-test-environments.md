---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Test ortamlarına veritabanı rol üyelikleri dağıtma | Microsoft Docs
author: jrjlee
description: Bu konu, bir test ortamı için bir çözüm dağıtımının parçası olarak veritabanı rollerine kullanıcı hesaplarını eklemek açıklar. İçeren bir çözümü dağıttığınızda...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a690d99df7a19c422fb217544ec183c311d1796f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828039"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Test ortamlarına veritabanı rol üyelikleri dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, bir test ortamı için bir çözüm dağıtımının parçası olarak veritabanı rollerine kullanıcı hesaplarını eklemek açıklar.
> 
> Bir hazırlık veya üretim ortamı için bir veritabanı projesi içeren bir çözümü dağıttığınızda, genellikle veritabanı rollerine kullanıcı hesaplarının eklenmesi otomatik hale getirmek için geliştirici istemezsiniz. Çoğu durumda, geliştirici bilmediği hangi kullanıcı hesaplarının hangi veritabanı rollerine eklenmesi gerekir ve bu gereksinimleri herhangi bir zamanda değişebilir. Bir geliştirme için bir veritabanı projesi içeren bir çözümü dağıtmak veya test ortamı, ancak genellikle çok farklı durumdur:
> 
> - Geliştirici genellikle çözüm düzenli olarak, günde genellikle birkaç kez yeniden dağıtır.
> - Veritabanı genellikle veritabanı kullanıcıları oluşturulmalı ve rollere her dağıtımdan sonra eklenen, yani her dağıtımda yeniden oluşturulur.
> - Geliştirici, genellikle hedef geliştirme veya test ortamı üzerinde tam denetime sahiptir.
> 
> Bu senaryoda, otomatik olarak veritabanı kullanıcıları oluşturma ve dağıtım işleminin bir parçası veritabanı rolü üyeliği atamak yararlıdır.
> 
> Bu işlem koşullu hedef ortama bağlı gerektiğini önemli etmendir. Bir hazırlama veya üretim ortamı dağıtıyorsanız, işlem atlamak istiyorsunuz. Daha fazla araya rol üyeliklerini dağıtmak istediğiniz bir geliştirici dağıtıyorsanız veya test ortamı. Bu konuda, bu sorunu çözmek için kullanabileceğiniz bir yaklaşım açıklanmaktadır.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bu konu başlığı altında olduğunu varsayar:

- Çözüm dağıtımı, bölünmüş proje dosyası yaklaşımı açıklandığı gibi kullandığınız [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Veritabanı projenizi dağıtmak için proje dosyasından VSDBCMD açıklandığı çağrı [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Veritabanı kullanıcıları oluşturma ve bir test ortamı için bir veritabanı projesi dağıttığınızda, rol üyeliklerini atamak için yapmanız:

- Gerekli veritabanı değişiklikleri yapan bir Transact yapılandırılmış sorgu dili (Transact-SQL) komut dosyası oluşturun.
- Sqlcmd.exe yardımcı programı SQL betiğini çalıştırmak için kullandığı Microsoft Build Engine (MSBuild) hedef oluşturun.
- Bir test ortamına çözümünüzü dağıtırken hedef çağırmak için proje dosyalarınızı yapılandırın.

Bu konuda, bu yordamların her biri gerçekleştirme gösterilmektedir.

## <a name="scripting-the-database-role-memberships"></a>Veritabanı rol üyelikleri betik oluşturma

Seçtiğiniz herhangi bir konumda ve çok sayıda farklı şekilde bir Transact-SQL betiği oluşturabilirsiniz. En kolay yaklaşım, Visual Studio 2010'da, çözüm içinde komut dosyası oluşturmaktır.

**SQL betiği oluşturmak için**

1. İçinde **Çözüm Gezgini** penceresinde, veritabanı projesi düğümünü açıyorsa ulaşılamaz.
2. Sağ **betikleri** klasörünü **Ekle**ve ardından **yeni klasör**.
3. Tür **Test** klasör adı ve Enter tuşuna basın.
4. Sağ **Test** klasörünü **Ekle**ve ardından **betik**.
5. İçinde **Yeni Öğe Ekle** iletişim kutusunda, betiğinizi anlamlı bir ad verin (örneğin, **AddRoleMemberships.sql**) ve ardından **Ekle**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. İçinde *AddRoleMemberships.sql* Transact-SQL deyimlerini ekleyin:

    1. Veritabanınızı erişecek SQL Server oturum açma için bir veritabanı kullanıcısı oluşturun.
    2. Veritabanı kullanıcısı için herhangi bir gerekli veritabanı rolü ekleyin.
7. Dosya şuna benzemelidir:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Dosyayı kaydedin.

## <a name="executing-the-script-on-the-target-database"></a>Hedef veritabanında komut dosyası yürütme

İdeal olarak, veritabanı projeniz dağıttığınızda bir dağıtım sonrası betiği bir parçası olarak gerekli tüm Transact-SQL betikleri çalıştırın. Ancak, dağıtım sonrası betikleri, koşullu olarak çözüm yapılandırmaları veya derleme özellikleri temel mantığı yürütmek izin verme. Alternatiftir oluşturarak MSBuild proje dosyası ' doğrudan SQL komut dosyalarınızı çalıştırmak için bir **hedef** sqlcmd.exe komut işletildikten öğesi. Hedef veritabanında betiğinizi çalıştırmak için bu komutu kullanabilirsiniz:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Sqlcmd komut satırı seçenekleri hakkında daha fazla bilgi için bkz. [sqlcmd yardımcı programını](https://msdn.microsoft.com/library/ms162773.aspx).


Bu komut bir MSBuild hedef ekleme önce hangi koşullar altında çalıştırılacak betik için istediğinize karar verin yapmanız gerekir:

- Kendi rol üyeliklerini değiştirmeden önce hedef veritabanının mevcut olması gerekir. Bu nedenle, bu komut dosyasını çalıştırmak için ihtiyacınız *sonra* veritabanı dağıtımı.
- Böylece komut dosyası, yalnızca test ortamları için yürütülür bir koşul eklemeniz gerekir.
- Bir "what IF" dağıtım çalıştırıyorsanız,&#x2014;başka bir deyişle, dağıtım betikleri oluşturma ancak bunlar gerçekten çalışan,&#x2014;SQL betiğini çalışmamalıdır.

Açıklanan bölünmüş proje dosyası yaklaşım kullanıyorsanız [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), örnek Kişi Yöneticisi çözümü tarafından gösterildiği gibi bu gibi SQL betiği için derleme yönergeleri ayırabilirsiniz:

- Ortama özgü özellikleri, izinler, dağıtılıp dağıtılmayacağını belirler özelliği, ortama özgü proje dosyasında gitmesini gerekli (örneğin, *Env Dev.proj*).
- MSBuild hedef kendisi, hedef ortamlar arasında değiştirmez herhangi bir özelliği ile birlikte Evrensel bir proje dosyasında gitmesi gereken (örneğin, *Publish.proj*).

Ortama özgü proje dosyasında, veritabanı sunucusu adı, hedef veritabanı adı ve kullanıcı rolü üyelikleri dağıtılıp dağıtılmayacağını belirtin sağlayan bir Boolean özelliği tanımlamanız gerekir.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


Evrensel bir proje dosyasında sqlcmd yürütülebilir dosyasını konumunu ve SQL betiğini çalıştırmak istediğiniz konumu sağlamanız gerekir. Bu özellikler, hedef ortam bakılmaksızın aynı kalır. Sqlcmd komutunu yürütmek için bir MSBuild hedefi oluşturmak gerekir.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Bu başka bir hedefe yararlı olabilir gibi statik bir özellik olarak sqlcmd yürütülebilir dosyasını konumunu ekleyin dikkat edin. Hedef yürütülmeden önce gerekli olmayacak buna karşılık, SQL betiğinizi konumunu ve sqlcmd komut söz dizimini hedef içinde dinamik özellikler olarak tanımlarsınız. Bu durumda, **DeployTestDBPermissions** hedef, yalnızca bu Koşullar karşılanıyorsa yürütülecek:

- **DeployTestDBRoleMemberships** özelliği **true**.
- Kullanıcı tarafından belirtilen edilmemiş bir **WhatIf = true** bayrağı.

Son olarak, hedef çağrılacak unutmayın. İçinde *Publish.proj* dosya, bunu yapabilirsiniz hedef varsayılan bağımlılık listesine ekleyerek **FullPublish** hedef. Emin olmak gereken **DeployTestDBPermissions** hedef yürütülmedi kadar **PublishDbPackages** hedef yürütüldü.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Sonuç

Bu konuda bir yolu, bir veritabanı projesi dağıttığınızda, veritabanı kullanıcıları ve rol üyeliklerini bir dağıtım sonrası eylemi olarak ekleyebileceğiniz açıklanmaktadır. Bu bir test ortamında bir veritabanı düzenli olarak yeniden oluşturursunuz ancak hazırlık veya üretim ortamları için veritabanları dağıttığınızda genellikle kaçınılmalıdır genellikle yararlı olur. Bu nedenle, bunu yapmak, uygun olduğunda veritabanı kullanıcılarının ve rol üyelikleri yalnızca oluşturulmasını sağlayacak şekilde gerekli koşullu mantığı kullanmak emin olmalısınız.

## <a name="further-reading"></a>Daha Fazla Bilgi

Veritabanı projeleri dağıtma VSDBCMD kullanma hakkında daha fazla bilgi için bkz. [veritabanı projeleri dağıtma](../web-deployment-in-the-enterprise/deploying-database-projects.md). Farklı bir hedef ortam için veritabanı dağıtımlarını özelleştirme ile ilgili yönergeler için bkz: [birden çok ortam için veritabanı dağıtımlarını özelleştirme](customizing-database-deployments-for-multiple-environments.md). Dağıtım işlemini denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz. [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Sqlcmd komut satırı seçenekleri hakkında daha fazla bilgi için bkz. [sqlcmd yardımcı programını](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Önceki](customizing-database-deployments-for-multiple-environments.md)
> [İleri](deploying-membership-databases-to-enterprise-environments.md)
