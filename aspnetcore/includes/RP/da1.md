# <a name="update-the-generated-pages"></a><span data-ttu-id="e0ff8-101">Oluşturulan sayfaları güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e0ff8-101">Update the generated pages</span></span>

<span data-ttu-id="e0ff8-102">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0ff8-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e0ff8-103">Film uygulaması için iyi bir başlangıç sahibiz ancak sunu ideal değildir.</span><span class="sxs-lookup"><span data-stu-id="e0ff8-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="e0ff8-104">Saat (12:00: 00'da aşağıdaki görüntüde) görmesini istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki kelimeye).</span><span class="sxs-lookup"><span data-stu-id="e0ff8-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film verileri gösteren Chrome'da açık film uygulaması](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="e0ff8-106">Oluşturulan kodu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="e0ff8-106">Update the generated code</span></span>

<span data-ttu-id="e0ff8-107">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterilen vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="e0ff8-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
