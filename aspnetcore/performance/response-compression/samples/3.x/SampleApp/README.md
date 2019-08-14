# <a name="response-compression-sample-application-aspnet-core-3x"></a>Yanıt sıkıştırma örnek uygulaması (ASP.NET Core 3. x)

Bu örnek, HTTP yanıtlarını sıkıştırmak için ASP.NET Core 3. x yanıt sıkıştırma ara yazılımı kullanımını gösterir. Örnek, metin ve görüntü yanıtları için gzip, Brotli ve özel sıkıştırma sağlayıcılarını gösterir ve sıkıştırma için bir MIME türünün nasıl ekleneceğini gösterir. ASP.NET Core 2. x örneği için bkz. [Yanıt sıkıştırması örnek uygulaması (ASP.NET Core 2. x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x).

## <a name="examples-in-this-sample"></a>Bu örnekteki örnekler

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum metin dosyası yanıtı, yaklaşık 979 bayt olarak sıkıştıran 2.044 bayttır.
    * **/testfile1kb.txt** -1.033 baytındaki metin dosyası yanıtı ~ 36 bayt olarak sıkıştırır.
    * **/Trickle** -yanıt, 1 saniyelik aralıklarla tek karakter olarak verildi.
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum metin dosyası yanıtı, yaklaşık 927 bayt olarak sıkıştıran 2.044 bayttır.
    * **/testfile1kb.txt** -1.033 baytındaki metin dosyası yanıtı ~ 47 bayt olarak sıkıştırır.
    * **/Trickle** -yanıt, 1 saniyelik aralıklarla tek karakter olarak verildi.
  * `image/svg+xml`
    * **/Banner.SVG** -yaklaşık 4.459 bayt olarak sıkıştıran 9.707 baytındaki ölçeklenebilir vektör GRAFIK (SVG) görüntü yanıtı.
* `CustomCompressionProvider`<br>Bir özel sıkıştırma sağlayıcısının, ara yazılım ile kullanılmak üzere nasıl uygulanacağını gösterir.

İstek `Accept-Encoding` üstbilgiyi içerdiğinde ve yanıt sıkıştırması başarılı olduğunda, ara yazılım yanıta otomatik olarak bir `Vary: Accept-Encoding` üst bilgi ekler. Üst bilgi, diğer `Accept-Encoding`değerlerine göre yanıtın birden çok kopyasını tutmak üzere önbellekler verir, bu nedenle hem sıkıştırılmış (gzip veya Brotli) hem de sıkıştırılmamış sürüm, `Vary` sıkıştırılmış veya sıkıştırılmamış yanıt.

## <a name="use-the-sample"></a>Örneği kullanın

1. `Accept-Encoding` Üst bilgi olmadan uygulama için [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/)veya [Postman](https://www.getpostman.com/) kullanarak bir istek yapın ve yanıt yükünü, yanıt boyutunu ve yanıt üst bilgilerini unutmayın.
1. Bir `Accept-Encoding: br` veya`Accept-Encoding: gzip` üst bilgisi ekleyin ve sıkıştırılan yanıt boyutunu ve yanıt üst bilgilerini aklınızda yapın. Yanıt boyutu düşer ve `Content-Encoding` yanıt üst bilgisi, gzip veya Brotli oluştuğunu belirten bir ara yazılım tarafından eklenir. Lorem Ipsum veya **testfile1kb. txt** yanıtının yanıt gövdesine baktığınızda, metnin sıkıştırıldığını ve okunmadığını görürsünüz.
1. Bir `Accept-Encoding: mycustomcompression` üst bilgi ekleyin ve yanıt üst bilgilerini aklınızda yapın. , `CustomCompressionProvider` Yanıtı gerçekten sıkıştıramadığında boş bir uygulama, ancak `CreateStream()` Yöntem için özel bir sıkıştırma akış sarmalayıcısı oluşturabilirsiniz.
