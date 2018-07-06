---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: 'Yineleme #7 – Ajax işlevselliği ekleme (C#) | Microsoft Docs'
author: microsoft
description: Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.
ms.author: aspnetcontent
ms.date: 02/20/2009
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: b7214a419b841f6814c3c37834b8ecd40e141926
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823303"
---
<a name="iteration-7--add-ajax-functionality-c"></a>Yineleme #7 – Ajax işlevselliği ekleme (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indir](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Bir kişi yönetimi ASP.NET MVC uygulama (C#)

Bu öğretici serisinde, tamamlanması bir tüm kişi yönetimi uygulaması ekleriz. Kişi Yöneticisi uygulama kişilerin bir listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamanızı sağlar.

Birden çok yineleme üzerinde uygulama ekleriz. Her yineleme ile biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı amacı, her değişikliğin nedenini anlamak etkinleştirmektir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Kişi Yöneticisi basit şekilde olası oluştururuz. Temel veritabanı işlemleri için destek ekliyoruz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - uygulamanın güzel görünmesini olun. Bu yineleme, varsayılan ASP.NET MVC görünüm ana sayfası değiştirme ve geçişli stil sayfası biz uygulamanın görünümünü geliştirin.

- Yineleme #3 - form doğrulaması ekleme. Üçüncü yinelemede temel form doğrulaması ekleriz. Biz, kişi formu gerekli form alanlarını tamamlamadan göndermesinin önlenmesine. Biz de e-posta adresi ve telefon numaralarını doğrulayın.

- Yineleme #4 - birbirine sıkı şekilde bağlı uygulama olun. Bu üçüncü yinelemede biz Bakım ve değişiklik kişi yöneticisi uygulamayı kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, biz uygulamamız depo deseni ve bağımlılık ekleme modelini kullanmak için yeniden düzenleyin.

- Yineleme #5 - birim testleri oluşturun. Beşinci yinelemede uygulamamız Bakım ve değişiklik birim testleri ekleyerek daha kolay vermiyoruz. Biz, bizim veri modeli sınıfları Sahne ve yapı denetleyicilerini ve Doğrulama mantığı birim testleri.

- Yineleme #6 - test odaklı geliştirme kullanma. Bu altıncı yinelemede yeni işlevsellik uygulamamız için ilk birim testleri yazma ve birim testlerini karşı kod yazma ekleriz. Bu yineleme, kişi grupları ekleriz.

- Yineleme #7 - Ajax işlevselliği ekleme. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Kişi Yöneticisi uygulamasının bu yinelemede biz uygulamamız Ajax yararlanması için yeniden düzenleyin. Ajax avantajlarından yararlanarak, uygulamamız daha hızlı yanıt vermiyoruz. Biz yalnızca belirli bir bölgede bir sayfa güncelleştirmek gerektiğinde bir sayfanın tamamını işleme önleyebilirsiniz.

Böylece biz t her birinin yeni bir kişi grubu seçtiğinde sayfanın tamamını görüntülemek gerek ki biz bizim dizini görünümü yeniden düzenleme. Bunun yerine, birisi bir kişi grubu tıkladığında, biz yalnızca ilgili kişi listesini güncelleştirmek ve sayfanın geri kalanını tek başına bırakın.

Ayrıca bizim silme bağlantı çalışır şekilde değiştireceğiz. Ayrı bir onay sayfası yerine bir JavaScript onay iletişim kutusunda görüntüleyeceğiz. Bir kişiyi silmek istediğinizi onaylayın, ilgili kişi kaydı veritabanından silmek için sunucuya yönelik bir HTTP DELETE işlemi gerçekleştirilir.

Ayrıca, biz animasyon efektleri bizim dizin görünümüne eklemek için jQuery yararlanır. Yeni kişiler listesi sunucudan getirildiğinde animasyon görüntüleyeceğiz.

