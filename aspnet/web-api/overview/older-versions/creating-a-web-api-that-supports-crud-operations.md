---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "ASP.NET Web API'de 1 CRUD işlemleri etkinleştirme | Microsoft Docs"
author: MikeWasson
description: "Bu öğreticide, ASP.NET Web API kullanarak, HTTP hizmeti CRUD işlemleri desteklemek gösterilmiştir. Eğitmen Visual Studio 2012 Web AP içinde kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 69b7d5453b6ff36d6e28a69428b016cb8cfd06e9
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2018
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>ASP.NET Web API'de 1 CRUD işlemleri etkinleştirme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Bu öğreticide, ASP.NET Web API kullanarak, HTTP hizmeti CRUD işlemleri desteklemek gösterilmiştir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Visual Studio 2012
> - Web API 1 (aynı zamanda Web API 2 ile çalışır)


CRUD anlamına gelir &quot;oluşturma, okuma, güncelleştirme ve silme,&quot; dört temel veritabanı işlemleri olduğu. Birçok HTTP Hizmetleri de CRUD işlemleri REST veya gibi REST API'leri aracılığıyla model.

Bu öğreticide, çok basit bir web API ürünlerin listesini yönetmek için oluşturacaksınız. Her ürün adı, fiyat ve kategorisi içerir (gibi &quot;toys&quot; veya &quot;donanım&quot;), artı bir ürün kimliği

API ürünler, yöntemleri açığa çıkarır.

