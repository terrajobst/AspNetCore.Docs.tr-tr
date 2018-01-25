---
title: "ASP.NET Core içinde etiket Yardımcıları yazma"
author: rick-anderson
description: "ASP.NET Core içinde etiket Yardımcıları yazmak öğrenin."
ms.author: riande
manager: wpickett
ms.date: 01/19/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/authoring
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1f1b2c2e60a1337c15f019185c764d0a9ada1b5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a>ASP.NET Core, örnekleri ile ilgili bir kılavuz olarak yazar etiket Yardımcıları

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="get-started-with-tag-helpers"></a>Etiket Yardımcıları ile çalışmaya başlama

Bu öğretici programlama etiket Yardımcıları tanıtılmaktadır. [Etiket Yardımcıları giriş](intro.md) etiket Yardımcıları sağlamak faydalarını anlatır.

Bir etiket Yardımcısı uygulayan herhangi bir sınıftır `ITagHelper` arabirimi. Bir etiket Yardımcısı yazarken, Bununla birlikte, genellikle öğesinden türetilen `TagHelper`, sizin için bunu erişmenizi yapılması `Process` yöntemi.

1. Adlı yeni bir ASP.NET Core projesi oluşturma **AuthoringTagHelpers**. Bu proje için kimlik doğrulama gerekmez.

2. Adlı etiket Yardımcıları tutmak için bir klasör oluşturun *TagHelpers*. *TagHelpers* klasörü *değil* gerekli, ancak makul bir kuraldır. Şimdi bazı basit etiket Yardımcıları yazmak başlayalım.

## <a name="a-minimal-tag-helper"></a>En az bir etiket Yardımcısı

Bu bölümde, bir e-posta etiketi güncelleştiren bir etiket Yardımcısı yazma. Örneğin:

```html
<email>Support</email>
   ```

Sunucu, bu biçimlendirme aşağıdaki dönüştürmek için bizim e-posta etiket Yardımcısı kullanır:

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

Diğer bir deyişle, bir yer işareti etiketi, bu bir e-posta bağlantısı sağlar. Blog altyapısıdır ve yazma ve aynı etki alanına tüm pazarlama, destek ve diğer kişiler için e-posta göndermek istiyorsanız bunu yapmak isteyebilirsiniz.

1.  Aşağıdakileri ekleyin `EmailTagHelper` sınıfının *TagHelpers* klasör.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    **Notlar:**
    
    * Etiket Yardımcıları kök sınıf adının öğeleri hedefleyen bir adlandırma kuralını kullanın (eksi *TagHelper* sınıf adı bölümü). Bu örnekte, kök adı **e-posta**TagHelper olan *e-posta*, böylece `<email>` etiketi hedeflenir. Bu adlandırma kuralını çoğu etiket yardımcılarında çalışması gerekir, daha sonra onu geçersiz kılmak nasıl göstereceğiz.
    
    * `EmailTagHelper` Sınıfı türer `TagHelper`. `TagHelper` Sınıfı sağlar yöntemleri ve özellikleri etiket Yardımcıları yazmak için.
    
    * Geçersiz kılınan `Process` yöntemini kontrol etiket Yardımcısı çalıştırıldığında yapar. `TagHelper` Sınıfı zaman uyumsuz bir sürümünü sağlar (`ProcessAsync`) aynı parametrelere sahip.
    
    * Bağlam parametresi `Process` (ve `ProcessAsync`) geçerli HTML etiket yürütülmesi ile ilgili bilgileri içerir.
    
    * Çıktı parametresi `Process` (ve `ProcessAsync`) bir HTML etiketi ve içeriği oluşturmak için kullanılan özgün kaynağını temsili bir durum bilgisi olan HTML öğesi içeriyor.
    
    * Bizim sınıf adı sonekine sahip **TagHelper**, olduğu *değil* gerekli, ancak en iyi yöntem kuralı dikkate almıştır. Sınıf olarak bildirebilirsiniz:
    
    ```csharp
    public class Email : TagHelper
    ```

