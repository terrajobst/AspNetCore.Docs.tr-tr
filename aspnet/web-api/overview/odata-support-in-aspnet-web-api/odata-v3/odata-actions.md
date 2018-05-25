---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET Web API 2 OData eylemleri destekleyen | Microsoft Docs
author: MikeWasson
description: 'OData içinde eylemler kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur. Bazı eylemler kullanımları şunlardır: uygulama...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>ASP.NET Web API 2 OData eylemlerinin destekleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Odata'da, *Eylemler* kolayca CRUD işlemleri varlıklar üzerinde olarak tanımlanmamış olan sunucu tarafı davranışları eklemek için bir yoldur. Bazı eylemler kullanımları şunları içerir:
> 
> - Karmaşık işlemleri uygulama.
> - Aynı anda birden fazla varlıkların düzenleme.
> - Bir varlığın belirli özellikleri için yalnızca güncelleştirmelere izin verme.
> - Bir varlığı tanımlı değil sunucu bilgileri gönderme.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2
> - OData sürüm 3
> - Varlık Çerçevesi 6


## <a name="example-rating-a-product"></a>Örnek: ürün değerlendirme

Bu örnekte, ürünleri oranı ve her ürün için Ortalama Derecelendirmelerini kullanıma kullanıcıların izin vermek istiyoruz. Veritabanında, biz derecelendirmeleri, ürün için anahtarlı listesini depolar.

Entity Framework derecelendirmeleri temsil etmek için kullanabilir modeli şöyledir:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ancak biz POST istemcilere istemediğiniz bir `ProductRating` "Derecelendirme" koleksiyonuna nesne. Sezgisel, derecelendirme ürünleri koleksiyonuyla ilişkilendirilen ve istemcinin yalnızca derecelendirme değerini sonrası gerekir.

Bu nedenle, normal CRUD işlemleri kullanmak yerine, bir istemci çağırabileceği bir eylem bir ürün üzerinde tanımlarız. OData terminolojisinde eylemdir *bağlı* ürün varlıklara.

>Eylemler sunucu üzerinde yan etkileri olması. Bu nedenle, bunlar HTTP POST istekleri kullanılarak çağrılır. Eylemler, parametreler ve hizmet meta verilerde tanımlanan dönüş türleri olabilir. İstemci istek gövdesinde parametreleri gönderir ve sunucu yanıt gövdesi içinde dönüş değeri gönderir. "Oranı ürün" eylemi çağırmak için istemci bir POST aşağıdaki gibi bir URI gönderir:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

POST isteğini veriler yalnızca ürün derecelendirme bağlıdır:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Varlık veri modeli eylemde bildirme

Web API yapılandırmanızda eylem varlık veri modeli (EDM) ekleyin:

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Bu kod "RateProduct" Ürün varlıklar üzerinde gerçekleştirilen eylem olarak tanımlar. Ayrıca eylem aldığını bildiren bir **int** parametresi "Derecelendirme" adlı ve döndürür bir **int** değeri.

## <a name="add-the-action-to-the-controller"></a>Denetleyici için eylem ekleme

"RateProduct" eylemi ürün varlıklara bağlıdır. Eylem uygulamak için adlandırılmış bir yöntem eklemek `RateProduct` ürünleri denetleyicisine:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Yöntem adı EDM eylemde adıyla eşleşen dikkat edin. Yöntemi iki parametreye sahiptir:

- *anahtar*: derecelendirmek için ürün anahtarı.
- *parametreleri*: eylem parametre değerleri sözlüğü.

Varsayılan yönlendirme kuralları kullanıyorsanız, anahtar parametreyi "anahtar" olarak adlandırılmalıdır. Eklenmesi önemlidir **[FromOdataUri]** gösterildiği gibi öznitelik. Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarına kullanmak için Web API söyler.

Kullanım *parametreleri* sözlük eylem parametrelerini almak için:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

İstemci eylem parametrelerini doğru durumda gönderirse biçimi, değeri **ModelState.IsValid** doğrudur. Bu durumda, kullanabileceğiniz **ODataActionParameters** parametre değerlerini almak için Sözlük. Bu örnekte, `RateProduct` eylemde "Derecelendirme" adlı tek bir parametre.

## <a name="action-metadata"></a>Eylem meta verileri

