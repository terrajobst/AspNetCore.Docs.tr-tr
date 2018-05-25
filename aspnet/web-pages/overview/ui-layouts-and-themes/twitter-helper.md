---
uid: web-pages/overview/ui-layouts-and-themes/twitter-helper
title: Twitter Yardımcısı ASP.NET Web sayfaları ile | Microsoft Docs
author: tfitzmac
description: Bu konu ve uygulama Twitter Yardımcısı, WebMatrix 3 projenize eklemek nasıl gösterir. Twitter Yardımcısı kodunu içerir ve yardımcı çağrısının nasıl gerçekleştireceğini...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: c1a1244e-b9c8-42e6-a00b-8456a4ec027c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/twitter-helper
msc.type: authoredcontent
ms.openlocfilehash: 07d9c4d485c42b78a42c54c9740af5f67cb44763
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2018
---
<a name="twitter-helper-with-aspnet-web-pages"></a>ASP.NET Web sayfaları ile twitter Yardımcısı
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu konu ve uygulama Twitter Yardımcısı, WebMatrix 3 projenize eklemek nasıl gösterir. Twitter Yardımcısı kodunu içerir ve nasıl yardımcı yöntemler çağrılacağını gösterir.
> 
> Twitter.cshtml dosyası için bu kodu tarafından geliştirilmiştir **Tian Pan** Microsoft.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="introduction"></a>Giriş

Bu konuda, bir Twitter Yardımcısı uygulamanıza ekleyin ve yardımcı yöntemleri çağırmak için Razor sözdizimini kullanan gösterilmiştir. Twitter Yardımcısı Twitter düğmeleri ve pencere öğeleri, uygulamanızda eklemenizi kolaylaştırır. Bir Twitter pencere, bir kullanıcının zaman çizelgesi veya bir hashtag için arama sonuçlarını gibi kullanmak için önce oluşturmanız gerekir [Twitter'da pencere öğesi](https://twitter.com/settings/widgets). Pencere öğesi oluşturduktan sonra bir pencere öğesi kimliği alırsınız. Bu pencere öğesi kimliği pencere öğesi Göster yardımcı yöntemler çağırırken parametre olarak geçirin.

Bu konu, Twitter API'si 1.1 sürümünü yazıldı. Projenize doğrudan Twitter Yardımcısı kodu ekleyerek, Twitter API'si değişirse yardımcı kodu güncelleştirebilirsiniz.

WebMatrix yükleme hakkında daha fazla bilgi için bkz: [Tanıtımı ASP.NET Web sayfaları Başlarken 2 -](../getting-started/introducing-aspnet-web-pages-2/getting-started.md).

## <a name="add-twitter-helper-to-your-project"></a>Twitter Yardımcısı projenize ekleyin

Twitter Yardımcısı eklemek için ilk olarak, adında bir klasör ekleyin **uygulama\_kod** projenize. Ardından, adlı bir dosya oluşturun **Twitter.cshtml**.

![App_Code klasörü](twitter-helper/_static/image1.png)

Twitter.cshtml varsayılan kodu aşağıdaki kodla değiştirin.

[!code-cshtml[Main](twitter-helper/samples/sample1.cshtml)]

## <a name="call-twitter-methods-from-your-web-pages"></a>Web sayfalarından twitter yöntemlerini çağırın

Aşağıdaki örnek projenizde sayfasından Twitter Yardımcısı yöntemleri kullanmayı gösterir. Projenizde, gereksinimlerinize uygun değerlerle parametre değerlerini değiştirmeniz isteyeceksiniz. Yöntemleri nasıl çalıştığını keşfetmek için sağlanan pencere kimlikleri kullanabilirsiniz, ancak kendi pencere öğelerinizi projeniz için oluşturmak istersiniz.

Aşağıda gösterilen parametreler hepsi gereklidir. İsteğe bağlı parametreler düğmesini veya pencere öğesi nasıl görüntüleneceğini özelleştirmek için kullanılır. Örneğin, izleyin düğmesi izlemek için kullanıcı adı yalnızca gerektiriyor, ancak örnek followers sayısını içerir ve nasıl düğmesi ve dil boyutunu belirtmek nasıl gösterir.

[!code-html[Main](twitter-helper/samples/sample2.html)]

## <a name="see-the-results"></a>Sonuçları görüntüleyin

Yukarıdaki kod aşağıdaki düğmeler ve pencere öğeleri oluşturur. Bu düğmelerin ve pencere öğeleri tam işlevsel, ekran görüntüleri değil. Dil parametresi olarak ayarlanmış olduğu için İspanyolca izleyin düğmesi görüntülenir **es**.

### <a name="follow-button"></a>Düğme izleyin

[İzleyin @aspnet)](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="tweet-button"></a>Tweet düğmesi

[Tweet](https://twitter.com/share)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + '://platform.twitter.com/widgets.js'; fjs.parentNode.insertBefore(js, fjs); } }(document, 'script', 'twitter-wjs');</script>

### <a name="user-timeline-profile"></a>Kullanıcı zaman çizelgesi (Profil)

[Tweet'leri tarafından @aspnet](https://twitter.com/aspnet)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="favorites"></a>Sık Kullanılanlar

[Sık kullanılan Tweet'leri tarafından @Microsoft](https://twitter.com/Microsoft/favorites)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="list"></a>List

[Gelen tweet'leri @Microsoft/MS \_tüketici\_bantlar](https://twitter.com/microsoft/ms-consumer-brands/)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>

### <a name="search"></a>Ara

[İlgili tweet'leri &quot;#asp.net&quot;](https://twitter.com/search?q=%23asp.net)<script>!function (d, s, id) { var js, fjs = d.getElementsByTagName(s)[0], p = /^http:/.test(d.location) ? 'http' : 'https'; if (!d.getElementById(id)) { js = d.createElement(s); js.id = id; js.src = p + "://platform.twitter.com/widgets.js"; fjs.parentNode.insertBefore(js, fjs); } }(document, "script", "twitter-wjs");</script>
