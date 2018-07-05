---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ASP.NET Web uygulamasında kullanıcı girdisi doğrulama sayfaları (Razor) siteler | Microsoft Docs
author: tfitzmac
description: Bu makalede, kullanıcılardan alma bilgileri doğrulamak anlatılmaktadır &mdash; diğer bir deyişle, geçerli kullanıcılar girdiğinizden emin olmak için bir as HTML bilgilerinde forms...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 2a35e9895c5b711d5c6c5544987f99fe7e2e0085
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368233"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitesinde kullanıcı girişini doğrulama
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, kullanıcılardan alma bilgileri doğrulamak anlatılmaktadır &mdash; diğer bir deyişle, geçerli kullanıcılar girdiğinizden emin olmak için bir ASP.NET Web sayfaları (Razor) sitesinde HTML bilgileri oluşturur.
> 
> Öğrenecekleriniz:
> 
> - Bir kullanıcının girişinin olduğunu denetlemek nasıl tanımladığınız doğrulama ölçütlerini eşleşir.
> - Tüm doğrulama sınamalarını geçtiğini belirlemek nasıl.
> - Doğrulama hataları görüntülemek nasıl (ve bunları biçimine).
> - Doğrudan kullanıcılarından gelmeyen veri doğrulama yapma.
> 
> Programlama Kavramları makalesinde sunulan ASP.NET şunlardır:
> 
> - `Validation` Yardımcısı.
> - `Html.ValidationSummary` Ve `Html.ValidationMessage` yöntemleri.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


Bu makalede, aşağıdaki bölümleri içerir:

- [Kullanıcı girdisi doğrulama genel bakış](#Overview_of_User_Input_Validation)
- [Kullanıcı girişini doğrulama](#Validating_User_Input)
- [İstemci tarafı doğrulama ekleme](#Adding_Client-Side_Validation)
- [Doğrulama hataları biçimlendirme](#Formatting_Validation_Errors)
- [Doğrudan kullanıcılarından gelmeyen veri doğrulama](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Kullanıcı girdisi doğrulama genel bakış

Bir sayfa bilgileri girmelerini istemek, — örneğin, bir forma — girmeleri değerlerinin geçerli olduğundan emin olmak önemlidir. Örneğin, kritik bilgiler eksik bir form işleme istemezsiniz.

Kullanıcılar, bir HTML formuna değerleri girin, girmeleri değerleri dizelerdir. Çoğu durumda, gereksinim duyduğunuz diğer veri türlerinden bazılarıyla, tam sayılar veya tarihler gibi değerlerdir. Bu nedenle, kullanıcıların giriş değerleri doğru uygun veri türlerine dönüştürülebilir emin olmak ' iniz de.

Ayrıca bazı kısıtlamalar değerlerine sahip olabilir. Örneğin, kullanıcıların bir tamsayı doğru olarak girmiş olsa bile, değeri belirli bir aralığa denk geldiğinden emin emin olmak gerekebilir.

![CSS stil sınıflarını kullanan doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Önemli** kullanıcı girişini doğrulama, ayrıca güvenlik için önemlidir. Kullanıcıların formlarında girebileceği değerleri kısıtladığınızda, birisi, sitenizin güvenliğini tehlikeye atabilir bir değer girebilirsiniz olasılığını azaltmaya.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

ASP.NET Web sayfaları 2'de kullanabileceğiniz `Validator` Yardımcısı kullanıcı girişini test etmek için. Aşağıdakileri yapmak için basit yaklaşımdır bakın:

1. Doğrulamak istediğiniz öğeleri (alanlar), giriş belirleyin.

    Değerleri genellikle doğrulama `<input>` form öğeleri. Ancak, bu gibi kısıtlı bir öğeden gelen tüm girişleri doğrulayın, hatta giriş için iyi bir uygulamadır bir `<select>` listesi. Bu, kullanıcıların bir sayfadaki denetimleri atlamak yoktur ve form gönderme emin olmak için yardımcı olur.
2. Yöntemleri kullanılarak öğesi her giriş için sayfa kodunda, tek tek doğrulama denetimleri ekleme `Validation` Yardımcısı.

    Gerekli alanlar kontrol için kullanın `Validation.RequireField(field, [error message])` (için tek tek alan) veya `Validation.RequireFields(field1, field2, ...))` (için alanların listesi). Diğer doğrulama türleri için kullanın `Validation.Add(field, ValidationType)`. İçin `ValidationType`, bu seçenekleri kullanabilirsiniz:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Sayfa gönderildiğinde, doğrulama denetleyerek geçip geçmediğini denetleyin `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Herhangi bir doğrulama hatası varsa, normal sayfa işleme atlayın. Sayfanın amacı, bir veritabanını güncelleştirmek için ise, tüm doğrulama hatalarını sabit kadar Örneğin, bunu yok.
4. Doğrulama hataları varsa, hata iletilerini sayfasının biçimlendirmesinde kullanarak görüntüleme `Html.ValidationSummary` veya `Html.ValidationMessage`, veya her ikisini de.

Aşağıdaki örnek bu adımları gösteren bir sayfa görüntülenir.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Doğrulama nasıl çalıştığını görmek için bu sayfayı çalıştırın ve kasıtlı olarak hata yapar. Örneğin, işte girerseniz gibi bir kurs adına girmek unutursanız sayfanın nasıl göründüğüne bir, ve geçersiz bir tarih girin:

![İşlenen sayfanın doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>İstemci tarafı doğrulama ekleme

Varsayılan olarak, kullanıcılar sayfa gönderildikten sonra kullanıcı girişi doğrulanır — diğer bir deyişle, doğrulama sunucu kodunda da yapılır. Bu yaklaşımın bir dezavantajı, kullanıcıların sayfası gönderme hatayla kadar sonrasında yaptığınız bilmiyorum ' dir. Bir form uzun veya karmaşık ise, yalnızca sayfa gönderildikten sonra hata raporlama kullanıcıya kullanışsız olabilir.

İstemci komut dosyası doğrulaması yapmak için destek ekleyebilirsiniz. Bu durumda, kullanıcılar tarayıcı içinde çalışırken doğrulama gerçekleştirilir. Örneğin, bir değer bir tamsayı olması gerektiğini belirtin varsayalım. Bir kullanıcı bir tamsayı olmayan değeri girerse, kullanıcı girişi alanından ayrılsa hemen sonra bir hata bildirilir. Kullanıcılar için uygun olan anında geri bildirim alın. İstemci tabanlı doğrulama birden çok hataları düzeltmek için formunun kullanıcının sayısını da azaltabilirsiniz.

> [!NOTE]
> İstemci tarafı doğrulama kullansanız bile, doğrulama sunucu kodunda her zaman da gerçekleştirilir. Sunucu kodunda doğrulama gerçekleştirme, kullanıcıların istemci tabanlı Doğrulamayı atla durumunda bir güvenlik önlemi olur.


1. Aşağıdaki JavaScript kitaplıklarını sayfasında kaydedin:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Mutlaka bilgisayara veya sunucuya olması gerekmez, iki kitaplıkları bir içerik teslim ağı (CDN) de yüklenebilir. Ancak, yerel bir kopyasını olmalıdır *jquery.validate.unobtrusive.js*. Zaten bir WebMatrix şablonu ile çalışıyorsanız değil (gibi **başlangıç sitesi** ) kitaplığı içeren, temel alan bir Web sayfaları sitesinde oluşturma **başlangıç sitesi**. Ardından kopyalama *.js* geçerli siteye dosya.
2. Biçimlendirmede doğrulamakta her öğe için bir çağrı ekleyin `Validation.For(field)`. Bu yöntem, istemci tarafı doğrulama tarafından kullanılan öznitelikler yayar. (Gerçek JavaScript kodu yayan yerine öznitelikleri gibi yöntem yayar `data-val-...`. Bu öznitelikler işini yapması için jQuery kullanan örtük istemci doğrulama desteklemiyor.)

Şu sayfaya, istemci doğrulama özelliklerini daha önce gösterilen örneğe ekleme işlemi gösterilmektedir.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

İstemci üzerinde çalışan tüm doğrulama denetimleri. Özellikle, veri türü doğrulama (tamsayı, tarih vb.) istemcide çalışmaz. Aşağıdaki denetimleri, istemci ve sunucu üzerinde çalışır:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

Bu örnekte, test için geçerli bir tarih, istemci kodu çalışmaz. Ancak, test sunucu kodunda da yapılır.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Doğrulama hataları biçimlendirme

Şu ayrılmış adların olan CSS sınıfı tanımlayarak doğrulama hataları nasıl görüntüleneceğini denetleyebilirsiniz:

- `field-validation-error`. Çıkışı tanımlar `Html.ValidationMessage` hata görüntülenirken yöntemi.
- `field-validation-valid`. Çıkışı tanımlar `Html.ValidationMessage` herhangi bir hata olduğunda yöntemi.
- `input-validation-error`. Tanımlar nasıl `<input>` öğeleri, bir hata olduğunda işlenir. (Örneğin, arka plan rengini ayarlamak için bu sınıf kullanabilirsiniz bir &lt;giriş&gt; farklı bir renk değeri geçersizse öğesine.) Bu bir CSS sınıfı (ASP.NET Web Pages 2) istemci doğrulama sırasında yalnızca kullanılır.
- `input-validation-valid`. Görünümünü tanımlayan `<input>` herhangi bir hata olduğunda öğeleri.
- `validation-summary-errors`. Çıkışı tanımlar `Html.ValidationSummary` hataların listesini görüntülemeden yöntem.
- `validation-summary-valid`. Çıkışı tanımlar `Html.ValidationSummary` herhangi bir hata olduğunda yöntemi.

Aşağıdaki `<style>` bloğu hata koşulları için kuralları gösterir.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Makalesinde daha önce örnek sayfalarından bu stil bloğu dahil ederseniz, hata ekran aşağıdaki gibi görünür:

![CSS stil sınıflarını kullanan doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> ASP.NET Web sayfaları 2'de istemci doğrulama kullanmıyorsanız için CSS sınıfları `<input>` öğeleri (`input-validation-error` ve `input-validation-valid` hiçbir etkisi yoktur.


### <a name="static-and-dynamic-error-display"></a>Statik ve dinamik hata görüntüleme

CSS kurallarını gibi çiftler halinde gelen `validation-summary-errors` ve `validation-summary-valid`. Bu çiftler iki koşul için kuralları tanımlamanıza olanak sağlar: bir hata koşulu ve "normal" (hata olmayan) koşul. Biçimlendirme hatası görüntülemek için her zaman işlenir hatasız olsa bile anlamak önemlidir. Örneğin, bir sayfa varsa bir `Html.ValidationSummary` biçimlendirme yöntemi, sayfa kaynağı içeren aşağıdaki biçimlendirme bile sayfa ilk istendiğinde:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Diğer bir deyişle, `Html.ValidationSummary` yöntemi her zaman oluşturur bir `<div>` öğesi ve hata listesinin boş olsa bile bir listesi. Benzer şekilde, `Html.ValidationMessage` yöntemi her zaman oluşturur bir `<span>` öğesi hata olsa bile bir tek alan hatası için bir yer tutucu olarak.

Bazı durumlarda, bir hata iletisi görüntüleniyor boyutlandırıldığında için sayfayı neden olabilir ve öğeleri değiştirmemiz sayfasında neden olabilir. Biten CSS kurallarını `-valid` bu sorunu önlemeye yardımcı olabilecek bir düzen tanımlamanıza olanak sağlar. Örneğin, tanımlayabileceğiniz `field-validation-error` ve `field-validation-valid` her ikisi de aynı boyutu sabit. Böylece, alan için görüntüleme alanı statiktir ve bir hata iletisi görüntülenirse, sayfa akışı değişmez.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Doğrudan kullanıcılarından gelmeyen veri doğrulama

Bazen, doğrudan bir HTML formundan olmadıktan bilgileri doğrulamak gerekir. Tipik bir örnek, bir değer aşağıdaki örnekte olduğu gibi bir sorgu dizesinde geçirildiği bir sayfa olacaktır:

`http://server/myapp/EditClassInformation?classid=1022`

Bu durumda, emin olmak istediğiniz sayfaya geçirilen değerin (burada, 1022 değerini `classid`) geçerlidir. Doğrudan kullanamazsınız `Validation` bu doğrulamayı gerçekleştirmek için yardımcı. Ancak, doğrulama sisteminin doğrulama hata iletilerinin olanağı gibi diğer özellikleri kullanabilirsiniz.

> [!NOTE] 
> 
> **Önemli** her zaman aldığınız değerleri doğrulaması *herhangi* kaynak, form alanı değerleri, sorgu dizesi değerleri ve tanımlama bilgisi değerleri dahil. Bu değerleri (belki de kötü amaçlı olarak) değiştirmek üzere kişiler için kolaydır. Bu nedenle uygulamanızı korumak için bu değerleri işaretlemeniz gerekir.


Aşağıdaki örnek, bir sorgu dizesinde geçirilen bir değeri nasıl doğrulamak gösterir. Kod, değer boş değil ve bir tamsayı olduğunu sınar.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

İstek bir form gönderimi olmadığında test gerçekleştirilir dikkat edin (`if(!IsPost)`). Bu test sayfası istenen ilk kez geçip geçmeyeceğini, ancak zaman istek form gönderme değil.

Bu hatayı görüntülemek için hata doğrulama hataları listesine çağırarak ekleyebileceğiniz `Validation.AddFormError("message")`. Sayfa için bir çağrı içeriyorsa `Html.ValidationSummary` yöntemi, hata, yalnızca bir kullanıcı girişini doğrulama hatası gibi görüntülenir.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları sitelerinde HTML formları ile çalışma](https://go.microsoft.com/fwlink/?LinkID=202892)
