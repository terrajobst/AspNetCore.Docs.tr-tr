<a name="codegenerator"></a><span data-ttu-id="7efb3-101">Aşağıdaki tabloda ASP.NET Core kod Oluşturucu parametrelerinin ayrıntıları verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="7efb3-101">The following table details the ASP.NET Core code generator parameters:</span></span>

| <span data-ttu-id="7efb3-102">Parametre</span><span class="sxs-lookup"><span data-stu-id="7efb3-102">Parameter</span></span>               | <span data-ttu-id="7efb3-103">Açıklama</span><span class="sxs-lookup"><span data-stu-id="7efb3-103">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="7efb3-104">-a</span><span class="sxs-lookup"><span data-stu-id="7efb3-104">-m</span></span>  | <span data-ttu-id="7efb3-105">Modelin adı.</span><span class="sxs-lookup"><span data-stu-id="7efb3-105">The name of the model.</span></span> |
| <span data-ttu-id="7efb3-106">-DC</span><span class="sxs-lookup"><span data-stu-id="7efb3-106">-dc</span></span>  | <span data-ttu-id="7efb3-107">Kullanılacak `DbContext` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="7efb3-107">The `DbContext` class to use.</span></span> |
| <span data-ttu-id="7efb3-108">-UDL</span><span class="sxs-lookup"><span data-stu-id="7efb3-108">-udl</span></span> | <span data-ttu-id="7efb3-109">Varsayılan düzeni kullanın.</span><span class="sxs-lookup"><span data-stu-id="7efb3-109">Use the default layout.</span></span> |
| <span data-ttu-id="7efb3-110">-outDir</span><span class="sxs-lookup"><span data-stu-id="7efb3-110">-outDir</span></span> | <span data-ttu-id="7efb3-111">Görünümleri oluşturmak için göreli çıkış klasörü yolu.</span><span class="sxs-lookup"><span data-stu-id="7efb3-111">The relative output folder path to create the views.</span></span> |
| <span data-ttu-id="7efb3-112">--referenceScriptLibraries</span><span class="sxs-lookup"><span data-stu-id="7efb3-112">--referenceScriptLibraries</span></span> | <span data-ttu-id="7efb3-113">Sayfaları düzenlemek ve oluşturmak için `_ValidationScriptsPartial` ekler</span><span class="sxs-lookup"><span data-stu-id="7efb3-113">Adds `_ValidationScriptsPartial` to Edit and Create pages</span></span> |

<span data-ttu-id="7efb3-114">`aspnet-codegenerator razorpage` komutu hakkında yardım almak için `h` anahtarını kullanın:</span><span class="sxs-lookup"><span data-stu-id="7efb3-114">Use the `h` switch to get help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="7efb3-115">Daha fazla bilgi için bkz. [DotNet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span><span class="sxs-lookup"><span data-stu-id="7efb3-115">For more information, see [dotnet aspnet-codegenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>
