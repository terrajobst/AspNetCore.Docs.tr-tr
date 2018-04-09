---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Kullanım ViewData ve uygulama ViewModel sınıfları | Microsoft Docs
author: microsoft
description: Adım 6 gösterir nasıl daha zengin form senaryolarını düzenleme için desteği etkinleştir ve veri görünümleri denetleyicilerinden iletmek için kullanılan iki yaklaşım da açıklanır:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a>Kullanım ViewData ve uygulama ViewModel sınıfları
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 6 bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> Adım 6 gösterir nasıl daha zengin form senaryolarını düzenleme için desteği etkinleştir ve veri görünümleri denetleyicilerinden iletmek için kullanılan iki yaklaşım da açıklanır: ViewData ve ViewModel.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner adım 6: ViewData ve ViewModel

Artık kapsanan form gönderme senaryolarına sayısı ve nasıl uygulanacağını ele alınan oluştur, Güncelleştir ve Sil (CRUD) desteği. Biz şimdi bizim DinnersController uygulama başka yapmanıza ve daha zengin form senaryolarını düzenleme desteği etkinleştirin. Bunu yaparken biz denetleyicilerinden görünüme veri iletmek için kullanılan iki yaklaşım ele alacağız: ViewData ve ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Veri denetleyicilerinden geçirilmesini şablonları göster

MVC örüntüsü tanımlama özelliklerini biri katı "ayrılması sorunları" bir uygulama farklı bileşenler arasında zorlanmasını sağlar. Modeller, denetleyicilere ve görünümler her iyi roller ve sorumluluklar tanımladığınız ve diğer arasında iyi tanımlanmış yollarla iletişim kurarlar. Bu Test Edilebilirlik yükseltin ve yeniden kod yardımcı olur.

HTML yanıtı istemciye işlemek bir denetleyici sınıfı kapılarını açtığında açıkça tüm verileri yanıtı işlemek için gereken görünüm şablonu geçirmesi sorumludur. Görünüm şablonları tüm veri alma veya uygulama mantığı – hiçbir zaman gerçekleştirmeniz gerekir ve bunun yerine yalnızca denetleyici tarafından geçirilen modeli/data dışına güdümlü işleme kod sağlamak için kendileri sınırlamanız gerekir.

Bizim DinnersController tarafından geçirilen görünüm şablonlarımız sınıfına basittir ve düz iletme – İNDİS() durumunda Yemeği nesnelerin bir listesini ve tek bir Yemeği Details(), Edit(), Create() ve Delete() söz konusu olduğunda nesnesi şu anda model verileri. Daha fazla kullanıcı Arabirimi özelliklerine uygulamamız için ekledikçe biz genellikle HTML yanıtlarını görünüm şablonlarımız içinde işlemek için bu verileri daha fazlasını geçirmeniz gereken alınacaktır. Örneğin, biz bizim düzenleme içinde "Ülke" alanı değiştirmek ve bir HTML metin kutusuna bir dropdownlist olmaktan görünümleri oluşturmak isteyebilirsiniz. Sabit kodlu yerine şablonu görüntüleme ülke adlarında açılır listesi, biz dinamik olarak doldurmak desteklenen ülkelerdeki listesinden oluşturmak isteyebilirsiniz. Her iki Yemeği nesneyi geçirmek için bir yol ihtiyacımız *ve* desteklenen ülkelere görünüm şablonlarımız bizim denetleyicisinden listesi.

Biz bu işlemi gerçekleştirmenin iki yolu bakalım.

### <a name="using-the-viewdata-dictionary"></a>ViewData sözlüğünü kullanarak

Denetleyici temel sınıf ek veri öğeleri denetleyicilerinden görünümlerine geçirmek için kullanılan bir "ViewData" sözlük özelliği sunar.

Örneğin, burada bir HTML metin kutusuna bir dropdownlist olmaktan bizim düzenleme görünümü içinde "Ülke" textbox değiştirmek istiyoruz senaryoyu desteklemek için biz m kullanılabilir bir SelectList nesnesi (Yemeği nesne ek olarak) geçirmek için bizim Edit() eylem yöntemini güncelleştirebilirsiniz bir ülke dropdownlist odel.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Yukarıdaki SelectList Oluşturucusu ile açılır liste doldurmak için ülkeler gibi şu anda seçili değer listesini kabul ediyor.

Biz, ardından daha önce kullandık Html.TextBox() yardımcı yöntemi yerine Html.DropDownList() yardımcı yöntemini kullanmak üzere bizim Edit.aspx görünüm şablonu güncelleştirebilirsiniz:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Yukarıdaki Html.DropDownList() yardımcı yöntemi iki parametre alır. İlk çıktısını almak için HTML form öğesi adıdır. İkinci biz ViewData sözlük geçirilen "SelectList" modelidir. C# anahtar sözcüğü "as" sözlük bir SelectList olarak içinde türe için kullanıyoruz.

Ve şimdi biz çalıştırdığınızda, uygulama ve erişim */Dinners/Edit/1* URL bizim tarayıcı içinde bizim düzenleme kullanıcı Arabirimi, metin kutusu yerine ülkelerin dropdownlist görüntülemek için güncelleştirilmiş göreceğiz:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Biz de HTTP POST Edit yöntemi (senaryolarda hatalar oluştuğunda) düzenleme görünümü şablonu oluşturmak için biz de şablonu görüntüleme hata senaryolarda işlendiğinde SelectList için ViewData eklemek için bu yöntem güncelleştiriyoruz emin olun isteyeceksiniz:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Ve şimdi DinnersController düzenleme Senaryomuzda bir DropDownList destekler.

### <a name="using-a-viewmodel-pattern"></a>ViewModel desen kullanma

