---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "Belirli bir yapı dağıtma | Microsoft Docs"
author: jrjlee
description: "Bu konuda, web paketleri ve hazırlık veya üretim ortamını gibi yeni bir hedefe belirli bir önceki yapıdan veritabanı komut dosyalarında dağıtmayı açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: be1000f0cbc2f509f5014789c2bc47ce2b12fb2f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-a-specific-build"></a>Belirli bir yapı dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, web paketleri ve bir hazırlık veya üretim ortamı gibi yeni bir hedefe belirli bir önceki yapıdan veritabanı komut dosyalarında dağıtmayı açıklar.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi iki proje dosyalarını & #x 2014 tarafından kontrol edilir; o Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir uygulama oluşturma yönergeleri içeren ne. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Şimdiye kadar Bu öğretici kümedeki konular yapı, paket ve web uygulamaları ve veritabanlarını tek adımlı bir parçası olarak dağıtma odaklanan veya işlem otomatik. Ancak, bazı genel senaryolar bırakma klasörüne derlemelerde listesinden dağıttığınız kaynakları seçin istersiniz. Diğer bir deyişle, en son sürüme dağıtmak istediğiniz yapı olmayabilir.

Önceki konu başlığı altında açıklanan sürekli tümleştirme (CI) senaryoyu göz önünde bulundurun [bir yapı tanımı olduğunu destekleyen dağıtım oluşturma](creating-a-build-definition-that-supports-deployment.md). Team Foundation Server (TFS) 2010 bir derleme tanımınız oluşturduğunuzu düşünün. Geliştirici kodunu TFS'ye denetler. her zaman, ekip kodunuzu yapı, web paketleri ve veritabanı komut dosyalarında yapılandırma işleminin bir parçası olarak, tüm birim testleri çalıştırma, oluşturup kaynaklarınızı bir test ortamına dağıtabilirsiniz. Yapı tanımı oluştururken yapılandırılmış bekletme ilkesi bağlı olarak, TFS belirli bir sayıda önceki yapıları korur.

![](deploying-a-specific-build/_static/image1.png)

Şimdi, doğrulama gerçekleştirdiğiniz ve bunlardan birini karşı test doğrulama, test ortamınızda oluşturur ve hazırlama ortamına uygulamanızı dağıtmak hazır varsayalım. Bu arada, geliştiricilerin yeni kodda kullanıma. Çözümü yeniden derleyin ve hazırlama ortamına dağıtmak istediğiniz yoktur ve en son sürüme hazırlama ortamına dağıtmayı istemiyorum. Bunun yerine, doğrulandı ve test sunucularda doğrulanmış belirli yapı dağıtmak istediğiniz.

Bunu başarmak için Microsoft Build Engine (MSBuild) web paketleri ve belirli bir yapı oluşturulan veritabanı komut dosyaları nerede bulacağını bildirir gerekir.

## <a name="overriding-the-outputroot-property"></a>OutputRoot özelliği geçersiz kılma

