---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: "Güvenlik Kılavuzu ASP.NET Web API 2 OData | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 799e2a0c742b545acf3b5cd27531d734aa7def80
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a>Güvenlik Kılavuzu ASP.NET Web API 2 OData
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

Bu konuda bir veri kümesi OData aracılığıyla gösterme verirken düşünmesi gereken güvenlik sorunlardan bazıları açıklanmaktadır.

## <a name="edm-security"></a>EDM güvenlik

Sorgu semantiği, varlık veri modeli üzerinde (EDM), temel alınan model türleri değil temel alır. EDM bir özelliği dışarıda bırakabilirsiniz ve sorgu için görünür olmaz. Örneğin, bir çalışan türü maaş özelliğiyle modelinizi içerdiğini varsayın. Bu özellik istemcilerden gizlemek için EDM ayarlayacağım isteyebilirsiniz.

EDM özelliğinden exlude için iki yolu vardır. Ayarlayabileceğiniz **[IgnoreDataMember]** model sınıfı özelliğinde özniteliği:

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

Ayrıca özelliği EDM program aracılığıyla kaldırabilirsiniz:

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a>Sorgu güvenlik

Kötü amaçlı veya naïve istemci yürütmek için çok uzun süren bir sorgu oluşturmak mümkün olabilir. Kötü durumda bu, hizmete erişimi kesintiye uğratabilir.

**[Queryable]** ayrıştırır, doğrular ve sorgu geçerli bir eylem filtresi bir özniteliktir. Filtre sorgu seçeneklerini bir LINQ ifadesine dönüştürür. OData denetleyicisi döndüğünde bir **Iqueryable** türü, **Iqueryable** LINQ sağlayıcısı LINQ ifadesi bir sorgu dönüştürür. Bu nedenle, performans kullanılan LINQ sağlayıcısı ve aynı zamanda dataset ya da veritabanı şemanızı belirli özelliklerine bağlıdır.

ASP.NET Web API'de OData sorgu seçeneklerini kullanma hakkında daha fazla bilgi için bkz: [destekleyen OData sorgu seçeneklerini](supporting-odata-query-options.md).

Tüm istemciler (örneğin, bir kuruluş ortamında) güvenilir olduğunu biliyorsanız veya Veri kümenizi küçükse, sorgu performansı sorunu olmayabilir. Aksi takdirde, aşağıdaki önerileri göz önünde bulundurmalısınız.

- Hizmetinizi çeşitli sorguları test edin ve DB profil.
- Büyük bir veri kümesinin tek bir sorguda döndürme önlemek sunucu tabanlı disk belleği, etkinleştirin. Daha fazla bilgi için bkz: [Server-Driven disk belleği](supporting-odata-query-options.md#server-paging). 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- $Filter ve $orderby gerekiyor mu? Bazı uygulamalar disk belleği, $top ve $skip kullanarak istemci izin ver, ancak diğer sorgu seçeneklerini devre dışı bırakın. 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- Kümelenmiş bir dizin özelliklerinde $orderby sınırlama göz önünde bulundurun. Kümelenmiş bir dizin olmadan büyük verileri sıralama yavaştır. 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- En fazla düğüm sayısı: **MaxNodeCount** özelliği **[Queryable]** $filter sözdizimi ağacı izin verilen en büyük sayı düğümleri ayarlar. Varsayılan değer 100'dür, ancak çok sayıda düğümü derlemek yavaş olabilir çünkü daha düşük bir değere ayarlamak isteyebilirsiniz. LINQ nesnelerine (yani, bir ara LINQ sağlayıcısı kullanmadan bellekte bir koleksiyonda LINQ sorgularını) kullanıyorsanız, bu özellikle doğrudur. 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- Bunlar yavaş olabilir olarak any() ve all() işlevlerini devre dışı bırakın. 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- Herhangi bir dize özelliği büyük dizeleri & #8212for örnek, bir ürün açıklaması veya blog girdisi & # dize işlevleri devre dışı bırakma 8212consider içeriyorsa. 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- Gezinti özellikleri filtreleme vermemek göz önünde bulundurun. Gezinti özellikleri filtreleme veritabanı şemasına göre yavaş olabilir bir birleştirme neden olabilir. Aşağıdaki kod, gezinti özellikleri filtreleme engelleyen bir sorgu Doğrulayıcı gösterir. Sorgu doğrulayıcıları hakkında daha fazla bilgi için bkz: [sorgu doğrulama](supporting-odata-query-options.md#query-validation). 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- Veritabanınız için özelleştirilmiş bir doğrulayıcı yazarak $filter sorguları kısıtlamayı göz önünde bulundurun. Örneğin, bu iki sorgular göz önünde bulundurun: 

    - Tüm filmler son adı 'A' ile başlayan aktörler ile.
    - 1994'te yayımlanan tüm filmler.

    Film aktörler tarafından dizinlenen sürece, ilk sorguyu filmler tam listesini taramak için veritabanı altyapısı gerektirebilir. İkinci sorguyu kabul edilebilir gelirken, olduğunu varsayarak filmler yayım yılına göre dizinlenir.

    Aşağıdaki kod "ReleaseYear" ve "Title" özellikleri ancak başka hiçbir özellik filtreleme izin veren bir doğrulayıcı gösterir.

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- Genel olarak, gerek duyduğunuz hangi $filter işlevleri göz önünde bulundurun. İstemcilerinizi $filter, tam anlamlılık ihtiyacınız yoksa, izin verilen işlevler sınırlayabilirsiniz.
