---
title: "Siteler arası komut dosyası önleme"
author: rick-anderson
description: "Bu belge, siteler arası komut dosyası (XSS) ve ASP.NET Core uygulama bu güvenlik açığı adresleme teknikleri tanıtır."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/cross-site-scripting
ms.openlocfilehash: af73a86aa6bcde084ecbe1a3fb5711c7da55871c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="preventing-cross-site-scripting"></a>Siteler arası komut dosyası önleme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Siteler arası komut dosyası (XSS) istemci tarafı komut dosyalarını (genellikle JavaScript) web sayfalarına yerleştirmek bir saldırgan sağlayan bir güvenlik açığı bulunmaktadır. Diğer kullanıcıların saldırganlar komut dosyaları çalıştırılır etkilenen sayfaları yüklediğinizde ve bu da saldırganın tanımlama bilgilerini ve oturum belirteçleri çalmak etkinleştirme DOM işleme aracılığıyla web sayfasının içeriği değiştirmek veya başka bir sayfaya tarayıcı yönlendirebilirsiniz. Uygulamanın kullanıcı girişini alır ve doğrulama, kodlama veya onu kaçış olmadan bir sayfasında çıkarır XSS Güvenlik Açıkları genellikle oluşur.

## <a name="protecting-your-application-against-xss"></a>Uygulamanızı XSS karşı koruma

En temel düzey XSS çalışır ekleme içine uygulamanızın kullanmak üzere kandırarak bir `<script>` etiketi, işlenen sayfasına veya ekleyerek bir `On*` bir öğenin içine olay. Geliştiriciler kendi uygulamasına XSS önlemek için aşağıdaki önleme adımları kullanmanız gerekir.

1. Hiçbir zaman aşağıdaki adımları izlemeden sürece güvenilmeyen verileri, HTML giriş yerleştirin. Denetlenmesi bir saldırgan, HTML form girişleri, sorgu dizeleri, HTTP üstbilgileri, bir saldırgan, uygulamanızın ihlal olamaz olsa bile, veritabanınızı ihlal mümkün olabilir gibi bir veritabanından kaynaklanan bile veri tarafından herhangi bir veri güvenilmeyen verilerdir.

2. Bir HTML öğesi içindeki güvenilmeyen verileri geçirmeden önce HTML kodlu olduğundan emin olun. HTML kodlamasını alır karakterler gibi &lt; ve bunları gibi güvenli bir forma değiştirir &amp;lt;

3. Bir HTML öznitelik güvenilmeyen veri geçirmeden önce kodlanmış HTML öznitelik olduğundan emin olun. HTML öznitelik kodlaması HTML kodlaması bir üst kümesidir ve ek karakterleri gibi kodlar "ve '.

4. JavaScript ile güvenilmeyen veri geçirmeden önce verileri içeriği çalışma zamanında almak bir HTML öğesi yerleştirin. Bu olası değil daha sonra verileri olun JavaScript kodlanır. JavaScript kodlama için JavaScript tehlikeli olabilecek karakterler alır ve bunları kendi onaltılık ile örneğin değiştirir &lt; olarak kodlanması `\u003C`.

5. Bir URL sorgu dizesine güvenilmeyen veri geçirmeden önce URL kodlanmış olduğundan emin olun.

## <a name="html-encoding-using-razor"></a>Razor kullanarak HTML kodlama

MVC'de otomatik olarak kullanılan Razor altyapısının tüm kodlar çıkış kaynaklanan değişkenlerinden, böylece önlemek için gerçekten çok çalışan sürece. HTML kodlama kurallarını, kullandığınızda özniteliğini kullanır  *@*  yönergesi. HTML olarak bunu kendiniz, HTML kodlaması veya HTML öznitelik kodlaması kullanmanız gerekir ile ilgili gerekmediği anlamına gelir HTML kodlaması bir üst öznitelik kodlaması kümesidir. Yalnızca @ bir HTML bağlamında doğrudan JavaScript ile güvenilmeyen giriş eklemek değil çalışırken kullandığınız emin olmanız gerekir. Etiket Yardımcıları giriş etiketi parametrelerinde kullanmak da kodlar.

Aşağıdaki Razor görünüm alın;

