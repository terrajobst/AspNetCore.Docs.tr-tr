---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Belirli bir derlemeyi dağıtma | Microsoft Docs
author: jrjlee
description: Bu konuda, web paketleri ve bir hazırlık veya üretim ortamını gibi yeni bir hedef veritabanı komutları belirli bir önceki yapıdan dağıtmayı açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6d55497dbc13133aa9c8b8eaecca0f6915fd9ed0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388230"
---
<a name="deploying-a-specific-build"></a>Belirli bir derlemeyi dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web paketleri ve bir hazırlık veya üretim ortamı gibi yeni bir hedef veritabanı komutları belirli bir önceki yapıdan dağıtmayı açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli derleme yönergeleri içeren. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Şimdiye kadar Bu öğretici kümesinde konuları derleme, paketleme ve web uygulamaları ve veritabanlarının tek adımlı bir parçası olarak dağıtma konusunda odaklanmış veya işlem otomatik. Ancak, bazı yaygın senaryolarda, dağıttığınız kaynakları yapı bırakma klasöründe bir listeden seçmek isteyebilirsiniz. Diğer bir deyişle, dağıtmak istediğiniz derleme en son sürüme sahip olmayabilir.

Önceki konu başlığında, sürekli tümleştirme (CI) senaryoyu göz önünde bulundurun [bir derleme tanımı, destekleyen dağıtım oluşturma](creating-a-build-definition-that-supports-deployment.md). Team Foundation Server (TFS) 2010 derleme tanımını oluşturdunuz. Bir geliştirici kod TFS'ye denetler. her zaman, ekip, kodunuzu derlemek, web paketleri ve veritabanı betikleri oluşturma işleminin bir parçası oluşturun, herhangi bir birim testleri çalıştırma ve kaynaklarınızın bir test ortamına dağıtın. Yapı tanımınızı oluştururken yapılandırılmış bekletme ilkesi bağlı olarak, TFS belirli bir önceki derleme sayısı korur.

![](deploying-a-specific-build/_static/image1.png)

Şimdi, doğrulama gerçekleştirdiğiniz doğrulama sınaması bunlardan birini karşı test ortamınızda oluşturur ve uygulamanızı bir hazırlama ortamına dağıtmak hazır olduğunu varsayalım. Bu arada, geliştiricilerin yeni kodu iade etmiş olabilir. Çözümü yeniden oluşturun ve hazırlama ortamına dağıtmak istemediğiniz ve en son sürüme hazırlama ortamına dağıtmak istemiyorsanız. Bunun yerine, doğrulanabilir ve test sunucuları doğrulanmış belirli yapı dağıtmak istediğiniz.

Bunu yapmak için Microsoft Build Engine (MSBuild) web paketleri ve belirli bir yapıyı oluşturan veritabanı komut dosyaları nerede bulacağını söylemeniz gerekir.

## <a name="overriding-the-outputroot-property"></a>OutputRoot özelliği geçersiz kılma

