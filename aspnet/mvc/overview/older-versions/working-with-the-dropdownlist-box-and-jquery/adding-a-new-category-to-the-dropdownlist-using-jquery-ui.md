---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: JQuery kullanıcı arabirimini kullanarak DropDownList'e yeni kategori ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: e2528b74b714a3f691b07ed2429b3fe9eb3c2074
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802532"
---
<a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a><span data-ttu-id="2d687-102">JQuery kullanıcı arabirimini kullanarak DropDownList'e yeni kategori ekleme</span><span class="sxs-lookup"><span data-stu-id="2d687-102">Adding a New Category to the DropDownList using jQuery UI</span></span>
====================
<span data-ttu-id="2d687-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2d687-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="2d687-104">HTML `Select` etikettir sabit kategori veri listesini göstermek için ideal, ancak çoğu zaman, yeni bir kategori eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2d687-104">The HTML `Select` tag is ideal for presenting a list of fixed category data, but often times you need to add a new category.</span></span> <span data-ttu-id="2d687-105">"Opera" tarzı veritabanımızdaki kategorileri eklemek istediğimiz varsayın.</span><span class="sxs-lookup"><span data-stu-id="2d687-105">Suppose we want to add the genre "Opera" to the categories in our database?</span></span> <span data-ttu-id="2d687-106">Bu bölümde, yeni kategori eklemek için kullanabiliriz bir iletişim kutusu eklemek için jQuery kullanıcı Arabirimi kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="2d687-106">In this section, we will use jQuery UI to add a dialog box we can use to add a new category.</span></span> <span data-ttu-id="2d687-107">Aşağıdaki görüntüde, UI tarayıcıda nasıl sunacaktır gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2d687-107">The image below shows how the UI will present in the browser.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

<span data-ttu-id="2d687-108">Bir kullanıcı seçtiğinde **ekleme yeni Tarz** bağlantı, bir açılır iletişim kutusu isteyen kullanıcı için yeni bir türe adı (ve isteğe bağlı olarak bir açıklama).</span><span class="sxs-lookup"><span data-stu-id="2d687-108">When a user selects the **Add New Genre** link, a pop-up dialog box prompts the user for a new genre name (and optionally a description).</span></span> <span data-ttu-id="2d687-109">Show aşağıdaki resim **ekleme Tarz** açılır iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="2d687-109">The image below show the **Add Genre** pop-up dialog.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

<span data-ttu-id="2d687-110">Ne zaman yeni bir türe adı girildikten ve **Kaydet** düğmesi gönderildi, aşağıdakiler gerçekleşir:</span><span class="sxs-lookup"><span data-stu-id="2d687-110">When a new genre name is entered and the **Save** button is pushed, the following happens:</span></span>

1. <span data-ttu-id="2d687-111">AJAX çağrısı yeni Tarz veritabanına kaydeder ve yeni Tarz bilgileri (Tarz adı ve kimliği) olarak JSON döndüren Tarz denetleyicisinin, oluşturma yöntemine veri gönderir.</span><span class="sxs-lookup"><span data-stu-id="2d687-111">An AJAX call posts the data to the Create method of the Genre Controller, which saves the new genre to the database and returns the new genre information (genre name and ID) as JSON.</span></span>
2. <span data-ttu-id="2d687-112">JavaScript yeni Tarz veri seçim listesine ekler.</span><span class="sxs-lookup"><span data-stu-id="2d687-112">JavaScript adds the new genre data to the select list.</span></span>
3. <span data-ttu-id="2d687-113">JavaScript yeni Tarz seçili öğe yapar.</span><span class="sxs-lookup"><span data-stu-id="2d687-113">JavaScript makes the new genre the selected item.</span></span>

   <span data-ttu-id="2d687-114">Aşağıdaki görüntüde **Opera** veritabanına eklenen ve seçili **Tarz** açılan liste.</span><span class="sxs-lookup"><span data-stu-id="2d687-114">In the image below, **Opera** was added to the database and selected in the **Genre** drop down list.</span></span> 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

