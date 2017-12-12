<span data-ttu-id="0a59f-101">Aşağıdaki tabloda Ayrıntılar ASP.NET Core oluşturucuları parametreleri kod:</span><span class="sxs-lookup"><span data-stu-id="0a59f-101">The following table details the ASP.NET Core code generators\` parameters:</span></span>

| <span data-ttu-id="0a59f-102">Parametre</span><span class="sxs-lookup"><span data-stu-id="0a59f-102">Parameter</span></span>               | <span data-ttu-id="0a59f-103">Açıklama</span><span class="sxs-lookup"><span data-stu-id="0a59f-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="0a59f-104">-m</span><span class="sxs-lookup"><span data-stu-id="0a59f-104">-m</span></span>  | <span data-ttu-id="0a59f-105">Modelin adı.</span><span class="sxs-lookup"><span data-stu-id="0a59f-105">The name of the model.</span></span> |
| <span data-ttu-id="0a59f-106">-dc</span><span class="sxs-lookup"><span data-stu-id="0a59f-106">-dc</span></span>  | <span data-ttu-id="0a59f-107">Veri bağlamı.</span><span class="sxs-lookup"><span data-stu-id="0a59f-107">The data context.</span></span> |
| <span data-ttu-id="0a59f-108">-udl</span><span class="sxs-lookup"><span data-stu-id="0a59f-108">-udl</span></span> | <span data-ttu-id="0a59f-109">Varsayılan düzenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="0a59f-109">Use the default layout.</span></span> |
| <span data-ttu-id="0a59f-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="0a59f-110">-outDir</span></span> | <span data-ttu-id="0a59f-111">Görünümleri oluşturmak için göreli çıkış klasörü yolu.</span><span class="sxs-lookup"><span data-stu-id="0a59f-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="0a59f-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="0a59f-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="0a59f-113">Ekler `_ValidationScriptsPartial` sayfaları oluşturmak ve düzenlemek için</span><span class="sxs-lookup"><span data-stu-id="0a59f-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="0a59f-114">Kullanım `h` hakkında Yardım almak için anahtar `aspnet-codegenerator razorpage` komutu:</span><span class="sxs-lookup"><span data-stu-id="0a59f-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```console
dotnet aspnet-codegenerator razorpage -h
```
<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="0a59f-115">Uygulamayı test etme</span><span class="sxs-lookup"><span data-stu-id="0a59f-115">Test the app</span></span>

* <span data-ttu-id="0a59f-116">Uygulamayı çalıştırın ve append `/Movies` URL tarayıcıda (`http://localhost:port/movies`).</span><span class="sxs-lookup"><span data-stu-id="0a59f-116">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="0a59f-117">Test **oluşturma** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="0a59f-117">Test the **Create** link.</span></span>

 ![Sayfa oluşturma](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="0a59f-119">Test **Düzenle**, **ayrıntıları**, ve **silmek** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="0a59f-119">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="0a59f-120">Aşağıdakine benzer hata alırsanız, geçişler çalıştırın ve veritabanı güncelleştirilmiş doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="0a59f-120">If you get the error similar to the following, verify you have run migrations and updated the database:</span></span>

```
An unhandled exception occurred while processing the request.
'no such table: Movie'.
```
