---
title: Bir ASP.NET Core MVC uygulaması için bir Görünüm Ekle
author: rick-anderson
description: Basit bir ASP.NET Core MVC uygulaması görünüm ekleme
ms.author: riande
ms.date: 03/04/2017
uid: tutorials/first-mvc-app/adding-view
ms.openlocfilehash: c86e5aba6dbd1e39460cc3d2c266ce704282ee09
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815427"
---
# <a name="add-a-view-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="8108c-103">Bir ASP.NET Core MVC uygulaması için bir Görünüm Ekle</span><span class="sxs-lookup"><span data-stu-id="8108c-103">Add a view to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="8108c-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8108c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8108c-105">Bu bölümde, değişiklik `HelloWorldController` kullanılacak sınıfı [Razor](xref:mvc/views/razor) görünüm dosyalarına indrebilirsiniz kapsülleyen bir istemci HTML yanıtlarını oluşturma işlemi.</span><span class="sxs-lookup"><span data-stu-id="8108c-105">In this section you modify the `HelloWorldController` class to use [Razor](xref:mvc/views/razor) view files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="8108c-106">Razor kullanarak görünüm şablon dosyası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8108c-106">You create a view template file using Razor.</span></span> <span data-ttu-id="8108c-107">Razor tabanlı bir görünüm şablonları bir *.cshtml* dosya uzantısı.</span><span class="sxs-lookup"><span data-stu-id="8108c-107">Razor-based view templates have a *.cshtml* file extension.</span></span> <span data-ttu-id="8108c-108">HTML çıkışı ile oluşturmak için zarif bir şekilde sağladıkları C#.</span><span class="sxs-lookup"><span data-stu-id="8108c-108">They provide an elegant way to create HTML output with C#.</span></span>

<span data-ttu-id="8108c-109">Şu anda `Index` yöntemi controller sınıfında sabit kodlu olduğunu belirten bir ileti içeren bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="8108c-109">Currently the `Index` method returns a string with a message that's hard-coded in the controller class.</span></span> <span data-ttu-id="8108c-110">İçinde `HelloWorldController` sınıfı, yerine `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="8108c-110">In the `HelloWorldController` class, replace the `Index` method with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_4)]

<span data-ttu-id="8108c-111">Önceki kod, denetleyicinin çağırır <xref:Microsoft.AspNetCore.Mvc.Controller.View*> yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8108c-111">The preceding code calls the controller's <xref:Microsoft.AspNetCore.Mvc.Controller.View*> method.</span></span> <span data-ttu-id="8108c-112">HTML yanıtı oluşturmak için bir görünüm şablonu kullanır.</span><span class="sxs-lookup"><span data-stu-id="8108c-112">It uses a view template to generate an HTML response.</span></span> <span data-ttu-id="8108c-113">Denetleyici yöntemlerinde (olarak da bilinen *eylem yöntemleri*), gibi `Index` yukarıdaki yöntemi genellikle iade bir <xref:Microsoft.AspNetCore.Mvc.IActionResult> (veya türetilmiş bir sınıf <xref:Microsoft.AspNetCore.Mvc.ActionResult>), bir tür değil istediğiniz `string`.</span><span class="sxs-lookup"><span data-stu-id="8108c-113">Controller methods (also known as *action methods*), such as the `Index` method above, generally return an <xref:Microsoft.AspNetCore.Mvc.IActionResult> (or a class derived from <xref:Microsoft.AspNetCore.Mvc.ActionResult>), not a type like `string`.</span></span>

## <a name="add-a-view"></a><span data-ttu-id="8108c-114">Görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="8108c-114">Add a view</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="8108c-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8108c-115">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="8108c-116">Sağ tıklayın *görünümleri* klasörünü ve ardından **Ekle > Yeni klasör** ve klasörünü adlandırın *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="8108c-116">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>

* <span data-ttu-id="8108c-117">Sağ tıklayın *görünümler/HelloWorld* klasörünü ve ardından **Ekle > Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="8108c-117">Right click on the *Views/HelloWorld* folder, and then **Add > New Item**.</span></span>

