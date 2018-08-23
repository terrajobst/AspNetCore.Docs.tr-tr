---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: ViewData kullanma ve ViewModel sınıfları uygulama | Microsoft Docs
author: microsoft
description: 6. adım gösterir daha zengin form senaryoları düzenleme desteği etkinleştirin ve görünümlere denetleyicilerinden veri iletmek için kullanılan iki yaklaşım da ele alınmaktadır:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 8df1ca30f8c0415b68d29eeaa0f7d83a606989ff
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754423"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>ViewData kullanma ve ViewModel sınıfları uygulama
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 6 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 6. adım gösterir daha zengin form senaryoları düzenleme desteği etkinleştirin ve görünümlere denetleyicilerinden veri iletmek için kullanılan iki yaklaşım da ele alınmaktadır: ViewData ve ViewModel.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner adım 6: ViewData ve ViewModel

Biz kapsanan form gönderme senaryolarına sayısı ve nasıl uygulanacağı ele oluştur, Güncelleştir ve Sil (CRUD) desteği. Biz artık DinnersController kararlılığımızın ileriye ve daha zengin form senaryoları düzenleme desteği etkinleştirin. Bunu yaparken görünümlerine denetleyicilerinden veri iletmek için kullanılan iki yaklaşım açıklayacağız: ViewData ve ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Görünüm şablonları denetleyicilerinden veri geçirme

MVC düzeni ayırt edici özelliklerinden biridir katı "ayrımı", bir uygulamanın farklı bileşenler arasında uygulanmasına yardımcı olur. Modelleri, denetleyicileri ve görünümleri her iyi rolleri ve sorumlulukları tanımladığınız ve diğer arasında iyi tanımlanmış şekilde iletişim kurarlar. Test Edilebilirlik yükseltin ve yeniden kod yardımcı olur.

Bir HTML yanıtı istemciye işlemek bir denetleyici sınıfı karar verdiğinde açıkça tüm veriler yanıta işlemek için gereken görünümü şablona geçirme sorumludur. Görünüm şablonları tüm veri alma veya uygulama mantığındaki hiçbir zaman gerçekleştirmeniz gerekir ve bunun yerine denetleyici tarafından geçirilen model/veri alanlarını temelli işleme kodu için yalnızca kendilerini sınırlamanız gerekir.

Bizim DinnersController tarafından geçirilen sınıf görünümü şablonlarımızı için basit ve rahatça – İNDİS() söz konusu olduğunda Dinner nesnelerin bir listesini ve tek bir Akşam Yemeği Details(), Edit(), Create() ve Delete() söz konusu olduğunda nesnesi şu anda model verileri. Daha fazla kullanıcı Arabirimi özellikleri uygulamamıza ekledikçe genellikle HTML yanıtlarını görünümü şablonlarımızı içinde işlemek için bu verileri daha fazlasını geçirmek için kullanacağız. Örneğin, biz bizim düzenleme "Ülke" alanı değiştirmek ve bir HTML metin kutusuna bir dropdownlist yüklenmesini görünümler oluşturmak isteyebilirsiniz. Sabit kodlamak yerine görünüm şablonu adlarında ülke açılan listesi, biz dinamik olarak doldurma desteklenen ülkeler listesinden oluşturmak isteyebilirsiniz. Akşam Yemeği nesneyi geçirmek için bir yol ihtiyacımız *ve* bizim denetleyicisinden desteklenen ülkeler için Görünüm şablonlarımızı listesi.

Şimdi biz bunu gerçekleştirmenin iki yollarına göz atın.

### <a name="using-the-viewdata-dictionary"></a>ViewData sözlük kullanma

Denetleyici temel sınıf ek veri öğelerinin denetleyicilerinden görünümlerine geçirmek için kullanılan bir "ViewData" sözlük özelliği sunar.

Örneğin, "Ülke" metin kutusunun bizim düzenleme görünümündeki bir HTML metin kutusuna bir dropdownlist yüklenmesini değiştirmek için istediğimiz senaryoyu desteklemek için biz m kullanılabilecek bir SelectList nesnesi (ek olarak Şimdi Akşam nesne) geçirmek için sunduğumuz Edit() eylem yöntemini güncelleştirebilirsiniz Ülkeler dropdownlist odel.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Yukarıdaki SelectList oluşturucusunun seçili değerin yanı sıra, ilçeler açılır liste ile doldurmak için bir listesini kabul ediyor.

Biz, ardından daha önce kullandığımız Html.TextBox() yardımcı yöntemi yerine Html.DropDownList() yardımcı yöntemi kullanmak üzere bizim Edit.aspx görünüm şablonu güncelleştirebilirsiniz:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Yukarıdaki Html.DropDownList() yardımcı yöntemi, iki parametre alır. İlk çıkış HTML form öğesi adıdır. İkincisi, biz ViewData sözlük geçirilen "SelectList" modelidir. C# anahtar sözcük "olarak" sözlük bir SelectList olarak içindeki tür dönüştürme için kullanıyoruz.

Ve şimdi biz çalıştırıldığında, uygulama ve erişim */Dinners/Edit/1* URL bizim tarayıcı içinde düzenleme bizim UI ülkelerde textbox yerine bir dropdownlist görüntüleyecek şekilde güncelleştirildi görüyoruz:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Biz de HTTP POST yöntemi Düzenle (senaryolarda hatalar oluştuğunda) düzenleme görünümü şablonu oluşturmak için biz de şablonu görüntüleme hatası senaryolarda işlendiğinde SelectList için ViewData eklemek için bu yöntem güncelleştiriyoruz emin olmak isteyeceksiniz:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Ve artık bir DropDownList DinnersController düzenleme senaryomuz destekler.

### <a name="using-a-viewmodel-pattern"></a>ViewModel desenini kullanarak

