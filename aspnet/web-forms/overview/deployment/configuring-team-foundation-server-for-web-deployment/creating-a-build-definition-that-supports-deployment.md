---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Dağıtımı destekleyen bir derleme tanımı oluşturma | Microsoft Docs
author: jrjlee
description: Team Foundation Server (TFS) 2010 derleme herhangi bir türden gerçekleştirmek istiyorsanız, takım projeniz içindeki bir yapı tanımı oluşturmanız gerekir. Bu konuda des...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 18f88cff032bd0694ef98f0b19849f0edf1681ee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827394"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Dağıtımı destekleyen bir derleme tanımı oluşturma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Team Foundation Server (TFS) 2010 derleme herhangi bir türden gerçekleştirmek istiyorsanız, takım projeniz içindeki bir yapı tanımı oluşturmanız gerekir. Bu konu, TFS'de yeni bir yapı tanımı oluşturma ve web dağıtımı ekip oluşturma işleminde bir parçası olarak denetlemek nasıl açıklar.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli derleme yönergeleri içeren. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Bir yapı tanımı tfs'deki takım projeleri için derleme nasıl ve ne zaman ortaya denetleyen mekanizmadır. Her yapı tanımı belirtir:

- Visual Studio çözüm dosyası veya özel Microsoft Build Engine (MSBuild) proje dosyaları gibi oluşturmak istediğiniz şey.
- Bir derleme gerçekleştirmesi gereken durumlarda belirleyen ölçütleri, elle tetikleyiciler gibi sürekli tümleştirme (CI) yerleştirin veya Geçitli iade.
- İçin ekip web paketleri ve veritabanı betikleri gibi dağıtım yapıtları dahil olmak üzere, derleme çıktılarını göndermesi gereken konum.
- Her yapı korunması gereken süre miktarı.
- Çeşitli diğer parametreler yapı işlemi.

