---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Visual Studio 2013'te ASP.NET Web Forms kodunu düzenleme | Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 322a9aa6fb366eb9d5be33838fd6ac7ea51047f8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371800"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a><span data-ttu-id="692d9-102">Visual Studio 2013'te kod düzenleme ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="692d9-102">Code Editing ASP.NET Web Forms in Visual Studio 2013</span></span>
====================
<span data-ttu-id="692d9-103">tarafından [Erik Reitan](https://github.com/Erikre)</span><span class="sxs-lookup"><span data-stu-id="692d9-103">by [Erik Reitan](https://github.com/Erikre)</span></span>

<span data-ttu-id="692d9-104">Çok sayıda ASP.NET Web formu sayfalarında, Visual Basic, C# veya başka bir dilde kod yazın.</span><span class="sxs-lookup"><span data-stu-id="692d9-104">In many ASP.NET Web Form pages, you write code in Visual Basic, C#, or another language.</span></span> <span data-ttu-id="692d9-105">Visual Studio Kod düzenleyicisinde hataları önlemeye yardımcı olurken hızla kod yazmanıza yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="692d9-105">The code editor in Visual Studio can help you write code quickly while helping you avoid errors.</span></span> <span data-ttu-id="692d9-106">Ayrıca, düzenleyici gerçekleştirmeniz gereken iş miktarını azaltmaya yardımcı olmak için yeniden kullanılabilir kod oluşturma yol sağlar.</span><span class="sxs-lookup"><span data-stu-id="692d9-106">In addition, the editor provides ways for you to create reusable code to help reduce the amount of work you need to do.</span></span>

<span data-ttu-id="692d9-107">Bu izlenecek yol, Visual Studio Kod Düzenleyicisi'nin çeşitli özelliklerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="692d9-107">This walkthrough illustrates various features of the Visual Studio code editor.</span></span>

<span data-ttu-id="692d9-108">Bu kılavuz boyunca, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="692d9-108">During this walkthrough, you will learn how to:</span></span>

- <span data-ttu-id="692d9-109">Satır içi kodlama hataları düzeltin.</span><span class="sxs-lookup"><span data-stu-id="692d9-109">Correct inline coding errors.</span></span>
- <span data-ttu-id="692d9-110">Yeniden düzenleme ve kod yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="692d9-110">Refactor and rename code.</span></span>
- <span data-ttu-id="692d9-111">Değişkenleri ve nesneleri yeniden adlandırın.</span><span class="sxs-lookup"><span data-stu-id="692d9-111">Rename variables and objects.</span></span>
- <span data-ttu-id="692d9-112">Kod parçacıkları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="692d9-112">Insert code snippets.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="692d9-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="692d9-113">Prerequisites</span></span>


<span data-ttu-id="692d9-114">Bu izlenecek yolu tamamlamak için şunlar gerekir:</span><span class="sxs-lookup"><span data-stu-id="692d9-114">In order to complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="692d9-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) veya [Web için Microsoft Visual Studio Express 2013](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span><span class="sxs-lookup"><span data-stu-id="692d9-115">[Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) or [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web).</span></span> <span data-ttu-id="692d9-116">.NET Framework otomatik olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="692d9-116">The .NET Framework is installed automatically.</span></span> 

    > [!NOTE] 
    > 
    > <span data-ttu-id="692d9-117">Microsoft Visual Studio 2013 ve Web için Visual Studio Express 2013 Microsoft genellikle için Visual Studio Bu öğretici serisinin denir.</span><span class="sxs-lookup"><span data-stu-id="692d9-117">Microsoft Visual Studio 2013 and Microsoft Visual Studio Express 2013 for Web will often be referred to as Visual Studio throughout this tutorial series.</span></span>  
    >   
    > <span data-ttu-id="692d9-118">Visual Studio kullanıyorsanız, bu kılavuzda, seçtiğiniz varsayılır **Web geliştirme** ayarlar koleksiyonu, Visual Studio'yu ilk başlattığınızda.</span><span class="sxs-lookup"><span data-stu-id="692d9-118">If you are using Visual Studio, this walkthrough assumes that you selected the **Web Development** collection of settings the first time that you started Visual Studio.</span></span> <span data-ttu-id="692d9-119">Daha fazla bilgi için [nasıl yapılır: Web geliştirme ortamı ayarlarını seçin](https://msdn.microsoft.com/library/ff521558.aspx).</span><span class="sxs-lookup"><span data-stu-id="692d9-119">For more information, see [How to: Select Web Development Environment Settings](https://msdn.microsoft.com/library/ff521558.aspx).</span></span>

  <span data-ttu-id="692d9-120">Visual Studio ve ASP.NET giriş için bkz [Visual Studio 2013'te temel ASP.NET 4.5 Web Forms sayfası oluşturma](creating-a-basic-web-forms-page.md).</span><span class="sxs-lookup"><span data-stu-id="692d9-120">For an introduction to Visual Studio and ASP.NET, see [Creating a basic ASP.NET 4.5 Web Forms page in Visual Studio 2013](creating-a-basic-web-forms-page.md).</span></span>   
 

## <a name="creating-a-web-application-project-and-a-page"></a><span data-ttu-id="692d9-121">Bir Web uygulaması projesi ve bir sayfa oluşturma</span><span class="sxs-lookup"><span data-stu-id="692d9-121">Creating a Web application project and a Page</span></span>

<a id="sectionToggle0"></a>

<span data-ttu-id="692d9-122">Kılavuzun bu bölümünde, bir Web uygulaması projesi oluşturmak ve yeni bir sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="692d9-122">In this part of the walkthrough, you will create a Web application project and add a new page to it.</span></span>

### <a name="to-create-a-web-application-project"></a><span data-ttu-id="692d9-123">Bir Web uygulaması projesi oluşturmak için</span><span class="sxs-lookup"><span data-stu-id="692d9-123">To create a Web application project</span></span>

1. <span data-ttu-id="692d9-124">Microsoft Visual Studio'yu açın.</span><span class="sxs-lookup"><span data-stu-id="692d9-124">Open Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="692d9-125">Üzerinde **dosya** menüsünde **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="692d9-125">On the **File** menu, select **New Project**.</span></span>  
    <span data-ttu-id="692d9-126">![Dosya menüsü](code-editing-in-web-forms-pages/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="692d9-126">![File Menu](code-editing-in-web-forms-pages/_static/image1.png)</span></span>

    <span data-ttu-id="692d9-127">**Yeni proje** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="692d9-127">The **New Project** dialog box appears.</span></span>
3. <span data-ttu-id="692d9-128">Seçin **şablonları**  - &gt; **Visual C#**  - &gt; **Web** soldaki şablonları grubu.</span><span class="sxs-lookup"><span data-stu-id="692d9-128">Select the **Templates** -&gt; **Visual C#** -&gt; **Web** templates group on the left.</span></span>
4. <span data-ttu-id="692d9-129">Seçin **ASP.NET Web uygulaması** Orta sütunda şablonu.</span><span class="sxs-lookup"><span data-stu-id="692d9-129">Choose the **ASP.NET Web Application** template in the center column.</span></span>
5. <span data-ttu-id="692d9-130">Projenizi adlandırın ***BasicWebApp*** tıklatıp **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="692d9-130">Name your project ***BasicWebApp*** and click the **OK** button.</span></span>   
<span data-ttu-id="692d9-131">![Yeni Proje iletişim kutusu](code-editing-in-web-forms-pages/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="692d9-131">![New Project dialog box](code-editing-in-web-forms-pages/_static/image2.png)</span></span>
6. <span data-ttu-id="692d9-132">Ardından, **Web Forms** şablonu ve tıklatın **Tamam** projeyi oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="692d9-132">Next, select the **Web Forms** template and click the **OK** button to create the project.</span></span>  
<span data-ttu-id="692d9-133">![Yeni ASP.NET projesi iletişim kutusu](code-editing-in-web-forms-pages/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="692d9-133">![New ASP.NET Project dialog box](code-editing-in-web-forms-pages/_static/image3.png)</span></span>  

    <span data-ttu-id="692d9-134">Visual Studio Web Forms şablonu temel alan önceden oluşturulmuş işlevler içeren yeni bir proje oluşturur.</span><span class="sxs-lookup"><span data-stu-id="692d9-134">Visual Studio creates a new project that includes prebuilt functionality based on the Web Forms template.</span></span>


## <a name="creating-a-new-aspnet-web-forms-page"></a><span data-ttu-id="692d9-135">Yeni bir ASP.NET Web Forms sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="692d9-135">Creating a new ASP.NET Web Forms Page</span></span>


<span data-ttu-id="692d9-136">Yeni Web Forms kullanarak bir uygulama oluşturduğunuzda **ASP.NET Web uygulaması** proje şablonu, Visual Studio ekler adlı bir ASP.NET sayfasına (Web Forms sayfası) *Default.aspx*, yanı sıra birkaç diğer dosya ve klasörler.</span><span class="sxs-lookup"><span data-stu-id="692d9-136">When you create a new Web Forms application using the **ASP.NET Web Application** project template, Visual Studio adds an ASP.NET page (Web Forms page) named *Default.aspx*, as well as several other files and folders.</span></span> <span data-ttu-id="692d9-137">Kullanabileceğiniz *Default.aspx* sayfasında Web uygulamanız için giriş sayfası olarak.</span><span class="sxs-lookup"><span data-stu-id="692d9-137">You can use the *Default.aspx* page as the home page for your Web application.</span></span> <span data-ttu-id="692d9-138">Ancak, bu kılavuz için oluşturur ve yeni bir sayfa ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="692d9-138">However, for this walkthrough, you will create and work with a new page.</span></span>

### <a name="to-add-a-page-to-the-web-application"></a><span data-ttu-id="692d9-139">Web uygulaması için bir sayfa eklemek için</span><span class="sxs-lookup"><span data-stu-id="692d9-139">To add a page to the Web application</span></span>


1. <span data-ttu-id="692d9-140">İçinde **Çözüm Gezgini**, Web uygulamasının adını sağ tıklayın (Bu öğreticide uygulama adı **BasicWebSite**) ve ardından **Ekle**  - &gt; **Yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="692d9-140">In **Solution Explorer**, right-click the Web application name (in this tutorial the application name is **BasicWebSite**), and then click **Add** -&gt; **New Item**.</span></span>   
<span data-ttu-id="692d9-141">**Yeni Öğe Ekle** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="692d9-141">The **Add New Item** dialog box is displayed.</span></span>
2. <span data-ttu-id="692d9-142">Seçin **Visual C#**  - &gt; **Web** soldaki şablonları grubu.</span><span class="sxs-lookup"><span data-stu-id="692d9-142">Select the **Visual C#** -&gt; **Web** templates group on the left.</span></span> <span data-ttu-id="692d9-143">Ardından, **Web formu** ortasından listesinde ve adlandırın *FirstWebPage.aspx*.</span><span class="sxs-lookup"><span data-stu-id="692d9-143">Then, select **Web Form** from the middle list and name it *FirstWebPage.aspx*.</span></span>   
    <span data-ttu-id="692d9-144">![Yeni Öğe Ekle iletişim kutusu](code-editing-in-web-forms-pages/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="692d9-144">![Add New Item dialog box](code-editing-in-web-forms-pages/_static/image4.png)</span></span>
3. <span data-ttu-id="692d9-145">Tıklayın **Ekle** Web Forms sayfası projenize eklenecek.</span><span class="sxs-lookup"><span data-stu-id="692d9-145">Click **Add** to add the Web Forms page to your project.</span></span>  
 <span data-ttu-id="692d9-146">Visual Studio, yeni bir sayfa oluşturur ve açar.</span><span class="sxs-lookup"><span data-stu-id="692d9-146">Visual Studio creates the new page and opens it.</span></span>
4. <span data-ttu-id="692d9-147">Ardından, bu yeni sayfa varsayılan başlangıç sayfası olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="692d9-147">Next, set this new page as the default startup page.</span></span> <span data-ttu-id="692d9-148">İçinde **Çözüm Gezgini**, adlı yeni bir sayfa sağ *FirstWebPage.aspx* seçip **başlangıç sayfası olarak ayarla**.</span><span class="sxs-lookup"><span data-stu-id="692d9-148">In **Solution Explorer**, right-click the new page named *FirstWebPage.aspx* and select **Set As Start Page**.</span></span> <span data-ttu-id="692d9-149">İlerlememizin, test etmek için bu uygulamayı bir sonraki çalıştırmanızda otomatik olarak bu tarayıcıdaki yeni bir sayfa görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="692d9-149">The next time you run this application to test our progress, you will automatically see this new page in the browser.</span></span>


## <a name="correcting-inline-coding-errors"></a><span data-ttu-id="692d9-150">Satır içi kodlama hataları düzeltme</span><span class="sxs-lookup"><span data-stu-id="692d9-150">Correcting Inline Coding Errors</span></span>


<span data-ttu-id="692d9-151">Visual Studio Kod düzenleyicisinde kod yazma ve bir hata yaptıysanız, Kod Düzenleyicisi hatayı düzeltmek için yardımcı olan hataları önlemek için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="692d9-151">The code editor in Visual Studio helps you to avoid errors as you write code, and if you have made an error, the code editor helps you to correct the error.</span></span> <span data-ttu-id="692d9-152">Kılavuzun bu bölümünde bir Düzenleyicisi'nde hata düzeltme özellikleri gösteren bir kod satırı yazar.</span><span class="sxs-lookup"><span data-stu-id="692d9-152">In this part of the walkthrough, you will write a line of code that illustrate the error correction features in the editor.</span></span>

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a><span data-ttu-id="692d9-153">Visual Studio'da basit kodlama hatalarını düzeltmek için</span><span class="sxs-lookup"><span data-stu-id="692d9-153">To correct simple coding errors in Visual Studio</span></span>


1. <span data-ttu-id="692d9-154">İçinde **tasarım** görünümünde, boş bir sayfa için bir işleyici oluşturmak için çift tıklatın **yük** sayfa için olay.</span><span class="sxs-lookup"><span data-stu-id="692d9-154">In **Design** view, double-click the blank page to create a handler for the **Load** event for the page.</span></span>   
   <span data-ttu-id="692d9-155">Olay işleyicisi yalnızca bir yer olarak bazı kodlar yazma için kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="692d9-155">You are using the event handler only as a place to write some code.</span></span>
2. <span data-ttu-id="692d9-156">İşleyici içinde bir hata ve ENTER tuşuna içeren aşağıdaki satırdan **ENTER**:</span><span class="sxs-lookup"><span data-stu-id="692d9-156">Inside the handler, type the following line that contains an error and press **ENTER**:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   <span data-ttu-id="692d9-157">Bastığınızda **ENTER**, Kod Düzenleyicisi, yeşil ve kırmızı alt çizgiler yerleştirir (sık çağrı &quot;dalgalı&quot; satırları) sorunları kod alanlarının altında.</span><span class="sxs-lookup"><span data-stu-id="692d9-157">When you press **ENTER**, the code editor places green and red underlines (commonly call &quot;squiggly&quot; lines) under areas of the code that have issues.</span></span> <span data-ttu-id="692d9-158">Yeşil bir çizgi bir uyarı gösterir.</span><span class="sxs-lookup"><span data-stu-id="692d9-158">A green underline indicates a warning.</span></span> <span data-ttu-id="692d9-159">Kırmızı alt çizgi, düzeltmeniz gerekir bir hata olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="692d9-159">A red underline indicates an error that you must fix.</span></span> 

    <span data-ttu-id="692d9-160">Fare işaretçisini tutun `myStr` hakkında uyarı bildiren bir araç ipucu görmek için.</span><span class="sxs-lookup"><span data-stu-id="692d9-160">Hold the mouse pointer over `myStr` to see a tooltip that tells you about the warning.</span></span> <span data-ttu-id="692d9-161">Ayrıca, fare işaretçisini hata iletisini görmek için kırmızı alt çizgi üzerinde tutun.</span><span class="sxs-lookup"><span data-stu-id="692d9-161">Also, hold your mouse pointer over the red underline to see the error message.</span></span>

    <span data-ttu-id="692d9-162">Aşağıdaki görüntüde, alt çizgi ile kod gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="692d9-162">The following image shows the code with the underlines.</span></span>

    <span data-ttu-id="692d9-163">![Hoş Geldiniz metni Tasarım görünümünde](code-editing-in-web-forms-pages/_static/image5.png "metin Tasarım görünümünde Hoş Geldiniz")</span><span class="sxs-lookup"><span data-stu-id="692d9-163">![Welcome text in Design view](code-editing-in-web-forms-pages/_static/image5.png "Welcome text in Design view")</span></span>  
   <span data-ttu-id="692d9-164">Noktalı virgül ekleyerek hatanın düzeltilmesi gereken `;` satırın sonuna.</span><span class="sxs-lookup"><span data-stu-id="692d9-164">The error must be fixed by adding a semicolon `;` to the end of the line.</span></span> <span data-ttu-id="692d9-165">Uyarı yeterlidir, kullanmadığınız alacağınızı `myStr` henüz değişkeni.</span><span class="sxs-lookup"><span data-stu-id="692d9-165">The warning simply notifies you that you haven't used the `myStr` variable yet.</span></span>  

    > [!NOTE] 
    > 
    > <span data-ttu-id="692d9-166">Visual Studio'da ayarlar seçerek biçimlendirme, geçerli kod görüntüleme **Araçları**  - &gt; **seçenekleri**  - &gt; **yazı tipleri ve Renkleri**.</span><span class="sxs-lookup"><span data-stu-id="692d9-166">You view your current code formatting settings in Visual Studio by selecting **Tools** -&gt; **Options** -&gt; **Fonts and Colors**.</span></span>


## <a name="refactoring-and-renaming"></a><span data-ttu-id="692d9-167">Yeniden düzenleme ve yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="692d9-167">Refactoring and Renaming</span></span>

<span data-ttu-id="692d9-168">Yeniden düzenleme, kodunuzu anlamak ve işlevselliğini korurken korumak için daha kolay hale getirmek için yeniden yapılandırma içeren bir yazılım Metodoloji olur.</span><span class="sxs-lookup"><span data-stu-id="692d9-168">Refactoring is a software methodology that involves restructuring your code to make it easier to understand and to maintain, while preserving its functionality.</span></span> <span data-ttu-id="692d9-169">Basit bir örnek veritabanından veri almak için bir olay işleyicisine kod yazma olabilir.</span><span class="sxs-lookup"><span data-stu-id="692d9-169">A simple example might be that you write code in an event handler to get data from a database.</span></span> <span data-ttu-id="692d9-170">Sayfanız geliştirirken, birkaç farklı işleyicilerindeki verilere erişmek gereken keşfedin.</span><span class="sxs-lookup"><span data-stu-id="692d9-170">As you develop your page, you discover that you need to access the data from several different handlers.</span></span> <span data-ttu-id="692d9-171">Bu nedenle, bir veri erişim yöntemi sayfasında oluşturarak ve yöntemine yönelik çağrılar işleyicileri ekleme sayfanın kodun yeniden.</span><span class="sxs-lookup"><span data-stu-id="692d9-171">Therefore, you refactor the page's code by creating a data-access method in the page and inserting calls to the method in the handlers.</span></span>

<span data-ttu-id="692d9-172">Kod Düzenleyicisi, yeniden düzenleme çeşitli görevleri gerçekleştirmenize yardımcı olacak araçlar içerir.</span><span class="sxs-lookup"><span data-stu-id="692d9-172">The code editor includes tools to help you perform various refactoring tasks.</span></span> <span data-ttu-id="692d9-173">Bu kılavuzda, yeniden düzenleme iki teknikleri ile çalışır: değişkenleri yeniden adlandırma ve yöntemleri ayıklanıyor.</span><span class="sxs-lookup"><span data-stu-id="692d9-173">In this walkthrough, you will work with two refactoring techniques: renaming variables and extracting methods.</span></span> <span data-ttu-id="692d9-174">Diğer yeniden düzenleme seçeneklerini alanları Kapsüllenen, yöntem parametreleri yerel değişkenlere yükseltme ve yöntem parametreleri yönetme içerir.</span><span class="sxs-lookup"><span data-stu-id="692d9-174">Other refactoring options include encapsulating fields, promoting local variables to method parameters, and managing method parameters.</span></span> <span data-ttu-id="692d9-175">Bu yeniden düzenleme seçeneklerini kullanılabilirliğini kodun konumuna bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="692d9-175">The availability of these refactoring options depends on the location in the code.</span></span>

### <a name="refactoring-code"></a><span data-ttu-id="692d9-176">Kodu yeniden düzenleme</span><span class="sxs-lookup"><span data-stu-id="692d9-176">Refactoring Code</span></span>

<span data-ttu-id="692d9-177">Genel bir yeniden düzenleme senaryoyu (Ayıkla) oluşturmaktır bir yöntem içinde bir yöntemi gibi başka bir üye kodu.</span><span class="sxs-lookup"><span data-stu-id="692d9-177">A common refactoring scenario is to create (extract) a method from code that is inside another member, such as a method.</span></span> <span data-ttu-id="692d9-178">Bu ilk üyenin boyutunu azaltır ve ayıklanan kod yeniden kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="692d9-178">This reduces the size of the original member and makes the extracted code reusable.</span></span>

<span data-ttu-id="692d9-179">Kılavuzun bu bölümünde bazı basit kod yazın ve ardından bir yöntem bundan ayıklayın.</span><span class="sxs-lookup"><span data-stu-id="692d9-179">In this part of the walkthrough, you will write some simple code, and then extract a method from it.</span></span> <span data-ttu-id="692d9-180">C# programlama dili kullanan bir sayfa oluşturacak şekilde yeniden düzenleme C# ' ta desteklenir.</span><span class="sxs-lookup"><span data-stu-id="692d9-180">Refactoring is supported for C#, so you will create a page that uses C# as its programming language.</span></span>

### <a name="to-extract-a-method-in-a-c-page"></a><span data-ttu-id="692d9-181">Bir C# sayfasında bir yöntem ayıklamak için</span><span class="sxs-lookup"><span data-stu-id="692d9-181">To extract a method in a C# page</span></span>

1. <span data-ttu-id="692d9-182">Geçiş **tasarım** görünümü.</span><span class="sxs-lookup"><span data-stu-id="692d9-182">Switch to **Design** view.</span></span>
2. <span data-ttu-id="692d9-183">İçinde **araç kutusu**, gelen **standart** sekmesinde, sürükleyin bir [düğmesi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) sayfaya denetim.</span><span class="sxs-lookup"><span data-stu-id="692d9-183">In the **Toolbox**, from the **Standard** tab, drag a [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) control onto the page.</span></span>
3. <span data-ttu-id="692d9-184">Çift **düğmesi** denetim işleyicisi oluşturmak için kendi [tıklayın](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) olay ve ardından aşağıdaki vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="692d9-184">Double-click the **Button** control to create a handler for its [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) event, and then add the following highlighted code:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   <span data-ttu-id="692d9-185">Kod oluşturur bir **ArrayList** nesnesi değerlerle yüklemek için bir döngü kullanır ve ardından içeriğini görüntülemek için başka bir döngü kullanır **ArrayList** nesne.</span><span class="sxs-lookup"><span data-stu-id="692d9-185">The code creates an **ArrayList** object, uses a loop to load it with values, and then uses another loop to display the contents of the **ArrayList** object.</span></span>
4. <span data-ttu-id="692d9-186">Tuşuna **CTRL + F5** sayfayı çalıştırın ve ardından **düğmesi** aşağıdaki çıktıyı gördüğünüzü emin olmak için:</span><span class="sxs-lookup"><span data-stu-id="692d9-186">Press **CTRL+F5** to run the page, and then click the **button** to make sure that you see the following output:</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. <span data-ttu-id="692d9-187">Kod Düzenleyicisi'ne dönmek ve olay işleyicisi aşağıdaki satırları seçin.</span><span class="sxs-lookup"><span data-stu-id="692d9-187">Return to the code editor, and then select the following lines in the event handler.</span></span>   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. <span data-ttu-id="692d9-188">Seçime sağ tıklayın, **yeniden düzenleme**ve ardından **yöntemi ayıklama**.</span><span class="sxs-lookup"><span data-stu-id="692d9-188">Right-click the selection, click **Refactor**, and then choose **Extract Method**.</span></span> 

    <span data-ttu-id="692d9-189">**Yöntemi ayıklama** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="692d9-189">The **Extract Method** dialog box appears.</span></span>
7. <span data-ttu-id="692d9-190">İçinde **yeni yöntem adı** kutusuna **DisplayArray**ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="692d9-190">In the **New Method Name** box, type **DisplayArray**, and then click **OK**.</span></span> 

    <span data-ttu-id="692d9-191">Kod Düzenleyicisi adlı yeni bir yöntem oluşturur `DisplayArray`ve yeni yönteme bir çağrı koyar **tıklayın** döngü nerede oluştuğunu başlangıçta işleyici.</span><span class="sxs-lookup"><span data-stu-id="692d9-191">The code editor creates a new method named `DisplayArray`, and puts a call to the new method in the **Click** handler where the loop was originally.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. <span data-ttu-id="692d9-192">Tuşuna **CTRL + F5** yeniden çalıştırırsanız ve **düğmesi**.</span><span class="sxs-lookup"><span data-stu-id="692d9-192">Press **CTRL+F5** to run the page again, and click the **button**.</span></span>

    <span data-ttu-id="692d9-193">Önce yaptığınız gibi sayfa aynı şekilde çalışır.</span><span class="sxs-lookup"><span data-stu-id="692d9-193">The page functions the same as it did before.</span></span> <span data-ttu-id="692d9-194">`DisplayArray` Yöntemi çağrısı nerede olursanız olun artık olabilir sayfa sınıfında.</span><span class="sxs-lookup"><span data-stu-id="692d9-194">The `DisplayArray` method can now be call from anywhere in the page class.</span></span>

## <a name="renaming-variables"></a><span data-ttu-id="692d9-195">Değişkenleri yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="692d9-195">Renaming Variables</span></span>

<span data-ttu-id="692d9-196">Değişkenleri, yanı sıra nesneleri çalışırken, zaten kodunuzda başvurulan sonra bunları yeniden adlandırmak isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="692d9-196">When you work with variables, as well as objects, you might want to rename them after they are already referenced in your code.</span></span> <span data-ttu-id="692d9-197">Ancak, değişkenler ve nesneler yeniden adlandırma başvurulardan birini yeniden adlandırma kaçırmanıza kesmek için kodu neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="692d9-197">However, renaming variables and objects can cause the code to break if you miss renaming one of the references.</span></span> <span data-ttu-id="692d9-198">Bu nedenle, yeniden adlandırma gerçekleştirmek için yeniden düzenleme kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="692d9-198">Therefore, you can use refactoring to perform the renaming.</span></span>

### <a name="to-use-refactoring-to-rename-a-variable"></a><span data-ttu-id="692d9-199">Bir değişkeni yeniden adlandırmak için yeniden düzenleme kullanmak için</span><span class="sxs-lookup"><span data-stu-id="692d9-199">To use refactoring to rename a variable</span></span>


1. <span data-ttu-id="692d9-200">İçinde **tıklayın** olay işleyicisi, aşağıdaki satırı bulun:</span><span class="sxs-lookup"><span data-stu-id="692d9-200">In the **Click** event handler, locate the following line:</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. <span data-ttu-id="692d9-201">Değişken adına sağ tıklayın `alist`, seçin **yeniden düzenleme**ve ardından **Yeniden Adlandır**.</span><span class="sxs-lookup"><span data-stu-id="692d9-201">Right-click the variable name `alist`, choose **Refactor**, and then choose **Rename**.</span></span>

    <span data-ttu-id="692d9-202">**Yeniden Adlandır** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="692d9-202">The **Rename** dialog box appears.</span></span>
3. <span data-ttu-id="692d9-203">İçinde **yeni adı** kutusuna **ArrayList1** emin **başvuru değişikliklerini önizle** onay kutusunun seçili.</span><span class="sxs-lookup"><span data-stu-id="692d9-203">In the **New name** box, type **ArrayList1** and make sure the **Preview reference changes** checkbox has been selected.</span></span> <span data-ttu-id="692d9-204">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="692d9-204">Then click **OK**.</span></span>

    <span data-ttu-id="692d9-205">**Değişiklikleri Önizle** iletişim kutusu görünür ve tüm başvuruları yeniden adlandırdığınız değişkeni içeren bir ağaç görüntüler.</span><span class="sxs-lookup"><span data-stu-id="692d9-205">The **Preview Changes** dialog box appears, and displays a tree that contains all references to the variable that you are renaming.</span></span>
4. <span data-ttu-id="692d9-206">Tıklayın **Uygula** kapatmak için **Değişiklikleri Önizle** iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="692d9-206">Click **Apply** to close the **Preview Changes** dialog box.</span></span>

    <span data-ttu-id="692d9-207">Özellikle, seçtiğiniz örneğine başvurma değişkenleri yeniden adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="692d9-207">The variables that refer specifically to the instance that you selected are renamed.</span></span> <span data-ttu-id="692d9-208">Ancak, unutmayın, değişken `alist` aşağıdaki satırda yeniden adlandırılmaz.</span><span class="sxs-lookup"><span data-stu-id="692d9-208">Note, however, that the variable `alist` in the following line is not renamed.</span></span>

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    <span data-ttu-id="692d9-209">Değişken `alist` değişkeni aynı değeri temsil etmediğinden bu satırda yeniden adlandırılmış değil `alist` adlandırdığınız.</span><span class="sxs-lookup"><span data-stu-id="692d9-209">The variable `alist` in this line is not renamed because it does not represent the same value as the variable `alist` that you renamed.</span></span> <span data-ttu-id="692d9-210">Değişken `alist` içinde `DisplayArray` bu yöntem için bir yerel değişken bildirimidir.</span><span class="sxs-lookup"><span data-stu-id="692d9-210">The variable `alist` in the `DisplayArray` declaration is a local variable for that method.</span></span> <span data-ttu-id="692d9-211">Bu değişkenler yeniden adlandırmak için yeniden düzenleme kullanarak yalnızca düzenleyicide Bul ve Değiştir eylemini gerçekleştirmekten daha farklı olduğunu gösterir; yeniden düzenleme ile çalışma değişkeni semantiği bilgisi değişkenlerle yeniden adlandırır.</span><span class="sxs-lookup"><span data-stu-id="692d9-211">This illustrates that using refactoring to rename variables is different than simply performing a find-and-replace action in the editor; refactoring renames variables with knowledge of the semantics of the variable that it is working with.</span></span>


## <a name="inserting-snippets"></a><span data-ttu-id="692d9-212">Kod parçacıkları ekleme</span><span class="sxs-lookup"><span data-stu-id="692d9-212">Inserting Snippets</span></span>

<span data-ttu-id="692d9-213">Web Forms geliştiriciler sık yapmanız gereken birçok kodlama görevleri olduğundan, Kod Düzenleyicisi kod parçacıkları veya önceden kod blokları bir kitaplık sağlar.</span><span class="sxs-lookup"><span data-stu-id="692d9-213">Because there are many coding tasks that Web Forms developers frequently need to perform, the code editor provides a library of snippets, or blocks of prewritten code.</span></span> <span data-ttu-id="692d9-214">Bu kod parçacıklarının sayfanıza ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="692d9-214">You can insert these snippets into your page.</span></span>

<span data-ttu-id="692d9-215">Visual Studio'da kullandığınız her bir dilin kod parçacıkları ekleme küçük farklılıklar vardır.</span><span class="sxs-lookup"><span data-stu-id="692d9-215">Each language that you use in Visual Studio has slight differences in the way you insert code snippets.</span></span> <span data-ttu-id="692d9-216">Kod parçacıkları ekleme hakkında daha fazla bilgi için bkz: [Visual Basic IntelliSense kodu parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx).</span><span class="sxs-lookup"><span data-stu-id="692d9-216">For information about inserting snippets, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx).</span></span> <span data-ttu-id="692d9-217">Visual C# içinde kod parçacıkları ekleme hakkında daha fazla bilgi için bkz: [Visual C# kod parçacıkları](https://msdn.microsoft.com/library/z41h7fat.aspx).</span><span class="sxs-lookup"><span data-stu-id="692d9-217">For information about inserting snippets in Visual C#, see [Visual C# Code Snippets](https://msdn.microsoft.com/library/z41h7fat.aspx).</span></span>

## <a name="next-steps"></a><span data-ttu-id="692d9-218">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="692d9-218">Next Steps</span></span>

<span data-ttu-id="692d9-219">Bu izlenecek kodunuzdaki hataları düzeltme, kodu yeniden düzenleme, yeniden adlandırma değişkenleri ve kodunuza kod parçacıkları ekleme için Visual Studio 2010 Kod Düzenleyicisi'ni temel özelliklerinde gösterilen.</span><span class="sxs-lookup"><span data-stu-id="692d9-219">This walkthrough has illustrated the basic features of the Visual Studio 2010 code editor for correcting errors in your code, refactoring code, renaming variables, and inserting code snippets into your code.</span></span> <span data-ttu-id="692d9-220">Ek özellik Düzenleyicisi'nde uygulama geliştirme hızlı ve kolay hale getirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="692d9-220">Additional features in the editor can make application development fast and easy.</span></span> <span data-ttu-id="692d9-221">Örneğin, aşağıdakileri yapabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="692d9-221">For example, you might want to:</span></span>

- <span data-ttu-id="692d9-222">IntelliSense kod parçacıkları için çevrimiçi arama IntelliSense seçeneklerini değiştirme ve kod parçacıkları yönetme gibi özellikleri hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="692d9-222">Learn more about the features of IntelliSense, such as modifying IntelliSense options, managing code snippets, and searching for code snippets online.</span></span> <span data-ttu-id="692d9-223">Daha fazla bilgi için [IntelliSense kullanarak](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span><span class="sxs-lookup"><span data-stu-id="692d9-223">For more information, see [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).</span></span>
- <span data-ttu-id="692d9-224">Kendi kod parçacıklarınızı oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="692d9-224">Learn how to create your own code snippets.</span></span> <span data-ttu-id="692d9-225">Daha fazla bilgi için [oluşturma ve IntelliSense kod parçacıkları kullanma](https://msdn.microsoft.com/library/ms165392.aspx)</span><span class="sxs-lookup"><span data-stu-id="692d9-225">For more information, see [Creating and Using IntelliSense Code Snippets](https://msdn.microsoft.com/library/ms165392.aspx)</span></span>
- <span data-ttu-id="692d9-226">Visual Basic'e özel IntelliSense kod parçacıkları, kod parçacıkları özelleştirme ve sorun giderme gibi özellikleri hakkında daha fazla bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="692d9-226">Learn more about the Visual Basic-specific features of IntelliSense code snippets, such as customizing the snippets and troubleshooting.</span></span> <span data-ttu-id="692d9-227">Daha fazla bilgi için [Visual Basic IntelliSense kod parçacıkları](https://msdn.microsoft.com/library/18yz4be4.aspx)</span><span class="sxs-lookup"><span data-stu-id="692d9-227">For more information, see [Visual Basic IntelliSense Code Snippets](https://msdn.microsoft.com/library/18yz4be4.aspx)</span></span>
- <span data-ttu-id="692d9-228">C# hakkında daha fazla bilgi-IntelliSense, yeniden düzenleme ve kod parçacıkları gibi belirli özellikleri.</span><span class="sxs-lookup"><span data-stu-id="692d9-228">Learn more about the C#-specific features of IntelliSense, such as refactoring and code snippets.</span></span> <span data-ttu-id="692d9-229">Daha fazla bilgi için [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span><span class="sxs-lookup"><span data-stu-id="692d9-229">For more information, see [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).</span></span>