```none
@{
       var untrustedInput = "<\"123\">";
   }

   @untrustedInput
   ```

Bu görünüm içeriğini çıkarır *untrustedInput* değişkeni. Bu değişken XSS saldırılarında, yani kullanılan bazı karakterler içeren &lt;, "ve &gt;. Kaynak inceleniyor olarak kodlanmış işlenmiş çıkış şunları gösterir:

```html
&lt;&quot;123&quot;&gt;
   ```

>[!WARNING]
> ASP.NET Core MVC sağlayan bir `HtmlString` otomatik olarak çıktı kodlanmamış sınıfı. Bu XSS Güvenlik Açığı maruz bu asla güvenilmeyen giriş ile birlikte kullanılmalıdır.

## <a name="javascript-encoding-using-razor"></a>Razor kullanarak JavaScript kodlama

JavaScript görünümünüzde işlemek için bir değer eklemek istediğiniz durumlar olabilir. Bunu yapmanın iki yolu vardır. Basit değerler eklemek için güvenli bir veri özniteliği bir etiket değeri koyun ve, JavaScript almak için yoludur. Örneğin:

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

Bu aşağıdaki HTML oluşturur

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

Çalıştırıldığında, aşağıdaki oluşturmaz;

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

Bu tarayıcıda şu şekilde oluşturulur;

```html
<script>
       document.write("\u003C\u0022123\u0022\u003E");
   </script>
   ```

>[!WARNING]
> DOM öğeleri oluşturmak için JavaScript güvenilmeyen girişinde birleştirme değil. Kullanmanız gereken `createElement()` ve özellik değerlerini uygun şekilde gibi atayın `node.TextContent=`, veya kullanmak `element.SetAttribute()` / `element[attribute]=` Aksi takdirde, kendiniz DOM tabanlı XSS ortaya.

## <a name="accessing-encoders-in-code"></a>Kodda kodlayıcılar erişme

