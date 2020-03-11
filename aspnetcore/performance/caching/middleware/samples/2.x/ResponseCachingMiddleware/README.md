# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core yanıtı önbelleğe alma örneği

Bu örnek ASP.NET Core [yanıt önbelleğe alma ara yazılımı](https://docs.microsoft.com/aspnet/core/performance/caching/middleware)kullanımını gösterir.

Uygulama, önbelleğe alma davranışını yapılandırmak için bir `Cache-Control` üst bilgisi de dahil olmak üzere dizin sayfasıyla yanıt verir. Uygulama, yalnızca sonraki isteklerin `Accept-Encoding` üstbilgisi özgün istekten eşleşen bir eşleşiyorsa yanıtı sunacak şekilde yapılandırmak için `Vary` üst bilgisini de ayarlar.

Örnek çalıştırılırken, Dizin sayfası depolanırken ve 10 saniyeye kadar önbelleğe alındığında önbellekten sunulur.

Önbelleğe alma davranışını test etmek için:

* Önbelleğe alma davranışını test etmek için bir tarayıcı kullanmayın. Tarayıcılar genellikle yeniden yükleme üzerinde, ara yazılımın önbelleğe alınmış bir sayfaya hizmet almasını önleyen bir önbellek denetim üst bilgisi ekler. Örneğin, `max-age=0`değeri olan bir `Cache-Control` üstbilgisi tarayıcı tarafından eklenebilir.
* <a href="https://www.telerik.com/fiddler">Fiddler</a> veya <a href="https://www.getpostman.com/">Postman</a>gibi istek üst bilgilerini açıkça ayarlamaya izin veren bir geliştirici aracı kullanın.