2.  Yapmak için `EmailTagHelper` bizim tüm Razor görünümleri kullanılabilir sınıfı, ekleme `addTagHelper` için yönerge *Views/_ViewImports.cshtml* dosyası:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]
    
    Yukarıdaki kod bizim derlemesindeki tüm etiket Yardımcıları kullanılabilir olacağını belirtmek için joker karakter sözdizimini kullanır. İlk dizesinden sonra `@addTagHelper` yüklemek için etiket Yardımcısı belirtir (kullan "*" tüm etiket Yardımcıları için), ve ikinci dize "AuthoringTagHelpers" etiketi yardımcı olan derleme belirtir. Ayrıca, ikinci satır joker karakter sözdizimini kullanarak ASP.NET Core MVC etiket Yardımcıları getirir unutmayın (Bu Yardımcıları ele alınmıştır [etiket Yardımcıları giriş](intro.md).) Bu `@addTagHelper` etiket Yardımcısı Razor görünüm için kullanılabilir hale getirir yönergesi. Alternatif olarak, aşağıda gösterildiği gibi bir etiket Yardımcısı, tam adı (FQN) sağlayabilirsiniz:
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
Bir FQN kullanarak görünüm için bir etiket Yardımcısı eklemek için önce FQN ekleyin (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) ve ardından derleme adı (*AuthoringTagHelpers*). Çoğu geliştirici, joker karakter sözdizimi kullanmayı tercih eder. [Etiket Yardımcıları giriş](intro.md) etiket Yardımcısı ekleme, kaldırma, hiyerarşi ve joker karakter sözdizimi, ayrıntıya gider.
    
3.  Biçimlendirme güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  Uygulamayı çalıştırın ve e-posta etiketleri olan bağlantı biçimlendirme değiştirilir doğrulayabilmeniz için HTML kaynağını görüntülemek için sık kullanılan tarayıcınızı kullanın (örneğin, `<a>Support</a>`). *Destek* ve *pazarlama* bağlantılar olarak işlenir ancak sahip olmayan bir `href` çalışır duruma getirme özniteliği. Biz, sonraki bölümde düzeltmesi.

Not: HTML etiketleri ve öznitelikleri, etiketler, sınıf adları ve Razor ve C# öznitelikleri gibi büyük küçük harfe duyarlı değildir.

## <a name="setattribute-and-setcontent"></a>SetAttribute ve SetContent

Bu bölümde, biz güncelleştireceğim `EmailTagHelper` böylece e-posta için geçerli yer işareti etiketi oluşturur. Razor görünüm bilgilerinden gerçekleştirecek şekilde güncelleştiriyoruz (biçiminde bir `mail-to` özniteliği) ve bağlantı oluşturma kullanan.

Güncelleştirme `EmailTagHelper` aşağıdaki sınıfı:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

**Notlar:**

* İçine çevrilmiş Pascal ortası sınıf ve özellik adlarını etiket Yardımcıları için kendi [alt kebab durumda](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101). Bu nedenle, kullanılacak `MailTo` kullanın, öznitelik `<email mail-to="value"/>` eşdeğer.

* Son satırı tamamlanmış içerik bizim için en düşük düzeyde işlev etiket Yardımcısı ayarlar.

* Vurgulanan satırı öznitelikler eklemek için sözdizimi gösterilmektedir:

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

Şu anda öznitelikleri koleksiyonunda yok sürece bu yaklaşımı "href" özniteliği için çalışır. Aynı zamanda `output.Attributes.Add` yöntemi etiketi yardımcı öznitelik etiketi özniteliklerin koleksiyonu sonuna ekleyin.

1.  Biçimlendirme güncelleştirme *Views/Home/Contact.cshtml* bu değişikliklerle dosyası:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]

2.  Uygulamayı çalıştırın ve doğru bağlantıları oluşturur doğrulayın.
    
    > [!NOTE]
    >E-posta otomatik olarak kapanış etiketi yazmak için olsaydı (`<email mail-to="Rick" />`), son çıkışı da kendi kendine kapanan olacaktır. Yalnızca bir başlangıç etiketi etiketiyle yazma özelliği etkinleştirmek için (`<email mail-to="Rick">`) aşağıdaki sınıfıyla tasarlamanız gerekir:
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    Bir kendi kendine kapanan e-posta etiket Yardımcısı ile çıkış olacaktır `<a href="mailto:Rick@contoso.com" />`. Kendi kendine kapanan bağlayıcı etiketler geçerli HTML oluşturmak istemezsiniz ancak kendi kendine kapanan etiketi yardımcı oluşturmak isteyebilirsiniz değildir. Etiket Yardımcıları türünü ayarlayın `TagMode` bir etiket okuma sonra özelliği.
    
