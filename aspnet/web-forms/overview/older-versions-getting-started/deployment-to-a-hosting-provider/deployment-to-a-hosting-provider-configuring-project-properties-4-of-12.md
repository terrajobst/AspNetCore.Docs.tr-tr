---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Proje özelliklerini yapılandırma - 4 / 12 | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: cb496c31ba09030087180ac67647c8c0854c372b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814203"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Proje özelliklerini yapılandırma - 4 / 12
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bazı dağıtım seçenekleri, proje dosyasında depolanan proje özellikleri'nde yapılandırılır ( *.csproj* veya *.vbproj* dosyası). Çoğu durumda, istediğiniz bu ayarlar için varsayılan değerleri olan ancak kullanabilirsiniz **proje özellikleri** kullanıcı Arabirimi, bunları değiştirmeniz gerekiyorsa, bu ayarlar ile çalışmak için Visual Studio'ya oluşturuldu. Bu öğreticide dağıtım ayarlarında gözden **proje özellikleri**. Dağıtılacak boş bir klasör neden olan bir yer tutucu dosyası oluşturabilir.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Proje Özellikleri penceresinde dağıtım ayarlarını yapılandırma

Aşağıdaki öğreticilerde anlatıldığı gibi dağıtımı sırasında neler etkileyen çoğu ayarlar yayımlama profilinde dahil edilir. Bilmeniz gereken birkaç ayar yerleştirilir **Paketle/Yayımla** sekmelerin **proje özellikleri** penceresi. Bu ayarların her derleme yapılandırması için belirtilen — diğer bir deyişle, hata ayıklama derlemesi için'den bir yayın yapısı için farklı ayarlara sahip olabilir.

İçinde **Çözüm Gezgini**, sağ **ContosoUniversity** proje seçin **özellikleri**ve ardından **Paketle/Yayımla Web**sekmesi.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Pencere görüntülendiğinde, hangi derleme yapılandırması çözüm için o anda etkin olan için ayarları gösterecek şekilde varsayılan olarak. Varsa **yapılandırma** kutusu belirtmez **etkin (sürüm)** seçin **yayın** yayın derleme yapılandırması için ayarları görüntülemek için. Yayın derlemeleri, test ve üretim ortamlarına dağıtacaksınız.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

İle **etkin (sürüm)** veya **yayın** seçili, yayın derleme yapılandırmasını kullanarak dağıttığınızda geçerli değerleri görmek:

