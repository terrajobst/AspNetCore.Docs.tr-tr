---
title: ASP.NET core'da Yazar etiket Yardımcıları
author: rick-anderson
description: ASP.NET core'da etiket Yardımcıları yazmak öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: dbfb63bb7406ee8e20ae25b366162702ac87942c
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236464"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>ASP.NET core'da Yazar etiket Yardımcıları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Etiket Yardımcıları ile çalışmaya başlama

Bu öğreticide programlama etiket Yardımcıları tanıtır. [Etiket Yardımcıları giriş](intro.md) etiket Yardımcıları sağlayan avantajlarına açıklar.

Etiket Yardımcısı uygulayan sınıf olan `ITagHelper` arabirimi. Etiket Yardımcısı geliştirdiğinizde, Bununla birlikte, genellikle öğesinden türetilen `TagHelper`, yapmak için bunu erişmenizi `Process` yöntemi.

1. Adlı yeni bir ASP.NET Core projesi oluşturma **AuthoringTagHelpers**. Bu proje için kimlik doğrulaması gerekmez.

1. Adlı etiket Yardımcıları tutmak için bir klasör oluşturun *TagHelpers*. *TagHelpers* klasördür *değil* gerekli, ancak bunu makul bir kuraldır. Şimdi bazı basit etiket Yardımcıları yazmaya başlayalım.

## <a name="a-minimal-tag-helper"></a>En az bir etiketi Yardımcısı

Bu bölümde, bir e-posta etiketi güncelleştiren etiket Yardımcısı yazmaya. Örneğin:

```html
<email>Support</email>
```

Sunucu, bizim e-posta etiketi Yardımcısı, biçimlendirme ve bunlar aşağıdaki şekilde dönüştürmek için kullanır:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Diğer bir deyişle, bir yer işareti etiketi, bu e-posta bağlantısı sağlar. Blog altyapısıdır ve yazma ve aynı etki alanı için pazarlama desteği ve diğer kişiler için e-posta göndermek istiyorsanız bunu yapmak isteyebilirsiniz.

1. Aşağıdaki `EmailTagHelper` sınıfının *TagHelpers* klasör.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * Etiket Yardımcıları kök sınıf adı öğelerini hedefleyen bir adlandırma kuralını kullanın (eksi *TagHelper* sınıf adı kısmı). Bu örnekte, kök adı **EmailTagHelper** olduğu *e-posta*, bu nedenle `<email>` etiketi hedeflenecek. Bu adlandırma çoğu etiket Yardımcıları için çalışır, daha sonra bu geçersiz kılma göstereceğiz.

   * `EmailTagHelper` Sınıf türetilir `TagHelper`. `TagHelper` SAX yöntemleri ve özellikleri etiket Yardımcıları yazmak için.

   * Geçersiz kılınan `Process` yöntemi denetimleri etiketi Yardımcısı yürütüldüğünde yapar. `TagHelper` Sınıfı zaman uyumsuz bir sürümü sağlar (`ProcessAsync`) aynı parametrelere sahip.

   * Bağlam parametresi `Process` (ve `ProcessAsync`) geçerli HTML etiketinin yürütme ile ilgili bilgiler içerir.

   * Çıkış parametresi `Process` (ve `ProcessAsync`) bir HTML etiketi ve içerik oluşturmak için kullanılan özgün kaynağını temsil eden bir durum bilgisi olan HTML öğesi içeriyor.

   * Bizim sınıf adı son eki olan **TagHelper**, olduğu *değil* gerekli, ancak bir en iyi yöntem kuralı dikkate almıştır. Sınıf olarak bildirebilirsiniz:

   ```csharp
   public class Email : TagHelper
   ```