<span data-ttu-id="2d687-115">Açık *Views\StoreManager\Create.cshtml* Tarz biçimlendirme aşağıdakiyle değiştirin ve dosya aşağıdaki kodu:</span><span class="sxs-lookup"><span data-stu-id="2d687-115">Open the *Views\StoreManager\Create.cshtml* file and replace the genre markup with the following the following code:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

<span data-ttu-id="2d687-116">`_ChooseGenre` Kısmi görünüm JavaScript ve jQuery Ekle yeni Tarz özelliği uygulamak için kullanılan kanca sağlayan tüm mantığı içerir.</span><span class="sxs-lookup"><span data-stu-id="2d687-116">The `_ChooseGenre` partial view will contain all the logic to hook up the JavaScript and jQuery used to implement the add new genre feature.</span></span> <span data-ttu-id="2d687-117">Biz sanatçının kullanıcı Arabirimi ile aynı yapmak basit kod tamamladıktan sonra.</span><span class="sxs-lookup"><span data-stu-id="2d687-117">Once we have completed the code it will be simple to do the same with the artist UI.</span></span>

<span data-ttu-id="2d687-118">Çözüm Gezgini'nde sağ tıklayın *Views\StoreManager* klasörü ve select **Ekle**, ardından **görünümü**.</span><span class="sxs-lookup"><span data-stu-id="2d687-118">In Solution Explorer, right click the *Views\StoreManager* folder and select **Add**, then **View**.</span></span> <span data-ttu-id="2d687-119">İçinde **Görünüm adı** giriş, girin `_ChooseGenre` seçip **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="2d687-119">In the **View name** input, enter `_ChooseGenre` then select **Add**.</span></span> <span data-ttu-id="2d687-120">Biçimlendirmeyi Değiştir *Views\StoreManager\\_ChooseGenre.cshtml* aşağıdaki dosya:</span><span class="sxs-lookup"><span data-stu-id="2d687-120">Replace the markup in the *Views\StoreManager\\_ChooseGenre.cshtml* file with the following:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

<span data-ttu-id="2d687-121">İlk satır içinde geçiriyoruz olduğunu bildirir. bir `Album` modelimizi tam olarak aynı model oluşturma Görünümü'nde bulunan deyiminin.</span><span class="sxs-lookup"><span data-stu-id="2d687-121">The first line declares that we are passing in an `Album` as our model, exactly the same model statement found in the Create view.</span></span> <span data-ttu-id="2d687-122">Sonraki birkaç satırlar **etiket** Yardımcısı biçimlendirme.</span><span class="sxs-lookup"><span data-stu-id="2d687-122">The next few lines are the **Label** helper markup.</span></span> <span data-ttu-id="2d687-123">Sonraki satırı **DropDownList** Yardımcısı çağrısı, tam olarak aynı özgün Oluştur görünümünün olduğu gibi.</span><span class="sxs-lookup"><span data-stu-id="2d687-123">The next line is the **DropDownList** helper call, exactly the same as in the original Create view.</span></span> <span data-ttu-id="2d687-124">Sonraki satıra ada sahip bir bağlantı ekler `Add New Genre`, ve gibi bir düğme stilleri.</span><span class="sxs-lookup"><span data-stu-id="2d687-124">The next line adds a link with the name `Add New Genre`, and styles it like a button.</span></span> <span data-ttu-id="2d687-125">İçeren satırda `ValidationMessageFor` doğrudan Oluştur görünümünden kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="2d687-125">The line containing `ValidationMessageFor` is copied directly from the Create view.</span></span> <span data-ttu-id="2d687-126">Aşağıdaki satırları:</span><span class="sxs-lookup"><span data-stu-id="2d687-126">The following lines:</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

