# <a name="adding-a-view-to-an-aspnet-core-mvc-app"></a>Bir ASP.NET Core MVC uygulamasına bir görünümü ekleme

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu bölümde, değişiklik `HelloWorldController` sınıfı şablon dosyalarını düzgün bir şekilde kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemine Razor görünümünü kullanın.

Razor kullanarak bir görünüm şablon dosyası oluşturun. Razor tabanlı görünümü şablonları bir *.cshtml* dosya uzantısı. C# kullanarak HTML çıktı oluşturmak için zarif bir şekilde sağlarlar.

Şu anda `Index` yöntemi controller sınıfında sabit kodlanmış olduğunu belirten bir ileti ile bir dize döndürür. İçinde `HelloWorldController` sınıfı, yerine `Index` aşağıdaki kod ile yöntemi:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

Önceki kod döndüren bir `View` nesnesi. Tarayıcıya bir HTML yanıtı oluşturmak için bir görünüm şablonu kullanır. Denetleyici yöntemlerine (eylem yöntemleri olarak da bilinir) gibi `Index` yukarıdaki genellikle döndürme bir [IActionResult](/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) (veya türetilmiş bir sınıf `ActionResult`), dize gibi bir tür değil.