- İçinde **dağıtmak için öğeleri** kutusunda **yalnızca uygulamayı çalıştırmak için gereken dosyaları** seçilir. Diğer Seçenekler **bu projedeki tüm dosyalar** veya **bu proje klasöründeki tüm dosyalar**. Varsayılan seçimi değiştirmeden bırakarak kaynak kodu dosyaları, örneğin dağıtma kaçının. Bu ayar, SQL Server Compact ikili dosyaları içeren klasörleri neden projeye dahil gerekirdi nedenidir. Bu ayar hakkında daha fazla bilgi için bkz. **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx).
- **Dışlama oluşturulan hata ayıklama sembolleri** seçilir. Bu yapı yapılandırmasını kullandığınızda, hata ayıklama olmaz.
- **Dosyaları uygulamadan çıkar\_veri klasörü** seçilmez. Bu üyelik veritabanı için SQL Server Compact dosyanızı klasörüdür ve dağıtılması sahipsiniz. Veritabanı değişiklikleri içermez güncelleştirmeleri dağıtırken, bu onay kutusunu seçersiniz.
- **Bu uygulamayı yayımlamadan önce ön derleme** seçilmez. Çoğu senaryoda, web uygulama projeleri derleneceği gerek yoktur. Bu seçenek hakkında daha fazla bilgi için bkz. [Paketle/Yayımla Web sekmesi, proje özellikleri](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) ve [Gelişmiş ön derleme Ayarları iletişim kutusu](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **SQL Paketle/Yayımla sekmesinde yapılandırılmış tüm veritabanlarını dahil et** seçilir, ancak yapılandırma değildir çünkü bu seçenek artık etkisizdir **SQL Paketle/Yayımla** sekmesi. Bu sekme, SQL Server veritabanlarını dağıtmak için tek seçenek olarak kullanılan eski veritabanı dağıtım yöntemi içindir. Kullanacağınız **SQL Paketle/Yayımla** sekmesinde [SQL Server'a geçirmeyi](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) öğretici.
- **Web dağıtım paketi ayarları** bölüm tek tıklamayla kullandığınız için geçerli değildir bu öğreticilerde yayımlayın.

Değişiklik **yapılandırma** açılır liste kutusunda hata ayıklama yapıları için varsayılan ayarları görmek için hata ayıklama. Aynı dışındaki değerler **dışlama oluşturulan hata ayıklama sembolleri** böylece bir hata ayıklama derlemesi dağıtmak, hata ayıklaması yapabilirsiniz temizlenir.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Elmah klasörüne dağıtılır emin yapma

Önceki öğreticide gördüğünüz gibi [Elmah NuGet paketini](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) için hata günlüğünü ve raporlama işlevlerini sağlar. Elmah Contoso University uygulamada hata ayrıntılarını adlı bir klasörde depolamak için yapılandırılmış *Elmah*:

![Elmah klasörü](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Belirli dosyaları veya klasörleri dağıtımdan dışlama sık karşılaşılan bir gereksinimdir; başka bir örnek, kullanıcılar dosyaları karşıya yükleyebilir bir klasör olabilir. Günlük dosyalarını istemediğiniz ya da üretim ortamına dağıtılması için geliştirme ortamınızda oluşturulan dosyalarını karşıya. Ve üretim için bir güncelleştirme dağıtıyorsanız, dağıtım işlemi, üretim ortamında bulunan dosyaları silmek için istemediğiniz. (Bir dosya hedef site, ancak kaynak site varsa, dağıtırken nasıl bir dağıtım seçeneği ayarladığınıza bağlı olarak, Web dağıtımı hedefinizden siler.)

Bu öğreticide daha önce bahsettiğim gibi **dağıtmak için öğeleri** seçeneğini **Paketle/Yayımla Web** sekmesinde ayarlanmış **bu uygulamayı çalıştırmak için gereken dosyalar yalnızca**. Sonuç olarak, geliştirme Elmah tarafından oluşturulan günlük dosyalarını, olmasını istediğiniz olduğu dağıtılmaz. (Dağıtılacak, bunlar projeye dahil etmesi gerekir ve bunların **derleme eylemi** özelliği ayarlamak için haritamın **içerik**. Daha fazla bilgi için **neden olmayan tüm my proje klasöründeki dosyaları dağıtılan?** içinde [ASP.NET Web uygulaması projesi dağıtım SSS](https://msdn.microsoft.com/library/ee942158.aspx)). Kopyalamak için en az bir dosya olmadıkça ancak, Web dağıtımı bir klasör hedef sitede oluşturulmaz. Bu nedenle, ekleyeceksiniz bir *.txt* dosyasına klasörüne kopyalanır böylece yer tutucu olarak görev yapacak.

İçinde **Çözüm Gezgini**, sağ *Elmah* klasörüne **Yeni Öğe Ekle**, adlı bir dosya oluşturun *Placeholder.txt*. Aşağıdaki metni koymak: "Bu klasörü dağıtılır emin olmak için bir yer tutucu dosyasıdır." ve dosyayı kaydedin. Tüm Visual Studio bu dosya ve klasör içinde çünkü dağıttığı emin olmak için yapmanız gereken **derleme eylemi** özelliği *.txt* dosyaları ayarlandığında **İçerik**varsayılan olarak.

Artık tüm dağıtım kurulum görevlerini tamamladınız. Sonraki öğreticide, Contoso University site test ortamına dağıtın ve test etmek.

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [İleri](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
