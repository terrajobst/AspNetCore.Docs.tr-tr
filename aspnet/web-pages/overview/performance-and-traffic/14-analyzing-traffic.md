---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: ASP.NET Web sayfaları (Razor) sitesinde ziyaretçi bilgi (analiz) izleme | Microsoft Docs
author: tfitzmac
description: Sitenizi giderek yönettiniz sonra Web sitesi trafiğini analiz etmek isteyebilirsiniz.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 48782606083b4aa1e32adf6163bcb3f2d9828bc3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387143"
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>İzleme için bir ASP.NET Web sayfaları (Razor) sitesinde ziyaretçi bilgileri (analiz)
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web Pages'da Web sitesi analizi eklemek için bir yardımcı kullanmayı açıklar.
> 
> Öğrenecekleriniz:
> 
> - Nasıl Web sitesi trafiğiniz hakkında bilgi bir analiz sağlayıcısına gönderebilirsiniz.
> 
> ASP.NET programlama makalesinde sunulan özellikler şunlardır:
> 
> - `Analytics` Yardımcısı.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - ASP.NET Web Yardımcıları kitaplığı (NuGet paketi)


Analytics sitesini nasıl kullandığını anlayın, böylece Web sitenize trafiği ölçer teknolojisi için genel bir terimdir. Birçok Analiz Hizmetleri, Google, Yahoo, StatCounter ve diğer hizmetleri dahil olmak üzere kullanılabilir.

İzlemek istediğiniz bir hesap için analiz sağlayıcısı ile site kaydetme yeri, kaydolduktan olduğunu çalıştığını şekilde analiz. Sağlayıcı bir kimliği ya da izleme kodu hesabınız için JavaScript kod parçacığını gönderir. İzlemek istediğiniz sitenin web sayfalarında JavaScript kod parçacığını ekleyin. (Genellikle analytics kod parçacığı bir alt bilgi veya düzen sayfası veya sitenizde her sayfada görünen diğer HTML biçimlendirmesi eklediğiniz.) Kullanıcılar bu JavaScript kod parçacıklarının birini içeren bir sayfa istediğinde, kod parçacığı sayfası ile ilgili çeşitli ayrıntılar hakkında bilgi kayıtları analiz sağlayıcısı geçerli sayfa hakkında bilgi gönderir.

Göz atın, site istatistikleri istediğinizde, analytics sağlayıcının Web sitesine oturum. Ardından siteniz hakkında raporlar her türlü gibi görüntüleyebilirsiniz:

- Tek tek sayfaları için sayfa görüntüleme sayısı. Bu, kaç (yaklaşık) kişiler siteyi ziyaret edin ve hangi sayfaların sitenizde en popüler olduğunu bildirir.
- Ne kadar süreyle kişiler, belirli bir sayfa ayırın. Bu, giriş sayfanızın Halk ilgi olup tutma gibi şeyler söyleyebilirsiniz.
- Sitenizi ziyaret önce hangi siteleri kişiler üzerinde yoktu. Bu bağlantıları, arama vb. trafiğiniz geldiği olup olmadığını anlamanıza yardımcı olur.
- Kişiler, sitenizi ziyaret ettiğinizde ve ne kadar uzun kalır.
- Hangi ülkelerde ziyaretçilerinizin arasındadır.
- Hangi tarayıcılar ve işletim sistemleri ziyaretçilerinizin kullanmaktadır.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Bir analiz eklemek için bir yardımcı kullanma

ASP.NET Web sayfaları içeren birkaç analytics Yardımcıları (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, ve `Analytics.GetStatCounterHtml`) kolaylaştıran analizler için kullanılan JavaScript kod parçacıkları yönetmek. Nasıl ve nerede çözülmesi yerine JavaScript kodu, tüm yapmanız gereken eklemektir yardımcı bir sayfaya ekleyin. Sağlamanız gereken yalnızca hesap adınız, kimliği ya da izleme kodu bilgilerdir. (StatCounter için Ayrıca bazı ek değerler sağlamanız gerekir.)

Bu yordamda, kullanan bir düzen sayfası oluşturacaksınız `GetGoogleHtml` Yardımcısı. Diğer analiz sağlayıcılardan birini zaten bir hesabınız varsa, bunun yerine bu hesabı kullanın ve gerektiğinde hafif ayarlamalar yapın.

> [!NOTE]
> Analytics hesabı oluşturmak için izleme yapmak istediğiniz sitenin URL'sini kaydedersiniz. Her şeyin yerel bilgisayarınızda test ediyorsanız, (yalnızca trafik şey) gerçek trafik izleme olmaz kaydetmenizi ve görüntülemenizi site istatistikleri mümkün olmayacaktır. Ancak bu yordamı nasıl analiz yardımcı bir sayfaya ekleyin gösterir. Sitenizi yayımladığınızda, Canlı site bilgileri, analiz sağlayıcısına gönderebilirsiniz.


1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), bunu zaten eklemediniz.
2. Bir hesap ile Google Analytics'e oluşturun ve hesap adını kaydedin.
3. Adlı bir düzen sayfası oluşturma *Analytics.cshtml* ve aşağıdaki işaretlemeyi ekleyin:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Çağrı yerleştirmelisiniz `Analytics` Yardımcısı, web sayfasının gövdesindeki (önce `</body>` etiketi). Aksi takdirde, tarayıcının betiği çalışmaz.

    Farklı bir analiz sağlayıcısı kullanıyorsanız, bunun yerine aşağıdaki Yardımcıları birini kullanın:

    - (Yahoo) `@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter) `@Analytics.GetStatCounterHtml("project", "security")`
4. Değiştirin `myaccount` hesabı, kimliği veya 1. adımda oluşturduğunuz izleme kod adı.
5. Sayfanın tarayıcıda çalıştırın. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.)
6. Tarayıcıda, sayfa kaynağı görüntüleyin. İşlenmiş analytics kodu görmek mümkün olacaktır:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Google Analytics sitesinde oturum ve istatistikleri sitenizi inceleyin. Sayfa Canlı site üzerinde çalıştırıyorsanız, sayfanızı ziyaret günlüğe kaydeden bir girdi görürsünüz.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [Google Analytics site](https://www.google.com/analytics/)
- [Yahoo! Web sitesi analizi](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter site](http://statcounter.com/)