İçinde [örnek çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* dosya bildirir adlandırılmış bir özelliği **OutputRoot**. Adından da anlaşılacağı gibi yapı işleminin oluşturduğu her şeyi içeren kök klasörü budur. İçinde *Publish.proj* görebilirsiniz, dosya, **OutputRoot** özelliği tüm dağıtım kaynakları kök konumunu belirtir.

> [!NOTE]
> **OutputRoot** yaygın olarak kullanılan özellik adı. Visual C# ve Visual Basic proje dosyaları, tüm derleme çıktılarını kök konumunu depolamak için bu özellik ayrıca bildirin.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Web paketleri dağıtma ve komut dosyaları farklı bir konumdan veritabanı için proje dosyanızı istiyorsanız&#x2014;ister bir önceki TFS derleme çıktıları&#x2014;geçersiz kılmak yeterlidir **OutputRoot** özelliği. İlgili yapı klasörüne özellik değeri ekip sunucusunda ayarlamanız gerekir. MSBuild komut satırından çalıştırmak için bir değer belirtebilirsiniz **OutputRoot** komut satırı bağımsız değişkeni olarak:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


Uygulamada, ancak aynı zamanda atlamak istediğiniz **derleme** hedef&#x2014;yapı çıkışlarını kullanmayı planlamıyorsanız, çözümünüzü oluşturmaya noktası yok. Komut satırından yürütmek istediğiniz hedefleri belirleyerek bunu yapabilirsiniz:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Ancak, çoğu durumda, bir TFS yapı tanımına dağıtım mantığınızı oluşturmak isteyebilirsiniz. Bu kullanıcılarla sağlar **derlemeleri sıraya** izni TFS sunucusu bağlantısı olan herhangi bir Visual Studio yüklemesinden dağıtım tetikleyin.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Belirli dağıtmak için bir yapı tanımı oluşturma yapıları

Sonraki yordam, kullanıcıların tetikleyici dağıtımlar için tek bir komutla bir hazırlık ortamı için sağlayan bir yapı tanımı oluşturmak açıklar.

Bu durumda, gerçekte herhangi bir şey oluşturmak için yapı tanımını istemediğiniz&#x2014;özel proje dosyanızı dağıtım mantıksal yürütmek için yalnızca istediğiniz. *Publish.proj* dosya atlar koşullu mantık içerir **derleme** dosya takım yapısı'nda çalışır durumda değilse hedef. Bunu yerleşik değerlendirerek yapar **BuildingInTeamBuild** özelliği ayarlandığında otomatik olarak **true** takım yapısı'nda proje dosyanız çalıştırırsanız. Sonuç olarak, derleme işleminin atlayın ve varolan bir yapıyı dağıtmak için proje dosyasını çalıştırmanız yeterlidir.

**Dağıtım el ile tetiklemek için bir yapı tanımı oluşturmak için**

1. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesi düğümünü genişletin, sağ **yapılar**ve ardından **yeni yapı tanımı**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Üzerinde **genel** sekmesinde, derleme tanımı bir ad verin (örneğin, **DeployToStaging**) ve isteğe bağlı bir açıklama.
3. Üzerinde **tetikleyici** sekmesinde **el ile-iadeler yeni bir derlemeyi tetiklemez**.
4. Üzerinde **Yapı Varsayılanları** sekmesinde **yapı çıkış aşağıdaki bırakma klasörüne Kopyala** bırakma klasörünüz Evrensel Adlandırma Kuralı (UNC) yolunu yazın (örneğin,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Üzerinde **işlem** sekmesinde **yapı işlem dosyası** açılan listesinde, bırakın **DefaultTemplate.xaml** seçili. Bu, tüm yeni takım projelerine eklenin varsayılan derleme işlem şablonlarını biridir.
6. İçinde **yapı işlemi parametreleri** tablo, tıklayın **yapı öğeleri** satır ve ardından **üç nokta** düğmesi.

    ![](deploying-a-specific-build/_static/image4.png)
7. İçinde **yapı öğeleri** iletişim kutusu, tıklayın **Ekle**.
8. İçinde **öğe türü** açılan listesinden **MSBuild proje dosyaları**.
9. Hangi, dağıtım işlemini denetleyen, dosyayı seçin ve ardından özel Proje dosyasının konumuna göz atın **Tamam**.

    ![](deploying-a-specific-build/_static/image5.png)
10. İçinde **yapı öğeleri** iletişim kutusu, tıklayın **Tamam**.
11. İçinde **yapı işlemi parametreleri** tablo, genişletme **Gelişmiş** bölümü.
12. İçinde **MSBuild bağımsız değişkenleri** satır, ortama özgü proje dosyanızın konumunu belirtin ve yapı klasörünüzün konumu için bir yer tutucu ekleyin:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Geçersiz kılmak ihtiyacınız olacak **OutputRoot** bir yapıyı sıraya her zaman değeri. Bu, bir sonraki yordamda ele alınmıştır.
13. **Kaydet**'e tıklayın.

Bir derleme tetiklemeyi, güncelleştirmeye gerek duyduğunuz **OutputRoot** dağıtmak istediğiniz yapıyı işaret edecek şekilde özelliği.

**Belirli bir yapının bir yapı tanımından dağıtmak için**

1. İçinde **Takım Gezgini** penceresinde derleme tanımı sağ tıklayın ve ardından **yeni derlemeyi sıraya koy**.

    ![](deploying-a-specific-build/_static/image7.png)
2. İçinde **derleme kuyruğu** iletişim kutusundaki **parametreleri** sekmesinde, genişletme **Gelişmiş** bölümü.
3. İçinde **MSBuild bağımsız değişkenleri** satır, değiştirin **OutputRoot** derleme iş klasörünüzün konumu ile özelliği. Örneğin:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Derleme iş klasörünüzün yolunu sonunda eğik eklediğinizden emin olun.
4. Tıklayın **kuyruk**.

Yapıyı kuyruğa alın, proje dosyası veritabanı betikleri dağıtır ve web, belirtilen yapı bırakma klasöründen paketleri **OutputRoot** özelliği.

## <a name="conclusion"></a>Sonuç

Bu konuda web paketleri ve veritabanı betikleri gibi dağıtım kaynakları yayımlama, önceki belirli bir bölünmüş proje dosyası dağıtım modelini kullanarak yapı açıklanmıştır. Geçersiz kılma açıklaması **OutputRoot** özelliği ve nasıl bir TFS'ye dağıtım mantığı eklemek derleme tanımı.

## <a name="further-reading"></a>Daha Fazla Bilgi

Derleme tanımı oluşturma hakkında daha fazla bilgi için bkz. [temel bir yapı tanımı oluşturma](https://msdn.microsoft.com/library/ms181716.aspx) ve [bilgisayarınızı yapı işlemi tanımlama](https://msdn.microsoft.com/library/ms181715.aspx). Derlemeler kuyruğa alınırken hakkında daha fazla yönergeler için bkz [bir yapıyı sıraya](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Önceki](creating-a-build-definition-that-supports-deployment.md)
> [İleri](configuring-permissions-for-team-build-deployment.md)
