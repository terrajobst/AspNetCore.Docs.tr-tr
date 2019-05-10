# <a name="response-compression-sample-application-aspnet-core-1x"></a>Yanıt sıkıştırma örnek uygulaması (ASP.NET Core 1.x)

Bu örnek kullanımını ASP.NET Core 1.x yanıt sıkıştırma ara yazılımı için HTTP yanıtları sıkıştırma. Örnek GZIP ve metin ve resim yanıtları için özel sıkıştırmasını sağlayıcıları gösterir ve sıkıştırma için MIME türü ekleme işlemi gösterilmektedir. ASP.NET Core 2.x örnek için bkz: [yanıt sıkıştırma örnek uygulaması (ASP.NET Core 2.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri

* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum metin dosyası yanıt 927 bayt için sıkışır 2,044 bayt
    * **/testfile1kb.txt** -47 bayt için sıkışır 1.033 bayt metin dosyası yanıt
    * **/ trickle** -1 saniyelik aralıklarla tek karakter olarak verilen yanıt
  * `image/svg+xml`
    * **/Banner.SVG** -A ölçeklenebilir vektör grafiği (SVG) görüntü yanıt 4,459 bayt için sıkışır 9,707 bayt
* `CustomCompressionProvider`<br>Ara yazılım ile kullanmak için özel sıkıştırma sağlayıcıyı uygulama işlemini gösterir

İstek zaman içerir `Accept-Encoding` örnek üstbilgisini ekler bir `Vary: Accept-Encoding` yanıt üst bilgisi. `Vary` Üstbilgi bildirir alternatif değerlerine dayalı yanıt birden çok kopyasını korumak için önbellekleri `Accept-Encoding`, aşağıdakilerden birini yapabilirsiniz sistemlerine sıkıştırılmış kabul etmek için bir sıkıştırılmış (gzip) ve sıkıştırılmamış önbelleklerinde depolanan veya sıkıştırılmamış yanıtı.

## <a name="using-the-sample"></a>Örnek kullanma

1. Bir istek kullanarak olun [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) olmadan uygulamasına bir `Accept-Encoding` üstbilgi ve Not yanıt yükünde yanıt boyutu ve yanıt üstbilgileri.
1. Ekleme bir `Accept-Encoding: gzip` üstbilgisi ve sıkıştırılmış yanıt boyutu ve yanıt üst bilgilerini not edin. Bırakma yanıt boyutu bakın ve `Content-Encoding: gzip` yanıt üst bilgisi, örnek uygulama tarafından eklenir. Yanıt gövdesi için Lorem Ipsum baktığınızda veya **testfile1kb.txt** yanıt, gördüğünüz sıkıştırılmış ve okunamaz metindir.
1. Ekleme bir `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üst bilgilerini not edin. `CustomCompressionProvider` Gerçekten yanıt sıkıştırma olmayan boş bir uygulama olduğu halde için özel sıkıştırma stream sarmalayıcı oluşturabilirsiniz `CreateStream()` yöntemi.
