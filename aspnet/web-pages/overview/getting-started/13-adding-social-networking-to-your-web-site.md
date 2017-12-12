---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: "Sosyal ağ eklemek için ASP.NET Web sayfaları (Razor) siteleri | Microsoft Docs"
author: tfitzmac
description: "Bu bölümde, sitenizi Sosyal Ağ Hizmetleri ile tümleştirmek açıklanmaktadır. Bu bölümde, Web sitenizin yer işareti/bağlantı kişilere öğreneceksiniz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 2c43fa7d286e43f3a4581662ce421c7435e1871f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) siteleri sosyal ağ ekleme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, sosyal ağ bağlantıları Facebook, Twitter, Reddit ve Digg için bir ASP.NET Web sayfaları (Razor) Web sayfalarında nasıl ekleneceğini ve Twitter akışlarını, Xbox oyuncu kartları ve Gravatar görüntüleri dahil olmak üzere nasıl açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Yer işareti/sitenizi bağlantı kişilere nasıl.
> - Bir Twitter akışı ekleme.
> - Bir Facebook ekleme **gibi** düğmesi sayfalara.
> - Gravatar.com resimleri işlemek nasıl.
> - Sitenizde Xbox oyuncu kartı görüntülemek nasıl.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - ASP.NET Web Yardımcısı kitaplığı (NuGet paketi)
>   
> 
> Bu öğreticide, ASP.NET Web Pages 3 ile ASP.NET Web Yardımcısını kitaplığını kullanan bölümleri dışında de çalışır.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Web sitenizi sosyal ağ sitelerinde bağlama

Kişiler, sitenizde bir şey istiyorsanız, genellikle arkadaşlarınızla paylaşmak isterler. Bu kişiler Digg, Reddit, Facebook, Twitter veya benzer siteleri sayfasında paylaşmak için tıklatabilirsiniz (simgeler) karakterlerin görüntüleyerek kolayca yapabilirsiniz.

Bu karakterlerin görüntülemek için add `LinkSharecode` bir sayfaya Yardımcısı. Sayfanızı ziyaret eden kişiler tek tek bir karakter tıklatabilirsiniz. Bunlar, sosyal ağ sitesiyle bir hesabınız varsa, bunlar sayfanıza bir bağlantı sonra bu sitede nakledebilirsiniz.