* <span data-ttu-id="8108c-118">İçinde **Yeni Öğe Ekle - MvcMovie** iletişim</span><span class="sxs-lookup"><span data-stu-id="8108c-118">In the **Add New Item - MvcMovie** dialog</span></span>

  * <span data-ttu-id="8108c-119">Sağ üst arama kutusuna girin *görüntüle*</span><span class="sxs-lookup"><span data-stu-id="8108c-119">In the search box in the upper-right, enter *view*</span></span>

  * <span data-ttu-id="8108c-120">Seçin **Razor Görünüm**</span><span class="sxs-lookup"><span data-stu-id="8108c-120">Select **Razor View**</span></span>

  * <span data-ttu-id="8108c-121">Tutun **adı** değer kutusuna *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8108c-121">Keep the **Name** box value, *Index.cshtml*.</span></span>

  * <span data-ttu-id="8108c-122">Seçin **Ekle**</span><span class="sxs-lookup"><span data-stu-id="8108c-122">Select **Add**</span></span>

![Yeni öğe iletişim kutusu Ekle](adding-view/_static/add_view.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="8108c-124">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="8108c-124">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="8108c-125">Ekleme bir `Index` görüntüleme `HelloWorldController`.</span><span class="sxs-lookup"><span data-stu-id="8108c-125">Add an `Index` view for the `HelloWorldController`.</span></span>

* <span data-ttu-id="8108c-126">Adlı yeni bir klasör eklemek *görünümler/HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="8108c-126">Add a new folder named *Views/HelloWorld*.</span></span>
* <span data-ttu-id="8108c-127">Yeni bir dosya ekleyin *görünümler/HelloWorld* klasör adı *Index.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8108c-127">Add a new file to the *Views/HelloWorld* folder name *Index.cshtml*.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="8108c-128">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="8108c-128">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="8108c-129">Sağ tıklayın *görünümleri* klasörünü ve ardından **Ekle > Yeni klasör** ve klasörünü adlandırın *HelloWorld*.</span><span class="sxs-lookup"><span data-stu-id="8108c-129">Right click on the *Views* folder, and then **Add > New Folder** and name the folder *HelloWorld*.</span></span>
* <span data-ttu-id="8108c-130">Sağ tıklayın *görünümler/HelloWorld* klasörünü ve ardından **Ekle > yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="8108c-130">Right click on the *Views/HelloWorld* folder, and then **Add > New File**.</span></span>
* <span data-ttu-id="8108c-131">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="8108c-131">In the **New File** dialog:</span></span>

  * <span data-ttu-id="8108c-132">Seçin **Web** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="8108c-132">Select **Web** in the left pane.</span></span>
  * <span data-ttu-id="8108c-133">Seçin **boş HTML dosyası** Orta bölmedeki.</span><span class="sxs-lookup"><span data-stu-id="8108c-133">Select **Empty HTML file** in the center pane.</span></span>
  * <span data-ttu-id="8108c-134">Tür *Index.cshtml* içinde **adı** kutusu.</span><span class="sxs-lookup"><span data-stu-id="8108c-134">Type *Index.cshtml* in the **Name** box.</span></span>
  * <span data-ttu-id="8108c-135">Seçin **yeni**.</span><span class="sxs-lookup"><span data-stu-id="8108c-135">Select **New**.</span></span>

![Yeni öğe iletişim kutusu Ekle](adding-view/_static/add_view.png)

---

<span data-ttu-id="8108c-137">Öğesinin içeriğini değiştirin *Views/HelloWorld/Index.cshtml* aşağıdaki Razor görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="8108c-137">Replace the contents of the *Views/HelloWorld/Index.cshtml* Razor view file with the following:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/HelloWorld/Index1.cshtml?highlight=7)]

