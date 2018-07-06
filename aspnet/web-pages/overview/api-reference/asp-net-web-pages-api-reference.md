---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: ASP.NET Web sayfaları (Razor) API hızlı başvurusu | Microsoft Docs
author: tfitzmac
description: Bu sayfa, en sık kullanılan nesnelerin, özellikleri ve yöntemleri için Razor sözdizimi olan ASP.NET Web sayfaları programlama örnekleri kısa bir listeyle içerir.
ms.author: aspnetcontent
ms.date: 02/10/2014
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5fb5b8d29fce25fb36704a58b673f5f3bd582873
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37836774"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>ASP.NET Web sayfaları (Razor) API hızlı başvurusu
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu sayfa, en sık kullanılan nesnelerin, özellikleri ve yöntemleri için Razor sözdizimi olan ASP.NET Web sayfaları programlama örnekleri kısa bir listeyle içerir.
> 
> ASP.NET Web sayfaları'nda sürüm 2 "(v2)" ile işaretlenmiş açıklamaları sunulur.
> 
> API başvuru belgeleri için bkz. [ASP.NET Web sayfaları başvuru belgeleri](https://go.microsoft.com/fwlink/?LinkId=208659) MSDN'de.
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, (v2 işaretlenmiş özellikleri dışında) ASP.NET Web sayfaları 1.0 ve ASP.NET Web Pages 2 ile de çalışır.


Bu sayfa aşağıdaki yönelik başvuru bilgileri içerir:

- [Sınıflar](#Classes)
- [Veri](#Data)
- [Yardımcıları](#Helpers)
- [Doğrulama](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Sınıflar

### `AppState[key], AppState[index],App`

Uygulamadaki tüm sayfalar tarafından paylaşılabilen veriler içerir. Dinamik kullanabileceğiniz `App` özelliği aşağıdaki örnekteki gibi aynı verilere erişmek için:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Bir dize değerini Boolean (true/false) değerine dönüştürür. Yanlış değerini döndürür veya belirtilen değer dizesi true/false temsil etmiyorsa.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Bir dize değerini tarih/saat değerine dönüştürür. Döndürür `DateTime.MinValue` veya belirtilen değer, dizeyi tarih/saat temsil etmiyor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Bir dize değeri bir ondalık değere dönüştürür. 0,0 döndürür veya belirtilen değer, dize ondalık bir değeri temsil etmiyor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Bir dize değeri kayana dönüştürür. 0,0 döndürür veya belirtilen değer, dize ondalık bir değeri temsil etmiyor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Bir dize değeri bir tamsayıya dönüştürür. 0 döndürür veya belirtilen değer, dize bir tamsayı temsil etmiyor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

İsteğe bağlı ek yol bölümleri içeren bir yerel dosya yolundan tarayıcı ile uyumlu bir URL oluşturur.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

İşler *değer* HTML biçimlendirmesi olarak HTML olarak kodlanan işleme yerine olarak.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Değer bir dizeden belirtilen türe dönüştürülebilir ise true döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Nesne veya değişkenin değeri yoksa true döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Bir POST isteğiyse true döndürür. (İlk istekler genellikle bir GET içindir.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Bu sayfaya uygulamak için bir düzen sayfasının yolunu belirtir.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Geçerli istekte sayfası, Düzen sayfaları ve kısmi sayfalar arasında paylaşılan veriler içerir. Dinamik kullanabileceğiniz `Page` özelliği aşağıdaki örnekteki gibi aynı verilere erişmek için:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Düzen sayfaları) Adlandırılmış bir bölümde yer almayan bir içerik sayfası içeriğini oluşturur.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Belirtilen yol ve isteğe bağlı ek verileri kullanarak bir içerik sayfasını işler. Ek parametreler değerlerini alabilirsiniz `PageData` konumu (örnek: 1) veya anahtar (örnek 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Düzen sayfaları) Bir ada sahip bir içerik bölümü oluşturur. Ayarlama *gerekli* bir bölümün isteğe bağlı yapmak için false.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Alır veya bir HTTP tanımlama bilgisi değerini ayarlar.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Geçerli istekte yüklenen dosyaları alır.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Ndaki bir forma (dize olarak) gönderilen verileri alır. `Request[key]` her ikisi de denetler `Request.Form` ve `Request.QueryString` koleksiyonları.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

URL sorgu dizesi belirtildi verilerini alır. `Request[key]` her ikisi de denetler `Request.Form` ve `Request.QueryString` koleksiyonları.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Seçmeli olarak devre dışı bırakır, bir form öğesi, sorgu dizesi değeri, tanımlama bilgisi veya üst bilgi değeri için doğrulama isteyin. İstek doğrulamanın, varsayılan olarak etkindir ve kullanıcıların biçimlendirme veya diğer potansiyel olarak tehlikeli olabilecek içeriğe nakil engeller.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Bir HTTP sunucusu üst yanıta ekler.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Sayfa çıktısının belirli bir süre boyunca önbelleğe alır. İsteğe bağlı olarak *kayan* zaman aşımı her sayfa erişimi sıfırlama ve *varyByParams* sayfanın her sayfa isteğinde farklı bir sorgu dizesi için farklı sürümlerini önbelleğe almak için.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Tarayıcı isteğini bir konuma yönlendirir.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Tarayıcıya gönderilen HTTP durum kodunu ayarlar.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

İçeriğini Yazar *veri* yanıta isteğe bağlı bir MIME türü.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Bir dosyanın içeriğini yanıta yazar.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Düzen sayfaları) Bir ada sahip bir içerik bölümü tanımlar.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

HTML ile kodlanmış bir dizenin kodunu çözer.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

HTML biçimlendirmede işleme için bir dize kodlar.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Belirtilen sanal yol için sunucu fiziksel yolunu döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Bir URL'den metnin kodunu çözer.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

URL'de koymak için metin kodlar.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Alır veya kullanıcının tarayıcıyı kapatmasına kadar var olan bir değer ayarlar.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Bir nesnenin değerinin dize gösterimini görüntüler.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Ek veri URL'den alır (örneğin, */sayfa/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Belirtilen kullanıcının parolasını değiştirir.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Hesap onayı belirtecini kullanarak bir hesap onaylar.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Belirtilen kullanıcı adı ve parola ile yeni bir kullanıcı hesabı oluşturur. Bir onay belirteci istemek için true geçirin *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Şu anda oturum açmış kullanıcı için tamsayı tanımlayıcısını alır.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Şu anda oturum açma kullanıcı adını alır.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

E-posta içinde kullanıcıya gönderilebilir ve böylece kullanıcı, parola sıfırlama bir parola sıfırlama simgesi üretir.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Kullanıcı kimliği, kullanıcı adını döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Geçerli kullanıcının oturum açtıysa true döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Kullanıcı (örneğin, bir onay e-posta) onaylanıp true değerini döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Belirtilen kullanıcı adı geçerli kullanıcı adının eşleşmesi durumunda true döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Kullanıcı bir kimlik doğrulama belirteci tanımlama bilgisinde ayarlayarak günlüğe kaydeder.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Kullanıcı, kimlik doğrulama belirteci tanımlama kaldırarak out günlüğe kaydeder.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Kullanıcının kimliği doğrulanmazsa HTTP durumunu 401 (yetkisiz) olarak ayarlar.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Geçerli kullanıcı belirtilen rollerden birinin üyesi değilse HTTP durumunu 401 (yetkisiz) olarak ayarlar.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Geçerli kullanıcı tarafından belirtilen kullanıcı değilse *kullanıcıadı*, HTTP durumunu 401 (yetkisiz) olarak ayarlar.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Parola sıfırlama simgesi geçerliyse, yeni parola kullanıcının parolasını değiştirir.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Veri

### `Database.Execute(SQLstatement [,parameters]`

Yürütür *sqldeyimi* (isteğe bağlı parametrelerle) ekleme, silme veya güncelleştirme gibi ve etkilenen kayıtların sayısını döndürür.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Kimlik sütunu, en son eklenen satırdan döndürür.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Belirtilen veritabanı dosyasını ya da bir adlandırılmış bağlantı dizesini kullanarak belirtilen veritabanı açılır *Web.config* dosya.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Bir veritabanı bağlantı dizesi kullanılarak açılır. (Bu ile karşılaştırır `Database.Open`, bağlantı dizesi adı kullanır.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Veritabanı kullanan sorgular *sqldeyimi* (isteğe bağlı parametre geçirme) ve sonuçları koleksiyon olarak döndürür.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Yürütür *sqldeyimi* (isteğe bağlı parametrelerle) ve tek bir kayıt döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Yürütür *sqldeyimi* (isteğe bağlı parametrelerle) ve tek bir değer döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Yardımcıları

### `Analytics.GetGoogleHtml(webPropertyId)`

Belirtilen kimlik için Google Analytics JavaScript kodu oluşturur

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Belirtilen proje StatCounter Analytics JavaScript kodunu oluşturur.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Belirtilen hesabın Yahoo Analytics JavaScript kodu oluşturur.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Bir arama Bing'e geçirir. Arama ve arama kutusu için bir başlık siteye belirtmek için ayarlayabileceğiniz `Bing.SiteUrl` ve `Bing.SiteTitle` özellikleri. Normalde bu özellikler kümesinde  *\_AppStart* sayfası.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Bir grafik başlatır.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Grafiğe bir açıklama ekler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Grafiğe bir dizi değer ekler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Belirtilen veriler için bir karma değer döndürür. Varsayılan algoritmadır `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Facebook kullanıcıların sayfalarına bağlantı sağlar.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Dosyaları karşıya yükleme için kullanıcı Arabirimi oluşturur.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Belirtilen Xbox oyuncu etiketi oluşturur.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Belirtilen e-posta adresi için Gravatar görüntü oluşturur.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Bir veri nesnesini JavaScript nesne gösterimi (JSON) biçiminde bir dizeye dönüştürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

JSON olarak kodlanmış giriş dizesi, üzerinden yineleme yapma veya bir veritabanına ekleme veri nesnesine dönüştürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Belirtilen başlık ve isteğe bağlı bir URL kullanarak sosyal ağ bağlantıları oluşturur.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Bir hata iletisi, bir form alanıyla ilişkilendirir. Kullanım `ModelState` bu üyeye erişmek için yardımcı.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Bir hata iletisi, bir form ile ilişkilendirir. Kullanım `ModelState` bu üyeye erişmek için yardımcı.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Doğrulama hataları varsa true değerini döndürür. Kullanım `ModelState` bu üyeye erişmek için yardımcı.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Özelliklerini ve değerlerini bir nesne ve tüm alt nesneleri oluşturur.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

ReCAPTCHA doğrulama testinden işler.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Ortak ve özel anahtarlar reCAPTCHA hizmeti için ayarlar. Normalde bu özellikler kümesinde  *\_AppStart* sayfası.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

ReCAPTCHA test sonucunu döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

ASP.NET Web sayfaları hakkında durum bilgileri işler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Belirtilen kullanıcı için bir Twitter akışı oluşturur.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Belirtilen arama metnini için bir Twitter akışı oluşturur.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

İsteğe bağlı bir genişlik ve yükseklik ile belirtilen dosyayı bir Flash video oynatıcı işler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

İsteğe bağlı bir genişlik ve yükseklik ile belirtilen dosya için bir Windows Media player yapar.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Belirtilen bir Silverlight player işler *.xap* dosyasıyla gerekli genişlik ve yükseklik.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Belirtilen nesneyi döndürür *anahtarı*, ya da nesne bulunamazsa null.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Tarafından belirtilen nesnede kaldırır *anahtar* önbellekten.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Koyar *değer* tarafından belirtilen adla önbelleğine *anahtar*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Yeni bir oluşturur `WebGrid` kullanarak bir sorgudan veri nesnesi.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

HTML tablosu halinde verileri görüntülemek için biçimlendirme oluşturur.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

İşler için bir çağrı `WebGrid` nesne.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Belirtilen yoldan görüntüyü yükler.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Belirtilen görüntü filigran olarak ekler.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Belirtilen metin görüntüye ekler.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Görüntüyü yatay veya dikey çevirir.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Görüntüyü bir dosyayı karşıya yükleme sırasında bir sayfa gönderildiğinde, görüntüyü yükler.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Yeniden boyutlandırır bir görüntüsü.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Resmi sağa veya sola döndürür.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Belirtilen yola resmi kaydeder.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

SMTP sunucusu için parolayı ayarlar. Bu özellik normalde ayarladığınız  *\_AppStart* sayfası.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Bir e-posta iletisi gönderir.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

SMTP sunucusu adını ayarlar. Bu özellik normalde ayarladığınız<em>\_AppStart</em> sayfası.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

SMTP sunucusu için kullanıcı adını ayarlar. Bu özellik normalde ayarlamanız gerekir  *\_AppStart* sayfası.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Doğrulama

### `Html.ValidationMessage(field)`

(v2) Belirtilen alan için bir doğrulama hatası iletisini işler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Tüm doğrulama hatalarının listesini görüntüler.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Belirtilen doğrulama türü için bir kullanıcı girişi öğesini kaydeder.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Doğrulama hatası iletilerini biçimlendirmek için istemci tarafı doğrulama CSS sınıfı öznitelikleri dinamik olarak işler. (Uygun istemci-komut dosyası Kitaplığı Başvurusu ve CSS sınıfları tanımlama gerektirir.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Kullanıcı giriş alanı için istemci tarafı doğrulamasını etkinleştirir. (Uygun istemci-komut dosyası kitaplığı başvurusu gerektirir.)

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Doğrulama için registred olan tüm kullanıcı girişi öğelerinin geçerli değerlerini içeriyorsa true döndürür.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Kullanıcıların kullanıcı girişi öğesi için bir değer belirtmeniz gerekir belirtir.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Kullanıcılar değerler her kullanıcı girişi öğelerinin sağlamanız gerektiğini belirtir. Bu yöntem bir özel hata iletisi belirtmenize izin vermez.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Kullanırken bir doğrulama testi belirtir `Validation.Add` yöntemi.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
