---
title: ASP.NET core'da Yazar etiket Yardımcıları
author: rick-anderson
description: ASP.NET core'da etiket Yardımcıları yazmak öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 43bd4eccfc06d27ade5de0e3387247a753609336
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662374"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>ASP.NET core'da Yazar etiket Yardımcıları

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Etiket Yardımcıları ile çalışmaya başlama

Bu öğreticide programlama etiket Yardımcıları tanıtır. [Etiket yardımcılarına giriş](intro.md) , etiket yardımcılarının sağladığı avantajları açıklar.

Etiket Yardımcısı `ITagHelper` arabirimini uygulayan herhangi bir sınıftır. Ancak, bir etiket Yardımcısı yazdığınızda genellikle `TagHelper`' den türetirsiniz, bunu yapmanız `Process` yöntemine erişmenizi sağlar.

1. **Authoringtaghelmakası**adlı yeni bir ASP.NET Core projesi oluşturun. Bu proje için kimlik doğrulaması gerekmez.

1. *Taghelmakası*adlı etiket yardımcılarını tutacak bir klasör oluşturun. *Taghelmakacıları* klasörü gerekli *değildir* , ancak makul bir kuraldır. Şimdi bazı basit etiket Yardımcıları yazmaya başlayalım.

## <a name="a-minimal-tag-helper"></a>En az bir etiketi Yardımcısı

Bu bölümde, bir e-posta etiketi güncelleştiren etiket Yardımcısı yazmaya. Örnek:

```html
<email>Support</email>
```

Sunucu, bizim e-posta etiketi Yardımcısı, biçimlendirme ve bunlar aşağıdaki şekilde dönüştürmek için kullanır:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Diğer bir deyişle, bir yer işareti etiketi, bu e-posta bağlantısı sağlar. Blog altyapısıdır ve yazma ve aynı etki alanı için pazarlama desteği ve diğer kişiler için e-posta göndermek istiyorsanız bunu yapmak isteyebilirsiniz.

