---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görünen veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin Web ASP.NET 4.7 ve Microsoft Visual Studio Community 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri sağlanır.
ms.author: riande
ms.date: 1/09/2019
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 73ae1660f5d6e3e28c1c155e745a62936e3502df
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207440"
---
<a name="display-data-items-and-details"></a>Görünen veri öğelerini ve ayrıntıları
====================
tarafından [Erik Reitan](https://github.com/Erikre)

> Bu öğretici serisinde, Web için ASP.NET 4.7 ve Microsoft Visual Studio Community 2017 ile bir ASP.NET Web Forms uygulaması oluşturmaya ilişkin temel bilgileri size öğretir.

Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First ile veri öğesi ayrıntılarını görüntülemek nasıl öğrenin. Bu öğreticide, Wingtip çocuğunun Store öğretici serisinin bir parçası olarak önceki "Kullanıcı Arabirimi ve gezinti" öğreticisini üzerine inşa edilmiştir. Tamamlanmış bir öğreticide, ürünlerin *ProductsList.aspx* sayfa ve bir ürün uygulamasının Ayrıntılar *ProductDetails.aspx* sayfası görüntülenir.

## <a name="what-you-learn"></a>Öğrenecekleriniz

- Veritabanı ürünleri görüntülemek için bir veri denetim ekleyin.
- Veri denetimi, seçilen verilere bağlanın.
- Ürün ayrıntılarını görüntülemek için bir veri denetim ekleyin.
- Bir sorgu dizesi değerini ayrıştırabilir ve alınan veritabanı verilerini filtrelemek için kullanın.

Bu öğreticide sunulan özellikler, model bağlama ve değer sağlayıcılarını içerir.

## <a name="add-a-data-control-to-display-products"></a>Ürünleri görüntülemek için bir veri denetim ekleme
 
Bir sunucu denetimine veri bağlama için birkaç seçeneğiniz vardır. En yaygın şunlardır:

 * Veri kaynak denetimi ekleme
 * Kodu el ile ekleme
 * Model bağlama uygulama

### <a name="use-a-data-source-control-to-bind-data"></a>Veri bağlama için bir veri kaynağı denetimi kullanma

Veri kaynak denetimi ekleme veri kaynak denetimi veri görüntüleyen denetimine bağlar. Bu yaklaşım, bildirimli olarak yapabilirsiniz yerine programlı olarak sunucu tarafı denetimlerdir veri kaynaklarına bağlanın.

### <a name="code-by-hand-to-bind-data"></a>Kodu el ile veri bağlama

El ile kodlama içerir:

1. Bir değer okuma
2. Null olup olmadığı denetleniyor
3. Uygun bir türe dönüştürme
4. Dönüştürme başarılı denetleniyor
5. Bir sorgu dönüştürülen değer ile yapma 

Bu yaklaşımda, veri erişim mantığı üzerinde tam denetime sahip.

### <a name="use-model-binding-to-bind-data"></a>Model bağlama veri bağlamak için kullanın

Model bağlama ile sonuçları çok daha az kod ile bağlama ve uygulamanızda işlevini yeniden kullanma olanağı sağlar. Kod odaklı veri erişim mantığı ile çalışma, yine de bir zengin, veri bağlama çerçevesi sağlarken basitleştirir.

## <a name="display-products"></a>Ürünleri görüntülemek

Bu öğreticide, veri bağlama için model bağlama kullanın. Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için denetimin ayarlayın. `SelectMethod` özelliği bir yönteme sayfanın kod. Veri denetimi, sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. Açıkça çağırmaya gerek yoktur `DataBind` yöntemi.

Aşağıdaki adımları izleyerek çalışmaya, değişiklik *ProductList.aspx* ürünleri görüntülemek üzere biçimlendirme.

1. İçinde **Çözüm Gezgini**açın *ProductList.aspx*.

2. Mevcut biçimlendirme, aşağıdaki biçimlendirme ile değiştirin: 

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample1.aspx)]

Önceki biçimlendirme kullanan bir **ListView** adlı Denetim `productList` ürünleri görüntülemek için.

[!code-aspx-csharp[Main](display_data_items_and_details/samples/sample2.aspx)]

Şablonları ve stilleri ile tanımladığınız nasıl **ListView** denetim verilerini görüntüler. Tüm yinelenen yapısındaki veriler için kullanışlıdır. Ancak bu **ListView** örnek yalnızca veritabanı verileri görüntüler, ayrıca, kodu olmadan etkinleştir kullanıcıların düzenlemek, eklemek ve verileri silmek için ve sıralama ve veri sayfasını kullanabilirsiniz.

Ayarladığınızda `ItemType` özelliğinde **ListView** denetimine veri bağlama ifadesi `Item` kullanılabilir ve Denetim türü kesin belirlenmiş olur. Önceki öğreticide belirtildiği gibi belirtme gibi IntelliSense ile öğesi nesne ayrıntıları seçebilirsiniz `ProductName`:

![Veri öğelerini ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

Model bağlamayla belirtme bir `SelectMethod` değeri (`GetProducts`). Bu koda Ekle yöntemi, arkasından sonraki adımda ürünleri görüntülemek için.

### <a name="add-code-to-display-products"></a>Ürünleri görüntülemek için kod ekleme

Bu adımda, doldurmak için kod ekleme **ListView** denetimi ile veritabanı ürün verileri. Tüm ürünler ve tek bir kategori ürünler gösteren kod destekler.

1. İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* seçip **kodu görüntüle**.
2. Varolan kodda değiştirin *ProductList.aspx.cs* bu dosya:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Bu kod gösterir `GetProducts` yöntemi, **ListView** denetimin `ItemType` özelliği başvurularının *ProductList.aspx*. Sonuçları belirli veritabanı kategorisi sınırlamak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerinden *ProductList.aspx*. `QueryStringAttribute` Sınıfını `System.Web.ModelBinding` sorgu dizesi değişkeni almak için kullanılan ad alanı `id`değerini. Bu model bağlama için bir sorgu dizesi değeri çalışma zamanında bağlamak için bildirir `categoryId` parametresi.

Geçerli bir kategori olduğunda (`categoryId`) olan sonuçlarını geçti, o kategorinin veritabanı ürünler için sınırlı. Örneğin, *ProductsList.aspx* sayfası URL'si şudur:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Sayfa yalnızca ürünleri görüntüler. burada `categoryId` eşittir `1`.

Hiçbir sorgu dizesi geçirilen tüm ürünleri görüntülenir.

Bu yöntemlere ait değer kaynaklarının adlı *değer sağlayıcıları* (gibi `QueryString`), ve kullanmak için hangi değer sağlayıcı gösterir parametre öznitelikleri adlı *değer sağlayıcısı öznitelikleri* () gibi `id`). ASP.NET değer sağlayıcıları ve tüm tipik Web Forms uygulaması kullanıcı giriş kaynakları özniteliklerini içerir. Bunlar, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimler, Görünüm durumu, oturum durumu ve profil özelliklerini içerir. Özel değer sağlayıcıları da yazabilirsiniz.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Tüm ürünler ya da bir kategorinin ürünler görüntülemek için uygulamayı şimdi çalıştırın.

1. Visual Studio'da **F5** uygulamayı çalıştırın.
 Tarayıcı açılır ve gösterir *Default.aspx* sayfası.

2. Ürün kategorisi menüden **otomobiller**.

   *ProductList.aspx* sayfası görüntülenirse, yalnızca ürünleri gösteren **otomobiller** kategorisi. Bu öğreticide daha sonra ürün ayrıntıları görüntüler.

    ![Veri öğelerini ve ayrıntıları - otomobiller](display_data_items_and_details/_static/image2.png)

3. Seçin **ürünleri** üstteki menüden.
 *ProductList.aspx* sayfası artık tüm ürünleri görüntüler. 

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image3.png)

4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

### <a name="add-a-data-control-to-display-product-details"></a>Ürün ayrıntılarını görüntülemek için bir veri denetim ekleme

Değiştirme *ProductDetails.aspx* biçimlendirme belirli ürün bilgilerini görüntülemek için önceki öğreticide eklendi:

1. İçinde **Çözüm Gezgini**açın *ProductDetails.aspx*.

2. Mevcut biçimlendirme, bu işaretleme ile değiştirin:

    [!code-aspx-csharp[Main](display_data_items_and_details/samples/sample5.aspx)] 

Bu işaretleme kullanan bir **FormView** belirli bir ürün ayrıntılarını görüntülemek için denetim. Verileri görüntülemek için kullanılanlar gibi yöntemleri kullanan *ProductList.aspx*. **FormView** denetimi, tek bir kaydı aynı anda bir veri kaynağından görüntülemek için kullanılır. Kullanırken **FormView** denetimi görüntülemek ve veri bağlama değerlerini düzenlemek için şablonlar oluşturun. Bu şablonlar ifadeleri, bağlama denetimleri içerir ve biçimlendirme formun görünümü ve yeni işlevleri tanımlayın.

Önceki biçimlendirme veritabanına bağlanırken, ek kod gerektirir.

1. İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* seçip **kodu görüntüle**.  
   *ProductDetails.aspx.cs* dosyası görüntülenir.

2. Mevcut kodu şununla değiştirin:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Bu kod denetleyen bir "`productID`" sorgu dizesi değeri. Geçerli bir değer bulunursa eşleşen ürün görüntülenir. Sorgu dizesi bulunamadığında veya değeri geçerli değil, hiçbir ürün görüntülenir.

### <a name="run-the-application"></a>Uygulamayı çalıştırma

Ürün kimliği temel alınarak belirli ürün ayrıntıları görmek için uygulamayı Şimdi Çalıştır

1. Visual Studio'da **F5** uygulamayı çalıştırın.  
 Tarayıcı açılır *Default.aspx*.

2. Kategori menüden **Boats**.  
 *ProductList.aspx* sayfası görüntülenir.

3. Seçin **kağıt bot**.  
 *ProductDetails.aspx* sayfası görüntülenir.   

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image4.png)

Sonraki öğreticide, Wingtip Toys uygulamaya alışveriş sepetine ekleyin.

## <a name="additional-resources"></a>Ek kaynaklar

[Model bağlama ve web forms ile verileri alma ve görüntüleme](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Önceki](ui_and_navigation.md)
> [İleri](shopping-cart.md)
