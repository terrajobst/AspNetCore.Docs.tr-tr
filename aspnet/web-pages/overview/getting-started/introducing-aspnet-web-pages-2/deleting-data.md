---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: ASP.NET Web sayfaları sunarak - veritabanı verilerini silme | Microsoft Docs
author: tfitzmac
description: Bu öğretici bir tek tek veritabanı girişi silmek gösterilmiştir. Bu, veritabanı verilerde güncelleştirme ASP.NET Web Pa aracılığıyla serisini tamamladınız varsayar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 146199e862cd6fa2607671d31633476b1cb67021
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>ASP.NET Web sayfaları sunarak - veritabanı verileri silme
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu öğretici bir tek tek veritabanı girişi silmek gösterilmiştir. Seri aracılığıyla tamamladığınızdan varsayar [güncelleştirme veritabanı veri ASP.NET Web Pages'de](updating-data.md).
> 
> Öğrenecekleriniz:
> 
> - Tek bir kaydın listesini kayıtları seçmek nasıl.
> - Tek kayıtlı bir veritabanından silmek nasıl.
> - Belirli bir düğme bir formda tıklandığını kontrol etme.
>   
> 
> Özellikler/teknolojilerini ele alınan:
> 
> - `WebGrid` Yardımcısı.
> - SQL `Delete` komutu.
> - `Database.Execute` Bir SQL çalıştırılacak yöntemi `Delete` komutu.


## <a name="what-youll-build"></a>Ne oluşturacağınız

Önceki öğreticide, var olan bir veritabanı kaydını güncelleştirmek öğrendiniz. Kayıt güncelleme yerine onu silersiniz Bu öğretici, benzerdir. Bu öğretici kısa olacak şekilde silme daha basit, olması dışında işlemleri çok aynıdır.

İçinde *filmler* update sayfasında, `WebGrid` onun görüntüler için yardımcı bir **silmek** eşlik etmek üzere her film yanındaki bağlantı **Düzenle** daha önce eklediğiniz bağlantı.

![Her film için Sil bağlantısını gösteren filmler sayfası](deleting-data/_static/image1.png)

Düzenleme gibi ile tıkladığınızda **silmek** bağlantı sürdüğünü, farklı bir sayfaya film bilgileri zaten bir formda olduğu:

![Görüntülenen bir filmi film sayfayı silin](deleting-data/_static/image2.png)

Ardından, kayıt kalıcı olarak silmek için düğmeyi tıklatabilirsiniz.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Film listenin Delete bağlantı ekleme

Ekleyerek başlayacaksınız bir **silmek** bağlantı `WebGrid` Yardımcısı. Bu bağlantıyı benzer **Düzenle** önceki öğreticide eklediğiniz bağlantı.

Açık *Movies.cshtml* dosya.

Değişiklik `WebGrid` bir sütunu ekleyerek sayfasının gövdesindeki biçimlendirme. Değiştirilen biçimlendirme aşağıdaki gibidir:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Yeni bir sütun bu bilgisayardır:

[!code-html[Main](deleting-data/samples/sample2.html)]

Kılavuz yapılandırılır, yol **Düzenle** sütundur kılavuzda soldaki ve **silmek** en sağdaki sütun. (Sonra bir virgül `Year` şimdi sütun durumunda, fark etmemiştir.) Bu bağlantı sütunları nereye özel bir şey yoktur ve kolayca bunları birbirinin yanına yerleştirdiğiniz. Bu durumda, kullanıcılar bunları karma daha zor hale getirmek için ayrı.

![Birbirinin yanına olmadıklarını olduğunu göstermek için filmler sayfa düzenleme ve ayrıntıları bağlantıları ile işaretlenmiş](deleting-data/_static/image3.png)

Yeni bir sütun bağlantıyı gösterir (`<a>` öğesi) "Delete" metnini söyler. Bağlantı hedefi (kendi `href` özniteliği) sonuçta bu URL şöyle ile çözümler kodu `id` her film için farklı bir değer:

[!code-css[Main](deleting-data/samples/sample3.css)]

Bu bağlantıyı adlı bir sayfaya çağıracağı *DeleteMovie* ve seçtiğiniz film Kimliğini geçirin.

Neredeyse aynı olduğu için bu öğreticiyi bu bağlantıyı nasıl oluşturulur, ayrıntılı gitmesi olmaz **Düzenle** önceki öğretici bağlantısından ([güncelleştirme veritabanı veri ASP.NET Web Pages'de](updating-data.md)).

## <a name="creating-the-delete-page"></a>Delete sayfası oluşturma

Hedef olacaktır sayfası oluşturabilirsiniz artık **silmek** kılavuzunda bağlantı.

> [!NOTE] 
> 
> **Önemli** ilk silmek için bir kayıt seçerek ve sonra işlemi onaylamak için ayrı bir sayfa ve düğmesini kullanarak tekniği güvenlik için son derece önemlidir. Önceki eğitimlerine okuduğunuz gibi yapma *herhangi* tür değişiklik sitenizin gereken *her zaman* yapılması formu kullanarak &mdash; diğer bir deyişle, bir HTTP POST işlemini kullanarak. Site (GET işlemi kullanan) bir bağlantıya tıklayarak değiştirmek mümkün hale, kişiler, sitenize basit isteklerde ve verilerinizi silmek. Bile, sitenizin dizinini bir arama motoru Gezgin yalnızca aşağıdaki bağlantılar tarafından yanlışlıkla veri silinemedi.
> 
> Uygulamanızı bir kaydı değiştirme kişiler sağlar, yine de düzenleme için kullanıcıya kaydı sunmak üzere vardır. Ancak, bir kaydın silinmesi için bu adımı atlamak için gerekebilir. Bu adım, ancak atlamayın. (Bu da kaydını görmek ve bunlar kayıt silmekte olduğunuz onaylamak kullanıcılar için yararlı olur.)
> 
> Bir sonraki öğretici kümesindeki bir kullanıcının bir kaydı silmeden önce oturum açması için oturum açma işlevselliği ekleme görürsünüz.


Adlı bir sayfa oluşturma *DeleteMovie.cshtml* ve dosyasında, aşağıdaki biçimlendirme nedir değiştirin:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Bu biçimlendirme benzer *EditMovie* , metin kutuları yerine dışındaki sayfalara (`<input type="text">`), biçimlendirme içeren `<span>` öğeleri. Düzenlemek için burada şey yoktur. Yapmanız gereken tek şey böylece kullanıcılar sağ film silmekte olduğunuz emin olabilirsiniz film ayrıntıları görüntüle.

İşaretleme film listeleme sayfaya dönmek kullanıcı olanak sağlayan bir bağlantı zaten var.

Olarak *EditMovie* sayfası, seçilen film Kimliğini, gizli bir alan depolanır. (Bu sayfaya ilk başta bir sorgu dizesi değeri olarak geçirilir.) Var olan bir `Html.ValidationSummary` doğrulama hataları görüntüler çağrısı. Bu durumda, hata film kimliği yok sayfasına geçildi veya film kimliği geçersiz olabilir. Birisi bu sayfayı bir filmi seçmeden çalıştıracaksanız, bu durum oluşabilir *filmler* sayfası.

Düğme resim yazısı **silmek film**, ve kendi ad özniteliği kümesine `buttonDelete`. `name` Özniteliği formun gönderilen düğmeyi saptamak için kodda kullanılacak.

1) film Ayrıntıları sayfası görüntülendiğinde okumak için kod yazmak ve kullanıcı düğmesini tıklattığında film 2) gerçekten silmek gerekir.

## <a name="adding-code-to-read-a-single-movie"></a>Tek bir filmi okumak için kod ekleme

Üstündeki *DeleteMovie.cshtml* sayfasında, aşağıdaki kod bloğunu ekleyin:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Bu biçimlendirme karşılık gelen kodu aynıdır *EditMovie* sayfası. Sorgu dizesi dışında film Kimliğini alır ve bir kayıt veritabanından okumak için Kimliğini kullanır. Doğrulama testi kodu içerir (`IsInt()` ve `row != null`) sayfasına geçirilen film Kimliğinin geçerli olduğundan emin olmak için.

Bu kod, sayfa ilk çalıştığında yalnızca çalışması gerektiğini unutmayın. Kullanıcı tıklattığında film kaydı veritabanından yeniden okumak istemediğiniz **silmek film** düğmesi. Bu nedenle, okuma film içinde belirten bir test kodu `if(!IsPost)` &mdash; diğer bir deyişle, *isteği post işlemine (form gönderme) değilse,*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Seçili film silmek için kod ekleme

Kullanıcı düğmesini tıklattığında film silmek için aşağıdaki kodu kapanış ayracı yalnızca eklemek `@` engelle:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Bu, varolan bir kaydı güncelleştirmek için kod benzer, ancak daha basit kodudur. Kod temelde bir SQL çalışır `Delete` deyimi.

 Olarak *EditMovie* kod sayfası, konusu bir `if(IsPost)` bloğu. Bu süre, `if()` durumdur biraz daha karmaşık: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Burada iki koşul vardır. Sayfa gönderiliyor olduğunu, ilk önce gördüğünüz gibi olan &mdash; `if(IsPost)`.

İkinci koşulu `!Request["buttonDelete"].IsEmpty()`, istek adlı bir nesne olduğu anlamına gelen `buttonDelete`. Kuşkusuz bu formun hangi düğmesi gönderilen sınama bir dolaylı yoludur. Bir form birden çok düğmeleri içeriyorsa, istekte yalnızca tıklandığını düğmenin adı görüntülenir. Bu nedenle, mantıksal olarak, belirli bir düğmeyi adını istekte görünüp görünmeyeceğini &mdash; veya bu düğme boş değilse kodda belirtildiği gibi &mdash; formun gönderilen düğmesi olmasıdır.

`&&` İşleci anlamına gelir "ve" (mantıksal AND). Bu nedenle tüm `if` koşul...

*Bu istek bir (ilk kez isteği değil) postasıdır*  
  
 AND  
  
`buttonDelete`*Düğmesi formun gönderilen düğmesi oluştu.*

Bu form (aslında, bu sayfayı) yalnızca bir düğme, bu nedenle içerir için ek sınama `buttonDelete` teknik gerekli değildir. Yine de, verileri kalıcı olarak kaldırır bir işlem gerçekleştirmek üzere olduğunuz. Böylece olabildiğince yalnızca kullanıcının açıkça, istediği zaman işlemi yaptığınızdan emin olmak istiyor. Örneğin, daha sonra bu sayfayı genişletilmiş ve diğer düğmeleri kendisine eklenmiş olduğunu varsayalım. Yalnızca film siler kodu dahi sonra çalıştıracak `buttonDelete` düğmesine tıklanana.

Olarak *EditMovie* sayfasında kimliği gizli alanından almanıza ve ardından SQL komutunu çalıştırın. Sözdizimi `Delete` ifadesi:

`DELETE FROM table WHERE ID = value`

Eklenecek önemlidir `WHERE` yan tümcesi ve kimliği. WHERE yan tümcesi bırakırsanız *tablosundaki tüm kayıtları silinecek*. Görülen, kimlik değeri SQL komutu için bir yer tutucu kullanarak geçirin.

## <a name="testing-the-movie-delete-process"></a>Film silme işlemi test etme

Artık test edebilirsiniz. Çalıştırma *filmler* sayfasında ve tıklayın **silmek** film yanındaki. Zaman *DeleteMovie* sayfası görüntülendikten sonra **silmek film**.

![Silme film düğmesi vurgulanmış filmi sayfayı silin](deleting-data/_static/image4.png)

Düğmeye tıkladığınızda, kod filmler siler ve film listesini döndürür. Silinen film için arama vardır ve silinmiş onaylayın.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğretici ortak bir görünüm ve düzeni sitenizdeki tüm sayfaları koyduğunuzdan gösterilmiştir.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Tam listesi için (Delete bağlantılarıyla güncelleştirilmiş) film sayfası

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Tam listesi için DeleteMovie sayfası

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [Razor sözdizimini kullanarak ASP.NET Web programlamaya giriş](../introducing-razor-syntax-c.md)
- [SQL DELETE deyimi](http://www.w3schools.com/sql/sql_delete.asp) W3Schools sitesinde

> [!div class="step-by-step"]
> [Önceki](updating-data.md)
> [sonraki](layouts.md)
