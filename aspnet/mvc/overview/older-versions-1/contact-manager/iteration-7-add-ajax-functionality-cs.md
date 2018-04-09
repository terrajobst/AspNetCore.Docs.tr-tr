---
uid: mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
title: 'Yineleme #7 – Ekle Ajax işlevleri (C#) | Microsoft Docs'
author: microsoft
description: Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: f1b0809e-8909-444e-b6bb-a5cd1dea3f72
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-7-add-ajax-functionality-cs
msc.type: authoredcontent
ms.openlocfilehash: 35d62383a571725749b2fc629bbb17954657b2f6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="iteration-7--add-ajax-functionality-c"></a>Yineleme #7 – Ekle Ajax işlevleri (C#)
====================
tarafından [Microsoft](https://github.com/microsoft)

[Kodu indirme](iteration-7-add-ajax-functionality-cs/_static/contactmanager_7_cs1.zip)

> Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>Bir kişi yönetim ASP.NET MVC uygulaması (C#) oluşturma

Bu öğretici serisinde tamamlamak için tüm kişi yönetim uygulamanın başından oluşturun. Kişi Yöneticisi uygulama kişi listesi için kişi bilgilerini - adları, telefon numarası ve e-posta adresleri - depolamak sağlar.

Biz uygulamayı birden çok kez oluşturun. Her bir yineleme, biz kademeli olarak uygulama geliştirin. Bu birden çok yineleme yaklaşımı, her değişiklik nedeni anlamak etkinleştirmek için hedefidir.

- Yineleme #1 - uygulama oluşturun. İlk yinelemede Contact Manager en basit yolu olası oluşturuyoruz. Temel veritabanı işlemleri için destek eklediğimiz: oluşturma, okuma, güncelleştirme ve silme (CRUD).

- Yineleme #2 - iyi Ara uygulama olun. Bu yinelemede biz ana görünüm sayfası, ASP.NET MVC varsayılan değiştirme ve geçişli stil sayfası uygulama görünümünü geliştirir.

- Yineleme #3 - form doğrulama ekleyin. Üçüncü yinelemede temel form doğrulama ekleyin. Biz, kişilerin gerekli form alanları tamamlamadan bir form gönderme engelleyin. Biz de e-posta adresleri ve telefon numaralarını doğrulayın.

- Yineleme #4 - gevşek uygulama olun. Bu üçüncü yinelemede biz Bakım ve ilgili kişi Yöneticisi uygulaması değişiklik kolaylaştırmak için çeşitli yazılım tasarım desenleri yararlanın. Örneğin, uygulamamız havuz deseni ve bağımlılık ekleme düzeni kullanmak üzere yeniden.

- Yineleme #5 - birim testleri oluşturma. Beşinci yinelemede biz uygulamamız korumak ve birim testleri ekleyerek değiştirmek kolaylaştırır. Biz bizim veri modeli sınıflarını mock ve bizim denetleyicileri ve Doğrulama mantığı için birim testleri oluşturma.

- Yineleme #6 - teste dayalı geliştirme kullanın. Bu altıncı yinelemede yeni işlevsellik uygulamamız için birim testleri ilk yazma ve birim testleri karşı kod yazma ekleriz. Bu yinelemede kişi grupları ekleyin.

- Yineleme #7 - Ajax işlevselliği ekleyin. Yedinci yinelemede biz uygulamamız performansını ve yanıt hızını Ajax için destek ekleyerek geliştirin.

## <a name="this-iteration"></a>Bu yineleme

Bu kişi Yöneticisi uygulaması yinelemede biz uygulamamız Ajax kullanmak için yeniden düzenleyin. Ajax yararlanarak, biz uygulamamız daha iyi yanıt vermesi. Biz, biz yalnızca belirli bir bölgede bir sayfa güncelleştirmeniz gerektiğinde tüm sayfa işleme önleyebilirsiniz.

Böylece biz t birinin yeni bir kişi grubu seçtiğinde sayfanın tamamını görüntülemek gerek güncelleştireceğinizi biz bizim dizini görünümü yeniden düzenlemeniz. Bunun yerine, birisi bir kişi grubu tıkladığında, biz yalnızca Kişiler listesini güncelleştirmek ve sayfa geri kalan tek başına bırakın.

Biz de bizim delete works bağlantı yolu değiştireceksiniz. Ayrı onay sayfası yerine bir JavaScript onay iletişim kutusu görüntülersiniz. Bir kişiyi silmek istediğinizi onaylayın, ilgili kişi kaydını veritabanından silmek için sunucusuna karşı bir HTTP DELETE işlemi gerçekleştirilir.

Ayrıca, biz animasyon efektleri bizim dizin görünüme eklemek üzere jQuery yararlanır. Yeni kişiler listesi sunucudan getirildiğinde biz animasyonun görüntülersiniz.

Son olarak, biz tarayıcı geçmişini yönetmek için ASP.NET AJAX framework desteğinden. Biz kişi listesini güncelleştirmek için bir Ajax çağrısı yaptığınızda geçmiş noktaları oluşturacağız. Bu şekilde tarayıcı geri ve İleri düğmelerini çalışır.

## <a name="why-use-ajax"></a>Ajax neden kullanılır?

AJAX kullanarak birçok avantaj vardır. İlk olarak, daha iyi bir kullanıcı deneyimi uygulama sonuçlarında Ajax işlevselliği ekleme. Normal web uygulamasında, sayfanın tamamını her zaman bir kullanıcı bir eylem gerçekleştirir sunucuya geri gönderilmelidir. Bir eylem gerçekleştirmek her sayfanın tamamını alınan ve yeniden kadar tarayıcı kilitler ve kullanıcının beklemesi gerekir.

Bu, bir masaüstü uygulaması durumunda kabul edilebilir bir deneyim olacaktır. Ancak, daha iyi yapabileceğimiz tanımamış çünkü geleneksel olarak, biz web uygulaması söz konusu olduğunda bu olumsuz kullanıcı deneyiminden ile yaşamakta. İsteğe bağlı olarak, çünkü yalnızca bizim imaginations bir kısıtlaması olduğu zaman, web uygulamalarının bir sınırlama olduğu düşünülen.

Ajax uygulamada yalnızca bir sayfa güncelleştirmek için bir durdurmak için kullanıcı deneyimini getirmek için t gerek istemiyorsunuz. Bunun yerine, sayfa güncelleştirmek için arka planda zaman uyumsuz isteği gerçekleştirebilirsiniz. Sayfanın parçası güncelleştirilir bekleyin kullanıcıya t zorla istemiyorsunuz.

Ajax yararlanarak, uygulamanızın performansını artırabilir. İlgili Kişi Yöneticisi uygulama şu anda Ajax işlevselliği işleyişi göz önünde bulundurun. Bir kişi grubu tıkladığınızda, tüm dizin görünümünün yeniden gerekir. Kişilerinizin listesini ve kişi grupları listesi veritabanı sunucusundan alınması gerekir. Tüm bu verileri ağ üzerinden web sunucusundan web tarayıcısına geçirilmelidir.

Biz uygulamamız için Ajax işlevselliği ekledikten sonra ancak biz bir kullanıcı bir kişi grubu tıkladığında sayfanın tamamını yeniden görüntüleme önleyebilirsiniz. Artık veritabanından kişi grupları şablonlarınızdan ihtiyacımız. Biz de t kablo tüm dizin görünümünün itme gerek güncelleştireceğinizi. Yararlanarak Ajax, biz bizim veritabanı sunucusu gerçekleştirmelisiniz iş miktarını azaltın ve biz uygulamamız tarafından gereken ağ trafiği miktarını azaltın.

## <a name="don-t-be-afraid-of-ajax"></a>Tan t Afraid, Ajax olabilir

Bazı geliştiriciler, alt düzey tarayıcılar hakkında endişelenmeniz çünkü Ajax kullanmaktan kaçının. Bunlar web uygulamalarını JavaScript desteklemeyen bir tarayıcı tarafından erişildiğinde hala çalışacağından emin olmak istersiniz. AJAX JavaScript üzerinde bağımlı olduğundan, bazı geliştiriciler Ajax kullanmaktan kaçının.

Ancak, Ajax nasıl uygulayacağınıza hakkında dikkatli varsa algılamadığı üst düzey ve alt düzey tarayıcılarla çalışan uygulamalar oluşturabilirsiniz. Contact Manager uygulamamız JavaScript destekleyen tarayıcılar ve desteklemeyen tarayıcılar ile çalışır.

İlgili Kişi Yöneticisi uygulaması JavaScript destekleyen bir tarayıcı ile kullanıyorsanız, daha iyi bir kullanıcı deneyimi sahip. Örneğin, bir kişi grubu tıkladığınızda, yalnızca Kişiler görüntüler sayfa bölge güncelleştirilir.

Diğer taraftan, JavaScript desteklemez (veya JavaScript devre dışı olan) bir tarayıcı ile ilgili kişi Yöneticisi uygulamasını kullanın, biraz daha az tercih bir kullanıcı deneyimi yüklemeniz gerekir. Bir kişi grubu tıkladığınızda, örneğin, tüm dizin görünümünün tarayıcıya kişiler eşleşen listesini görüntülemek için gönderilmelidir.

## <a name="adding-the-required-javascript-files"></a>Gerekli JavaScript dosyaları ekleme

Uygulamamız için Ajax işlevselliği eklemek için üç JavaScript dosyaları kullanmanız gerekecektir. Bu dosyaları üç yeni bir ASP.NET MVC uygulaması betikleri klasöründe bulunur.

Ajax içinde birden çok sayfada uygulamanızı kullanmayı planlıyorsanız, daha sonra uygulama s görünümü ana sayfasına gerekli JavaScript dosyaları eklemek mantıklıdır. Bu şekilde, JavaScript dosyaları, uygulamanızdaki sayfaların tümünü otomatik olarak eklenecektir.

Şu JavaScript içerir içine ekleme &lt;head&gt; , görünüm ana sayfanızın etiketi:

[!code-html[Main](iteration-7-add-ajax-functionality-cs/samples/sample1.html)]

## <a name="refactoring-the-index-view-to-use-ajax"></a>Ajax kullanacak şekilde dizin görünümünün yeniden düzenleme

Kişi bir gruba tıklayarak yalnızca Kişiler görüntüleyen görünümü bölge güncelleştirilebilmesi için bizim dizini görünümü değiştirerek Başlat s olanak tanır. Şekil 1'deki kırmızı kutu biz güncelleştirmek istediğiniz bölgeyi içerir.


[![Yalnızca Kişiler güncelleştiriliyor](iteration-7-add-ajax-functionality-cs/_static/image1.jpg)](iteration-7-add-ajax-functionality-cs/_static/image1.png)

**Şekil 01**: yalnızca Kişiler güncelleştirme ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-7-add-ajax-functionality-cs/_static/image2.png))


