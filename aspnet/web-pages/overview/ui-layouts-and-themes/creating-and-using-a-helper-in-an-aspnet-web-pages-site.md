---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Oluşturma ve bir Yardımcısı kullanarak bir ASP.NET Web sayfaları (Razor) sitesinde | Microsoft Docs
author: Rick-Anderson
description: Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar. Bir yardımcı kod ve iyileştirilmiş işaretlemede içeren yeniden kullanılabilir bir bileşen olan...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 3b54ba8a35a354eb0562a47866cd8b5dcb66bc86
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021514"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Oluşturma ve bir ASP.NET Web sayfaları (Razor) sitesinde bir Yardımcısını kullanma
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, bir ASP.NET Web sayfaları (Razor) Web sitesinde bir yardımcı oluşturulacağını açıklar. A *Yardımcısı* kod ve yorucu bir süreç veya karmaşık olabilir bir görevi gerçekleştirmek için biçimlendirme içeren yeniden kullanılabilir bir bileşendir.
> 
> **Öğrenecekleriniz:** 
> 
> - Nasıl oluşturmak ve basit bir Yardımcısı'nı kullanın.
> 
> Bu makalede sunulan ASP.NET özellikleri şunlardır:
> 
> - `@helper` Söz dizimi.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


## <a name="overview-of-helpers"></a>Yardımcıları genel bakış

Aynı görevleri, sitedeki farklı sayfalar gerçekleştirmeniz gerekiyorsa yardımcıyı kullanabilirsiniz. ASP.NET Web sayfaları içeren bir dizi yardımcıları ve indirip yükleme çok daha fazlası vardır. (Bir listesi yerleşik ASP.NET Web Pages'de yardımcıların listelenen [ASP.NET API hızlı başvurusu](https://go.microsoft.com/fwlink/?LinkId=202907).) Mevcut Yardımcıları hiçbiri gereksinimlerinizi karşılamıyorsa, kendi Yardımcısı oluşturabilirsiniz.

Yardımcı, ortak bir kod bloğu birden çok sayfada kullanmanızı sağlar. Sayfanızın, genellikle normal paragraflar dışında ayarlanmış bir not öğesi oluşturmak istediğinizi varsayalım. Belki Not olarak oluşturulan bir `<div>` öğesi, bir sınır içeren bir kutu olarak biçimlendirilmiş. Not görüntülemek istediğiniz her zaman bir sayfaya bu aynı biçimlendirme eklemek yerine, bir yardımcı işaretleme paketleyebilirsiniz. Daha sonra tek bir kod satırı ile Not ekleyebileceğiniz herhangi bir yere ihtiyacınız.

Böyle bir Yardımcısını kullanarak kodu her sayfalarınızın daha basit ve okuması daha kolay hale getirir. Notları nasıl görüneceğini değiştirmeniz gerekiyorsa, tek bir yerde işaretleme değişebildiğinden, ayrıca, sitenizin bakımı kolaylaştırır.

## <a name="creating-a-helper"></a>Bir yardımcı oluşturma

Bu yordam yalnızca tanımlandığı gibi bir not oluşturur yardımcı oluşturulacağını gösterir. Bu basit bir örnektir, ancak özel yardımcı herhangi bir işaretleme ve ihtiyaç duyduğunuz ASP.NET kod içerebilir.

1. Web sitesinin kök klasöründe adlı bir klasör oluşturun *uygulama\_kod*. Kod yardımcıları gibi bileşenlerin yere koyabilirsiniz ayrılmış bir klasör adı ASP.NET budur.
2. İçinde *uygulama\_kod* yeni bir klasör oluşturun *.cshtml* adlandırın ve dosya *MyHelpers.cshtml*.
3. Mevcut içeriğini aşağıdakiyle değiştirin:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Kod `@helper` adlı yeni bir Yardımcısı bildirmek için söz dizimi `MakeNote`. Bu belirli Yardımcısı adlı bir parametre olarak geçirmenize olanak tanır `content` metni ve biçimlendirmeyi bir birleşimini içerebilir. Yardımcı, Not gövdesi kullanarak bir dize ekler `@content` değişkeni.

    Dosya olarak adlandırıldığına dikkat edin *MyHelpers.cshtml*, ancak yardımcı adlı `MakeNote`. Tek bir dosyada birden çok özel Yardımcıları koyabilirsiniz.
4. Dosyayı kaydedin ve kapatın.

## <a name="using-the-helper-in-a-page"></a>Bir sayfa Yardımcısını kullanma

1. Adlı yeni bir boş dosyası kök klasöründe oluşturma *TestHelper.cshtml*.
2. Dosyaya aşağıdaki kodu ekleyin:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Oluşturduğunuz Yardımcısı çağırmak için kullanın `@` yardımcı olduğu bir nokta, dosya adının ardından ve yardımcı adı. (Birden çok klasör olsaydı *uygulama\_kod* klasörü, aşağıdaki söz dizimini kullanabilirsiniz `@FolderName.FileName.HelperName` yardımcınız herhangi içinde çağırmak için klasör düzeyinde iç içe geçmiş). Parantez içindeki tırnak işaretleri içindeki eklediğiniz yardımcı Not web sayfasındaki bir parçası olarak görüntülenecek metni metindir.
3. Sayfayı kaydedin ve bir tarayıcıda çalıştırın. Yardımcı adlı burada yardımcı Not öğesi hemen oluşturur: iki paragraflar arasındaki.

    ![Tarayıcı ve yardımcı belirtilen metin etrafına bir kutu koyar biçimlendirme nasıl oluşturulacağını sayfasını gösteren ekran görüntüsü.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Ek Kaynaklar


[Bir Razor yardımcı olarak yatay menü](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Bu blog girişi Mike Pope tarafından biçimlendirme, CSS ve kod kullanarak bir yardımcı yatay menü oluşturma işlemi gösterilmektedir.

[Yararlanarak HTML5'te ASP.NET Web sayfaları Yardımcıları WebMatrix ve ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Bu blog girişi Sam Abraham tarafından bir HTML5 işleyen bir yardımcı gösterir `Canvas` öğesi.

[Arasındaki fark @Helpers ve @Functions webmatrix'te](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Bu blog girişi Mike Brind tarafından açıklar `@helper` söz dizimi ve `@function` söz dizimi ve ne zaman her kullanılır.
