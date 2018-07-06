---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Verimli veri sayfalama uygulama | Microsoft Docs
author: microsoft
description: 8. adım, böylece aynı anda azalma 1000 yerine yalnızca 10 yaklaşan azalma, görüntüleyeceğiz bizim /Dinners URL'sine sayfalama desteği ekleme işlemi açıklanır...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: bcd7fdf59fac8328752aa2ebab61c1d50a8b6b0d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842149"
---
<a name="implement-efficient-data-paging"></a>Verimli veri sayfalama uygulama
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 8 bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 8. adım, disk belleği desteği, bizim /Dinners URL'sine ekleyebilirsiniz, böylece aynı anda azalma 1000 görüntülemek yerine, biz yalnızca teker teker - 10 yaklaşan azalma görüntülemek ve geri sayfasında ve listenin tamamını SEO kolay bir şekilde iletmek son kullanıcılara izin ver işlemi gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner adım 8: Disk belleği desteği

Sitemizi başarılı olursa, yaklaşan azalma binlerce olacaktır. Kullanıcı Arabirimimizi tüm bu azalma ölçeklendirir ve bunları göz atmak kullanıcılara emin olmanız gerekir. Bunu etkinleştirmek için sayfalama desteği ekleyeceğiz bizim */Dinners* azalma 1000'lik bir kez biz görüntüleme URL'si, bunun yerine yalnızca teker teker - 10 yaklaşan azalma görüntüle ve geri sayfasında ve listenin tamamını üzerinden iletmek son kullanıcılara izin ver bir SEO kolay yolu.

### <a name="index-action-method-recap"></a>İNDİS() eylem yöntemi özeti

Bizim DinnersController sınıf içindeki İNDİS() eylem yönteminin şu anda aşağıdaki gibi görünür:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Ne zaman bir isteği yapılır */Dinners* URL, tüm gelecek azalma listesini alır ve ardından bunları tümünün listesini oluşturur:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Anlama IQuerable&lt;T&gt;

*Iqueryable&lt;T&gt;*  LINQ ile .NET 3.5 bir parçası olarak sunulan bir arabirimdir. Ancak biz sayfalama desteğini uygulamak için yararlanabilirsiniz güçlü "ertelenmiş yürütme" senaryolarını etkinleştirir.

Bizim DinnerRepository biz Iqueryable iade ettiğiniz&lt;Dinner&gt; dizisinden bizim FindUpcomingDinners() yöntemi:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Iqueryable&lt;Dinner&gt; bizim FindUpcomingDinners() yöntemi tarafından döndürülen nesne bizim veritabanından LINQ to SQL kullanarak Dinner nesneleri almak için bir sorgu kapsüller. Önemlisi, veritabanında erişim / sorgudaki verileri gezinilen denediğimiz kadar veya ToList() yöntemi üzerinde diyoruz kadar sorguyu olmaz. Bizim FindUpcomingDinners() yöntemini çağırarak kod ek "zincirleme" işlemler/filtreler Iqueryable'a eklemek için isteğe bağlı olarak seçebileceğiniz&lt;Dinner&gt; sorguyu çalıştırmadan önce nesne. LINQ to SQL ardından veri istendiğinde veritabanında birleşik sorguyu yürütmek akıllı bir hale gelir.

Döndürülen Iqueryable'a ek "Atla" ve "Take" işleçleri geçerli olacak şekilde sayfalama mantığını uygulamak için biz bizim DinnersController'ın İNDİS() eylem yöntemini güncelleştirme yetkisi&lt;Dinner&gt; ToList() çağrısından önce dizisi:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Yukarıdaki kod, veritabanındaki ilk 10 yaklaşan azalma üzerinden atlar ve 20 azalma geri döndürür. LINQ to SQL, bu mantıksal SQL veritabanı – ve değil web-server'ı atlama gerçekleştirir en iyi duruma getirilmiş bir SQL sorgusu oluşturmak akıllıca olur. Başka bir deyişle, biz yaklaşan azalma milyonlarca veritabanında olsa bile, yalnızca istediğimiz 10 (Bu etkili ve ölçeklenebilir hale) bu isteğin bir parçası alınır.

### <a name="adding-a-page-value-to-the-url"></a>URL "page" değer ekleme

Belirli sayfa aralığı kodlamak yerine, Url'lerimizi, kullanıcının isteme hangi Yemeği aralığı gösteren "page" parametre eklemek isteyeceksiniz.

#### <a name="using-a-querystring-value"></a>Bir sorgu dizesi değerini kullanarak

