---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Dağıtım destekleyen bir yapı tanımı oluşturma | Microsoft Docs
author: jrjlee
description: Team Foundation Server (TFS) 2010 her türlü derleme gerçekleştirmek istiyorsanız, bir derleme tanımı takım projenizin içinde oluşturmanız gerekir. Bu konuda des...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Dağıtım destekleyen bir yapı tanımı oluşturma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Team Foundation Server (TFS) 2010 her türlü derleme gerçekleştirmek istiyorsanız, bir derleme tanımı takım projenizin içinde oluşturmanız gerekir. Bu konuda TFS'de yeni bir derleme tanımı oluşturun ve web dağıtımı ekip oluşturma işleminde bir parçası olarak denetim açıklar.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici seri kullanan örnek bir çözüm&#x2014; [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;bir ASP.NET MVC 3 uygulama, bir Windows Communication dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulaması temsil etmek için Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme ve dağıtım süreci tarafından denetlenen içinde iki dosyaları proje&#x2014;bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir uygulama oluşturma yönergeleri içeren. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

Yapı tanımı nasıl ve ne zaman derlemeleri TFS takım projelerinde oluşabilir denetleyen mekanizmadır. Her bir yapı tanımı belirtir:

- Visual Studio çözümü veya özel Microsoft Build Engine (MSBuild) proje dosyaları gibi oluşturmak istediğiniz şey.
- Bir yapı zaman belirleyen ölçütleri, el ile tetikleyiciler gibi sürekli tümleştirme (CI) yerleştirin veya iadeler geçişli.
- Web paketleri ve veritabanı betikleri gibi dağıtım yapıları dahil olmak üzere için ekip yapı göndermesi gereken konum çıkarır.
- Her yapı korunması gereken süre miktarı.
- Diğer çeşitli parametreler yapı işlemi.

