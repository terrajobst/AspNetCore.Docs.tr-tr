# <a name="update-the-generated-pages"></a>Oluşturulan sayfaları güncelleştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Film uygulaması için iyi bir başlangıç sahibiz ancak sunu ideal değildir. Saat (12:00: 00'da aşağıdaki görüntüde) görmesini istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki kelimeye).

![Film verileri gösteren Chrome'da açık film uygulaması](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a>Oluşturulan kodu güncelleştirme

Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterilen vurgulanan satırları ekleyin:

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
