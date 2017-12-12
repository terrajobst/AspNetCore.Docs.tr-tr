---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "(Razor) Site ASP.NET Web sayfaları için izleme ziyaretçi bilgileri (Analytics) | Microsoft Docs"
author: tfitzmac
description: "Giderek Web sitenizin kabulünüzü sonra Web sitesi trafiğini analiz etmek isteyebilirsiniz."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Ziyaretçi bilgi (Analytics) bir ASP.NET Web sayfaları (Razor) sitesi için izleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sayfaları Web sitesi analytics eklemek için bir yardımcı kullanmayı açıklar.
> 
> Öğrenecekleriniz:
> 
> - Bir analiz sağlayıcısı, Web sitesi trafiğini hakkında bilgi göndermek nasıl.
> 
> Bu makalede sunulan özellikler programlama ASP.NET şunlardır:
> 
> - `Analytics` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)


Analytics kişiler site kullanımını anlamak için Web trafiği ölçer teknolojisini için genel bir terimdir. Birçok Analiz Hizmetleri Hizmetleri'nden Google, Yahoo, StatCounter ve diğerleri de dahil olmak üzere kullanılabilir.

Bir hesap için siteyi kaydettirmeniz burada analiz sağlayıcısı ile kayıt olduğunu Works olduğu şekilde analytics izlemek istiyorsunuz. Sağlayıcı bir kimlik veya izleme hesabınız için kodu içeren JavaScript kod parçacığını gönderir. İzlemek istediğiniz sitenin web sayfalarında JavaScript kod parçacığı ekleyin. (Genellikle analytics kod parçacığını bir alt bilgi veya düzen sayfası veya sitenizdeki her sayfada görünen diğer HTML biçimlendirmesi eklediğiniz.) Kullanıcılar bu JavaScript parçacıkları birini içeren bir sayfa istediğinde, kod parçacığında sayfa çeşitli ayrıntılarını kaydeder analiz sağlayıcısı geçerli sayfa hakkında bilgi gönderir.

Site İstatistikler bakın istediğinizde, analytics sağlayıcının Web sitesinde oturum açmak. Ardından, gibi siteniz hakkında raporlar her türlü görüntüleyebilirsiniz:

- Tek tek sayfaları için sayfa görünümleri sayısı. Bu, kaç (kabaca) kişinin siteyi ziyaret ediyorsanız ve hangi sitenizde en popüler sayfalarıdır bildirir.
- Ne kadar süreyle kişiler belirli sayfalara ayırın. Bu giriş sayfanız kişilerin ilgi olup tutma gibi şeyler anlayabilirsiniz.
- Sitenizi ziyaret önce üzerinde hangi siteleri kişiler yoktu. Bu trafiğinizi arar ve benzeri bağlantılardan gelen olup olmadığını anlamanıza yardımcı olur.
- Kişiler, sitenizi ziyaret ettiğinizde ve ne kadar süreyle kalırlar.
- Hangi ülkelerde, ziyaretçileri arasındadır.
- Hangi tarayıcılar ve işletim sistemleri, ziyaretçileri kullanıyor.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Bir sayfaya Analytics eklemek için bir yardımcı kullanma

ASP.NET Web sayfaları içeren birkaç analytics Yardımcıları (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, ve `Analytics.GetStatCounterHtml`) kolaylaştıran analizi için kullanılan JavaScript parçacıkları yönetmek. Nasıl ve nerede çözülmesi yerine JavaScript kodu koymak için tüm yapmanız gereken olduğu yardımcı bir sayfasına ekleyin. Sağlamanız gereken tek bilgi hesap adı, kimliği veya izleme kodunu ' dir. (StatCounter için Ayrıca birkaç ek değerler sağlamanız gerekir.)

Bu yordamda kullanan bir düzen sayfasını oluşturacaksınız `GetGoogleHtml` Yardımcısı. Diğer analytics sağlayıcılardan biri ile zaten bir hesabınız varsa, bu hesabı kullanın ve gerektiğinde hafif ayarlamaları yapın.

> [!NOTE]
> Bir analytics hesabı oluşturmak için izleme olmasını istediğiniz site URL'sini kaydedersiniz. Her şeyin yerel bilgisayarınızda test ediyorsanız, (yalnızca trafiğini şey) gerçek trafiği izleme olmaz kaydı ve görünüm site İstatistikler mümkün olmayacaktır. Ancak bu yordam analytics yardımcı bir sayfaya nasıl eklediğiniz gösterir. Sitenizi yayımladığınızda, canlı sitede analytics sağlayıcınız bilgi gönderin.


1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), onu zaten eklemediniz.
2. Google Analytics ile bir hesap oluşturun ve hesap adını kaydedin.
3. Adlı bir düzen sayfası oluşturun *Analytics.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Çağrı yerleştirmelisiniz `Analytics` Yardımcısı, web sayfasının gövdesindeki (önce `</body>` etiketi). Aksi durumda, tarayıcının betiği çalışmaz.

    Farklı analiz sağlayıcısı kullanıyorsanız, bunun yerine aşağıdaki Yardımcılar birini kullanın:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Değiştir `myaccount` hesabı, kimliği veya 1. adımda oluşturduğunuz izleme kodu adı.
5. Sayfayı tarayıcıda çalıştırın. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.)
6. Tarayıcıda, sayfa kaynağı görüntüleyin. İşlenmiş analytics kod kullanabileceksiniz:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Google Analytics sitesinde oturum ve sitenizi istatistikleri inceleyin. Canlı bir sitede sayfa çalıştırıyorsanız, sayfanızı ziyaret günlüklerini bir girişine bakın.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Google Analytics site](https://www.google.com/analytics/)
- [Yahoo! Web sitesi analizi](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter site](http://statcounter.com/)