<span data-ttu-id="8108c-138">          `https://localhost:xxxx/HelloWorld` sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="8108c-138">Navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="8108c-139">`Index` Yönteminde `HelloWorldController` çok başarmadık; çalıştığını deyim `return View();`, hangi belirtilen tarayıcı yanıt işlemek için yöntemi bir görünüm şablon dosyası kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="8108c-139">The `Index` method in the `HelloWorldController` didn't do much; it ran the statement `return View();`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="8108c-140">MVC görünümü şablon dosyası adı belirtilmedi çünkü varsayılan görünüm dosyası kullanarak varsayılan olarak.</span><span class="sxs-lookup"><span data-stu-id="8108c-140">Because a view template file name wasn't specified, MVC defaulted to using the default view file.</span></span> <span data-ttu-id="8108c-141">Varsayılan görünüm dosyası yöntemi olarak aynı ada sahip (`Index`), dolayısıyla */Views/HelloWorld/Index.cshtml* kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8108c-141">The default view file has the same name as the method (`Index`), so in the */Views/HelloWorld/Index.cshtml* is used.</span></span> <span data-ttu-id="8108c-142">Aşağıdaki görüntüde "Bizim görünümü şablondan Merhaba!" dizesini gösterir.</span><span class="sxs-lookup"><span data-stu-id="8108c-142">The image below shows the string "Hello from our View Template!"</span></span> <span data-ttu-id="8108c-143">sabit kodlanmış görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="8108c-143">hard-coded in the view.</span></span>

![Tarayıcı penceresi](~/tutorials/first-mvc-app/adding-view/_static/hell_template.png)

## <a name="change-views-and-layout-pages"></a><span data-ttu-id="8108c-145">Değişiklik görünümleri ve yerleşim sayfaları</span><span class="sxs-lookup"><span data-stu-id="8108c-145">Change views and layout pages</span></span>

<span data-ttu-id="8108c-146">Menü bağlantıyı seçin (**MvcMovie**, **giriş**, ve **gizlilik**).</span><span class="sxs-lookup"><span data-stu-id="8108c-146">Select the menu links (**MvcMovie**, **Home**, and **Privacy**).</span></span> <span data-ttu-id="8108c-147">Her sayfada aynı menüsü düzeni gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8108c-147">Each page shows the same menu layout.</span></span> <span data-ttu-id="8108c-148">Menüsü düzeni uygulanan *Views/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8108c-148">The menu layout is implemented in the *Views/Shared/_Layout.cshtml* file.</span></span> <span data-ttu-id="8108c-149">Açık *Views/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="8108c-149">Open the *Views/Shared/_Layout.cshtml* file.</span></span>

<span data-ttu-id="8108c-150">[Düzen](xref:mvc/views/layout) , tek bir yerde sitenizin HTML kapsayıcı düzenini belirtin ve ardından sitenizdeki birden çok sayfada uygulamak şablonlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="8108c-150">[Layout](xref:mvc/views/layout) templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="8108c-151">Bulma `@RenderBody()` satır.</span><span class="sxs-lookup"><span data-stu-id="8108c-151">Find the `@RenderBody()` line.</span></span> <span data-ttu-id="8108c-152">`RenderBody` olduğundan burada tüm görünüm özel sayfalar, yer tutucu oluşturma Göster, *sarmalanmış* Düzen sayfasında.</span><span class="sxs-lookup"><span data-stu-id="8108c-152">`RenderBody` is a placeholder where all the view-specific pages you create show up, *wrapped* in the layout page.</span></span> <span data-ttu-id="8108c-153">Örneğin, **gizlilik** bağlantı **Views/Home/Privacy.cshtml** görünümü içinde işlenir `RenderBody` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="8108c-153">For example, if you select the **Privacy** link, the **Views/Home/Privacy.cshtml** view is rendered inside the `RenderBody` method.</span></span>

## <a name="change-the-title-footer-and-menu-link-in-the-layout-file"></a><span data-ttu-id="8108c-154">Düzen dosyası başlık, alt bilgi ve menü Bağlantıyı Değiştir</span><span class="sxs-lookup"><span data-stu-id="8108c-154">Change the title, footer, and menu link in the layout file</span></span>

