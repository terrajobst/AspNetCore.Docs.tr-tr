---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 41bea625a53afbf7b39c03a2e8a92a03ce144289
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371826"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, Visual Studio web yayımlama, dağıtım sırasında ek bir görev için işlem hattı genişletmek gösterilmektedir. Hedef web sitesine proje klasöründeki olmayan ek dosyaları kopyalamak için bir görevdir.

Bir ek dosya kopyalamak için bu öğreticiyi: *robots.txt*. Bu dosya hazırlama, ancak üretim dağıtmak istediğiniz. İçinde [üretime dağıtma](deploying-to-production.md) öğretici, bu dosyayı projeye eklenir ve üretim yapılandırılmış dışlamak istediğiniz profili yayımlayın. Bu öğreticide, bu durum, dağıtmak istediğiniz, ancak projeye dahil etmek istemediğiniz dosyalar için yararlı olacak bir işlemek için alternatif bir yöntem görürsünüz.

## <a name="move-the-robotstxt-file"></a>Robots.txt dosya taşıma

Farklı bir yöntem işleme için hazırlamak üzere *robots.txt*öğreticinin bu bölümünde, dosyanın projede yer almayan bir klasöre taşıyın ve sildiğiniz *robots.txt* hazırlamadan ortam. Böylece dosyanın söz konusu ortama dağıtma yeni yönteminizi düzgün çalıştığını doğrulayabilirsiniz hazırlama alanından dosya silmek gereklidir.

1. İçinde **Çözüm Gezgini**, sağ *robots.txt* tıklayın ve dosya **projeden Çıkart**.
2. Windows dosya Gezgini'ni kullanarak çözüm klasöründe yeni bir klasör oluşturun ve adlandırın *ExtraFiles*.
3. Taşıma *robots.txt* dosya *ContosoUniversity* proje klasörüne *ExtraFiles* klasör.

    ![ExtraFiles klasörü](deploying-extra-files/_static/image1.png)
4. FTP aracını kullanarak silme *robots.txt* hazırlama web sitesinden dosya.

    Alternatif olarak, seçtiğiniz **hedefteki ek dosyaları Kaldır** altında **dosya yayımlama seçeneği** üzerinde **ayarları** hazırlama yayımlama profili sekmesinde ve Hazırlık ortamına yeniden yayımlayın.

## <a name="update-the-publish-profile-file"></a>Yayımlama profili dosyasını güncelleştirme

Yalnızca gereksinim duyduğunuz *robots.txt* bunu dağıtmak için güncelleştirmeniz yalnızca yayımlama profili hazırlama için hazırlama, içinde.

1. Visual Studio'da açın *Staging.pubxml*.
2. Kapatmadan önce dosyanın sonunda `</Project>` etiketinde, aşağıdaki işaretlemeyi ekleyin:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Bu kod yeni bir oluşturur *hedef* dağıtılacak ek dosyaların toplayacak. Bir hedef birini oluşur veya MSBuild yürütecek daha fazla görev belirttiğiniz koşullara göre.

    `Include` Öznitelik dosyaları bulmak klasörü olduğunu belirtir *ExtraFiles*bulunan proje klasörüyle aynı düzeyde. MSBuild, klasör ve (çift yıldız işareti yinelemeli alt klasörleri belirtir) tüm alt klasörlerde yinelemeli olarak tüm dosyaları toplar. Bu kodu, birden fazla dosya ve dosya alt klasörleri yerleştirebilirsiniz *ExtraFiles* klasörünü ve tüm dağıtılacak.

    `DestinationRelativePath` Öğe içinde bulunan dosya ve klasörleri hedef web sitesinin aynı dosya ve klasör yapısını kök klasörüne kopyalanacağını belirtir *ExtraFiles* klasör. Kopyalamak isterseniz *ExtraFiles* klasör kendisini `DestinationRelativePath` değeri olacak *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Kapatmadan önce dosyanın sonunda `</Project>` etiketinde, yeni hedef yürütme zamanı belirten aşağıdaki işaretlemeyi ekleyin.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Bu kod yeni neden `CustomCollectFiles` hedef dosyaları hedef klasöre kopyalar. hedef yürütüldüğünde yürütülecek. Var ayrı bir hedef için yayımlama dağıtım paketi oluşturma ve yayımlama yerine bir dağıtım paketi kullanarak dağıtmaya karar durumunda yeni hedef hem de hedef eklenmiş olur.

    *.Pubxml* dosya şimdi aşağıdaki örnekteki gibi görünür:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Kaydet ve Kapat *Staging.pubxml* dosya.

## <a name="publish-to-staging"></a>Hazırlık ortamına yayımlama

Tek tıklamayla kullanarak yayımla veya hazırlama profili kullanarak uygulama yayımlama komut satırı.

Tek tıklamayla kullanırsanız, yayımlama, içinde doğrulayabilirsiniz **Önizleme** penceresi, *robots.txt* kopyalanır. Aksi takdirde doğrulamak için FTP aracını kullanın *robots.txt* web sitesinin kök klasöründe dağıtımdan sonra dosyasıdır.

## <a name="summary"></a>Özet

Bu, Bu öğretici serisinde, bir üçüncü taraf barındırma sağlayıcısı bir ASP.NET web uygulamasına dağıtma tamamlar. Aşağıdaki öğreticilerde kapsamdaki konularına hakkında daha fazla bilgi için bkz. [ASP.NET dağıtım içerik haritası](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Daha fazla bilgi

MSBuild dosyaları ile çalışma konusunda biliyorsanız, kod yazarak diğer birçok dağıtım görevleri otomatikleştirebilirsiniz *.pubxml* dosyalarını (Profil özgü görevler) veya proje *. wpp.targets* dosyası (için görevler, Tüm profiller için geçerlidir). Hakkında daha fazla bilgi için *.pubxml* ve *. wpp.targets* dosyaları için bkz [nasıl yapılır: dağıtım ayarlarını düzenle yayımlama profili (.pubxml) dosyalarını ve. wpp.targets Visual Studio Web dosyasında Projeleri](https://msdn.microsoft.com/library/ff398069). MSBuild kod temel bir giriş için bkz: **bir proje dosyası anatomisi** içinde [kurumsal dağıtım serisi: Proje dosyasını anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Bu kitap kendi senaryolarınız için görevleri gerçekleştirmek için MSBuild dosyaları ile çalışma hakkında bilgi almak için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation derlemesi](http://msbuildbook.com) Sayed Ibraham Hashimi ve William Bartholomew.

## <a name="acknowledgements"></a>Onayları

Bu öğretici serisinin önemli katkılar içeriğe yapılan aşağıdaki kişilerin teşekkür ister misiniz:

- [Alberto Poblacion, MVP &amp; MCT, İspanya](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, veri platformu geliştirme MVP, Amerika Birleşik Devletleri
- Sert Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, İtalya](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sırbistan](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Önceki](command-line-deployment.md)
> [İleri](troubleshooting.md)
