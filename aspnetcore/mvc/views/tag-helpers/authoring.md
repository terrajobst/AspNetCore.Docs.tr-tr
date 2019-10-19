---
title: ASP.NET Core etiket yardımcıları yazma
author: rick-anderson
description: ASP.NET Core etiket yardımcılarını yazmayı öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 04/29/2019
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: f0c7e114583b2ca2e681c507bef3487c863d8cd0
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2019
ms.locfileid: "72589868"
---
# <a name="author-tag-helpers-in-aspnet-core"></a>ASP.NET Core etiket yardımcıları yazma

[Rick Anderson](https://twitter.com/RickAndMSFT) tarafından

[Örnek kodu görüntüleme veya indirme](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Etiket yardımcıları ile çalışmaya başlama

Bu öğreticide programlama etiketi yardımcıları hakkında bir giriş sunulmaktadır. [Etiket yardımcılarına giriş](intro.md) , etiket yardımcılarının sağladığı avantajları açıklar.

Etiket Yardımcısı `ITagHelper` arabirimini uygulayan herhangi bir sınıftır. Ancak, bir etiket Yardımcısı yazdığınızda genellikle `TagHelper` ' den türetirsiniz, bunu yapmanız `Process` yöntemine erişmenizi sağlar.

1. **Authoringtaghelmakası**adlı yeni bir ASP.NET Core projesi oluşturun. Bu proje için kimlik doğrulamaya gerek yok.

1. *Taghelmakası*adlı etiket yardımcılarını tutacak bir klasör oluşturun. *Taghelmakacıları* klasörü gerekli *değildir* , ancak makul bir kuraldır. Şimdi bazı basit etiket yardımcıları yazmaya başlaalım.

## <a name="a-minimal-tag-helper"></a>En az bir etiket Yardımcısı

Bu bölümde, bir e-posta etiketini güncelleştiren bir etiket Yardımcısı yazarsınız. Örneğin:

```html
<email>Support</email>
```

Sunucu, bu biçimlendirmeyi şu şekilde dönüştürmek için e-posta etiketi yardımımız kullanacaktır:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Diğer bir deyişle, bu e-posta bağlantısını yapan bir bağlantı etikettir. Bir blog altyapısı yazıyorsanız ve BT 'nin pazarlama, destek ve diğer kişiler için aynı etki alanına e-posta göndermesini istiyorsanız bunu yapmak isteyebilirsiniz.

1. Aşağıdaki `EmailTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]

   * Etiket Yardımcıları, kök sınıf adı öğelerini hedefleyen bir adlandırma kuralı kullanır (sınıf adının sonunda *Taghelof* kısmı). Bu örnekte, **Emailtaghelper** 'nin kök adı *e-postadır*, bu nedenle `<email>` etiketi hedeflenmiş olur. Bu adlandırma kuralı, daha sonra bu şekilde nasıl geçersiz kılabileceğiniz hakkında etiket yardımcıları için çalışmalıdır.

   * @No__t_0 sınıfı `TagHelper` türetilir. @No__t_0 sınıfı, etiket yardımcıları yazmak için yöntemler ve özellikler sağlar.

   * Geçersiz kılınan `Process` yöntemi, yürütüldüğünde etiket Yardımcısı 'nın ne yaptığını denetler. @No__t_0 sınıfı ayrıca aynı parametrelerle zaman uyumsuz bir sürüm (`ProcessAsync`) sağlar.

   * @No__t_0 (ve `ProcessAsync`) için bağlam parametresi, geçerli HTML etiketinin yürütülmesiyle ilişkili bilgiler içerir.

   * @No__t_0 (ve `ProcessAsync`) çıkış parametresi, bir HTML etiketi ve içeriği oluşturmak için kullanılan orijinal kaynağın durum bilgisi olan HTML öğesi temsilcisini içerir.

   * Sınıfımızın adı, gerekli *olmayan* bir **ıghelper**sonekine sahiptir ancak en iyi yöntem kuralı olarak kabul edilir. Sınıfı şu şekilde bildirebilirsiniz:

   ```csharp
   public class Email : TagHelper
   ```

1. @No__t_0 sınıfını tüm Razor görünümlerimize uygun hale getirmek için, *views/_Viewwimports. cshtml* dosyasına `addTagHelper` yönergesini ekleyin:

   [!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]

   Yukarıdaki kod, derlemeizdeki tüm etiket yardımcılarını belirtmek için joker karakter sözdizimini kullanır. @No__t_0 sonraki ilk dize, yüklenecek etiket yardımcısını (tüm etiket yardımcıları için "*" kullanın) ve ikinci dize olan "Authoringtaghelmakaı", etiket Yardımcısı 'nın bulunduğu derlemeyi belirtir. Ayrıca, ikinci satırın joker karakter söz dizimini kullanarak ASP.NET Core MVC etiket yardımcılarına getirdiğine unutmayın (Bu yardımcılar [etiket yardımcılarına giriş](intro.md)bölümünde ele alınmıştır.) Bu, etiket yardımcısını Razor görünümü için kullanılabilir hale getiren `@addTagHelper` yönergedir. Alternatif olarak, aşağıda gösterildiği gibi bir etiket yardımcısından tam nitelikli adı (FQN) sağlayabilirsiniz:

```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->

FQN kullanarak bir görünüme bir etiket Yardımcısı eklemek için, önce FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından **derleme adı** (*authoringtaghel,* `namespace`) eklersiniz. Çoğu geliştirici joker karakter söz dizimini kullanmayı tercih eder. [Etiket yardımcılarına giriş](intro.md) , etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi hakkında ayrıntılı bilgi sağlar.

1. *Görünümler/Home/Contact. cshtml* dosyasındaki biçimlendirmeyi şu değişikliklerle güncelleştirin:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uygulamayı çalıştırın ve HTML kaynağını görüntülemek için en sevdiğiniz tarayıcıyı kullanın, böylece e-posta etiketlerinin yer işareti işaretlemesi ile değiştirildiğini doğrulayabilirsiniz (örneğin, `<a>Support</a>`). *Destek* ve *Pazarlama* bir bağlantı olarak işlenir, ancak bunları işlevsel hale getirmek için `href` bir özniteliğe sahip değildir. Sonraki bölümde bunu çözeceğiz.

## <a name="setattribute-and-setcontent"></a>SetAttribute ve SetContent

Bu bölümde, e-posta için geçerli bir tutturucu etiketi oluşturacak şekilde `EmailTagHelper` güncelleştireceğiz. Bunu, Razor görünümünden bilgi almak (`mail-to` özniteliği biçiminde) ve bağlayıcıyı oluştururken kullanmak için güncelleştireceğiz.

@No__t_0 sınıfını şu şekilde güncelleştirin:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

* Etiket Yardımcıları için Pascal özellikli sınıf ve özellik adları, [Kebab durumlarına](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101)çevrilir. Bu nedenle, `MailTo` özniteliğini kullanmak için `<email mail-to="value"/>` eşdeğerini kullanırsınız.

* Son satır, en az işlevsel etiket yardımımız için tamamlanan içeriği ayarlar.

* Vurgulanan satırda öznitelik ekleme söz dizimi gösterilmektedir:

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Bu yaklaşım, öznitelik koleksiyonunda mevcut olmadığı sürece "href" özniteliği için geçerlidir. Etiket öznitelikleri koleksiyonunun sonuna bir etiket Yardımcısı özniteliği eklemek için `output.Attributes.Add` yöntemini de kullanabilirsiniz.

1. *Görünümler/Home/Contact. cshtml* dosyasındaki biçimlendirmeyi şu değişikliklerle güncelleştirin:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

1. Uygulamayı çalıştırın ve doğru bağlantıları oluşturmadığını doğrulayın.

<a name="self-closing"></a>

   > [!NOTE]
   > E-posta etiketi kendinden kapanış (`<email mail-to="Rick" />`) yazarsanız, nihai çıkış de kendi kendini kapatıyor. Etiketi yalnızca başlangıç etiketiyle (`<email mail-to="Rick">`) yazma özelliğini etkinleştirmek için, sınıfı aşağıdaki ile tasarlamanız gerekir:
   >
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]

   Kendi kendine kapanan bir e-posta etiketi Yardımcısı ile çıkış `<a href="mailto:Rick@contoso.com" />`. Kendi kendine kapanan bağlantı etiketleri geçerli HTML değildir, bu nedenle bir tane oluşturmak istemezsiniz, ancak kendi kendini kapatan bir etiket Yardımcısı oluşturmak isteyebilirsiniz. Etiket Yardımcıları, bir etiketi okuduktan sonra `TagMode` özelliğinin türünü ayarlar.

### <a name="processasync"></a>ProcessAsync

Bu bölümde, zaman uyumsuz bir e-posta Yardımcısı yazılacak.

1. @No__t_0 sınıfını aşağıdaki kodla değiştirin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   **Notlar:**

   * Bu sürüm, zaman uyumsuz `ProcessAsync` yöntemini kullanır. Zaman uyumsuz `GetChildContentAsync`, `TagHelperContent` içeren bir `Task` döndürür.

   * HTML öğesinin içeriğini almak için `output` parametresini kullanın.

1. Etiket Yardımcısı 'nın hedef e-postayı kullanabilmesi için *Görünümler/giriş/ilgili. cshtml* dosyasında aşağıdaki değişikliği yapın.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

1. Uygulamayı çalıştırın ve geçerli bir e-posta bağlantıları oluşturmadığını doğrulayın.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent. SetHtmlContent ve PostContent. SetHtmlContent

1. Aşağıdaki `BoldTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   * @No__t_0 özniteliği, "Bold" adlı bir HTML özniteliği içeren herhangi bir HTML öğesinin eşleşeceğini ve sınıftaki `Process` geçersiz kılma yönteminin çalıştırılacağını belirten bir öznitelik parametresi geçirir. Örneğimizde, `Process` yöntemi "Bold" özniteliğini kaldırır ve içeren biçimlendirmeyi `<strong></strong>` ile çevreler.

   * Varolan etiket içeriğini değiştirmek istemediğiniz için, açma `<strong>` etiketini `PreContent.SetHtmlContent` yöntemi ve Closing `</strong>` etiketiyle `PostContent.SetHtmlContent` yöntemiyle yazmanız gerekir.

1. *About. cshtml* görünümünü bir `bold` özniteliği değeri içerecek şekilde değiştirin. Tamamlanan kod aşağıda gösterilmiştir.

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

1. Uygulamayı çalıştırın. En sevdiğiniz tarayıcıyı kullanarak kaynağı inceleyebilir ve işaretlemeyi doğrulayabilirsiniz.

   Yukarıdaki `[HtmlTargetElement]` özniteliği yalnızca "Bold" öznitelik adını sağlayan HTML işaretlemesini hedefler. @No__t_0 öğesi etiket Yardımcısı tarafından değiştirilmedi.

1. @No__t_0 öznitelik satırına açıklama ekleyin ve `<bold>` etiketlerin hedeflenmesi, diğer bir deyişle, form `<bold>` HTML biçimlendirmesi. Varsayılan adlandırma kuralının, `<bold>` etiketleri ile **kalın**taghelper sınıf adıyla eşleştiğini unutmayın.

1. Uygulamayı çalıştırın ve `<bold>` etiketinin etiket Yardımcısı tarafından işlendiğini doğrulayın.

Birden çok `[HtmlTargetElement]` özniteliği olan bir sınıfı dekorasyon, hedeflerin mantıksal veya mantıksal bir sonucu olarak sonuçlanır. Örneğin, aşağıdaki kodu kullanarak kalın bir etiket veya kalın bir öznitelik eşleşir.

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Aynı deyime birden çok öznitelik eklendiğinde, çalışma zamanı bunları mantıksal ve olarak değerlendirir. Örneğin, aşağıdaki kodda, bir HTML öğesinin eşleşmesi için "Bold" (`<bold bold />`) adlı bir özniteliğe sahip "Bold" olarak adlandırılması gerekir.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Ayrıca, hedeflenen öğenin adını değiştirmek için `[HtmlTargetElement]` de kullanabilirsiniz. Örneğin, `BoldTagHelper` `<MyBold>` etiketleri hedeflemesini istediyseniz, aşağıdaki özniteliği kullanacaksınız:

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a>Etiket Yardımcısı 'na model geçirme

1. *Modeller* klasörü ekleyin.

1. Aşağıdaki `WebsiteContext` sınıfını *modeller* klasörüne ekleyin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

1. Aşağıdaki `WebsiteInformationTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]

   * Daha önce belirtildiği gibi, etiket yardımcıları, etiket yardımcıları için C# Pascal özellikli sınıf adlarını ve özellikleri [Kebab örneğine](https://wiki.c2.com/?KebabCase)çevirir. Bu nedenle `WebsiteInformationTagHelper` Razor 'de kullanmak için `<website-information />` yazacaksınız.

   * Hedef öğeyi `[HtmlTargetElement]` özniteliğiyle açıkça tanımlamamanız, bu nedenle `website-information` varsayılan olarak hedeflenecek. Aşağıdaki özniteliği uyguladıysanız (Bu Not, Kebab büyük/küçük harf değil, ancak sınıf adıyla eşleşir):

   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```

   Kebab Case etiketi `<website-information />` eşleşmiyor. @No__t_0 özniteliğini kullanmak istiyorsanız, aşağıda gösterildiği gibi Kebab durumunu kullanacaksınız:

   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```

   * Kendi kendini kapatan öğelerde içerik yoktur. Bu örnekte, Razor biçimlendirmesi bir kendi kendine kapatma etiketi kullanacaktır, ancak etiket Yardımcısı bir [bölüm](https://www.w3.org/TR/html5/sections.html#the-section-element) öğesi oluşturacak (kendi kendini kapatmakta ve `section` öğesi içinde içerik yazıyor). Bu nedenle, çıkış yazmak için `StartTagAndEndTag` `TagMode` ayarlamanız gerekir. Alternatif olarak, `TagMode` satır ayarı ve bir kapanış etiketiyle biçimlendirme yazma ekleyebilirsiniz. (Örnek biçimlendirme Bu öğreticinin ilerleyen kısımlarında verilmiştir.)

   * Aşağıdaki satırdaki `$` (dolar işareti), bir [enterpolasyonlu dize](/dotnet/csharp/language-reference/keywords/interpolated-strings)kullanır:

   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

1. Aşağıdaki işaretlemeyi *About. cshtml* görünümüne ekleyin. Vurgulanan biçimlendirme Web sitesi bilgilerini görüntüler.

   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,4-8, 18-999)]

   > [!NOTE]
   > Aşağıda gösterilen Razor biçimlendirmesinde:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=18-18)]
   >
   > Razor `info` özniteliğin bir dize değil bir sınıf olduğunu ve kod yazmak C# istediğinizi bilir. Dize olmayan herhangi bir etiket Yardımcısı özniteliği `@` karakteri olmadan yazılmalıdır.

