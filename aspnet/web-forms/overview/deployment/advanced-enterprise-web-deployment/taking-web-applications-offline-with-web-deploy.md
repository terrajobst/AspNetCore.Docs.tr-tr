---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: "Alma Web çevrimdışı ile Web uygulamalarını dağıtma | Microsoft Docs"
author: jrjlee
description: "Bu konu, Internet Information Services (IIS) Web Dağı kullanarak bir otomatik dağıtım süre için çevrimdışı web uygulaması yapılacak açıklar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1c262ec7b834107524a18c6552b171f731452c91
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/15/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Alma Web çevrimdışı ile Web uygulamalarını dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, bir web uygulaması çevrimdışı için Internet Information Services (IIS) Web Dağıtım Aracı (Web dağıtımı) kullanarak bir otomatik dağıtım süresini yapılacak açıklar. Web uygulaması'na göz kullanıcılar için yönlendirilir bir *uygulama\_offline.htm* dağıtım işlemi tamamlanana kadar dosya.


Bu konuda eğitim serileri Fabrikam Ltd. adlı kurgusal bir şirket kurumsal dağıtım gereksinimleri dayalı parçası formlar Bu öğretici serisi örnek çözümü & #x 2014; kullanır [Contact Manager çözüm](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& Windows bir ASP.NET MVC 3 uygulama da dahil olmak üzere karmaşıklıkta gerçekçi düzeyine sahip bir web uygulamasını temsil eden #x 2014; Communication Foundation (WCF) hizmetini ve veritabanı projesi.

Bu öğreticileri merkezinde dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi derleme süreci tarafından denetlenen içinde iki dosyaları & #x 2014; proje bir içeren Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir için geçerli olan yönergeleri oluşturun. Derleme zamanında ortama özgü proje dosyası oluşturma yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

İçinde çok sayıda senaryoları, veritabanları veya web Hizmetleri gibi ilgili bileşenler için değişiklikler yaparken bir web uygulaması çevrimdışına istersiniz. Genellikle, IIS ve ASP.NET, bunu adlı bir dosya koyarak gerçekleştirirsiniz *uygulama\_offline.htm* IIS Web sitesi veya web uygulamasının kök klasöründe. *Uygulama\_offline.htm* dosya standart bir HTML dosyası ve genellikle kullanıcı site bakım nedeniyle geçici olarak devre dışı olduğunu bildiren basit bir ileti içerir. Sırada *uygulama\_offline.htm* dosya var Web sitesinin kök klasöründe, IIS otomatik olarak yeniden yönlendirme tüm istekler için dosya. Güncelleştirmeleri yapma tamamladığınızda kaldırdığınız *uygulama\_offline.htm* dosyası ve Web sitesini devam ettirir istekleri her zamanki gibi hizmet veren.

Hedef ortam için otomatik olarak veya tek adımlı dağıtımları gerçekleştirmek için Web dağıtımı kullandığınızda, ekleme ve kaldırma dahil isteyebilirsiniz *uygulama\_offline.htm* dağıtım işleminiz dosyasına. Bunu yapmak için bu üst düzey görevleri tamamlamanız gerekir:

- Microsoft Build Engine (MSBuild) proje dosyasında dağıtım işlemini denetleyen bir MSBuild hedef oluşturmak için kullanmanız kopyalar bir *uygulama\_offline.htm* dosyasını hedef sunucuya önce tüm dağıtım görevleri başlayın.
- Kaldırır başka bir MSBuild hedef ekleme *uygulama\_offline.htm* tüm dağıtım görevler tamamlandığında hedef sunucudan dosya.
- Web uygulaması projesi oluşturduğunuzda bir *. wpp.targets* sağlar dosya bir *uygulama\_offline.htm* Web dağıtımı çağrıldığında dosya Dağıtım paketine eklenir.

