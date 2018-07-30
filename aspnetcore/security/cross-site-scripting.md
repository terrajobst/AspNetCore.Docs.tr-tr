---
title: Siteler arası betik kullanmayı (XSS) ASP.NET core'da engelle
author: rick-anderson
description: Siteler arası betik (XSS) ve ASP.NET Core uygulaması, bu güvenlik açığını ele alan teknikleri öğrenin.
ms.author: riande
ms.date: 10/14/2016
uid: security/cross-site-scripting
ms.openlocfilehash: 4784b1775d955f0ef00526e50b960fc873ea218d
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342217"
---
# <a name="prevent-cross-site-scripting-xss-in-aspnet-core"></a>Siteler arası betik kullanmayı (XSS) ASP.NET core'da engelle

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Siteler arası betik (XSS), bir saldırganın istemci tarafı komut dosyalarını (genellikle JavaScript) web sayfalarına yerleştirmek sağlayan bir güvenlik açığı var. Diğer kullanıcıların saldırganlar komut dosyaları çalıştırılır etkilenen sayfaları yüklediğinizde, tanımlama bilgileri ve oturum belirteçleri çalmaya saldırgan etkinleştirme DOM işlemesi aracılığıyla web sayfasının içeriğini değiştirebilir veya tarayıcıyı yeniden yönlendirmek için başka bir sayfa. Uygulamanın kullanıcı girişini alır ve bir sayfa doğrulanırken, kodlama veya, kaçış olmadan çıkarır XSS Güvenlik Açıkları genellikle ortaya çıkar.

## <a name="protecting-your-application-against-xss"></a>Uygulamanızı XSS karşı koruma

En temel bir düzey XSS çalışır içine ekleyerek uygulamanızı şekilde kandırma tarafından bir `<script>` etiketi ekleyerek veya işlenen sayfanız bir `On*` olay içine bir öğe. Geliştiriciler kendi uygulamasına XSS önlemek için aşağıdaki önleme adımları kullanmalısınız.

1. Hiçbir zaman aşağıdaki adımları izlemeden sürece güvenilir olmayan verileri, HTML giriş yerleştirin. Güvenilir olmayan verileri kontrol edilebilir bir saldırgan, HTML form girişleri, sorgu dizeleri, HTTP üstbilgileri, bir saldırganın uygulamanızı ihlal edemiyor olsanız bile, veritabanınızı güvenlik ihlali çözebileceğiniz gibi bir veritabanından kaynaklanan bile veri herhangi bir veridir.

2. HTML öğesi içinde güvenilir olmayan verileri geçirmeden önce kodlanmış HTML olduğundan emin olun. HTML kodlaması gereken karakter gibi &lt; ve bunları gibi güvenli bir forma değişiklikleri &amp;lt;

3. HTML özniteliğin güvenilir olmayan verileri geçirmeden önce kodlanmış HTML öznitelik olduğundan emin olun. HTML öznitelik kodlaması HTML kodlaması bir üst kümesidir ve ek karakterler gibi kodlar "ve '.

4. JavaScript ile güvenilir olmayan verileri geçirmeden önce veri içerikleri çalışma zamanında almak bir HTML öğesi koyun. Bu mümkün değilse daha sonra veriler JavaScript kodlanır. JavaScript kodlama için JavaScript tehlikeli karakterleri alır ve örneğin kendi onaltılık ile değiştirir &lt; olarak kodlanması `\u003C`.

5. URL sorgu dizesi içinde güvenilir olmayan verileri geçirmeden önce URL kodlanmış olduğundan emin olun.

## <a name="html-encoding-using-razor"></a>Razor kullanarak HTML kodlama

Razor altyapı MVC'de otomatik olarak kullanılan tüm kodlar gerçekten sabit, bunu önlemek için çalışmıyorsanız değişkenlerinden, çıkış kaynağı. HTML öznitelik her kullandığınızda kodlama kurallarını kullanan *@* yönergesi. HTML öznitelik kodlaması, kendiniz, HTML kodlaması veya HTML öznitelik kodlaması kullanmanız gerekir ile uğraşmak zorunda olmadığınız anlamına gelir HTML kodlaması bir üst kümesidir. Yalnızca @ HTML bağlamında, güvenilmeyen girişler doğrudan JavaScript eklemek değil çalışırken kullanmanızı emin olmanız gerekir. Etiket Yardımcıları de giriş etiketi parametrelerinde kullandığınız kodlar.