ViewData sözlük yaklaşım oldukça hızlı ve kolay uygulanacak olan avantajına sahiptir. Yazım hatalarını derleme zamanında yakalanan değil hatalara yol açabilir bu yana bazı geliştiriciler dize tabanlı sözlükleri kullanılarak yine de istemeyiz. Türsüz ViewData sözlük ayrıca "olarak" işlecini kullanarak veya bir kesin türü belirtilmiş dil C# gibi bir görünüm şablonunda kullanırken atama gerektirir.

Biz kullanabilirsiniz alternatif bir yaklaşım genellikle "ViewModel" deseni olarak adlandırılan biridir. Bu deseni kullanılırken bizim belirli görünüm senaryolar için iyileştirilen ve hangi görünüm şablonlarımız tarafından gerekli dinamik değerler/içerik özelliklerini ortaya kesin türü belirtilmiş sınıfları oluşturuyoruz. Bizim denetleyicisi sınıfları sonra doldurmak ve bu görünüm için iyileştirilmiş sınıfları kullanmak üzere bizim görünüm şablonu geçirin. Bu tür güvenliği, derleme zamanı denetlemek ve düzenleyici IntelliSense görünümü şablonlar içindeki sağlar.

Örneğin, iki kesin türü belirtilmiş özellikleri sunar aşağıda biz "DinnerFormViewModel" oluşturabilir düzenleme senaryoları sınıfı gibi Yemeği formunu etkinleştirmek için: Yemeği nesne ve ülkelerde dropdownlist doldurmak için gereken SelectList modeli:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

Biz bizim depodan alıyoruz Yemeği nesnesini kullanarak DinnerFormViewModel oluşturmak için bizim Edit() eylem yöntemini güncelleştirebilir ve bizim görünüm şablonu geçirin:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Bu BT "Yemeği" yerine "DinnerFormViewModel" bekliyor şekilde bizim şablonu görüntüleme nesne sayfanın üst kısmındaki edit.aspx "devralır" özniteliği değiştirerek güncelleştirme ister sonra bunu biz gerekir:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Biz bunu yaptıktan sonra bizim görünüm şablonu içindeki "Model" özelliği IntelliSense, geçiriyoruz DinnerFormViewModel türünde nesne modeli yansıtacak şekilde güncelleştirilir:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Biz dışına çalışmaya görünüm kodumuza sonra güncelleştirebilirsiniz. Biz oluşturmakta biz nasıl girişi öğelerinin adları değiştirmiyorsanız aşağıda bildirimi (form öğeleri hala adlı "Title", "Ülke") – ancak biz DinnerFormViewModel sınıfını kullanarak değerleri almak için HTML yardımcı yöntemler güncelleştiriliyor:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Ayrıca bizim düzenleme post yöntemini hataları işlenirken DinnerFormViewModel sınıfını kullanmak için güncelleştiriyoruz:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

Biz de tam yeniden kullanmayı bizim Create() eylem yöntemleri aynı güncelleştirebilirsiniz *DinnerFormViewModel* ülkede DropDownList olanlar da etkinleştirmek için sınıf. HTTP GET uygulama aşağıdadır:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

HTTP POST oluşturmak yöntemin kullanımı aşağıdadır:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

Ve şimdi bizim hem düzenleme ve oluşturma ekranlar açılan değerlerinizi ülke çekmek için destek sağlar.

### <a name="custom-shaped-viewmodel-classes"></a>Özel şeklinde ViewModel sınıfları

Yukarıdaki senaryoda bizim DinnerFormViewModel sınıfı doğrudan Yemeği model nesnesi destekleyen SelectList model özelliğinin yanı sıra bir özellik olarak kullanıma sunar. Bu yaklaşım, burada biz bizim görünüm şablonu içinde oluşturmak istediğiniz HTML UI görece yakından bizim etki alanı model nesneleri karşılık gelen senaryoları için düzgün çalışır.

Burada bu değildir senaryoları için kullanabileceğiniz bir seçenek olan nesne modeli tarafından görünümü – daha tüketimi için optimize edilmiştir ve, temel alınan etki alanı modeli nesnesinden tamamen farklı görünebilir özel şeklinde ViewModel sınıfı oluşturmak için bir durumdur. Örneğin, onu potansiyel olarak farklı özellik adları ve/veya birden çok model nesneden toplanan toplama özellikleri geçmesine neden olabilir.

Özel şeklinde ViewModel sınıfları olabilir hem de görünümlere işlemek gibi geri bir denetleyicinin eylem yöntemine gönderilen form verilerini işlemeye yardımcı olmak için denetleyicilerinden veri iletmek için kullanılan. Daha sonra bu senaryo için ViewModel nesnesi ile form gönderilen verileri güncelleştirmek ve sonra ViewModel örneği eşlemek veya gerçek etki alanı model nesnesini almak için eylem yöntemi olabilir.

Özel şeklinde ViewModel sınıfları büyük bir bölümünü esneklik sağlayabilir ve işleme kod görünümü şablonlarınızı veya form nakil kodu içinde çok karmaşık almak başlatma eylemi yöntemlerinizi içinde bulduğunuz her zaman araştırmak için bir şeyler şunlardır. Bu genellikle bir etki alanı Modellerinizi düzgün bir şekilde, veren UI karşılık gelen yok ve Ara bir özel şeklinde ViewModel sınıf yardımcı olabilecek işaretidir.

### <a name="next-step"></a>Sonraki adım

Şimdi biz kısmi ve nasıl ana sayfalar yeniden kullanmak ve kullanıcı Arabirimi uygulamamız arasında paylaşmak için kullanabileceğinizi bakalım.

> [!div class="step-by-step"]
> [Önceki](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [sonraki](re-use-ui-using-master-pages-and-partials.md)