| Eylem | HTTP yöntemi | Göreli URI |
| --- | --- | --- |
| Tüm ürünlerin listesini al | AL | / api/ürünleri |
| Ürün Kimliği tarafından Al | AL | /api/products/*id* |
| Bir ürün kategorisine göre alma | AL | /api/products?category=*category* |
| Yeni Ürün oluşturma | YAYINLA | / api/ürünleri |
| Bir ürün güncelleştir | PUT | /api/products/*id* |
| Ürünü silme | DELETE | /api/products/*id* |

Bazı URI'ler ürün kimliği yolundaki dahil dikkat edin. Örneğin, Kimliğine sahip 28 ürün almak için istemci bir GET isteği gönderir `http://hostname/api/products/28`.

### <a name="resources"></a>Kaynaklar

API ürünleri URI'ler iki kaynak türleri için tanımlar:

| Kaynak | URI |
| --- | --- |
| Tüm ürünler listesi. | / api/ürünleri |
| Bir ürünün. | /api/products/*id* |

### <a name="methods"></a>Yöntemler

Dört ana HTTP yöntemleri (GET, PUT, POST ve Sil) CRUD işlemleri için aşağıdaki gibi eşlenebilir:

- GET Belirtilen URI kaynak gösterimini alır. GET sunucu üzerinde hiçbir yan etkisi olmalıdır.
- Belirtilen URI kaynakta PUT güncelleştirir. Sunucu yeni URI'ler belirtmek istemcileri izin veriyorsa PUT ayrıca belirtilen bir URI'da, yeni bir kaynak oluşturmak için kullanılabilir. Bu öğreticide, API PUT ile oluşturma desteklemez.
- POST yeni bir kaynak oluşturur. Sunucu yeni bir nesne için URI atar ve yanıt iletisi bir parçası olarak bu URI döndürür.
- Sil, belirtilen URI'ye kaynakta siler.

Not: Tüm ürün varlık PUT yöntemini değiştirir. Diğer bir deyişle, istemci güncelleştirilen ürün eksiksiz bir gösterimini göndermesi beklenir. Kısmi güncelleştirmeler desteklemek istiyorsanız, düzeltme eki tercih edilen yöntemdir. Bu öğretici, düzeltme eki uygulamıyor.

## <a name="create-a-new-web-api-project"></a>Yeni bir Web API projesi oluşturma

Visual Studio çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde, select **yeni** ve ardından **proje**.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı &quot;ProductStore&quot; tıklatıp **Tamam**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Web API** tıklatıp **Tamam**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Model ekleme

A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir. ASP.NET Web API, modelleri olarak kesin türü belirtilmiş CLR nesnelerini kullanabilirsiniz ve bunlar otomatik olarak XML veya JSON istemci için seri hale getirilir.

Adlı yeni bir sınıf oluşturacağız şekilde ProductStore API için verilerimizi ürünlerden, oluşur. `Product`.

Çözüm Gezgini görünür değilse, **Görünüm** menü ve select **Çözüm Gezgini**. Çözüm Gezgini'nde sağ **modelleri** klasör. Bağlam meny seçin **Ekle**seçeneğini belirleyip **sınıfı**. Sınıf adını &quot;ürün&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Aşağıdaki özellikleri ekleyin `Product` sınıfı.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Bir havuz ekleme

Ürünler koleksiyonu depolamak gerekir. Bizim hizmet uygulaması koleksiyonundan ayırmak için iyi bir fikirdir. Böylece, biz yedekleme deposu hizmet sınıfı yeniden yazma işlemi olmadan değiştirebilirsiniz. Bu tür bir tasarım adlı *depo* düzeni. Depo için genel bir arabirim tanımlayarak başlatın.

Çözüm Gezgini'nde sağ **modelleri** klasör. Seçin **Ekle**seçeneğini belirleyip **yeni öğe**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve C# düğümünü genişletin. C# altında seçin **kod**. Kod şablonları listesinde seçin **arabirimi**. Arabirim adı &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Aşağıdaki uygulama ekleyin:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Şimdi adlı modeller klasörü için başka bir sınıf ekleyin &quot;ProductRepository.&quot; Bu sınıf gerçekleştireceksiniz `IProductRespository` arabirimi. Aşağıdaki uygulama ekleyin:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Depo listesi yerel bellekte saklar. Bu öğretici için Tamam, ancak gerçek bir uygulamada veri harici olarak ya da bir veritabanı saklayacağından veya Bulut depolama. Depo düzeni uygulama daha sonra değiştirmek hale getirir.

## <a name="adding-a-web-api-controller"></a>Bir Web API denetleyicisi ekleme

ASP.NET MVC ile çalıştıysanız, ardından denetleyicileriyle bilginiz. ASP.NET Web API içinde bir *denetleyicisi* istemciden gelen HTTP isteklerini işleyen sınıftır. Proje oluşturduğunuzda Yeni Proje Sihirbazı'nı iki denetleyicileri oluşturulan. Bunları görmek için Çözüm Gezgini'nde denetleyicileri klasörünü genişletin.

- HomeController geleneksel bir ASP.NET MVC denetleyicisi değil. Site için HTML sayfaları hizmet vermek için sorumludur ve doğrudan sunduğumuz web API ilişkili değil.
- ValuesController örnek Webapı denetleyicisidir.

Bir tane ValuesController, Çözüm Gezgini'nde dosyaya sağ tıklayıp seçerek silme **silin.** Şimdi yeni bir denetleyici aşağıdaki şekilde ekleyin:

İçinde **Çözüm Gezgini**, denetleyicileri klasörünü sağ tıklatın. Seçin **Ekle** ve ardından **denetleyicisi**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

İçinde **denetleyici Ekle** Sihirbazı, denetleyici adı &quot;ProductsController&quot;. İçinde **şablonu** aşağı açılan listesinden, **boş API denetleyicisi**. Ardından **Ekle**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Contollers denetleyicileri adlı bir klasöre yerleştirmek gerekli değildir. Klasör adı önemli değildir; Bu Kaynak dosyalarınız düzenlemek için yalnızca bir yoludur.


**Denetleyici Ekle** Sihirbazı denetleyicileri klasöründe ProductsController.cs adlı bir dosya oluşturur. Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın. Aşağıdakileri ekleyin **kullanarak** deyimi:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Tutan bir alan ekleyebilmek bir **IProductRepository** örneği.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Çağırma `new ProductRepository()` belirli bir uygulamaya denetleyicisine bağlar denetleyicide en iyi tasarım olmadığından `IProductRepository`. Daha iyi bir yaklaşım için bkz: [Web API bağımlılık çözümleyicisini kullanarak](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Bir kaynak alınıyor

ProductStore API birkaç açığa çıkarır &quot;okuma&quot; HTTP GET yöntemi olarak eylemler. Her eylem bir yöntem karşılık gelir `ProductsController` sınıfı.

| Eylem | HTTP yöntemi | Göreli URI |
| --- | --- | --- |
| Tüm ürünlerin listesini al | AL | / api/ürünleri |
| Ürün Kimliği tarafından Al | AL | /api/products/*id* |
| Bir ürün kategorisine göre alma | AL | /api/products?category=*category* |

Tüm ürünlerin listesini almak için bu yöntemi ekleyin `ProductsController` sınıfı:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Yöntem adı ile başlayan &quot;almak&quot;, isteğe bağlı olarak kurala göre GET isteklerinin eşler. Ayrıca, bu eşlemeleri içermeyen bir URI yöntemi hiçbir parametre yoktur çünkü bir  *&quot;kimliği&quot;*  yol kesimi.

Kimliğe göre bir ürün almak için bu yöntemi ekleyin `ProductsController` sınıfı:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Bu yöntem adı da ile başlayan &quot;almak&quot;, ancak yöntem adlı bir parametre *kimliği*. Bu parametre eşlenmiş &quot;kimliği&quot; URI yolu kesimi. ASP.NET Web API çerçevesi, Kimliğini otomatik olarak doğru veri türüne dönüştürür. (**int**) parametresi için.

GetProduct yöntemi türünde bir özel durum atar **HttpResponseException** varsa *kimliği* geçerli değil. Bu özel durum 404 (bulunamadı) hatası çerçevesi tarafından çevrilir.

Son olarak, ürün kategorisine göre bulmak için bir yöntem ekleyin:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

İsteğin URI sorgu dizesine sahipse, Web API denetleyicisi yöntemi parametrelerine sorgu parametreleri eşleştirmeyi dener. Bu nedenle, bir URI biçiminde "API/ürünleri? kategori =*kategori*" Bu yönteme eşler.

## <a name="creating-a-resource"></a>Kaynak oluşturma

Ardından, bir yönteme ekleyeceğiz `ProductsController` yeni ürün oluşturmak için sınıfı. Basit bir yöntemin kullanımı şöyledir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Bu yöntem hakkında iki şey dikkat edin:

- Yöntem adı ile başlayan &quot;Post... &quot;. Yeni bir ürün oluşturmak için istemci bir HTTP POST isteği gönderir.
- Yöntem ürün türünde bir parametre alır. Web API'de parametreleri Karmaşık türlerle isteği gövdesinden serisi. Bu nedenle, XML veya JSON biçiminde bir ürün nesnesinin serileştirilmiş bir gösterimi göndermek için istemci bekliyoruz.

Bu uygulama çalışır, ancak oldukça tam değil. İdeal olarak, aşağıdakiler dahil etmek için HTTP yanıtı isteriz:

- **Yanıt kodu:** varsayılan olarak, Web API çerçevesi yanıt durum kodu 200'e (Tamam) ayarlar. Ancak bir POST isteği bir kaynak oluşturulmasında sonuçlandığında HTTP/1.1 protokolünü göre sunucu durum 201 (oluşturuldu) ile yanıtla.
- **Konumu:** sunucu kaynak oluşturduğunda, yanıt konum üstbilgisinin yeni kaynak URI'si içermelidir.

ASP.NET Web API HTTP yanıt iletisi işlemek kolaylaştırır. Geliştirilmiş uygulama şöyledir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Yöntemin dönüş türü artık olduğuna dikkat edin **httpresponsemessage öğesini**. Döndürerek bir **httpresponsemessage öğesini** bir ürün yerine biz durum kodunu ve Location üst bilgisini de dahil olmak üzere HTTP yanıt iletisinin ayrıntılarını kontrol edebilirsiniz.

**CreateResponse** yöntemi oluşturur bir **httpresponsemessage öğesini** ve otomatik olarak serileştirilmiş bir gösterimi Product nesnesinin gövdesi Yazar fo yanıt iletisi.

> [!NOTE]
> Bu örnek doğrulanmadı `Product`. Model doğrulama hakkında daha fazla bilgi için bkz: [ASP.NET Web API Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Bir kaynak güncelleştirme

Bir ürün ile PUT güncelleştirme basittir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Yöntem adı ile başlayan &quot;yerleştirin... &quot;, Web API PUT isteklerini eşleşecek şekilde. Yöntemi iki parametre, ürün kimliği ve güncelleştirilen ürün alır. *Kimliği* parametresi URI yolundan alınır ve *ürün* parametre istek gövdesinden serisi. Varsayılan olarak, ASP.NET Web API çerçevesi rotadaki basit parametre türleri ve karmaşık türler istek gövdesi alır.

## <a name="deleting-a-resource"></a>Bir kaynağı siliniyor

Bir resourse silmek için "Delete..." yöntemi tanımlayın.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Bir silme isteği başarılı olursa, bir varlık durumu açıklayan gövdesi ile durum 200 (Tamam) döndürebilir; Durum 202 (silme hala ise, kabul edilen) bekleyen; veya durumu ile hiçbir varlık gövdesini 204 (No içerik). Bu durumda, `DeleteProduct` yöntemi sahip bir `void` dönüş türü, durumunu, bu otomatik olarak ASP.NET Web API dönüşür şekilde 204 (No içerik) kodu.
