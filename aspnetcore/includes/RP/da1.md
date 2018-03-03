# <a name="update-the-generated-pages"></a><span data-ttu-id="aa9f8-101">Oluşturulan sayfaları güncelleştir</span><span class="sxs-lookup"><span data-stu-id="aa9f8-101">Update the generated pages</span></span>

<span data-ttu-id="aa9f8-102">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa9f8-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa9f8-103">Film uygulaması için iyi bir başlangıç sahip olduğumuz ancak sunu ideal değil.</span><span class="sxs-lookup"><span data-stu-id="aa9f8-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="aa9f8-104">Biz (12:00: 00'da aşağıdaki görüntüde) zaman görmek istemediğiniz ve **ReleaseDate** olmalıdır **yayın tarihi** (iki sözcük).</span><span class="sxs-lookup"><span data-stu-id="aa9f8-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![Film uygulaması film verileri gösteren Chrome'da Aç](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="aa9f8-106">Oluşturulan kod güncelleştir</span><span class="sxs-lookup"><span data-stu-id="aa9f8-106">Update the generated code</span></span>

<span data-ttu-id="aa9f8-107">Açık *Models/Movie.cs* dosya ve aşağıdaki kodda gösterildiği vurgulanan satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="aa9f8-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
