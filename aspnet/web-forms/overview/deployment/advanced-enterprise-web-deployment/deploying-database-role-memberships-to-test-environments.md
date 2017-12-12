---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "Test ortamları için veritabanı rolü üyeliği dağıtma | Microsoft Docs"
author: jrjlee
description: "Bu konu, veritabanı rolleri bir test ortamı için çözüm dağıtımının bir parçası olarak kullanıcı hesaplarını eklemek açıklar. İçeren bir çözümü dağıttığınızda..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: ac780c6cd522f9216cafe3b5f23772ef6ebf5d11
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Test ortamları için veritabanı rolü üyeliği dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konu, veritabanı rolleri bir test ortamı için çözüm dağıtımının bir parçası olarak kullanıcı hesaplarını eklemek açıklar.
> 
> Bir hazırlık veya üretim ortamı için bir veritabanı proje içeren bir çözümü dağıttığınızda, kullanıcı hesapları veritabanı rolleri için eklenmesi otomatikleştirmek için geliştirici genellikle istemezsiniz. Çoğu durumda, geliştirici hangi veritabanı rolleri eklenmesi gereken hangi kullanıcı hesaplarının bilemezsiniz ve bu gereksinimleri dilediğiniz zaman değiştirebilirsiniz. Bir geliştirme için bir veritabanı proje içeren bir çözümü dağıtmak veya sınama ortamında, ancak genellikle yerine farklı durumdur:
> 
> - Geliştirici genellikle düzenli olarak çözüm günde genellikle birkaç kez yeniden dağıtır.
> - Veritabanı genellikle veritabanı kullanıcıları oluşturulmalı ve her dağıtım sonrası rollere eklenen, yani her dağıtımda yeniden oluşturulur.
> - Geliştirici genellikle hedef geliştirme veya test ortamı üzerinde tam denetime sahiptir.
> 
> Bu senaryoda, otomatik olarak veritabanı kullanıcıları oluşturun ve dağıtım işleminin bir parçası veritabanı rolü üyeliği atamak faydalıdır.
> 
> Bu işlem koşullu hedef ortamına bağlı gerektiğini anahtar faktördür. Bir hazırlık veya üretim ortamı dağıtıyorsanız, işlemi atlamak istiyor. Bir geliştirici dağıtıyorsunuz veya sınama ortamında, rol üyeliklerini daha fazla müdahalesi olmadan dağıtmak istediğiniz. Bu konuda, bu sorunu gidermek için kullanabileceğiniz bir yaklaşım açıklanmaktadır.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bu konuda, varsayılır:

- Bölünmüş proje dosyası yaklaşımı Çözüm dağıtımı açıklandığı gibi kullandığınız [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- VSDBCMD açıklandığı gibi veritabanı projenizi dağıtmanın proje dosyasından çağrılmasına [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Veritabanı kullanıcıları oluşturun ve bir test ortamı için bir veritabanı projesi dağıttığınızda rol üyeliklerini atamak için yapmanız:

- Gerekli veritabanı değişiklikleri yapar Transact yapılandırılmış sorgu dili (Transact-SQL) komut dosyası oluşturun.
- SQL komut dosyasını çalıştırmak için sqlcmd.exe yardımcı programını kullanan bir Microsoft Build Engine (MSBuild) hedef oluşturun.
- Çözümünüz için bir test ortamı dağıtıyorsanız hedef çağırmak için proje dosyalarınıza yapılandırın.

Bu konuda her bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir.

## <a name="scripting-the-database-role-memberships"></a>Veritabanı rol üyeliklerini komut dosyası oluşturma

Seçtiğiniz herhangi bir konuma ve farklı şekillerde çok miktarda bir Transact-SQL komut dosyası oluşturabilirsiniz. En kolay yaklaşım, Visual Studio 2010'komut dosyası, çözümünüz içinde oluşturmaktır.

**Bir SQL betiği oluşturmak için**

1. İçinde **Çözüm Gezgini** penceresi, veritabanı projesi düğümünü genişletin.
2. Sağ **betikleri** klasörünü **Ekle**ve ardından **yeni klasör**.
3. Tür **Test** klasör adı ve ENTER tuşuna basın.
4. Sağ **Test** klasörünü **Ekle**ve ardından **betik**.
5. İçinde **Yeni Öğe Ekle** iletişim kutusunda, kodunuzu anlamlı bir ad verin (örneğin, **AddRoleMemberships.sql**) ve ardından **Ekle**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. İçinde *AddRoleMemberships.sql* dosya, Transact-SQL deyimlerini ekleyin:

    1. Bir veritabanı kullanıcısı, veritabanına erişecek SQL Server oturumu açma oluşturun.
    2. Veritabanı kullanıcı için tüm gerekli veritabanı rolleri ekleyin.
7. Dosya şuna benzemelidir:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Dosyayı kaydedin.

## <a name="executing-the-script-on-the-target-database"></a>Hedef veritabanı komut dosyası yürütme

İdeal olarak, veritabanı projenizi dağıttığınızda bir dağıtım sonrası komut dosyasının bir parçası gerekli herhangi bir Transact-SQL betiği çalıştırın. Ancak, dağıtım sonrası komut dosyalarını, çözüm yapılandırmaları veya derleme özellikleri koşullu temel mantığı yürütmek izin verme. Alternatiftir oluşturarak MSBuild proje dosyası ' doğrudan SQL komut dosyaları çalıştırmak için bir **hedef** sqlcmd.exe komutu yürütür öğesi. Hedef veritabanında kodunuzu çalıştırmak için bu komutu kullanın:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Sqlcmd komut satırı seçenekleri hakkında daha fazla bilgi için bkz: [sqlcmd yardımcı programını](https://msdn.microsoft.com/en-us/library/ms162773.aspx).


Bu komut bir MSBuild hedef katıştırmak önce komut dosyasının çalışmasını istediğiniz hangi koşullarda dikkate almanız gerekir:

- Rol üyeliklerini değiştirmeden önce hedef veritabanının mevcut olması gerekir. Bu nedenle, bu komut dosyasını çalıştırmak gereken *sonra* veritabanı dağıtımı.
- Komut yalnızca test ortamları için yürütülebilir bir koşul içermesi gerekir.
- Diğer bir deyişle, bir "," dağıtım & #x 2014; çalıştırıyorsanız dağıtım betikleri oluşturma ancak aslında bunları & #x 2014; çalışan SQL betiği çalıştırma döndürmemelidir.

Açıklanan bölünmüş proje dosyası yaklaşım kullanıyorsanız [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), kişinin Yöneticisi örnek çözümü tarafından gösterildiği gibi bu gibi SQL betiği için yapı yönergeleri bölebilirsiniz:

- Ortama özgü özellikleri, izinler, dağıtılıp dağıtılmayacağını belirler özelliği ortama özgü proje dosyasında gitmesini gerekli (örneğin, *Env Dev.proj*).
- MSBuild hedef kendisini hedef ortamlar arasında değiştirmez herhangi bir özellik birlikte Evrensel proje dosyasında gitmesi gereken (örneğin, *Publish.proj*).

Ortama özgü proje dosyasında veritabanı sunucusu adı, hedef veritabanı adı ve rol üyeliklerini dağıtılıp dağıtılmayacağını belirtin kullanıcı olanak sağlayan bir Boolean özelliği tanımlamanız gerekir.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


Evrensel proje dosyasında yürütülebilir sqlcmd konumunu ve çalıştırmak istediğiniz SQL komut dosyası konumunu sağlamanız gerekir. Bu özellikler, hedef ortam bakılmaksızın aynı kalır. Ayrıca sqlcmd komutu çalıştırmak için bir MSBuild hedef oluşturmanız gerekir.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Bu diğer hedefler için yararlı olabilecek gibi bir statik özellik olarak yürütülebilir sqlcmd konumunu ekleyin dikkat edin. Hedef yürütülmeden önce gerekli olmayacak buna karşılık, SQL komut dosyasının konumunu ve sqlcmd komut sözdizimi hedef içinde dinamik özellikleri olarak tanımladığınız. Bu durumda, **DeployTestDBPermissions** hedef, yalnızca bu Koşullar karşılanıyorsa yürütülür:

- **DeployTestDBRoleMemberships** özelliği ayarlanmış **doğru**.
- Kullanıcı belirtilen kurmadı bir **whatIf = true** bayrağı.

Son olarak, hedef çağrılacak unutmayın. İçinde *Publish.proj* dosya, bunu yapabilirsiniz varsayılan bağımlılık listesine hedef ekleyerek **FullPublish** hedef. Emin olmak gereken **DeployTestDBPermissions** hedef yürütülmez kadar **PublishDbPackages** hedef yürütüldü.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Sonuç

Bu konuda bir yolu, bir veritabanı projesi dağıttığınızda, veritabanı kullanıcıları ve rol üyeliklerini bir dağıtım sonrası eylemi olarak ekleyebileceğiniz açıklanmaktadır. Bu, düzenli olarak test ortamında bir veritabanını yeniden oluşturun, ancak genellikle hazırlık veya üretim ortamları için veritabanları dağıttığınızda kaçınılmalıdır genelde yararlıdır. Bu nedenle, böylece bunu yapmak, uygun olduğunda veritabanı kullanıcıları ve rol üyeliklerini yalnızca oluşturulur gerekli koşullu mantık kullanmak emin olmalısınız.

## <a name="further-reading"></a>Daha Fazla Bilgi

Veritabanı projeleri dağıtmak için VSDBCMD kullanma hakkında daha fazla bilgi için bkz: [dağıtma veritabanı projeleri](../web-deployment-in-the-enterprise/deploying-database-projects.md). Farklı bir hedef ortamlar için veritabanı dağıtımlarını özelleştirme ile ilgili yönergeler için bkz: [veritabanı dağıtımlarını özelleştirme birden çok ortamları](customizing-database-deployments-for-multiple-environments.md). Dağıtım işlemi denetlemek için özel MSBuild proje dosyalarını kullanma hakkında daha fazla bilgi için bkz: [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md) ve [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Sqlcmd komut satırı seçenekleri hakkında daha fazla bilgi için bkz: [sqlcmd yardımcı programını](https://msdn.microsoft.com/en-us/library/ms162773.aspx).

>[!div class="step-by-step"]
[Önceki](customizing-database-deployments-for-multiple-environments.md)
[sonraki](deploying-membership-databases-to-enterprise-environments.md)