Aşağıdaki Razor görünüm yararlanın;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Bu görünüm içeriğini çıkarır *untrustedInput* değişkeni. Bu değişken XSS saldırılarında, yani kullanılan bazı karakterler içeren &lt;, "ve &gt;. Kaynak İnceleme olarak kodlanmış işlenmiş çıktı gösterir:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC sağlayan bir `HtmlString` sınıfını otomatik olarak çıktı kodlanmış değil. Bu, XSS bir güvenlik açığı harekete geçirecek şekilde bu hiçbir zaman güvenilmeyen giriş ile birlikte kullanılmalıdır.

## <a name="javascript-encoding-using-razor"></a>Razor kullanarak JavaScript kodlama

JavaScript görünümünüzde işlemek için bir değer eklemek istediğiniz zamanlar olabilir. Bunu yapmanın iki yolu vardır. Bir veri özniteliği bir etiketin değeri koyun ve, JavaScript dilinde almak için değerleri eklemek için en güvenli yolu var. Örneğin:

```none
@{
       var untrustedInput = "<\"123\">";
   }

   <div
       id="injectedData"
       data-untrustedinput="@untrustedInput" />

   <script>
     var injectedData = document.getElementById("injectedData");

     // All clients
     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     // HTML 5 clients only
     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Bu aşağıdaki HTML'yi oluşturur

```html
<div
     id="injectedData"
     data-untrustedinput="&lt;&quot;123&quot;&gt;" />

   <script>
     var injectedData = document.getElementById("injectedData");

     var clientSideUntrustedInputOldStyle =
         injectedData.getAttribute("data-untrustedinput");

     var clientSideUntrustedInputHtml5 =
         injectedData.dataset.untrustedinput;

     document.write(clientSideUntrustedInputOldStyle);
     document.write("<br />")
     document.write(clientSideUntrustedInputHtml5);
   </script>
   ```

Çalıştığında, aşağıdaki işlenir;

```none
<"123">
   <"123">
   ```

JavaScript Kodlayıcı doğrudan çağırabilir,

```none
@using System.Text.Encodings.Web;
   @inject JavaScriptEncoder encoder;

   @{
       var untrustedInput = "<\"123\">";
   }

   <script>
       document.write("@encoder.Encode(untrustedInput)");
   </script>
   ```

Bu tarayıcıda şu şekilde işlenir;

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> DOM öğeleri oluşturmak için JavaScript güvenilmeyen girişinde birleştirme yok. Kullanmanız gereken `createElement()` ve özellik değerlerini uygun şekilde aşağıdaki gibi atayabilirsiniz `node.TextContent=`, veya `element.SetAttribute()` / `element[attribute]=` Aksi takdirde, kendiniz için DOM tabanlı XSS kullanıma.

## <a name="accessing-encoders-in-code"></a>Kod kodlayıcılara erişme

Aracılığıyla ekleyebilir, HTML, JavaScript ve URL kodlayıcılarda kodunuzu iki şekilde kullanılabilir [bağımlılık ekleme](xref:fundamentals/dependency-injection) veya içerdiği varsayılan Kodlayıcıları kullanabilirsiniz `System.Text.Encodings.Web` ad alanı. İçin uygulanan tüm sonra varsayılan kodlayıcılarda kullanırsanız güvenli olarak kabul edilmesi için karakter aralıkları uygulanmayacak - varsayılan kodlayıcılarda olası güvenli kodlama kurallarını kullanın.

DI, oluşturucu kısa sürecektir aracılığıyla yapılandırılabilir kodlayıcılarda kullanmak için bir *HtmlEncoder*, *JavaScriptEncoder* ve *UrlEncoder* uygun şekilde parametresi. Örneğin;

```csharp
public class HomeController : Controller
   {
       HtmlEncoder _htmlEncoder;
       JavaScriptEncoder _javaScriptEncoder;
       UrlEncoder _urlEncoder;

       public HomeController(HtmlEncoder htmlEncoder,
                             JavaScriptEncoder javascriptEncoder,
                             UrlEncoder urlEncoder)
       {
           _htmlEncoder = htmlEncoder;
           _javaScriptEncoder = javascriptEncoder;
           _urlEncoder = urlEncoder;
       }
   }
   ```

## <a name="encoding-url-parameters"></a>URL parametrelerini kodlama

URL sorgu dizesi güvenilmeyen giriş olarak bir değer kullanımı ile derlemek istiyorsanız `UrlEncoder` değeri kodlamak için. Örneğin,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

EncodedValue kodladıktan sonra değişken içerecektir `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Alanları, tırnak işaretleri, noktalama işaretleri ve diğer güvenli olmayan karakterleri kendi onaltılı değerine kodlanmış yüzde olacaktır, örneğin bir boşluk karakteri % 20 olur.