1. Aşağıdaki `EmailTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * Etiket Yardımcıları, kök sınıf adı öğelerini hedefleyen bir adlandırma kuralı kullanır (sınıf adının sonunda *Taghelof* kısmı). Bu örnekte, **Emailtaghelper** 'nin kök adı *e-postadır*, bu nedenle `<email>` etiketi hedeflenmiş olur. Bu adlandırma çoğu etiket Yardımcıları için çalışır, daha sonra bu geçersiz kılma göstereceğiz.

   * `EmailTagHelper` sınıfı `TagHelper`türetilir. `TagHelper` sınıfı, etiket yardımcıları yazmak için yöntemler ve özellikler sağlar.

   * Geçersiz kılınan `Process` yöntemi, yürütüldüğünde etiket Yardımcısı 'nın ne yaptığını denetler. `TagHelper` sınıfı ayrıca aynı parametrelerle zaman uyumsuz bir sürüm (`ProcessAsync`) sağlar.

   * `Process` (ve `ProcessAsync`) için bağlam parametresi, geçerli HTML etiketinin yürütülmesiyle ilişkili bilgiler içerir.

   * `Process` (ve `ProcessAsync`) çıkış parametresi, bir HTML etiketi ve içeriği oluşturmak için kullanılan orijinal kaynağın durum bilgisi olan HTML öğesi temsilcisini içerir.

   * Sınıfımızın adı, gerekli *olmayan* bir **ıghelper**sonekine sahiptir ancak en iyi yöntem kuralı olarak kabul edilir. Sınıf olarak bildirebilirsiniz:

   ```csharp
   public class Email : TagHelper
   ```

1. `EmailTagHelper` sınıfını tüm Razor görünümlerimize kullanılabilir hale getirmek için, `addTagHelper` yönergesini *views/_ViewImports. cshtml* dosyasına ekleyin:

   [!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]

   Yukarıdaki kod, bizim derlemedeki tüm etiket Yardımcıları kullanılabilir olacağını belirtmek için joker karakter sözdizimini kullanır. `@addTagHelper` sonraki ilk dize, yüklenecek etiket yardımcısını (tüm etiket yardımcıları için "*" kullanın) ve ikinci dize olan "Authoringtaghelmakaı", etiket Yardımcısı 'nın bulunduğu derlemeyi belirtir. Ayrıca, ikinci satırın joker karakter söz dizimini kullanarak ASP.NET Core MVC etiket yardımcılarına getirdiğine unutmayın (Bu yardımcılar [etiket yardımcılarına giriş](intro.md)bölümünde ele alınmıştır.) Bu, etiket yardımcısını Razor görünümü için kullanılabilir hale getiren `@addTagHelper` yönergedir. Alternatif olarak, aşağıda gösterildiği gibi bir etiket Yardımcısı'nın (FQN) tam nitelikli ad sağlayabilirsiniz:

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

FQN kullanarak bir görünüme bir etiket Yardımcısı eklemek için, önce FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından **derleme adı** (*authoringtaghel,* `namespace`) eklersiniz. Çoğu geliştirici, joker karakter sözdizimini kullanmayı tercih eder. [Etiket yardımcılarına giriş](intro.md) , etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi hakkında ayrıntılı bilgi sağlar.

1. *Görünümler/Home/Contact. cshtml* dosyasındaki biçimlendirmeyi şu değişikliklerle güncelleştirin:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uygulamayı çalıştırın ve HTML kaynağını görüntülemek için en sevdiğiniz tarayıcıyı kullanın, böylece e-posta etiketlerinin yer işareti işaretlemesi ile değiştirildiğini doğrulayabilirsiniz (örneğin, `<a>Support</a>`). *Destek* ve *Pazarlama* bir bağlantı olarak işlenir, ancak bunları işlevsel hale getirmek için `href` bir özniteliğe sahip değildir. Sonraki bölümde, gidereceğiz.

## <a name="setattribute-and-setcontent"></a>SetAttribute ve SetContent

Bu bölümde, e-posta için geçerli bir tutturucu etiketi oluşturacak şekilde `EmailTagHelper` güncelleştireceğiz. Bunu, Razor görünümünden bilgi almak (`mail-to` özniteliği biçiminde) ve bağlayıcıyı oluştururken kullanmak için güncelleştireceğiz.

`EmailTagHelper` sınıfını şu şekilde güncelleştirin:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* Etiket Yardımcıları için Pascal özellikli sınıf ve özellik adları, [Kebab durumlarına](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)çevrilir. Bu nedenle, `MailTo` özniteliğini kullanmak için `<email mail-to="value"/>` eşdeğerini kullanırsınız.

* Son satırı tamamlanmış içeriği bizim için en düşük düzeyde işlevsel etiket Yardımcısı ayarlar.

* Vurgulanan satır öznitelikleri eklemek için sözdizimini gösterir:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Şu anda öznitelikleri koleksiyonda yok sürece bu yaklaşımı "href =" özniteliği için çalışır. Etiket öznitelikleri koleksiyonunun sonuna bir etiket Yardımcısı özniteliği eklemek için `output.Attributes.Add` yöntemini de kullanabilirsiniz.

1. *Görünümler/Home/Contact. cshtml* dosyasındaki biçimlendirmeyi şu değişikliklerle güncelleştirin:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

1. Uygulamayı çalıştırın ve doğrulayın, doğru bağlantıları oluşturur.

<a name="self-closing"></a>

   > [!NOTE]
   > E-posta etiketi kendinden kapanış (`<email mail-to="Rick" />`) yazarsanız, nihai çıkış de kendi kendini kapatıyor. Etiketi yalnızca bir başlangıç etiketiyle (`<email mail-to="Rick">`) yazma özelliğini etkinleştirmek için, sınıfı şu şekilde işaretlemeniz gerekir:
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   Kendi kendine kapanan bir e-posta etiketi Yardımcısı ile çıkış `<a href="mailto:Rick@contoso.com" />`. Kendi kendine kapanan bağlantı etiketler geçerli HTML oluşturmak istemezsiniz, ancak kendi kendine kapanan etiket Yardımcısı oluşturmak isteyebilirsiniz değildir. Etiket Yardımcıları, bir etiketi okuduktan sonra `TagMode` özelliğinin türünü ayarlar.

### <a name="processasync"></a>ProcessAsync

Bu bölümde, biz bir zaman uyumsuz bir e-posta yardımcı yazacaksınız.

1. `EmailTagHelper` sınıfını aşağıdaki kodla değiştirin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **Notlar:**

   * Bu sürüm, zaman uyumsuz `ProcessAsync` yöntemini kullanır. Zaman uyumsuz `GetChildContentAsync`, `TagHelperContent`içeren bir `Task` döndürür.

   * HTML öğesinin içeriğini almak için `output` parametresini kullanın.

1. Etiket Yardımcısı 'nın hedef e-postayı kullanabilmesi için *Görünümler/giriş/ilgili. cshtml* dosyasında aşağıdaki değişikliği yapın.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uygulamayı çalıştırın ve geçerli bir e-posta bağlantılarını oluşturur doğrulayın.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll ve PreContent.SetHtmlContent PostContent.SetHtmlContent

1. Aşağıdaki `BoldTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * `[HtmlTargetElement]` özniteliği, "Bold" adlı bir HTML özniteliği içeren herhangi bir HTML öğesinin eşleşeceğini ve sınıftaki `Process` geçersiz kılma yönteminin çalıştırılacağını belirten bir öznitelik parametresi geçirir. Örneğimizde, `Process` yöntemi "Bold" özniteliğini kaldırır ve içeren biçimlendirmeyi `<strong></strong>`ile çevreler.

   * Varolan etiket içeriğini değiştirmek istemediğiniz için, açma `<strong>` etiketini `PreContent.SetHtmlContent` yöntemi ve Closing `</strong>` etiketiyle `PostContent.SetHtmlContent` yöntemiyle yazmanız gerekir.

