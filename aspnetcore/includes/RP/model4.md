<span data-ttu-id="13298-101">Aşağıdaki tabloda, ASP.NET Core Kod Oluşturucu parametreleri ayrıntıları:</span><span class="sxs-lookup"><span data-stu-id="13298-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="13298-102">Parametre</span><span class="sxs-lookup"><span data-stu-id="13298-102">Parameter</span></span>               | <span data-ttu-id="13298-103">Açıklama</span><span class="sxs-lookup"><span data-stu-id="13298-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="13298-104">-m</span><span class="sxs-lookup"><span data-stu-id="13298-104">-m</span></span>  | <span data-ttu-id="13298-105">Modelin adı.</span><span class="sxs-lookup"><span data-stu-id="13298-105">The name of the model.</span></span> |
| <span data-ttu-id="13298-106">-dc</span><span class="sxs-lookup"><span data-stu-id="13298-106">-dc</span></span>  | <span data-ttu-id="13298-107">Veri bağlamı.</span><span class="sxs-lookup"><span data-stu-id="13298-107">The data context.</span></span> |
| <span data-ttu-id="13298-108">-udl</span><span class="sxs-lookup"><span data-stu-id="13298-108">-udl</span></span> | <span data-ttu-id="13298-109">Ekran düzenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="13298-109">Use the default layout.</span></span> |
| <span data-ttu-id="13298-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="13298-110">-outDir</span></span> | <span data-ttu-id="13298-111">Görünümleri oluşturmak için göreli çıkış klasör yolu.</span><span class="sxs-lookup"><span data-stu-id="13298-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="13298-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="13298-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="13298-113">Ekler `_ValidationScriptsPartial` düzenleyip sayfaları oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="13298-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="13298-114">Kullanım `h` hakkında Yardım almak için anahtar `aspnet-codegenerator razorpage` komutu:</span><span class="sxs-lookup"><span data-stu-id="13298-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="13298-115">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="13298-115">Test the app</span></span>

* <span data-ttu-id="13298-116">Uygulamayı çalıştırın ve ekleme `/Movies` tarayıcıda URL'sine (`http://localhost:port/Movies`).</span><span class="sxs-lookup"><span data-stu-id="13298-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/Movies`).</span></span>
* <span data-ttu-id="13298-117">Test **Oluştur** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="13298-117">Test the **Create** link.</span></span>

  ![sayfası oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="13298-119">Test **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="13298-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="13298-120">Aşağıdakine benzer bir hata alırsanız, geçişler çalıştırma ve veritabanına güncelleştirilmiş doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="13298-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

`An unhandled exception occurred while processing the request. 'no such table: Movie'.`