* <span data-ttu-id="8108c-155">Başlık ve alt öğelerini değiştirmeniz `MvcMovie` için `Movie App`.</span><span class="sxs-lookup"><span data-stu-id="8108c-155">In the title and footer elements, change `MvcMovie` to `Movie App`.</span></span>
* <span data-ttu-id="8108c-156">Bağlayıcı bir öğe değiştirme `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` için `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span><span class="sxs-lookup"><span data-stu-id="8108c-156">Change the anchor element `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` to `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`.</span></span>

<span data-ttu-id="8108c-157">Aşağıdaki biçimlendirmede vurgulanan değişiklikleri gösterir:</span><span class="sxs-lookup"><span data-stu-id="8108c-157">The following markup shows the highlighted changes:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Shared/_Layout.cshtml?highlight=6,24,51)]

<span data-ttu-id="8108c-158">Önceki biçimlendirmede `asp-area` [yer işareti etiketi Yardımcısı özniteliği](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) bu uygulamayı değil kullandığından atlandı [alanları](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="8108c-158">In the preceding markup, the `asp-area` [anchor Tag Helper attribute](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) was omitted because this app is not using [Areas](xref:mvc/controllers/areas).</span></span>

<!-- Routing has changed in 2.2, it's going to the last route.
>[!WARNING]
> We haven't implemented the `Movies` controller yet, so if you click the `Movie App` link, you get a 404 (Not found) error.
-->

<span data-ttu-id="8108c-159">**Not**: `Movies` Denetleyicisi uygulanmamış.</span><span class="sxs-lookup"><span data-stu-id="8108c-159">**Note**: The `Movies` controller has not been implemented.</span></span> <span data-ttu-id="8108c-160">Bu noktada, `Movie App` bağlantısı işlevsel değil.</span><span class="sxs-lookup"><span data-stu-id="8108c-160">At this point, the `Movie App` link is not functional.</span></span>

<span data-ttu-id="8108c-161">Seçin ve değişiklikleri kaydetmek **gizlilik** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8108c-161">Save your changes and select the **Privacy** link.</span></span> <span data-ttu-id="8108c-162">Tarayıcı sekmesini başlığında nasıl görüntülendiğine dikkat edin **gizlilik ilkesi - film uygulaması** yerine **gizlilik ilkesi - Mvc film**:</span><span class="sxs-lookup"><span data-stu-id="8108c-162">Notice how the title on the browser tab displays **Privacy Policy - Movie App** instead of **Privacy Policy - Mvc Movie**:</span></span>

![Gizlilik sekmesi](~/tutorials/first-mvc-app/adding-view/_static/about2.png)

<span data-ttu-id="8108c-164">Seçin **giriş** bağlamak ve başlık ve bağlantı metni de görüntülemek fark **film uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="8108c-164">Select the **Home** link and notice that the title and anchor text also display **Movie App**.</span></span> <span data-ttu-id="8108c-165">Biz Düzen şablonda değişiklik kez sunmayı başarabilseydiniz nasıl olurdu ve sahip sitesindeki tüm sayfalara yansıtan yeni bir başlık ve yeni bağlantı metni.</span><span class="sxs-lookup"><span data-stu-id="8108c-165">We were able to make the change once in the layout template and have all pages on the site reflect the new link text and new title.</span></span>

<span data-ttu-id="8108c-166">İnceleme *Views/_ViewStart.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8108c-166">Examine the *Views/_ViewStart.cshtml* file:</span></span>

```HTML
@{
    Layout = "_Layout";
}
```

<span data-ttu-id="8108c-167">*Views/_ViewStart.cshtml* dosya getirdiği *Views/Shared/_Layout.cshtml* dosyaya her görünüm.</span><span class="sxs-lookup"><span data-stu-id="8108c-167">The *Views/_ViewStart.cshtml* file brings in the *Views/Shared/_Layout.cshtml* file to each view.</span></span> <span data-ttu-id="8108c-168">`Layout` Özelliği, farklı bir görünümü ayarlamak veya ayarlamanız için kullanılabilir `null` hiçbir Düzen dosyası yeniden kullanılacak şekilde.</span><span class="sxs-lookup"><span data-stu-id="8108c-168">The `Layout` property can be used to set a different layout view, or set it to `null` so no layout file will be used.</span></span>

<span data-ttu-id="8108c-169">Başlığı değiştirmek ve `<h2>` öğesinin *Views/HelloWorld/Index.cshtml* dosyasını görüntüle:</span><span class="sxs-lookup"><span data-stu-id="8108c-169">Change the title and `<h2>` element of the *Views/HelloWorld/Index.cshtml* view file:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Index2.cshtml?highlight=2,5)]

<span data-ttu-id="8108c-170">Başlığı ve `<h2>` hangi bit kod değişiklikleri görüntüleme görebilmeniz için öğe biraz farklı.</span><span class="sxs-lookup"><span data-stu-id="8108c-170">The title and `<h2>` element are slightly different so you can see which bit of code changes the display.</span></span>

<span data-ttu-id="8108c-171">`ViewData["Title"] = "Movie List";` koddaki kümeleri üstündeki `Title` özelliği `ViewData` sözlüğe "Film listesi".</span><span class="sxs-lookup"><span data-stu-id="8108c-171">`ViewData["Title"] = "Movie List";` in the code above sets the `Title` property of the `ViewData` dictionary to "Movie List".</span></span> <span data-ttu-id="8108c-172">`Title` Özelliği kullanılan `<title>` HTML öğesini düzen sayfası:</span><span class="sxs-lookup"><span data-stu-id="8108c-172">The `Title` property is used in the `<title>` HTML element in the layout page:</span></span>

```HTML
<title>@ViewData["Title"] - Movie App</title>
   ```

<span data-ttu-id="8108c-173">Değişikliği kaydetmek ve gidin `https://localhost:xxxx/HelloWorld`.</span><span class="sxs-lookup"><span data-stu-id="8108c-173">Save the change and navigate to `https://localhost:xxxx/HelloWorld`.</span></span> <span data-ttu-id="8108c-174">Tarayıcı başlığı, başlığı birincil ve ikincil başlıklar değiştirildi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="8108c-174">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="8108c-175">(Tarayıcı değişiklikleri görmüyorsanız, önbelleğe alınmış içerikleri görüntülüyor olabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8108c-175">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="8108c-176">Tarayıcınızda yüklenmesine sunucudan yanıt zorlamak için CTRL + F5'e basın.) Tarayıcı başlığı ile oluşturulan `ViewData["Title"]` biz kümesinde *Index.cshtml* şablonu ve ek görüntüleme "-film uygulaması" Düzen dosyasında eklendi.</span><span class="sxs-lookup"><span data-stu-id="8108c-176">Press Ctrl+F5 in your browser to force the response from the server to be loaded.) The browser title is created with `ViewData["Title"]` we set in the *Index.cshtml* view template and the additional "- Movie App" added in the layout file.</span></span>