### <a name="processasync"></a>ProcessAsync

Bu bölümde, biz bir zaman uyumsuz e-posta Yardımcısı yazacaksınız.

1.  Değiştir `EmailTagHelper` aşağıdaki kodla sınıfı:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    **Notlar:**

    * Bu sürümü zaman uyumsuz kullanan `ProcessAsync` yöntemi. Zaman uyumsuz `GetChildContentAsync` döndüren bir `Task` içeren `TagHelperContent`.

    * Kullanım `output` HTML öğesi içeriğini almak için parametre.

2.  Aşağıdaki değişiklik *Views/Home/Contact.cshtml* etiket Yardımcısı hedef e-posta sınıflandırıp dosya.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  Uygulamayı çalıştırın ve geçerli e-posta bağlantılar oluşturup doğrulayın.

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a>RemoveAll, PreContent.SetHtmlContent ve PostContent.SetHtmlContent

1.  Aşağıdakileri ekleyin `BoldTagHelper` sınıfının *TagHelpers* klasör.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    **Notlar:**
    
    * `[HtmlTargetElement]` Öznitelik HTML özniteliği içeren herhangi bir HTML öğesi "kalın" adlı belirten bir öznitelik parametre eşleşir, geçişleri ve `Process` sınıfında geçersiz kılma yöntemi çalışır. Bizim örnek `Process` yöntemi "kalın" özniteliğini kaldırır ve içeren biçimlendirme ile çevreleyen `<strong></strong>`.
    
    * İçerik varolan etiketi değiştirmek istemeyeceğiniz için açılış yazmalısınız `<strong>` ile etiketi `PreContent.SetHtmlContent` yöntemi ve kapanış `</strong>` ile etiketi `PostContent.SetHtmlContent` yöntemi.
    
2.  Değiştirme *About.cshtml* görünüm içeren bir `bold` öznitelik değeri. Tamamlanan kodu aşağıda gösterilmiştir.

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  Uygulamayı çalıştırın. Sık kullanılan tarayıcınız kaynak inceleyin ve biçimlendirmesini doğrulamak için kullanabilirsiniz.

    `[HtmlTargetElement]` Yukarıda öznitelik yalnızca hedef bir özniteliğin adını "kalın" sağlayan HTML biçimlendirmesi. `<bold>` Öğesi etiket Yardımcısı tarafından değiştirilmiş değildi.

4. Out açıklama `[HtmlTargetElement]` özniteliği satır ve varsayılan olarak hedefleniyor `<bold>` etiketleri, diğer bir deyişle, HTML biçimlendirmesi biçiminde `<bold>`. Unutmayın, varsayılan adlandırma kuralını sınıf adı eşleşir **kalın**TagHelper için `<bold>` etiketler.

5. Uygulamayı çalıştırın ve doğrulayın `<bold>` etiket etiket Yardımcısı tarafından işlenir.

Birden çok sınıfıyla dekorasyon `[HtmlTargetElement]` hedefleri veya mantıksal sonuçlarını öznitelikleri. Örneğin, aşağıdaki kodu kullanarak, bir kalın etiketi veya kalın özniteliği ile eşleşir.

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

Birden çok öznitelik aynı ifadesine eklendiğinde, çalışma zamanı bunları bir mantıksal-and değerlendirir Örneğin kodda bir HTML öğesi "kalın" adlı bir özniteliği "bold" olarak adlandırılmalıdır (`<bold bold />`) eşleştirmek için.

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

Aynı zamanda `[HtmlTargetElement]` hedeflenen öğesinin adını değiştirmek için. İsterseniz örneğin `BoldTagHelper` hedef `<MyBold>` etiketleri, aşağıdaki öznitelik kullanırsınız:

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a>Bir model için bir etiket Yardımcısı geçirin

1.  Ekleme bir *modelleri* klasör.

