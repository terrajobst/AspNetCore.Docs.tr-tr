---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: ASP.NET Web uygulamasında kullanıcı girdisi doğrulama sayfaları (Razor) siteleri | Microsoft Docs
author: tfitzmac
description: Bu makalede kullanıcılardan alma bilgileri doğrulamak nasıl ele &mdash; diğer bir deyişle, geçerli kullanıcıların girmesini emin olmak için bir AS bilgi HTML formları...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 34f703e6db70ac79c22f4a50d4cfd4e2326b4c74
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30899184"
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>ASP.NET Web sayfaları (Razor) sitelerdeki kullanıcı girişini doğrulama
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede kullanıcılardan alma bilgileri doğrulamak nasıl ele &mdash; diğer bir deyişle, geçerli kullanıcıların girmesini emin olmak için bir ASP.NET Web sayfaları (Razor) sitede HTML bilgileri oluşturur.
> 
> Öğrenecekleriniz:
> 
> - Bir kullanıcı girişi denetlemek nasıl tanımladığınız doğrulama ölçütleriyle eşleşiyor.
> - Tüm doğrulama testlerini geçtiğini belirlemek nasıl.
> - Doğrulama hataları görüntülemek nasıl (ve bunları biçimlendirme).
> - Doğrudan kullanıcılardan gelmeyen veri doğrulama yapma.
> 
> Programlama Kavramları makalesinde sunulan ASP.NET şunlardır:
> 
> - `Validation` Yardımcısı.
> - `Html.ValidationSummary` Ve `Html.ValidationMessage` yöntemleri.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ile de çalışır.


Bu makalede aşağıdaki bölümleri içerir:

- [Kullanıcı girdisi doğrulama genel bakış](#Overview_of_User_Input_Validation)
- [Kullanıcı girişini doğrulama](#Validating_User_Input)
- [İstemci tarafı doğrulama ekleme](#Adding_Client-Side_Validation)
- [Doğrulama hataları biçimlendirme](#Formatting_Validation_Errors)
- [Doğrudan kullanıcılardan olmadıktan verileri doğrulama](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Kullanıcı girdisi doğrulama genel bakış

Bir sayfa bilgilerini girmesini ister varsa — örneğin, bir forma — girmeleri değerlerinin geçerli olduğundan emin olmak önemlidir. Örneğin, kritik bilgiler eksik bir formu işlemek istemiyorsanız.

Kullanıcılar bir HTML formuna değerler girdiğinde girmeleri değerleri dizelerdir. Çoğu durumda, gereken bazı diğer veri türleri, tamsayı veya tarih gibi değerlerdir. Bu nedenle, ayrıca kullanıcıların giriş değerleri için uygun veri türleri doğru dönüştürülebilir emin olmak vardır.

Ayrıca bazı kısıtlamalar değerlerine sahip olabilir. Örneğin, kullanıcıların doğru bir tamsayı girin olsa bile değeri belirli bir aralığa sınırları emin olmak gerekebilir.

![CSS stil sınıflarını kullanmak doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Önemli** kullanıcı girişini doğrulama ayrıca güvenlik için önemli. Kullanıcıların formlarında girebileceği değerleri kısıtladığınızda birisi, sitenizin güvenliğini tehlikeye atabilir bir değer girebilirsiniz olasılığını azaltır.


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Kullanıcı girişini doğrulama

ASP.NET Web Pages 2'de, kullandığınız `Validator` Yardımcısı kullanıcı girişini test etmek için. Aşağıdakileri yapmak için temel yaklaşım şöyledir:

1. Hangi doğrulamak istediğiniz öğeleri (alanları) giriş belirler.

    Genellikle değerleri doğrulamak `<input>` öğeleri bir biçimde. Ancak, bu gibi kısıtlı bir öğeden gelen tüm giriş doğrulamak için hatta giriş iyi bir uygulamadır bir `<select>` listesi. Bu, kullanıcıların bir sayfadaki denetimlerin atlama yoksa ve bir form gönderme emin olmanıza yardımcı olur.
2. Yöntemlerini kullanarak her öğe girişi için sayfa kodunda, tek tek doğrulama denetimlerini ekleme `Validation` Yardımcısı.

    Gerekli alanlar için denetlemek için kullanın `Validation.RequireField(field, [error message])` (için tek bir alanı) veya `Validation.RequireFields(field1, field2, ...))` (için bir alanlar listesi). Doğrulama diğer türleri için kullanmak `Validation.Add(field, ValidationType)`. İçin `ValidationType`, bu seçenekleri kullanabilirsiniz:

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
3. Sayfa gönderildiğinde, doğrulama denetleyerek başarılı olup olmadığını denetleyin `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Herhangi bir doğrulama hatası varsa, normal sayfa işleme atlayın. Sayfa amacı bir veritabanını güncelleştirmek için ise, tüm doğrulama hataları sabit kadar Örneğin, bunu yok.
4. Doğrulama hatası varsa, hata iletileri sayfanın biçimlendirmede kullanarak görüntüleme `Html.ValidationSummary` veya `Html.ValidationMessage`, veya her ikisini de.

Aşağıdaki örnek, şu adımları gösterilmektedir bir sayfa gösterir.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Doğrulama nasıl çalıştığını görmek için bu sayfayı çalıştırın ve kasıtlı olarak hataları olun. Örneğin, işte sayfa göründüğünü girerseniz gibi indirmelere adı girmek unutursanız ve geçersiz bir tarih girin:

![İşlenen sayfanın doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>İstemci tarafı doğrulama ekleme

Kullanıcıların sayfayı gönderdikten sonra varsayılan olarak, kullanıcı girişi doğrulanır — diğer bir deyişle, doğrulama sunucu kodunda da yapılır. Bu yaklaşımın bir dezavantajı, kullanıcıların gönderme sayfası hata kadar bunlar sonra yapmış olduğunuz bilmiyorsanız ' dir. Bir form uzun veya karmaşık ise, yalnızca sayfa gönderildikten sonra hatalarını raporlama kullanıcıya bilinmelidir.

İstemci komut dosyası doğrulama gerçekleştirmek için destek ekleyebilirsiniz. Bu durumda, kullanıcılar tarayıcı içinde çalışırken doğrulama gerçekleştirilir. Örneğin, bir değeri bir tamsayı olmalıdır belirttiğiniz varsayalım. Bir kullanıcı bir tamsayı olmayan değerler girerse, kullanıcı girişi alanı terk hemen hata bildirilir. Kullanıcılar için uygun olduğunu anında geri bildirim alır. İstemci tabanlı doğrulama da birden çok hataları düzeltmek için formu göndermek için kullanıcının sahip sayısını azaltabilir.

> [!NOTE]
> İstemci tarafı doğrulama kullansanız bile, doğrulama her zaman aynı zamanda sunucu kodunda da yapılır. Kullanıcılar istemci tabanlı Doğrulamayı atla durumunda sunucu kodunda doğrulama gerçekleştirmeden güvenlik önlemi olur.


1. Şu JavaScript kitaplıklarını sayfasında kaydedin:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Mutlaka bilgisayara veya sunucuya sahip zorunda kalmamak için iki kitaplıkların bir içerik teslim ağı (CDN) yüklenebilir. Bununla birlikte, yerel bir kopyasını olmalıdır *jquery.validate.unobtrusive.js*. Zaten bir WebMatrix şablonu ile çalışıyorsanız değil (gibi **başlangıç sitesi** ) kitaplığı içeren, temel alan bir Web sayfaları site oluşturma **başlangıç sitesi**. Ardından kopyalama *.js* geçerli sitenize dosya.
2. Biçimlendirmede doğrulama her öğe için bir çağrı ekleyin `Validation.For(field)`. Bu yöntem, istemci tarafı doğrulama tarafından kullanılan öznitelikleri gösterir. (Gerçek JavaScript kod yayma yerine yöntemi gibi özniteliklere yayar `data-val-...`. Bu öznitelikler yapması için jQuery kullanan örtük istemci doğrulama destekler.)

Aşağıdaki sayfayı istemci doğrulama özelliklere daha önce gösterilen örnek gösterilmektedir.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

İstemci üzerinde çalışan tüm doğrulama olup olmadığını denetler. Özellikle, veri türü doğrulama (tamsayı, tarih vb.) istemci üzerinde çalıştırma. Aşağıdaki denetimleri istemci ve sunucu üzerinde çalışır:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

Bu örnekte, test için geçerli bir tarih istemci kodu çalışmaz. Ancak, test sunucu kodunda gerçekleştirilir.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Doğrulama hataları biçimlendirme

Aşağıdaki ayrılmış adları olan CSS sınıfı tanımlayarak doğrulama hataları nasıl görüntüleneceğini denetleyebilirsiniz:

- `field-validation-error`. Çıktısını tanımlar `Html.ValidationMessage` hata görüntülenirken yöntemi.
- `field-validation-valid`. Çıktısını tanımlar `Html.ValidationMessage` herhangi bir hata olduğunda yöntemi.
- `input-validation-error`. Tanımlar nasıl `<input>` öğeleri hata olduğunda işlenir. (Örneğin, arka plan rengini ayarlamak için bu sınıf kullanabilirsiniz bir &lt;giriş&gt; değeri geçersiz, farklı bir renk öğesine.) Bu CSS sınıfı (ASP.NET Web Pages 2)'deki istemci doğrulama sırasında yalnızca kullanılır.
- `input-validation-valid`. Görünümünü tanımlayan `<input>` herhangi bir hata olduğunda öğeleri.
- `validation-summary-errors`. Çıktısını tanımlar `Html.ValidationSummary` hataların listesini görüntülemeden yöntemi.
- `validation-summary-valid`. Çıktısını tanımlar `Html.ValidationSummary` herhangi bir hata olduğunda yöntemi.

Aşağıdaki `<style>` blok hata koşulları için kuralları gösterir.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Makalenin örnek sayfalarından bu stil bloğu dahil ederseniz, hata görünen aşağıdaki gibi görünmelidir:

![CSS stil sınıflarını kullanmak doğrulama hataları](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> ASP.NET Web Pages 2'de istemci doğrulaması kullanmıyorsanız için CSS sınıfları `<input>` öğeleri (`input-validation-error` ve `input-validation-valid` herhangi bir etkisi yoktur.


### <a name="static-and-dynamic-error-display"></a>Statik ve dinamik hata görüntüleme

CSS kuralları gibi çiftler halinde gelen `validation-summary-errors` ve `validation-summary-valid`. Bu çiftlerine, her iki koşul için kuralları tanımlamanıza olanak sağlar: bir hata koşulu ve "normal" (hata olmayan) koşulu. Hiçbir hata olsa bile biçimlendirme hata görüntülemek için her zaman işlenir anlamak önemlidir. Örneğin, bir sayfa varsa bir `Html.ValidationSummary` işaretleme yönteminde, sayfa kaynağı içerecek aşağıdaki biçimlendirme bile sayfa ilk kez istendiğinde:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

Diğer bir deyişle, `Html.ValidationSummary` yöntemi her zaman işler bir `<div>` öğesi ve hata listesinin boş olsa bile bir listesi. Benzer şekilde, `Html.ValidationMessage` yöntemi her zaman işler bir `<span>` öğesi hiçbir hata olsa bile bir tek tek alan hatası için bir yer tutucu olarak.

Bazı durumlarda, hata iletisi görüntüleyen sayfanın yeniden akışı neden olabilir ve öğeleri hareket etmek için sayfasında neden olabilir. İçinde sona CSS kuralları `-valid` , bu sorunu önlemek bir düzen tanımlamanıza olanak sağlar. Örneğin, tanımlayabilirsiniz `field-validation-error` ve `field-validation-valid` her ikisi de aynı boyutu düzeltildi. Böylece, alan görüntü alanını statiktir ve bir hata iletisi görüntülenirse sayfa akışı değiştirmez.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Doğrudan kullanıcılardan olmadıktan verileri doğrulama

Bazen doğrudan bir HTML form olmadıktan bilgileri doğrulamak sahip. Burada bir değer aşağıdaki örnekteki gibi bir sorgu dizesi geçirilen bir sayfa buna tipik bir örnektir:

`http://server/myapp/EditClassInformation?classid=1022`

Bu durumda, emin olmak istediğiniz sayfaya geçirilen değer (burada, 1022 değerini `classid`) geçerli değil. Doğrudan kullanamazsınız `Validation` bu doğrulamayı gerçekleştirmek için yardımcı. Ancak, diğer hata iletilerini görüntüleme yeteneği gibi doğrulama sistemi özelliklerini kullanabilirsiniz.

> [!NOTE] 
> 
> **Önemli** her zaman aldığınız değerleri doğrulaması *herhangi* form alanı değerleri, sorgu dizesi değerleri ve tanımlama bilgisi değerleri dahil olmak üzere kaynak. (Belki de kötü amaçlı olarak) bu değerleri değiştirmek üzere kişiler için kolay bir işlemdir. Bu nedenle, uygulamanızın korumak için bu değerleri işaretlemeniz gerekir.


Aşağıdaki örnek, nasıl bir sorgu dizesinde geçirilen bir değeri doğrulamak gösterir. Kodu test değeri boş olmayan ve bir tamsayı değil.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

İstek bir form gönderme olmadığında test gerçekleştirilir dikkat edin (`if(!IsPost)`). Bu test sayfası istenen ilk kez geçip geçmeyeceğini, ancak ne zaman isteği form gönderme değil.

Bu hatayı görüntülemek için hata doğrulama hataları listesine çağırarak ekleyebileceğiniz `Validation.AddFormError("message")`. Sayfa için bir çağrı içeriyorsa `Html.ValidationSummary` yöntemi, hata, yalnızca bir kullanıcı girişini doğrulama hatası gibi görüntülenir.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web Pages sitelerinde HTML formları ile çalışma](https://go.microsoft.com/fwlink/?LinkID=202892)