<span data-ttu-id="8108c-177">Ayrıca dikkat edin nasıl içeriği *Index.cshtml* görünüm şablonu ile birleştirilmiş *Views/Shared/_Layout.cshtml* şablonu görüntüleme ve tek bir HTML yanıtını tarayıcıya gönderilen.</span><span class="sxs-lookup"><span data-stu-id="8108c-177">Also notice how the content in the *Index.cshtml* view template was merged with the *Views/Shared/_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="8108c-178">Düzen şablonları gerçekten tüm uygulamanızdaki sayfaların uygulanacak değişiklikler yapmak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="8108c-178">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span> <span data-ttu-id="8108c-179">Daha fazla bilgi edinmek için [Düzen](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="8108c-179">To learn more see [Layout](xref:mvc/views/layout).</span></span>

![Film liste görünümü](~/tutorials/first-mvc-app/adding-view/_static/hell3.png)

<span data-ttu-id="8108c-181">(Bu durumda "Hello bizim görünümü şablondan!", "veri" az bizim bit</span><span class="sxs-lookup"><span data-stu-id="8108c-181">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="8108c-182">ileti) sabit kodlanmıştır, ancak.</span><span class="sxs-lookup"><span data-stu-id="8108c-182">message) is hard-coded, though.</span></span> <span data-ttu-id="8108c-183">MVC uygulaması bir "V" (view) ve "C" (denetleyicisi), ancak hiçbir "M" (modeli) henüz kendinizi.</span><span class="sxs-lookup"><span data-stu-id="8108c-183">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="8108c-184">Görünüm denetleyicisinden veri geçirme</span><span class="sxs-lookup"><span data-stu-id="8108c-184">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="8108c-185">Denetleyici eylemleri, bir gelen URL isteğine yanıt olarak çağrılır.</span><span class="sxs-lookup"><span data-stu-id="8108c-185">Controller actions are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="8108c-186">Kod gelen tarayıcı istekleri işleyen yazıldığı bir denetleyici sınıftır.</span><span class="sxs-lookup"><span data-stu-id="8108c-186">A controller class is where the code is written that handles the incoming browser requests.</span></span> <span data-ttu-id="8108c-187">Denetleyici, verileri bir veri kaynağından alır ve ne tür bir tarayıcıya gönderilecek yanıt karar verir.</span><span class="sxs-lookup"><span data-stu-id="8108c-187">The controller retrieves data from a data source and decides what type of response to send back to the browser.</span></span> <span data-ttu-id="8108c-188">Bir denetleyiciden görünüm şablonları oluşturmak ve bir HTML yanıtını tarayıcıya biçimlendirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="8108c-188">View templates can be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="8108c-189">Denetleyicileri sırada bir yanıt işlemek bir görünüm şablon için gerekli olan veriler sağlamaktan sorumludur.</span><span class="sxs-lookup"><span data-stu-id="8108c-189">Controllers are responsible for providing the data required in order for a view template to render a response.</span></span> <span data-ttu-id="8108c-190">En iyi yöntem: Görünüm şablonları gereken **değil** iş mantığını gerçekleştirebilir veya bir veritabanı ile doğrudan etkileşim.</span><span class="sxs-lookup"><span data-stu-id="8108c-190">A best practice: View templates should **not** perform business logic or interact with a database directly.</span></span> <span data-ttu-id="8108c-191">Bunun yerine, bir şablonu görüntüleme için denetleyici tarafından sağlanan veri ile çalışması gerekir.</span><span class="sxs-lookup"><span data-stu-id="8108c-191">Rather, a view template should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="8108c-192">Bu "ayrımı" bakımı, temiz, sınanabilir ve sürdürülebilir Kod tutmaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="8108c-192">Maintaining this "separation of concerns" helps keep the code clean, testable, and maintainable.</span></span>