1. Uygulamayı çalıştırın ve Web sitesi bilgilerini görmek için hakkında görünümüne gidin.

   > [!NOTE]
   > Aşağıdaki biçimlendirmeyi bir kapanış etiketiyle kullanabilir ve etiket yardımından `TagMode.StartTagAndEndTag` satırı kaldırabilirsiniz:
   >
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=20-21)]

## <a name="condition-tag-helper"></a>Koşul etiketi Yardımcısı

Koşul etiketi Yardımcısı, doğru bir değer geçirildiğinde çıktıyı işler.

1. Aşağıdaki `ConditionTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

1. *Views/Home/Index. cshtml* dosyasının içeriğini aşağıdaki biçimlendirme ile değiştirin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

1. @No__t_1 denetleyicisindeki `Index` yöntemini aşağıdaki kodla değiştirin:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

1. Uygulamayı çalıştırın ve giriş sayfasına gidin. Koşullu `div` işaretleme işlenmez. URL 'ye `?approved=true` sorgu dizesi ekleyin (örneğin, `http://localhost:1235/Home/Index?approved=true`). `approved` true olarak ayarlanır ve koşullu biçimlendirme görüntülenir.

> [!NOTE]
> Kalın etiket Yardımcısı ile yaptığınız gibi bir dize belirtmek yerine, hedeflenecek özniteliği belirtmek için [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecini kullanın:
>
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
> [NameOf](/dotnet/csharp/language-reference/keywords/nameof) işlecinin kodu korumasını yeniden düzenlenmiş olması gerekir (adı `RedCondition` olarak değiştirmek isteyebilirsiniz).

### <a name="avoid-tag-helper-conflicts"></a>Etiket Yardımcısı çakışmalarından kaçının

Bu bölümde, bir çift bağlantı otomatik bağlama etiketi yardımcılarını yazarsınız. İlki, HTTP ile başlayan ve aynı URL 'yi içeren bir HTML Tutturucu etiketine (ve bu nedenle URL 'ye bir bağlantı oluşturan) bir URL içeren biçimlendirmenin yerini alır. İkincisi, WWW ile başlayan bir URL için aynı olur.