Zaman uyumsuz olarak ayrı bir kısmi (Görünüm kullanıcı denetimi) güncelleştirmek için istiyoruz görünümün parçası ayırmak için ilk adımdır bakın. Kişiler tablosunu görüntüler dizin görünümünün bölümünü listeleme 1 kısmi içine taşındı.

**1 - Views\Contact\ContactList.ascx listeleme**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample2.aspx)]

1. listeleme kısmi dizin görünümünün daha farklı bir modele sahip olmadığına dikkat edin. *Inherits* özniteliğini &lt;% @ sayfa %&gt; yönergesi belirtir kısmi ViewUserControl devralan&lt;grup&gt; sınıfı.

Güncelleştirilmiş dizin görünümünün listeleme 2'de yer alır.

**Listing 2 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample3.aspx)]

Güncelleştirilmiş bir görünüm listeleme 2'deki hakkında dikkat etmelidir iki nokta vardır. İlk olarak, tüm içeriği kısmi taşındı bildirimi Html.RenderPartial() çağrısıyla değiştirilir. Dizin görünümünün ilk kişiler ilk kümesini görüntülemek için istendiğinde Html.RenderPartial() yöntemi çağrılır.

İkinci olarak, kişi grupları görüntülemek için kullanılan Html.ActionLink() bir Ajax.ActionLink() ile değiştirilmiştir dikkat edin. Ajax.ActionLink() aşağıdaki parametrelerle çağrılır:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample4.aspx)]

