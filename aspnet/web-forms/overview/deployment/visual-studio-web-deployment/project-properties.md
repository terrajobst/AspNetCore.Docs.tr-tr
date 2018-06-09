---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Proje özelliklerini | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: fba3f089bf1693eec873b08b4bc50e3accba06ee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30879887"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Proje Özellikleri
====================
Tarafından [zel Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bazı dağıtım seçenekleri proje dosyasında depolanan proje özellikleri'nde yapılandırılır ( *.csproj* veya *.vbproj* dosyası). Çoğu durumda, bu ayarlar varsayılan değerlerini istediğinizi olsa da kullanabilirsiniz **proje özelliklerini** UI bunları değiştirmeniz gerekiyorsa bu ayarlarla çalışmak için Visual Studio'ya yerleşik. Bu öğreticide, dağıtım ayarlarını gözden **proje özelliklerini**. Ayrıca dağıtılması boş bir klasöre neden olan bir yer tutucu dosyasını oluşturursunuz.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Proje Özellikleri penceresinde dağıtım ayarlarını yapılandırma

Aşağıdaki öğreticilerde göreceğiniz gibi dağıtımı sırasında neler etkileyen çoğu ayarlar yayımlama profilinde dahil edilir. Bilmeniz gereken birkaç ayarları içinde bulunur **Paketle/Yayımla** sekmelerinde **proje özelliklerini** penceresi. Bu ayarların her yapı yapılandırması için belirtilen — diğer bir deyişle, için hata ayıklama derlemesi'den sürüm yapı için farklı ayarlara sahip olabilir.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje, select **özellikleri**ve ardından **Paketle/Yayımla Web**sekmesi.

![Paketle/Yayımla Web sekmesi](project-properties/_static/image1.png)

Pencere görüntülendiğinde, hesaptaki yapı yapılandırması çözüm için şu anda etkin olan ayarları göstermek için varsayılan olarak. Varsa **yapılandırma** kutusunu belirtmez **etkin (sürüm)** seçin **sürüm** sürüm yapı yapılandırma ayarlarını görüntülemek için. Yayın derlemeleri, test ve üretim ortamları için dağıtacaksınız.

![Yayın derleme yapılandırması seçme](project-properties/_static/image2.png)

İle **etkin (sürüm)** veya **sürüm** seçili, yayın yapı yapılandırması'nı kullanarak dağıttığınızda etkili değerleri bakın:

- İçinde **dağıtmak için öğeleri** kutusunda **yalnızca uygulamayı çalıştırmak için gerekli dosyaları** seçilir. Diğer Seçenekler **bu projedeki tüm dosyalar** veya **bu proje klasöründeki tüm dosyalar**. Değişmeden varsayılan seçimi bırakarak kaynak kodu dosyaları, örneğin dağıtma kaçının. Bu ayar, SQL Server Compact ikili dosyaları içeren klasörlerin neden projeye dahil gerekiyordu nedenidir. Bu ayar hakkında daha fazla bilgi için bkz: **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx).
- **Oluşturulan dışlama hata ayıklama simgeleri** seçilir. Bu yapı yapılandırmayı kullandığınızda, hata ayıklama olmaz.
- **SQL Paketle/Yayımla sekmesinde yapılandırılmış tüm veritabanları dahil** seçilir. Visual Studio dosyaların yanı sıra veritabanlarını dağıtmak olup olmadığını belirtir. Onay kutusu etiket rağmen yalnızca belirtilenlerden **SQL Paketle/Yayımla** sekmesinde bu onay kutusunun temizlenmesi da devre dışı yayımlama profilinde yapılandırılan veritabanı dağıtımı. Onay kutusunun seçili kalması gereken şekilde, daha sonra bunu. **SQL Paketle/Yayımla** sekmesi, bu öğreticileri kullanarak olmaz yöntemi yayımlama eski bir veritabanı için kullanılır.
- **Web dağıtım paketi ayarları** tek tıklatmayla kullandığımızdan bölümü geçerli olmayan bu öğreticileri yayımlayın.

Değişiklik **yapılandırma** hata ayıklama derlemeleri için varsayılan ayarları görmek için hata ayıklama açılan kutusu. Aynı dışında değerlerdir **dışlama oluşturulan hata ayıklama simgeleri** böylece hata ayıklama derlemesi dağıttığınızda ayıklayabilirsiniz temizlenir.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Elmah klasörü dağıtılan emin olun

Önceki öğreticide gördüğünüz gibi [Elmah NuGet paketi](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) için hata günlüğünü ve raporlama işlevselliği sağlar. Contoso University uygulamada Elmah hata ayrıntıları adlı bir klasörde depolamak için yapılandırılmış *Elmah*:

![Elmah klasörü](project-properties/_static/image3.png)

Dağıtımdan belirli dosyaları veya klasörleri hariç ortak bir gereksinimdir; başka bir örnek, kullanıcılar dosyaları karşıya yükleyebilir bir klasör olabilir. Günlük dosyaları istemiyorsanız veya üretim dağıtılması için geliştirme ortamınızı oluşturulan dosyalarını karşıya. Ve üretim için bir güncelleştirme dağıtıyorsanız üretimde mevcut dosyaları silmek için dağıtım işlemi istemiyorum. (Bir dosya hedef site, ancak kaynak site varsa, dağıtırken nasıl bir dağıtım seçeneği ayarladığınıza bağlı olarak, Web dağıtımı, hedef siler.)

Bu öğreticide daha önce gördüğünüz gibi **dağıtmak için öğeleri** seçeneğini **Paketle/Yayımla Web** sekmesini ayarlanmış **bu uygulamayı çalıştırmak için gereken dosyalar yalnızca**. Sonuç olarak, geliştirme Elmah tarafından oluşturulan günlük dosyalarını, olmasını istediğiniz olduğu dağıtılmaz. (Dağıtılması, bunlar projeye dahil edilmesi gerekir ve bunların **yapı eylemi** özelliği ayarlamak için sahip olabilir **içerik**. Daha fazla bilgi için bkz: **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx)). Kopyalamak için en az bir dosya olmadıkça ancak, Web dağıtımı bir klasörü hedef sitede oluşturmaz. Bu nedenle, ekleyeceksiniz bir *.txt* dosyasına klasörüne kopyalanmasını böylece yer tutucu olarak hareket edecek.

İçinde **Çözüm Gezgini**, sağ *Elmah* klasöründe seçin **Yeni Öğe Ekle**ve adlı bir metin dosyası oluşturun *Placeholder.txt*. Aşağıdaki metni koymak: "Bu klasörü dağıtılan emin olmak için bir yer tutucu dosyasıdır." Ve dosyayı kaydedin. Tüm Visual Studio bu dosya ve klasör içinde çünkü dağıttığı emin olmak için yapmanız gereken olan **yapı eylemi** özelliği *.txt* dosyaları ayarlanmış **İçerik**varsayılan olarak.

## <a name="summary"></a>Özet

Tüm dağıtım kurulum görevlerini tamamladınız. Sonraki öğreticide Contoso University site test ortamını dağıtmak ve var. sınayın.

> [!div class="step-by-step"]
> [Önceki](web-config-transformations.md)
> [sonraki](deploying-to-iis.md)
