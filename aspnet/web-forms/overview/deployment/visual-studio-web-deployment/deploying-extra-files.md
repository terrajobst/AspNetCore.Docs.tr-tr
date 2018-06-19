---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 5cefcedde7715844a7d7a9db1455193564ef9805
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881769"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: ek dosyaları dağıtma
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide, Visual Studio web yayımlama dağıtımı sırasında ek bir görev için ardışık düzen genişletmek gösterilmiştir. Hedef web sitesine proje klasöründeki olmayan ek dosyaları kopyalamak için bir görevdir.

Bu öğretici için bir ek dosyasını kopyalamanız: *robots.txt*. Bu dosya hazırlama, ancak üretim dağıtmak istediğiniz. İçinde [üretime dağıtma](deploying-to-production.md) öğretici, bu dosya projeye eklendi ve üretim yapılandırılmış dışlamak istediğiniz profili yayımlayın. Bu öğreticide, bu durum, dağıtmak istediğiniz, ancak projeye dahil etmek istemediğiniz tüm dosyalar için yararlı olacak bir işlemek için alternatif bir yöntem görürsünüz.

## <a name="move-the-robotstxt-file"></a>Robots.txt dosya taşıma

Farklı bir işleme yöntemi için hazırlamak üzere *robots.txt*, öğreticinin bu bölümünde, dosyayı projeye dahil edilmeyen bir klasöre taşıyın ve sildiğiniz *robots.txt* hazırlamadan ortam. Böylece bu ortam için dosyayı dağıtma yeni yönteminizi düzgün çalıştığını doğrulayabilirsiniz hazırlama dosyayı silmek gereklidir.

1. İçinde **Çözüm Gezgini**, sağ *robots.txt* dosya ve tıklayın **projenin dışında tut**.
2. Windows dosya Gezgini'ni kullanarak, çözüm klasöründe yeni bir klasör oluşturun ve adlandırın *ExtraFiles*.
3. Taşıma *robots.txt* dosya *ContosoUniversity* proje klasörüne *ExtraFiles* klasör.

    ![ExtraFiles klasörü](deploying-extra-files/_static/image1.png)
4. FTP aracınızı kullanarak silin *robots.txt* dosyasını hazırlama web sitesinden.

    Alternatif olarak, seçtiğiniz **hedefteki ek dosyaları Kaldır** altında **dosya yayımlama seçeneği** üzerinde **ayarları** hazırlama yayımlama profili sekmesinde ve Hazırlama için yeniden yayımlayın.

## <a name="update-the-publish-profile-file"></a>Yayımlama profili dosyasını güncelleştirme

Yalnızca gereksinim duyduğunuz *robots.txt* bunu dağıtmak için güncellemeniz gerekir yalnızca yayımlama profili hazırlama için hazırlama, içinde.

1. Visual Studio'da açın *Staging.pubxml*.
2. Kapatmadan önce dosyanın sonunda `</Project>` etiketi, aşağıdaki biçimlendirmeyi ekleyin:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Bu kod yeni bir oluşturur *hedef* dağıtılması için ek dosyalar toplamak. Bir hedef birini oluşur veya MSBuild yürütülmez daha fazla görev, belirttiğiniz koşullara göre.

    `Include` Özniteliği belirtir dosyaları bulunacağı klasöründe olduğunu *ExtraFiles*, proje klasöründen ile aynı düzeyde yer. MSBuild bu klasörü ve (yinelemeli alt çift yıldız belirtir) tüm alt yinelemeli olarak tüm dosyaları toplar. Bu kodu, birden çok dosya ve dosyaları içinde alt koyabilirsiniz *ExtraFiles* klasörünü ve tüm dağıtılacak.

    `DestinationRelativePath` Öğesi belirttiğinden içinde bulunan gibi dosya ve klasörleri hedef web sitesinin aynı dosya ve klasör yapısını kök klasörü kopyalanacağı olduğunu *ExtraFiles* klasör. Kopyalamak istiyorsanız *ExtraFiles* klasörünün kendisi, `DestinationRelativePath` değer olacaktır *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Kapatmadan önce dosyanın sonunda `</Project>` etiketi, yeni hedef yürütmek ne zaman belirtir aşağıdaki biçimlendirmeyi ekleyin.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Bu kod yeni neden `CustomCollectFiles` her dosyaları hedef klasöre kopyalar hedef yürütüldüğünde yürütülecek hedef. Yoktur için ayrı bir hedef yayımlama karşı dağıtım paketi oluşturma ve yayımlama yerine bir dağıtım paketi kullanarak dağıtmaya karar durumda yeni hedef iki hedefleri eklenmiş.

    *.Pubxml* dosya şimdi aşağıdaki gibi görünür:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Kaydet ve Kapat *Staging.pubxml* dosya.

## <a name="publish-to-staging"></a>Hazırlama için yayımlama

Tek tıklamayla yayımlama veya hazırlama profili kullanarak uygulama yayımlama komut satırı.

Tek tıklamayla kullanırsanız, yayımlama, içinde doğrulayabilirsiniz **Önizleme** penceresi, *robots.txt* kopyalanacak. Aksi takdirde, FTP doğrulamak için aracını *robots.txt* web sitesinin kök klasöründe dağıtımdan sonra dosyasıdır.

## <a name="summary"></a>Özet

Bu, bir üçüncü taraf barındırma sağlayıcısına ASP.NET web uygulaması dağıtma öğreticileri bu dizi tamamlar. Herhangi biri bu öğreticileri kapsanan konular hakkında daha fazla bilgi için bkz: [ASP.NET dağıtım içerik haritası](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Daha fazla bilgi

MSBuild dosyalar üzerinde çalışmak nasıl biliyorsanız, kod yazarak diğer birçok dağıtım görevlerini otomatikleştirebilirsiniz *.pubxml* dosyaları (Profil özgü görevler) ya da proje *. wpp.targets* dosyası (için görevleri, Tüm profiller için geçerli). Hakkında daha fazla bilgi için *.pubxml* ve *. wpp.targets* dosyaları görmek [nasıl yapılır: dağıtım ayarlarını düzenle yayımlama profili (.pubxml) dosyaları ve. wpp.targets Visual Studio Web dosyasında Projeleri](https://msdn.microsoft.com/library/ff398069). MSBuild kod temel bir giriş için bkz: **bir proje dosyası anatomisi** içinde [kurumsal dağıtım serisi: Proje dosyası anlama](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Bu kitap kendi senaryoları için görevleri gerçekleştirmek için MSBuild dosyaları ile çalışma konusunda bilgi edinmek için bkz: [içinde Microsoft Build Engine: MSBuild kullanma ve Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi ve William Bartholomew.

## <a name="acknowledgements"></a>Katkıda Bulunanlar

Bu öğretici dizisinin içeriğe önemli ölçüde katkıda yapılan aşağıdaki kişilere teşekkür ederiz ister misiniz:

- [Adı Poblacion, MVP &amp; MCT, İspanya](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, veri Platform geliştirme MVP, Amerika Birleşik Devletleri
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italy](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Tan Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sırbistan](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Önceki](command-line-deployment.md)
> [sonraki](troubleshooting.md)
