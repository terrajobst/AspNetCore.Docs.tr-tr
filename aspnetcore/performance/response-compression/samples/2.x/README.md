# <a name="response-compression-sample-application-aspnet-core-2x"></a>Yanıt sıkıştırma örnek uygulaması (ASP.NET Core 2.x)

Bu örnek ASP.NET Core kullanımını göstermektedir 2.x için HTTP yanıtları sıkıştırma yanıt sıkıştırma Ara. Örnek Gzip ve metin ve resim yanıtlar için özel sıkıştırma sağlayıcıları gösterir ve sıkıştırma MIME türü eklemeyi gösterir. ASP.NET Core 1.x örnek için bkz: [yanıt sıkıştırma örnek uygulaması (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x).

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Lorem Ipsum metin dosyası yanıt 927 bayt sıkışır 2,044 bayt
    * **/testfile1kb.txt** -metin dosyası yanıt 47 bayt sıkışır 1.033 bayt
    * **/ trickle** -1 ikinci aralıklarla tek karakter olarak verilen yanıt 
  * `image/svg+xml`
    * **/Banner.SVG** -A ölçeklenebilir vektör grafikleri (SVG) görüntüsü yanıt 4,459 bayt sıkışır 9,707 bayt
* `CustomCompressionProvider`<br>Ara yazılım ile kullanılmak üzere özel sıkıştırma sağlayıcısı için uygulanacak nasıl gösterir

İstek zaman içerir `Accept-Encoding` üstbilgi ve yanıt sıkıştırma başarılı olur, ara yazılımı otomatik olarak ekler bir `Vary: Accept-Encoding` yanıtı üstbilgisi. `Vary` Üstbilgi bildirir alternatif değerlerine dayalı yanıt birden çok kopyasını korumak için önbellekleri `Accept-Encoding`, seçebilir ya da sistemleri sıkıştırılmış kabul etmek için bir sıkıştırılmış (gzip) ve sıkıştırılmamış önbelleklerinde depolanır veya sıkıştırılmamış yanıtı.

## <a name="using-the-sample"></a>Örnek kullanma
1. İstek kullanarak yaptığınız [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), veya [Postman](https://www.getpostman.com/) olmaksızın uygulamaya bir `Accept-Encoding` üstbilgi ve Not yanıt yükü yanıt boyutu ve yanıt üstbilgileri.
2. Ekleme bir `Accept-Encoding: gzip` üstbilgisi ve sıkıştırılmış yanıt boyutu ve yanıt üstbilgileri not edin. Yanıt boyutu bırakır ve `Content-Encoding: gzip` yanıt üstbilgisi ara yazılım tarafından eklenir. Yanıt gövdesi için Lorem Ipsum baktığınızda veya **testfile1kb.txt** yanıt, gördüğünüz metin sıkıştırılmış ve okunamaz durumda.
3. Ekleme bir `Accept-Encoding: mycustomcompression` üstbilgi ve yanıt üst bilgilerini not edin. `CustomCompressionProvider` Yanıt gerçekten sıkıştırmak değil boş bir uygulamasıdır, ancak için özel sıkıştırma akış sarmalayıcı oluşturabilirsiniz `CreateStream()` yöntemi.