Son olarak, biz tarayıcı geçmişini yönetmek için ASP.NET AJAX framework desteğinden. Şu kişi listesini güncelleştirmek için bir Ajax çağrısı yaptığınızda geçmişi noktaları oluşturacağız. Bu şekilde, tarayıcısı İleri ve geriye dönük düğmeleri çalışır.

## <a name="why-use-ajax"></a>Ajax neden kullanmalısınız?

AJAX kullanarak birçok faydası vardır. İlk olarak, bir uygulama sonuçları daha iyi bir kullanıcı deneyimi için Ajax işlevselliği ekleme. Normal web uygulamasında, sayfanın tamamını bir kullanıcı bir eylem gerçekleştiren her zaman sunucuya geri gönderilmelidir. Bir eylem gerçekleştirmek her sayfanın tamamını alınan ve yeniden kadar tarayıcı kilitler ve kullanıcının beklemesi gerekir.

Bu bir masaüstü uygulaması söz konusu olduğunda kabul edilemez bir deneyim olacaktır. Ancak, daha iyi yapabileceğimiz tanımamış çünkü geleneksel olarak, biz ile bir web uygulaması söz konusu olduğunda bu olumsuz kullanıcı deneyiminden beklenir. İsteğe bağlı olarak, çünkü yalnızca bizim imaginations bir kısıtlaması olduğu zaman, web uygulamaları ile ilgili bir sınırlama olduğu düşündük.

Ajax uygulamada t bir sayfa yalnızca güncelleştirilecek durdurmak için kullanıcı deneyimi getirme gerek istemiyorsunuz. Bunun yerine, sayfayı güncelleştirmek için arka planda bir zaman uyumsuz istek gerçekleştirebilirsiniz. Sayfanın parçası güncelleştirilir bekleyin kullanıcının t zorla istemiyorsunuz.

Ajax avantajlarından yararlanarak uygulamanızın performansını artırabilir. Kişi Yöneticisi uygulama Ajax işlevselliği hemen şimdi nasıl çalıştığını düşünün. Bir kişi grubu tıkladığınızda, tüm dizin görünümü yeniden gerekir. Kişiler listesi ve kişi grupları listesi, veritabanı sunucusundan alınmalıdır. Tüm bu verileri ağ üzerinden web sunucusundan web tarayıcısına geçirilmelidir.

Biz Ajax işlevselliği uygulamamıza ekledikten sonra ancak biz bir kullanıcı bir kişi grubu tıkladığında sayfanın tamamını yeniden görüntüleme önleyebilirsiniz. Artık veritabanından kişi grupları almak ihtiyacımız var. Biz de t tüm dizin görünümünün kablo anında iletme gerek ki. Yararlanarak Ajax, biz veritabanı Sunucumuz gerçekleştirmesi gereken iş miktarını azaltmak ve biz uygulamamız tarafından gerekli ağ trafiği miktarını azaltır.

## <a name="don-t-be-afraid-of-ajax"></a>Azure keşfetmek, Ajax Rsquo olabilir

Bazı geliştiriciler, çünkü bunlar alt düzey tarayıcılar hakkında endişe Ajax kullanmaktan kaçının. Bunlar web uygulamalarını JavaScript desteklemeyen bir tarayıcı tarafından erişildiğinde yine de çalışacağından emin olmak istersiniz. AJAX JavaScript üzerinde bağlı olduğundan, Ajax kullanarak bazı geliştiriciler kaçının.

Ancak, Ajax nasıl uygulamanız hakkında dikkatli olduğunuz algılamadığı üst düzey hem de alt düzey tarayıcıları ile çalışan uygulamalar oluşturabilirsiniz. Kişi Yöneticisi uygulamamız olmayan tarayıcıları ve JavaScript destekleyen tarayıcılar ile çalışır.

