---
uid: web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
title: Sosyal ağ ekleme için ASP.NET Web sayfaları (Razor) siteler | Microsoft Docs
author: tfitzmac
description: Bu bölümde, sitenize sosyal ağ hizmetleriyle tümleştirmeye yönelik açıklanmaktadır. Bu bölümde, Web sitenizi yer işareti/bağlantı kişilere öğreneceksiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 03c342f9-b35c-4d7c-b9ed-cd9aaaffedb6
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/13-adding-social-networking-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 4f987c9022056ddfa3cdcac746f688f562d9003e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398407"
---
<a name="adding-social-networking-to-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) siteleri sosyal ağ ekleme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, sosyal ağ bağlantıları Facebook, Twitter, Reddit ve Digg için bir ASP.NET Web sayfaları (Razor) Web sitesi sayfalarına ekleme ve Twitter feed'lerine ve Xbox oyuncu kartları Gravatar görüntülerini ekleme açıklanmaktadır.
> 
> Öğrenecekleriniz:
> 
> - Yer işareti/sitenizi bağlantı kişilere nasıl.
> - Bir Twitter akışı ekleme.
> - Bir Facebook ekleme **gibi** sayfalara düğmesi.
> - Gravatar.com görüntülerin nasıl.
> - Sitenizde bir Xbox oyuncu kartını görüntülemek nasıl.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 2
> - ASP.NET Web yardımcı kitaplık (NuGet paketi)
>   
> 
> Bu öğretici ile ASP.NET Web Pages 3, ASP.NET Web Yardımcısı kitaplığını kullanan bölümleri dışında da çalışır.


<a id="Linking_Your_Website"></a>
## <a name="linking-your-website-on-social-networking-sites"></a>Sosyal ağ sitelerine sitenizde bağlama

Kişiler, sitenizdeki bir şey istiyorsanız, bunlar genellikle arkadaşlarınızla paylaşmak istiyorsunuz. Bu kişiler, Digg, Reddit, Facebook, Twitter veya benzer siteleri sayfada paylaşmak için tıklayabileceği karakterleri (simge) görüntüleyerek kolayca yapabilirsiniz.

Bu karakterleri görüntülenecek ekleme `LinkSharecode` yardımcı olacak bir sayfa. Sayfanızı ziyaret eden kişiler, bireysel bir karakter tıklayabilirsiniz. Bunlar, sosyal ağ sitesine bir hesabınız varsa, bunlar sitede ardından sayfanıza bir bağlantı gönderebilirsiniz.

![Resim 1](13-adding-social-networking-to-your-web-site/_static/image1.jpg)

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), zaten halen - eklemediyseniz adlı bir sayfa oluşturun *ListLinkShare.cshtml* ekleyin Aşağıdaki biçimlendirmede:

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample1.cshtml)]

    Bu örnekte, zaman `LinkShare` Yardımcısını çalıştırmaları, sayfa başlığı, sırayla sosyal ağ sitesine sayfa başlığının geçen bir parametre olarak geçirilir. Ancak, istediğiniz herhangi bir dize içinde geçirebiliriz. Bu örnek ayrıca listesinde içermek için hangi sosyal ağ sitelerine belirtir. İlgili sitenize sosyal ağ sitelerine belirtebilirsiniz.
2. Çalıştırma *ListLinkShare.cshtml* sayfasını bir tarayıcıda. (Emin sayfanın içinde seçili **dosyaları** çalıştırmadan önce çalışma alanı.)
3. Bir yazıtipi için Kayıtlısınız sitelerden birine tıklayın. Bağlantı sayfasına bağlantı paylaşabileceğiniz seçili sosyal ağ sitesinde götürür. Reddit bağlantıya tıklarsanız, örneğin, adresine yönlendirilirsiniz `submit to reddit` Reddit sitesinde sayfasında.

     ![Resim 2](13-adding-social-networking-to-your-web-site/_static/image2.jpg)

<a id="Adding_a_Twitter_Feed"></a>
## <a name="adding-a-twitter-feed"></a>Bir Twitter ekleme akışı

Twitter API'si geçerli sürümü ile uyumlu bir Twitter Yardımcısı'nı kullanma hakkında daha fazla bilgi için bkz: [Twitter Yardımcısı](../ui-layouts-and-themes/twitter-helper.md). Bu örnekte, kolayca birçok sayfalarından kodunu yeniden kullanabilmesi kendi Yardımcısı yazma işlemi gösterilmektedir.

<a id="Displaying_a_Facebook_Button"></a>
## <a name="displaying-a-facebook-quotlikequot-button"></a>Bir Facebook görüntüleme &quot;gibi&quot; düğmesi

Bazı durumlarda, en iyi seçenek, üzerinde bir yardımcı güvenmek yerine sosyal ağ sağlayıcısına doğrudan kod elde etmektir. Sosyal ağ sağlayıcısına seçeneklerini yardımcı güncelleştirilir daha hızlı güncelleştiriyorsa bu özellikle doğrudur.