ViewData sözlük yaklaşım oldukça hızlı ve uygulanması kolaydır avantajına sahiptir. Yazım hatası derleme zamanında yakalanabilmesini değil hatalara neden olabileceği bazı geliştiriciler dize tabanlı sözlükleri kullanarak yine de beğenmediniz. Türsüz ViewData sözlük ayrıca "olarak" işlecini kullanarak veya bir türü kesin belirlenmiş dilde C# gibi bir şablonu görüntüleme kullanırken atama gerektirir.

Kullanabiliriz alternatif bir yaklaşım genellikle "ViewModel" deseni olarak adlandırılan biridir. Bu model kullanılırken, bizim belirli görünüm senaryolar için iyileştirilen ve hangi özellikleri görünümü şablonlarımızı gerekli dinamik değerler/içerik üzerinden kullanıma sunacaksınız kesin türü belirtilmiş sınıfları oluştururuz. Bizim denetleyici sınıflarına doldurun ve kullanmak üzere bizim görünüm şablonu bu görünüm için iyileştirilmiş sınıfların geçirin. Bu tür güvenliği, derleme zamanı denetlemek ve düzenleyici IntelliSense içinde görüntüleme şablonları sağlar.

Örneğin, iki kesin olarak belirlenmiş özellikler sunan Şimdi Akşam formunu aşağıdaki gibi biz "DinnerFormViewModel" oluşturabilirsiniz düzenleme senaryoları sınıfı etkinleştirmek için: Şimdi Akşam nesne ve ülkeler dropdownlist doldurmak için gereken SelectList modeli:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Biz ardından bizim Edit() eylem yöntemini bizim depodan alıyoruz Dinner nesnesini kullanarak DinnerFormViewModel güncelleştirin ve bizim şablonu görüntüle geçirin:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Güncelleştirme olan "Akşam Yemeği" yerine "DinnerFormViewModel" bekliyor. Bu nedenle bizim görünüm şablonu nesnesi edit.aspx sayfanın üstündeki "devralır" öznitelik değiştirerek ister sonra bunu gerçekleştireceğiz:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Bunu bir kez "Modeli" özelliğinin bizim görünüm şablonu içindeki IntelliSense, geçiriyoruz DinnerFormViewModel türü nesne modeli yansıtacak şekilde güncelleştirildi:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Biz, ardından Görünüm kodumuz dışına çalışmaya güncelleştirebilirsiniz. Biz nasıl girişi öğelerinin adları değiştirmiyorsanız aşağıda oluşturma bildirimi (form öğeleri hala adlandırılacak "Title", "Ülke") – ancak DinnerFormViewModel sınıfını kullanarak değerleri almak için HTML yardımcı yöntemler güncelleştiriyoruz:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

DinnerFormViewModel sınıfı hataları işlenirken kullanılacak bizim düzenleme post yöntemini de güncelleştireceğiz:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Biz de müşterilerimizin Create() eylem yöntemleri tam yeniden kullanmak için aynı güncelleştirebilirsiniz *DinnerFormViewModel* ülkede DropDownList olanlar da etkinleştirmek için sınıf. HTTP GET uygulama aşağıda verilmiştir:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Uygulamasını oluşturma HTTP POST yöntemi aşağıda verilmiştir:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Ve artık müşterilerimize hem düzenleme ve oluşturma ekranlar ülke seçmek için açılan değerlerinizi destekler.

### <a name="custom-shaped-viewmodel-classes"></a>Özel biçimli ViewModel sınıfları

Yukarıdaki senaryoda bizim DinnerFormViewModel sınıfı doğrudan Dinner model nesnesi destekleyici SelectList model özelliğinin yanı sıra bir özellik olarak gösterir. Bu yaklaşım, burada size bizim görünüm şablonu içinde oluşturmak istediğiniz HTML kullanıcı Arabirimi oldukça yakından bizim için etki alanı model nesneleri karşılık gelen senaryoları için düzgün çalışır.

Bu senaryolar için kullanabileceğiniz bir seçenek durumda olan nesne modeli tarafından görünümü – daha tüketimi için optimize edilmiştir ve hangi temel etki alanı model nesnesi tamamen farklı görünebilir özel biçimli ViewModel sınıfı oluşturmaktır. Örneğin, bu büyük olasılıkla farklı özellik adları ve/veya birden çok model nesneleri toplanan toplama özellikleri kullanımına sunulmasına neden olabilir.

Özel biçimli ViewModel sınıfları olabilir hem de veri görünümleri oluşturma gibi geri bir denetleyicinin eylem yöntemine gönderilen form verilerinin işlenmesi amacıyla denetleyicilerinden geçirmek için kullanılan. Daha sonra bu senaryo için ViewModel nesnesi form gönderilen veriler ile güncelleştirmek ve ardından ViewModel örneği harita veya bir gerçek etki alanı modeli nesne almak için eylem yöntemine sahip olabilir.

Özel biçimli ViewModel sınıfları büyük ölçüde esneklik sağlayabilir ve bir şey, işleme kodu görünümü şablonlarınızı veya form-posta kodu içinde çok karmaşık almak başlatma eylemi yöntemlerinizi içinde bulmak istediğiniz zaman araştırmak için olan. Bu genellikle işareti, etki alanı modellerini oluşturma kullanıcı Arabirimi düzgün bir şekilde karşılık gelmeyen ve Ara bir özel biçimli ViewModel sınıfı yardımcı olur.

### <a name="next-step"></a>Sonraki adım

Şimdi nasıl kısmi ve ana sayfalar yeniden kullanmak ve UI uygulamamız arasında paylaşmak için kullanabiliriz göz atalım.

> [!div class="step-by-step"]
> [Önceki](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [İleri](re-use-ui-using-master-pages-and-partials.md)