Bu iki yardımcılar yakından ilişkili olduğundan ve gelecekte yeniden düzenleme yapacağından, bunları aynı dosyada tutacağız.

1. Aşağıdaki `AutoLinkerHttpTagHelper` sınıfını *Taghelmakaa* klasörüne ekleyin.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   >@No__t_0 sınıfı, `p` öğeleri hedefler ve bağlayıcıyı oluşturmak için [Regex](/dotnet/standard/base-types/regular-expression-language-quick-reference) kullanır.

1. *Görünümler/Home/Contact. cshtml* dosyasının sonuna aşağıdaki biçimlendirmeyi ekleyin:

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

1. Uygulamayı çalıştırın ve etiket Yardımcısı 'nın bağlayıcıyı doğru bir şekilde işlediğini doğrulayın.

1. Www metnini, özgün www metnini de içeren bir tutturucu etiketine dönüştürecek `AutoLinkerWwwTagHelper` eklemek için `AutoLinker` sınıfını güncelleştirin. Güncelleştirilmiş kod aşağıda vurgulanır:

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

1. Uygulamayı çalıştırın. Www metninin bir bağlantı olarak işlendiğine, ancak HTTP metninin olmadığına dikkat edin. Her iki sınıfa bir kesme noktası yerleştirirseniz, önce HTTP etiketi yardımcı sınıfının çalıştığını görebilirsiniz. Bu sorun, etiket Yardımcısı çıktısının önbelleğe alınması ve WWW etiketi Yardımcısı çalıştırıldığında, HTTP etiketi yardımcısından önbelleğe alınmış çıkışın üzerine yazar. Öğreticide daha sonra, etiket yardımcıların içinde çalıştığı sırayı nasıl denetleyebiliriz. Kodu aşağıdaki kodla düzeltireceğiz:

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > Otomatik bağlama etiketi yardımcıların ilk sürümünde, hedefin içeriğini aşağıdaki kodla aldınız:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   >
   > Yani, `ProcessAsync` yöntemine geçirilen `TagHelperOutput` kullanarak `GetChildContentAsync` çağıralırsınız. Daha önce belirtildiği gibi, çıkış önbelleğe alındığından, WINS çalıştırmak için son etiket Yardımcısı. Bu sorunu aşağıdaki kodla çözdük:
   >
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   >
   > Yukarıdaki kod, içeriğin değiştirilip değiştirilmediğini denetler ve varsa çıkış arabelleğinden içeriği alır.