>[!WARNING]
> Güvenilmeyen girişler, bir URL yolu bir parçası olarak kullanmayın. Her zaman güvenilmeyen Giriş bir sorgu dizesi değerini geçirin.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Kodlayıcıları özelleştirme

Varsayılan olarak kodlayıcılar temel Latin Unicode aralığın sınırlı güvenli bir listesini kullanın ve bu aralığın dışında tüm karakterleri Karakter kodu eşdeğerlerine olarak kodlayın. Dizelerinizi çıktısını almak için Kodlayıcıları kullanır gibi bu davranış, Razor TagHelper ve HtmlHelper işleme de etkiler.

Bunun ardındaki mantık (önceki tarayıcı hataları İngilizce olmayan karakterleri işleme dayalı ayrıştırma yukarı dönüş) bilinmeyen ya da gelecekte tarayıcı hataları karşı korunmasını sağlamaktır. Web sitenizi Çince gibi Latin olmayan karakterler ağır olarak kullanan yaparsa Kiril veya başkalarının istediğiniz davranış budur olmayabilir.

Kodlayıcı güvenli listeleri de aralık uygun uygulamanıza başlatma sırasında Unicode içerecek şekilde özelleştirebilirsiniz `ConfigureServices()`.

Örneğin, varsayılan yapılandırmayla bir Razor HtmlHelper kullanıyor olabileceğiniz gibi bunu;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Kaynak web sayfası görüntülediğinizde gibi kodlanan metnin Çince ile işlendikten görürsünüz;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Karakter olarak kabul genişletmek için Kodlayıcı tarafından güvenli, aşağıdaki satır içine ekler `ConfigureServices()` yönteminde `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

Bu örnekte, Unicode aralığı CjkUnifiedIdeographs dahil etmek için güvenli listeye widens. İşlenen çıkışı artık hale gelir

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Güvenli listeye aralıkları Unicode kod grafikleri, değil diller belirtilir. [Unicode standart](http://unicode.org/) listesine sahip [kod grafikleri](http://www.unicode.org/charts/index.html) , karakterler içeren grafik bulmak için kullanabilirsiniz. Her Kodlayıcı, Html, JavaScript ve Url, ayrı olarak yapılandırılması gerekir.

> [!NOTE]
> Güvenli listeye özelleştirmesini yalnızca DI kaynaklanan kodlayıcılar etkiler. Bir kodlayıcı aracılığıyla doğrudan erişirseniz `System.Text.Encodings.Web.*Encoder.Default` sonra varsayılan olarak, temel Latin yalnızca güvenli liste kullanılır.

## <a name="where-should-encoding-take-place"></a>Kodlama Al nereye yerleştirmeniz gerekir?

Genel Uygulama kodlama çıkış noktasında gerçekleşir ve kodlanmış değerler hiçbir zaman bir veritabanında depolanacak kabul edilir. Çıkış noktasında kodlama kullanımı verileri, örneğin, bir sorgu dizesi değeri için HTML değiştirmenize izin verir. Ayrıca, arama yapmadan önce değerleri kodlamak zorunda kalmadan verilerinizi kolayca aramanızı sağlar ve herhangi bir değişiklik veya hata düzeltmeleri kodlayıcıya yapılan avantajlarından yararlanmanıza olanak tanır.

## <a name="validation-as-an-xss-prevention-technique"></a>Bir XSS önleme teknik olarak doğrulama

Doğrulama XSS saldırılarını sınırlama yararlı bir aracı olabilir. Örneğin, yalnızca 0-9 karakterleri içeren bir sayısal dize XSS saldırının tetiklemez. Doğrulama uygulamasında kullanıcı girdisi - HTML kabul istemeniz durumunda HTML giriş ayrıştırma zor imkansız, daha karmaşık hale gelir. MarkDown ve diğer metin biçimleri zengin bir giriş için güvenli bir seçenek olabilir. Hiçbir zaman tek başına doğrulama yararlanmalıdır. Güvenilir olmayan girişlere karşı ne olursa olsun, gerçekleştirdiğiniz hangi doğrulama çıktı önce her zaman kodlayın.