> [!NOTE]
> Derleme tanımları hakkında daha fazla bilgi için bkz: [bilgisayarınızı derleme işlemi tanımlamak](https://msdn.microsoft.com/library/ms181715.aspx).


Bu konuda bir geliştirici yeni içerik denetlerken bir yapı tetiklenir CI, kullanan bir derleme tanımınız oluşturmak üzere nasıl yapacağınızı gösterir. Yapı başarılı olursa, yapı hizmeti bir test ortamına çözümü dağıtmak için özel proje dosyasını çalıştırır.

Bir derlemeyi tetiklemeyi, bu eylemleri gerçekleşmesi gerekir:

- İlk olarak, ekip çözüm oluşturması gerekir. Bu işlemin bir parçası olarak, ekip Web yayımlama ardışık düzen (her web uygulama projeleri için web dağıtımı paketleri oluşturmak için (WPP) çağırır. Ekip derlemesi da Çözümle ilişkili herhangi bir birim testleri çalıştırın.
- Çözüm derleme başarısız olursa, ekip başka bir eylem almanız gerekir. Birim test başarısızlıklarının derleme hatası olarak değerlendirilmelidir.
- Çözüm oluşturma işlemi başarılı olursa, ekip çözüm dağıtımını denetler özel proje dosyasını çalıştırmanız gerekir. Bu işlemin bir parçası olarak ekip (hedef web sunucularında paketlenmiş web uygulamalarını yüklemek için Web dağıtımı) Internet Information Services (IIS) Web dağıtım aracı çağırır ve veritabanı oluşturma çalıştırmak için VSDBCMD.exe yardımcı programını çağırır Hedef veritabanı sunucularında komut dosyaları.

Bu işlem gösterilmektedir:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

[Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü içeren özel bir MSBuild proje dosyası *Publish.proj*, MSBuild veya ekip çalıştırabilirsiniz. Bölümünde açıklandığı gibi [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md), bu proje dosyası web paketleri ve veritabanları için bir hedef ortam dağıtır mantığını tanımlar. Dosyayı çalıştırmak için yalnızca dağıtım görevlerini bırakarak ekip içinde çalışıyorsa yapı ve Paketleme işlemini atlar mantığı içerir. Bu dağıtım bu şekilde Otomasyon gerçekleştirirken, genellikle çözümün başarıyla derlemeler emin olmak isteyebilirsiniz olduğundan ve dağıtım işlemine başlamadan önce herhangi bir birim testi geçirir.

Sonraki bölümde, yeni bir derleme tanımı oluşturarak bu işlemi uygulamak açıklanmaktadır.

> [!NOTE]
> Bu yordamı&#x2014;, tek bir otomatik işlemi oluşturur, test ve bir çözüm dağıtır&#x2014;ortamları test etmek için dağıtım için en uygun olması olasıdır. Hazırlama ve üretim ortamları için içerik, önceden doğrulandı ve bir test ortamında doğrulanmış önceki derlemeden dağıtmak istediğiniz çok büyük olasılıkla. Bu yaklaşım sonraki konu olan açıklanan [belirli bir yapı dağıtma](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Bu yordamı kimin yapar?

Genellikle, bu yordamı TFS yönetici gerçekleştirir. Bazı durumlarda, bir geliştirici Ekip Lideri TFS'de takım projesi koleksiyonu için sorumluluk sürebilir. Yeni bir derleme tanımı oluşturmak için bir üyesi olmanız gerekir **proje koleksiyonu yapı yöneticileri** çözümünüzü içeren takım projesi koleksiyonu için Grup.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>CI ve dağıtım için bir yapı tanımı oluşturun

Sonraki yordam CI tetikleyen bir derleme tanımınız oluşturmayı açıklar. Yapı başarılı olursa, çözüm bir özel MSBuild proje dosyası mantığı kullanılarak dağıtılır.

**CI ve dağıtımı için derleme tanımını oluşturmak için**

1. Visual Studio 2010 içinde **Takım Gezgini** penceresinde, takım projesi düğümünü genişletin, sağ **derlemeler**ve ardından **yeni yapı tanımı**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Üzerinde **genel** sekmesinde, yapı tanımı bir ad verin (örneğin, **DeployToTest**) ve isteğe bağlı bir açıklama.
3. Üzerinde **tetikleyici** sekmesinde, yeni bir derlemeyi tetiklemeyi ölçütünü seçin. Örneğin, çözümü oluşturun ve yeni kodda bir geliştirici denetler her zaman test ortamını dağıtmak istiyorsanız, seçin **sürekli tümleştirme**.
4. Üzerinde **Yapı Varsayılanları** sekmesinde **kopyalama yapı çıktısını aşağıdaki bırakma klasörüne** açılan klasörünüze Evrensel Adlandırma Kuralı (UNC) yolunu yazın (örneğin,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Bu bırakma konumu yapılandırdığınız bekletme ilkesine bağlı olarak birkaç derlemeleri depolar. Belirli bir yapı dağıtım yapılardan hazırlık veya üretim ortamına yayınlama istediğinizde, bunları burada bulabilirsiniz budur.
5. Üzerinde **işlem** sekmesinde **derleme işlemi dosya** açılır listesinde, bırakın **DefaultTemplate.xaml** seçili. Bu, tüm yeni takım projeleri için eklenir varsayılan derleme işlem şablonları biridir.
6. İçinde **oluşturma işlemi parametreleri** tablo, tıklatın **yapı öğelerine** satır ve ardından **üç nokta** düğmesi.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. İçinde **yapı öğelerine** iletişim kutusu, tıklatın **Ekle**.
8. Çözüm dosyası konumuna göz atın ve ardından **Tamam**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. İçinde **yapı öğelerine** iletişim kutusu, tıklatın **Ekle**.
10. İçinde **türdeki öğeleri** açılır listesinden, **MSBuild proje dosyalarını**.
11. Hangi, dağıtım işlemini denetleyen, dosyayı seçin ve ardından özel Proje dosyasının konumuna göz atın **Tamam**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. **Yapı öğelerine** iletişim kutusu iki öğe artık gösterilmesi. **Tamam**'ı tıklatın.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Üzerinde **işlem** sekmesinde **oluşturma işlemi parametreleri** tablo, genişletin **Gelişmiş** bölümü.
14. İçinde **MSBuild bağımsız değişkenleri** satır, tüm MSBuild komut satırı bağımsız değişkenleri eklemek, *ya da* oluşturmak için öğelerin gerektirir. Contact Manager çözüm senaryosunda, bu bağımsız değişkenler gereklidir:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Bu örnekte:

    1. **DeployOnBuild = true** ve **DeployTarget paket =** bağımsız değişkenleri, kişinin Yöneticisi çözümü yapılandırdığınızda gereklidir. Bu web dağıtım paketleri her web uygulaması projesi oluşturduktan sonra açıklandığı gibi oluşturmak için MSBuild bildirir [bina ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. **TargetEnvPropsFile** bağımsız değişkeni, oluşturma sırasında gereklidir *Publish.proj* dosya. Bu özellik açıklandığı gibi ortama özgü yapılandırma dosyasının konumu gösterir [oluşturma işlemini anlama](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Üzerinde **Bekletme İlkesi** sekmesinde, gerekli olarak korumak istediğiniz her tür kaç derlemeler yapılandırın.
17. **Kaydet**'e tıklayın.

## <a name="queue-a-build"></a>Bir Derlemeyi Sıraya Koyma

Bu noktada, en az bir yeni derleme tanımı oluşturdunuz. Tanımlanan oluşturma işlemi artık yapı tanımında belirtilen Tetikleyicileri göre çalışır.

Yapı tanımınızı CI kullanmak için yapılandırdıysanız, iki yolla yapı tanımınızı test edebilirsiniz:

- Bazı içerikler için otomatik bir derlemeyi tetiklemek için takım projesi iade edin.
- El ile bir yapıyı sıraya.

**El ile bir yapıyı sıraya almak için**

1. İçinde **Takım Gezgini** penceresinde, yapı tanımına sağ tıklayın ve ardından **yeni yapıyı sıraya al**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. İçinde **Yapıyı Sıraya Al** iletişim kutusu, yapı özelliklerini gözden geçirin ve ardından **sıra**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

İlerleme durumunu ve bir yapı sonucunu gözden geçirmek için&#x2014;olup, el ile veya otomatik olarak tetiklendi bağımsız olarak&#x2014;derleme tanımı'nda çift **Takım Gezgini** penceresi. Bu açılır bir **Yapı Gezgini** sekmesi.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Buradan, başarısız derlemeleri giderebilirsiniz. Tek bir derleme çift tıklarsanız özet bilgileri görüntülemek ve aracılığıyla ayrıntılı günlük dosyalarına'ı tıklatın.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Başarısız derlemeler sorun giderme ve başka bir yapı çalışmadan önce sorunları çözmek için bu bilgileri kullanın.

> [!NOTE]
> Dağıtım mantığını yürütme derlemeleri büyük olasılıkla hedef ortamda gereken izinler yapı sunucunun verilen kadar başarısız olur. Daha fazla bilgi için bkz: [takım yapı dağıtımı için izinleri yapılandırma](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Derleme işlemi izleyin

TFS geniş bir yapı işlemi izlemenize yardımcı olmak için işlevsellik sağlar. Örneğin, TFS bir e-posta göndermek veya bir yapı tamamlandığında, görev çubuğu bildirim alanında uyarıları görüntüler. Daha fazla bilgi için bkz: [çalıştırma ve İzleyici derlemeler](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Sonuç

Bu konuda bir derleme tanımınız TFS'de oluşturma açıklanmaktadır. Her takım projesine içerik bir geliştirici denetim gerçekleştirildiğinde derleme işlemi çalıştırılan için derleme tanımını CI için yapılandırılır. Yapı tanımı web paketleri ve veritabanı komut dosyalarında bir hedef sunucu ortamı dağıtmak için özel bir MSBuild proje dosyası yürütür.

Derleme işleminin bir parçası başarılı olması bir otomatik dağıtım için sırayla yapı hizmeti hesabı hedef web sunucuları ve hedef veritabanı sunucusunda uygun izinleri gerekir. Bu öğretici, son konusunda [takım yapı dağıtımı için izinleri yapılandırma](configuring-permissions-for-team-build-deployment.md), tanımlamak ve bir ekip sunucusundan otomatik dağıtımı için gerekli izinleri yapılandırma açıklar.

## <a name="further-reading"></a>Daha Fazla Bilgi

Derleme tanımları oluşturma hakkında daha fazla bilgi için bkz: [temel yapı tanımı oluşturma](https://msdn.microsoft.com/library/ms181716.aspx) ve [bilgisayarınızı derleme işlemi tanımlamak](https://msdn.microsoft.com/library/ms181715.aspx). Sıraya alma yapılar hakkında daha fazla yönergeler için bkz [bir yapıyı sıraya](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Önceki](configuring-a-tfs-build-server-for-web-deployment.md)
> [sonraki](deploying-a-specific-build.md)