<span data-ttu-id="2d687-127">Gizli bir div kimliği ile oluşturur `genreDialog`.</span><span class="sxs-lookup"><span data-stu-id="2d687-127">creates a hidden div, with the ID of `genreDialog`.</span></span> <span data-ttu-id="2d687-128">Bağlama için jQuery kullanacağız bizim **ekleme Tarz** Kimliğine sahip iletişim kutusu `genreDialog` bu DIV içinde</span><span class="sxs-lookup"><span data-stu-id="2d687-128">We will use jQuery to hook up our **Add Genre** dialog box with the ID `genreDialog` in this div.</span></span> <span data-ttu-id="2d687-129">Son iki komut dosyası etiketlerini Ekle yeni Tarz özelliği uygulamak için kullanacağız JavaScript dosyalarının bağlantılarını içerir.</span><span class="sxs-lookup"><span data-stu-id="2d687-129">The last two script tags contain links to the JavaScript files we will use to implement the add new genre feature.</span></span> <span data-ttu-id="2d687-130">*/Scripts/chooseGenre.js* dosyasıdır sağlanan, projedeki bu öğreticinin ilerleyen bölümlerinde inceleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="2d687-130">The */Scripts/chooseGenre.js* file is provided for you in the project, we will examine it later in the tutorial.</span></span>

<span data-ttu-id="2d687-131">Uygulamayı çalıştırmak ve tıklayın **ekleme yeni Tarz** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2d687-131">Run the application and click on the **Add New Genre** button.</span></span> <span data-ttu-id="2d687-132">İçinde **ekleme Tarz** iletişim kutusuna **Opera** içinde **adı** giriş kutusu.</span><span class="sxs-lookup"><span data-stu-id="2d687-132">In the **Add Genre** dialog box, enter **Opera** in the **Name** input box.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

<span data-ttu-id="2d687-133">Tıklayın **Kaydet** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2d687-133">Click the **Save** button.</span></span> <span data-ttu-id="2d687-134">AJAX çağrısı Opera kategorisi oluşturur ve ardından açılır listeden Opera ile doldurur ve Opera seçilen türe ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2d687-134">An AJAX call creates the Opera category and then populates the dropdown list with Opera, and sets Opera as the selected genre.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

<span data-ttu-id="2d687-135">Bir sanatçının, başlık ve fiyat girin ve ardından **Oluştur** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2d687-135">Enter an artist, title and price, then select the **Create** button.</span></span> <span data-ttu-id="2d687-136">$8.99 değerinden küçük bir fiyat girerseniz, yeni albümü Index görünümünü üst kısmında görünür.</span><span class="sxs-lookup"><span data-stu-id="2d687-136">If you enter a price less than $8.99, the new album will appear at the top of the Index view.</span></span> <span data-ttu-id="2d687-137">Yeni albüm girdiyi veritabanında kaydedilmiş doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="2d687-137">Verify the new album entry was saved in the database.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

<span data-ttu-id="2d687-138">Yalnızca bir harf ile yeni bir türe oluşturmayı deneyin.</span><span class="sxs-lookup"><span data-stu-id="2d687-138">Try creating a new genre with only one letter.</span></span> <span data-ttu-id="2d687-139">Aşağıdaki kod içinde *Models\Genre.cs* dosya Tarz adı minimum ve maksimum uzunluğunu ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2d687-139">The following code in the *Models\Genre.cs* file sets the minimum and maximum length of the genre name.</span></span>

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

<span data-ttu-id="2d687-140">İstemci tarafı doğrulama 2 ile 20 karakter arasında bir dize girin bildirir.</span><span class="sxs-lookup"><span data-stu-id="2d687-140">Client side validation reports you must enter a string between 2 and 20 characters.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a><span data-ttu-id="2d687-141">Nasıl bir yeni Tarz inceleme, veritabanı ve Select LIST eklenir.</span><span class="sxs-lookup"><span data-stu-id="2d687-141">Examining How a New Genre is Added to the Database and the Select List.</span></span>

<span data-ttu-id="2d687-142">Açık *Scripts\chooseGenre.js* dosya ve kodu inceleyin.</span><span class="sxs-lookup"><span data-stu-id="2d687-142">Open the *Scripts\chooseGenre.js* file and examine the code.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

