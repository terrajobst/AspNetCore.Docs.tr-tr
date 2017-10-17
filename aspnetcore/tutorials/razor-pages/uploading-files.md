---
title: "ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme"
author: guardrex
description: "Bir Razor sayfasına dosyaları karşıya yükleme hakkında bilgi edinin."
keywords: "ASP.NET Core, Razor, Razor sayfalarının, IFormFile, karşıya dosya yükleme, dosya yükleme"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3c5841f8c623f09530b60cc9997281dcb8e3c4f6
ms.sourcegitcommit: 94b7e0f95b92c98b182a93d2b3dc0287e5f97976
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="4a655-104">ASP.NET Core bir Razor sayfasına dosyaları karşıya yükleme</span><span class="sxs-lookup"><span data-stu-id="4a655-104">Uploading files to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="4a655-105">Tarafından [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="4a655-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="4a655-106">Bu bölümde, Razor sayfasını içeren dosyaları karşıya yükleme gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4a655-106">In this section, uploading files with a Razor Page is demonstrated.</span></span>

<span data-ttu-id="4a655-107">[Razor sayfalarının film örnek uygulaması](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) dosyaları karşıya yükleme bağlama Bu öğretici kullanan basit modelde çalıştığı iyi küçük dosyaları yüklemek için.</span><span class="sxs-lookup"><span data-stu-id="4a655-107">The [Razor Pages Movie sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) in this tutorial uses simple model binding to upload files, which works well for uploading small files.</span></span> <span data-ttu-id="4a655-108">Büyük dosyaları akış hakkında daha fazla bilgi için bkz: [akış ile büyük dosyalar](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span><span class="sxs-lookup"><span data-stu-id="4a655-108">For information on streaming large files, see [Uploading large files with streaming](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).</span></span>

<span data-ttu-id="4a655-109">Aşağıdaki adımlarda örnek uygulama için bir filmi zamanlama dosya karşıya yükleme özelliği ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4a655-109">In the steps below, you add a movie schedule file upload feature to the sample app.</span></span> <span data-ttu-id="4a655-110">Bir filmi zamanlama tarafından temsil edilen bir `Schedule` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4a655-110">A movie schedule is represented by a `Schedule` class.</span></span> <span data-ttu-id="4a655-111">Sınıfı, zamanlama iki sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="4a655-111">The class includes two versions of the schedule.</span></span> <span data-ttu-id="4a655-112">Bir sürüm müşterilere sağlanan `PublicSchedule`.</span><span class="sxs-lookup"><span data-stu-id="4a655-112">One version is provided to customers, `PublicSchedule`.</span></span> <span data-ttu-id="4a655-113">Başka bir sürüm şirket çalışanlarının kullanılan `PrivateSchedule`.</span><span class="sxs-lookup"><span data-stu-id="4a655-113">The other version is used for company employees, `PrivateSchedule`.</span></span> <span data-ttu-id="4a655-114">Her bir sürümü ayrı bir dosya olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4a655-114">Each version is uploaded as a separate file.</span></span> <span data-ttu-id="4a655-115">Öğretici iki dosya yüklemeleriyle tek bir POST ile bir sayfadan sunucuya nasıl gerçekleştirileceğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="4a655-115">The tutorial demonstrates how to perform two file uploads from a page with a single POST to the server.</span></span>

## <a name="add-a-helper-method-to-upload-files"></a><span data-ttu-id="4a655-116">Dosyaları karşıya yüklemek için bir yardımcı yöntemi ekleyin</span><span class="sxs-lookup"><span data-stu-id="4a655-116">Add a helper method to upload files</span></span>

<span data-ttu-id="4a655-117">Karşıya yüklenen zamanlama dosyalarını işlemek için kod yinelemesinden kaçınmak için önce bir statik yardımcı yöntemi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4a655-117">To avoid code duplication for processing uploaded schedule files, add a static helper method first.</span></span> <span data-ttu-id="4a655-118">Oluşturma bir *yardımcı programları* uygulama klasöründe ve ekleme bir *FileHelpers.cs* dosya aşağıdaki içeriğe sahip.</span><span class="sxs-lookup"><span data-stu-id="4a655-118">Create a *Utilities* folder in the app and add a *FileHelpers.cs* file with the following content.</span></span> <span data-ttu-id="4a655-119">Yardımcı yöntemi `ProcessFormFile`, alan bir [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) ve [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) ve dosyanın boyutu ve içeriğini içeren bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="4a655-119">The helper method, `ProcessFormFile`, takes an [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) and [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) and returns a string containing the file's size and content.</span></span> <span data-ttu-id="4a655-120">İçerik türü ve uzunluğu denetlenir.</span><span class="sxs-lookup"><span data-stu-id="4a655-120">The content type and length are checked.</span></span> <span data-ttu-id="4a655-121">Dosya bir doğrulama denetimi geçmiyor, bir hata eklenen `ModelState`.</span><span class="sxs-lookup"><span data-stu-id="4a655-121">If the file doesn't pass a validation check, an error is added to the `ModelState`.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a><span data-ttu-id="4a655-122">Zamanlama sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="4a655-122">Add the Schedule class</span></span>

<span data-ttu-id="4a655-123">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="4a655-123">Right click the *Models* folder.</span></span> <span data-ttu-id="4a655-124">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="4a655-124">Select **Add** > **Class**.</span></span> <span data-ttu-id="4a655-125">Sınıf adını **zamanlama** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4a655-125">Name the class **Schedule** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

<span data-ttu-id="4a655-126">Sınıf kullandığı `Display` ve `DisplayFormat` kolay başlıkları ve zamanlama veri işlendiğinde biçimlendirme oluşturur özniteliklerini.</span><span class="sxs-lookup"><span data-stu-id="4a655-126">The class uses `Display` and `DisplayFormat` attributes, which produce friendly titles and formatting when the schedule data is rendered.</span></span>

## <a name="update-the-moviecontext"></a><span data-ttu-id="4a655-127">Güncelleştirme MovieContext</span><span class="sxs-lookup"><span data-stu-id="4a655-127">Update the MovieContext</span></span>

<span data-ttu-id="4a655-128">Belirtin bir `DbSet` içinde `MovieContext` (*Models/MovieContext.cs*) tabloları için:</span><span class="sxs-lookup"><span data-stu-id="4a655-128">Specify a `DbSet` in the `MovieContext` (*Models/MovieContext.cs*) for the schedules:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a><span data-ttu-id="4a655-129">Zamanlama tablo veritabanına ekleyin</span><span class="sxs-lookup"><span data-stu-id="4a655-129">Add the Schedule table to the database</span></span>

<span data-ttu-id="4a655-130">(PMC) Paket Yöneticisi konsolunu açın: **Araçları** > **NuGet Paket Yöneticisi** > **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="4a655-130">Open the Package Manger Console (PMC): **Tools** > **NuGet Package Manager** > **Package Manager Console**.</span></span>

![PMC menüsü](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="4a655-132">PMC aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="4a655-132">In the PMC, execute the following commands.</span></span> <span data-ttu-id="4a655-133">Bu komutlar ekleme bir `Schedule` veritabanı tablosuna:</span><span class="sxs-lookup"><span data-stu-id="4a655-133">These commands add a `Schedule` table to the database:</span></span>

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-fileupload-class"></a><span data-ttu-id="4a655-134">Dosya yükleme sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="4a655-134">Add a FileUpload class</span></span>

<span data-ttu-id="4a655-135">Ardından, eklemek bir `FileUpload` zamanlama verileri elde etmek için sayfaya bağlı sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4a655-135">Next, add a `FileUpload` class, which is bound to the page to obtain the schedule data.</span></span> <span data-ttu-id="4a655-136">Sağ tıklayın *modelleri* klasör.</span><span class="sxs-lookup"><span data-stu-id="4a655-136">Right click the *Models* folder.</span></span> <span data-ttu-id="4a655-137">Seçin **ekleme** > **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="4a655-137">Select **Add** > **Class**.</span></span> <span data-ttu-id="4a655-138">Sınıf adını **dosya yükleme** ve aşağıdaki özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4a655-138">Name the class **FileUpload** and add the following properties:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

<span data-ttu-id="4a655-139">Sınıfı, zamanlamanın başlık özelliğini ve her iki sürümü zamanlama için bir özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="4a655-139">The class has a property for the schedule's title and a property for each of the two versions of the schedule.</span></span> <span data-ttu-id="4a655-140">Tüm üç özellikleri gereklidir ve başlığı 3-60 karakter uzunluğunda olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="4a655-140">All three properties are required, and the title must be 3-60 characters long.</span></span>

## <a name="add-a-file-upload-razor-page"></a><span data-ttu-id="4a655-141">Bir dosyayı karşıya yükleme Razor sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="4a655-141">Add a file upload Razor Page</span></span>

<span data-ttu-id="4a655-142">İçinde *sayfaları* klasörü oluşturmak bir *zamanlamaları* klasör.</span><span class="sxs-lookup"><span data-stu-id="4a655-142">In the *Pages* folder, create a *Schedules* folder.</span></span> <span data-ttu-id="4a655-143">İçinde *zamanlamaları* klasörünü adlı bir sayfa oluşturma *Index.cshtml* aşağıdaki içeriğe sahip bir zamanlama karşıya yükleme için:</span><span class="sxs-lookup"><span data-stu-id="4a655-143">In the *Schedules* folder, create a page named *Index.cshtml* for uploading a schedule with the following content:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

<span data-ttu-id="4a655-144">Her form grubu içeren bir  **\<etiketi >** her sınıf özelliğinin adını görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4a655-144">Each form group includes a **\<label>** that displays the name of each class property.</span></span> <span data-ttu-id="4a655-145">`Display` Öznitelikleri `FileUpload` modeli etiketlerini görüntüleme değerleri belirtin.</span><span class="sxs-lookup"><span data-stu-id="4a655-145">The `Display` attributes in the `FileUpload` model provide the display values for the labels.</span></span> <span data-ttu-id="4a655-146">Örneğin, `UploadPublicSchedule` özelliğin görünen adı ayarlandığında `[Display(Name="Public Schedule")]` ve formu işleyen böylece "Ortak zamanlama" etiketi görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4a655-146">For example, the `UploadPublicSchedule` property's display name is set with `[Display(Name="Public Schedule")]` and thus displays "Public Schedule" in the label when the form renders.</span></span>

<span data-ttu-id="4a655-147">Her form grubu bir doğrulama içeren  **\<span >**.</span><span class="sxs-lookup"><span data-stu-id="4a655-147">Each form group includes a validation **\<span>**.</span></span> <span data-ttu-id="4a655-148">Kullanıcı karşılamak üzere başarısız giriş olması durumunda özellik öznitelikleri kümesinde `FileUpload` sınıfı veya varsa `ProcessFormFile` yöntemi dosya doğrulama başarısız denetler, model doğrulamak başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="4a655-148">If the user's input fails to meet the property attributes set in the `FileUpload` class or if any of the `ProcessFormFile` method file validation checks fail, the model fails to validate.</span></span> <span data-ttu-id="4a655-149">Model doğrulama başarısız olduğunda, bir yardımcı doğrulama ileti kullanıcıya işlenir.</span><span class="sxs-lookup"><span data-stu-id="4a655-149">When model validation fails, a helpful validation message is rendered to the user.</span></span> <span data-ttu-id="4a655-150">Örneğin, `Title` özellik açıklama ile `[Required]` ve `[StringLength(60, MinimumLength = 3)]`.</span><span class="sxs-lookup"><span data-stu-id="4a655-150">For example, the `Title` property is annotated with `[Required]` and `[StringLength(60, MinimumLength = 3)]`.</span></span> <span data-ttu-id="4a655-151">Kullanıcı bir başlık sağlamanız başarısız olursa, bir değer gerekli olduğunu belirten bir ileti alırlar.</span><span class="sxs-lookup"><span data-stu-id="4a655-151">If the user fails to supply a title, they receive a message indicating that a value is required.</span></span> <span data-ttu-id="4a655-152">Kullanıcı değerinden üç veya daha fazla altmış karakter değeri girerse, bunlar değeri yanlış bir uzunluğa sahip olduğunu belirten bir ileti alırsınız.</span><span class="sxs-lookup"><span data-stu-id="4a655-152">If the user enters a value less than three characters or more than sixty characters, they receive a message indicating that the value has an incorrect length.</span></span> <span data-ttu-id="4a655-153">İçeriği yok sağlanan bir dosya varsa, dosyayı boş olduğunu belirten bir ileti görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4a655-153">If a file is provided that has no content, a message appears indicating that the file is empty.</span></span>

## <a name="add-the-code-behind-file"></a><span data-ttu-id="4a655-154">Arka plan kod dosyasına ekleyin</span><span class="sxs-lookup"><span data-stu-id="4a655-154">Add the code-behind file</span></span>

<span data-ttu-id="4a655-155">Arka plan kod dosyasına ekleyin (*Index.cshtml.cs*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="4a655-155">Add the code-behind file (*Index.cshtml.cs*) to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

<span data-ttu-id="4a655-156">Sayfa modeli (`IndexModel` içinde *Index.cshtml.cs*) bağlar `FileUpload` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="4a655-156">The page model (`IndexModel` in *Index.cshtml.cs*) binds the `FileUpload` class:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="4a655-157">Model zamanlamalar listesini de kullanır (`IList<Schedule>`) sayfasında veritabanında depolanan zamanlamaları görüntülemek için:</span><span class="sxs-lookup"><span data-stu-id="4a655-157">The model also uses a list of the schedules (`IList<Schedule>`) to display the schedules stored in the database on the page:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="4a655-158">Sayfa yüklediğinde ile `OnGetAsync`, `Schedules` veritabanından doldurulur ve yüklenen zamanlamalar HTML tablosu oluşturmak için kullanılan:</span><span class="sxs-lookup"><span data-stu-id="4a655-158">When the page loads with `OnGetAsync`, `Schedules` is populated from the database and used to generate an HTML table of loaded schedules:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

<span data-ttu-id="4a655-159">Sunucuya form gönderildiğinde `ModelState` denetlenir.</span><span class="sxs-lookup"><span data-stu-id="4a655-159">When the form is posted to the server, the `ModelState` is checked.</span></span> <span data-ttu-id="4a655-160">Geçersiz, `Schedule` yeniden oluşturulur ve sayfa doğrulamayı neden geçemediğini belirten bir veya daha fazla doğrulama iletileri ile sayfasını işler.</span><span class="sxs-lookup"><span data-stu-id="4a655-160">If invalid, `Schedule` is rebuilt, and the page renders with one or more validation messages stating why page validation failed.</span></span> <span data-ttu-id="4a655-161">Geçerliyse, `FileUpload` özellikleri kullanıldığı *OnPostAsync* zamanlama iki sürümleri için dosya karşıya yükleme işlemini tamamlamak için ve yeni bir oluşturmak için `Schedule` verileri depolamak için nesne.</span><span class="sxs-lookup"><span data-stu-id="4a655-161">If valid, the `FileUpload` properties are used in *OnPostAsync* to complete the file upload for the two versions of the schedule and to create a new `Schedule` object to store the data.</span></span> <span data-ttu-id="4a655-162">Zamanlama sonra veritabanına kaydedilir:</span><span class="sxs-lookup"><span data-stu-id="4a655-162">The schedule is then saved to the database:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a><span data-ttu-id="4a655-163">Dosya karşıya yükleme Razor sayfasını bağlantı</span><span class="sxs-lookup"><span data-stu-id="4a655-163">Link the file upload Razor Page</span></span>

<span data-ttu-id="4a655-164">Açık *_Layout.cshtml* ve bir bağlantı dosya karşıya yükleme sayfasına ulaşmak için gezinti çubuğu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4a655-164">Open *_Layout.cshtml* and add a link to the navigation bar to reach the file upload page:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a><span data-ttu-id="4a655-165">Zamanlama silmeyi onaylamak için bir sayfa ekleyin</span><span class="sxs-lookup"><span data-stu-id="4a655-165">Add a page to confirm schedule deletion</span></span>

<span data-ttu-id="4a655-166">Kullanıcı bir zamanlama silinecek tıkladığında, bunları işlemi iptal etmek için bir fırsat olmasını istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="4a655-166">When the user clicks to delete a schedule, you want them to have a chance to cancel the operation.</span></span> <span data-ttu-id="4a655-167">Silme onayı sayfası ekleme (*Delete.cshtml*) için *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="4a655-167">Add a delete confirmation page (*Delete.cshtml*) to the *Schedules* folder:</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

<span data-ttu-id="4a655-168">Arka plan kod dosyasına (*Delete.cshtml.cs*) tarafından tanımlanan tek bir zamanlama yükler `id` isteğin rota verilerindeki.</span><span class="sxs-lookup"><span data-stu-id="4a655-168">The code-behind file (*Delete.cshtml.cs*) loads a single schedule identified by `id` in the request's route data.</span></span> <span data-ttu-id="4a655-169">Ekleme *Delete.cshtml.cs* dosya *zamanlamaları* klasörü:</span><span class="sxs-lookup"><span data-stu-id="4a655-169">Add the *Delete.cshtml.cs* file to the *Schedules* folder:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

<span data-ttu-id="4a655-170">`OnPostAsync` Yöntemi işler tarafından zamanlama silinirken kendi `id`:</span><span class="sxs-lookup"><span data-stu-id="4a655-170">The `OnPostAsync` method handles deleting the schedule by its `id`:</span></span>

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

<span data-ttu-id="4a655-171">Zamanlama başarıyla sildikten sonra `RedirectToPage` zamanlamaları için kullanıcı gönderir *Index.cshtml* sayfası.</span><span class="sxs-lookup"><span data-stu-id="4a655-171">After successfully deleting the schedule, the `RedirectToPage` sends the user back to the schedules *Index.cshtml* page.</span></span>

## <a name="the-working-schedules-razor-page"></a><span data-ttu-id="4a655-172">Zamanlamalar Razor sayfasını çalışma</span><span class="sxs-lookup"><span data-stu-id="4a655-172">The working Schedules Razor Page</span></span>

<span data-ttu-id="4a655-173">Etiketleri ve girişleri zamanlama başlık, sayfa yüklendiğinde ortak zamanlama ve özel zamanlama bir gönderme düğmesi ile işlenir:</span><span class="sxs-lookup"><span data-stu-id="4a655-173">When the page loads, labels and inputs for schedule title, public schedule, and private schedule are rendered with a submit button:</span></span>

![Hiçbir doğrulama hataları ve boş alanları ile ilk yükü görülen Razor sayfasını zamanlar](uploading-files/_static/browser1.png)

<span data-ttu-id="4a655-175">Seçme **karşıya** tüm alanları doldurmak ihlal olmadan düğmesini `[Required]` model üzerinde öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="4a655-175">Selecting the **Upload** button without populating any of the fields violates the `[Required]` attributes on the model.</span></span> <span data-ttu-id="4a655-176">`ModelState` Geçersiz.</span><span class="sxs-lookup"><span data-stu-id="4a655-176">The `ModelState` is invalid.</span></span> <span data-ttu-id="4a655-177">Kullanıcı için bir doğrulama hata iletisi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="4a655-177">The validation error messages are displayed to the user:</span></span>

![Her giriş denetiminin yanındaki bir doğrulama hata iletisi görüntülenir](uploading-files/_static/browser2.png)

<span data-ttu-id="4a655-179">İki harf içine yazın **başlık** alan.</span><span class="sxs-lookup"><span data-stu-id="4a655-179">Type two letters into the **Title** field.</span></span> <span data-ttu-id="4a655-180">Başlık 3-60 karakter arasında olması gerektiğini belirtmek için doğrulama ileti değişiklikler:</span><span class="sxs-lookup"><span data-stu-id="4a655-180">The validation message changes to indicate that the title must be between 3-60 characters:</span></span>

![Başlık doğrulama ileti değiştirildi](uploading-files/_static/browser3.png)

<span data-ttu-id="4a655-182">Bir veya daha fazla Zamanlama yüklenirken **yüklenen zamanlamaları** bölüm yüklenen zamanlamaları işler:</span><span class="sxs-lookup"><span data-stu-id="4a655-182">When one or more schedules are uploaded, the **Loaded Schedules** section renders the loaded schedules:</span></span>

![Yüklenen zamanlamaları, her zamanlamanın başlık gösteren tablosunun tarihi UTC, genel bir sürümü dosya boyutu ve özel sürüm dosya boyutu karşıya](uploading-files/_static/browser4.png)

<span data-ttu-id="4a655-184">Kullanıcı tıklatabilirsiniz **silmek** buradan onaylayın ya da silme işlemini iptal etmek için bir fırsat sahip oldukları silme onayı görünüm ulaşmak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="4a655-184">The user can click the **Delete** link from there to reach the delete confirmation view, where they have an opportunity to confirm or cancel the delete operation.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="4a655-185">Sorun giderme</span><span class="sxs-lookup"><span data-stu-id="4a655-185">Troubleshooting</span></span>

<span data-ttu-id="4a655-186">Sorun giderme bilgileri ile `IFormFile` yüklemek bkz [dosya yüklemeleri ASP.NET Core: sorun giderme](xref:mvc/models/file-uploads#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="4a655-186">For troubleshooting information with `IFormFile` uploading, see the [File uploads in ASP.NET Core: Troubleshooting](xref:mvc/models/file-uploads#troubleshooting).</span></span>

<span data-ttu-id="4a655-187">Bu giriş Razor sayfalarının tamamlamak için teşekkür ederiz.</span><span class="sxs-lookup"><span data-stu-id="4a655-187">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="4a655-188">Çıkmadan açıklamaları veriyoruz.</span><span class="sxs-lookup"><span data-stu-id="4a655-188">We appreciate any comments you leave.</span></span> <span data-ttu-id="4a655-189">[MVC ve EF çekirdek Başlarken](xref:data/ef-mvc/intro) mükemmel bir izleme Bu öğretici kadar olan.</span><span class="sxs-lookup"><span data-stu-id="4a655-189">[Getting started with MVC and EF Core](xref:data/ef-mvc/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4a655-190">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4a655-190">Additional resources</span></span>

* [<span data-ttu-id="4a655-191">ASP.NET Core dosya yüklemeleri</span><span class="sxs-lookup"><span data-stu-id="4a655-191">File uploads in ASP.NET Core</span></span>](xref:mvc/models/file-uploads)
* [<span data-ttu-id="4a655-192">IFormFile</span><span class="sxs-lookup"><span data-stu-id="4a655-192">IFormFile</span></span>](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[<span data-ttu-id="4a655-193">Önceki: doğrulama</span><span class="sxs-lookup"><span data-stu-id="4a655-193">Previous: Validation</span></span>](xref:tutorials/razor-pages/validation)
