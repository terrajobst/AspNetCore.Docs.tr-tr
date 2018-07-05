---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Project Properties | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 48f2d5066986860b657d5608bd32fcfc9b6a1eda
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365892"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Proje Özellikleri
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bazı dağıtım seçenekleri, proje dosyasında depolanan proje özellikleri'nde yapılandırılır ( *.csproj* veya *.vbproj* dosyası). Çoğu durumda, istediğiniz bu ayarlar için varsayılan değerleri olan ancak kullanabilirsiniz **proje özellikleri** kullanıcı Arabirimi, bunları değiştirmeniz gerekiyorsa, bu ayarlar ile çalışmak için Visual Studio'ya oluşturuldu. Bu öğreticide dağıtım ayarlarında gözden **proje özellikleri**. Dağıtılacak boş bir klasör neden olan bir yer tutucu dosyası oluşturabilir.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Proje Özellikleri penceresinde dağıtım ayarlarını yapılandırma

Aşağıdaki öğreticilerde anlatıldığı gibi dağıtımı sırasında neler etkileyen çoğu ayarlar yayımlama profilinde dahil edilir. Bilmeniz gereken birkaç ayar yerleştirilir **Paketle/Yayımla** sekmelerin **proje özellikleri** penceresi. Bu ayarların her derleme yapılandırması için belirtilen — diğer bir deyişle, hata ayıklama derlemesi için'den bir yayın yapısı için farklı ayarlara sahip olabilir.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje seçin **özellikleri**ve ardından **Paketle/Yayımla Web**sekmesi.

![Web'i Paketle/Yayımla sekmesinde](project-properties/_static/image1.png)

Pencere görüntülendiğinde, hangi derleme yapılandırması çözüm için o anda etkin olan için ayarları gösterecek şekilde varsayılan olarak. Varsa **yapılandırma** kutusu belirtmez **etkin (sürüm)** seçin **yayın** yayın derleme yapılandırması için ayarları görüntülemek için. Yayın derlemeleri, test ve üretim ortamlarına dağıtacaksınız.

![Yayın derleme yapılandırması seçme](project-properties/_static/image2.png)

İle **etkin (sürüm)** veya **yayın** seçili, yayın derleme yapılandırmasını kullanarak dağıttığınızda geçerli değerleri görmek:

- İçinde **dağıtmak için öğeleri** kutusunda **yalnızca uygulamayı çalıştırmak için gereken dosyaları** seçilir. Diğer Seçenekler **bu projedeki tüm dosyalar** veya **bu proje klasöründeki tüm dosyalar**. Varsayılan seçimi değiştirmeden bırakarak kaynak kodu dosyaları, örneğin dağıtma kaçının. Bu ayar, SQL Server Compact ikili dosyaları içeren klasörleri neden projeye dahil gerekirdi nedenidir. Bu ayar hakkında daha fazla bilgi için bkz. **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx).
- **Dışlama oluşturulan hata ayıklama sembolleri** seçilir. Bu yapı yapılandırmasını kullandığınızda, hata ayıklama olmaz.
- **SQL Paketle/Yayımla sekmesinde yapılandırılmış tüm veritabanlarını dahil et** seçilir. Visual Studio veritabanlarının yanı sıra dosyaları dağıtma olup olmadığını belirtir. Etiket onay kutusu ancak yalnızca bahsetmeleri **SQL Paketle/Yayımla** sekmesinde bu onay kutusunun işaretini da devre dışı yayımlama profilinde yapılandırılan veritabanı dağıtımı. Onay kutusunu seçili olarak kalması gereken şekilde, daha sonra yaptığınız. **SQL Paketle/Yayımla** sekmesi, bu öğreticilerde kullanmayacaksanız yöntemi yayımlama eski bir veritabanı için kullanılır.
- **Web dağıtım paketi ayarları** bölüm tek tıklamayla kullandığınız için geçerli değildir bu öğreticilerde yayımlayın.

Değişiklik **yapılandırma** açılır liste kutusunda hata ayıklama yapıları için varsayılan ayarları görmek için hata ayıklama. Aynı dışındaki değerler **dışlama oluşturulan hata ayıklama sembolleri** böylece bir hata ayıklama derlemesi dağıtmak, hata ayıklaması yapabilirsiniz temizlenir.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Elmah klasörüne dağıtılır emin olun

Önceki öğreticide gördüğünüz gibi [Elmah NuGet paketini](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) için hata günlüğünü ve raporlama işlevlerini sağlar. Elmah Contoso University uygulamada hata ayrıntılarını adlı bir klasörde depolamak için yapılandırılmış *Elmah*:

![Elmah klasörü](project-properties/_static/image3.png)

Belirli dosyaları veya klasörleri dağıtımdan dışlama sık karşılaşılan bir gereksinimdir; başka bir örnek, kullanıcılar dosyaları karşıya yükleyebilir bir klasör olabilir. Günlük dosyalarını istemediğiniz ya da üretim ortamına dağıtılması için geliştirme ortamınızda oluşturulan dosyalarını karşıya. Ve üretim için bir güncelleştirme dağıtıyorsanız, dağıtım işlemi, üretim ortamında bulunan dosyaları silmek için istemediğiniz. (Bir dosya hedef site, ancak kaynak site varsa, dağıtırken nasıl bir dağıtım seçeneği ayarladığınıza bağlı olarak, Web dağıtımı hedefinizden siler.)

Bu öğreticide daha önce bahsettiğim gibi **dağıtmak için öğeleri** seçeneğini **Paketle/Yayımla Web** sekmesinde ayarlanmış **bu uygulamayı çalıştırmak için gereken dosyalar yalnızca**. Sonuç olarak, geliştirme Elmah tarafından oluşturulan günlük dosyalarını, olmasını istediğiniz olduğu dağıtılmaz. (Dağıtılacak, bunlar projeye dahil etmesi gerekir ve bunların **derleme eylemi** özelliği ayarlamak için haritamın **içerik**. Daha fazla bilgi için **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx)). Kopyalamak için en az bir dosya olmadıkça ancak, Web dağıtımı bir klasör hedef sitede oluşturulmaz. Bu nedenle, ekleyeceksiniz bir *.txt* dosyasına klasörüne kopyalanır böylece yer tutucu olarak görev yapacak.

İçinde **Çözüm Gezgini**, sağ *Elmah* klasörüne **Yeni Öğe Ekle**, adlı bir metin dosyasını oluşturup *Placeholder.txt*. Aşağıdaki metni koymak: "Bu klasörü dağıtılır emin olmak için bir yer tutucu dosyasıdır." ve dosyayı kaydedin. Tüm Visual Studio bu dosya ve klasör içinde çünkü dağıttığı emin olmak için yapmanız gereken **derleme eylemi** özelliği *.txt* dosyaları ayarlandığında **İçerik**varsayılan olarak.

## <a name="summary"></a>Özet

Artık tüm dağıtım kurulum görevlerini tamamladınız. Sonraki öğreticide, Contoso University site test ortamına dağıtın ve test etmek.

> [!div class="step-by-step"]
> [Önceki](web-config-transformations.md)
> [İleri](deploying-to-iis.md)
