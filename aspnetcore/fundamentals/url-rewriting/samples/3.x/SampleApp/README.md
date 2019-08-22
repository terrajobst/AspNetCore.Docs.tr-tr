# <a name="aspnet-core-url-rewriting-sample"></a>ASP.NET Core URL yeniden yazma örneği

Bu örnek ASP.NET Core URL yeniden yazma ara yazılımı kullanımını gösterir. Uygulama, URL yeniden yönlendirme ve URL yeniden yazma seçeneklerini gösterir.

Örneği çalıştırırken dosya olmayan yanıtlar, kurallardan biri bir istek URL 'sine uygulandığında yeniden yazan veya yeniden yönlendirilen URL 'YI döndürür. XML ve metin dosyası örnekleri için, statik dosya ara yazılımı, istek URL 'SI, ara yazılım tarafından yeniden alındıktan sonra dosyayı hizmet eder.

## <a name="examples-in-this-sample"></a>Bu örnekteki örnekler

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Başarı durum kodu: 302 (bulundu)
  - Örnek (yeniden yönlendirme): **/redirect-Rule/{capture_group}** ila **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Başarı durum kodu: 200 (TAMAM)
  - Örnek (yeniden yazma): **/rewrite-Rule/{capture_group_1}/{capture_group_2}** - **/yeniden yazıldı? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Başarı durum kodu: 302 (bulundu)
  - Örnek (yeniden yönlendirme): **/Apache-mod-Rules-Redirect/{capture_group}** to **/yönlendirileceği? id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Başarı durum kodu: 200 (TAMAM)
  - Örnek (yeniden yazma): **/iis-Rules-rewrite/{capture_group}** to **/yeniden yazıldı? id = {capture_group}**
* `Add(RedirectXmlFileRequests)`
  - Başarı durum kodu: 301 (kalıcı olarak taşındı)
  - Örnek (yeniden yönlendirme): **/File.xml** to **/xmlfiles/File.xml**
* `Add(RewriteTextFileRequests)`
  - Başarı durum kodu: 200 (TAMAM)
  - Örnek (yeniden yazma): **/some_dosya.txt** **/dosya.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Başarı durum kodu: 301 (kalıcı olarak taşındı)
  - Örnek (yeniden yönlendirme): **/Image.png 'den** **/PNG-images/Image.png**
  - Örnek (yeniden yönlendirme): **/Image.jpg** - **/jpg-images/Image.jpg**

## <a name="use-a-physicalfileprovider"></a>PhysicalFileProvider kullanma

`IFileProvider` Ayrıca, `PhysicalFileProvider` ve`AddIISUrlRewrite()`yöntemlerine geçirilecek bir oluşturarak elde edebilirsiniz: `AddApacheModRewrite()`

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Güvenli yeniden yönlendirme uzantıları

Bu örnek, `WebHostBuilder` güvenli yeniden yönlendirme yöntemlerini keşfetmeye yardımcı olmak üzere`https://localhost:5001`, `https://localhost`uygulamanın URL 'leri (,) ve bir test sertifikası (*testCert. pfx*) kullanması için yapılandırma içerir. Sunucuda 443 numaralı bağlantı noktası zaten varsa veya `https://localhost` kullanımda ise, örnek çalışmıyor &mdash;program.cs dosyası `CreateWebHostBuilder` yönteminde bağlantı noktası `ListenOptions` 443 için veya Kestrel 'in kullanabilmesi için sunucudaki bağlantı noktası 443 bağlantısını kaldırın. bağlantı noktası.

| Yöntem                           | Durum kodu |    Bağlantı Noktası    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