1. *About. cshtml* görünümünü bir `bold` özniteliği değeri içerecek şekilde değiştirin. Tamamlanan kodu aşağıda gösterilmiştir.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. Uygulamayı çalıştırın. Kaynak inceleyin ve biçimlendirme doğrulamak için sık kullandığınız tarayıcıyı kullanabilirsiniz.

   Yukarıdaki `[HtmlTargetElement]` özniteliği yalnızca "Bold" öznitelik adını sağlayan HTML işaretlemesini hedefler. `<bold>` öğesi etiket Yardımcısı tarafından değiştirilmedi.

1. `[HtmlTargetElement]` öznitelik satırına açıklama ekleyin ve `<bold>` etiketlerin hedeflenmesi, diğer bir deyişle, form `<bold>`HTML biçimlendirmesi. Varsayılan adlandırma kuralının, `<bold>` etiketleri ile **kalın**taghelper sınıf adıyla eşleştiğini unutmayın.

1. Uygulamayı çalıştırın ve `<bold>` etiketinin etiket Yardımcısı tarafından işlendiğini doğrulayın.

Birden çok `[HtmlTargetElement]` özniteliği olan bir sınıfı dekorasyon, hedeflerin mantıksal veya mantıksal bir sonucu olarak sonuçlanır. Örneğin, aşağıdaki kodu kullanarak, kalın bir etiket veya bold özniteliğinin eşleşir.

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Birden çok öznitelik için aynı deyimde eklendiğinde, çalışma zamanı bunları bir mantıksal-and davranır Örneğin, aşağıdaki kodda, bir HTML öğesinin eşleşmesi için "Bold" (`<bold bold />`) adlı bir özniteliğe sahip "Bold" olarak adlandırılması gerekir.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Ayrıca, hedeflenen öğenin adını değiştirmek için `[HtmlTargetElement]` de kullanabilirsiniz. Örneğin, `BoldTagHelper` `<MyBold>` etiketleri hedeflemesini istediyseniz, aşağıdaki özniteliği kullanacaksınız:

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a>Etiket Yardımcısı için bir model geçirin

1. *Modeller* klasörü ekleyin.

