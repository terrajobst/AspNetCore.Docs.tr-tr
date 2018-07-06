---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET Web API 2 OData eylemleri destekleyen | Microsoft Docs
author: MikeWasson
description: 'OData, varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yol eylemlerdir. Bazı eylemler kullanımları şunlardır: uygulama...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837778"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>ASP.NET Web API 2 OData eylemleri destekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Odata'da, *eylemleri* varlıkları CRUD işlemleri olarak kolayca tanımlanmayan bir sunucu tarafı davranışları eklemek için bir yol sağlar. Bazı eylemler kullanımları şunlardır:
> 
> - Karmaşık işlemleri uygulama.
> - Çeşitli varlıklar aynı anda işleniyor.
> - Güncelleştirmeleri yalnızca belirli özellikleri bir varlığın izin verme.
> - Bir varlıkta tanımlı değil sunucu bilgileri gönderiliyor.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2
> - OData sürüm 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>Örnek: ürün değerlendirme

Bu örnekte, ürünleri oranı ve her ürün için ortalama derecelendirme sunarsınız kullanıcılara bildirmek istiyoruz. Derecelendirme, ürün anahtarlı listesini veritabanında depolarız.

Entity Framework derecelendirmelerini temsil etmek için kullanabilir model şu şekildedir:

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

Ancak biz posta istemcilerine istemiyorsanız bir `ProductRating` "Derecelendirmeleri" koleksiyonu için nesne. Sezgisel, derecelendirme ürünleri koleksiyonla ilişkilidir ve istemci yalnızca derecelendirme değeri gerekir.

Bu nedenle, normal CRUD işlemleri kullanmak yerine, bir istemci çağırabileceği bir eylem bir ürün üzerinde tanımlarız. OData terminolojisinde eylemdir *bağlı* ürün varlıkları için.

>Eylemler, sunucuda yan etkilere sahip. Bu nedenle, bunlar HTTP POST istekleri kullanılarak çağrılır. Eylemler, parametreler ve dönüş türleri, hizmet meta verilerde tanımlanan olabilir. İstek gövdesinde parametreleri istemciye gönderir ve sunucu yanıt gövdesinde döndürülen değer gönderir. "Oranı Product" eylemi çağırmak için posta istemci aşağıdaki gibi bir URI gönderir:

[!code-console[Main](odata-actions/samples/sample2.cmd)]

POST isteğinde verilerdir yalnızca ürün derecelendirmesi:

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>Varlık veri modeli eylemi bildirme

Web API yapılandırmanızda eylemi varlık veri modeli (EDM) ekleyin:

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

Bu kod, "RateProduct" Ürün varlıklar üzerinde gerçekleştirilecek bir eylem olarak tanımlar. Eylemin kullandığı da bildirir bir **int** parametresi "Sıralama" adlı ve döndüren bir **int** değeri.

## <a name="add-the-action-to-the-controller"></a>Denetleyici için eylem ekleme

"RateProduct" eylemi için ürün varlıkları bağlıdır. Eylemi uygulamak için adında bir yöntem ekleyin `RateProduct` ürünleri denetleyicisi:

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

Yöntem adı EDM eylemin adını eşleştiğine dikkat edin. Yöntemi iki parametreye sahiptir:

- *anahtar*: derecelendirmek için bir ürün anahtarı.
- *parametreleri*: eylem parametresi değerleri sözlüğü.

Varsayılan rota kuralları kullanıyorsanız, anahtar parametresi "önemli" olarak adlandırılmalıdır. Eklenmesi önemlidir **[FromOdataUri]** gösterildiği gibi öznitelik. Bu öznitelik, istek URI'si anahtarından ayrıştırırken OData sözdizimi kurallarını kullanmak için Web API'si söyler.

Kullanım *parametreleri* sözlük eylem parametrelerini almak için:

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

İstemci eylem parametrelerini doğru gönderirse biçimi, değerini **ModelState.IsValid** geçerlidir. Bu durumda, kullanabileceğiniz **ODataActionParameters** parametre değerlerini almak için bir sözlük. Bu örnekte, `RateProduct` eylem "Sıralama" adlı tek bir parametre alır.

## <a name="action-metadata"></a>Eylem meta verileri

Hizmet meta verileri görüntülemek için /odata/$ meta verileri için bir GET isteği gönderin. Bildiren bir meta veri bölümünü işte `RateProduct` eylem:

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**Functionımport** eylemi öğe bildirir. Alanların çoğu kendinden açıklamalıdır ancak iki çıkarabileceğini şunlardır:

