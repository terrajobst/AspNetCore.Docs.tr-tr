* `-F|--no-formatting`

  <span data-ttu-id="bf210-101">HTTP yanıt biçimlendirme, varlığı bastırır bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="bf210-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="bf210-102">Bir HTTP isteği üstbilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="bf210-102">Sets an HTTP request header.</span></span> <span data-ttu-id="bf210-103">Aşağıdaki iki değer biçimleri desteklenir:</span><span class="sxs-lookup"><span data-stu-id="bf210-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="bf210-104">Bir dosya (üstbilgi ve gövde dahil) tüm HTTP yanıtı yazılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bf210-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="bf210-105">Örneğin: `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="bf210-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="bf210-106">Dosya yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bf210-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="bf210-107">Bir dosya HTTP yanıt gövdesinde yazılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bf210-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="bf210-108">Örneğin: `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="bf210-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="bf210-109">Dosya yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bf210-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="bf210-110">Bir dosya HTTP yanıt üstbilgilerinin yazılması gerektiğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="bf210-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="bf210-111">Örneğin: `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="bf210-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="bf210-112">Dosya yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bf210-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="bf210-113">HTTP yanıtı akış, durum sağlayan bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="bf210-113">A flag whose presence enables streaming of the HTTP response.</span></span>
