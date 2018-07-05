---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: ASP.NET Web API 1'de CRUD işlemlerini etkinleştirme | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API'sini kullanarak, HTTP hizmeti CRUD işlemlerini desteklemek gösterilir. Öğretici Visual Studio 2012 Web AP içinde kullanılan yazılım sürümleri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: 1658e120225cd3c9425168238981133c96ff398a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402934"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>ASP.NET Web API 1'de CRUD işlemlerini etkinleştirme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Bu öğreticide, ASP.NET Web API'sini kullanarak, HTTP hizmeti CRUD işlemlerini desteklemek gösterilir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Visual Studio 2012
> - Web API 1 (aynı zamanda Web API 2 ile çalışır)


CRUD anlamına gelen &quot;oluşturma, okuma, güncelleştirme ve silme,&quot; dört temel veritabanı işlemlerinin olduğu. Birçok HTTP Hizmetleri de CRUD işlemlerini REST veya gibi REST API'leri aracılığıyla model.

Bu öğreticide, çok basit bir web API ürünlerin listesini yönetmek için oluşturacaksınız. Her ürün adı, fiyatı ve kategorisi içerir (gibi &quot;toys&quot; veya &quot;donanım&quot;), artı ürün kimliği

API ürünler, yöntemleri açığa çıkarır.

| Eylem | HTTP yöntemi | Göreli URI'si |
| --- | --- | --- |
| Tüm ürünlerin listesini alın | AL | / api/ürünleri |
| Bir ürün Kimliğine göre Al | AL | /API'si/ürünler/*kimliği* |
| Bir ürün kategorisine göre Al | AL | / api/ürünleri? kategori =*kategorisi* |
| Yeni Ürün oluşturma | YAYINLA | / api/ürünleri |
| Bir ürün güncelleştirmesi | PUT | /API'si/ürünler/*kimliği* |
| Bir ürün Sil | DELETE | /API'si/ürünler/*kimliği* |

Bir URI'leri bazıları ürün kimliği yolundaki Ekle dikkat edin. Örneğin, 28 Kimliğine sahip ürün almak için istemci bir GET isteği gönderir `http://hostname/api/products/28`.

### <a name="resources"></a>Kaynaklar

API ürünleri iki kaynak türleri için URI tanımlar:

| Kaynak | URI |
| --- | --- |
| Tüm ürünler listesi. | / api/ürünleri |
| Tek bir ürün. | /API'si/ürünler/*kimliği* |

### <a name="methods"></a>Yöntemler

Dört ana HTTP yöntemleri (GET, PUT, POST ve DELETE) için CRUD işlemleri gibi eşlenebilir:

- GET, belirtilen URI'de kaynağın bir gösterimini alır. GET sunucu üzerinde hiçbir yan etkisinin olmaması gerekir.
- PUT, belirtilen URI'de bir kaynağı güncelleştirir. Sunucu yeni bir URI'leri belirtmek istemcileri izin veriyorsa PUT bir belirtilen URI'de yeni bir kaynak oluşturmak için de kullanılabilir. Bu öğreticide, API ile PUT oluşturma desteklemez.
- POST, yeni bir kaynak oluşturur. Sunucu yeni bir nesne için URI atar ve bu URI, yanıt iletisinin bir parçası olarak döndürür.
- SİLME, belirtilen URI'de bir kaynak siler.

Not: Tüm ürün varlığı PUT yöntemini değiştirir. Diğer bir deyişle, güncelleştirilen ürün tam bir temsilini göndermek için istemci bekleniyor. Kısmi güncelleştirmeleri desteklemek istiyorsanız, PATCH yöntemini tercih edilir. Bu öğreticide, düzeltme eki uygulamıyor.

## <a name="create-a-new-web-api-project"></a>Yeni bir Web API projesi oluşturma

Visual Studio çalıştırarak ve seçin **yeni proje** gelen **Başlat** sayfası. Veya **dosya** menüsünde **yeni** ardından **proje**.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Projeyi adlandırın &quot;ProductStore&quot; tıklatıp **Tamam**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Web API** tıklatıp **Tamam**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Model ekleme

A *modeli* uygulamanızdaki verileri temsil eden bir nesnedir. ASP.NET Web API, türü kesin belirlenmiş CLR nesne modelleri olarak kullanabilirsiniz ve bunlar otomatik olarak XML veya JSON istemci için seri hale.

Adlı yeni bir sınıf oluşturacağız. böylece ProductStore API'si için verilerimizi, ürünlerinin aynısından. `Product`.