Hizmet meta verilerini görüntülemek için /odata/$ meta verileri için bir GET isteği gönderin. Bildirir meta veri bölümü işte `RateProduct` eylem:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**Functionımport** öğesi eylem bildirir. Alanların çoğu kendinden açıklamalıdır, ancak iki belirtmeye değerinde şunlardır:

- **IsBindable** eylemi çağrılabilir hedef varlık üzerinde en az anlamına gelir sürenin bir kısmı.
- **Isalwaysbindable** eylem her zaman hedef varlık üzerinde çağrılacak anlamına gelir.

Bazı eylemler her zaman istemciler için kullanılabilir, ancak diğer eylemler varlık durumuna bağlı olabilir farktır. Örneğin, "Satın Al" eylemi tanımlamak varsayalım. Yalnızca stokta olan bir öğeyi satın alabilirsiniz. Madde hisse senedi dışında ise, bir istemci bu eylemi çağıramaz.

EDM tanımlarken **eylem** yöntemi her zaman bağlanabilirse bir eylem oluşturur:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Not-her zaman-bağlanabilirse eylemler hakkında konuşun (olarak da bilinir *geçici* Eylemler) Bu konuda daha sonra.

## <a name="invoking-the-action"></a>Eylemi Çağırma

Şimdi bir istemci bu eylem nasıl çağıracaktır görelim. Kimlikli Ürün 2 derecesi sağlamak istemcinin istediği varsayalım = 4. İstek gövdesi için JSON biçimini kullanarak bir örnek istek iletisi şudur:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Yanıt iletisi şudur:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Bir varlık kümesine bir eylem bağlama

Önceki örnekte eylem tek bir varlığa bağlıdır: tek bir ürün istemci derecelendirir. Varlık koleksiyonu için bir eylem de bağlayabilirsiniz. Yalnızca aşağıdaki değişiklikleri yapın:

Varlığın EDM içinde Ekle eylemi **koleksiyonu** özelliği.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Denetleyici yönteminin atlayın *anahtar* parametresi.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Şimdi istemci ürünleri varlık kümesini eylemini çağırır:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Toplama parametrelerini olan eylemler

Eylemler değerler koleksiyonu ele parametrelerine sahip olabilirsiniz. EDM içinde kullanmak **CollectionParameter&lt;T&gt;**  parametresi bildirmek için.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Bu "bir koleksiyonunu alır derecelendirmeleri" adlı bir parametre bildirir **int** değerleri. Denetleyici yönteminin, parametre değerinin almaya devam **ODataActionParameters** nesne ancak şimdi değeri olan bir **ICollection&lt;int&gt;**  değeri:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Geçici Eylemler

Eylem her zaman kullanılabilir olacak şekilde "RateProduct" örnekte, kullanıcıların her zaman bir ürün oranı. Ancak bazı eylemler varlık durumuna bağlıdır. Örneğin, bir video kiralama hizmetinde "CheckOut" eylem her zaman kullanılabilir değil. (Bu videonun bir kopyasını kullanılabilir olup olmadığını bağlıdır.) Bu tür eylem olarak adlandırılan bir *geçici* eylem.

Hizmet meta verilerini geçici bir eylemi var. **Isalwaysbindable** false eşittir. Meta veriler şu şekilde görünmesi için varsayılan değeri gerçekte şudur:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Bu neden önemlidir burada'nın: sunucu bir eylem geçici ise, eylem kullanılabilir olduğunda istemci söyleyin gerekir. Bu varlıkta eylem bağlantısı dahil olmak üzere yapar. Film varlık için örnek aşağıda verilmiştir:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

"#CheckOut" özelliği CheckOut eylemi için bir bağlantı içerir. Eylem kullanılamıyorsa, sunucunun bağlantıyı atlar.

EDM geçici bir eylemde bildirmek için çağrı **TransientAction** yöntemi:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Ayrıca, belirli bir varlık için bir eylem bağlantısı döndüren bir işlev sağlamanız gerekir. Bu işlev aranarak **HasActionLink**. Lambda ifadesi işlevi yazabilirsiniz:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Eylem varsa, lambda ifadesi bir bağlantı eyleme döndürür. Varlık serileştiren olduğunda OData serileştirici Bu bağlantı içerir. Eylem kullanılamadığında işlevi döndürür `null`.

## <a name="additional-resources"></a>Ek Kaynaklar

[OData Eylemler örnek](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