<span data-ttu-id="8108c-193">Şu anda `Welcome` yönteminde `HelloWorldController` sınıfı alır bir `name` ve `ID` parametresi ve çıkışları doğrudan tarayıcıya değerleri.</span><span class="sxs-lookup"><span data-stu-id="8108c-193">Currently, the `Welcome` method in the `HelloWorldController` class takes a `name` and a `ID` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="8108c-194">Bu yanıt dize olarak işleme denetleyiciniz yerine bir şablonu görüntüleme kullanmayı denetleyici değiştirin.</span><span class="sxs-lookup"><span data-stu-id="8108c-194">Rather than have the controller render this response as a string, change the controller to use a view template instead.</span></span> <span data-ttu-id="8108c-195">Şablonu Görüntüle uygun veri bitleri denetleyiciden görünüm yanıtı oluşturmak için geçirilmesi gerektiğini yani dinamik bir yanıt oluşturur.</span><span class="sxs-lookup"><span data-stu-id="8108c-195">The view template generates a dynamic response, which means that appropriate bits of data must be passed from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="8108c-196">Şablonu görüntüleme, gereken dinamik (Parametreler) verilerinizden denetleyicisi sağlayarak bunu bir `ViewData` görünüm şablonu erişebiliyorsa sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="8108c-196">Do this by having the controller put the dynamic data (parameters) that the view template needs in a `ViewData` dictionary that the view template can then access.</span></span>