2.  Aşağıdakileri ekleyin `WebsiteContext` sınıfının *modelleri* klasörü:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  Aşağıdakileri ekleyin `WebsiteInformationTagHelper` sınıfının *TagHelpers* klasör.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    **Notlar:**
    
    * Daha önce belirtildiği gibi etiket Yardımcıları çevirir Pascal ortası C# sınıf adları ve özellikleri etiket Yardımcıları için [alt kebab durumda](http://wiki.c2.com/?KebabCase). Bu nedenle, kullanılacak `WebsiteInformationTagHelper` Razor içinde yazacaksınız `<website-information />`.
    
    * Açıkça hedef öğeyle belirlemek değil `[HtmlTargetElement]` özniteliği, bunu varsayılan değer olan `website-information` hedeflenir. Aşağıdaki öznitelik (kebab söz konusu değildir ancak sınıfı adıyla eşleşen unutmayın) uyguladıysanız:
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    Daha düşük kebab durum etiketini `<website-information />` eşleşen olmayacaktır. Kullanmak istiyorsanız, `[HtmlTargetElement]` özniteliği, aşağıda gösterildiği gibi kebab durumda kullanmak:
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * Kendi kendine kapanan öğe hiçbir içeriğe sahip. Bu örnekte, kendi kendine kapanan etiketi Razor biçimlendirme kullanır, ancak etiket Yardımcısı oluşturma bir [bölüm](http://www.w3.org/TR/html5/sections.html#the-section-element) öğesi (hangi kendi kendine kapanan değil ve içeriğini yazıyorsanız `section` öğesi). Bu nedenle, ayarlamanız gerekir `TagMode` için `StartTagAndEndTag` çıkışını yazmak için. Satır ayarını WMI'ya alternatif olarak, açıklama `TagMode` ve kapanış etiketi ile işaretleme yazma. (Örneğin biçimlendirme daha sonra Bu öğreticide sağlanır.)
    
    * `$` (Dolar işareti) aşağıdaki satırda kullanan bir [Ara değerli dize](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  Aşağıdaki biçimlendirmeleri eklemek *About.cshtml* görünümü. Vurgulanan biçimlendirme web sitesi bilgilerini görüntüler.
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-)]
    
    >[!NOTE]
    > Aşağıda gösterilen Razor biçimlendirme içinde:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    >Razor bilir `info` özniteliği bir sınıf, bir dize değil ve C# kod yazmak istiyorsanız. Herhangi bir dize olmayan etiketi yardımcı öznitelik olmadan yazılması gereken `@` karakter.
    
5.  Uygulamayı çalıştırın ve web sitesi bilgileri görmek için hakkında görünümüne gidin.

    >[!NOTE]
    >Aşağıdaki biçimlendirmede kapanış etiketi ile kullanın ve içeren satırı Kaldır `TagMode.StartTagAndEndTag` etiket Yardımcısı içinde:
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a>Koşul etiket Yardımcısı

Koşul etiket Yardımcısı true değeri geçirildiğinde çıkış işler.

1.  Aşağıdakileri ekleyin `ConditionTagHelper` sınıfının *TagHelpers* klasör.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  Değiştir *Views/Home/Index.cshtml* aşağıdaki biçimlendirme dosyasıyla:

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  Değiştir `Index` yönteminde `Home` aşağıdaki kodla denetleyicisi:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  Uygulamayı çalıştırın ve giriş sayfasına göz atın. Koşullu biçimlendirme `div` işlenen olmaz. Sorgu dizesi eklemek `?approved=true` URL (örneğin, `http://localhost:1235/Home/Index?approved=true`). `approved`TRUE olarak ve koşullu biçimlendirme görüntülenir.

>[!NOTE]
>Kullanım [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) işleci kalın etiket Yardımcısı'nı kullanarak yaptığınız gibi bir dize belirtme yerine hedef özniteliği belirtin:
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
>[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) işleci korumak kodu herhangi bir zamanda yeniden (biz adına değiştirmek isteyebileceğiniz `RedCondition`).

### <a name="avoid-tag-helper-conflicts"></a>Etiket Yardımcısı çakışmaları önleme

Bu bölümde, etiket Yardımcıları otomatik bağlama çifti yazma. İlk biçimlendirme içeren bir HTML yer işareti etiketi aynı URL'sini içeren (ve bu nedenle bağlantı URL'sini verir) HTTP ile başlayan URL yerini alır. İkinci aynı URL'sini WWW ile başlayan yapın.