- **İsbindable olduğunda** eylemi çağrılabilir hedef varlık üzerinde en az anlamına gelir sürenin bir kısmı.
- **Isalwaysbindable** eylem her zaman hedef varlık üzerinde çağrılacak anlamına gelir.

Bazı eylemler her zaman istemciler için kullanılabilir, ancak diğer eylemler varlık durumuna bağlı olabilir farktır. Örneğin, "Satın Al" eylemini tanımladığınız varsayalım. Yalnızca stokta olan bir öğeyi satın alabilirsiniz. Öğe sağlanamıyor ise, bir istemci o eylemi çağrılamıyor.

EDM tanımlarken **eylem** yöntemi her zaman bağlanabilirse bir eylem oluşturur:

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Not-her zaman-bağlanabilir eylemleri hakkında ele alacağız (olarak da bilinir *geçici* eylemleri) Bu konuda.

## <a name="invoking-the-action"></a>Eylemi Çağırma

Artık bu eylem bir istemci nasıl çağıracaktır görelim. İstemci Kimliğine sahip ürün derecelendirmesi 2 vermek istediğini varsayalım = 4. İstek gövdesi JSON biçimini kullanarak bir örnek istek iletisi aşağıda verilmiştir:

[!code-console[Main](odata-actions/samples/sample9.cmd)]

Yanıt iletisi aşağıda verilmiştir:

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>Bir varlık kümesi için bir eylem bağlama

Önceki örnekte, tek bir varlığa bağlı eylemi: tek bir ürün istemci derecelendirir. Ayrıca, bir eylem bir varlık koleksiyonuna bağlayabilirsiniz. Yalnızca aşağıdaki değişiklikleri yapın:

EDM içinde varlığın Eylem Ekle **koleksiyon** özelliği.

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

Denetleyici yöntemin atlamak *anahtar* parametresi.

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

Artık istemci ürünleri varlık kümesini eylemini çağırır:

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>Toplama parametrelerini eylemleri

Eylemler, bir değerler topluluğunu parametreleri olabilir. EDM içinde kullanmak **CollectionParameter&lt;T&gt;**  parametre bildirmek için.

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

Bu "koleksiyonunu alır derecelendirmeleri" adlı bir parametre bildirir **int** değerleri. Denetleyici yöntemin almaya devam ediyorsanız parametre değerinin **ODataActionParameters** nesne, ancak artık değeri bir **ICollection&lt;int&gt;**  değeri:

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>Geçici eylemleri

Eylemin her zaman kullanılabilir olacak şekilde "RateProduct" örnekte, kullanıcıların her zaman bir ürün derecelendirebilirsiniz. Ancak, bazı eylemler varlık durumuna bağlıdır. Örneğin, bir video kiralama hizmetinde "Kullanıma Al" eylemini her zaman kullanılabilir değil. (Bu, söz konusu videoyu bir kopyasını kullanılabilir olup olmadığını bağlıdır.) Bu tür bir eylem olarak adlandırılan bir *geçici* eylem.

Hizmet meta verileri geçici bir eyleme sahip **Isalwaysbindable** false eşittir. Meta veriler aşağıdaki gibi görünmesi için gerçekten varsayılan değer olan:

[!code-xml[Main](odata-actions/samples/sample16.xml)]

Bu neden önemli olduğunu aşağıdadır: sunucunun eylem geçici ise, istemci eylemi kullanılabilir olduğunda söylemeniz gerekir. Varlık içinde eylem bağlantısı ekleyerek bunu yapar. Bir film varlık için bir örnek aşağıda verilmiştir:

[!code-console[Main](odata-actions/samples/sample17.cmd)]

"#CheckOut" özelliği CheckOut eylemi bir bağlantı içerir. Eylem kullanılamıyorsa, sunucunun bağlantı atlar.

EDM geçici bir eylemde bildirmek için çağrı **TransientAction** yöntemi:

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

Ayrıca, belirli bir varlık için bir eylem bağlantısı döndüren bir işlev sağlamanız gerekir. Bu işlev çağırarak ayarlamak **HasActionLink**. İşlevi bir lambda ifadesi yazabilirsiniz:

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

Eylem varsa, lambda ifadesi bir bağlantı için eylem döndürür. OData serileştirici varlık serileştiren bu bağlantıyı içerir. Eylemi kullanılabilir olmadığı durumlarda, bir işlevi döndürür `null`.

## <a name="additional-resources"></a>Ek Kaynaklar

[OData eylem örneği](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