1. Yapmak `EmailTagHelper` bizim Razor görünümleri tüm kullanılabilir sınıf, ekleme `addTagHelper` yönergesini *Views/_ViewImports.cshtml* dosyası: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]

   Yukarıdaki kod, bizim derlemedeki tüm etiket Yardımcıları kullanılabilir olacağını belirtmek için joker karakter sözdizimini kullanır. Sonra ilk dize `@addTagHelper` yüklemek için etiket Yardımcısı belirtir (kullan "*" tüm etiket Yardımcıları için), ve ikinci dize "AuthoringTagHelpers" etiketi Yardımcısı olan derlemeyi belirtir. Ayrıca, ikinci satır joker karakter sözdizimini kullanarak ASP.NET Core MVC etiket Yardımcıları getirir unutmayın (Bu Yardımcıları ele alınmıştır [etiket Yardımcıları giriş](intro.md).) Bu `@addTagHelper` etiketi Yardımcısı Razor görünüm kullanılabilmesini yönergesi. Alternatif olarak, aşağıda gösterildiği gibi bir etiket Yardımcısı'nın (FQN) tam nitelikli ad sağlayabilirsiniz:

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

Etiket Yardımcısı kullanarak bir FQN görünümüne eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından **derleme adı** (*AuthoringTagHelpers*, değil necessarly `namespace`). Çoğu geliştirici, joker karakter sözdizimini kullanmayı tercih eder. [Etiket Yardımcıları giriş](intro.md) etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi, ayrıntıya gider.

1. Biçimlendirme içinde güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uygulamayı çalıştırın ve e-posta etiketler ile bağlantı biçimlendirme değiştirilir doğrulayabilmeniz için HTML kaynağını görüntülemek için sık kullandığınız tarayıcıyı kullanabilirsiniz (örneğin, `<a>Support</a>`). *Destek* ve *pazarlama* bir bağlantı olarak işlenir ancak bir `href` işlevsel hale getirmek için özniteliği. Sonraki bölümde, gidereceğiz.

## <a name="setattribute-and-setcontent"></a>SetAttribute ve SetContent

Bu bölümde, güncelleştireceğiz `EmailTagHelper` böylece e-posta için bir geçerli yer işareti etiketi oluşturur. Razor görünümü bilgilerinden gerçekleştirecek şekilde güncelleştireceğiz (biçiminde bir `mail-to` özniteliği) ve bağlantı oluşturmak kullanın.

