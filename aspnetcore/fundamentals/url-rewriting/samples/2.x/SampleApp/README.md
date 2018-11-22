# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>ASP.NET Core URL yeniden yazma örnek (ASP.NET Core 2.x)

Bu örnek bir ASP.NET Core kullanımını 2.x URL yeniden yazma ara yazılımı. Uygulama URL yeniden yönlendirme ve URL yeniden yazma seçenekleri gösterir.

Kurallardan biri için bir istek URL'si uygulandığında örnek çalışırken, dosya olmayan yanıtları yeniden ya da yeniden yönlendirilen URL'sini döndürür. İstek URL'si, ara yazılım tarafından yazılan sonra XML ve metin dosyasının örnekleri için statik dosya ara yazılımlarını dosya işlevi görür.

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Başarılı durum kodu: 302 (bulunamadı)
  - Örnek (yeniden yönlendirme): **/redirect-rule / {capture_group}** için **/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Başarılı durum kodu: 200 (Tamam)
  - Örnek (yeniden): **/rewrite-rule / {capture_group_1} / {capture_group_2}** için **/ yeniden? var1 {capture_group_1} = & var2 {capture_group_2} =**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Başarılı durum kodu: 302 (bulunamadı)
  - Örnek (yeniden yönlendirme): **/apache-mod-rules-redirect / {capture_group}** için **/ yeniden yönlendirilen? kimliği = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Başarılı durum kodu: 200 (Tamam)
  - Örnek (yeniden): **/iis-rules-rewrite / {capture_group}** için **/ yeniden? kimliği = {capture_group}**
* `Add(RedirectXmlFileRequests)`
  - Başarılı durum kodu: 301 (kalıcı taşındı)
  - Örnek (yeniden yönlendirme): **/file.xml** için **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - Başarılı durum kodu: 200 (Tamam)
  - Örnek (yeniden): **/some_file.txt** için **/file.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Başarılı durum kodu: 301 (kalıcı taşındı)
  - Örnek (yeniden yönlendirme): **/image.png** için **/png-images/image.png**
  - Örnek (yeniden yönlendirme): **/image.jpg** için **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>Bir PhysicalFileProvider kullanın

De edinebilirsiniz bir `IFileProvider` oluşturarak bir `PhysicalFileProvider` uygulamasına geçirilecek `AddApacheModRewrite()` ve `AddIISUrlRewrite()` yöntemleri:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Güvenli bir yeniden yönlendirme uzantıları

Bu örnek içeren `WebHostBuilder` uygulama URL'lerini kullanacak şekilde yapılandırma (`https://localhost:5001`, `https://localhost`) ve bir test sertifikası (*testCert.pfx*) güvenli bir yeniden yönlendirme yöntemleri keşfetme yardımcı olmak için. Sunucu bağlantı noktası 443 atanan sahipse veya kullanımda, `https://localhost` örnek işe yaramazsa&mdash;Kaldır `ListenOptions` için bağlantı noktası 443 `CreateWebHostBuilder` yöntemi *Program.cs* dosya veya bağlantı noktası 443 üzerinde bağlamayı Kaldır Sunucu bağlantı noktası, Kestrel'in kullanabilirsiniz.

| Yöntem                           | Durum kodu |    Bağlantı Noktası    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