> [!NOTE]
> Derleme tanımları hakkında daha fazla bilgi için bkz. [bilgisayarınızı yapı işlemi tanımla](https://msdn.microsoft.com/library/ms181715.aspx).


Bu konuda, bir geliştirici yeni içeriği iade ederken bir derleme tetiklenmesi CI, kullanan bir yapı tanımı oluşturma gösterilmektedir. Oluşturma işlemi başarılı olursa, derleme hizmeti bir test ortamına çözümü dağıtmak için özel proje dosyası çalıştırır.

Bir derleme tetiklemeyi, bu eylemleri olması gerekir:

- İlk olarak, ekip çözüm oluşturması gerekir. Bu işlemin bir parçası, ekip Web yayımlama işlem hattı (her biri çözümdeki web uygulama projeleri için web dağıtımı paketleri oluşturmak için WPP) çağırır. Takım derlemesi Çözümle ilişkili herhangi bir birim testleri de çalışır.
- Çözüm yapı başarısız olursa, ekip başka bir işlem yapması. Birim testi hatalarına bir derleme hatası olarak değerlendirilmelidir.
- Ekip, çözüm derlemesi başarılı olursa, çözümün dağıtımı denetleyen özel proje dosyasını çalıştırmanız gerekir. Bu işlemin bir parçası olarak ekip (hedef web sunucularına paketlenmiş web uygulamalarını yüklemek için Web dağıtımı) Internet Information Services (IIS) Web dağıtım aracı çağırır ve veritabanı oluşturma çalıştırmak için VSDBCMD.exe yardımcı programını çağırır Hedef veritabanı sunucularında betikler.

Bu işlem gösterilmektedir:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü içeren özel bir MSBuild proje dosyası *Publish.proj*, MSBuild veya ekip çalıştırabilirsiniz. Bölümünde anlatıldığı gibi [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), bu proje dosyası web paketleri ve veritabanları için bir hedef ortam dağıtan mantığı tanımlar. Dosyayı çalıştırmak için yalnızca dağıtım görevlerini bırakarak takım yapı içinde çalışıyorsa oluşturma ve paketleme işlemi atlar mantığı içerir. Bu işlem, çünkü bu şekilde dağıtımı otomatik hale getirmek, genellikle çözüm başarıyla derlenebildiğinden emin olmak isteyebilirsiniz ve dağıtım işlemi işlemi başlamadan önce herhangi bir birim test geçirir.

Sonraki bölümde, yeni bir derleme tanımı oluşturarak bu işlemi uygulamak açıklanmaktadır.

> [!NOTE]
> Bu yordamı&#x2014;, tek bir otomatik olarak işlem yapıları, testleri ve bir çözüm dağıtır&#x2014;test ortamları için dağıtım için en uygun hale gelmesi muhtemeldir. Hazırlama ve üretim ortamları için çok daha önceden doğrulandı ve bir test ortamında doğrulanmış bir önceki yapıdan içeriği dağıtmak istediğiniz olasılığınız vardır. Bu yaklaşım, sonraki konu başlığında açıklanan [belirli bir yapı dağıtma](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Kim, yordama gerçekleştirir?

Genellikle, bir TFS Yöneticisi, bu yordamı gerçekleştirir. Bazı durumlarda, bir geliştirici Ekip Lideri, TFS'de takım projesi koleksiyonu için sorumluluk alabilir. Yeni bir yapı tanımı oluşturmak için bir üyesi olmanız gerekir **Project Collection Build Administrators** çözümünüzü içeren takım projesi koleksiyonu için Grup.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>CI ve dağıtım için bir yapı tanımı oluşturun

Sonraki yordamda CI tetikleyen bir yapı tanımı oluşturmayı açıklar. Derleme başarılı olursa, çözümü mantığı kullanarak özel bir MSBuild Projesi dosyası içinde dağıtılır.

**CI ve dağıtım için bir yapı tanımı oluşturmak için**

1. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesi düğümünü genişletin, sağ **yapılar**ve ardından **yeni yapı tanımı**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Üzerinde **genel** sekmesinde, derleme tanımı bir ad verin (örneğin, **DeployToTest**) ve isteğe bağlı bir açıklama.
3. Üzerinde **tetikleyici** sekmesinde, yeni derleme tetikleme istediğiniz ölçütleri seçin. Örneğin, çözümü oluşturun ve her zaman yeni kod, bir geliştirici denetler sınama ortamında dağıtmak istiyorsanız, seçin **sürekli tümleştirme**.
4. Üzerinde **Yapı Varsayılanları** sekmesinde **yapı çıkış aşağıdaki bırakma klasörüne Kopyala** bırakma klasörünüz Evrensel Adlandırma Kuralı (UNC) yolunu yazın (örneğin,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Bu bırakma konumu, yapılandırdığınız bekletme ilkesi bağlı olarak çeşitli yapıları depolar. Belirli bir yapıyı bir hazırlık veya üretim ortamına dağıtım yapılardan yayımlamak istediğiniz, bunları burada bulabilirsiniz budur.
5. Üzerinde **işlem** sekmesinde **yapı işlem dosyası** açılan listesinde, bırakın **DefaultTemplate.xaml** seçili. Bu, tüm yeni takım projelerine eklenin varsayılan derleme işlem şablonlarını biridir.
6. İçinde **yapı işlemi parametreleri** tablo, tıklayın **yapı öğeleri** satır ve ardından **üç nokta** düğmesi.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. İçinde **yapı öğeleri** iletişim kutusu, tıklayın **Ekle**.
8. Çözüm dosyanızın konumuna göz atın ve ardından **Tamam**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. İçinde **yapı öğeleri** iletişim kutusu, tıklayın **Ekle**.
10. İçinde **öğe türü** açılan listesinden **MSBuild proje dosyaları**.
11. Hangi, dağıtım işlemini denetleyen, dosyayı seçin ve ardından özel Proje dosyasının konumuna göz atın **Tamam**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **Yapı öğeleri** iletişim kutusu iki öğe artık gösterilmesi. **Tamam**'ı tıklatın.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Üzerinde **işlem** sekmesinde **yapı işlemi parametreleri** tablo, genişletme **Gelişmiş** bölümü.
14. İçinde **MSBuild bağımsız değişkenleri** satır, tüm MSBuild komut satırı bağımsız değişkenlerini ekleyin, *ya da* oluşturmak için öğelerin gerektirir. Kişi Yöneticisi çözümü senaryosunda, bu bağımsız değişkenleri gereklidir:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Bu örnekte:

    1. **DeployOnBuild = true** ve **DeployTarget paket =** Kişi Yöneticisi çözümü oluşturduğunuzda bağımsız değişkenleri gereklidir. Bu web dağıtımı paketleri her web uygulaması projesi oluşturduktan sonra açıklandığı gibi oluşturmak için MSBuild bildirir [oluşturma ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. **TargetEnvPropsFile** bağımsız değişkeni, oluşturma sırasında gereklidir *Publish.proj* dosya. Bu özellik anlatıldığı gibi ortama özgü yapılandırma dosyasının konumunu gösterir [derleme işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Üzerinde **Bekletme İlkesi** sekmesinde, gerektiği gibi korumak istediğiniz her türden kaç yapılar yapılandırın.
17. **Kaydet**'e tıklayın.

## <a name="queue-a-build"></a>Bir Derlemeyi Sıraya Koyma

Bu noktada, en az bir yapı tanımını oluşturdunuz. Tanımladığınız derleme işlemi, artık derleme tanımında belirtilen Tetikleyiciler göre çalıştırılır.

Yapı tanımınızı CI kullanmak için yapılandırdıysanız, yapı tanımı iki şekilde test edebilirsiniz:

- Bazı içerikler otomatik bir derleme tetiklemeyi takım projesine iade edin.
- Bir derlemeyi el ile sıraya.

**Bir derlemeyi el ile sıraya koyma**

1. İçinde **Takım Gezgini** penceresinde derleme tanımı sağ tıklayın ve ardından **yeni derlemeyi sıraya koy**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. İçinde **derleme kuyruğu** iletişim kutusunda, derleme özellikleri gözden geçirin ve ardından **kuyruk**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

İlerleme ve bir derleme sonucunu gözden geçirmek için&#x2014;olup olmadığını onu el ile veya otomatik olarak tetiklendi bakılmaksızın&#x2014;derleme tanımında çift **Takım Gezgini** penceresi. Bunu bir **Yapı Gezgini** sekmesi.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Buradan, başarısız yapılara giderebilirsiniz. Tek bir yapıya çift tıklayın, Özet bilgileri görüntülemek ve ayrıntılı günlük dosyaları için tıklayın.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Başarısız derlemeler ve başka bir yapı çalışmadan önce tüm sorunları gidermek için bu bilgileri kullanabilirsiniz.

> [!NOTE]
> Dağıtım mantıksal yürütülen yapıların büyük olasılıkla hedef ortamda gereken izinler yapı sunucusu izni kadar başarısız olacak. Daha fazla bilgi için [takım derleme dağıtımı izinlerini yapılandırma](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Yapı işlemini izleyin

TFS, geniş bir yapı işlemi izlemenize yardımcı olmak için işlevsellik sağlar. Örneğin, TFS e-posta Gönder veya bir derleme tamamlandığında, görev çubuğu bildirim alanında uyarıları görüntüler. Daha fazla bilgi için [çalıştırma ve izleme yapılar](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Sonuç

Bu konuda TFS'de bir yapı tanımı oluşturmak nasıl kaydedileceği açıklanır. Derleme tanımı, bu nedenle her bir geliştirici içeriği takım projesine iade derleme işlemi çalıştırılır CI için yapılandırılır. Derleme tanımı, web paketleri ve veritabanı betikleri için bir hedef sunucu ortamı dağıtmak için özel bir MSBuild proje dosyası yürütür.

Bir yapı işleminin bir parçası başarılı olması bir otomatik dağıtım için sırada derleme hizmeti hesabının hedef web sunucuları ve hedef veritabanı sunucusunda uygun izinleri vermeniz gerekir. Bu öğreticide, son konu [takım derleme dağıtımı izinlerini yapılandırma](configuring-permissions-for-team-build-deployment.md), tanımlamak ve bir takım derleme sunucusu otomatik dağıtımı için gerekli izinleri yapılandırma açıklanmaktadır.

## <a name="further-reading"></a>Daha Fazla Bilgi

Derleme tanımı oluşturma hakkında daha fazla bilgi için bkz. [temel bir yapı tanımı oluşturma](https://msdn.microsoft.com/library/ms181716.aspx) ve [bilgisayarınızı yapı işlemi tanımlama](https://msdn.microsoft.com/library/ms181715.aspx). Derlemeler kuyruğa alınırken hakkında daha fazla yönergeler için bkz [bir yapıyı sıraya](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Önceki](configuring-a-tfs-build-server-for-web-deployment.md)
> [İleri](deploying-a-specific-build.md)
