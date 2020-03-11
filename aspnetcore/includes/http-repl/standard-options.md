* `-F|--no-formatting`

  <span data-ttu-id="dfe37-101">Durumu HTTP yanıt biçimlendirmesini belirten bir bayrak.</span><span class="sxs-lookup"><span data-stu-id="dfe37-101">A flag whose presence suppresses HTTP response formatting.</span></span>

* `-h|--header`

  <span data-ttu-id="dfe37-102">HTTP istek üst bilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="dfe37-102">Sets an HTTP request header.</span></span> <span data-ttu-id="dfe37-103">Aşağıdaki iki değer biçimi desteklenir:</span><span class="sxs-lookup"><span data-stu-id="dfe37-103">The following two value formats are supported:</span></span>

  * `{header}={value}`
  * `{header}:{value}`

* `--response`

  <span data-ttu-id="dfe37-104">Tüm HTTP yanıtının (üstbilgiler ve gövde dahil) yazılması gereken bir dosyayı belirtir.</span><span class="sxs-lookup"><span data-stu-id="dfe37-104">Specifies a file to which the entire HTTP response (including headers and body) should be written.</span></span> <span data-ttu-id="dfe37-105">Örneğin, `--response "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="dfe37-105">For example, `--response "C:\response.txt"`.</span></span> <span data-ttu-id="dfe37-106">Dosya yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfe37-106">The file is created if it doesn't exist.</span></span>

* `--response:body`

  <span data-ttu-id="dfe37-107">HTTP yanıt gövdesinin yazılması gereken bir dosyayı belirtir.</span><span class="sxs-lookup"><span data-stu-id="dfe37-107">Specifies a file to which the HTTP response body should be written.</span></span> <span data-ttu-id="dfe37-108">Örneğin, `--response:body "C:\response.json"`.</span><span class="sxs-lookup"><span data-stu-id="dfe37-108">For example, `--response:body "C:\response.json"`.</span></span> <span data-ttu-id="dfe37-109">Dosya yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfe37-109">The file is created if it doesn't exist.</span></span>

* `--response:headers`

  <span data-ttu-id="dfe37-110">HTTP yanıt üst bilgilerinin yazılması gereken bir dosyayı belirtir.</span><span class="sxs-lookup"><span data-stu-id="dfe37-110">Specifies a file to which the HTTP response headers should be written.</span></span> <span data-ttu-id="dfe37-111">Örneğin, `--response:headers "C:\response.txt"`.</span><span class="sxs-lookup"><span data-stu-id="dfe37-111">For example, `--response:headers "C:\response.txt"`.</span></span> <span data-ttu-id="dfe37-112">Dosya yoksa oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="dfe37-112">The file is created if it doesn't exist.</span></span>

* `-s|--streaming`

  <span data-ttu-id="dfe37-113">Mevcut bir bayrak, HTTP yanıtının akışını mümkün bir şekilde sunar.</span><span class="sxs-lookup"><span data-stu-id="dfe37-113">A flag whose presence enables streaming of the HTTP response.</span></span>