İlk parametre bağlantı için görüntülenecek metni temsil eder, ikinci parametre rota değerlerini ve üçüncü parametre Ajax seçenekleri temsil eder. Bu durumda, UpdateTargetId Ajax seçenek HTML işaret edecek şekilde kullanırız &lt;div&gt; Ajax isteği tamamlandıktan sonra güncelleştirme istiyoruz etiketi. Biz güncelleştirmek istediğiniz &lt;div&gt; kişiler yeni listesi etiketi.

Güncelleştirilmiş İNDİS() yöntemi kişi denetleyicisinin listeleme 3'te yer alır.

**3 - Controllers\ContactController.cs (dizin yöntemi) listeleme**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample5.cs)]

Güncelleştirilmiş İNDİS() eylem koşullu olarak ikisinden birini döndürür. İNDİS() eylem bir Ajax isteği tarafından çağrılırsa denetleyicisi kısmi döndürür. Aksi takdirde, tüm görünüm İNDİS() eylem döndürür.

İNDİS() eylem bir Ajax isteği tarafından çağrıldığında kadar verileri döndürmek gerekmez dikkat edin. Normal bir isteği bağlamında dizin eylem tüm kişi grupları ve seçili kişi grubu listesini döndürür. Ajax isteği bağlamında İNDİS() eylem yalnızca seçilen grup döndürür. AJAX veritabanı sunucunuz üzerinde daha az iş anlamına gelir.