<span data-ttu-id="2d687-143">İkinci satır kimliği kullanan `genreDialog` div etiketinde bir iletişim kutusu oluşturmak için *Views\StoreManager\\_ChooseGenre.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="2d687-143">The second line uses the ID `genreDialog` to create a dialog box on the div tag in the *Views\StoreManager\\_ChooseGenre.cshtml* file.</span></span> <span data-ttu-id="2d687-144">Adlandırılmış parametreler kendini açıklayıcı çoğu.</span><span class="sxs-lookup"><span data-stu-id="2d687-144">Most of the named parameters are self explanatory.</span></span> <span data-ttu-id="2d687-145">`autoOpen` Parametresini false olarak ayarlayın seçerek **oluşturma Tarz** düğmesi açılır iletişim kutusu açıkça (ikinci üzerinde açıklanmıştır).</span><span class="sxs-lookup"><span data-stu-id="2d687-145">The `autoOpen` parameter is set to false, selecting the **Create Genre** button will open the dialogue explicitly (this is described latter on).</span></span> <span data-ttu-id="2d687-146">İletişim kutusunun iki düğme var **Kaydet** ve **iptal**.</span><span class="sxs-lookup"><span data-stu-id="2d687-146">The dialog has two buttons, **Save** and **Cancel**.</span></span> <span data-ttu-id="2d687-147">**İptal** düğmesi iletişim kutusunu kapatır.</span><span class="sxs-lookup"><span data-stu-id="2d687-147">The **Cancel** button closes the dialog.</span></span> <span data-ttu-id="2d687-148">Aşağıdaki kodda gösterildiği **Kaydet** düğmesini işlevi.</span><span class="sxs-lookup"><span data-stu-id="2d687-148">The following code shows the **Save** button function.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

<span data-ttu-id="2d687-149">`var createGenreForm` Seçilir `createGenreForm` kimliği</span><span class="sxs-lookup"><span data-stu-id="2d687-149">The `var createGenreForm` is selected from the `createGenreForm` ID.</span></span> <span data-ttu-id="2d687-150">`createGenreForm` Kimliği, aşağıdaki kodda bulunan ayarlandığı *Views\Genre\\_CreateGenre.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="2d687-150">The `createGenreForm` ID was set in the following code found in the *Views\Genre\\_CreateGenre.cshtml* file.</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

<span data-ttu-id="2d687-151">[Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) Yardımcısı aşırı kullanılan *Views\Genre\\_CreateGenre.cshtml* dosyası HTML form gönderme URL'sini içeren bir eylem özniteliğiyle oluşturur.</span><span class="sxs-lookup"><span data-stu-id="2d687-151">The [Html.BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) helper overload used in the *Views\Genre\\_CreateGenre.cshtml* file generates HTML with an action attribute containing the URL to submit the form.</span></span> <span data-ttu-id="2d687-152">Bir tarayıcıda Oluştur albüm sayfası görüntüleme ve tarayıcıda göster kaynak seçerek görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d687-152">You can see this by displaying the create album page in a browser and selecting show source in the browser.</span></span> <span data-ttu-id="2d687-153">Aşağıdaki biçimlendirmede oluşturulan HTML form etiketi bulunduğu gösterir.</span><span class="sxs-lookup"><span data-stu-id="2d687-153">The following markup shows the generated HTML containing the form tag.</span></span>

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