1. Aşağıdaki `WebsiteContext` sınıfını *modeller* klasörüne ekleyin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. Aşağıdaki `WebsiteInformationTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * Daha önce belirtildiği gibi, etiket yardımcıları, etiket yardımcıları için C# Pascal özellikli sınıf adlarını ve özellikleri [Kebab örneğine](https://wiki.c2.com/?KebabCase)çevirir. Bu nedenle `WebsiteInformationTagHelper` Razor 'de kullanmak için `<website-information />`yazacaksınız.

   * Hedef öğeyi `[HtmlTargetElement]` özniteliğiyle açıkça tanımlamamanız, bu nedenle `website-information` varsayılan olarak hedeflenecek. Aşağıdaki özniteliği (kebab durum geçerli değildir ama sınıfın adıyla eşleşen unutmayın) uygulandığında:

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   Kebab Case etiketi `<website-information />` eşleşmiyor. `[HtmlTargetElement]` özniteliğini kullanmak istiyorsanız, aşağıda gösterildiği gibi Kebab durumunu kullanacaksınız:

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * Kendi kendine kapanan öğeler hiçbir içeriğe sahip. Bu örnekte, Razor biçimlendirmesi bir kendi kendine kapatma etiketi kullanacaktır, ancak etiket Yardımcısı bir [bölüm](https://www.w3.org/TR/html5/sections.html#the-section-element) öğesi oluşturacak (kendi kendini kapatmakta ve `section` öğesi içinde içerik yazıyor). Bu nedenle, çıkış yazmak için `StartTagAndEndTag` `TagMode` ayarlamanız gerekir. Alternatif olarak, `TagMode` satır ayarı ve bir kapanış etiketiyle biçimlendirme yazma ekleyebilirsiniz. (Daha sonra Bu öğreticide örnek biçimlendirme sağlanır.)

   * Aşağıdaki satırdaki `$` (dolar işareti), bir [enterpolasyonlu dize](/dotnet/csharp/language-reference/keywords/interpolated-strings)kullanır:

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. Aşağıdaki işaretlemeyi *About. cshtml* görünümüne ekleyin. Vurgulanan biçimlendirme web site bilgilerini görüntüler.

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,4-8, 18-999)]

   > [!NOTE]
   > Aşağıda gösterilen Razor biçimlendirme içinde:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=18-18)]
   >
   > Razor `info` özniteliğin bir dize değil bir sınıf olduğunu ve kod yazmak C# istediğinizi bilir. Dize olmayan herhangi bir etiket Yardımcısı özniteliği `@` karakteri olmadan yazılmalıdır.

1. Uygulamayı çalıştırın ve web sitesi bilgileri görmek için hakkında görünümüne gidin.

   > [!NOTE]
   > Aşağıdaki biçimlendirmeyi bir kapanış etiketiyle kullanabilir ve etiket yardımından `TagMode.StartTagAndEndTag` satırı kaldırabilirsiniz:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=20-21)]

## <a name="condition-tag-helper"></a>Koşul etiketi Yardımcısı

True değeri geçirildiğinde çıkış koşulu etiket Yardımcısı işler.

1. Aşağıdaki `ConditionTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. *Views/Home/Index. cshtml* dosyasının içeriğini aşağıdaki biçimlendirme ile değiştirin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. `Home` denetleyicisindeki `Index` yöntemini aşağıdaki kodla değiştirin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. Uygulamayı çalıştırın ve giriş sayfasına göz atın. Koşullu `div` işaretleme işlenmez. URL 'ye `?approved=true` sorgu dizesi ekleyin (örneğin, `http://localhost:1235/Home/Index?approved=true`). `approved` true olarak ayarlanır ve koşullu biçimlendirme görüntülenir.

> [!NOTE]
> Kalın etiket Yardımcısı ile yaptığınız gibi bir dize belirtmek yerine, hedeflenecek özniteliği belirtmek için [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecini kullanın:
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kodu korumasını yeniden düzenlenmiş olması gerekir (adı `RedCondition`olarak değiştirmek isteyebilirsiniz).

### <a name="avoid-tag-helper-conflicts"></a>Etiket Yardımcısı çakışmaları

Bu bölümde, otomatik bağlama etiket Yardımcıları çifti yazın. İlk biçimlendirme içeren bir HTML yer işareti etiketi aynı URL'yi içeren (ve bu nedenle bağlantı URL'si sonuçlanmıyor) HTTP ile başlayan bir URL yerini alır. İkinci aynı WWW ile başlayan bir URL yapar.

Bu iki Yardımcıları yakından ilgili ve gelecekte yeniden çünkü bunların aynı dosyada saklayacağız.