Çözüm Gezgini görünür değilse, **görünümü** menü ve select **Çözüm Gezgini**. Çözüm Gezgini'nde sağ **modelleri** klasör. Bağlam meny seçin **Ekle**, ardından **sınıfı**. Sınıf adı &quot;ürün&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Aşağıdaki özellikleri `Product` sınıfı.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Bir depo ekleme

Ürünleri koleksiyonunu depolamak gerekir. Hizmet kararlılığımızın koleksiyondan ayırmak için iyi bir fikirdir. Bu şekilde yedekleme deposu hizmet sınıfı yeniden yazma olmadan Değiştirebiliriz. Bu tür bir tasarım adlı *depo* deseni. Depo için genel bir arabirim tanımlayarak başlatın.

Çözüm Gezgini'nde sağ **modelleri** klasör. Seçin **Ekle**, ardından **yeni öğe**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

İçinde **şablonları** bölmesinde **yüklü şablonlar** ve C# düğümünü genişletin. C# altında seçin **kod**. Kod şablonları listesinde seçin **arabirimi**. Arabirim adı &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Aşağıdaki uygulama ekleyin:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Artık başka bir sınıf adlı modelleri klasörüne ekleyin &quot;ProductRepository.&quot; Bu sınıf uygulayacak `IProductRespository` arabirimi. Aşağıdaki uygulama ekleyin:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Depoyu yerel belleğinde listesini tutar. Bu öğretici için Tamam olduğunu, ancak gerçek bir uygulamada verilerin harici olarak ya da bir veritabanını saklayacağından veya Bulut depolama. Depo düzeni daha sonra uygulama değiştirme hale getirir.

## <a name="adding-a-web-api-controller"></a>Web API denetleyici ekleme

ASP.NET MVC ile çalıştıysanız, ardından denetleyicileriyle bilginiz. ASP.NET Web API, bir *denetleyicisi* istemciden gelen HTTP isteklerini işleyen sınıftır. Proje oluşturduğunuz sırada yeni proje sihirbazını iki denetleyici oluşturulan. Bunları görmek için Çözüm Gezgini'nde denetleyicileri klasörünü genişletin.

- HomeController, geleneksel bir ASP.NET MVC denetleyicisi değil. HTML sayfaları site için hizmet vermek için sorumludur ve müşterilerimizin web API'sine doğrudan ilgili değildir.
- ValuesController bir örnek Webapı denetleyicisi değil.

Devam edin ve ValuesController, Çözüm Gezgini'nde dosyaya sağ tıklatıp seçerek silme **silin.** Şimdi yeni bir denetleyici aşağıdaki şekilde ekleyin:

İçinde **Çözüm Gezgini**, denetleyicileri klasörü sağ tıklatın. Seçin **Ekle** seçip **denetleyicisi**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

İçinde **denetleyici Ekle** Sihirbazı, denetleyici adı &quot;ProductsController&quot;. İçinde **şablon** aşağı açılan listesinden **boş API denetleyicisi**. Ardından **Ekle**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Contollers denetleyicileri adlı klasöre koyun gerekli değildir. Klasör adı önemli değildir; Bu kaynak dosyaları düzenlemek için yalnızca bir yoludur.


