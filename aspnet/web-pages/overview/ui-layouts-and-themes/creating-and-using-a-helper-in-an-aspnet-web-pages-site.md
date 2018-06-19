---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Oluşturma ve bir Yardımcısını kullanarak bir ASP.NET Web sayfaları (Razor) Site | Microsoft Docs
author: tfitzmac
description: Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar. Bir yardımcı kod ve performans için biçimlendirme içeren yeniden kullanılabilir bir bileşenidir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573306"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Oluşturma ve bir ASP.NET Web sayfaları (Razor) sitesinde bir Yardımcısını kullanma
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar. A *yardımcı* kod ve sıkıcı veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.
> 
> **Öğrenecekleriniz:** 
> 
> - Nasıl oluşturmak ve basit bir Yardımcısı'nı kullanabilirsiniz.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `@helper` Sözdizimi.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="overview-of-helpers"></a>Yardımcıları genel bakış

Siteniz farklı sayfalarında aynı görevleri gerçekleştirmeniz gerekirse, yardımcıyı kullanabilirsiniz. ASP.NET Web sayfaları Yardımcıları çeşitli içerir ve indirmek ve yüklemek çok daha fazlasını vardır. (Yerleşik ASP.NET Web Pages'de Yardımcıları listesini listelenen [ASP.NET API hızlı başvuru](https://go.microsoft.com/fwlink/?LinkId=202907).) Varolan Yardımcıları hiçbiri gereksinimlerinizi karşılıyorsa, kendi yardımcı oluşturabilirsiniz.

Yardımcıyı, ortak bir kod bloğunu birden çok sayfada kullanmanızı sağlar. Sayfanızda genellikle normal paragraflar dışında ayarlanmış bir not öğesi oluşturmak istediğinizi varsayalım. Not belki de oluşturulur bir `<div>` öğesi, bir sınır içeren bir kutu olarak biçimlendirilmiş. Not görüntülemek istediğiniz her zaman bir sayfaya bu aynı biçimlendirme eklemek yerine, bir yardımcı işaretleme paketleyebilirsiniz. Not tek satırlık bir kod ile birlikte daha sonra ekleyebileceğiniz ihtiyacınız herhangi bir yere.

Böyle bir Yardımcısını kullanarak kodu her sayfalarınızın daha basit ve okuması daha kolay hale getirir. Notlar nasıl göründüğünü değiştirmeniz gerekiyorsa, tek bir yerde biçimlendirme değişebildiğinden, ayrıca, sitenizin korumak kolaylaştırır.

## <a name="creating-a-helper"></a>Yardımcıyı oluşturma

Bu yordam yalnızca tanımlandığı gibi not oluşturur yardımcı oluşturulacağını gösterir. Bu basit bir örnektir, ancak özel yardımcı tüm biçimlendirme ve ihtiyacınız ASP.NET kodu içerebilir.

1. Web sitesinin kök klasöründe adlı bir klasör oluşturun *uygulama\_kod*. Bu ayrılmış bir klasör ASP.NET kodu yardımcıları gibi bileşenleri için burada koyabilirsiniz adıdır.
2. İçinde *uygulama\_kod* yeni bir klasör oluşturun *.cshtml* dosya ve adlandırın *MyHelpers.cshtml*.
3. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Kod kullanan `@helper` adlı yeni bir Yardımcısı bildirmek için sözdizimi `MakeNote`. Bu belirli yardımcı adlı bir parametre geçirmenize olanak verir `content` metin ve biçimlendirme bileşimini içerebilir. Not gövde kullanarak içine yardımcı bir dize ekler `@content` değişkeni.

    Dosya adlandırılır fark *MyHelpers.cshtml*, ancak yardımcı adlı `MakeNote`. Tek bir dosyaya birden çok özel Yardımcıları koyabilirsiniz.
4. Dosyayı kaydedin ve kapatın.

## <a name="using-the-helper-in-a-page"></a>Bir sayfa Yardımcısını kullanma

1. Kök klasöründe adlı yeni boş bir dosya oluşturun *TestHelper.cshtml*.
2. Dosyasına aşağıdaki kodu ekleyin:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Oluşturduğunuz Yardımcısı çağırmak için kullanırsınız `@` yardımcı olduğu bir nokta dosya adı ve ardından yardımcı adı gelir. (Birden çok klasörleri olsaydı *uygulama\_kod* klasörü, sözdizimini kullanabilirsiniz `@FolderName.FileName.HelperName` yardımcınız herhangi içinde çağırmak için klasör düzeyinde iç içe). Parantez içinde tırnak işaretleri içindeki eklediğiniz metin yardımcı Not web sayfasında bir parçası olarak görüntülenecek metindir.
3. Sayfayı kaydedin ve tarayıcıda çalıştırın. Adlı burada yardımcı Not öğesi sağ yardımcı oluşturur: iki paragraf arasında.

    ![Tarayıcı ve Yardımcısı, belirtilen metnin etrafında bir kutu koyan biçimlendirme nasıl oluşturulacağını sayfasını gösteren ekran görüntüsü.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Ek Kaynaklar


[Yatay menü Razor yardımcı olarak](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Bu blog girişi CAN Pope tarafından bir yatay menüsünün biçimlendirme, CSS ve kod kullanarak bir yardımcı nasıl oluşturulacağını gösterir.

[ASP.NET Web HTML5 yararlanarak sayfaları Yardımcıları WebMatrix ve ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Bu blog girişi Sam Abraham tarafından bir HTML5 işleyen yardımcıyı gösterir `Canvas` öğesi.

[Arasındaki farkı @Helpers ve @Functions webmatrix'te](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Bu blog girişi CAN Brind tarafından açıklar `@helper` sözdizimi ve `@function` sözdizimi ve ne zaman her kullanılır.
