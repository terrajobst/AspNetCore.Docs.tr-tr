---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görünen veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.7 ve Microsoft Visual Studio 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri gösterir.
ms.author: riande
ms.date: 1/04/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: acc2f8e78375ef0455d467e2af750ecbee623224
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396231"
---
<a name="display-data-items-and-details"></a>Görünen veri öğelerini ve ayrıntıları
====================
tarafından [Erik Reitan](https://github.com/Erikre)

> Bu öğretici serisinin ASP.NET 4.7 ve Microsoft Visual Studio 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.

Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First ile veri öğesi ayrıntılarını görüntülemeyi öğreneceksiniz. Bu öğreticide, Wingtip çocuğunun Store öğretici serisinin bir parçası olarak önceki "Kullanıcı Arabirimi ve gezinti" öğreticisini üzerine inşa edilmiştir. Bu öğreticiyi tamamladıktan sonra üzerinde ürünleri görürsünüz *ProductsList.aspx* sayfa ve bir ürün uygulamasının Ayrıntılar *ProductDetails.aspx* sayfası.

## <a name="youll-learn-how-to"></a>Öğreneceksiniz nasıl yapılır:

- Veritabanından ürünleri görüntülemek için bir veri denetim ekleme
- Bir veri denetimi seçilen verileri bağlayın
- Veritabanından ürün ayrıntıları görüntülemek için bir veri denetim ekleme
- Sorgu dizesinden bir değer almak ve veritabanından alınan verileri sınırlamak için bu değeri kullanın

### <a name="features-introduced-in-this-tutorial"></a>Bu öğreticide sunulan özellikleri:

- Model bağlama
- Değer sağlayıcıları

## <a name="add-a-data-control"></a>Veri denetim ekleme

Sunucu denetimine veri bağlama için birkaç farklı Seçenekleri'ni kullanabilirsiniz. En yaygın şunlardır:

 * Veri kaynak denetimi ekleme
 * Kodu el ile ekleme
 * Model bağlama işlemini kullanma

### <a name="use-a-data-source-control-to-bind-data"></a>Veri bağlama için bir veri kaynağı denetimi kullanma

Veri kaynağı denetim ekleme, verileri görüntüleyen denetime veri kaynak denetimine bağlamak sağlar. Bu yaklaşım, bildirimli olarak yapabilirsiniz yerine programlı olarak sunucu tarafı denetimlerdir veri kaynaklarına bağlanın.

### <a name="code-by-hand-to-bind-data"></a>Kodu el ile veri bağlama

El ile kodlama içerir:

1. Bir değer okuma
2. Null olup olmadığı denetleniyor
3. Uygun bir türe dönüştürme
4. Dönüştürme başarılı denetleniyor
5. Sorgu değeri kullanma 

Bu yaklaşım, veri erişim mantığı üzerinde tam denetime sahip olanak sağlar.

### <a name="use-model-binding-to-bind-data"></a>Model bağlama veri bağlamak için kullanın

Model bağlama, sonuçları daha az kod ile bağlamanıza imkan sağlar ve uygulamanızda işlevini yeniden kullanma olanağı sağlar. Kod odaklı veri erişim mantığı ile çalışma, yine de bir zengin, veri bağlama çerçevesi sağlarken basitleştirir.

## <a name="display-products"></a>Ürünleri görüntülemek

Bu öğreticide, veri bağlama için model bağlama kullanacaksınız. Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için denetimin ayarlayın. `SelectMethod` özelliğini sayfanın kodda bir yöntem adı. Veri denetimi, sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. Açıkça çağırmaya gerek yoktur `DataBind` yöntemi.

1. İçinde **Çözüm Gezgini**açın *ProductList.aspx*.
2. Mevcut biçimlendirme, bu işaretleme ile değiştirin:   

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Bu kod bir **ListView** adlı Denetim `productList` ürünleri görüntülemek için.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Şablonları ve stilleri ile tanımladığınız nasıl **ListView** denetim verilerini görüntüler. Tüm yinelenen yapısındaki veriler için kullanışlıdır. Ancak bu **ListView** örnek yalnızca veritabanı verileri görüntüler, ayrıca, kodu olmadan etkinleştir kullanıcıların düzenlemek, eklemek ve verileri silmek için ve sıralama ve veri sayfasını kullanabilirsiniz.

Ayarlayarak `ItemType` özelliğinde **ListView** denetimine veri bağlama ifadesi `Item` kullanılabilir ve Denetim türü kesin belirlenmiş olur. Önceki öğreticide belirtildiği gibi belirtme gibi IntelliSense ile öğesi nesne ayrıntıları seçebilirsiniz `ProductName`:

![Veri öğelerini ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

Model bağlama belirtmek için kullanıyorsanız bir `SelectMethod` değeri. Bu değer (`GetProducts`) kodu eklemeniz yöntemine karşılık gelen arkasından sonraki adımda ürünleri görüntülemek için.

### <a name="add-code-to-display-products"></a>Ürünleri görüntülemek için kod ekleme

Bu adımda, doldurmak için kod ekleyeceksiniz **ListView** ürün verileri veritabanından denetimi. Tüm ürünler ve tek bir kategori ürünler gösteren kod destekler.

1. İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* seçip **kodu görüntüle**.
2. Varolan kodda değiştirin *ProductList.aspx.cs* bu dosya:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Bu kod gösterir `GetProducts` yöntemi, **ListView** denetimin `ItemType` özelliği başvurularının *ProductList.aspx* sayfası. Sonuçları belirli veritabanı kategorisi sınırlamak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerini değerinden *ProductList.aspx* ne zaman sayfasında *ProductList.aspx* sayfası için gitme. `QueryStringAttribute` Sınıfını `System.Web.ModelBinding` ad alanı değeri sorgu dizesi değişkeni almak için kullanılan `id`. Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya için bildirir `categoryId` çalışma zamanında parametresi.

Geçerli bir kategori sorgu dizesi olarak sayfasına geçildiğinde, sorgunun sonuçlarını eşleşen bu ürünlere veritabanında sınırlı `categoryId` değeri. Örneğin, *ProductsList.aspx* sayfası URL'si şudur:


[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Sayfa yalnızca ürünleri görüntüler. burada `categoryId` eşittir `1`.

Hiçbir sorgu dizesi dahildir tüm ürünleri görüntülenir *ProductList.aspx* sayfa çağrılır.

Bu yöntemlere ait değerlerin kaynakları olarak ifade edilir *değer sağlayıcıları* (gibi *QueryString*), ve kullanmak için hangi değer sağlayıcı gösterir parametre öznitelikleri adlandırılır*değer sağlayıcısı öznitelikleri* (gibi `id`). ASP.NET, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimler, Görünüm durumu, oturum durumu ve profil özellikleri gibi bir Web Forms uygulamasındaki değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen özniteliklerini içerir. Özel değer sağlayıcıları da yazabilirsiniz.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Tüm ürünler ya da bir kategorinin ürünler görüntülemek için uygulamayı şimdi çalıştırın.

1. Tuşuna **F5** uygulamayı çalıştırmak için Visual Studio çalışırken.  
   Tarayıcı açılır ve gösterir *Default.aspx* sayfası.

2. Seçin **otomobiller** ürün kategorisi Gezinti menüsünde.  
   *ProductList.aspx* sayfası görüntüler yalnızca gösteren **otomobiller** kategori ürünleri. Bu öğreticide daha sonra ürün ayrıntıları görüntüleyeceksiniz.  

    ![Veri öğelerini ve ayrıntıları - otomobiller](display_data_items_and_details/_static/image2.png)

3. Seçin **ürünleri** üst gezinti menüsünde.  
   Yeniden *ProductList.aspx* sayfası görüntülenir, ancak isteğe bağlı olarak şu ürünlerin tam listesini gösterir.   

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image3.png)

4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

### <a name="add-a-data-control-to-display-product-details"></a>Ürün Ayrıntıları görüntülemek için bir veri denetim ekleme

Ardından, işaretlemede değiştireceksiniz *ProductDetails.aspx* belirli ürün bilgilerini görüntülemek için önceki öğreticide eklenen sayfası.

1. İçinde **Çözüm Gezgini**açın *ProductDetails.aspx*.

2. Mevcut biçimlendirme, bu işaretleme ile değiştirin:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

    Bu kod bir **FormView** belirli bir ürün ayrıntılarını görüntülemek için denetim. Bu işaretleme verileri görüntülemek için kullanılan yöntemler gibi yöntemleri kullanan *ProductList.aspx* sayfası. **FormView** denetimi, tek bir kaydı aynı anda bir veri kaynağından görüntülemek için kullanılır. Kullanırken **FormView** denetimi görüntülemek ve veri bağlama değerlerini düzenlemek için şablonlar oluşturun. Bu şablonlar ifadeleri, bağlama denetimleri içerir ve biçimlendirme formun görünümü ve yeni işlevleri tanımlayın.

Önceki biçimlendirme veritabanına bağlanırken, ek kod gerektirir.

1. İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* ve ardından **kodu görüntüle**.  
   *ProductDetails.aspx.cs* dosyası görüntülenir.

2. Varolan kodu şu kodla değiştirin:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Bu kod denetleyen bir "`productID`" sorgu dizesi değeri. Geçerli sorgu dize bulunursa eşleşen ürün görüntülenir. Sorgu-dize bulunamadığında veya değeri geçerli değil, hiçbir ürün görüntülenir.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Görüntülenen tek bir ürün görmek için uygulamayı çalıştırabilirsiniz artık ürün kimliğini temel alan

1. Tuşuna **F5** uygulamayı çalıştırmak için Visual Studio çalışırken.  
   Tarayıcı açılır ve gösterir *Default.aspx* sayfası.

2. Seçin **Boats** kategori Gezinti menüsünde.  
   *ProductList.aspx* sayfası görüntülenir.

3. Seçin **kağıt bot** ürün listesinde.
   *ProductDetails.aspx* sayfası görüntülenir.

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image4.png)
    
4. Tarayıcıyı kapatın.


## <a name="additional-resources"></a>Ek kaynaklar

[Model bağlama ve web forms ile verileri alma ve görüntüleme](../../presenting-and-managing-data/model-binding/retrieving-data.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, biçimlendirme ve ürünler ve ürün ayrıntıları görüntülemek için koddaki eklendi. Kesin türü belirtilmiş veri denetimleri, model bağlama ve değer sağlayıcıları hakkında bilgi edindiniz. Sonraki öğreticide, Wingtip Toys örnek uygulamaya bir alışveriş sepeti ekleyeceksiniz. 

> [!div class="step-by-step"]
> [Önceki](ui_and_navigation.md)
> [İleri](shopping-cart.md)