Aşağıdaki kod, biz bir sorgu dizesi parametresi desteği ve URL'leri gibi etkinleştirmek için sunduğumuz İNDİS() eylem yöntemine nasıl güncelleştirebilirsiniz gösterir */Dinners? sayfa = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Yukarıdaki İNDİS() eylem yöntemi, "page" adlı bir parametreye sahiptir. Parametresi boş değer atanabilir bir tamsayı olarak bildirildi (ne Int olan? gösterir). Diğer bir deyişle */Dinners? sayfa = 2* URL parametre değeri olarak geçirilecek "2" değerini neden olur. */Dinners* URL (olmadan bir sorgu dizesi değeri) bir null değer geçirilmesine neden olur.

Biz sayfası değeri sayfa boyutunu (Bu durumda 10. satır) üzerinden atlamak için kaç azalma belirlemek için çarparak. Kullanıyoruz ["C# null birleştirme" işleci (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) boş değer atanabilir türler ile ilgilenirken kullanışlı olduğu. Sayfa parametre null ise Yukarıdaki kod sayfası 0 değeri atar.

#### <a name="using-embedded-url-values"></a>Katıştırılmış URL değerlerini kullanma

Gerçek URL içinde sayfa parametre eklemek için bir sorgu dizesi değerini kullanarak bir alternatif olacaktır. Örneğin: */Dinners/Page/2* veya */azalma/2*. ASP.NET MVC böyle senaryoları desteklemek kolaylaştıran güçlü bir yönlendirme URL'si altyapısını içerir.

Size özel yönlendirme kuralları gelen URL veya URL format istediğimiz bir denetleyici sınıfı veya eylem yöntemi için eşlenen kaydedebilir. Yapılacaklar ihtiyacımız olan Projemizin içinde Global.asax dosyası açmak için:

![](implement-efficient-data-paging/_static/image2.png)

Ve ardından rotaları ilk çağrıda gibi MapRoute() yardımcı yöntemi kullanarak yeni eşleme kuralı kaydedin. Aşağıdaki MapRoute():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Yukarıdaki biz "UpcomingDinners" adlı yeni bir yönlendirme kuralı kaydettirmekte olduğunuz. URL biçimi olan biz gösteren "azalma/sayfa / {sayfasında}" – {sayfası} URL'si içinde katıştırılmış bir parametre değeri olduğu. Üçüncü parametre MapRoute() yönteme DinnersController sınıfındaki İNDİS() eylem yöntemi için bu biçim ile eşleşmesi URL'leri eşlemelisiniz gösterir.

URL ve sorgu dizesi değil, "page" parametresi artık gelir dışında önce Querystring senaryomuz – vardı tam aynı İNDİS() kod kullanabiliriz:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Ve artık size uygulamayı çalıştırın ve yazın */Dinners* ilk 10 yaklaşan azalma görüyoruz:

![](implement-efficient-data-paging/_static/image3.png)

Ve biz yazdığınızda */Dinners/Page/1* azalma sonraki sayfasına görüyoruz:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Sayfa gezintisi kullanıcı Arabirimi ekleme

"İleri" ve "önceki" kullanıcı Arabirimi içinde gezinme Şimdi Akşam verilerinde kolayca atlamak kullanıcıları etkinleştirmek için sunduğumuz görünüm şablonu uygulamak için disk belleği senaryomuz tamamlamak için son adım olacaktır.

Aşağıdaki ifadeyi veri birçok sayfaları için nasıl bunu doğru uygulamaya, biz azalma toplam sayısı veritabanındaki bilmeniz de gerekecektir. Ardından şu anda istenen "page" değerinin veri başında veya sonunda olup olmadığını hesaplamak ve göstermek veya "önceki" ve "İleri" UI'ı uygun şekilde gizlemek gerekir. Biz bu mantığı bizim İNDİS() eylem yöntemi içinde uygulayabilirsiniz. Alternatif olarak biz Projemizin daha yeniden kullanılabilir bir yolla bu mantığı kapsülleyen bir yardımcı sınıfı ekleyebilirsiniz.

Aşağıdaki listeden türetilen bir basit "PaginatedList" yardımcı sınıf olan&lt;T&gt; koleksiyon sınıfı'nda yerleşik olarak bulunan .NET Framework. Iqueryable veri herhangi bir dizi gösterilecek şekilde sayfalara bölmek için kullanılan bir yeniden kullanılabilir koleksiyon sınıfı uygular. NerdDinner uygulamamız biz Iqueryable üzerinde çalışması gerekir&lt;Dinner&gt; sonuçlar, ancak yalnızca kolayca kullanılabilen Iqueryable karşı&lt;ürün&gt; veya Iqueryable&lt;müşteri&gt;diğer uygulama senaryolarında sonuçlanır:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Nasıl hesaplar yukarıda dikkat edin ve ardından "PageIndex", "PageSize", "TotalCount" ve "TotalPages" kullanıma sunan özellikler ister. Sonra koleksiyonundaki verileri sayfasının orijinal sıranın başında veya sonunda olup olmadığını gösteren iki yardımcı özellikleri "HasPreviousPage" ve "HasNextPage" kullanıma sunar. Yukarıdaki kod çalıştırılması - Dinner nesne toplam sayısını sayısını almak için ilk iki SQL sorguları neden olur (Bu nesneleri döndürmüyor – bir tamsayı döndüren bir "Sayısı seçin" ifadesi gerçekleştirdiği yerine), yalnızca satırlarını almak için ve ikincisi Bizim veritabanından veri geçerli sayfa için ihtiyacımız olan veriler.

Size sunduğumuz DinnersController.Index() yardımcı yöntemini bir PaginatedList sonra güncelleştirebilirsiniz&lt;Dinner&gt; bizim DinnerRepository.FindUpcomingDinners() gelen neden ve bizim görünümü şablona geçirebilirsiniz:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Biz ViewPage devralacak şekilde şablonu \Views\Dinners\Index.aspx görüntüleme sonra güncelleştirebilirsiniz&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt; ViewPage yerine&lt;IEnumerable&lt;Dinner&gt;&gt;ve ardından önceki ve sonraki gezinme Arabiriminin gizlemek veya göstermek için sunduğumuz görünüm şablonu altına aşağıdaki kodu ekleyin:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Nasıl Html.RouteLink() yardımcı yöntem bizim köprü oluşturmak için kullanıyoruz dikkat edin. Bu yöntem, daha önce kullandığımız Html.ActionLink() yardımcı yöntemine benzerdir. "UpcomingDinners" biz bizim Global.asax dosyası Kurulum kural yönlendirme kullanarak URL ürettiğini farktır. Bu başlığında URL biçimi bizim İNDİS() eylem yöntemine oluşturacağız sağlar: */Dinners/sayfa / {sayfası}* – sağlıyoruz yukarıda geçerli PageIndex alarak bir değişken {sayfası} değerdir.

Ve biz uygulamamız çalıştırdığınızda, artık yeniden aynı anda 10 azalma Tarayıcımıza görüyoruz:

![](implement-efficient-data-paging/_static/image5.png)

Biz de &lt; &lt; &lt; ve &gt; &gt; &gt; gezinme Arabiriminin ileten atlayın ve bizim verileri kullanarak üzerinden erişilebilen URL'leri altyapısı geriye doğru ara olanak sağlayan sayfanın alt kısmındaki:

![](implement-efficient-data-paging/_static/image6.png)

| **Yan konu: Iqueryable etkilerini anlama&lt;T&gt;** |
| --- |
| Iqueryable&lt;T&gt; , ertelenmiş yürütme senaryoları çeşitli sağlayan çok güçlü bir özelliktir (disk belleği ve birleştirme gibi sorguları kıyasla). Olarak tüm güçlü özellikler, nasıl kullandığınıza ile dikkat etmek istediğiniz ve değil kötüye emin olun. Döndüren bir Iqueryable bilmek önemlidir&lt;T&gt; zincirleme işleci yöntemleri üzerinde eklemek ve bu nedenle ultimate sorgu yürütme katılmak çağıran kod deponuzdan sonucu sağlar. Çağıran kod bu yeteneği sağlamak istemediğiniz sonra döndürmesi gerekir IList geri&lt;T&gt; ya da IEnumerable&lt;T&gt; sonuçları - zaten yürütüldü bir sorgunun sonuçlarını içerir. Sayfalandırma senaryoları için bu depoyu yönteme çağrılan gerçek veri sayfalandırma mantığı göndermenize izin gerekir. Bu senaryoda, biz bizim FindUpcomingDinners() Bulucu metodunu ya da bir PaginatedList döndürülen bir imzaya sahip güncelleştirebilir: PaginatedList&lt; Dinner&gt; FindUpcomingDinners (PageIndex int, int pageSize) {} veya return bir IList &lt;Dinner&gt;ve param çıkış "totalCount" azalma toplam sayısını döndürmek için kullanın: IList&lt;Dinner&gt; FindUpcomingDinners (PageIndex int, int totalCount kullanıma int pageSize) {} |

### <a name="next-step"></a>Sonraki adım

Şimdi nasıl kimlik doğrulaması ve yetkilendirme uygulamamız için destek ekleyebiliriz konumunda göz atalım.

> [!div class="step-by-step"]
> [Önceki](re-use-ui-using-master-pages-and-partials.md)
> [İleri](secure-applications-using-authentication-and-authorization.md)
