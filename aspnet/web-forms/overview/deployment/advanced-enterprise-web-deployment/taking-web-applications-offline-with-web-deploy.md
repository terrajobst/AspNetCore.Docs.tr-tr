---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Web uygulamalarını çevrimdışı yapma Web ile dağıtma | Microsoft Docs
author: jrjlee
description: Bu konu, Internet Information Services (IIS) Web Dağı kullanarak bir otomatik dağıtım süre için çevrimdışı web uygulaması gerçekleştirilecek açıklar...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: b8dc1ff26bbbe7dee1ee90fd929b2e98de5360db
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829521"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Web uygulamalarını çevrimdışı yapma Web ile dağıtma
====================
tarafından [Jason Lee](https://github.com/jrjlee)

[PDF'yi indirin](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Bu konuda, bir web uygulaması çevrimdışı süre (Web dağıtımı) Internet Information Services (IIS) Web Dağıtım Aracı'nı kullanarak bir otomatik dağıtım için nasıl açıklar. Web uygulaması'na göz kullanıcılar için yönlendirilir bir *uygulama\_offline.htm* dağıtımı tamamlanana kadar dosya.


Bu konuda öğreticileri, Fabrikam, Inc. adlı kurgusal bir şirkete kurumsal dağıtım gereksinimleri bir dizi parçası oluşturur. Bu öğretici serisinin kullanan örnek bir çözüm&#x2014; [Kişi Yöneticisi çözümü](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;karmaşıklık bir ASP.NET MVC 3 uygulama, bir Windows iletişim dahil olmak üzere, gerçekçi bir düzeyi ile bir web uygulaması temsil etmek için Foundation (WCF) hizmet ve bir veritabanı projesi.

Bu öğreticileri temelini dağıtım yöntemi, açıklanan bölünmüş proje dosyası yaklaşım dayalı [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md), hangi yapı işlemi tarafından denetlenir içinde iki proje dosyaları&#x2014;içeren bir Her hedef ortam ve ortama özgü derleme ve dağıtım ayarları içeren bir geçerli yönergeleri oluşturun. Derleme sırasında ortama özgü proje dosyası derleme yönergeleri eksiksiz bir kümesini oluşturmak için ortam belirsiz proje dosyasına birleştirilir.

## <a name="task-overview"></a>Görev genel bakış

İçinde çok sayıda senaryo, bir web uygulaması veritabanları ya da web Hizmetleri gibi ilgili bileşenlerini değişiklik sırasında çevrimdışı duruma getirmek istersiniz. Genellikle, IIS ve ASP.NET, bunu adlı bir dosyaya yerleştirerek gerçekleştirirsiniz *uygulama\_offline.htm* IIS Web sitesi veya web uygulaması kök klasöründe. *Uygulama\_offline.htm* dosya standart bir HTML dosyası ve genellikle kullanıcı site bakım nedeniyle geçici olarak devre dışı olduğunu bildiren basit bir ileti içerir. Sırada *uygulama\_offline.htm* dosyası var Web sitesinin kök klasöründe, IIS tüm istekleri otomatik olarak dosyaya yönlendirir. Güncelleştirmeleri yapmayı bitirdiğinizde, kaldırdığınız *uygulama\_offline.htm* dosyası ve Web sitesi modundan zamanki isteklerine hizmet.

Bir hedef ortam için otomatik olarak veya tek adımlı dağıtımları gerçekleştirmek için Web dağıtımı kullandığınızda, ekleme ve kaldırma birleştirmek isteyebilirsiniz *uygulama\_offline.htm* dağıtım işleminizi dosyasına. Bunu yapmak için bu üst düzey görevleri tamamlamanız gerekir:

- Microsoft Build Engine (MSBuild) proje dosyasında dağıtım işlemini denetlemek için MSBuild hedefi oluşturan kullanmanızı kopyalar bir *uygulama\_offline.htm* dosyasını hedef sunucuya önce herhangi bir dağıtım görevi başlayın.
- Kaldırır başka bir MSBuild hedefi eklemek *uygulama\_offline.htm* dosya tüm dağıtım görevlerini tamamlandığı zaman hedef sunucudan.
- Web uygulama projesinde oluşturmak bir *. wpp.targets* sağlar dosya bir *uygulama\_offline.htm* dosya, Web dağıtımı çağrıldığında Dağıtım paketine eklenir.

Bu konuda, bu yordamları gerçekleştirmek nasıl gösterilmektedir. Görevleri ve bu konudaki yönergeler, en az bir web uygulaması projesi içeren bir çözüm zaten oluşturdunuz ve açıklandığı gibi dağıtım işlemini denetlemek için özel proje dosyasını kullanmanız varsayılır [, Web dağıtımı Kurumsal](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Alternatif olarak, [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnekleme konusunda örnekleri izlemek için çözüm.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Bir uygulamayı ekledikten\_bir Web uygulaması projesi için çevrimdışı dosya

Eklenecek ilk görevi tamamlamak için ihtiyacınız olan bir *uygulama\_çevrimdışı* web uygulaması projenize dosyasına:

- Dosya geliştirme sürecinin engellemesini önlemek amacıyla (uygulamanızın kalıcı olarak çevrimdışı olmasını istemiyorsanız), bunu bir şey dışında çağırmalısınız *uygulama\_offline.htm*. Örneğin, dosyayı adlandırabilirsiniz *uygulama\_template.htm çevrimdışı*.
- Dosya olarak dağıtılan önlemek için-olduğu derleme eylemi ayarlamalıdır **hiçbiri**.

**Uygulama ekleme\_bir web uygulaması projesi için çevrimdışı dosya**

1. Visual Studio 2010'da çözümünüzü açın.
2. İçinde **Çözüm Gezgini** penceresinde web uygulaması projenize sağ tıklayın, fareyle **Ekle**ve ardından **yeni öğe**.
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **HTML sayfası**.
4. İçinde **adı** kutusuna **uygulama\_template.htm çevrimdışı**ve ardından **Ekle**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Kullanıcılar uygulamanın kullanılabilir olduğunu bildirmek için bazı basit bir HTML ekleyin ve ardından dosyayı kaydedin. Herhangi bir sunucu tarafı etiket içermez (örneğin, tüm ön eki etiketler "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. İçinde **Çözüm Gezgini** penceresinde, yeni dosyaya sağ tıklayın ve ardından **özellikleri**.
7. İçinde **özellikleri** penceresi, **derleme eylemi** satır, select **hiçbiri**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Dağıtma ve uygulama silme\_çevrimdışı dosya

Sonraki adım, dosyayı dağıtım işleminin başlangıcında hedef sunucuya kopyalayın ve sonunda kaldırmak için dağıtım mantığınızı değiştirmektir.

> [!NOTE]
> Sonraki yordamda, özel bir MSBuild proje dosyası, dağıtım işlemini denetlemek için açıklandığı gibi kullandığınız varsayılır [proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Doğrudan Visual Studio'dan dağıtıyorsanız, farklı bir yaklaşım kullanmanız gerekir. Bu tür bir yaklaşım sayed Ibrahim Hashimi açıklar [nasıl ele uygulamanızın Web uygulamasını çevrimdışı sırasında yayımlama](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Dağıtmak için bir *uygulama\_çevrimdışı* dosya MSDeploy.exe kullanarak çağırmak gereken bir hedef IIS Web sitesine [Web dağıtımı **contentPath** sağlayıcısı](https://technet.microsoft.com/library/dd569034(WS.10).aspx). **ContentPath** sağlayıcının desteklediği hem fiziksel dizin yolları hem de IIS Web sitenize veya uygulamanıza yolları, Visual Studio Proje klasörü ve IIS web uygulaması arasında bir dosya eşitlemek için ideal seçim kolaylaştırır. Dosyayı dağıtmak için MSDeploy komutunuz şuna benzemelidir:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Hedef site dağıtım işleminin sonunda dosyayı kaldırmak için MSDeploy komutunuz şuna benzemelidir:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Derleme ve dağıtım işleminin bir parçası bu komutları otomatikleştirmek için bunları özel MSBuild proje dosyanıza tümleştirmek gerekir. Sonraki yordam bunun nasıl yapılacağı açıklanır.

**Dağıtma ve uygulama silme\_çevrimdışı dosya**

1. Visual Studio 2010'da, dağıtım işlemini denetleyen MSBuild proje dosyasını açın. İçinde [Kişi Yöneticisi](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) örnek çözüm budur *Publish.proj* dosya.
2. Kök **proje** öğesi, yeni bir **PropertyGroup** depolamak için değişkenleri için öğe *uygulama\_çevrimdışı* dağıtım:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. **SourceRoot** özelliği içinde tanımlandığından, başka bir yerde *Publish.proj* dosya. Geçerli göreli kaynak içerik için kök klasör konumunu gösteren&#x2014;başka bir deyişle, göreli konumunu *Publish.proj* dosya.
4. **ContentPath** sağlayıcısı değil kabul edeceği göreli dosya yolları, kaynak dosyanız için mutlak bir yol onu dağıtabilmek için ihtiyaç duyduğunuz şekilde. Kullanabileceğiniz [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) Bunu yapmak için görev.
5. Yeni bir **hedef** adlı bir öğe **GetAppOfflineAbsolutePath**. Bu hedef içinde kullanmak **ConvertToAbsolutePath** mutlak yolunu almak için görev *uygulama\_şablon çevrimdışı* proje klasörünüzdeki dosya.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Bu hedef için göreli yol alan *uygulama\_şablon çevrimdışı* proje klasörünüzdeki dosya ve yeni bir özelliği mutlak bir dosya yolu olarak kaydeder. **BeforeTargets** özniteliği belirtir, bu hedeften önce yürütülecek istediğiniz **DeployAppOffline** sonraki adımda oluşturacağınız hedefi.
7. Adlı yeni bir hedef ekleyin **DeployAppOffline**. Bu hedef içinde dağıtan MSDeploy.exe komutu çağırabilir, *uygulama\_çevrimdışı* hedef web sunucusuna dosya.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Bu örnekte, **ContactManagerIisPath** özelliği başka bir proje dosyasında tanımlanır. Bu, yalnızca bir IIS uygulama yolu biçiminde *[IIS Web sitesi adı] / [uygulama adı]*. Hedef koşul dahil olmak üzere, kullanıcıların geçiş sağlar *uygulama\_çevrimdışı* özellik değerini değiştirme veya komut satırı parametresi sağlayarak dağıtım veya kapat.
9. Adlı yeni bir hedef ekleyin **DeleteAppOffline**. Bu hedef içinde kaldıran MSDeploy.exe komutu çağırabilir, *uygulama\_çevrimdışı* hedef web sunucusundan dosya.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. Son uygun noktalarda yeni bu hedefleri proje dosyanız yürütülmesi sırasında çağrılacak bir görevdir. Çeşitli yollarla bunu yapabilirsiniz. Örneğin, *Publish.proj* dosyası **FullPublishDependsOn** özellik yürütülmelidir hedeflerin listesi belirtir, sipariş ne zaman **FullPublish** varsayılan Hedef çağrılır.
11. Çağırmak için MSBuild proje dosyasını değiştirmek **DeployAppOffline** ve **DeleteAppOffline** yayımlama işlemi uygun noktalarında hedefler.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Özel MSBuild proje dosyası çalıştırdığınızda *uygulama\_çevrimdışı* dosyanın dağıtılacağı sunucuya hemen sonra başarılı bir derleme. Tüm dağıtım görevlerini tamamlandıktan sonra ardından sunucudan silinir.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Bir uygulamayı ekledikten\_dağıtım paketleri için çevrimdışı dosya

Dağıtımınızı nasıl yapılandırdığınıza bağlı olarak tüm IIS hedefte içerik mevcut web uygulaması&#x2014;gibi *uygulama\_offline.htm* dosya&#x2014;bir web dağıtımı otomatik olarak silinebilir hedefe paket. Emin olmak için *uygulama\_offline.htm* dosya dağıtım süresince yerde kalır, dosyanın doğrudan başlangıcında dağıtmak için web dağıtımı paketi kendi dosyasında ayrıca eklemeniz gerekir dağıtım işlemi.

- Bu konudaki önceki görevleri, uyguladıysanız, eklediğiniz *uygulama\_offline.htm* web uygulaması projenize farklı bir dosya adı altında bir dosyaya (kullandık *uygulama\_ Çevrimdışı template.htm*) ve derleme eylemi ayarlayın **hiçbiri**. Bu değişiklikler, engellemesini ile geliştirme ve hata ayıklama dosyadan önlemek gereklidir. Sonuç olarak, emin olmak için paketleme işlemi özelleştirmek ihtiyacınız *uygulama\_offline.htm* dosya web dağıtım paketi içinde yer almaktadır.

Web yayımlama işlem hattı (WPP) adlı bir öğe listesi kullanan **FilesForPackagingFromProject** web dağıtım paketinin dahil edilmesi gereken dosyaların listesini oluşturmak için. Bu listeye kendi öğeleri ekleyerek, web paketlerinizi içeriğini özelleştirebilirsiniz. Bunu yapmak için üst düzey adımları tamamlamanız gerekir:

1. Adlı bir özel proje dosyası oluşturmayı *[Proje adı].wpp.targets* , proje dosyası ile aynı klasörde.

    > [!NOTE]
    > *. Wpp.targets* dosyasına gereken web uygulaması proje dosyanız ile aynı klasörde Git&#x2014;gibi *ContactManager.Mvc.csproj*&#x2014;yerine aynı klasöre herhangi bir özel Proje dosyaları derleme ve dağıtım işlemini denetlemek için kullanın.
2. İçinde *. wpp.targets* dosya, yürüten yeni bir MSBuild hedefi oluşturma *önce* **CopyAllFilesToSingleFolderForPackage** hedef. Bu paket içerisine dâhil edilecek noktalar listesini oluşturan WPP hedefidir.
3. Yeni hedef oluşturma bir **ItemGroup** öğesi.
4. İçinde **ItemGroup** öğe, Ekle bir **FilesForPackagingFromProject** öğesini ve belirtin *uygulama\_offline.htm* dosya.

*. Wpp.targets* dosya bu benzemesi gerekir:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Bu örnekte Not, önemli noktaları şunlardır:

- **BeforeTargets** öznitelik ekler hemen önce yapılmalıdır WPP belirterek bu hedefe **CopyAllFilesToSingleFolderForPackage** hedef.
- **FilesForPackagingFromProject** öğesinin kullandığı **DestinationRelativePath** dosyayı yeniden adlandırmak için meta veri değeri *uygulama\_template.htm çevrimdışı* için *uygulama\_offline.htm* listesine eklenir.

Sonraki yordamda bu ekleme işlemi açıklanır *. wpp.targets* dosyasına bir web uygulaması projesi.

**Eklemek için bir. web dağıtım paketi wpp.targets dosyasına**

1. Visual Studio 2010'da çözümünüzü açın.
2. İçinde **Çözüm Gezgini** penceresinde, web uygulaması proje düğümüne sağ tıklayın (örneğin, **ContactManager.Mvc**), işaret **Ekle**ve ardından**Yeni öğe**.
3. İçinde **Yeni Öğe Ekle** iletişim kutusunda **XML dosyası** şablonu.
4. İçinde **adı** kutusuna *[Proje adı] ***.wpp.targets** (örneğin, **ContactManager.Mvc.wpp.targets**) ve ardından **Ekle**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Bir proje kök düğümü için yeni bir öğe eklerseniz, dosyayı proje dosyası ile aynı klasörde oluşturulur. Bu, Windows Gezgini'nde klasörü açarak doğrulayabilirsiniz.
5. Daha önce açıklanan MSBuild biçimlendirme dosyasına ekleyin.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Kaydet ve Kapat *[Proje adı].wpp.targets* dosya.

Sonraki kez, derleme ve paketleme, web uygulaması projenizin WPP otomatik olarak algılar *. wpp.targets* dosya. *Uygulama\_template.htm çevrimdışı* dosyası dahil edilecek elde edilen web dağıtım paketi *uygulama\_offline.htm*.

> [!NOTE]
> Dağıtım başarısız olursa *uygulama\_offline.htm* dosya, yerinde kalmaya ve uygulamanız çevrimdışı kalır. Bu genellikle istenen davranıştır. Uygulamanızı getirmek için yeniden çevrimiçi silebilirsiniz *uygulama\_offline.htm* web sunucusundan dosya. Alternatif olarak, varsa hataları düzeltin ve başarılı bir dağıtım çalıştırma *uygulama\_offline.htm* dosyası kaldırılacak.


## <a name="conclusion"></a>Sonuç

Bu konuda açıklanan yayımlayarak bir dağıtım süre için çevrimdışı web uygulaması nasıl bir *uygulama\_offline.htm* dosyasını hedef sunucuya dağıtım işleminin başlangıcında ve adresinden kaldırılıyor Bitiş Olayı. Aynı zamanda ekleme kapsamında bir *uygulama\_offline.htm* web dağıtım paketi dosyasında.

## <a name="further-reading"></a>Daha Fazla Bilgi

Paketleme ve dağıtım işlemi hakkında daha fazla bilgi için bkz. [oluşturma ve paketleme Web Uygulama projeleri](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Web paketi dağıtımı için yapılandırma parametreleri](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), ve [ Web paketleri dağıtma](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Yerine web uygulamalarınızı doğrudan Visual Studio'dan yayımlama, bu öğreticilerde açıklanan özel MSBuild proje dosyası yaklaşımı kullanarak uygulama yayımlama sırasında çevrimdışı olması için biraz daha farklı bir yaklaşım kullanmanız gerekecektir işlem. Daha fazla bilgi için [yayımlama sırasında çevrimdışı web uygulamanızı nasıl](https://go.microsoft.com/?linkid=9805135) (blog gönderisi).

> [!div class="step-by-step"]
> [Önceki](excluding-files-and-folders-from-deployment.md)
> [İleri](running-windows-powershell-scripts-from-msbuild-project-files.md)