Güncelleştirme `EmailTagHelper` aşağıdaki sınıfı:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* İçine çevrilmiş Pascal büyük küçük harfleri sınıf ve özellik adlarını etiket Yardımcıları için kendi [kebab çalışması](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Bu nedenle, kullanılacak `MailTo` kullanacağınız özniteliği `<email mail-to="value"/>` eşdeğer.

* Son satırı tamamlanmış içeriği bizim için en düşük düzeyde işlevsel etiket Yardımcısı ayarlar.

* Vurgulanan satır öznitelikleri eklemek için sözdizimini gösterir:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Şu anda öznitelikleri koleksiyonda yok sürece bu yaklaşımı "href =" özniteliği için çalışır. Ayrıca `output.Attributes.Add` etiketi yardımcı öznitelik etiketi özniteliklerinin koleksiyonunu sonuna eklemek için yöntemi.

1. Biçimlendirme içinde güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

1. Uygulamayı çalıştırın ve doğrulayın, doğru bağlantıları oluşturur.

<a name="self-closing"></a>

   > [!NOTE]
   > E-posta kendi kendine kapanan etiket yazmak için olsaydı (`<email mail-to="Rick" />`), son Çıkışta aynı zamanda kendi kendine kapanan olacaktır. Etiket yalnızca bir başlangıç etiketi ile yazma olanağı sağlamak (`<email mail-to="Rick">`) aşağıdaki sınıf tasarlamanız gerekir:
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   Bir kendi kendine kapanan e-posta etiket Yardımcısı ile çıkış olacaktır `<a href="mailto:Rick@contoso.com" />`. Kendi kendine kapanan bağlantı etiketler geçerli HTML oluşturmak istemezsiniz, ancak kendi kendine kapanan etiket Yardımcısı oluşturmak isteyebilirsiniz değildir. Etiket Yardımcıları türünü ayarlayın `TagMode` sonra bir etiketi okuma özelliği.

### <a name="processasync"></a>ProcessAsync

Bu bölümde, biz bir zaman uyumsuz bir e-posta yardımcı yazacaksınız.

1. Değiştirin `EmailTagHelper` aşağıdaki kodla sınıfı:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **Notlar:**

   * Bu sürüm, zaman uyumsuz kullanır `ProcessAsync` yöntemi. Zaman uyumsuz `GetChildContentAsync` döndürür bir `Task` içeren `TagHelperContent`.

   * Kullanım `output` parametresi HTML öğesinden içeriği alamadı.

1. Aşağıdaki değişiklik *Views/Home/Contact.cshtml* etiketi Yardımcısı hedef e-posta alabilmeniz için dosya.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uygulamayı çalıştırın ve geçerli bir e-posta bağlantılarını oluşturur doğrulayın.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll ve PreContent.SetHtmlContent PostContent.SetHtmlContent

1. Aşağıdaki `BoldTagHelper` sınıfının *TagHelpers* klasör.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * `[HtmlTargetElement]` Özniteliği belirten bir HTML öznitelik içeren herhangi bir HTML öğesi "bold" adlı bir öznitelik parametresi eşleşir, geçişleri ve `Process` sınıfı yöntemi geçersiz kılma çalışır. Bizim örnek içinde `Process` yöntemi "kalın" özniteliğini kaldırır ve içeren biçimlendirme çevreleyen `<strong></strong>`.

   * İçerik var olan etiket değiştirilecek istemediğinden açılış yazmanız gereken `<strong>` ile etiketi `PreContent.SetHtmlContent` yöntemi ve kapatma `</strong>` ile etiketi `PostContent.SetHtmlContent` yöntemi.

1. Değiştirme *About.cshtml* görünümü içerecek bir `bold` öznitelik değeri. Tamamlanan kodu aşağıda gösterilmiştir.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. Uygulamayı çalıştırın. Kaynak inceleyin ve biçimlendirme doğrulamak için sık kullandığınız tarayıcıyı kullanabilirsiniz.

   `[HtmlTargetElement]` Yukarıdaki özniteliği bir öznitelik adı "kalın" sağlayan HTML biçimlendirmesi hedefler. `<bold>` Öğesi etiketi Yardımcısı tarafından değiştirilen değildi.

1. Açıklama `[HtmlTargetElement]` özniteliği satır ve varsayılan hedeflemeye `<bold>` etiketleri, diğer bir deyişle, HTML biçimlendirmesi formun `<bold>`. Unutmayın, varsayılan adlandırma kuralı, sınıf adı eşleşir **kalın**için TagHelper `<bold>` etiketler.

1. Uygulamayı çalıştırın ve doğrulayın `<bold>` etiketin, etiket Yardımcısı tarafından işlenir.

Bir sınıf birden çok dekorasyon `[HtmlTargetElement]` hedefleri mantıksal-veya sonuçlarını öznitelikleri. Örneğin, aşağıdaki kodu kullanarak, kalın bir etiket veya bold özniteliğinin eşleşir.

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Birden çok öznitelik için aynı deyimde eklendiğinde, çalışma zamanı bunları bir mantıksal-and davranır Örneğin, aşağıdaki kod bir HTML öğesi "bold" adlı bir öznitelik ile "bold" olarak adlandırılmalıdır (`<bold bold />`) eşleştirilecek.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Ayrıca `[HtmlTargetElement]` hedeflenen öğesi adını değiştirmek için. İsterseniz örneğin `BoldTagHelper` hedef `<MyBold>` etiketleri, aşağıdaki öznitelik kullanmanız gerekir:

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a>Etiket Yardımcısı için bir model geçirin

1. Ekleme bir *modelleri* klasör.

1. Aşağıdaki `WebsiteContext` sınıfının *modelleri* klasörü:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. Aşağıdaki `WebsiteInformationTagHelper` sınıfının *TagHelpers* klasör.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * Daha önce belirtildiği gibi etiket Yardımcıları çevirir Pascal büyük küçük harfleri C# sınıf adları ve Özellikler etiket Yardımcıları için [kebab çalışması](http://wiki.c2.com/?KebabCase). Bu nedenle, kullanılacak `WebsiteInformationTagHelper` Razor içinde yazacaksınız `<website-information />`.

   * Açıkça hedef öğeyi tanımlayan değil `[HtmlTargetElement]` özniteliği, bunu varsayılan `website-information` hedeflenir. Aşağıdaki özniteliği (kebab durum geçerli değildir ama sınıfın adıyla eşleşen unutmayın) uygulandığında:

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   Kebab case etiketi `<website-information />` eşleşen mıydı. Kullanmak istediğiniz `[HtmlTargetElement]` özniteliği, aşağıda gösterildiği gibi kebab çalışması kullanmanız:

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * Kendi kendine kapanan öğeler hiçbir içeriğe sahip. Bu örnekte, bir kendi kendine kapanan etiketi Razor işaretlemesi kullanır, ancak etiketi Yardımcısı oluşturma bir [bölümü](http://www.w3.org/TR/html5/sections.html#the-section-element) öğesi (hangi kendi kendine kapanan değildir ve içeriği yazıyorsanız `section` öğesi). Bu nedenle, ayarlanacak ihtiyacınız `TagMode` için `StartTagAndEndTag` çıkışını yazmak için. Satır ayarını WMI'ya alternatif olarak, yorum `TagMode` ve bir kapatma etiketi ile işaretleme yazma. (Daha sonra Bu öğreticide örnek biçimlendirme sağlanır.)

   * `$` (Dolar işareti) dosyasında aşağıdaki satırı kullanan bir [ilişkilendirilmiş bir dizedir](/dotnet/csharp/language-reference/keywords/interpolated-strings):

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. İçin aşağıdaki işaretlemeyi ekleyin *About.cshtml* görünümü. Vurgulanan biçimlendirme web site bilgilerini görüntüler.

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,4-8, 18-999)]

   > [!NOTE]
   > Aşağıda gösterilen Razor biçimlendirme içinde:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=18-18)]
   >
   > Razor bilir `info` özniteliği bir sınıf, bir dize değil ve C# kodu yazmak istediğiniz. Herhangi bir dize olmayan etiketi yardımcı öznitelik olmadan yazılmalıdır `@` karakter.

