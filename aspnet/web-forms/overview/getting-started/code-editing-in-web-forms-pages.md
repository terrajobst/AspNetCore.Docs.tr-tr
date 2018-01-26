---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: "ASP.NET Web Forms Visual Studio 2013'te kod düzenleme | Microsoft Docs"
author: Erikre
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 8714f673cb0434189ca23d2dda14035d8652a051
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="4f7cc-102">Visual Studio 2013'te kod düzenleme ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="4f7cc-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="4f7cc-103">Tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="4f7cc-104">Birçok ASP.NET Web formu sayfalarında, Visual Basic, C# veya başka bir dil kodu yazın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="4f7cc-105">Visual Studio'da Kod Düzenleyicisi hataları önlemenize yardımcı olurken kod hızlı yazmanıza yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="4f7cc-106">Ayrıca, düzenleyici yapmanız gereken çalışma miktarını azaltmak için yeniden kullanılabilir kod oluşturma yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="4f7cc-107">Bu kılavuzda Visual Studio kod düzenleyicisinin çeşitli özellikleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="4f7cc-108">Bu gözden geçirme sırasında öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="4f7cc-109">Satır içi kodlama hataları düzeltin.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="4f7cc-110">Yeniden düzenlemeniz ve kod yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-110">Refactor and rename code.</span></span>
- <span data-ttu-id="4f7cc-111">Değişkenleri ve nesneleri yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-111">Rename variables and objects.</span></span>
- <span data-ttu-id="4f7cc-112">Kod parçacıkları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f7cc-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4f7cc-113">Prerequisites</span></span>