**Denetleyici Ekle** Sihirbazı ProductsController.cs denetleyicileri klasöründe adlı bir dosya oluşturur. Bu dosya zaten açık değilse, açmak için dosyaya çift tıklayın. Aşağıdaki **kullanarak** deyimi:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Tutan bir alan eklemek bir **IProductRepository** örneği.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Çağırma `new ProductRepository()` özel uygulanışı denetleyiciye bölümlere denetleyicide en iyi tasarım olmadığından `IProductRepository`. Daha iyi bir yaklaşım için bkz. [Web API bağımlılık çözümleyicisini kullanarak](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Bir kaynak alma

ProductStore API birkaç açığa çıkarır &quot;okuma&quot; Eylemler HTTP GET yöntemleri olarak. Her eylem bir yöntemde karşılık gelir `ProductsController` sınıfı.

| Eylem | HTTP yöntemi | Göreli URI'si |
| --- | --- | --- |
| Tüm ürünlerin listesini alın | AL | / api/ürünleri |
| Bir ürün Kimliğine göre Al | AL | /API'si/ürünler/*kimliği* |
| Bir ürün kategorisine göre Al | AL | / api/ürünleri? kategori =*kategorisi* |

Tüm ürünlerin listesini almak için bu yönteme ekleme `ProductsController` sınıfı:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Yöntem adı ile başlayan &quot;alma&quot;, isteğe bağlı olarak Kural gereği GET isteklerinin eşler. Ayrıca, bu eşler içermeyen bir URI yöntemin parametre olduğundan, bir *&quot;kimliği&quot;* yolunda kesimi.

Kimliğe göre bir ürün almak için bu yönteme ekleme `ProductsController` sınıfı:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Bu yöntem adı ile başlayan ayrıca &quot;alma&quot;, ancak yöntem adlı bir parametre *kimliği*. Bu parametre eşlenen &quot;kimliği&quot; URI yolu kesimi. ASP.NET Web API çerçevesi, kimlik otomatik olarak doğru veri türüne dönüştürür. (**int**) parametresi için.

Türünde bir özel durum GetProduct yöntemi oluşturur **HttpResponseException** varsa *kimliği* geçerli değil. Bu özel durumun framework tarafından bir 404 (bulunamadı) hatası çevrilir.

Son olarak, kategoriye göre ürünleri bulmak için bir yöntem ekleyin:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

İstek URI'SİNDEKİ bir sorgu dizesine sahipse, Web API denetleyici yönteminin parametreleri sorgu parametreleri eşleştirmeye çalışır. Bu nedenle, bir URI biçiminde "API/ürünleri? kategori =*kategori*" Bu yönteme eşler.

## <a name="creating-a-resource"></a>Kaynak oluşturma

Ardından, bir yönteme ekleyeceğiz `ProductsController` sınıfının yeni bir ürün oluşturun. Basit bir yöntem uygulaması şu şekildedir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Bu yöntem hakkında iki şeyi göz önünde bulundurun:

- Yöntem adı ile başlayan &quot;Post... &quot;. Yeni ürün oluşturmak için istemci bir HTTP POST isteği gönderir.
- Yöntem ürün türünde bir parametre alır. Web API'de gövdeden parametrelerle karmaşık türleri seri. Bu nedenle, XML veya JSON biçiminde bir ürün nesnesinin serileştirilmiş bir gösterimi göndermek için istemci bekliyoruz.

Bu uygulama çalışır, ancak henüz tamamlanmadı. İdeal olarak, aşağıdakiler dahil etmek için HTTP yanıt istiyoruz:

- **Yanıt kodu:** varsayılan olarak, Web API çerçevesi yanıt durum kodu 200 (Tamam) ayarlar. Ancak, bir POST isteği bir kaynak oluşturulmasını içinde sonuçlandığında göre HTTP/1.1 protokolünü, sunucunun 201 (oluşturuldu) durumu ile yanıtla.
- **Konum:** sunucu kaynak oluşturduğunda, yeni kaynağın URI'sini yanıtının Location üst bilgisini içermelidir.

ASP.NET Web API HTTP yanıt iletisini işlemek kolaylaştırır. Gelişmiş uygulama şu şekildedir:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Yöntem dönüş türü artık olduğuna dikkat edin **HttpResponseMessage**. Döndürerek bir **HttpResponseMessage** yerine bir ürün, biz durum kodunu ve Location üst bilgisini dahil olmak üzere HTTP yanıt iletisinin ayrıntıları kontrol edebilirsiniz.

**CreateResponse** yöntemi oluşturur bir **HttpResponseMessage** ve otomatik olarak serileştirilmiş bir gösterimi olan ürün nesne gövdesine Yazar fo yanıt iletisi.

> [!NOTE]
> Bu örnekte doğrulamamanız `Product`. Model doğrulama hakkında daha fazla bilgi için bkz. [ASP.NET Web API'de Model doğrulama](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Bir kaynak güncelleştirme

Bir ürün ile PUT güncelleştirmek kolaydır:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Yöntem adı ile başlayan &quot;yerleştirin... &quot;, Web API'si için PUT isteklerini eşleşecek şekilde. Yöntem iki parametre, ürün kimliği ve güncelleştirilen ürün alır. *Kimliği* parametresi URI yolundan alınır ve *ürün* gövdeden parametresi seri durumdan. Varsayılan olarak, ASP.NET Web API çerçevesi rotadaki basit parametre türleri ve karmaşık türler istek gövdesinden alır.

## <a name="deleting-a-resource"></a>Kaynak siliniyor

Bir resourse silmek için "Sil..." yöntemi tanımlayın.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Bir silme isteği başarılı olursa, bir varlık durumunu açıklayan gövdesi ile 200 (Tamam) durum döndürebilir; Durum 202 (kabul edildi silme işlemi hala ise) bekleyen; veya durumu ile hiçbir varlık gövdesini 204 (içerik yok). Bu durumda, `DeleteProduct` yöntemi olan bir `void` dönüş türü, durumuna, bu otomatik olarak ASP.NET Web API dönüşür şekilde 204 (içerik yok) kodu.