1. Aşağıdaki `AutoLinkerHttpTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >`AutoLinkerHttpTagHelper` sınıfı, `p` öğeleri hedefler ve bağlayıcıyı oluşturmak için [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) kullanır.

1. *Görünümler/Home/Contact. cshtml* dosyasının sonuna aşağıdaki biçimlendirmeyi ekleyin:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. Uygulamayı çalıştırın ve etiket Yardımcısı bağlantı doğru şekilde işlediğinden emin olun.

1. Www metnini, özgün www metnini de içeren bir tutturucu etiketine dönüştürecek `AutoLinkerWwwTagHelper` eklemek için `AutoLinker` sınıfını güncelleştirin. Güncelleştirilmiş kod aşağıda vurgulanır:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. Uygulamayı çalıştırın. Www metin bağlantı olarak işlenir ancak HTTP metin değil dikkat edin. Her iki sınıflarda kesme noktası koymak, HTTP etiket Yardımcısı sınıfı ilk çalıştığını görebilirsiniz. Etiket Yardımcısı çıkışı önbelleğe alınır ve WWW etiketi Yardımcısı çalıştırdığınızda, önbelleğe alınan HTTP etiketi Yardımcısı çıktısı üzerine yazar sorunudur. Etiket Yardımcıları Çalıştır sırasını denetlemek nasıl öğreticinin ilerleyen bölümlerinde göreceğiz. Kodu aşağıdakiler ile gidereceğiz:

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > Otomatik bağlama etiket Yardımcıları, ilk sürümünde, aşağıdaki kod ile hedef içeriğini alındı:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > Yani, `ProcessAsync` yöntemine geçirilen `TagHelperOutput` kullanarak `GetChildContentAsync` çağıralırsınız. Belirtildiği gibi daha önce çıktıyı önbelleğe alındığından, son etiket WINS çalışmasına yardımcı. Aşağıdaki kod ile ilgili sorun düzeltildi:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > Yukarıdaki kod içeriği değiştirildi ve varsa, çıkış arabellek içeriği alır olup olmadığını denetler.

1. Uygulamayı çalıştırın ve iki bağlantı beklendiği gibi çalıştığını doğrulayın. Bizim otomatik bağlayıcı etiket Yardımcısı doğru ve eksiksiz görünebilir, ancak ince bir sorun var. WWW etiketi Yardımcısı ilk çalıştırıyorsa, www bağlantıları doğru olmayacaktır. Etiketin içinde çalıştığı sırayı denetlemek için `Order` aşırı yüklemesini ekleyerek kodu güncelleştirin. `Order` özelliği, aynı öğeyi hedefleyen diğer etiket yardımcıları ile ilişkili yürütme sırasını belirler. Varsayılan sıra değeri sıfırdır ve düşük değerler örnekleriyle önce yürütülür.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   Yukarıdaki kod, HTTP etiketi Yardımcısı önce WWW etiketi Yardımcısı çalıştığını garanti eder. `Order` `MaxValue` olarak değiştirin ve WWW etiketi için oluşturulan biçimlendirmenin yanlış olduğunu doğrulayın.

## <a name="inspect-and-retrieve-child-content"></a>Alt içeriği almak ve İnceleme

Etiket Yardımcıları içerik almak için çeşitli özellikler sağlar.

* `GetChildContentAsync` sonucu `output.Content`eklenebilir.
* `GetChildContentAsync` sonucunu `GetContent`ile inceleyebilirsiniz.
* `output.Content`değiştirirseniz, otomatik bağlayıcı örneğimizde olduğu gibi `GetChildContentAsync` çağırmadığınız sürece TagHelper gövdesi yürütülmez veya işlenmez:

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* Birden çok `GetChildContentAsync` çağrısı aynı değeri döndürür ve önbelleğe alınmış sonucu kullanmamasını belirten yanlış bir parametre geçirmediğiniz takdirde `TagHelper` gövdesini yeniden çalıştırmaz.

## <a name="load-minified-partial-view-taghelper"></a>Mini yükleme küçük bir kısmı görüntüleme TagHelper

Üretim ortamlarında, Mini karşılaşılan kısmi görünümler yüklenirken performans artırılabilir. Üretimde küçültülmüş kısmi görünümden faydalanmak için:

* Kısmi görünümleri mini görüntülemesi için derleme öncesi bir işlem oluşturun/ayarlayın.
* Geliştirme dışı ortamlarda mini kullanılan kısmi görünümleri yüklemek için aşağıdaki kodu kullanın.

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/MinifiedVersionTagHelper.cs?name=snippet)]
