---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
title: Görünen veri öğelerini ve ayrıntıları | Microsoft Docs
author: Erikre
description: Bu öğretici serisinin ASP.NET 4.5 ve Visual Studio 2013 Express için kullandığımız bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 64a491a8-0ed6-4c2f-9c1c-412962eb6006
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/display_data_items_and_details
msc.type: authoredcontent
ms.openlocfilehash: 083f7182416012c85f05db255fcab4d8e535b52a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820353"
---
<a name="display-data-items-and-details"></a>Görünen veri öğelerini ve ayrıntıları
====================
tarafından [Erik Reitan](https://github.com/Erikre)

[Wingtip Toys örnek projeyi (C#) indirin](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) veya [indirme E-kitabı (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Bu öğretici serisinin Web için ASP.NET 4.5 ve Visual Studio 2013 Express kullanarak bir ASP.NET Web Forms uygulaması oluşturmaya yönelik temel bilgiler sağlanır. Bir Visual Studio 2013'ün [C# kaynak kodu ile proje](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) Bu öğretici serisinin eşlik etmek üzere hazırdır.


Bu öğreticide, veri öğeleri ve ASP.NET Web Forms ve Entity Framework Code First kullanarak veri öğesi ayrıntılarını görüntülemek açıklar. Bu öğreticide, önceki öğreticide "Kullanıcı Arabirimi ve gezinti" oluşturur ve Wingtip çocuğunun Store öğretici serisinin bir parçasıdır. Bu öğreticiyi tamamladıktan sonra ürünleri görmek mümkün olacaktır *ProductsList.aspx* sayfa ve bir ürünün ayrıntılarını *ProductDetails.aspx* sayfası.

## <a name="what-youll-learn"></a>Öğrenecekleriniz:

- Veritabanından ürünleri görüntülemek için bir veri denetim ekleme.
- Veri denetimi seçilen verileri bağlanma.
- Veritabanından ürün ayrıntıları görüntülemek için bir veri denetim ekleme.
- Nasıl Sorgu dizesinden bir değer almak ve veritabanından alınan verileri sınırlamak için bu değeri kullanın.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Bu öğreticide sunulan özellikler şunlardır:

- Model bağlama
- Değer sağlayıcıları

## <a name="adding-a-data-control-to-display-products"></a>Ürünleri görüntülemek için bir veri denetim ekleme

Bir sunucu denetimine veri bağlama sırasında kullanabileceğiniz birkaç farklı seçenek vardır. En yaygın Seçenekler şunlardır: veri kaynağı denetim ekleme, kodu el ile ekleme veya model bağlama kullanarak.

### <a name="using-a-data-source-control-to-bind-data"></a>Veri bağlama için bir veri kaynağı denetimi kullanma

Veri kaynağı denetim ekleme, verileri görüntüleyen denetime veri kaynak denetimine bağlamak sağlar. Bu yaklaşım, sunucu tarafı denetimlerdir, doğrudan veri kaynakları için programlı bir yaklaşım kullanmak yerine bildirimli olarak bağlanmanıza olanak sağlar.

### <a name="coding-by-hand-to-bind-data"></a>Veri bağlama için el ile kodlama

Ekleme kodunu el ile ilgilidir değeri okuma, null değerini denetlemek, uygun türe dönüştürme yapma, dönüştürme başarılı olup olmadığını denetleme ve son olarak, sorguyu değeri'ı kullanarak. Veri erişim mantığı üzerinde tam denetimi korumak gerektiğinde bu yaklaşımı kullanmanız.

### <a name="using-model-binding-to-bind-data"></a>Model veri bağlamak için bağlama kullanma

Model bağlama kullanarak, çok daha az kod kullanarak sonuçları bağlamanıza olanak sağlar ve uygulamanızda işlevini yeniden kullanma olanağı sağlar. Model bağlama, kod odaklı veri erişim mantığı ile çalışma, yine de bir zengin, veri bağlama çerçevesi avantajlarını korurken basitleştirmek için amaçlar.

## <a name="displaying-products"></a>Ürünleri görüntüleme

Bu öğreticide, veri bağlama için model bağlama kullanacaksınız. Model bağlama verileri seçmek için kullanılacak veri denetimi yapılandırmak için denetimin ayarlayın. `SelectMethod` özelliğini sayfanın kodda bir yöntemin adı. Veri denetimi, sayfa yaşam döngüsü içinde uygun zamanda yöntemini çağırır ve döndürülen verileri otomatik olarak bağlar. Açıkça çağırmaya gerek yoktur `DataBind` yöntemi.

Aşağıdaki adımları kullanarak, işaretlemede değiştireceksiniz *ProductList.aspx* sayfa ürünleri görüntüleyebilmesi sayfa.

1. İçinde **Çözüm Gezgini**açın *ProductList.aspx* sayfası.
2. Mevcut biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample1.aspx)]

Bu kod bir **ListView** ürünleri görüntülemek için "productList" adlı Denetim.

[!code-aspx[Main](display_data_items_and_details/samples/sample2.aspx)]

**ListView** denetim şablonları ve stilleri kullanarak tanımladığınız bir biçimde verileri görüntüler. Tüm yinelenen yapısındaki veriler için kullanışlıdır. Bu **ListView** örnek yalnızca veritabanı, ancak kullanıcıların düzenlemek, eklemek ve verileri silmek ve sıralamak için etkinleştirebilirsiniz ve kod kullanmadan sayfa verileri verilerini gösterir.

Ayarlayarak `ItemType` özelliğinde **ListView** denetimine veri bağlama ifadesi `Item` kullanılabilir ve Denetim türü kesin belirlenmiş olur. Önceki öğreticide belirtildiği gibi ayrıntılarını belirtme gibi IntelliSense'i kullanarak öğesi nesnesi seçebileceğiniz `ProductName`:

![Veri öğelerini ve ayrıntıları - IntelliSense](display_data_items_and_details/_static/image1.png)

Ayrıca, belirtmek için model bağlama kullanmakta olduğunuz bir `SelectMethod` değeri. Bu değer (`GetProducts`) sonraki adımda ürünleri görüntülemek üzere arkasındaki kodda ekleyeceğiniz yöntemine karşılık gelir.

### <a name="adding-code-to-display-products"></a>Ürünleri görüntülemek için kod ekleme

Bu adımda, doldurmak için kod ekleyeceksiniz **ListView** ürün verileri veritabanından denetimi. Kod bireysel kategori yanı sıra tüm ürünleri gösteren gösteren ürünlerini destekler.

1. İçinde **Çözüm Gezgini**, sağ *ProductList.aspx* ve ardından **kodu görüntüle**.
2. Varolan kodda değiştirin *ProductList.aspx.cs* dosyasındaki kodu aşağıdaki kodla:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample3.cs)]