<span data-ttu-id="8108c-197">İçinde *HelloWorldController.cs*, değiştirme `Welcome` yöntemi eklemek için bir `Message` ve `NumTimes` değerini `ViewData` sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="8108c-197">In *HelloWorldController.cs*, change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewData` dictionary.</span></span> <span data-ttu-id="8108c-198">`ViewData` Sözlüktür her türlü kullanılabilir; yani dinamik bir nesne `ViewData` nesnesi içine yerleştirin kadar tanımlı hiçbir özellik sahiptir.</span><span class="sxs-lookup"><span data-stu-id="8108c-198">The `ViewData` dictionary is a dynamic object, which means any type can be used; the `ViewData` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="8108c-199">[MVC model bağlama sistemi](xref:mvc/models/model-binding) adlandırılmış parametreleri otomatik olarak eşlenir (`name` ve `numTimes`) Sorgu dizesinden yönteminizi parametrelere adres çubuğundaki.</span><span class="sxs-lookup"><span data-stu-id="8108c-199">The [MVC model binding system](xref:mvc/models/model-binding) automatically maps the named parameters (`name` and `numTimes`) from the query string in the address bar to parameters in your method.</span></span> <span data-ttu-id="8108c-200">Tam *HelloWorldController.cs* dosya şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="8108c-200">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/HelloWorldController.cs?name=snippet_5)]

<span data-ttu-id="8108c-201">`ViewData` Sözlük nesnesi, görünüme iletilen verileri içerir.</span><span class="sxs-lookup"><span data-stu-id="8108c-201">The `ViewData` dictionary object contains data that will be passed to the view.</span></span>

<span data-ttu-id="8108c-202">Adlı bir Hoş Geldiniz görünüm şablonu oluşturma *Views/HelloWorld/Welcome.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="8108c-202">Create a Welcome view template named *Views/HelloWorld/Welcome.cshtml*.</span></span>

<span data-ttu-id="8108c-203">Bir döngüde oluşturacaksınız *Welcome.cshtml* "Hello" görüntüleyen bir görünüm şablonu `NumTimes`.</span><span class="sxs-lookup"><span data-stu-id="8108c-203">You'll create a loop in the *Welcome.cshtml* view template that displays "Hello" `NumTimes`.</span></span> <span data-ttu-id="8108c-204">Öğesinin içeriğini değiştirin *Views/HelloWorld/Welcome.cshtml* aşağıdaki:</span><span class="sxs-lookup"><span data-stu-id="8108c-204">Replace the contents of *Views/HelloWorld/Welcome.cshtml* with the following:</span></span>

[!code-html[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/HelloWorld/Welcome.cshtml)]

<span data-ttu-id="8108c-205">Yaptığınız değişiklikleri kaydedin ve aşağıdaki URL'ye gidin:</span><span class="sxs-lookup"><span data-stu-id="8108c-205">Save your changes and browse to the following URL:</span></span>

`https://localhost:xxxx/HelloWorld/Welcome?name=Rick&numtimes=4`

<span data-ttu-id="8108c-206">Verileri URL'den gerçekleştirilen ve denetleyicisi kullanarak geçirilen [MVC model bağlayıcı](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="8108c-206">Data is taken from the URL and passed to the controller using the [MVC model binder](xref:mvc/models/model-binding) .</span></span> <span data-ttu-id="8108c-207">Denetleyici verileri paketleri bir `ViewData` sözlük ve Görünüm nesnesi geçer.</span><span class="sxs-lookup"><span data-stu-id="8108c-207">The controller packages the data into a `ViewData` dictionary and passes that object to the view.</span></span> <span data-ttu-id="8108c-208">Görünümü ardından verilerin tarayıcıya HTML olarak işler.</span><span class="sxs-lookup"><span data-stu-id="8108c-208">The view then renders the data as HTML to the browser.</span></span>

![Hoş Geldiniz bir etiket ve Hello dört kez gösterilen Rick ifadesini gösteren gizlilik görünümü](~/tutorials/first-mvc-app/adding-view/_static/rick2.png)

<span data-ttu-id="8108c-210">Yukarıdaki örnekteki `ViewData` denetleyicisinden bir görünüme veri iletmek için kullanılan sözlüğü.</span><span class="sxs-lookup"><span data-stu-id="8108c-210">In the sample above, the `ViewData` dictionary was used to pass data from the controller to a view.</span></span> <span data-ttu-id="8108c-211">Öğreticinin sonraki bölümlerinde bir görünüm modeli, bir denetleyiciden bir görünüme veri iletmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="8108c-211">Later in the tutorial, a view model is used to pass data from a controller to a view.</span></span> <span data-ttu-id="8108c-212">Veri geçirme görünüm modeli yaklaşım genellikle çok tercih üzerinden `ViewData` sözlük yaklaşım.</span><span class="sxs-lookup"><span data-stu-id="8108c-212">The view model approach to passing data is generally much preferred over the `ViewData` dictionary approach.</span></span> <span data-ttu-id="8108c-213">Bkz: [ViewBag, ViewData veya TempData kullanıldığı durumlar](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="8108c-213">See [When to use ViewBag, ViewData, or TempData](https://www.rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp-net-mvc-3-applications/) for more information.</span></span>

<span data-ttu-id="8108c-214">Sonraki öğreticide, filmler, bir veritabanı oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="8108c-214">In the next tutorial, a database of movies is created.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8108c-215">[Önceki](adding-controller.md)
> [İleri](adding-model.md)</span><span class="sxs-lookup"><span data-stu-id="8108c-215">[Previous](adding-controller.md)
[Next](adding-model.md)</span></span>
