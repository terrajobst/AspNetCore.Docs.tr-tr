---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: Verimli veri Sayfalaması uygulamak | Microsoft Docs
author: microsoft
description: Adım 8 sayfalama desteği, bizim /Dinners URL'sine ekleyebilirsiniz, böylece aynı anda azalma 1000'lik görüntülemek yerine, biz yalnızca 10 yaklaşan azalma adresindeki görüntülersiniz gösterilmektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873254"
---
<a name="implement-efficient-data-paging"></a>Uygulama verimli veri disk belleği
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Bu adım 8 bir ücretsiz olan ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> 8. adım, disk belleği desteği, bizim /Dinners URL'sine ekleyebilirsiniz, böylece aynı anda azalma 1000'lik görüntülemek yerine, biz yalnızca 10 yaklaşan azalma aynı anda - görüntüle ve geri sayfasında ve listenin tamamını bir SEO kolay şekilde iletmek son kullanıcılara izin ver gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner 8. adım: Sayfalama desteği

Sitemizi başarılı olursa, gelecek azalma binlerce sahip olur. Kullanıcı arabirimimizi tüm bu azalma işlemeye ölçeklendirir ve bunları göz atmak kullanıcılara emin olmak gerekir. Bu ayarı etkinleştirmek için sayfalama desteği ekleyeceğiz bizim */Dinners* azalma 1000'lik bir kez şu görüntüleme URL'sini şekilde yerine yalnızca aynı anda - 10 yaklaşan azalma görüntüle ve geri sayfasında ve listenin tamamını üzerinden iletmek son kullanıcılara izin ver bir SEO kolay yoludur.

### <a name="index-action-method-recap"></a>İNDİS() eylem yöntemi özeti

Bizim DinnersController sınıfı içinde İNDİS() eylem yöntemi şu anda aşağıdaki gibi görünür:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

Ne zaman bir istek yapıldığında için */Dinners* URL, tüm yaklaşan azalma listesini alır ve bunları tümünün listesini oluşturur:

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>Anlama IQuerable&lt;T&gt;

*Iqueryable&lt;T&gt;*  LINQ ile .NET 3.5 bir parçası olarak tanıtılan bir arabirimdir. Biz, disk belleği destek uygulamak için yararlanabilir güçlü "ertelenmiş yürütme" senaryoları mümkün kılar.

Bizim DinnerRepository biz Iqueryable iade ettiğiniz&lt;Yemeği&gt; bizim FindUpcomingDinners() yönteminden dizisi:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

Iqueryable&lt;Yemeği&gt; bizim FindUpcomingDinners() yöntemi tarafından döndürülen nesne LINQ-SQL kullanarak bizim veritabanından Yemeği nesneleri almak için bir sorgu yalıtır. Önemlisi, veritabanında sorgu erişim/yinelemek sorgudaki verileri üzerinden denemesi kadar veya ToList() yöntemi üzerinde diyoruz kadar yürütebilirsiniz olmaz. Bizim FindUpcomingDinners() yöntemini çağırarak kodu isteğe bağlı olarak ek "zincirleme" operations/filtreler Iqueryable'a eklemek için seçebileceğiniz&lt;Yemeği&gt; sorguyu çalıştırmadan önce nesnesi. LINQ-SQL veri istendiğinde veritabanında birleşik sorguyu yürütmek akıllı ise.

Böylece ek "Atla" ve "Al" işleçleri döndürülen Iqueryable'a uygular disk belleği mantığını uygulamak için biz bizim DinnersController'ın İNDİS() eylem yöntemi güncelleştirebilirsiniz&lt;Yemeği&gt; ToList() üzerinde çağırmadan önce dizisi:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

Yukarıdaki kod ilk 10 yaklaşan azalma veritabanındaki üzerinden atlar ve 20 azalma geri döndürür. LINQ-SQL bu mantığı web sunucusu değil de, SQL veritabanında – atlanıyor gerçekleştirir en iyi duruma getirilmiş bir SQL sorgusu oluşturmak akıllıca olur. Başka bir deyişle, biz yaklaşan azalma milyonlarca veritabanında olsa bile, yalnızca istiyoruz 10 (verimli ve ölçeklenebilir hale) bu isteğin bir parçası alınır.

### <a name="adding-a-page-value-to-the-url"></a>"Sayfa" değeri URL'ye ekleniyor

Belirli sayfa aralıklarını kodlamak yerine, bir kullanıcı isteyen hangi Yemeği aralığı gösteren bir "Sayfa" parametresini içerecek biçimde Url'lerimizi isteyeceksiniz.

#### <a name="using-a-querystring-value"></a>Bir sorgu dizesi değer kullanma

