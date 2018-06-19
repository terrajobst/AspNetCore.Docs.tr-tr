---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görüntü veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 biz için kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 5fea654aa5116193cb7496c1b9020ed8e25fc06f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892575"
---
<a name="display-data-items-and-details"></a>Görüntü veri öğelerini ve ayrıntıları
====================
Tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek proje (C#) karşıdan](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [karşıdan E-kitap (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici seri ASP.NET 4.5 ve Microsoft Visual Studio Express 2013 için Web kullanarak bir ASP.NET Web Forms uygulaması oluşturma temellerini öğretmek. Visual Studio 2013 [C# kaynak kodu projeyle](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici seri eşlik etmek üzere hazırdır.


Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First kullanarak veri öğesi ayrıntılarını görüntülemek açıklar. Bu öğretici önceki öğreticiyi "Kullanıcı Arabirimi ve gezinti" oluşturur ve Wingtip Oyuncak deposu öğretici serinin bir parçasıdır. Bu öğreticiyi tamamladıktan sonra ürünleri görmek kullanabileceksiniz *ProductsList.aspx* sayfası ve tek tek bir üründe ayrıntılarını *ProductDetails.aspx* sayfası.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Veritabanından ürünleri görüntülemek için bir veri denetim ekleme.
- Bir veri denetimi seçilen verileri bağlanma.
- Veritabanından ürün ayrıntıları görüntülemek için bir veri denetim ekleme.
- Nasıl Sorgu dizesinden bir değer almak ve veritabanından alınan veri sınırlamak için bu değeri kullanın.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Bu öğreticide sunulan özellikler şunlardır:

- Model Binding
- Değer sağlayıcıları

## <a name="adding-a-data-control-to-display-products"></a>Ürünleri görüntülemek için veri denetim ekleme

Sunucu denetimine veri bağlama sırasında kullanabileceğiniz birkaç farklı seçenekler vardır. En yaygın Seçenekler şunlardır: bir veri kaynağı denetimi ekleme, kod el ile ekleme veya model bağlama kullanarak.

### <a name="using-a-data-source-control-to-bind-data"></a>Veri bağlama için bir veri kaynağı denetimi kullanma

Veri kaynağı denetim ekleme verileri görüntüler denetimi veri kaynağı denetimi bağlantı sağlar. Bu yaklaşım bildirimli olarak sunucu tarafı denetimleri doğrudan veri kaynakları için programlı bir yaklaşım kullanmak yerine bağlanmanıza olanak sağlar.

### <a name="coding-by-hand-to-bind-data"></a>Veri bağlamak için el ile kodlama

El ile kod içerir ekleme değeri okunurken, null değerini denetimi, uygun türe dönüştürme yapma, dönüştürme başarılı olup olmadığını denetleme ve son olarak, sorguyu değeri'ı kullanarak. Veri erişimi mantığınızı üzerinde tam denetimi korumak istediğinizde bu yaklaşımı kullanırsınız.

### <a name="using-model-binding-to-bind-data"></a>Veri bağlama bağlama modelini kullanma

Model bağlama kullanarak, çok daha az kod kullanarak sonuçları bağlamanıza olanak sağlar ve uygulamanız genelinde işlevini yeniden kullanma olanağı sağlar. Bir zengin, veri bağlama çerçeve yararları hala korurken kod odaklı veri erişimi mantığı ile çalışma basitleştirmek için model bağlama amaçlar.

## <a name="displaying-products"></a>Ürünler görüntüleme

Bu öğreticide, veri bağlamak için model bağlama kullanacaksınız. Model bağlama verileri seçmek için kullanılacak veri denetimini yapılandırmak için denetimin ayarlayın `SelectMethod` özelliğini sayfanın kodda bir yöntemin adı. Veri denetimi sayfa yaşam döngüsündeki uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. Açıkça çağırmak için gerek yoktur `DataBind` yöntemi.

Aşağıdaki adımları kullanarak, biçimlendirmede değiştireceksiniz *ProductList.aspx* sayfa ürünleri görüntüleyebilmesi sayfa.

1. İçinde **Çözüm Gezgini**, açık *ProductList.aspx* sayfası.
2. Varolan biçimlendirme aşağıdaki biçimlendirme ile değiştirin:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Bu kodu kullanan bir **ListView** ürünleri görüntülemek için "productList" adlı bir denetim.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView** denetim şablonları ve stiller kullanarak tanımladığınız bir biçimde verileri görüntüler. Yinelenen bir yapı veriler için kullanışlıdır. Bu **ListView** örnek yalnızca veritabanı, ancak kullanıcılar düzenleme, ekleme ve verileri silmek ve sıralamak için etkinleştirebilir ve tüm Kodsuz sayfa verileri verilerini gösterir.

Ayarlayarak `ItemType` özelliğinde **ListView** denetlemek, veri bağlama ifadesi `Item` kullanılabilir ve denetim kesin türü belirtilmiş haline gelir. Önceki öğreticide belirtildiği gibi IntelliSense, belirtme gibi kullanarak öğesi nesnesi ayrıntılarını seçebilirsiniz `ProductName`:

![Görüntüleme veri öğeleri ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

Ayrıca, belirtmek için model bağlama kullanmakta olduğunuz bir `SelectMethod` değeri. Bu değer (`GetProducts`) sonraki adımda ürünler görüntülenecek arkasındaki kodda ekleyeceksiniz yöntemi karşılık gelir.

### <a name="adding-code-to-display-products"></a>Ürünleri görüntülemek için kod ekleme

Bu adımda, doldurmak için kod ekleyeceksiniz **ListView** ürün verileri veritabanından denetimiyle. Kodu gösterme ürünleri tek tek Kategori yanı sıra tüm ürünleri gösteren tarafından destekleyecektir.

1. İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* ve ardından **görünümü kodu**.
2. Varolan kodla *ProductList.aspx.cs* aşağıdaki kod ile dosya:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Bu kod gösterir `GetProducts` tarafından başvurulan yöntemi `ItemType` özelliği **ListView** denetim *ProductList.aspx* sayfası. Veritabanı belirli bir kategorideki sonuçları sınırlamak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerini değerinden *ProductList.aspx* ne zaman sayfa *ProductList.aspx* sayfası için gittiğinizde. `QueryStringAttribute` Sınıfını `System.Web.ModelBinding` ad alanı, sorgu dizesi değişkeni kimliği değerini almak için kullanılır. Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya bildirir `categoryId` çalışma zamanında parametre.

Geçerli bir kategori sayfasına bir sorgu dizesi geçirildiğinde, sorgunun sonuçlarını uyan ürünleri için veritabanında sınırlı `categoryId` değeri. Örneği için URL'ye *ProductsList.aspx* sayfasıdır aşağıdaki:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Yalnızca ürün sayfasını görüntüler nerede `category` eşittir `1`.

Hiçbir sorgu dizesi için gezinirken eklenirse *ProductList.aspx* sayfası, tüm ürünleri görüntülenir.

Bu yöntemlere ait değerlerin kaynakları denir *değer sağlayıcıları* (gibi *QueryString*), ve kullanacağı değer sağlayıcısını belirtmek parametre öznitelikleri değeri olarak adlandırılır Sağlayıcı öznitelikleri (gibi "`id`"). ASP.NET Web Forms uygulamasında, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimleri, Görünüm durumu, oturum durumu ve profil özellikleri gibi değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen öznitelikleri içerir. Özel değer sağlayıcıları de yazabilirsiniz.

### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Tüm ürünleri veya ürün kategorisine göre sınırlı yalnızca bir dizi nasıl görüntüleyebileceğiniz görmek için uygulamayı şimdi çalıştırın.

1. İçinde **Çözüm Gezgini**, sağ *Default.aspx* sayfasından seçim yapıp **tarayıcıda görüntüle**.  
 Tarayıcıyı açın ve göstermek *Default.aspx* sayfası.
2. Seçin **araba** ürün kategorisi Gezinti menüsünde.  
 *ProductList.aspx* sayfası, yalnızca "Araba" kategorisindeki ürünleri gösteren görüntülenir. Daha sonra Bu öğreticide, ürün ayrıntıları görüntüler.  

    ![Görüntüleme veri öğeleri ve ayrıntıları - araba](display_data_items_and_details/_static/image2.png)
3. Seçin **ürünleri** üst gezinti menüsünde.  
 Yeniden *ProductList.aspx* sayfası görüntülenir, ancak isteğe bağlı olarak bu zaman tüm ürünlerin listesini gösterir.   

    ![Görüntüleme veri öğeleri ve ayrıntıları - ürünler](display_data_items_and_details/_static/image3.png)
4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

### <a name="adding-a-data-control-to-display-product-details"></a>Ürün Ayrıntıları görüntülemek için veri denetim ekleme

Ardından, biçimlendirmede değiştireceksiniz *ProductDetails.aspx* sayfa ayrı bir ürün hakkında bilgi görüntüleyebilmesi önceki öğreticide eklediğiniz sayfası.

1. İçinde **Çözüm Gezgini**, açık *ProductDetails.aspx* sayfası.
2. Varolan biçimlendirme aşağıdaki biçimlendirme ile değiştirin:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Bu kodu kullanan bir **FormView** ayrı bir ürün hakkındaki ayrıntıları görüntülemek için denetim. Bu biçimlendirme verileri görüntülemek için kullanılan gelenler yöntemleri kullanır *ProductList.aspx* sayfası. **FormView** denetimi, bir veri kaynağından bir kerede tek bir kaydını görüntülemek için kullanılır. Kullandığınızda **FormView** denetimi, görüntüle ve veri bağlama değerlerini düzenlemek için şablonlar oluşturma. Şablonları görünümünü ve form işlevselliğini tanımlayan ifadeleri bağlama ve biçimlendirme denetimleri içerir.

Yukarıdaki biçimlendirme veritabanına bağlanmak için ek kod ekleme *ProductDetails.aspx* kodu.

1. İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* ve ardından **görünümü kodu**.  
   *ProductDetails.aspx.cs* dosya görüntülenir.
2. Var olan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Bu kod denetler bir "`productID`" sorgu dizesi değeri. Geçerli sorgu dize bulunursa, eşleşen ürün görüntülenir. Hiçbir sorgu dizesi bulunamadı veya sorgu dizesi değeri geçerli değil, hiçbir ürün görüntülenen *ProductDetails.aspx* sayfası.

### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Görüntülenen bir bireysel ürün görmek için uygulamayı çalıştırın artık ürün kimliği temel alarak.

1. Tuşuna **F5** while uygulamayı çalıştırmak için Visual Studio.  
 Tarayıcıyı açın ve göstermek *Default.aspx* sayfası.
2. "Boats" kategorisi Gezinti menüsünden seçin.  
 *ProductList.aspx* sayfası görüntülenir.
3. "Kağıt bot" Ürün ürün listeden seçin.  
 *ProductDetails.aspx* sayfası görüntülenir.   

    ![Görüntüleme veri öğeleri ve ayrıntıları - ürünler](display_data_items_and_details/_static/image4.png)
4. Tarayıcıyı kapatın.

## <a name="summary"></a>Özet

Serinin Bu öğreticide sahip eklediğiniz biçimlendirme ve kodun ürün listesini görüntülemek ve ürün ayrıntıları görüntülemek için. Bu işlem sırasında kesin türü belirtilmiş veri denetimleri, model bağlama ve değer sağlayıcıları hakkında öğrendiniz. Sonraki öğreticide Wingtip Toys örnek uygulamaya alışveriş sepeti ekleyeceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[Model bağlama ve web forms ile verilerini görüntüleme ve alma](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Önceki](ui_and_navigation.md)
> [sonraki](shopping-cart.md)