1. Uygulamayı çalıştırın ve iki bağlantısının beklendiği gibi çalıştığını doğrulayın. Otomatik bağlayıcı etiket yardımımız doğru ve tamamlanmış olsa da, hafif bir sorun ortaya çıkabilir. Önce WWW etiketi Yardımcısı çalışıyorsa, www bağlantıları doğru olmayacaktır. Etiketin içinde çalıştığı sırayı denetlemek için `Order` aşırı yüklemesini ekleyerek kodu güncelleştirin. @No__t_0 özelliği, aynı öğeyi hedefleyen diğer etiket yardımcıları ile ilişkili yürütme sırasını belirler. Varsayılan sıra değeri sıfırdır ve ilk olarak değerleri daha düşük olan örnekler yürütülür.

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]

   Yukarıdaki kod, HTTP etiketi Yardımcısı 'nın WWW etiketi Yardımcısı 'ndan önce çalışmasını güvence altına alır. @No__t_0 `MaxValue` olarak değiştirin ve WWW etiketi için oluşturulan biçimlendirmenin yanlış olduğunu doğrulayın.

## <a name="inspect-and-retrieve-child-content"></a>Alt içeriği İncele ve al

Etiket Yardımcıları, içerik almak için çeşitli özellikler sağlar.

* @No__t_0 sonucu `output.Content` eklenebilir.
* @No__t_0 sonucunu `GetContent` ile inceleyebilirsiniz.
* @No__t_0 değiştirirseniz, otomatik bağlayıcı örneğimizde olduğu gibi `GetChildContentAsync` çağırmadığınız sürece TagHelper gövdesi yürütülmez veya işlenmez:

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

* Birden çok `GetChildContentAsync` çağrısı aynı değeri döndürür ve önbelleğe alınmış sonucu kullanmamasını belirten yanlış bir parametre geçirmediğiniz takdirde `TagHelper` gövdesini yeniden çalıştırmaz.

## <a name="load-minified-partial-view-taghelper"></a>Mini yükleme küçük bir kısmı görüntüleme TagHelper

Üretim ortamlarında, Mini karşılaşılan kısmi görünümler yüklenirken performans artırılabilir. Üretimde küçültülmüş kısmi görünümden faydalanmak için:

* Kısmi görünümleri mini görüntülemesi için derleme öncesi bir işlem oluşturun/ayarlayın.
* Geliştirme dışı ortamlarda mini kullanılan kısmi görünümleri yüklemek için aşağıdaki kodu kullanın.

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/MinifiedVersionTagHelper.cs?name=snippet)]
