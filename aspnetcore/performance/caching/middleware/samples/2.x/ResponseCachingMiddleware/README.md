# <a name="aspnet-core-response-caching-sample"></a>ASP.NET Core yanıt önbelleğe alma örneği

Bu örnek ASP.NET Core kullanımını [yanıt önbelleğe alma ara yazılımı](https://docs.microsoft.com/aspnet/core/performance/caching/middleware).

Uygulama, dizin sayfasıyla yanıt dahil olmak üzere bir `Cache-Control` önbelleğe alma davranışını yapılandırmak için üst bilgi. Uygulama ayrıca ayarlar `Vary` yanıt yalnızca şu durumlarda hizmet önbelleğini yapılandırmak için üst bilgi `Accept-Encoding` sonraki istekleri üstbilgisinin özgün istekteki eşleşen.

Örnek çalışırken, dizin sayfası depolandığında ve 10 saniye boyunca önbelleğe önbelleğinden sunulur.

Önbelleğe alma davranışını test etmek için:

* Bir tarayıcı, önbelleğe alma davranışını test etmek için kullanmayın. Tarayıcılar genellikle bir önbellek denetim üstbilgisini ara yazılım, önbelleğe alınmış bir sayfaya sunmasını önlemek yeniden ekleyin. For example, bir `Cache-Control` üstbilgi değerini `max-age=0`) tarayıcı tarafından eklenmiş olabilir.
* İstek üstbilgilerini açıkça ayarı izin veren bir geliştirici aracı kullanma gibi <a href="https://www.telerik.com/fiddler">Fiddler</a> veya <a href="https://www.getpostman.com/">Postman</a>.