![Resim 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), kendisine - henüz eklediyseniz adlı bir sayfa oluşturma *ListLinkShare.cshtml* ve Ekle Aşağıdaki biçimlendirmede:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    Bu örnekte, zaman `LinkShare` Yardımcısı çalıştığında, sayfa başlığı, sosyal ağ sitesine sayfa başlığı sırayla geçirmeden bir parametre olarak geçirilir. Ancak, istediğiniz herhangi bir dize geçirebilirdiniz. Bu örnek ayrıca hangi sosyal ağ sitelerine listeye dahil belirtir. Siteniz için uygun olan sosyal ağ sitelerine belirtebilirsiniz.
- Çalıştırma *ListLinkShare.cshtml* sayfasını bir tarayıcıda. (Emin olun sayfa seçildiğinde, **dosyaları** çalıştırmadan önce onu çalışma.)
- Oturum açtıysanız sitelerden biri karaktere'ı tıklatın. Bağlantıyı sayfaya bir bağlantı burada paylaşabilirsiniz seçili sosyal ağ sitesinde alır. Reddit bağlantısını tıklatırsanız, örneğin, gittiğiniz `submit to reddit` Reddit Web sayfasında.

    ![Resim 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Bir Twitter ekleme akışı

Twitter API'si geçerli sürümü ile uyumlu olan bir Twitter Yardımcısı kullanma hakkında daha fazla bilgi için bkz: [Twitter Yardımcısı](../ui-layouts-and-themes/twitter-helper.md). Bu örnek nasıl birden çok sayfa kodundan kolayca yeniden kullanabilirsiniz şekilde kendi yardımcı yazılacağını gösterir.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Bir Facebook görüntüleme &quot;gibi&quot; düğmesi

Bazı durumlarda, sizin için en iyi seçenek Yardımcısı üzerinde güvenmek yerine sosyal ağ sağlayıcı doğrudan kod elde etmektir. Sosyal ağ sağlayıcısına seçeneklerini yardımcı güncelleştirilmiş daha hızlı güncelleştirmeleri olursa bu özellikle doğrudur.

Facebook özellikleri (örneğin, LIKE düğmesi) sitenize eklemek için gelen kod parçacıkları alabilirsiniz [developers.facebook.com](https://developers.facebook.com/) site. Facebook sitesinde sitenize ilgili bir kod parçacığı oluşturmak için kendi araçlarını kullanın.

Aşağıdaki vurgulanmış kodu developers.facebook.com sitesinde gibi düğmesi aracından alınan kodudur. Kendi uygulama kimliğini sağlamanız gerekiyor.

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Gravatar görüntü işleme

A *Gravatar* (bir &quot;genel tanınan avatar&quot;) görüntüyü kullanılabilecek birden çok Web sitelerinde, avatar &#8212;olarak; bu ise, temsil eden bir görüntü. Örneğin, bir Gravatar kişisel bir blog açıklamasında bir forum gönderisi olarak belirleyin ve benzeri. (Kendi Gravatar Gravatar Web sitesindeki kaydedebilirsiniz [http://www.gravatar.com/](http://www.gravatar.com/).) Kişilerin adlarını veya e-posta adreslerini yanındaki resimleri, Web sitenizde görüntülemek istiyorsanız, Gravatar yardımcıyı kullanabilirsiniz.

Bu örnekte, kendiniz temsil eden tek bir Gravatar kullanıyorsunuz. Bir Gravatar kullanmak için başka bir sitenizde kaydettiğinizde, Gravatar adreslerini belirtin kişilere yoludur. (Kaydetmek kişilere öğrenebilirsiniz [güvenlik ekleme ve bir ASP.NET Web sayfaları Site üyeliği](https://go.microsoft.com/fwlink/?LinkId=202904).) Bu kullanıcı bilgilerini görüntüleme olduğunda, daha sonra yalnızca Gravatar kullanıcının adını burada görüntülemek için ekleyebilirsiniz.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir web sayfası oluşturun *Gravatar.cshtml*.
3. Aşağıdaki biçimlendirmede dosyaya ekleyin: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml` Yöntemi Gravatar görüntü sayfasında görüntülenir. Görüntü boyutunu değiştirmek için ikinci parametre olarak bir sayı içerebilir. Varsayılan boyut 80'dir. 80'den az yapma görüntünün küçük numaralandırır. 80'den büyük sayılar görüntü büyütün.
4. İçinde `Gravatar.GetHtml` yöntemleri Değiştir `<Your Gravatar account here>` Gravatar hesabınız için kullandığınız e-posta adresine sahip. (Gravatar hesabınız yoksa, mu birinin e-posta adresini kullanabilirsiniz.)
5. Sayfayı tarayıcınızda çalıştırın. Sayfasında, belirttiğiniz e-posta adresi için iki Gravatar görüntü görüntüler. İkinci görüntünün ilk küçüktür. 

    ![Resim 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Xbox oyuncu kartı görüntüleme

Kişiler çevrimiçi oyunlar Microsoft Xbox yürütürken, her kullanıcı bir benzersiz kimliği vardır. İstatistikleri her Player kendi saygınlığı, oyuncu puanı gösterilir ve son oynanan oyunları bir oyuncu kartı biçiminde saklanır. Oyuncu Xbox değilseniz, size oyuncu kartınız sitenizdeki sayfalarında kullanarak gösterebilir `GamerCard` Yardımcısı.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı gibi Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web Pages sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturma *XboxGamer.cshtml* ve aşağıdaki biçimlendirmeyi ekleyin.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Kullandığınız `GamerCard.GetHtml` özelliği görüntülenecek oyuncu kartı için diğer ad belirtin.
3. Sayfayı tarayıcınızda çalıştırın. Sayfa belirttiğiniz Xbox oyuncu kartı görüntüler.

    ![Resim 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
