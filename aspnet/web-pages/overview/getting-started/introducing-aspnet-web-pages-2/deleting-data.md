---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: ASP.NET Web sayfaları ile tanışın - veritabanı verilerini silme | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide tek veritabanı girdiyi Sil gösterilmektedir. Bu, veritabanı verilerinde güncelleştirmeyi ASP.NET Web Pa aracılığıyla serisi tamamladınız varsayar...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: b2ef8fcc8cc534bd31fea83bf0b085b85995f417
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51020448"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>ASP.NET Web sayfalarına giriş - veritabanı verilerini silme
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu öğreticide tek veritabanı girdiyi Sil gösterilmektedir. Bu seriyi aracılığıyla bitirdiğinizi [veritabanı verilerini güncelleştirme ASP.NET Web Pages'de](updating-data.md).
> 
> Öğrenecekleriniz:
> 
> - Tek bir kayıtta kayıtların bir listeden seçmek nasıl.
> - Nasıl bir veritabanından tek bir kaydı silinir.
> - Belirli bir düğmeyi içinde bir formun tıklandığını nasıl kontrol edileceğini.
>   
> 
> Ele alınan özelliklerin/teknolojiler:
> 
> - `WebGrid` Yardımcısı.
> - SQL `Delete` komutu.
> - `Database.Execute` SQL çalıştırılacak yöntemi `Delete` komutu.


## <a name="what-youll-build"></a>Ne oluşturacaksınız

Önceki öğreticide, var olan bir veritabanı kaydını güncelleştirme öğrendiniz. Kaydı güncelleştirmek yerine bunu silersiniz dışında Bu öğreticide benzerdir. Bu öğreticide kısa olacak şekilde silme daha basit olması dışında işlemleri çok aynıdır.

İçinde *filmler* sayfasında, güncelleştirme `WebGrid` yardımcı olan görünmesi bir **Sil** eşlik eden her filmin yanındaki bağlantısını **Düzenle** daha önce eklediğiniz bağlantı.

![Bir silme bağlantısı için her filmin gösteren filmler sayfası](deleting-data/_static/image1.png)

Düzenleme gibi ile tıkladığınızda **Sil** bağlantı sürer, başka bir sayfaya film bilgileri zaten bir biçimde olduğu:

![Görüntülenen sahip bir film film sayfayı Sil](deleting-data/_static/image2.png)

Ardından, kaydı kalıcı olarak silmek için düğmeyi tıklatabilirsiniz.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Film listenin bir silme bağlantısı ekleme

Ekleyerek başlayacaksınız bir **Sil** bağlantı `WebGrid` Yardımcısı. Bu bağlantıyı benzer **Düzenle** bağlantı önceki bir öğreticide eklendi.

Açık *Movies.cshtml* dosya.

Değişiklik `WebGrid` biçimlendirme içinde bir sütun ekleyerek sayfasının gövdesi. Değiştirilen biçimlendirmesi şöyledir:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

Bu yeni bir sütun verilmiştir:

[!code-html[Main](deleting-data/samples/sample2.html)]

Kılavuz yapılandırılmış yolu **Düzenle** kılavuzunda en soldaki sütun ve **Sil** en sağdaki sütun. (Sonra bir virgül var. `Year` sütun şimdi durumda fark etmemiştir.) Bu bağlantı sütunlar nereye özel bir şey yoktur ve sizin gibi bir kolayca bunları birbirinin yanına yerleştirebilirsiniz. Bu durumda, bunlar karışmasına daha zor hale getirmek için ayrı.

![Düzenle ve ayrıntıları bağlantılarla filmler sayfası birbirinin yanına olmadıklarını olduğunu göstermek için işaretlenmiş](deleting-data/_static/image3.png)

Yeni bir sütun bağlantıyı gösterir (`<a>` öğesi) metni "Sil" diyor. Bağlantının hedefi (kendi `href` özniteliği) sonuçta bu URL, benzer bir şey ile çözümler kodu `id` her filmin için farklı bir değer:

[!code-css[Main](deleting-data/samples/sample3.css)]

Bu bağlantıyı adlı sayfanın çağıracağı *DeleteMovie* ve seçtiğiniz film kimliği geçirin.

Neredeyse aynı olduğundan, bu Öğreticide bu bağlantıyı nasıl oluşturulur, ilgili ayrıntıya gitmiyor **Düzenle** önceki öğreticide bağlantıdan ([veritabanı verilerini güncelleştirme ASP.NET Web Pages'de](updating-data.md)).

## <a name="creating-the-delete-page"></a>Silme sayfası oluşturma

Hedefi olan sayfanın oluşturabilirsiniz artık **Sil** kılavuzunda bağlantı.

> [!NOTE] 
> 
> **Önemli** ilk kez bir kaydı silmek için seçme ve ardından işlemini onaylamak için ayrı bir sayfa ve düğmesini kullanarak bir teknik güvenliği için son derece önemlidir. Önceki öğreticilerde makaleyi okudunuz, yaparak *herhangi* tür değişiklik sitenize gereken *her zaman* yapılması formu kullanarak &mdash; diğer bir deyişle, bir HTTP POST işlemi kullanarak. Site (bir alma işlemi kullanma) bir bağlantıya tıklayarak değiştirmek mümkün hale, kişilerin sitenize basit isteklerde ve verilerinizi silin. Hatta sitenizi dizin bir arama motoru Gezgin, yalnızca aşağıdaki bağlantılardan yanlışlıkla veri silebilir.
> 
> Uygulamanızı bir kaydı değiştirme kişilere izin verdiğinde, yine de düzenleme için kullanıcıya kayıt sunmak gerekir. Ancak, bir kaydı silmek için bu adımı atlamak için fikri size cazip olabilir. Bu adım, ancak atlamayın. (Bu da kaydını görmek ve bunlar kaydı silmekte olduğunuz onaylamak, kullanıcılar için yararlıdır.)
> 
> Bir sonraki öğretici kümesinde, bir kullanıcı bir kayıt silmeden önce oturum açmak bu nedenle oturum açma işlevselliği ekleme görürsünüz.


Adlı bir sayfa oluşturun *DeleteMovie.cshtml* dosyasında aşağıdaki işaretlemeyle nedir değiştirin:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Bu işaretleme benzer *EditMovie* sayfaları, metin kutuları yerine hariç (`<input type="text">`), biçimlendirme içeren `<span>` öğeleri. Düzenlemek için burada şey yoktur. Tek yapmanız gereken olan kullanıcıların doğru film silmekte olduğunuz emin yapabilmeleri için film ayrıntılarını görüntüler.

Biçimlendirme film listesi sayfasına geri dönmek kullanıcının sağlayan bir bağlantı zaten var.

Olarak *EditMovie* sayfasında, seçili film kimliği, gizli bir alanında saklanır. (Bu sayfaya ilk başta bir sorgu dizesi değeri geçirilir.) Var olan bir `Html.ValidationSummary` doğrulama hataları görüntüler çağrısı. Bu durumda, hata film kimliği yok sayfasına geçildi veya film kimliği geçersiz olabilir. Birisi bu sayfa içinde bir film seçmeden çalıştırdıysanız bu durum ortaya çıkabilir *filmler* sayfası.

Düğme başlık **Sil film**, ve onun name özniteliği kümesine `buttonDelete`. `name` Özniteliği formun gönderilen düğmeyi saptamak için kodda kullanılır.

(1) film Ayrıntıları sayfası ilk görüntülendiğinde okumak için kod yazma ve kullanıcı düğmeye tıkladığında film 2) gerçekten silmek gerekecektir.

## <a name="adding-code-to-read-a-single-movie"></a>Tek bir filmi okumayı kod ekleme

Üst kısmındaki *DeleteMovie.cshtml* sayfasında, aşağıdaki kod bloğunu ekleyin:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Bu işaretleme ilgili kod içinde aynıdır *EditMovie* sayfası. Sorgu dizesi dışında film Kimliğini alır ve bir kayıt veritabanından okumak için Kimliğini kullanır. Doğrulama testi kodu içerir (`IsInt()` ve `row != null`) sayfasına geçirilen film Kimliğinin geçerli olduğundan emin olmak için.

Bu kod yalnızca ilk kez çalıştırdığında sayfasında çalışması gerektiğini unutmayın. Kullanıcı tıkladığında veritabanından film kaydı yeniden okunuyorsa istemediğiniz **Sil film** düğmesi. Bu nedenle, film olduğunu bildiren bir test içinde okumayı kod `if(!IsPost)` &mdash; diğer bir deyişle, *isteği gönderme işlemi (form gönderme) değilse*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Seçili film silmek için kod ekleme

Kullanıcı düğmeye tıkladığında film silmek için aşağıdaki kodu yalnızca kapanış ayracı Ekle `@` engelle:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Bu kod, varolan bir kaydı güncelleştirmek için koda benzer, ancak daha basit değildir. Kod temel olarak bir SQL çalışır `Delete` deyimi.

 Olarak *EditMovie* kod sayfası, konusu bir `if(IsPost)` blok. Bu kez, `if()` biraz daha karmaşık koşul: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Burada iki koşul vardır. Sayfa gönderiliyor olduğunu, ilk önce gördüğünüz gibi olduğu &mdash; `if(IsPost)`.

İkinci koşul `!Request["buttonDelete"].IsEmpty()`, istek adlı bir nesne olduğu anlamına gelen `buttonDelete`. Kuşkusuz, formun hangi düğmeden gönderilen test bir dolaylı yoludur. Bir form birden çok düğmeleri içeriyorsa, istekte yalnızca tıklandığını düğme adı görüntülenir. Bu nedenle, mantıksal olarak, belirli bir düğmeyi adını istekte görünüp görünmeyeceğini &mdash; veya bu düğmeyi boş değilse kod içinde belirtildiği gibi &mdash; formunu düğmesi olan.

`&&` İşleci anlamına gelir "ve" (mantıksal ve). Bu nedenle tüm `if` koşul...

*Bu isteği bir gönderi (ilk kez istek değil) olup*  
  
 AND  
  
`buttonDelete`*Düğmesi formun gönderilen düğmesi oluştu.*

Bu formu (aslında bu sayfayı) yalnızca bir düğme şekilde içeren ek test için `buttonDelete` teknik olarak gerekli değildir. Yine de verileri kalıcı olarak kaldırır bir işlem gerçekleştirmek üzere olduğunuz. Bu nedenle olabildiğince yalnızca kullanıcının açıkça, istediği zaman işlemi gerçekleştiriyorsunuz emin olmak istersiniz. Örneğin, daha sonra bu sayfayı genişletilmiş ve diğer düğmeleri eklenmiş olduğunu varsayalım. Film silen kod yalnızca aşağıdaki durumlarda bile çalışır `buttonDelete` düğmeye tıkladı.

Olarak *EditMovie* sayfasından gizli alanı Kimliğini alın ve ardından SQL komutunu çalıştırın. Sözdizimi `Delete` deyimidir:

`DELETE FROM table WHERE ID = value`

Dahil etmek için çok önemli `WHERE` yan tümcesi ve kimliği. WHERE yan tümcesi bırakırsanız *tablodaki tüm kayıtları silinen*. Sizin gördüğünüz gibi kimlik değeri SQL komutu bir yer tutucu kullanarak geçirin.

## <a name="testing-the-movie-delete-process"></a>Film silme işlemi test etme

Artık test edebilirsiniz. Çalıştırma *filmler* sayfasında ve tıklayın **Sil** film yanında. Zaman *DeleteMovie* sayfası görüntülendikten sonra **Sil film**.

![Film Sil düğmesi vurgulanmış film sayfayı Sil](deleting-data/_static/image4.png)

Düğmeye tıkladığınızda, kod filmler siler ve film listeye döndürür. Silinen film arama vardır ve silinmiş onaylayın.

## <a name="coming-up-next"></a>Sıradaki gelen

Sonraki öğreticiye genel bir görünüm ve Düzen sitenizdeki tüm sayfaları bildirimde bulunma konusunda gösterir.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Tam listesi için (Sil bağlantılarla güncelleştirilmiş) film sayfası

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Tam listesi için DeleteMovie sayfası

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web programlama Razor söz dizimini kullanarak giriş](../introducing-razor-syntax-c.md)
- [SQL DELETE deyimi](http://www.w3schools.com/sql/sql_delete.asp) W3Schools sitesinde

> [!div class="step-by-step"]
> [Önceki](updating-data.md)
> [İleri](layouts.md)