Kişi Yöneticisi uygulama JavaScript destekleyen bir tarayıcı ile kullanırsanız, daha iyi bir kullanıcı deneyimi gerekir. Örneğin, bir kişi grubu tıkladığınızda, yalnızca ilgili kişileri görüntüleyen sayfanın bölge güncelleştirilecektir.

Öte yandan, JavaScript desteklemez (veya JavaScript devre dışı olan) bir tarayıcı ile kişi Yöneticisi uygulama kullanın, biraz daha az tercih bir kullanıcı deneyimi yüklemeniz gerekir. Bir kişi grubu tıkladığınızda, örneğin, tüm dizin görünümünün tarayıcıya kişiler eşleşen listesini görüntülemek için gönderilmelidir.

## <a name="adding-the-required-javascript-files"></a>Gerekli JavaScript dosyaları ekleme

Uygulamamız için Ajax işlevselliği ekleme için üç JavaScript dosyalarının kullanılması gerekir. Bu dosyaların üç yeni bir ASP.NET MVC uygulaması betikler klasörüne eklenir.

İçinde uygulamanızın birden çok sayfada Ajax kullanmayı planlıyorsanız, daha sonra uygulama s görünüm ana sayfasında gerekli JavaScript dosyaları içerecek şekilde mantıklıdır. Bu şekilde, JavaScript dosyalarının tüm uygulamanızdaki sayfaların otomatik olarak dahil edilir.

Aşağıdaki JavaScript içeren içine ekleme &lt;baş&gt; görünüm ana sayfanızın etiketi:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Ajax kullanılacak şekilde dizin görünümünün yeniden düzenleme

S kişi grubu tıklayarak yalnızca görüntüleyen kişiler görünümü bölgesi güncelleştirilebilmesi için sunduğumuz dizin görünümünün değiştirerek başlamanızı sağlar. Şekil 1'görüntüsünde, güncelleştirmek istediğiniz bölgeyi içerir.


