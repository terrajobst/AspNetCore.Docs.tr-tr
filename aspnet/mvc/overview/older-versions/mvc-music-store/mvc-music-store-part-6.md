---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: '6. Bölüm: Model doğrulama için veri ek açıklamalarını kullanma | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 6. bölüm veri ek açıklamaları Model için V incelemektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: ca0ac2ca909f838f1c91e6cc01b8aafa90c0b193
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397664"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>6. Bölüm: Kullanmak için veri ek açıklamaları Model doğrulama
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 6. bölüm veri ek açıklamaları Model doğrulama için incelemektedir.


Önemli bir sorun bizim oluşturma ve düzenleme formlarla sahibiz: herhangi bir doğrulama yaptıkları değil. Fiyat alanında harflerini yazın ya da gerekli alanları boş bırakın gibi işlemler yapabileceğimiz ve görüyoruz ilk veritabanından hatadır.

Kolayca doğrulama uygulamamıza bizim model sınıfları için veri ek açıklamaları ekleyerek ekleyebiliriz. Veri ek açıklamaları tanımlamak sunduğumuz modeli özelliklerine uygulanan istiyoruz kuralları olanak tanır ve ASP.NET MVC uygulamadan ve kullanıcılarımız için uygun iletileri görüntülemeyi ilgileniriz.

## <a name="adding-validation-to-our-album-forms"></a>Bizim albüm formları için doğrulama ekleme

Aşağıdaki veri ek açıklama öznitelikleri kullanacağız:

- **Gerekli** – gerekli bir alan özelliği belirtir
- **DisplayName** – form alanlarını ve doğrulama iletilerinin kullanılan istiyoruz metni tanımlar
- **StringLength** – tanımlayan bir dize alanı için en fazla uzunluk
- **Aralık** – maksimum ve minimum değer için sayısal bir alan sağlar.
- **Bağlama** – listeler hariç tutma veya parametresi veya form değerleri, model özellikleri bağlanırken eklemek için alanlar
- **ScaffoldColumn** – Düzenleyicisi formlarından alanları gizleme sağlar

*Not: veri ek açıklama öznitelikleri kullanarak Model doğrulama hakkında daha fazla bilgi için MSDN belgelerine bakın.*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Albüm sınıfını açın ve aşağıdakileri ekleyin *kullanarak* en.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

Ardından, aşağıda gösterildiği gibi görünen ve doğrulama öznitelikleri eklemek için özelliklerini güncelleştirir.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Biz var. kullanmadığınız esnada özelliğin ayrıca tarz ve sanatçı sanal özelliklerine değiştirdik. Bu, yavaş bunları gerektiği şekilde yük Entity Framework sağlar.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Bu öznitelikler, albüm modelimizi eklenen sonra bizim oluşturma ve düzenleme ekranı hemen başlamak alanları doğrulama ve görünen adlarını kullanarak biz (örneğin albüm resim URL'si yerine AlbumArtUrl) seçtiniz. Uygulamayı çalıştırmak ve /StoreManager/Create için göz atın.

![](mvc-music-store-part-6/_static/image1.png)

Ardından, biz bazı doğrulama kuralı ihlal. Bir fiyat 0 girin ve başlık boş bırakın. Biz Oluştur düğmesine tıkladığınızda, formun tanımladığımız hangi alanların doğrulama kuralları karşılamayan gösteren bir doğrulama hata iletisi ile görüntülenen göreceğiz.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>İstemci tarafı doğrulama sınaması

Sunucu tarafı doğrulama bir uygulama açısından oldukça önemli çünkü kullanıcılar istemci tarafı doğrulama atlayabilir. Ancak, yalnızca sunucu tarafı doğrulama uygulayan Web forms üç önemli sorunlar sergiler.

1. Kullanıcının formun gönderilen, sunucu üzerinde doğrulanır ve kendi tarayıcıya gönderilecek yanıt için beklemek gerekir.
2. Artık doğrulama kuralları geçirir, böylece bir alan düzeltirken kullanıcıya anında geri bildirim elde edemez.
3. Biz, kullanıcının tarayıcı yararlanarak yerine doğrulama mantığını gerçekleştirmesi için sunucu kaynağı israfına yol.

Neyse ki, ASP.NET MVC 3 iskele şablonlarında yerleşik istemci tarafı doğrulama olmadan hiçbir ek iş gerek vardır.

Doğrulama iletisini hemen kaldırılır başlık alanında tek bir harf yazarak doğrulama gereksinimlerini karşılar.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-5.md)
> [İleri](mvc-music-store-part-7.md)