İçinde [örnek çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* dosya adlı bir özelliği bildirir **OutputRoot**. Adı da anlaşılacağı gibi derleme işlemini oluşturan her şeyi içeren kök klasörü budur. İçinde *Publish.proj* dosya, gördüğünüz, **OutputRoot** tüm dağıtım kaynaklarını kök konumunu başvurduğu özelliği.

> [!NOTE]
> **OutputRoot** yaygın olarak kullanılan özellik adı. Visual C# ve Visual Basic proje dosyalarını da tüm yapı çıkışları kök konumunu depolamak için bu özelliği bildirin.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Proje dosyanızı web paketleri ve farklı bir konuma & #x 2014; bir önceki TFS derlemesi & #x 2014; çıkışları gibi veritabanı komut dosyalarında dağıtmak istiyorsanız, geçersiz kılmak yeterlidir **OutputRoot** özelliği. Özellik değeri ilgili derleme klasörüne ekip sunucuda ayarlamanız gerekir. MSBuild komut satırından çalışıyormuş için bir değer belirtebilirsiniz **OutputRoot** komut satırı bağımsız değişkeni olarak:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


Uygulamada, ancak aynı zamanda atlamak istediğiniz **yapı** hedef & #x 2014; derleme çıktıları kullanmayı planlamıyorsanız, çözümü oluşturma içinde noktası yok. Bu komut satırından çalıştırmak istediğiniz hedefleri belirterek yapabilirsiniz:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Bununla birlikte, çoğu durumda, dağıtım mantığınızı TFS derleme tanımı oluşturmak istersiniz. Bu kullanıcılarla sağlar **sıraya derlemeleri** bağlantısı olan herhangi bir Visual Studio yüklemesi dağıtımından TFS sunucusuna tetiklemek için izni.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Dağıtmak için derleme tanımını özel oluşturma yapıları

Sonraki yordam, tetikleyici dağıtımları için hazırlama ortamında tek bir komutla kullanıcılara sağlayan bir derleme tanımınız oluşturmayı açıklar.

Bu durumda, gerçekten her şeyi oluşturmak için derleme tanımını istemediğiniz & #x 2014; yalnızca özel proje dosyanızda dağıtım mantığı yürütmek istediğiniz. *Publish.proj* dosyası atlar koşullu mantık içerir **yapı** dosya ekip çalışıyorsa, hedef. Bunu yerleşik değerlendirerek yapar **BuildingInTeamBuild** otomatik olarak ayarlamak için özellik **true** ekip içinde proje dosyanızı çalıştırırsanız. Sonuç olarak, yapı işlemi atlayın ve varolan bir yapıyı dağıtmak için proje dosyası çalıştırmanız yeterlidir.

**El ile dağıtımı tetikleyecek bir yapı tanımı oluşturmak için**

1. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesi düğümünü genişletin, sağ **derlemeler**ve ardından **yeni yapı tanımı**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Üzerinde **genel** sekmesinde, yapı tanımı bir ad verin (örneğin, **DeployToStaging**) ve isteğe bağlı bir açıklama.
3. Üzerinde **tetikleyici** sekmesine **el ile – iadeler yeni bir yapı tetiklemez**.
4. Üzerinde **Yapı Varsayılanları** sekmesinde **kopyalama yapı çıktısını aşağıdaki bırakma klasörüne** açılan klasörünüze Evrensel Adlandırma Kuralı (UNC) yolunu yazın (örneğin,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Üzerinde **işlem** sekmesinde **derleme işlemi dosya** açılır listesinde, bırakın **DefaultTemplate.xaml** seçili. Bu, tüm yeni takım projeleri için eklenir varsayılan derleme işlem şablonları biridir.
6. İçinde **oluşturma işlemi parametreleri** tablo, tıklatın **yapı öğelerine** satır ve ardından **üç nokta** düğmesi.

    ![](deploying-a-specific-build/_static/image4.png)
7. İçinde **yapı öğelerine** iletişim kutusu, tıklatın **Ekle**.
8. İçinde **türdeki öğeleri** açılır listesinden, **MSBuild proje dosyalarını**.
9. Hangi, dağıtım işlemini denetleyen, dosyayı seçin ve ardından özel Proje dosyasının konumuna göz atın **Tamam**.

    ![](deploying-a-specific-build/_static/image5.png)
10. İçinde **yapı öğelerine** iletişim kutusu, tıklatın **Tamam**.
11. İçinde **oluşturma işlemi parametreleri** tablo, genişletin **Gelişmiş** bölümü.
12. İçinde **MSBuild bağımsız değişkenleri** satır, ortama özgü proje dosyasının konumunu belirtin ve yapı klasörünün konumu için bir yer tutucu ekleyin:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Geçersiz kılma gerekir **OutputRoot** bir yapıyı sıraya her zaman değeri. Bu bir sonraki yordamda ele alınmıştır.
13. **Kaydet**'e tıklayın.

Bir derlemeyi tetiklemeyi, güncelleştirmeniz gerekir **OutputRoot** dağıtmak istediğiniz yapı işaret edecek şekilde özelliği.

**Bir derleme tanımınız belirli bir derlemeden dağıtmak için**

1. İçinde **Takım Gezgini** penceresinde, yapı tanımına sağ tıklayın ve ardından **yeni yapıyı sıraya al**.

    ![](deploying-a-specific-build/_static/image7.png)
2. İçinde **Yapıyı Sıraya Al** iletişim kutusundaki **parametreleri** sekmesinde, genişletin **Gelişmiş** bölümü.
3. İçinde **MSBuild bağımsız değişkenleri** satır, değerini **OutputRoot** yapı klasörünün konumunu özelliğiyle. Örneğin:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Eğik yapı klasörünüzün yolunu sonunda eklediğinizden emin olun.
4. Tıklatın **sıra**.

Yapıyı sıraya, proje dosyası veritabanı komut dosyalarında dağıtır ve web paketler derleme bırakma klasöründen belirttiğiniz **OutputRoot** özelliği.

## <a name="conclusion"></a>Sonuç

Bu konuda web paketleri ve veritabanı betikleri gibi dağıtım kaynaklarını yayınlamak için bölme proje dosyası dağıtım modelini kullanarak önceki belirli bir yapı açıklanmıştır. Geçersiz kılmak nasıl açıklandığı **OutputRoot** özelliği ve dağıtım mantığı bir TFS içerecek şekilde nasıl yapı tanımı.

## <a name="further-reading"></a>Daha Fazla Bilgi

Derleme tanımları oluşturma hakkında daha fazla bilgi için bkz: [temel yapı tanımı oluşturma](https://msdn.microsoft.com/library/ms181716.aspx) ve [bilgisayarınızı derleme işlemi tanımlamak](https://msdn.microsoft.com/library/ms181715.aspx). Sıraya alma yapılar hakkında daha fazla yönergeler için bkz [bir yapıyı sıraya](https://msdn.microsoft.com/library/ms181722.aspx).

>[!div class="step-by-step"]
[Önceki](creating-a-build-definition-that-supports-deployment.md)
[sonraki](configuring-permissions-for-team-build-deployment.md)
