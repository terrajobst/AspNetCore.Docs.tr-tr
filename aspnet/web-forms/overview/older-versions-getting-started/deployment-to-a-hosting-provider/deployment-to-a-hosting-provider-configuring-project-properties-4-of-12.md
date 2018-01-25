---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: Proje özelliklerini yapılandırma - 12 4 | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5632b801586c13084f887c4c414fc8686731094c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: Proje özellikleri - 4 12 yapılandırma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bazı dağıtım seçenekleri proje dosyasında depolanan proje özellikleri'nde yapılandırılır ( *.csproj* veya *.vbproj* dosyası). Çoğu durumda, bu ayarlar varsayılan değerlerini istediğinizi olsa da kullanabilirsiniz **proje özelliklerini** UI bunları değiştirmeniz gerekiyorsa bu ayarlarla çalışmak için Visual Studio'ya yerleşik. Bu öğreticide, dağıtım ayarlarını gözden **proje özelliklerini**. Ayrıca dağıtılması boş bir klasöre neden olan bir yer tutucu dosyasını oluşturursunuz.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Proje Özellikleri penceresinde dağıtım ayarlarını yapılandırma

Aşağıdaki öğreticilerde göreceğiniz gibi dağıtımı sırasında neler etkileyen çoğu ayarlar yayımlama profilinde dahil edilir. Bilmeniz gereken birkaç ayarları içinde bulunur **Paketle/Yayımla** sekmelerinde **proje özelliklerini** penceresi. Bu ayarların her yapı yapılandırması için belirtilen — diğer bir deyişle, için hata ayıklama derlemesi'den sürüm yapı için farklı ayarlara sahip olabilir.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje, select **özellikleri**ve ardından **Paketle/Yayımla Web**sekmesi.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Pencere görüntülendiğinde, hesaptaki yapı yapılandırması çözüm için şu anda etkin olan ayarları göstermek için varsayılan olarak. Varsa **yapılandırma** kutusunu belirtmez **etkin (sürüm)**seçin **sürüm** sürüm yapı yapılandırma ayarlarını görüntülemek için. Yayın derlemeleri, test ve üretim ortamları için dağıtacaksınız.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

İle **etkin (sürüm)** veya **sürüm** seçili, yayın yapı yapılandırması'nı kullanarak dağıttığınızda etkili değerleri bakın:

- İçinde **dağıtmak için öğeleri** kutusunda **yalnızca uygulamayı çalıştırmak için gerekli dosyaları** seçilir. Diğer Seçenekler **bu projedeki tüm dosyalar** veya **bu proje klasöründeki tüm dosyalar**. Değişmeden varsayılan seçimi bırakarak kaynak kodu dosyaları, örneğin dağıtma kaçının. Bu ayar, SQL Server Compact ikili dosyaları içeren klasörlerin neden projeye dahil gerekiyordu nedenidir. Bu ayar hakkında daha fazla bilgi için bkz: **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx).
- **Oluşturulan dışlama hata ayıklama simgeleri** seçilir. Bu yapı yapılandırmayı kullandığınızda, hata ayıklama olmaz.
- **Uygulama dosyaları dışarıda\_veri klasörü** seçilmemiş. SQL Server Compact dosyanızı üyelik veritabanı için bu klasörde ve bunu dağıtmak sahip. Veritabanı değişikliklerini içermez güncelleştirmeleri dağıtırken, bu onay kutusunu seçersiniz.
- **Bu uygulamayı yayımlamadan önce ön derleme yap** seçilmemiş. Çoğu senaryoda, web uygulaması projelerine derleneceği gerek yoktur. Bu seçenek hakkında daha fazla bilgi için bkz: [Paketle/Yayımla Web sekmesi, proje özellikleri](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) ve [Gelişmiş önceden derleme Ayarları iletişim kutusu](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **SQL Paketle/Yayımla sekmesinde yapılandırılmış tüm veritabanları dahil** seçilir, ancak yapılandırma değil çünkü bu seçenek artık etkisizdir **SQL Paketle/Yayımla** sekmesi. Bu sekme, SQL Server veritabanları dağıtmak için tek seçenek için kullanılan eski veritabanı dağıtım yöntemi için kullanılabilir. Kullanacağınız **SQL Paketle/Yayımla** sekmesinde [SQL Server'a geçirmeyi](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) Öğreticisi.
- **Web dağıtım paketi ayarları** tek tıklatmayla kullandığımızdan bölümü geçerli olmayan bu öğreticileri yayımlayın.

Değişiklik **yapılandırma** hata ayıklama derlemeleri için varsayılan ayarları görmek için hata ayıklama açılan kutusu. Aynı dışında değerlerdir **dışlama oluşturulan hata ayıklama simgeleri** böylece hata ayıklama derlemesi dağıttığınızda ayıklayabilirsiniz temizlenir.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Elmah klasörü dağıtılan emin yapma

Önceki öğreticide gördüğünüz gibi [Elmah NuGet paketi](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) için hata günlüğünü ve raporlama işlevselliği sağlar. Contoso University uygulamada Elmah hata ayrıntıları adlı bir klasörde depolamak için yapılandırılmış *Elmah*:

![Elmah klasörü](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Dağıtımdan belirli dosyaları veya klasörleri hariç ortak bir gereksinimdir; başka bir örnek, kullanıcılar dosyaları karşıya yükleyebilir bir klasör olabilir. Günlük dosyaları istemiyorsanız veya üretim dağıtılması için geliştirme ortamınızı oluşturulan dosyalarını karşıya. Ve üretim için bir güncelleştirme dağıtıyorsanız üretimde mevcut dosyaları silmek için dağıtım işlemi istemiyorum. (Bir dosya hedef site, ancak kaynak site varsa, dağıtırken nasıl bir dağıtım seçeneği ayarladığınıza bağlı olarak, Web dağıtımı, hedef siler.)

Bu öğreticide daha önce gördüğünüz gibi **dağıtmak için öğeleri** seçeneğini **Paketle/Yayımla Web** sekmesini ayarlanmış **bu uygulamayı çalıştırmak için gereken dosyalar yalnızca**. Sonuç olarak, geliştirme Elmah tarafından oluşturulan günlük dosyalarını, olmasını istediğiniz olduğu dağıtılmaz. (Dağıtılması, bunlar projeye dahil edilmesi gerekir ve bunların **yapı eylemi** özelliği ayarlamak için sahip olabilir **içerik**. Daha fazla bilgi için bkz: **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx)). Kopyalamak için en az bir dosya olmadıkça ancak, Web dağıtımı bir klasörü hedef sitede oluşturmaz. Bu nedenle, ekleyeceksiniz bir *.txt* dosyasına klasörüne kopyalanmasını böylece yer tutucu olarak hareket edecek.

İçinde **Çözüm Gezgini**, sağ *Elmah* klasöründe seçin **Yeni Öğe Ekle**ve adlı bir dosya oluşturun *Placeholder.txt*. Aşağıdaki metni koymak: "Bu klasörü dağıtılan emin olmak için bir yer tutucu dosyasıdır." Ve dosyayı kaydedin. Tüm Visual Studio bu dosya ve klasör içinde çünkü dağıttığı emin olmak için yapmanız gereken olan **yapı eylemi** özelliği *.txt* dosyaları ayarlanmış **İçerik**varsayılan olarak.

Tüm dağıtım kurulum görevlerini tamamladınız. Sonraki öğreticide Contoso University site test ortamını dağıtmak ve var. sınayın.

>[!div class="step-by-step"]
[Önceki](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
[sonraki](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