Facebook özellikleri (örneğin, sıra Beğen düğmesi) sitenize eklemek için kod parçacıkları'nden alabilirsiniz [developers.facebook.com](https://developers.facebook.com/) site. Facebook sitede, site için ilgili kod parçacığı oluşturmak için kendi araçlarını kullanın.

Aşağıdaki vurgulanmış kodu developers.facebook.com sitesinde düğmesi gibi aracından alınan kodudur. Kendi uygulama kimliğini sağlamanız gerekir

[!code-html[Main](13-adding-social-networking-to-your-web-site/samples/sample2.html?highlight=7-14,16-17)]

<a id="Rendering_a_Gravatar_Image"></a>
## <a name="rendering-a-gravatar-image"></a>Gravatar görüntü işleme

A *Gravatar* (bir &quot;genel tanınan avatar&quot;) birden çok Web sitelerinde avatarınız kullanılabilecek bir görüntü &#8212; diğer bir deyişle, gösteren görüntü. Örneğin, bir Gravatar bir kişi bir forum gönderisi, blog açıklamasında tanımlamak ve benzeri. (Gravatar Web sitesinden kendi Gravatar kaydedebilirsiniz [ http://www.gravatar.com/ ](http://www.gravatar.com/).) Web sitenizde kişilerin adları veya e-posta adreslerinin yanındaki görüntüleri görüntülemek istiyorsanız, Gravatar yardımcıyı kullanabilirsiniz.

Bu örnekte, kendiniz temsil eden tek bir Gravatar kullanıyorsanız. Bir Gravatar kullanmak için başka bir sitenizde'na kaydederken Gravatar adresini belirtin kişilere yoludur. (Kaydetmek kişilere öğrenebilirsiniz [güvenlik ekleme ve bir ASP.NET Web sayfaları sitesinde üyelik](https://go.microsoft.com/fwlink/?LinkId=202904).) Kullanıcı bilgilerini görüntüleme olduğunda, ardından yalnızca Gravatar kullanıcının adını burada görüntülemek için ekleyebilirsiniz.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir web sayfası oluşturma *Gravatar.cshtml*.
3. Aşağıdaki biçimlendirmede dosyaya ekleyin: 

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample3.cshtml)]

    `Gravatar.GetHtml` Yöntemi Gravatar görüntü sayfasında görüntüler. Görüntü boyutunu değiştirmek için ikinci parametre olarak bir sayı içerebilir. Varsayılan boyutu 80'dir. 80'den az yapma görüntüyü daha küçük sayılar. Görüntü 80 büyük sayılar büyütün.
4. İçinde `Gravatar.GetHtml` yöntemlerini değiştirin `<Your Gravatar account here>` Gravatar hesabınız için kullandığınız e-posta adresi. (Bir Gravatar hesabınız yoksa, mu kişinin e-posta adresini kullanabilirsiniz.)
5. Sayfayı tarayıcınızda çalıştırın. Sayfa, belirttiğiniz e-posta adresi için iki Gravatar görüntülerini görüntüler. İkinci görüntü, daha küçük. 

    ![Resim 4](13-adding-social-networking-to-your-web-site/_static/image3.jpg)

<a id="Displaying_an_Xbox_Gamer_Card"></a>
## <a name="displaying-an-xbox-gamer-card"></a>Xbox oyuncu kart görüntüleme

Kişiler Microsoft Xbox çevrimiçi oyun, her kullanıcının benzersiz bir kimliğe sahiptir. İstatistikleri kendi saygınlığı, oyuncu puanı gösterir ve oyunlar oynatılan bir oyuncu kart biçiminde her oyuncu için tutulur. Xbox oyuncu kullanıyorsanız, oyuncu kartınız sitenizde sayfalarında kullanarak gösterebilirsiniz `GamerCard` Yardımcısı.

1. ASP.NET Web Yardımcıları kitaplığı açıklandığı Web sitenize ekleyin [yükleme Yardımcıları bir ASP.NET Web sayfaları sitesinde](https://go.microsoft.com/fwlink/?LinkId=252372), henüz yapmadıysanız.
2. Adlı yeni bir sayfa oluşturun *XboxGamer.cshtml* ve aşağıdaki işaretlemeyi ekleyin.

    [!code-cshtml[Main](13-adding-social-networking-to-your-web-site/samples/sample4.cshtml)]

    Kullandığınız `GamerCard.GetHtml` özelliği görüntülenecek oyuncu kart için bir diğer ad belirtin.
3. Sayfayı tarayıcınızda çalıştırın. Sayfa belirttiğiniz Xbox oyuncu kartı görüntüler.

    ![Resim 5](13-adding-social-networking-to-your-web-site/_static/image4.jpg)