<span data-ttu-id="2d687-154">JQuery `$.post` satır eylem özniteliği için bir AJAX çağrısı yapar (`/StoreManager/Create`) ve verileri geçirir **oluşturma Tarz** iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="2d687-154">The jQuery `$.post` line makes an AJAX call to the action attribute (`/StoreManager/Create`) and passes in the data from the **Create Genre** dialog box.</span></span> <span data-ttu-id="2d687-155">Verileri yeni türe ve isteğe bağlı bir açıklama için ad oluşur.</span><span class="sxs-lookup"><span data-stu-id="2d687-155">The data consists of the name for the new genre and an optional description.</span></span> <span data-ttu-id="2d687-156">AJAX çağrısı başarılı olursa yeni Tarz ad ve değer seçin işaretlemede eklenir ve yeni Tarz seçili değerine ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="2d687-156">If the AJAX call is successful, the new genre name and value are added to the Select markup, and the new genre is set to the selected value.</span></span> <span data-ttu-id="2d687-157">Bu, dinamik olarak üretilen biçimlendirme olduğu için tarayıcıda kaynak görüntüleyerek yeni seçin seçeneğini göremiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="2d687-157">Because this is dynamically generated markup, you can't see the new select option by viewing the source in the browser.</span></span> <span data-ttu-id="2d687-158">Yeni HTML IE 9 F12 Geliştirici araçlarıyla görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d687-158">You can see the new HTML with the IE 9 F12 developer tools.</span></span> <span data-ttu-id="2d687-159">Yeni seçme seçeneği, Internet Explorer 9'da görüntülemek için F12 Geliştirici Araçları'nı başlatmak için F12 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="2d687-159">To view the new select option, in Internet Explorer 9, hit the F12 key to start the F12 developer tools.</span></span> <span data-ttu-id="2d687-160">Oluşturma sayfasına gidin ve yeni Tarz Tarz seçim listesinde seçili olmaması için yeni bir türe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="2d687-160">Navigate to the Create page and add a new genre so the new genre is selected in the genre select list.</span></span> <span data-ttu-id="2d687-161">F12 geliştirici araçları:</span><span class="sxs-lookup"><span data-stu-id="2d687-161">In the F12 developer tools:</span></span>

1. <span data-ttu-id="2d687-162">HTML sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="2d687-162">Select the HTML tab.</span></span>
2. <span data-ttu-id="2d687-163">Yenile simgesine basın.</span><span class="sxs-lookup"><span data-stu-id="2d687-163">Hit the refresh icon.</span></span>  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. <span data-ttu-id="2d687-164">Arama kutusuna GenreID girin.</span><span class="sxs-lookup"><span data-stu-id="2d687-164">In the search box, enter GenreID.</span></span>
4. <span data-ttu-id="2d687-165">Sonraki simgeyi kullanarak</span><span class="sxs-lookup"><span data-stu-id="2d687-165">Using the next icon,</span></span>   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   <span data-ttu-id="2d687-166">Aşağıdaki select etiketine gidin:</span><span class="sxs-lookup"><span data-stu-id="2d687-166">navigate to the following select tag:</span></span>

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. <span data-ttu-id="2d687-167">Son seçenek değerini genişletin.</span><span class="sxs-lookup"><span data-stu-id="2d687-167">Expand the last option value.</span></span>

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

<span data-ttu-id="2d687-168">Aşağıdaki kod *Scripts\chooseGenre.js* dosyasını nasıl gösterir **yeni Tarz Ekle** düğme tıklama olayı için bağlı ve nasıl **ekleme yeni Tarz** iletişim kutusu oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="2d687-168">The following code in the *Scripts\chooseGenre.js* file shows the how the **Add New Genre** button gets connected to the click event, and how the **Add New Genre** dialog box is created.</span></span>

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

<span data-ttu-id="2d687-169">Bağlı bir tıklatma işlevi ilk satırı oluşturur **ekleme yeni Tarz** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2d687-169">The first line creates a click function attached to the **Add New Genre** button.</span></span> <span data-ttu-id="2d687-170">Aşağıdaki Views\StoreManager biçimlendirmeden\\_ChooseGenre.cshtml dosya gösterir nasıl **yeni Tarz ekleme** düğmesi oluşturulur:</span><span class="sxs-lookup"><span data-stu-id="2d687-170">The following markup from the Views\StoreManager\\_ChooseGenre.cshtml file shows how the **Add New Genre** button is created:</span></span>

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

<span data-ttu-id="2d687-171">Load yöntemi oluşturur ve Tarz Ekle iletişim kutusu açılır ve jQuery çağırır `parse` yöntemi için istemci doğrulama iletişim kutusuna girilen veriler üzerinde gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="2d687-171">The load method creates and opens the Add Genre dialog and calls the jQuery `parse` method so client validation occurs on data entered in the dialog.</span></span>