HTML, JavaScript ve URL kodlayıcılar kodunuzu iki yolla kullanılabilir, bunları aracılığıyla Ekle [bağımlılık ekleme](../fundamentals/dependency-injection.md#fundamentals-dependency-injection) veya içinde yer alan varsayılan Kodlayıcıları kullanabilirsiniz `System.Text.Encodings.Web` ad alanı. Herhangi bir karakter aralıkları uygulanan sonra varsayılan kodlayıcılar kullanırsanız, güvenli olarak kabul edilmesi için kazanmaz - varsayılan kodlayıcılar olası güvenli kodlama kuralları kullanın.

DI, Oluşturucular almalıdır aracılığıyla yapılandırılabilir kodlayıcılar kullanmak için bir *HtmlEncoder*, *JavaScriptEncoder* ve *UrlEncoder* uygun şekilde parametresi. Örneğin;

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

## <a name="encoding-url-parameters"></a>URL parametreleri kodlama

Güvenilmeyen giriş olarak bir değer kullanımı bir URL sorgu dizesi oluşturmak istiyorsanız `UrlEncoder` değeri kodlamak için. Örneğin,

```csharp
var example = "\"Quoted Value with spaces and &\"";
   var encodedValue = _urlEncoder.Encode(example);
   ```

EncodedValue kodlama sonra değişken içerecek `%22Quoted%20Value%20with%20spaces%20and%20%26%22`. Onaltılık değerlerine kodlanmış yüzde olacaktır, boşluk, tırnak işareti, noktalama ve diğer güvenli olmayan karakterler, örneğin bir boşluk karakteri % 20 olur.

>[!WARNING]
> Güvenilmeyen girdi bir URL yolu bir parçası olarak kullanmayın. Her zaman güvenilmeyen Giriş bir sorgu dizesi değerini geçirin.

<a name="security-cross-site-scripting-customization"></a>

## <a name="customizing-the-encoders"></a>Kodlayıcılar özelleştirme

Varsayılan olarak kodlayıcılar temel Latin Unicode aralığı için sınırlı güvenli bir listesini kullanın ve bu aralığın dışında tüm karakterleri Karakter kodu eşdeğerlerine olarak kodlayın. Dizelerinizi çıktısını almak için kodlayıcılar kullanacak şekilde bu davranış Razor TagHelper ve HtmlHelper işleme de etkiler.

Bu arkasındaki mantığı (önceki tarayıcı hatalar üzerindeki İngilizce olmayan karakterler işleme dayalı ayrıştırma yukarı dönüş) bilinmeyen veya gelecekteki tarayıcı hatalar karşı korumaktır. Web sitenizi Çince gibi Latin olmayan karakterleri kullanımına ağırlık yaparsa Kiril veya başkalarının istediğiniz davranış budur olmayabilir.

Unicode içinde aralıkları uygun başlatma sırasında uygulamanıza eklenecek Kodlayıcı güvenli listeler özelleştirebilirsiniz `ConfigureServices()`.

Örneğin, bir Razor HtmlHelper kullanabilecekleri varsayılan yapılandırması'nı kullanarak şu şekilde;

```html
<p>This link text is in Chinese: @Html.ActionLink("汉语/漢語", "Index")</p>
   ```

Kaynak web sayfası görüntülediğinizde, aşağıdaki gibi kodlanmış Çince metinle işlenip işlenmediğini görürsünüz;

```html
<p>This link text is in Chinese: <a href="/">&#x6C49;&#x8BED;/&#x6F22;&#x8A9E;</a></p>
   ```

Kabul edilen karakterler genişletmek için güvenli Kodlayıcı tarafından aşağıdaki satır içine ekler `ConfigureServices()` yönteminde `startup.cs`;

```csharp
services.AddSingleton<HtmlEncoder>(
     HtmlEncoder.Create(allowedRanges: new[] { UnicodeRanges.BasicLatin,
                                               UnicodeRanges.CjkUnifiedIdeographs }));
   ```

Bu örnek Unicode aralığı CjkUnifiedIdeographs dahil etmek için güvenli listesi widens. İşlenmiş çıkış şimdi olur

```html
<p>This link text is in Chinese: <a href="/">汉语/漢語</a></p>
   ```

Güvenli listesi aralıkları Unicode kod grafikleri, değil diller belirtilir. [Unicode standart](http://unicode.org/) listesini içeren [kod grafikleri](http://www.unicode.org/charts/index.html) , karakterler içeren grafik bulmak için kullanabilirsiniz. Html, JavaScript ve Url, her Kodlayıcı ayrı olarak yapılandırılması gerekir.

> [!NOTE]
> Güvenli listesi özelleştirilmesi yalnızca dı kaynaklanan kodlayıcılar etkiler. Bir kodlayıcı aracılığıyla doğrudan erişim `System.Text.Encodings.Web.*Encoder.Default` sonra varsayılan olarak, temel Latin yalnızca güvenli liste kullanılır.

## <a name="where-should-encoding-take-place"></a>Kodlama Al nereye?

Genel kodlama çıkış noktasında gerçekleşir ve kodlanmış değerler hiçbir zaman bir veritabanında saklanmalıdır uygulamadır kabul edildi. Çıktı noktasında kodlama bir sorgu dizesi değerini HTML gelen verilerin, örneğin, kullanımını değiştirmenize izin verir. Ayrıca, arama yapmadan önce değerleri kodlamak zorunda kalmadan, verilerinizi kolayca aramanıza olanak tanır ve herhangi bir değişiklik veya hata düzeltmeleri kodlayıcıya yapılan yararlanmak sağlar.

## <a name="validation-as-an-xss-prevention-technique"></a>Bir XSS önleme teknik olarak doğrulama

Doğrulama XSS saldırılarını sınırlama de yararlı bir aracı olabilir. Örneğin, yalnızca 0-9 karakter içeren basit bir sayısal dize XSS saldırısı tetiklemez. Doğrulama HTML uygulamasında kullanıcı girdisi - kabul etmek istediğiniz HTML giriş ayrıştırma imkansız olmasa zordur daha karmaşık hale gelir. MarkDown ve diğer metin biçimleri zengin girişi için daha güvenli bir seçenek olabilir. Hiçbir zaman tek başına doğrulama yararlanmalıdır. Her zaman güvenilmeyen giriş çıkış önce kodlama, ne olursa olsun, doğrulamanın.