Bizim değiştirilmiş dizin görünümünün algılamadığı üst düzey ve alt düzey tarayıcılar söz konusu olduğunda çalışır. Bir kişi grubuna tıklayın ve tarayıcınızın JavaScript'i destekleyip yalnızca kişilerinizin listesini içeren görünümü bölge güncelleştirilir. Diğer taraftan, tarayıcınızın JavaScript desteklemiyorsa, tüm görünüm güncelleştirilir.


Bizim güncelleştirilmiş dizin görünümünün bir sorun vardır. Bir kişi grubu tıklattığınızda, seçilen grup vurgulanmış değil. Grup listesini bir Ajax isteği sırasında güncelleştirilir bölgesi dışında görüntülendiğinden, doğru Grup vurgulanmış değil. Biz, sonraki bölümde bu sorunu giderin.


## <a name="adding-jquery-animation-effects"></a>JQuery animasyon efektleri ekleme

Normalde, bir web sayfası bir bağlantıya tıkladığınızda, tarayıcı etkin bir şekilde güncelleştirilmiş içerik getirme olup olmadığını algılamak için tarayıcı ilerleme çubuğunu kullanabilirsiniz. Öte yandan, bir Ajax isteği gerçekleştirirken, tarayıcı ilerleme çubuğu herhangi ilerleme durumunu göstermez. Bu kullanıcılar seni çileden yapabilir. Tarayıcı dondurulmuş olup olmadığını nasıl bilebilirsiniz?

İş bir Ajax istek gerçekleştirilirken gerçekleştirildiği kullanıcıya belirtebilir birkaç yolu vardır. Bir yaklaşım basit bir animasyon görüntülemektir. Örneğin, bir Ajax isteği başlar ve istek tamamlandığında bölgede silinerek bir bölge silinerek.

Animasyon efektlerini oluşturmak için Microsoft ASP.NET MVC çerçevesiyle içerdiği jQuery kitaplık kullanacağız. Güncelleştirilmiş dizin görünümünün listeleme 4'te yer alır.

**Listing 4 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample6.aspx)]

Güncelleştirilmiş dizin görünümünün üç yeni JavaScript işlevleri içerdiğine dikkat edin. İlk iki işlevleri jQuery Kıs ve yeni bir kişi grubu tıklattığınızda kişiler listesinde silinerek için kullanın. Üçüncü işlevi bir hata (örneğin, ağ zaman aşımı) bir Ajax isteği sonuçları olduğunda hata iletisi görüntüler.

İlk işlevi de seçilen grup vurgulama mvc'deki. Bir sınıf = seçili öznitelik üst öğe tıklatıldığında öğesinin (LI öğesi) eklenir. Yeniden, jQuery, doğru öğeyi seçin ve CSS sınıfı eklemek kolaylaştırır.