[![Yalnızca kişileri güncelleştiriliyor](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Şekil 01**: yalnızca ilgili kişileri güncelleştiriliyor ([tam boyutlu görüntüyü görmek için tıklatın](iteration-7-add-ajax-functionality-cs/_static/image2.png))


İlk adım, zaman uyumsuz olarak ayrı bir kısmi (görünümü kullanıcı denetimi) güncelleştirmek için istediğimiz görünümü parçası ayırmaktır. Kişiler tablosunu görüntüler dizin görünümünün bölümünü listeleme 1 kısmi içine taşındı.

**1 - Views\Contact\ContactList.ascx listeleme**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

Listeleme 1. kısmi dizin görünümünün değerinden farklı bir model olduğuna dikkat edin. *Inherits* özniteliğini &lt;% @ sayfa %&gt; yönergesi belirtir kısmi ViewUserControl devralan&lt;grubu&gt; sınıfı.

Güncelleştirilmiş dizin görünümünün listeleme 2'de yer alır.

**2 - Views\Contact\Index.aspx listeleme**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Güncelleştirilmiş görünümü 2 listeleme hakkında fark etmişsinizdir iki şey vardır. İlk olarak, tüm içeriğin kısmi taşındı bildirimi Html.RenderPartial() çağrısı ile değiştirilir. Index görünümünü ilk kez kişiler ilk kümesini görüntülemek için istenen Html.RenderPartial() yöntem çağrılır.

İkinci olarak, kişi grupları göstermek için kullanılan Html.ActionLink() bir Ajax.ActionLink() ile değiştirilmiştir dikkat edin. Aşağıdaki parametrelerle Ajax.ActionLink() çağrılır:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

Bağlantı için görüntülenecek metin ilk parametresini temsil eder, ikinci parametre, rota değerlerini temsil eder ve üçüncü parametre Ajax seçeneklerini temsil eder. Bu durumda, UpdateTargetId Ajax seçenek HTML işaret edecek şekilde kullanırız &lt;div&gt; Ajax isteği tamamlandıktan sonra güncelleştirmek için istediğimiz etiketi. Güncelleştirmek istediğiniz &lt;div&gt; yeni kişi listesinin etiketi.

İlgili kişi denetleyicisi güncelleştirilmiş İNDİS() yöntemi listeleme 3'te yer alır.

**3 - Controllers\ContactController.cs (Index yöntemi) listeleme**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

Güncelleştirilmiş İNDİS() eylem koşullu olarak ikisinden birini döndürür. Ardından İNDİS() eylemi bir Ajax isteği tarafından çağrılan denetleyici kısmi döndürür. Aksi takdirde İNDİS() eylem tamamı döndürür.

İNDİS() eylemi bir Ajax isteği tarafından çağrıldığında kadar veri döndürmeyen gerekmez dikkat edin. Normal bir isteği bağlamında dizin eylem tüm kişi grupları ve seçilen kişi grubu listesini döndürür. Bir Ajax isteği bağlamında İNDİS() eylem yalnızca seçilen grup döndürür. AJAX veritabanı sunucunuzdaki daha az iş anlamına gelir.

Bizim değiştirilmiş dizin görünümünün algılamadığı üst düzey hem de alt düzey tarayıcılar söz konusu olduğunda çalışır. Bir kişi grubu tıklatın ve tarayıcınızın JavaScript'i destekleyip yalnızca Kişiler listesi içeren görünümü bölgesi güncelleştirilir. Öte yandan, tarayıcınız JavaScript desteklemiyor, ardından tüm görünüm güncelleştirilir.


Güncelleştirilmiş bizim dizin görünümünün bir sorun var. Bir kişi grubu tıkladığınızda, seçili grubun vurgulanmaz. Grup listesini bir Ajax isteği sırasında güncelleştirildiğinde bölgesinin dışındaki görüntülendiğinden, doğru grubu vurgulanmış değil. Sonraki bölümde bu sorunu gidereceğiz.


## <a name="adding-jquery-animation-effects"></a>JQuery animasyon efektleri ekleme

Normalde, bir web sayfasındaki bir bağlantıya tıkladığında, tarayıcı etkin bir şekilde güncelleştirilmiş içeriği getirilirken olup olmadığını algılamak için tarayıcı ilerleme çubuğunu kullanabilirsiniz. Öte yandan, bir Ajax isteği gerçekleştirirken, tarayıcı ilerleme çubuğu hiçbir ilerleme durumunu göstermez. Bu kullanıcılar seni çileden yapabilir. Tarayıcı dondurulmuş olup olmadığını nasıl biliyor musunuz?

Bir kullanıcı için bir Ajax isteği gerçekleştirilirken iş gerçekleştirilmekte olduğunu belirtebilir birkaç yolu vardır. Basit animasyon görüntüleme bir yaklaşımdır. Örneğin, bir Ajax isteği başlar ve istek tamamlandığında bölgede Soldurma bir bölgede Soldurma.

Animasyon efektleri oluşturmak için Microsoft ASP.NET MVC çerçevesiyle dahildir jQuery kitaplığı kullanacağız. Güncelleştirilmiş dizin görünümünün listeleme 4'te yer alır.

**4 - Views\Contact\Index.aspx listeleme**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Güncelleştirilmiş dizin görünümünün üç yeni JavaScript işlevleri içerdiğine dikkat edin. İlk iki işlevleri jQuery Kıs ve kişiler listesinde yeni bir kişi grubu tıkladığınızda Soldurma için kullanın. Üçüncü işlev, bir hata (örneğin, ağ zaman aşımı) bir Ajax isteği sonuçları, bir hata iletisi görüntüler.

İlk işlev Ayrıca seçili grup vurgulama üstlenir. Bir sınıf = seçilen öznitelik, üst öğeye tıklandığında öğenin (LI öğesi) eklenir. Yine, jQuery, doğru öğeyi seçin ve CSS sınıfı eklemek kolaylaştırır.

Bu betikler Ajax.ActionLink() AjaxOptions parametre Yardım grup bağlantıları gerektirir. Güncelleştirilmiş Ajax.ActionLink() yöntem çağrısının şöyle görünür:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Tarayıcı geçmişini desteği ekleme

Normalde, bir sayfayı güncelleştirmek üzere bir bağlantıya tıkladığında, tarayıcı geçmişinizi güncelleştirilir. Bu şekilde, zaman içinde sayfasının önceki durumuna geri taşımak için tarayıcı geri düğmesine tıklayabilirsiniz. Örneğin, arkadaş kişi grubu tıklatın ve sonra iş kişi grubu tıklatın, arkadaş kişi grubu seçildiğinde, sayfanın duruma geri gitmek için tarayıcıda geri düğmesini tıklatabilirsiniz.

Ne yazık ki, bir Ajax isteği gerçekleştiren gözatma geçmişini otomatik olarak güncelleştirilmez. Bir kişi grubu tıklatın ve kişiler eşleşen listesi olan bir Ajax isteği alınır, tarayıcı geçmişini güncelleştirilmez. Bir kişi grubuna yeni bir kişi grubu seçtikten sonra gitmek için tarayıcıda geri düğmesini kullanamazsınız.

Ajax isteği gerçekleştirdikten sonra kullanıcılar tarayıcı yeniden kullanabilmek için düğme istiyorsanız biraz daha fazla işi gerçekleştirmek gerekir. ASP.NET AJAX Framework'e yerleşik tarayıcı geçmişini yönetim işlevselliğinin yararlanmak için gerekir.

ASP.NET AJAX tarayıcı geçmişini üç işlem yapmanız gerekir:

1. Tarayıcı geçmişini enableBrowserHistory özelliği true olarak ayarlayarak etkinleştirin.
2. Görünüm durumu değiştiğinde addHistoryPoint() yöntemi çağırarak geçmişi noktalarını kaydedin.
3. Navigate olay ortaya çıktığında görünümünün durumunu yeniden yapılandırma.

Güncelleştirilmiş dizin görünümünün listeleme 5'te yer alır.

**5 - Views\Contact\Index.aspx listeleme**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Listeleme 5'te tarayıcı geçmişini pageInit() işlevinde etkinleştirildi. PageInit() işlevi navigate olayı için olay işleyicisi ayarlamak için de kullanılır. Tarayıcı İleri veya geri düğmesine değiştirmek için sayfanın durumunu neden olur. her navigate olay tetiklenir.

Bir kişi grubu tıkladığınızda beginContactList() yöntemi çağrılır. Bu yöntem, yeni bir geçmiş noktası addHistoryPoint() yöntemi çağırarak oluşturur. Tıklandı kişi grubu kimliği geçmişe eklenir.

Grup Kimliği, bir kişi grubu bağlantısına expando özniteliğinden alınır. Bağlantıyı aşağıdaki çağrı Ajax.ActionLink() ile işlenir.

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

Son Ajax.ActionLink() için geçirilen parametre (XHTML uyumluluk için küçük harf) bağlantısına GroupID adlı expando öznitelik ekler.

Bir kullanıcı tarayıcı geri veya İleri düğmesine dokunduğunda navigate olay tetiklenir ve navigate() yöntemi çağrılır. Bu yöntem, navigate yönteme tarayıcı geçmişini noktasına karşılık gelen sayfanın durumunu eşleştirilecek sayfasında görüntülenen kişiler güncelleştirir.

## <a name="performing-ajax-deletes"></a>AJAX gerçekleştirme siler

Şu anda, bir kişiyi silmek için Sil bağlantısını tıklatın ve ardından silme onay sayfasında görüntülenen Sil düğmesine tıklayın gerekir (bkz: Şekil 2). Bu, çok sayıda veritabanı kaydını silme gibi basit bir şeyler için sayfa istekleri gibi görünüyor.


[![Silme onayı sayfası](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Şekil 02**: silme onayı sayfası ([tam boyutlu görüntüyü görmek için tıklatın](iteration-7-add-ajax-functionality-cs/_static/image4.png))


Silme onayı sayfasını atlayın ve doğrudan dizini görünümünden bir kişiyi sil daha cazip. Bu yaklaşımı uygulamanıza güvenlik açıkları açar çünkü bu dürtüsüne kapılmayın. Genel olarak, t don istediğiniz web uygulamanızın durumunu değiştiren bir eylem çağrılırken bir HTTP GET işlemi gerçekleştirmek. Bir silme işlemi gerçekleştirirken, bir HTTP POST gerçekleştirin ya da bir HTTP DELETE işlemi henüz, daha iyi istiyorsunuz.

Silme bağlantısı kısmi ContactList yer alır. Kısmi ContactList güncelleştirilmiş bir sürümünü listeleme 6'da yer alır.

**6 - Views\Contact\ContactList.ascx listeleme**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Silme bağlantısı Ajax.ImageActionLink() yöntemine aşağıdaki çağrıyı ile oluşturulur:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() ASP.NET MVC çerçevesi standart bir parçası değil. Kişi Yöneticisi projeye dahil bir özel yardımcı yöntemler Ajax.ImageActionLink() olur.


AjaxOptions parametre iki özelliğe sahiptir. İlk olarak Onayla özellik açılan JavaScript onay iletişim kutusunu görüntülemek için kullanılır. İkinci olarak, HttpMethod özelliği, bir HTTP DELETE işlemi gerçekleştirmek için kullanılır.

7 listeleme kişi denetleyiciye eklenmiş olan yeni bir AjaxDelete() eylemi içerir.

**7 - Controllers\ContactController.cs (AjaxDelete) listeleme**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

AcceptVerbs özniteliğiyle donatılmış AjaxDelete() eylem. Bu özniteliği eylemin çağrılmasını dışında bir HTTP DELETE işlemi dışında herhangi bir HTTP işlemi tarafından engeller. Özellikle, bu eylem bir HTTP GET ile çağrılamaz.

Veritabanı kaydı sildikten sonra silinen kayıt içermeyen kişiler güncelleştirilmiş listesini görüntülemek gerekir. AjaxDelete() yöntemin kısmi ContactList ve kişiler güncelleştirilmiş listesini döndürür.

## <a name="summary"></a>Özet

Bu yineleme, kişi yöneticisi uygulamamız için Ajax işlevselliği ekledik. Uygulamamızı performansını ve yanıt hızını artırmak için Ajax kullandık.

İlk olarak, böylece bir kişi grubu tıklayarak tüm görünüm güncelleştirmez biz Index görünümünü yeniden. Bunun yerine, bir kişi grubu tıklayarak yalnızca ilgili kişi listesini güncelleştirir.

Ardından, jQuery animasyon efektleri Kıs ve kişiler listesinde Soldurma kullandık. Ajax uygulamaya animasyon ekleme tarayıcı ilerleme çubuğu denk uygulamanızın kullanıcılarla sağlamak için kullanılabilir.

Ajax uygulamamız için tarayıcı geçmişini desteği de ekledik. Tarayıcı geri ve İleri düğmeleri dizin görünümünün durumunu değiştirmek için kullanıcıların etkinleştirdik.

Son olarak, HTTP DELETE işlemleri destekleyen bir silme bağlantısı oluşturduk. AJAX siler gerçekleştirerek, biz bir ek silme onayı sayfası istemek kullanıcının gerek kalmadan veritabanı kayıtlarını silmek kullanıcıları etkinleştirin.

> [!div class="step-by-step"]
> [Önceki](iteration-6-use-test-driven-development-cs.md)
> [İleri](iteration-1-create-the-application-vb.md)
