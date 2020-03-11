# <a name="aspnet-core-url-rewriting-sample"></a>ASP.NET Core URL yeniden yazma örneği

Bu örnek ASP.NET Core URL yeniden yazma ara yazılımı kullanımını gösterir. Uygulama, URL yeniden yönlendirme ve URL yeniden yazma seçeneklerini gösterir.

Örneği çalıştırırken dosya olmayan yanıtlar, kurallardan biri bir istek URL 'sine uygulandığında yeniden yazan veya yeniden yönlendirilen URL 'YI döndürür. XML ve metin dosyası örnekleri için, statik dosya ara yazılımı, istek URL 'SI, ara yazılım tarafından yeniden alındıktan sonra dosyayı hizmet eder.

## <a name="examples-in-this-sample"></a>Bu örnekteki örnekler

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - Başarı durum kodu: 302 (bulundu)
  - Örnek (yeniden yönlendirme): **/redirect-Rule/{capture_group}** - **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - Başarı durum kodu: 200 (Tamam)
  - Örnek (yeniden yazma): **/rewrite-Rule/{capture_group_1}/{capture_group_2}** için **/yeniden yazıldı? var1 = {capture_group_1} & var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - Başarı durum kodu: 302 (bulundu)
  - Örnek (Redirect): **/apache-mod-rules-redirect/{capture_group}** **/yeniden yönlendirme için? ıd = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - Başarı durum kodu: 200 (Tamam)
  - Örnek (yeniden yazma): **/iis-rules-rewrite/{capture_group}** için **/yeniden yazıldı? id = {capture_group}**
* `Add(RedirectXmlFileRequests)`
  - Başarı durum kodu: 301 (kalıcı olarak taşındı)
  - Örnek (yeniden yönlendirme): **/File.xml** to **/xmlfiles/File.xml**
* `Add(RewriteTextFileRequests)`
  - Başarı durum kodu: 200 (Tamam)
  - Örnek (yeniden yazma): **/some_file. txt** - **/dosya.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - Başarı durum kodu: 301 (kalıcı olarak taşındı)
  - Örnek (yeniden yönlendirme): **/Image.png 'den** **/PNG-images/Image.png**
  - Örnek (yeniden yönlendirme): **/Image.jpg** - **/jpg-images/Image.jpg**

## <a name="use-a-physicalfileprovider"></a>PhysicalFileProvider kullanma

Ayrıca, `AddApacheModRewrite()` ve `AddIISUrlRewrite()` yöntemlerine geçirilecek bir `PhysicalFileProvider` oluşturarak `IFileProvider` elde edebilirsiniz:

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>Güvenli yeniden yönlendirme uzantıları

Bu örnek, güvenli yeniden yönlendirme yöntemlerini keşfetmeye yardımcı olmak üzere, uygulamanın URL 'Leri (`https://localhost:5001`, `https://localhost`) ve bir test sertifikası (*testCert. pfx*) kullanması için `WebHostBuilder` yapılandırmayı içerir. Sunucuda 443 numaralı bağlantı noktası zaten varsa veya kullanımda ise, `https://localhost` örnek çalışmaz&mdash;*program.cs* dosyasının `CreateWebHostBuilder` yönteminde bağlantı noktası 443 için `ListenOptions` kaldırır ya da sunucu bağlantı noktasını kullanabilmesi için sunucudaki bağlantı noktası 443 bağlantısını kaldırın.

| Yöntem                           | Durum Kodu |    Bağlantı Noktası    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