Bu kod gösterir `GetProducts` tarafından başvurulan yöntemi `ItemType` özelliği **ListView** denetim *ProductList.aspx* sayfası. Veritabanını belirli bir kategorideki sonuçları sınırlandırmak için kod ayarlar `categoryId` geçirilen sorgu dizesi değerini değerinden *ProductList.aspx* ne zaman sayfasında *ProductList.aspx* sayfası için gitme. `QueryStringAttribute` Sınıfını `System.Web.ModelBinding` ad alanı, sorgu dizesi değişkeni kimliği değerini almak için kullanılır. Bu model bağlama için Sorgu dizesinden bir değer bağlanmaya için bildirir `categoryId` çalışma zamanında parametresi.

Geçerli bir kategori sorgu dizesi olarak sayfasına geçildiğinde, sorgunun sonuçlarını eşleşen bu ürünlere veritabanında sınırlı `categoryId` değeri. Örneği için URL'ye *ProductsList.aspx* aşağıdaki sayfasıdır:

[!code-console[Main](display_data_items_and_details/samples/sample4.cmd)]

Sayfa yalnızca ürünleri görüntüler. burada `category` eşittir `1`.

Hiçbir sorgu dizesi dahil için gezinirken ise *ProductList.aspx* sayfasında, tüm ürünleri görüntülenir.

Bu yöntemlere ait değerlerin kaynakları olarak ifade edilir *değer sağlayıcıları* (gibi *QueryString*), ve kullanmak için hangi değer sağlayıcı gösterir parametre öznitelikleri için değer olarak adlandırılır Sağlayıcı öznitelikleri (örneğin "`id`"). ASP.NET Web Forms uygulamasında, sorgu dizesi, tanımlama bilgileri, form değerleri, denetimler, Görünüm durumu, oturum durumu ve profil özellikleri gibi değer sağlayıcıları ve tüm kullanıcı girişi tipik kaynakları için karşılık gelen öznitelikleri içerir. Özel değer sağlayıcıları da yazabilirsiniz.

### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Tüm ürünleri ya da yalnızca sınırlı bir kategoriye göre ürünler kümesini nasıl görüntüleyebileceğiniz görmek için uygulamayı şimdi çalıştırın.