<span data-ttu-id="4f7cc-114">Bu kılavuzu tamamlamak için gerekir:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="4f7cc-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="4f7cc-116">.NET Framework otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="4f7cc-117">Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici seri anılacaktır.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="4f7cc-118">Visual Studio kullanıyorsanız, bu kılavuzda, seçtiğiniz varsayar **Web geliştirme** ayarlar koleksiyonu, Visual Studio ilk başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="4f7cc-119">Daha fazla bilgi için bkz: [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

 <span data-ttu-id="4f7cc-120">Visual Studio ve ASP.NET bir giriş için bkz: [Visual Studio 2013'te temel ASP.NET 4.5 Web Forms sayfası oluşturma](creating-a-basic-web-forms-page.md).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="4f7cc-121">Bir Web uygulaması projesi ve bir sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="4f7cc-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="4f7cc-122">Kılavuzun bu bölümünde, bir Web uygulaması projesi oluşturun ve yeni bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="4f7cc-123">Bir Web uygulaması projesi oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="4f7cc-123">To create a Web application project</span></span>

1. <span data-ttu-id="4f7cc-124">Microsoft Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="4f7cc-125">Üzerinde **dosya** menüsünde, select **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="4f7cc-126">![Dosya menüsü](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="4f7cc-127">**Yeni proje** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="4f7cc-128">Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki templates grubu.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="4f7cc-129">Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="4f7cc-130">Projenizin adı ***BasicWebApp*** tıklatıp **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="4f7cc-131">![Yeni Proje iletişim kutusu](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="4f7cc-132">Ardından, **Web Forms** şablonu ve tıklatın **Tamam** projesi oluşturmak için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="4f7cc-133">![Yeni ASP.NET projesi iletişim kutusu](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="4f7cc-134">Visual Studio Web Forms şablona dayalı önceden oluşturulmuş işlevselliği içeren yeni bir proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="4f7cc-135">Yeni bir ASP.NET Web Forms sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="4f7cc-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="4f7cc-136">Kullanarak yeni bir Web Forms uygulaması oluşturduğunuzda **ASP.NET Web uygulaması** proje şablonu, Visual Studio ekler adlı bir ASP.NET sayfası (Web Forms sayfası) *Default.aspx*, birkaç diğer dosyaların yanı sıra ve klasörler.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="4f7cc-137">Kullanabileceğiniz *Default.aspx* sayfası, Web uygulamanız için giriş sayfası olarak.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="4f7cc-138">Ancak, bu kılavuzda oluşturacak ve yeni bir sayfa ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="4f7cc-139">Sayfa Web uygulamasına eklemek için</span><span class="sxs-lookup"><span data-stu-id="4f7cc-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="4f7cc-140">İçinde **Çözüm Gezgini**, Web uygulamasının adını sağ tıklayın (uygulama adı bu öğreticideki **BasicWebSite**) ve ardından **Ekle**  - &gt; **Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="4f7cc-141">**Yeni Öğe Ekle** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="4f7cc-142">Seçin **Visual C#**  - &gt; **Web** soldaki templates grubu.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="4f7cc-143">Ardından, seçin **Web formu** ortasından listelemek ve adlandırın *FirstWebPage.aspx*.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="4f7cc-144">![Yeni Öğe Ekle iletişim kutusu](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="4f7cc-145">Tıklatın **Ekle** Web Forms sayfası projenize eklemek için.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="4f7cc-146">Visual Studio yeni sayfa oluşturur ve bunu açar.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="4f7cc-147">Ardından, bu yeni sayfa varsayılan başlangıç sayfası olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="4f7cc-148">İçinde **Çözüm Gezgini**, adlı yeni sayfa sağ *FirstWebPage.aspx* seçip **başlangıç sayfası olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="4f7cc-149">Bizim ilerleme test etmek için bu uygulamayı bir sonraki çalıştırmanızda bu yeni sayfa tarayıcıdaki otomatik olarak görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="4f7cc-150">Satır içi kodlama hataları düzeltme</span><span class="sxs-lookup"><span data-stu-id="4f7cc-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="4f7cc-151">Visual Studio Kod düzenleyicisinde, kod yazmak ve bir hata yaptıysanız, Kod düzenleyicisinde hatayı düzeltmek için yardımcı hatalarını önlemek için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="4f7cc-152">Kılavuzun bu bölümünde, bir satırı Düzenleyicisi'nde hata düzeltme özellikleri gösteren kod yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="4f7cc-153">Basit kodlama hataları düzeltmek için Visual Studio'da</span><span class="sxs-lookup"><span data-stu-id="4f7cc-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="4f7cc-154">İçinde **tasarım** görüntülemek için bir işleyici oluşturmak için boş sayfa çift **yük** sayfası için olay.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
<span data-ttu-id="4f7cc-155">Bazı kodlar yazmak için yalnızca bir yer olarak olay işleyicisi kullanıyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="4f7cc-156">İşleyicisinin içinden bir hata ve tuşuna içeren aşağıdaki satırdan **ENTER**:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

 <span data-ttu-id="4f7cc-157">Bastığınızda **ENTER**, Kod düzenleyicisinde yeşil ve kırmızı alt çizgileri yerleştirir (genellikle çağrısı &quot;dalgalı&quot; satırları) sorunları olan kodu alanlarını altında.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="4f7cc-158">Yeşil alt çizgi, bir uyarı gösterir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-158">A green underline indicates a warning.</span></span> <span data-ttu-id="4f7cc-159">Kırmızı alt çizgi düzeltmeniz gerekir bir hata gösterir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="4f7cc-160">Fare işaretçisini tutun `myStr` hakkında uyarı bildiren bir araç ipucu görmek için.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="4f7cc-161">Ayrıca, fare işaretçisini hata iletisini görmek için kırmızı altı çizili basılı tutun.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="4f7cc-162">Aşağıdaki resimde, alt çizgileri koduyla gösterir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="4f7cc-163">![Hoş Geldiniz metin Tasarım görünümünde](code-editing-in-web-forms-pages/_static/image5.png "Tasarım görünümünde metin'na Hoş Geldiniz")</span><span class="sxs-lookup"><span data-stu-id="4f7cc-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
 <span data-ttu-id="4f7cc-164">Noktalı virgül ekleyerek hatanın düzeltilmesi gerektiğini `;` satırın sonuna.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="4f7cc-165">Uyarı yalnızca size, kullanmadığınız bildirir `myStr` henüz değişkeni.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="4f7cc-166">Visual Studio'da ayarlar seçerek biçimlendirme geçerli kodunuzu görüntülemek **Araçları**  - &gt; **seçenekleri**  - &gt; **yazı tipleri ve Renkleri**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="4f7cc-167">Yeniden düzenleme ve yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="4f7cc-167">Refactoring and Renaming</span></span>

<span data-ttu-id="4f7cc-168">Yeniden düzenleme anlamak ve işlevselliğini korurken korumak için daha kolay hale getirmek için kodunuzu yeniden yapılandırma içeren bir yazılım Metodoloji olur.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="4f7cc-169">Basit bir örnek kod bir veritabanından veri almak için bir olay işleyicisi yazma olabilir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="4f7cc-170">Sayfanız geliştirdikçe birkaç farklı işleyicilerini veri erişim için gereken bulur.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="4f7cc-171">Bu nedenle, bir veri erişim yöntemi sayfasında oluşturarak ve yöntemine yönelik çağrılar işleyicileri ekleme sayfanın kodu yeniden.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="4f7cc-172">Kod Düzenleyicisi'ni yeniden düzenleme çeşitli görevleri gerçekleştirmenize yardımcı olacak araçlar içerir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="4f7cc-173">Bu kılavuzda, iki düzenleme teknikleri ile çalışır: değişkenleri yeniden adlandırma ve yöntemleri ayıklanıyor.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="4f7cc-174">Yeniden düzenleme diğer seçenekler alanları Kapsüllenen, yerel değişkenleri yöntemi parametrelerine yükseltme ve yöntem parametreleri yönetme içerir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="4f7cc-175">Yeniden düzenleme bu seçeneklerin kullanılabilirliği kodu konumda bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="4f7cc-176">Kodu yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="4f7cc-176">Refactoring Code</span></span>

<span data-ttu-id="4f7cc-177">Bir ortak yeniden düzenleme (extract) oluşturmak için bir senaryodur içinde bir yöntem gibi başka bir üye kodu yönteminden.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="4f7cc-178">Bu, özgün üye boyutunu azaltır ve ayıklanan kodu yeniden kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="4f7cc-179">Kılavuzun bu bölümünde, bazı basit kod yazın ve ardından bir yöntem ondan ayıklayın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="4f7cc-180">C# programlama dili kullanan bir sayfa oluşturacak şekilde yeniden düzenleme C# ' ta desteklenir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="4f7cc-181">Bir C# sayfasında bir yöntem ayıklamak için</span><span class="sxs-lookup"><span data-stu-id="4f7cc-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="4f7cc-182">Geçiş **tasarım** görünümü.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="4f7cc-183">İçinde **araç**, gelen **standart** sekmesinde, sürükleyin bir [düğmesini](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sayfaya denetim.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="4f7cc-184">Çift tıklatın **düğmesini** denetim için bir işleyici oluşturmak için kendi [tıklatın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay ve ardından aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

 <span data-ttu-id="4f7cc-185">Kod oluşturur bir **ArrayList** nesnesi değerlerle yüklemek için bir döngü kullanır ve içeriğini görüntülemek için başka bir döngü kullanır **ArrayList** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="4f7cc-186">Tuşuna **CTRL + F5** sayfayı çalıştırın ve ardından **düğmesini** aşağıdaki çıkış gördüğünüzden emin olmak için:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="4f7cc-187">Kod Düzenleyicisi'ne dönmek ve olay işleyicisini aşağıdaki satırları seçin.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="4f7cc-188">Seçime sağ tıklayın, **yeniden düzenlemeniz**ve ardından **ayıklama yöntemi**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="4f7cc-189">**Ayıklama yöntemi** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="4f7cc-190">İçinde **yeni yöntem adı** kutusuna **DisplayArray**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="4f7cc-191">Kod Düzenleyicisi adlı yeni bir yöntem oluşturur `DisplayArray`ve yeni yöntemine bir çağrı yerleştirir **tıklatın** döngü bulunduğu başlangıçta işleyici.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="4f7cc-192">Tuşuna **CTRL + F5** sayfa yeniden çalıştırmanız ve tıklatın **düğmesini**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="4f7cc-193">Önce yaptığınız gibi sayfa aynı çalışır.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-193">The page functions the same as it did before.</span></span> <span data-ttu-id="4f7cc-194">`DisplayArray` Yöntemi çağrısı yerden şimdi olabilir sayfa sınıfındaki.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="4f7cc-195">Değişkenleri yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="4f7cc-195">Renaming Variables</span></span>

<span data-ttu-id="4f7cc-196">Değişkenleri yanı sıra ile nesneleri çalışırken zaten kodunuzda başvurulan sonra bunları yeniden adlandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="4f7cc-197">Ancak, değişkenler ve nesneleri yeniden adlandırma başvurulardan birini yeniden adlandırma kaçırılması durumunda ayırmak kodu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="4f7cc-198">Bu nedenle, yeniden adlandırma gerçekleştirmek için yeniden düzenleme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="4f7cc-199">Bir değişken yeniden adlandırmak için yeniden düzenleme kullanmak için</span><span class="sxs-lookup"><span data-stu-id="4f7cc-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="4f7cc-200">İçinde **tıklatın** olay işleyicisi, aşağıdaki satırı bulun:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="4f7cc-201">Değişken adını sağ `alist`, seçin **yeniden düzenlemeniz**ve ardından **yeniden adlandırma**.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="4f7cc-202">**Yeniden adlandırma** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="4f7cc-203">İçinde **yeni bir ad** kutusuna **ArrayList1** ve emin olun **başvuru değişikliklerini Önizleme** onay kutusunun seçili.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="4f7cc-204">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-204">Then click **OK**.</span></span>

    <span data-ttu-id="4f7cc-205">**Değişiklikleri Önizle** iletişim kutusu belirir ve adlandırdığınız değişkeni tüm başvuruları içeren bir ağaç görüntüler.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="4f7cc-206">Tıklatın **Uygula** kapatmak için **Değişiklikleri Önizle** iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="4f7cc-207">Özellikle, seçtiğiniz örneğine başvuru değişkenleri yeniden adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="4f7cc-208">Ancak, unutmayın, değişkeni `alist` aşağıdaki satırda yeniden adlandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="4f7cc-209">Değişkeni `alist` değişkeni aynı değeri göstermiyor çünkü bu satırda yeniden adlandırılmış değil `alist` adlandırdığınız.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="4f7cc-210">Değişkeni `alist` içinde `DisplayArray` bu yöntem için yerel bir değişken bildirimidir.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="4f7cc-211">Bu değişkenleri yeniden adlandırmak için yeniden düzenleme kullanarak Bul ve Değiştir eylemi Düzenleyicisi'nde yalnızca gerçekleştirmeden daha farklı olduğunu gösterir; ile çalışma değişkeni semantiği bilgisini yeniden adlandırır değişkenlerle yeniden düzenleme.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="4f7cc-212">Kod parçacıkları ekleme</span><span class="sxs-lookup"><span data-stu-id="4f7cc-212">Inserting Snippets</span></span>

<span data-ttu-id="4f7cc-213">Web Forms geliştiriciler sık gerçekleştirmeniz gereken birçok kodlama görevleri olduğundan, Kod Düzenleyicisi parçacıkları ya da önceden kod bloklarını kitaplığını sağlar.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="4f7cc-214">Bu parçacıkları sayfanıza ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="4f7cc-215">Visual Studio'da kullandığınız her bir dilin kod parçacıkları Ekle şekilde küçük farklar vardır.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="4f7cc-216">Kod parçacıkları ekleme hakkında daha fazla bilgi için bkz: [Visual Basic IntelliSense kodu parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="4f7cc-217">Kod parçacıkları Visual C# dilinde ekleme hakkında daha fazla bilgi için bkz: [Visual C# kod parçacıkları](https://msdn.microsoft.com/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f7cc-218">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="4f7cc-218">Next Steps</span></span>

<span data-ttu-id="4f7cc-219">Bu izlenecek kodunuzdaki hataları düzeltme, kodu yeniden düzenleme, değişkenleri yeniden adlandırma ve kod parçacıkları kodunuza ekleme için Visual Studio 2010 kod düzenleyicisini, temel özellikleri gösterilen.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="4f7cc-220">Ek özellikler Düzenleyicisi'nde uygulama geliştirme hızlı ve kolay hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="4f7cc-221">Örneğin, aşağıdakileri yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="4f7cc-221">For example, you might want to:</span></span>

- <span data-ttu-id="4f7cc-222">IntelliSense seçeneklerini değiştirerek, kod parçacıkları yönetme ve çevrimiçi kod parçacıkları için arama gibi IntelliSense özellikleri hakkında daha fazla bilgi.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="4f7cc-223">Daha fazla bilgi için bkz: [kullanarak IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="4f7cc-224">Kendi kod parçacıkları oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="4f7cc-225">Daha fazla bilgi için bkz: [oluşturma ve kullanma IntelliSense kod parçacıkları](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="4f7cc-226">IntelliSense kod parçacıkları parçacıkları özelleştirme ve sorun giderme gibi Visual Basic'e özel özellikleri hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="4f7cc-227">Daha fazla bilgi için bkz: [Visual Basic IntelliSense kodu parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="4f7cc-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="4f7cc-228">C# hakkında daha fazla bilgi-IntelliSense, belirli özelliklerini yeniden düzenleme ve kod parçacıkları gibi.</span><span class="sxs-lookup"><span data-stu-id="4f7cc-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="4f7cc-229">Daha fazla bilgi için bkz: [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="4f7cc-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
