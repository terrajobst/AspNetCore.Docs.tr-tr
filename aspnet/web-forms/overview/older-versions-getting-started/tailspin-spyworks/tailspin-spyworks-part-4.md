---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: '4. Kısım: Ürünleri listeleme | Microsoft Docs'
author: JoeStagner
description: Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 4 GridView Sözl listeleme ürünleriyle kapsayan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a>4. Kısım: Listeleme ürünleri
====================
tarafından [CAN Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks .NET platformu için güçlü, ölçeklendirilebilir uygulamalar oluşturun nasıl alıyoruz basit gerçekleştiğini gösterir. Alışveriş, kullanıma alma ve yönetim dahil olmak üzere bir çevrimiçi mağaza oluşturmak için ASP.NET 4'te harika yeni özellikleri kullanmak nasıl devre dışı gösterir.
> 
> Bu öğretici seri Tailspin Spyworks örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 4 GridView denetimi ile listeleme ürünleri kapsar.


## <a id="_Toc260221670"></a>  GridView denetiminin ürünleriyle listeleme

"Sağ tıklayarak" ProductsList.aspx sayfamızı uygulama çözümümüzdür ve "Ekle" ve "Yeni öğesi" seçme başlayalım.

![](tailspin-spyworks-part-4/_static/image1.jpg)

"Web formu kullanarak ana sayfa" seçin ve ProductsList.aspx sayfa adını girin".

"Ekle" yi tıklatın.

![](tailspin-spyworks-part-4/_static/image2.jpg)

Sonraki biz Site.Master sayfa nereye yerleştirileceğini "Stilleri" klasörü seçin ve "Klasörünün içeriğini" penceresinden seçin.

![](tailspin-spyworks-part-4/_static/image3.jpg)

Sayfayı oluşturmak için "Tamam"'i tıklatın.

Veritabanımıza aşağıda görüldüğü gibi ürün verilerle doldurulur.

![](tailspin-spyworks-part-4/_static/image4.jpg)

Sayfamızı oluşturulduktan sonra yeniden bir varlık veri kaynağı bu ürün verilere erişmek için kullanacağız ancak bu örnek Ürün varlıklar seçmeliyiz ve yalnızca seçilen kategori için döndürülürsünüz öğeleri kısıtlamak ihtiyacımız.

Bunu gerçekleştirmek için otomatik oluşturmak için EntityDataSource WHERE yan tümcesi anlatılır ve WhereParameter belirtmeniz.

Menü öğeleri bizim "ürün kategorisi menüde" oluşturduğumuz zaman dinamik olarak bağlantı her bağlantı için sorgu dizesi CatagoryID ekleyerek oluşturduğumuz olduğunu Hatırlayacağınız. Biz bu sorgu dizesi parametresi WHERE parametre çıkarmaya varlık veri kaynağı söyler.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

Ardından, ürünlerin listesini görüntülemek için ListView denetimi yapılandıracağız. En iyi bir alışveriş deneyimi oluşturmak için size çeşitli kısa özellikler bizim ListVew görüntülenen her ürünün düzenlenecek.

- Ürün adı, ürünün ayrıntı görünümü için bir bağlantı olur.
- Ürünün fiyat görüntülenir.
- Ürün görüntüsü görüntülenir ve biz dinamik olarak görüntünün bir kataloğu resimleri dizinden bizim uygulamada seçersiniz.
- Biz hemen ürününün alışveriş sepetine eklemek için bir bağlantı içerir.

Bizim ListView denetimi örneği için biçimlendirme aşağıdaki gibidir.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

Biz, dinamik olarak görüntülenen her ürün için birkaç bağlantıları oluşturmakta olduğunuz.

Ayrıca, kendi yeni sayfa test ederiz önce biz ürün için dizin yapısını Kataloğu resimleri şu şekilde oluşturmanız gerekir.

![](tailspin-spyworks-part-4/_static/image1.png)

Bizim ürün görüntüleri erişilebilir olduğunda size ürün listesi sayfamızı test edebilirsiniz.

![](tailspin-spyworks-part-4/_static/image5.jpg)

Sitenin giriş sayfasından kategori listesi bağlantılardan birini tıklatın.

![](tailspin-spyworks-part-4/_static/image6.jpg)

Şimdi biz ProductDetials.apsx sayfası ve AddToCart işlevselliği uygulamanız gerekir.

Dosya - kullanın&gt;sayfa adı daha önce yaptığımız gibi sitenin ana sayfa kullanarak ProductDetails.aspx oluşturmak için yeni.

Biz yeniden EntityDataSource denetim veritabanında belirli bir ürün kaydı erişmek için kullanır ve bir ASP.NET FormView denetimi gibi ürün verileri görüntülemek için kullanacağız.

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

Biçimlendirme biraz size komik değilse endişelenmeyin. Yukarıdaki biçimlendirme görüntüleme düzeni odada birkaç biz daha sonra uygulama özelliklerinin bırakır.

Alışveriş sepetine uygulamamız daha karmaşık mantık temsil eder. Başlamak için dosya - kullanın&gt;MyShoppingCart.aspx adlı bir sayfa oluşturmak için yeni.

Biz ShoppingCart.aspx adı almadığınızdan unutmayın.

Veritabanımıza "Alışveriş sepeti" adlı bir tablo içeriyor. Biz bir varlık veri modeli oluşturulan seçtiğinizde bir sınıf veritabanındaki her tablo için oluşturulmuştur. Bu nedenle, varlık veri modeli "Alışveriş sepeti" adlı bir varlık sınıfı oluşturulur. Model böylece biz bizim alışveriş sepeti uygulaması için bu adı kullanın veya bizim ihtiyaçları için genişletme ancak biz yalnızca farklı çakışmayı önlemek adı yerine benimseyeceksiniz düzenleyebilirsiniz.

Ayrıca biz basit alışveriş sepeti oluşturacağınız belirtmeye ve alışveriş sepeti mantığı ile alışveriş sepeti görüntüle katıştırma değer değildir. Biz alışveriş sepetimizi tamamen ayrı bir iş Katmanı'nı uygulamak seçebilirsiniz.

> [!div class="step-by-step"]
> [Önceki](tailspin-spyworks-part-3.md)
> [sonraki](tailspin-spyworks-part-5.md)