1. Uygulamayı çalıştırın ve web sitesi bilgileri görmek için hakkında görünümüne gidin.

   > [!NOTE]
   > Aşağıdaki biçimlendirme bir kapatma etiketi ile kullanın ve içeren satırı Kaldır `TagMode.StartTagAndEndTag` etiketi Yardımcısı'nda:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Koşul etiketi Yardımcısı

True değeri geçirildiğinde çıkış koşulu etiket Yardımcısı işler.

1. Aşağıdaki `ConditionTagHelper` sınıfının *TagHelpers* klasör.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. Öğesinin içeriğini değiştirin *Views/Home/Index.cshtml* aşağıdaki işaretlemeyle dosyası:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. Değiştirin `Index` yönteminde `Home` aşağıdaki kodla denetleyicisi:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. Uygulamayı çalıştırın ve giriş sayfasına göz atın. Koşullu biçimlendirmeyi `div` işlenmez. Sorgu dizesini URL'ye `?approved=true` URL'sine (örneğin, `http://localhost:1235/Home/Index?approved=true`). `approved` TRUE olarak ve koşullu biçimlendirme görüntülenir.

> [!NOTE]
> Kullanım [nameof](/dotnet/csharp/language-reference/keywords/nameof) işleci ile kalın etiket Yardımcısı yaptığınız gibi dize belirtme yerine hedef özniteliğe belirtmek için:
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> [Nameof](/dotnet/csharp/language-reference/keywords/nameof) işleci korumak kod hiç olmadığı kadar yeniden (biz adını değiştirmek de isteyebilirsiniz `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Etiket Yardımcısı çakışmaları

Bu bölümde, otomatik bağlama etiket Yardımcıları çifti yazın. İlk biçimlendirme içeren bir HTML yer işareti etiketi aynı URL'yi içeren (ve bu nedenle bağlantı URL'si sonuçlanmıyor) HTTP ile başlayan bir URL yerini alır. İkinci aynı WWW ile başlayan bir URL yapar.

Bu iki Yardımcıları yakından ilgili ve gelecekte yeniden çünkü bunların aynı dosyada saklayacağız.

1. Aşağıdaki `AutoLinkerHttpTagHelper` sınıfının *TagHelpers* klasör.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >`AutoLinkerHttpTagHelper` Sınıfı hedefler `p` öğeleri ve kullanıyor [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) bağlantı oluşturmak için.

1. Sonuna aşağıdaki işaretlemeyi ekleyin *Views/Home/Contact.cshtml* dosyası:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. Uygulamayı çalıştırın ve etiket Yardımcısı bağlantı doğru şekilde işlediğinden emin olun.

1. Güncelleştirme `AutoLinker` içerecek şekilde sınıfı `AutoLinkerWwwTagHelper` hangi www metne dönüştürme ayrıca orijinal www metni içeren bir yer işareti etiketi. Güncelleştirilmiş kod aşağıda vurgulanır:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. Uygulamayı çalıştırın. Www metin bağlantı olarak işlenir ancak HTTP metin değil dikkat edin. Her iki sınıflarda kesme noktası koymak, HTTP etiket Yardımcısı sınıfı ilk çalıştığını görebilirsiniz. Etiket Yardımcısı çıkışı önbelleğe alınır ve WWW etiketi Yardımcısı çalıştırdığınızda, önbelleğe alınan HTTP etiketi Yardımcısı çıktısı üzerine yazar sorunudur. Etiket Yardımcıları Çalıştır sırasını denetlemek nasıl öğreticinin ilerleyen bölümlerinde göreceğiz. Kodu aşağıdakiler ile gidereceğiz:

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > Otomatik bağlama etiket Yardımcıları, ilk sürümünde, aşağıdaki kod ile hedef içeriğini alındı:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > Diğer bir deyişle, çağırma `GetChildContentAsync` kullanarak `TagHelperOutput` yöntemlere geçirilen `ProcessAsync` yöntemi. Belirtildiği gibi daha önce çıktıyı önbelleğe alındığından, son etiket WINS çalışmasına yardımcı. Aşağıdaki kod ile ilgili sorun düzeltildi:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > Yukarıdaki kod içeriği değiştirildi ve varsa, çıkış arabellek içeriği alır olup olmadığını denetler.

1. Uygulamayı çalıştırın ve iki bağlantı beklendiği gibi çalıştığını doğrulayın. Bizim otomatik bağlayıcı etiket Yardımcısı doğru ve eksiksiz görünebilir, ancak ince bir sorun var. WWW etiketi Yardımcısı ilk çalıştırıyorsa, www bağlantıları doğru olmayacaktır. Ekleyerek kodu güncelleştirmeniz `Order` etiket çalışan sırasını denetlemek için aşırı yükleme. `Order` Özelliği aynı öğeye hedefleyen diğer etiket Yardımcıları göre yürütme sırasını belirler. Varsayılan sıra değeri sıfırdır ve düşük değerler örnekleriyle önce yürütülür.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   Yukarıdaki kod, HTTP etiketi Yardımcısı önce WWW etiketi Yardımcısı çalıştığını garanti eder. Değişiklik `Order` için `MaxValue` ve WWW etiketi için oluşturulmuş biçimlendirmeyi yanlış olduğundan emin olun.

## <a name="inspect-and-retrieve-child-content"></a>Alt içeriği almak ve İnceleme

Etiket Yardımcıları içerik almak için çeşitli özellikler sağlar.

* Sonucu `GetChildContentAsync` eklenerek `output.Content`.
* Sonucu inceleyebilirsiniz `GetChildContentAsync` ile `GetContent`.
* Değiştirirseniz `output.Content`, TagHelper gövdesi olmaz yürütülen veya siz işlenen `GetChildContentAsync` otomatik bağlayıcı örneğimizi olduğu gibi:

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* Birden çok çağrılar `GetChildContentAsync` aynı değeri döndürür ve yeniden yürütülmez `TagHelper` önbelleğe alınan sonuç kullanmayı gösteren yanlış parametre geçirmezseniz gövde.