Bu konuda bu yordamları gerçekleştirmek nasıl yapacağınızı gösterir. Görevleri ve bu konudaki yönergeler en az bir web uygulaması projesi içeren bir çözüm önceden oluşturduğunuz ve dağıtım işlemini açıklandığı gibi denetlemek için özel proje dosyasını kullanmanız varsayılır [Web dağıtımı Kurumsal](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternatif olarak, kullanabileceğiniz [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü konudaki örnekler izleyin.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Bir uygulama ekleme\_bir Web uygulaması projesi için çevrimdışı dosya

Tamamlamanız gereken ilk görev eklemek için bir *uygulama\_çevrimdışı* web uygulaması projenize dosyası:

- Dosya geliştirme işlemine engellemesini önlemek amacıyla (uygulamanızın kalıcı olarak çevrimdışı olmasını istemiyorsanız), onu bir şey dışında çağırmalısınız *uygulama\_offline.htm*. Örneğin, dosyayı adlandırabilirsiniz *uygulama\_template.htm çevrimdışı*.
- Dosya olarak dağıtılan önlemek için-olduğu yapı eylemi ayarlamalıdır **hiçbiri**.

**Bir uygulama eklemek için\_bir web uygulaması projesi için çevrimdışı dosya**

1. Çözümünüzü Visual Studio 2010'da açın.
2. İçinde **Çözüm Gezgini** penceresinde, web uygulaması projenize sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **HTML sayfası**.
4. İçinde **adı** kutusuna **uygulama\_template.htm çevrimdışı**ve ardından **Ekle**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Kullanıcıların uygulama kullanılamaz olduğunu bildirmek üzere bazı basit HTML ekleyin ve ardından dosyayı kaydedin. Herhangi bir sunucu tarafı etiket içermez (örneğin, ile önek tüm etiketleri "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. İçinde **Çözüm Gezgini** penceresinde, yeni dosyaya sağ tıklayın ve ardından **özellikleri**.
7. İçinde **özellikleri** penceresi, **yapı eylemi** satır, select **hiçbiri**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Dağıtma ve uygulama silme\_çevrimdışı dosya

Sonraki adım, dosyayı dağıtım işlemi başlangıcında hedef sunucuya kopyalayın ve sonunda kaldırmak için dağıtım mantığınızı değiştirmektir.

> [!NOTE]
> Sonraki yordam özel MSBuild proje dosyası, dağıtım işleminiz denetlemek için açıklandığı gibi kullandığınız olduğunu varsayar [proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Visual Studio'dan doğrudan dağıtıyorsanız, farklı bir yaklaşım kullanmanız gerekir. Bu tür bir yaklaşım sayed Ibrahim Hashimi açıklar [nasıl ele bilgisayarınızı Web uygulaması çevrimdışı sırasında yayımlama](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Dağıtmak için bir *uygulama\_çevrimdışı* dosya MSDeploy.exe kullanarak çağırmak gereken bir hedef IIS Web sitesini [Web dağıtımı **contentPath** sağlayıcı](https://technet.microsoft.com/library/dd569034(WS.10).aspx). **ContentPath** sağlayıcının desteklediği fiziksel dizin yolu ve IIS Web sitesi veya uygulama yolu, Visual Studio Proje klasörünü ve bir IIS web uygulaması arasında bir dosya eşitleme için ideal seçim kolaylaşır. Dosyayı dağıtmak için MSDeploy komutunuzu şuna benzemelidir:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Dağıtım işlemi sonunda hedef sitesinden dosyayı kaldırmak için MSDeploy komutunuzu şuna benzemelidir:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Derleme ve dağıtım işleminin bir parçası bu komutları otomatikleştirmek için özel MSBuild proje dosyanıza tümleştirmeniz gerekir. Sonraki yordam bunun nasıl yapılacağını açıklar.

**Dağıtma ve bir uygulamayı silmek için\_çevrimdışı dosya**

1. Visual Studio 2010'da, dağıtım işleminiz denetimleri MSBuild proje dosyasını açın. İçinde [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözümü budur *Publish.proj* dosya.
2. Kök **proje** öğesi, yeni bir **PropertyGroup** değişkenleri depolamak için öğesi *uygulama\_çevrimdışı* dağıtım:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot** özelliği tanımlanmış başka bir yerde *Publish.proj* dosya. Geçerli yolda & #x 2014 göre; diğer bir deyişle, göreli konumunu kaynak içerik için kök klasör konumunu belirten *Publish.proj* dosya.
4. **ContentPath** sağlayıcısı değil kabul edeceği göreli dosya yolları dağıtabilmeniz için önce bir mutlak yol kaynak dosyanıza almanız gereken şekilde. Kullanabileceğiniz [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) Bunu yapmak için görev.
5. Yeni bir ekleme **hedef** adlı öğe **GetAppOfflineAbsolutePath**. Bu hedef içinde kullanmak **ConvertToAbsolutePath** mutlak bir yol almak için görev *uygulama\_şablonu çevrimdışı* proje klasörünüzdeki dosya.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Bu hedef için göreli yol alır *uygulama\_şablonu çevrimdışı* proje klasörünüzdeki dosya ve yeni bir özellik mutlak bir dosya yolu olarak kaydeder. **BeforeTargets** özniteliği belirtir. Bu hedef önce yürütülecek istediğiniz **DeployAppOffline** sonraki adımda oluşturacağınız hedef.
7. Adlı yeni bir hedef ekleme **DeployAppOffline**. Bu hedef içinde dağıtır MSDeploy.exe komut çağırma, *uygulama\_çevrimdışı* hedef web sunucusuna dosya.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Bu örnekte, **ContactManagerIisPath** özelliği başka bir yerde proje dosyasında tanımlanır. Bu yalnızca bir IIS uygulama yolu, biçiminde olduğunu *[IIS Web sitesi adı] / [uygulama adı]*. Hedef bir koşul da dahil olmak üzere, geçiş yapmak kullanıcıların sağlar *uygulama\_çevrimdışı* bir özellik değerini değiştirme veya komut satırı parametresi sağlanarak dağıtım açıp kapatma.
9. Adlı yeni bir hedef ekleme **DeleteAppOffline**. Bu hedef içinde kaldırır MSDeploy.exe komut çağırma, *uygulama\_çevrimdışı* hedef web sunucusundan dosya.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Son Proje dosyanızı yürütme sırasında bu yeni hedeflere uygun noktalarda çağrılacak bir görevdir. Çeşitli yollarla bunu yapabilirsiniz. Örneğin, *Publish.proj* dosyası **FullPublishDependsOn** özelliği yürütülmelidir hedefleri listesini belirtir, sipariş ne zaman **FullPublish** varsayılan Hedef çağrılır.
11. Çağrılacak MSBuild proje dosyanızı değiştirmek **DeployAppOffline** ve **DeleteAppOffline** yayımlama işlemi uygun noktalarında hedefler.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Özel, MSBuild proje dosyası çalıştırdığınızda *uygulama\_çevrimdışı* dosya dağıtılacak sunucuya hemen başarılı derleme sonrası. Tüm dağıtım görevleri tamamlandıktan sonra sunucudan sonra silinir.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Bir uygulama ekleme\_dağıtım paketleri için çevrimdışı dosya

Nasıl dağıtımınız, varolan içeriğin hedef IIS web uygulamasını & #x 2014; yapılandırdığınıza bağlı olarak ister *uygulama\_offline.htm* dosyası & #x 2014; bir web dağıtımı otomatik olarak silinebilir paketi hedef. Emin olmak için *uygulama\_offline.htm* dosyası için dağıtım süresini yerde kalır, dosyanın doğrudan başlangıcında dağıtmak için web dağıtım paketi kendisini dosyasında ayrıca eklemeniz gerekir dağıtım işlemi.

- Bu konuda önceki görevleri varsa uyguladıysanız, eklediğiniz *uygulama\_offline.htm* dosyasını farklı bir dosya adı altında web uygulaması projenize (kullandık *uygulama\_ template.htm çevrimdışı*) ve yapı eylemi ayarladığınız **hiçbiri**. Bu değişiklikler ile geliştirme çakışarak ve hata ayıklama dosyadan önlemek gereklidir. Sonuç olarak, emin olmak için paketleme işlemi özelleştirmek ihtiyacınız *uygulama\_offline.htm* dosyası web dağıtım paketinde yer almaktadır.

Web yayımlama ardışık düzen (WPP) adlı bir öğe listesi kullanan **FilesForPackagingFromProject** web dağıtım paketi eklenmesi gereken dosyaların bir listesini oluşturmak için. Bu listeye kendi öğeleri ekleyerek web paketlerinizi içeriğini özelleştirebilirsiniz. Bunu yapmak için üst düzey adımları tamamlamanız gerekir:

1. Adlı bir özel proje dosyası oluşturma *[Proje adı].wpp.targets* proje dosyası ile aynı klasörde.

    > [!NOTE]
    > *. Wpp.targets* dosya gereken web uygulaması proje dosyası & #x 2014; aynı klasöre gitmek Örneğin, *ContactManager.Mvc.csproj*& tüm #x 2014; yerine aynı klasörü Özel proje dosyalarını denetimi derleme ve dağıtım işlemini kullanın.
2. İçinde *. wpp.targets* dosya, yürütür yeni bir MSBuild hedef oluşturmak *önce* **CopyAllFilesToSingleFolderForPackage** hedef. Bu pakete dahil edilecek noktalar listesini oluşturan WPP hedefidir.
3. Yeni hedef oluşturma bir **ItemGroup** öğesi.
4. İçinde **ItemGroup** öğesi ekleme bir **FilesForPackagingFromProject** öğesi ve belirtin *uygulama\_offline.htm* dosya.

*. Wpp.targets* dosyası bu benzer:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Bunlar, bu örnekte Not, önemli noktalar şunlardır:

- **BeforeTargets** öznitelik ekler hemen önce yapılmalıdır belirterek WPP bu hedefe **CopyAllFilesToSingleFolderForPackage** hedef.
- **FilesForPackagingFromProject** öğesi kullanır **DestinationRelativePath** dosyayı yeniden adlandırmak için meta veri değeri *uygulama\_template.htm çevrimdışı* için *uygulama\_offline.htm* gibi listesine eklenir.

Sonraki yordamda bu eklemek gösterilmiştir *. wpp.targets* bir web uygulaması projesi dosyasına.

**Eklemek için bir. web dağıtım paketi wpp.targets dosyasına**

1. Çözümünüzü Visual Studio 2010'da açın.
2. İçinde **Çözüm Gezgini** penceresinde, web uygulama projesi düğümüne sağ tıklayın (örneğin, **ContactManager.Mvc**), işaret **Ekle**ve ardından **Yeni öğe**.
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **XML dosyası** şablonu.
4. İçinde **adı** kutusuna *[Proje adı] ***.wpp.targets** (örneğin, **ContactManager.Mvc.wpp.targets**) ve ardından **Ekle**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Yeni bir öğe bir proje kök düğüme eklerseniz, dosya proje dosyası ile aynı klasörde oluşturulur. Bu klasörü Windows Gezgini'nde açarak doğrulayabilirsiniz.
5. Dosyasında daha önce açıklanan MSBuild biçimlendirme ekleyin.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Kaydet ve Kapat *[Proje adı].wpp.targets* dosya.

Sonraki başlatışınızda yapı ve paket, web uygulama projesi, WPP otomatik olarak algılar *. wpp.targets* dosya. *Uygulama\_template.htm çevrimdışı* dosya dahil edilir elde edilen web dağıtım paketi *uygulama\_offline.htm*.

> [!NOTE]
> Dağıtımınızı başarısız olursa, *uygulama\_offline.htm* dosya yerde kalır ve uygulamanızı çevrimdışı olarak kalır. Genellikle istenen davranış budur. Uygulamanızı getirmek için yeniden çevrimiçi silebilirsiniz *uygulama\_offline.htm* web sunucusundan dosya. Alternatif olarak, hataları düzeltin ve başarılı bir dağıtım çalıştırırsanız *uygulama\_offline.htm* dosyası kaldırılacak.


## <a name="conclusion"></a>Sonuç

Bu konuda açıklanan yayımlayarak bir dağıtım süre için çevrimdışı web uygulamasını nasıl bir *uygulama\_offline.htm* dosya dağıtım işleminin başında hedef sunucuya yükleyip adresindeki kaldırma Son. Ayrıca nasıl dahil edileceği kapsanan bir *uygulama\_offline.htm* web dağıtım paketi dosyasında.

## <a name="further-reading"></a>Daha Fazla Bilgi

Paketleme ve dağıtım işlemi hakkında daha fazla bilgi için bkz: [bina ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Web paketi dağıtımı için yapılandırma parametreleri](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), ve [ Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Yerine web uygulamalarınızın doğrudan Visual Studio'dan yayımlamak varsa bu öğreticileri açıklanan özel MSBuild proje dosyası yaklaşımı kullanarak, uygulamanızı çevrimdışı yayımlama sırasında olması için biraz farklı bir yaklaşım kullanmanız gerekecektir işlem. Daha fazla bilgi için bkz: [yayımlama sırasında çevrimdışı web uygulamanızı nasıl](https://go.microsoft.com/?linkid=9805135) (blog yayını).

>[!div class="step-by-step"]
[Önceki](excluding-files-and-folders-from-deployment.md)
[sonraki](running-windows-powershell-scripts-from-msbuild-project-files.md)