<span data-ttu-id="2d687-172">Bu bölümde yeni kategori verileri bir seçim listesine eklemek için kullanılan bir iletişim kutusu oluşturulacağını öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="2d687-172">In this section you have learned how to create a dialog that can be used to add new category data to a select list.</span></span> <span data-ttu-id="2d687-173">Yeni bir sanatçının sanatçının seçim listesine eklemek için kullanıcı Arabirimi oluşturmak için aynı yordamı izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2d687-173">You can follow the same procedure to create UI to add a new artist to the artist select list.</span></span> <span data-ttu-id="2d687-174">Bu öğreticide, ASP.NET MVC HTML Yardımcısı ile çalışmaya genel bir bakış verdiği **DropDownList**.</span><span class="sxs-lookup"><span data-stu-id="2d687-174">This tutorial has given an overview of working with the ASP.NET MVC HTML helper **DropDownList**.</span></span> <span data-ttu-id="2d687-175">İle çalışma hakkında ek bilgi **DropDownList**, aşağıdaki ek başvurular bölümüne bakın.</span><span class="sxs-lookup"><span data-stu-id="2d687-175">For additional information on working with the **DropDownList**, see the addition references section below.</span></span> <span data-ttu-id="2d687-176">Lütfen Bu öğretici yararlı olmadığını bize bildirin.</span><span class="sxs-lookup"><span data-stu-id="2d687-176">Please let us know if this tutorial has been helpful.</span></span>

<span data-ttu-id="2d687-177">Rick.Anderson[at]Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="2d687-177">Rick.Anderson[at]Microsoft.com</span></span>

### <a name="additional-references"></a><span data-ttu-id="2d687-178">Ek başvurular</span><span class="sxs-lookup"><span data-stu-id="2d687-178">Additional References</span></span>

- <span data-ttu-id="2d687-179">[ASP.NET MVC – basamaklı açılır listeler öğretici](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) tarafından [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span><span class="sxs-lookup"><span data-stu-id="2d687-179">[ASP.NET MVC–Cascading Dropdown Lists Tutorial](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) by [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)</span></span>
- <span data-ttu-id="2d687-180">[Seçilen](http://harvesthq.github.com/chosen/) çoklu seçim ve filtreleme destekleyen bir JavaScript eklentisi.</span><span class="sxs-lookup"><span data-stu-id="2d687-180">[Chosen](http://harvesthq.github.com/chosen/) A JavaScript plugin that support multi-select and filtering.</span></span>

### <a name="contributors"></a><span data-ttu-id="2d687-181">Katkıda Bulunanlar</span><span class="sxs-lookup"><span data-stu-id="2d687-181">Contributors</span></span>

- [<span data-ttu-id="2d687-182">Radu Enuca</span><span class="sxs-lookup"><span data-stu-id="2d687-182">Radu Enuca</span></span>](https://weblogs.asp.net/raduenuca/default.aspx)
- <span data-ttu-id="2d687-183">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="2d687-183">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="2d687-184">Brad Wilson '</span><span class="sxs-lookup"><span data-stu-id="2d687-184">Brad Wilson</span></span>](http://bradwilson.typepad.com/)

### <a name="reviewers"></a><span data-ttu-id="2d687-185">Gözden geçirenler</span><span class="sxs-lookup"><span data-stu-id="2d687-185">Reviewers</span></span>

- <span data-ttu-id="2d687-186">Jean Sébastien Goupil</span><span class="sxs-lookup"><span data-stu-id="2d687-186">Jean-Sébastien Goupil</span></span>
- [<span data-ttu-id="2d687-187">Brad Wilson '</span><span class="sxs-lookup"><span data-stu-id="2d687-187">Brad Wilson</span></span>](http://bradwilson.typepad.com/)
- <span data-ttu-id="2d687-188">Mike Pope</span><span class="sxs-lookup"><span data-stu-id="2d687-188">Mike Pope</span></span>
- <span data-ttu-id="2d687-189">Tom Dykstra</span><span class="sxs-lookup"><span data-stu-id="2d687-189">Tom Dykstra</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2d687-190">Önceki</span><span class="sxs-lookup"><span data-stu-id="2d687-190">Previous</span></span>](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