1. İçinde **Çözüm Gezgini**, sağ *Default.aspx* sayfasından seçim yapıp **tarayıcıda görüntüle**.  
 Bir tarayıcı açın ve göstermek *Default.aspx* sayfası.
2. Seçin **otomobiller** ürün kategorisi Gezinti menüsünde.  
 *ProductList.aspx* sayfası, yalnızca "Arabalar" kategorisindeki ürünlerin gösteren görüntülenir. Bu öğreticide daha sonra ürün ayrıntıları görüntülenir.  

    ![Veri öğelerini ve ayrıntıları - otomobiller](display_data_items_and_details/_static/image2.png)
3. Seçin **ürünleri** üst gezinti menüsünde.  
 Yeniden *ProductList.aspx* sayfası görüntülenir, ancak isteğe bağlı olarak şu ürünlerin tam listesini gösterir.   

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image3.png)
4. Tarayıcıyı kapatın ve Visual Studio'ya geri dönün.

### <a name="adding-a-data-control-to-display-product-details"></a>Ürün Ayrıntıları görüntülemek için bir veri denetim ekleme

Ardından, işaretlemede değiştireceksiniz *ProductDetails.aspx* sayfa önceki öğreticide eklediğiniz ve böylece sayfa tek bir ürünle ilgili bilgileri görüntüleyebilirsiniz.

1. İçinde **Çözüm Gezgini**açın *ProductDetails.aspx* sayfası.
2. Mevcut biçimlendirme, aşağıdaki biçimlendirme ile değiştirin:   

    [!code-aspx[Main](display_data_items_and_details/samples/sample5.aspx)]

Bu kod bir **FormView** bir ürünün ayrıntılarını görüntülemek için denetimi. Bu biçimlendirme gibi verileri görüntülemek için kullanılan yöntemleri kullanan *ProductList.aspx* sayfası. **FormView** denetimi, tek bir kaydı aynı anda bir veri kaynağından görüntülemek için kullanılır. Kullanırken **FormView** denetimi görüntülemek ve veri bağlama değerlerini düzenlemek için şablonlar oluşturun. Şablonları formunun işlevselliği ve görünüm tanımlayan bağlama ifadeleri ve biçimlendirme denetimlerini içerir.

Yukarıdaki biçimlendirme veritabanına bağlanmak için ek kod ekleme *ProductDetails.aspx* kod.

1. İçinde **Çözüm Gezgini**, sağ *ProductDetails.aspx* ve ardından **kodu görüntüle**.  
   *ProductDetails.aspx.cs* dosyası görüntülenir.
2. Varolan kodu aşağıdaki kodla değiştirin:   

    [!code-csharp[Main](display_data_items_and_details/samples/sample6.cs)]

Bu kod denetleyen bir "`productID`" sorgu dizesi değeri. Geçerli sorgu dize bulunursa eşleşen ürün görüntülenir. Sorgu-dize bulunduğunda veya sorgu-dize değeri geçerli değil, hiçbir ürün görüntülenen *ProductDetails.aspx* sayfası.

### <a name="running-the-application"></a>Uygulamayı Çalıştırma

Görüntülenen tek bir ürün görmek için uygulamayı çalıştırabilirsiniz artık ürün kimliğine dayanarak.

1. Tuşuna **F5** uygulamayı çalıştırmak için Visual Studio çalışırken.  
 Bir tarayıcı açın ve göstermek *Default.aspx* sayfası.
2. Kategori Gezinti menüsünden "Boats" seçin.  
 *ProductList.aspx* sayfası görüntülenir.
3. "Kağıt bot" Ürün ürün listeden seçin.  
 *ProductDetails.aspx* sayfası görüntülenir.   

    ![Veri öğelerini ve ayrıntıları - ürünleri](display_data_items_and_details/_static/image4.png)
4. Tarayıcıyı kapatın.

## <a name="summary"></a>Özet

Bu öğretici serisinin sahip eklediğiniz biçimlendirme ve ürün listesini görüntülemek ve ürün ayrıntıları görüntülemek için koddaki. Bu işlem sırasında kesin türü belirtilmiş veri denetimleri, model bağlama ve değer sağlayıcıları hakkında öğrendiniz. Sonraki öğreticide, Wingtip Toys örnek uygulamaya bir alışveriş sepeti ekleyeceksiniz.

## <a name="additional-resources"></a>Ek Kaynaklar

[Model bağlama ve web forms ile verileri alma ve görüntüleme](../../presenting-and-managing-data/model-binding/retrieving-data.md)

> [!div class="step-by-step"]
> [Önceki](ui_and_navigation.md)
> [İleri](shopping-cart.md)
