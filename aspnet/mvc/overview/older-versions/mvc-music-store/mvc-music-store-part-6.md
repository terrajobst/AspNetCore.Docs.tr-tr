---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: "Bölüm 6: Veri ek açıklamaları Model doğrulama için kullanarak | Microsoft Docs"
author: jongalloway
description: "Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 6 V modeli için veri ek açıklamaları kullanılarak kapsayan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Bölüm 6: Kullanmak için veri ek açıklamaları Model doğrulama
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 6 veri ek açıklamaları Model doğrulama için kullanmayı ele alır.


Bizim oluşturma ve düzenleme formlarla önemli sorun sunuyoruz: tüm doğrulama yaptıkları değil. Biz gerekli alanları boş veya harflerini yazın fiyat alanında bırakın gibi şeyler yapabilir ve göreceğiz ilk veritabanından hatasıdır.

Biz kolayca doğrulama, bizim model sınıflarına veri ek açıklamaları ekleyerek uygulamamız için ekleyebilirsiniz. ASP.NET MVC koymadan ve kullanıcılarımızın uygun iletilerin görüntüleme ilgilenebilmek ve veri ek açıklamaları bize bizim modeli özelliklerine uygulanan istiyoruz kurallar tanımlamak izin verin.

## <a name="adding-validation-to-our-album-forms"></a>Bizim albüm formlara doğrulama ekleme

Aşağıdaki veri ek açıklamasını öznitelikleri kullanacağız:

- **Gerekli** – özelliği gerekli bir alan olduğunu gösterir
- **DisplayName** – form alanları ve doğrulama ileti üzerinde kullanılan istiyoruz metni tanımlar
- **StringLength** – bir dize alanı için en fazla uzunluk tanımlar
- **Aralık** – sayısal bir alan için bir maksimum ve minimum değeri verir
- **Bağlama** – listeler hariç tutma veya parametresi ya da form değerleri model özelliklerine bağlanırken eklemek için alanları
- **ScaffoldColumn** – Düzenleyicisi form alanlardan gizleme sağlar

*Not: veri ek açıklamasını özniteliklerini kullanarak Model doğrulama hakkında daha fazla bilgi için MSDN belgelerine bakın.*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Albüm sınıfı açın ve aşağıdakileri ekleyin *kullanarak* deyimleri dön.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Ardından, aşağıda gösterildiği gibi görünen ve doğrulama öznitelikleri eklemek için özelliklerini güncelleştirir.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Var ki, ancak biz de tarz ve sanatçı sanal özellikleri değiştirdiniz. Bu, yavaş bunları gerektiği şekilde yük Entity Framework sağlar.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Bu öznitelikler albüm modelimizi eklenmiş sonra bizim oluşturma ve düzenleme ekran hemen başlamak alanları doğrulama ve görünen adları kullanarak biz (örneğin albüm resim URL'si AlbumArtUrl yerine) seçtiniz. Uygulamayı çalıştırın ve /StoreManager/Create için göz atın.

![](mvc-music-store-part-6/_static/image1.png)

Ardından, biz bazı doğrulama kuralları bölün. Bir fiyat 0 girin ve başlık boş bırakın. Biz Oluştur düğmesine tıkladığınızda, biz tanımladığınız hangi alanların doğrulama kuralları karşılamayan gösteren bir doğrulama hata iletisi ile görüntülenen form göreceğiz.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>İstemci tarafı doğrulama test etme

Kullanıcıların istemci tarafı doğrulama atlayabilir için sunucu tarafında doğrulama bir uygulama açısından çok önemlidir. Ancak, sunucu tarafında doğrulama yalnızca uygulayan Web forms üç önemli sorunlar sergiler.

1. Formun gönderilen, sunucuda doğrulanması ve bunların tarayıcıya gönderilecek yanıt için beklemek kullanıcının vardır.
2. Doğrulama kuralları şimdi sağlayacak şekilde bir alan düzeltirken kullanıcı anında geri bildirim alma değil.
3. Biz, kullanıcının tarayıcısının yararlanarak yerine doğrulama mantığını gerçekleştirmek için sunucu kaynaklarını israfı.

Neyse ki, ASP.NET MVC 3 iskele şablonları, yerleşik istemci tarafı doğrulama doğabilecek hiçbir ek iş gerek vardır.

Doğrulama ileti hemen kaldırılır başlık alanında tek bir harf yazarak doğrulama gereksinimlerini karşılar.

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
[Önceki](mvc-music-store-part-5.md)
[sonraki](mvc-music-store-part-7.md)
