---
ms.openlocfilehash: c82571d3cfa57ccd6e7c83f654f119bdd8991486
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58210459"
---
```console
npm run release
```

<span data-ttu-id="34e0b-101">Bu komut, uygulamayı çalıştırırken alınacağı istemci-tarafı varlıkları verir.</span><span class="sxs-lookup"><span data-stu-id="34e0b-101">This command yields the client-side assets to be served when running the app.</span></span> <span data-ttu-id="34e0b-102">Varlıkları yerleştirilir *wwwroot* klasör.</span><span class="sxs-lookup"><span data-stu-id="34e0b-102">The assets are placed in the *wwwroot* folder.</span></span>

<span data-ttu-id="34e0b-103">Web, aşağıdaki görevleri tamamlandı:</span><span class="sxs-lookup"><span data-stu-id="34e0b-103">Webpack completed the following tasks:</span></span>

* <span data-ttu-id="34e0b-104">İçeriğini temizleneceği *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="34e0b-104">Purged the contents of the *wwwroot* directory.</span></span>
* <span data-ttu-id="34e0b-105">JavaScript için TypeScript dönüştürülen&mdash;olarak da bilinen bir işlem *transpilation*.</span><span class="sxs-lookup"><span data-stu-id="34e0b-105">Converted the TypeScript to JavaScript&mdash;a process known as *transpilation*.</span></span>
* <span data-ttu-id="34e0b-106">Dosya boyutunu küçültmek için oluşturulan JavaScript karıştırılmış&mdash;olarak da bilinen bir işlem *küçültme*.</span><span class="sxs-lookup"><span data-stu-id="34e0b-106">Mangled the generated JavaScript to reduce file size&mdash;a process known as *minification*.</span></span>
* <span data-ttu-id="34e0b-107">İşlenen JavaScript, CSS ve HTML dosyalarından kopyalanan *src* için *wwwroot* dizin.</span><span class="sxs-lookup"><span data-stu-id="34e0b-107">Copied the processed JavaScript, CSS, and HTML files from *src* to the *wwwroot* directory.</span></span>
* <span data-ttu-id="34e0b-108">Aşağıdaki öğeleri içine eklenen *wwwroot/index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="34e0b-108">Injected the following elements into the *wwwroot/index.html* file:</span></span>
  * <span data-ttu-id="34e0b-109">A `<link>` başvuran etiketi *wwwroot/main.\< karma\>.css* dosya.</span><span class="sxs-lookup"><span data-stu-id="34e0b-109">A `<link>` tag, referencing the *wwwroot/main.\<hash\>.css* file.</span></span> <span data-ttu-id="34e0b-110">Bu etiket kapatılmadan hemen önce yerleştirilir `</head>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="34e0b-110">This tag is placed immediately before the closing `</head>` tag.</span></span>
  * <span data-ttu-id="34e0b-111">A `<script>` küçültülmüş başvuran etiketi *wwwroot/main.\< karma\>.js* dosya.</span><span class="sxs-lookup"><span data-stu-id="34e0b-111">A `<script>` tag, referencing the minified *wwwroot/main.\<hash\>.js* file.</span></span> <span data-ttu-id="34e0b-112">Bu etiket kapatılmadan hemen önce yerleştirilir `</body>` etiketi.</span><span class="sxs-lookup"><span data-stu-id="34e0b-112">This tag is placed immediately before the closing `</body>` tag.</span></span>
