# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a>ASP.NET Core URL yeniden yazma örnek (ASP.NET 1.x çekirdek)

Bu örnek ASP.NET Core kullanımını göstermektedir 1.x URL yeniden yazma işlemi ara yazılım. Uygulama URL yeniden yönlendirme ve URL yeniden yazma seçenekleri gösterir. ASP.NET Core 2.x örnek için bkz: [ASP.NET Core URL yeniden yazma işlemi örnek (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).

Örnek çalıştırırken, bir yanıt kurallardan biri için bir istek URL'sini uygulanan ne zaman yeniden veya yeniden yönlendirilmiş bir URL gösteren sunulacak.

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Başarı durumu kodu: 302 (bulundu)
  - Örnek (Yönlendirme): **/redirect-rule / {capture_group}** için **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Başarı durumu kodu: 200 (Tamam)
  - Örnek (yeniden): **/rewrite-rule / {capture_group_1} / {capture_group_2}** için **/ yeniden yazılmıştır? var1 {capture_group_1} = & var2 {capture_group_2} =**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Başarı durumu kodu: 302 (bulundu)
  - Örnek (Yönlendirme): **/apache-mod-rules-redirect / {capture_group}** için **/ yeniden yönlendirilen? kimliği = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Başarı durumu kodu: 200 (Tamam)
  - Örnek (yeniden): **/iis-rules-rewrite / {capture_group}** için **/ yeniden yazılmıştır? kimliği = {capture_group}**
* `Add(RedirectXMLRequests)`
  - Başarı durumu kodu: 301 (taşınmış kalıcı olarak)
  - Örnek (Yönlendirme): **/file.xml** için **/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - Başarı durumu kodu: 301 (taşınmış kalıcı olarak)
  - Örnek (Yönlendirme): **/image.png** için **/png-images/image.png**
  - Örnek (Yönlendirme): **/image.jpg** için **/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>Kullanarak bir`PhysicalFileProvider`
Edinebilirsiniz bir `IFileProvider` oluşturarak bir `PhysicalFileProvider` uygulamasına geçirmek için `AddApacheModRewrite()` ve `AddIISUrlRewrite()` yöntemleri:
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>Yeniden yönlendirme uzantıları güvenli
Bu örnek içeren `WebHostBuilder` URL'leri kullanmak üzere uygulamayı yapılandırma (**https://localhost:5001**, **https://localhost**) ve bir test sertifikası (**testCert.pfx**) yardımcı olmak için size bu keşfetme yöntemlerini yeniden yönlendir. Herhangi birinin için ekleyin `RewriteOptions()` içinde **haline** davranışlarını incelemek için.

Yöntem | Durum kodu | Bağlantı Noktası
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