Bu iki Yardımcıları yakından ilişkili olan ve gelecekte yeniden çünkü biz aynı dosyada tutarsınız.

1.  Aşağıdakileri ekleyin `AutoLinkerHttpTagHelper` sınıfının *TagHelpers* klasör.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    >`AutoLinkerHttpTagHelper` Sınıfı hedefler `p` öğeleri ve kullandığı [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) bağlantı oluşturmak için.

2.  Aşağıdaki biçimlendirmede sonuna ekleyin *Views/Home/Contact.cshtml* dosyası:

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  Uygulamayı çalıştırın ve etiket Yardımcısı bağlantı düzgün işler doğrulayın.

4.  Güncelleştirme `AutoLinker` eklemek için sınıfı `AutoLinkerWwwTagHelper` hangi www Metne Dönüştür Ayrıca özgün www metin içeren bir yer işareti etiketi. Güncelleştirilmiş kod aşağıda vurgulanmıştır:

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  Uygulamayı çalıştırın. Www metin bağlantı olarak işlenir, ancak HTTP metin değil dikkat edin. Her iki sınıflarda bir kesme noktası yerleştirirseniz HTTP etiket Yardımcısı sınıfı ilk çalıştığını görebilirsiniz. , Etiket Yardımcısı çıkış önbelleğe alınır ve WWW etiket Yardımcısı çalıştırdığınızda, önbelleğe alınan çıktısı HTTP etiket Yardımcısı, üzerine yazar sorunudur. Etiket Yardımcıları Çalıştır sırasını denetlemek nasıl daha sonra öğreticide göreceğiz. Size kodu aşağıdakilerle düzeltme:

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    >Aşağıdaki kod ile hedef içeriği aldığınız otomatik bağlama etiket Yardımcıları ilk sürümü:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    >Diğer bir deyişle, çağrı `GetChildContentAsync` kullanarak `TagHelperOutput` içine geçirilen `ProcessAsync` yöntemi. Belirtildiği gibi daha önce çıktıyı önbelleğe olduğundan, son etiket WINS çalışmasına yardımcı. Aşağıdaki kod ile bu sorun düzeltilmiştir:
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    >Yukarıdaki kod içeriği değiştirilmiş ve varsa, çıktı arabelleğinden içeriği alır bakar.

6.  Uygulamayı çalıştırın ve iki bağlantı beklendiği gibi çalıştığını doğrulayın. Bizim otomatik bağlayıcı etiket Yardımcısı doğru ve eksiksiz görünebilir, ancak zarif bir sorun vardır. WWW etiket Yardımcısı ilk çalıştırıyorsa, www bağlantılar doğru olmayacaktır. Kodu ekleyerek güncelleştirme `Order` etiket çalışan sırasını denetlemek için aşırı yükleme. `Order` Özelliği aynı öğeye hedefleme diğer etiket Yardımcıları göre yürütme sırasını belirler. Varsayılan sıra değeri sıfırdır ve düşük değerler örnekleriyle önce yürütülür.

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    Yukarıdaki kod HTTP etiket Yardımcısı WWW etiket Yardımcısı önce çalışır garanti. Değişiklik `Order` için `MaxValue` ve WWW etiketi için oluşturulan biçimlendirme yanlış olduğundan emin olun.

## <a name="inspect-and-retrieve-child-content"></a>İnceleyebilir ve alt içeriğini alır

Etiket Yardımcıları içerik almak için çeşitli özellikler sağlar.

-  Sonucu `GetChildContentAsync` için eklenen `output.Content`.
-  Sonucu inceleyebilirsiniz `GetChildContentAsync` ile `GetContent`.
-  Değiştirirseniz `output.Content`, TagHelper gövde olmaz yürütülen veya çağırmanız sürece çizilir `GetChildContentAsync` otomatik bağlayıcı örneğimizde olduğu gibi:

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  Birden çok çağrılar `GetChildContentAsync` aynı değere döndürür ve yeniden yürütmez `TagHelper` önbelleğe alınmış sonucu kullanmayacak şekilde belirten bir yanlış parametre geçirmezseniz gövde.