Bu komut dosyalarını Ajax.ActionLink() AjaxOptions parametresi yardımıyla grup bağlantıları bağlıdır. Güncelleştirilmiş Ajax.ActionLink() yöntem çağrısı şöyle görünür:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample7.aspx)]

## <a name="adding-browser-history-support"></a>Tarayıcı geçmişi desteği ekleme

Normalde, bir sayfayı güncelleştirmek üzere bir bağlantıya tıkladığınızda, tarayıcı geçmişi güncelleştirilir. Bu şekilde zamanında sayfasının önceki durumuna geri taşımak için tarayıcıda geri düğmesini tıklatabilirsiniz. Örneğin, arkadaş kişi grubunu tıklatın ve sonra iş kişi grubunu tıklatın, arkadaş kişi grubu seçildiğinde durumuna sayfasının geri gitmek için tarayıcıda geri düğmesini tıklatabilirsiniz.

Ne yazık ki, bir Ajax isteği gerçekleştirme tarayıcı geçmişi otomatik olarak güncelleştirilmez. Kişi grubunu tıklatın ve kişiler eşleşen listesi ile Ajax isteği alınır, tarayıcı geçmişini güncelleştirilmez. Yeni bir kişi grubu seçtikten sonra bir kişi grubuna gitmek için tarayıcıda geri düğmesini kullanamazsınız.

Ajax istekleri gerçekleştirildikten sonra tarayıcı geri kullanabilmek için kullanıcıların düğmesi istiyorsanız biraz daha fazla iş gerçekleştirmek için gerekir. ASP.NET AJAX Framework yerleşik tarayıcı geçmişini yönetimi işlevselliğini yararlanmak gerekir.

ASP.NET AJAX tarayıcı geçmişi, üç şey yapmanız gerekir:

1. Tarayıcı geçmişi enableBrowserHistory özelliği true olarak ayarlayarak etkinleştirin.
2. Bir görünüm durumunu addHistoryPoint() yöntemini çağırarak değiştirdiğinde geçmiş noktalarını kaydedin.
3. Bul olay oluşturulduğunda görünüm durumunu yeniden yapılandırma.

Güncelleştirilmiş dizin görünümünün listeleme 5'te yer alır.

**Listing 5 - Views\Contact\Index.aspx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample8.aspx)]

Listeleme 5'te tarayıcı geçmişini pageInit() işlevinde etkinleştirilir. PageInit() işlevi Bul olay için olay işleyicisini ayarlamak için de kullanılır. Tarayıcı İleri veya geri düğmesini değiştirmek için sayfanın durumunu neden olan her Bul olay tetiklenir.

Bir kişi grubu tıklattığınızda beginContactList() yöntemi çağrılır. Bu yöntem, addHistoryPoint() yöntemini çağırarak yeni bir geçmiş noktası oluşturur. Tıklanan kişi grubunun kimliği geçmişine eklenir.

Grup Kimliği, bir kişi grubu bağlantıyı expando özniteliğinden alınır. Bağlantıyı Ajax.ActionLink() aşağıdaki çağrısı ile işlenir.

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample9.aspx)]

Ajax.ActionLink() geçirilen son parametre (XHTML uyumluluk için küçük harf) bağlantısına GroupID adlı bir expando özniteliği ekler.

Kullanıcı tarayıcının geri veya İleri düğmesi geldiğinde Bul olay tetiklenir ve navigate() yöntemi çağrılır. Bu yöntem Bul yönteme geçirilen tarayıcı geçmişini noktasına karşılık gelen sayfanın durumunu eşleşecek şekilde sayfasında görüntülenen kişiler güncelleştirir.

## <a name="performing-ajax-deletes"></a>AJAX gerçekleştirme siler

Şu anda bir kişiyi silmek için Delete bağlantısına tıklayın ve ardından Sil onay sayfasında görüntülenen Sil düğmesine tıklayın gerekir (bkz: Şekil 2). Bu, çok sayıda sayfa isteği veritabanı kaydını silme gibi basit bir şeyler gibi görünüyor.