Aşağıdaki kodu biz bir sorgu dizesi parametresi desteği ve URL'ler gibi etkinleştirmek için bizim İNDİS() eylem yönteminin nasıl güncelleştirebilirsiniz gösterir */Dinners? sayfa = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

Yukarıdaki İNDİS() eylem yöntemi "sayfasında" adlı bir parametre içeriyor. Parametre boş değer atanabilir bir tamsayı olarak bildirilmiş (hangi int olan? gösterir). Bunun anlamı */Dinners? sayfa = 2* URL parametre değeri olarak geçirilecek "2" değerini neden olur. */Dinners* URL (olmadan bir sorgu dizesi değer) geçirilecek bir null değer neden olur.

Biz sayfa değeri sayfa boyutu (Bu durumda 10 satır) üzerinden geçmek için kaç tane azalma belirlemek için çarparak. Kullanıyoruz [C# null "birleştirmesi" işleci (?) ](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx) boş değer atanabilir türleri ile ilgilenirken yararlı olduğu. Sayfa parametre null ise Yukarıdaki kod sayfası 0 değeri atar.

#### <a name="using-embedded-url-values"></a>Katıştırılmış URL değerleri kullanarak

Bir sorgu dizesi değeri kullanılarak alternatif gerçek URL içinde sayfa parametre katıştırmak için olacaktır. Örneğin: */Dinners/Page/2* veya */azalma/2*. ASP.NET MVC şöyle senaryoları desteklemek kolaylaştıran güçlü bir yönlendirme URL'si altyapısını içerir.

Size özel yönlendirme kuralları gelen URL veya URL format istiyoruz herhangi denetleyicisi sınıfı veya eylem yöntemi için eşlenen kaydedebilirsiniz. Tüm yapılacaklar ihtiyacımız olan Projemizin Global.asax dosyasında açmak için:

![](implement-efficient-data-paging/_static/image2.png)

Ve yolları ilk çağrıda gibi MapRoute() yardımcı yöntemi kullanarak yeni eşleme kuralı kaydedin. Aşağıdaki MapRoute():

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

Yukarıdaki biz "UpcomingDinners" adlı yeni bir yönlendirme kuralını kaydediyorsunuz. URL biçimi olan biz gösteren "azalma/sayfa / {sayfa}" – {page} URL içinde katıştırılmış bir parametre değeri olduğu. Üçüncü parametre MapRoute() yöntemine İNDİS() eylem yöntemine DinnersController sınıfındaki bu biçim ile eşleşmesi URL'leri eşlemelisiniz gösterir.

Biz bizim "Sayfa" parametresi URL ve querystring şimdi gelecektir dışında biz önce Querystring Senaryomuzda ile – vardı tam aynı İNDİS() kodu kullanabilirsiniz:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

Şimdi ne zaman biz uygulamayı çalıştırın ve yazın */Dinners* ilk 10 yaklaşan azalma göreceğiz:

![](implement-efficient-data-paging/_static/image3.png)

Ve biz yazdığınızda */Dinners/Page/1* biz azalma sonraki sayfasını görürsünüz:

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>Sayfa gezintisi UI ekleme

Disk belleği Senaryomuzda tamamlamak için son adımı, "İleri" ve "önceki" Gezinti kullanıcı Arabirimi Yemeği verileri kolayca atlamak kullanıcıların sağlamak için bizim görünüm şablonu içindeki uygulamak için olacaktır.

Bu dönüşür veri birçok sayfa için nasıl bunu doğru şekilde uygulamak için biz azalma toplam sayısı veritabanında de bilmeniz gerekir. Biz, ardından istenen "Sayfa" değeri verileri başında veya sonunda olup olmadığını hesaplamak ve göstermek veya "önceki" ve "İleri" UI uygun şekilde gizlemek gerekir. Biz bu mantığı içinde bizim İNDİS() eylem yönteminin uygulamanız. Alternatif olarak Biz bu mantığı daha yeniden kullanılabilir bir biçimde yalıtan Projemizin bir yardımcı sınıfı ekleyebilirsiniz.

Aşağıdaki listeden türetilen bir basit "PaginatedList" yardımcı sınıf olan&lt;T&gt; koleksiyon sınıfı yerleşik-.NET Framework. Iqueryable veri herhangi bir dizi sayfalara bölme için kullanılan bir yeniden kullanılabilir koleksiyon sınıfı uygular. NerdDinner uygulamamız biz Iqueryable üzerinde çalışması gerekir&lt;Yemeği&gt; sonuçları, ancak yalnızca kolayca kullanılabilirdi karşı Iqueryable&lt;ürün&gt; veya Iqueryable&lt;müşteri&gt;diğer uygulama senaryolarında neden olur:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

Daha sonra düzenlemenizi sağlayan özellikler "PageIndex", "PageSize", "TotalCount" ve "TotalPages" gibi ve nasıl hesaplar yukarıda dikkat edin. Daha sonra koleksiyonundaki veri sayfasının özgün sırası başında veya sonunda olup olmadığını gösteren iki yardımcı özellikleri "HasPreviousPage" ve "HasNextPage" kullanıma sunar. Yukarıdaki kod çalıştırılması - Yemeği nesnelerin sayısı toplam sayısı almak için ilk iki SQL sorguları neden olur (Bu nesneleri döndürmez – yerine bir tamsayı döndürür "Sayısı seçin" deyimi gerçekleştirir), yalnızca satırlarını almak için ikinci veriler geçerli sayfa verilerin bizim veritabanından ihtiyacımız var.

Biz bir PaginatedList oluşturmak için bizim DinnersController.Index() yardımcı yöntem sonra güncelleştirebilirsiniz&lt;Yemeği&gt; bizim DinnerRepository.FindUpcomingDinners() gelen neden ve bizim görünüm şablonu geçirin:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

Biz ViewPage devralacak şekilde şablonu \Views\Dinners\Index.aspx görüntüleme sonra güncelleştirebilirsiniz&lt;NerdDinner.Helpers.PaginatedList&lt;Yemeği&gt; &gt; ViewPage yerine&lt;IEnumerable&lt;Yemeği&gt;&gt;ve ardından göstermek veya gizlemek önceki ve sonraki Gezinti kullanıcı Arabirimi için bizim görünüm şablonu altına aşağıdaki kodu ekleyin:

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

Nasıl Html.RouteLink() yardımcı yöntem bizim köprüler oluşturmak için kullanıyoruz dikkat edin. Bu yöntem, daha önce kullandığımız Html.ActionLink() yardımcı yöntemine benzer. "Biz bizim Global.asax dosyasında Kurulum kuralı yönlendirme UpcomingDinners" kullanarak URL'ye oluşturduğunu farktır. Bu şu URL biçimi bizim İNDİS() eylem yöntemi oluşturacaksınız sağlar: */Dinners/sayfa / {sayfası}* – biz sağlama yukarıda dayalı geçerli PageIndex üzerinde bir değişken {page} değerdir.

Ve biz uygulamamız çalıştırdığınızda şimdi yeniden birer birer 10 azalma bizim tarayıcıda göreceğiz:

![](implement-efficient-data-paging/_static/image5.png)

Biz de &lt; &lt; &lt; ve &gt; &gt; &gt; Gezinti ileten atlayın ve bizim verileri kullanarak üzerinden altyapısı erişilebilir URL'leri geriye doğru arama kurmamızı sağlayan sayfanın sonundaki UI:

![](implement-efficient-data-paging/_static/image6.png)

| **Yan konu: Iqueryable etkilerini anlama&lt;T&gt;** |
| --- |
| Iqueryable&lt;T&gt; , çeşitli ilginç ertelenmiş yürütme senaryolarda sağlayan çok güçlü bir özelliktir (disk belleği ve birleşim gibi sorguları göre). Olarak tüm güçlü özellikler ile nasıl kullandığınız ile dikkatli olun istediğiniz ve değil kötüye emin olun. Bu Iqueryable döndürme bilmek önemlidir&lt;T&gt; deponuz sonucundan zincirleme işleci yöntemlere ekleme ve bu nedenle ultimate sorgu yürütme katılmak çağıran kodu sağlar. Çağrıyı yapan kod bu yeteneği sağlamak istemediğiniz sonra döndürme zorunluluğu IList geri&lt;T&gt; veya IEnumerable&lt;T&gt; zaten yürütüldü bir sorgunun sonuçlarını içeren sonuçları -. Sayfa numaralandırma senaryoları için bu, gerçek veri sayfalandırma mantığı çağrılan deposu yöntemi göndermek gerektirir. Bu senaryoda bizim FindUpcomingDinners() Bulucu yöntemi ya da bir PaginatedList döndürülen bir imzaya sahip güncelleştiriyoruz: PaginatedList&lt; Yemeği&gt; FindUpcomingDinners (int PageIndex, int pageSize) {} veya return IList &lt;Yemeği&gt;ve param çıkışı "totalCount" azalma toplam sayısını döndürmek için kullanın: IList&lt;Yemeği&gt; FindUpcomingDinners (int PageIndex, int totalCount çıkışı int pageSize) {} |

### <a name="next-step"></a>Sonraki adım

Şimdi biz kimlik doğrulama ve yetkilendirme uygulamamız için destek eklemek için ne konumundaki bakalım.

> [!div class="step-by-step"]
> [Önceki](re-use-ui-using-master-pages-and-partials.md)
> [sonraki](secure-applications-using-authentication-and-authorization.md)
