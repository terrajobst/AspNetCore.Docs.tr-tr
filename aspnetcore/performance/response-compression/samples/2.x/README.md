# <a name="response-compression-sample-application-aspnet-core-2x"></a>Yanıt sıkıştırma örnek uygulaması (ASP.NET Core 2.x)

Bu örnek kullanımını ASP.NET Core 2.x yanıt sıkıştırma ara yazılımı HTTP yanıtları sıkıştırma. Örnek Gzip Brotli ve metin ve resim yanıtları için özel sıkıştırmasını sağlayıcıları gösterir ve sıkıştırma için MIME türü ekleme işlemi gösterilmektedir. ASP.NET Core 1.x örnek için bkz: [yanıt sıkıştırma örnek uygulaması (ASP.NET Core 1.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -~ 979 bayt sıkıştıran Lorem Ipsum metin dosyası yanıtında 2,044 bayt.
    * **/testfile1kb.txt** -yaklaşık 36 bayt sıkıştırır metin dosyası yanıt 1.033 bayt.
    * **/ trickle** -1 saniyelik aralıklarla tek karakter olarak verilen yanıt.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -~ 927 bayt sıkıştıran Lorem Ipsum metin dosyası yanıtında 2,044 bayt.
    * **/testfile1kb.txt** -yaklaşık 47 bayt sıkıştırır metin dosyası yanıt 1.033 bayt.
    * **/ trickle** -1 saniyelik aralıklarla tek karakter olarak verilen yanıt.
  * `image/svg+xml`
    * **/Banner.SVG** -~ 4,459 bayt sıkıştıran bir ölçeklenebilir vektör grafiği (SVG) görüntü yanıt 9,707 bayt.
* `CustomCompressionProvider`<br>Ara yazılım ile kullanmak için özel sıkıştırma sağlayıcıyı uygulama işlemi gösterilmektedir.

İstek zaman içerir `Accept-Encoding` ara yazılım başlığı ve yanıt sıkıştırma başarılı olur, otomatik olarak ekler bir `Vary: Accept-Encoding` yanıt üst bilgisi. `Vary` Üstbilgi bildirir alternatif değerlerine dayalı yanıt birden çok kopyasını korumak için önbellekleri `Accept-Encoding`, böylece hem bir sıkıştırılmış (Gzip veya Brotli) ve sıkıştırılmamış, önbelleklerinde depolanan, aşağıdakilerden birini yapabilirsiniz sistemlerine kabul etmek için sıkıştırılmış ve sıkıştırılmamış yanıt.

## <a name="use-the-sample"></a>Kullanım örneği

1. Bir istek kullanarak olun [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) olmadan uygulamasına bir `Accept-Encoding` üstbilgi ve Not yanıt yükünde yanıt boyutu ve yanıt üstbilgileri.
1. Ekleme bir `Accept-Encoding: br` veya `Accept-Encoding: gzip` üstbilgisi ve sıkıştırılmış yanıt boyutu ve yanıt üst bilgilerini not edin. Yanıt boyutu bırakır ve `Content-Encoding` yanıt üst bilgisi belirten ya da Gzip ile bu sıkıştırma ara yazılımı tarafından eklenir veya Brotli oluştu. Yanıt gövdesi için Lorem Ipsum baktığınızda veya **testfile1kb.txt** yanıt, gördüğünüz sıkıştırılmış ve okunamaz metindir.
1. Ekleme bir `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üst bilgilerini not edin. `CustomCompressionProvider` Gerçekten yanıt sıkıştırma olmayan boş bir uygulama olduğu halde için özel sıkıştırma stream sarmalayıcı oluşturabilirsiniz `CreateStream()` yöntemi.