[![Silme onayı sayfası](iteration-7-add-ajax-functionality-cs/_static/image2.jpg)](iteration-7-add-ajax-functionality-cs/_static/image3.png)

**Şekil 02**: silme onayı sayfası ([tam boyutlu görüntüyü görüntülemek için tıklatın](iteration-7-add-ajax-functionality-cs/_static/image4.png))


Silme onayı sayfası atlayıp doğrudan dizini görünümünden bir kişiyi silmek için tempting. Bu yaklaşımı güvenlik açıklarını uygulamanıza açtığından bu buradaki eðilim kaçınmalısınız. Genel olarak, t tan istediğiniz web uygulamanızın durumunu değiştiren bir eylem çağrılırken bir HTTP GET işlemi gerçekleştirmek. Bir silme gerçekleştirirken, bir HTTP POST gerçekleştirmek ya da bir HTTP DELETE işlemi henüz, daha iyi istiyor.

Sil bağlantısını kısmi ContactList yer alır. Kısmi ContactList güncelleştirilmiş bir sürümünü listeleme 6'yer alır.

**Listing 6 - Views\Contact\ContactList.ascx**

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample10.aspx)]

Sil bağlantısını Ajax.ImageActionLink() yöntemine aşağıdaki çağrıyı ile oluşturulur:

[!code-aspx[Main](iteration-7-add-ajax-functionality-cs/samples/sample11.aspx)]

> [!NOTE] 
> 
> Ajax.ImageActionLink() ASP.NET MVC çerçevesi standart bir parçası değil. Ajax.ImageActionLink() Contact Manager projeye dahil özel yardımcı yöntemler ' dir.


AjaxOptions parametre iki özellik içeriyor. İlk olarak, Onayla özelliği açılan JavaScript onay iletişim kutusu görüntülemek için kullanılır. İkinci olarak, HttpMethod özelliği, bir HTTP DELETE işlemi gerçekleştirmek için kullanılır.

7 listeleme kişi CONTROLLER'a eklenen yeni bir AjaxDelete() eylemi içerir.

**7 - Controllers\ContactController.cs (AjaxDelete) listeleme**

[!code-csharp[Main](iteration-7-add-ajax-functionality-cs/samples/sample12.cs)]

AjaxDelete() eylem AcceptVerbs özniteliği ile donatılmış. Bu öznitelik dışında bir HTTP DELETE işlemi dışında herhangi bir HTTP işlemi tarafından çağrılan eylemin engeller. Özellikle, bu eylem bir HTTP GET ile çağrılamaz.

Veritabanı kaydını sildikten sonra silinen kaydı içermeyen kişiler güncelleştirilmiş listesini görüntülemek gerekir. AjaxDelete() yöntemi kısmi ContactList ve kişiler güncelleştirilmiş listesini döndürür.

## <a name="summary"></a>Özet

Bu yinelemede Contact Manager uygulamamız Ajax işlevselliği eklediğimiz. Yanıt hızını ve uygulamamızı performansını geliştirmek için Ajax kullandık.

İlk olarak, kişi bir gruba tıklayarak tüm görünüm güncelleştirmez böylece biz dizin görünümünün bulunanad. Bunun yerine, bir kişi gruba tıklayarak yalnızca kişilerinizin listesini güncelleştirir.

Ardından, jQuery animasyon efektleri Kıs ve kişiler listesinde silinerek kullandık. Ajax uygulamaya animasyon ekleme bir tarayıcı ilerleme çubuğu denk uygulamanın kullanıcılarıyla sağlamak için kullanılabilir.

Biz de Ajax uygulamamız tarayıcı geçmişini desteği eklendi. Biz kullanıcıların tarayıcı geri'i tıklatın ve İleri düğmeleri dizin görünümünün durumunu değiştirmek için etkin.

Son olarak, HTTP DELETE işlemlerini destekleyen bir delete bağlantı oluşturuldu. AJAX siler gerçekleştirerek, biz kullanıcıların bir ek silme onayı sayfası istemek kullanıcının gerek kalmadan veritabanı kayıtlarını sil olanak tanır.

> [!div class="step-by-step"]
> [Önceki](iteration-6-use-test-driven-development-cs.md)
> [sonraki](iteration-1-create-the-application-vb.md)
