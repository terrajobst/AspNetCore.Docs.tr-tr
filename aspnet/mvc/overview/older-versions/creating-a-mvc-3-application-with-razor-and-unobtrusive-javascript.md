---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: MVC 3 oluşturma Razor ve Unobtrusive JavaScript ile uygulama | Microsoft Docs
author: microsoft
description: Kullanıcı listesi örnek web uygulaması, Razor görünüm altyapısını kullanarak ASP.NET MVC 3 uygulama oluşturmanın ne kadar basit olduğunu gösterir. Örnek uygulama s...
ms.author: aspnetcontent
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: 136c87cba70525da53c1f74576c50c12f8759539
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840474"
---
<a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a><span data-ttu-id="81f77-104">MVC 3 oluşturma Razor ve Unobtrusive JavaScript ile uygulama</span><span class="sxs-lookup"><span data-stu-id="81f77-104">Creating a MVC 3 Application with Razor and Unobtrusive JavaScript</span></span>
====================
<span data-ttu-id="81f77-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="81f77-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="81f77-106">Kullanıcı listesi örnek web uygulaması, Razor görünüm altyapısını kullanarak ASP.NET MVC 3 uygulama oluşturmanın ne kadar basit olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="81f77-106">The User List sample web application demonstrates how simple it is to create ASP.NET MVC 3 applications using the Razor view engine.</span></span> <span data-ttu-id="81f77-107">Örnek uygulama ile ASP.NET MVC 3 sürümü yeni Razor görünüm altyapısı ve Visual Studio 2010 oluşturma, görüntüleme, düzenleme ve silme kullanıcıları gibi işlevler içeren kurgusal bir kullanıcı listesinde Web sitesi oluşturmak için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="81f77-107">The sample application shows how to use the new Razor view engine with ASP.NET MVC version 3 and Visual Studio 2010 to create a fictional User List website that includes functionality such as creating, displaying, editing, and deleting users.</span></span>
> 
> <span data-ttu-id="81f77-108">Bu öğreticide, kullanıcı listesinde örnek ASP.NET MVC 3 uygulama oluşturmak için gerçekleştirilen adımlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="81f77-108">This tutorial describes the steps that were taken in order to build the User List sample ASP.NET MVC 3 application.</span></span> <span data-ttu-id="81f77-109">C# ve VB kaynak koduna sahip bir Visual Studio projesi bu konuya eşlik etmek üzere kullanılabilir: [indirme](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span><span class="sxs-lookup"><span data-stu-id="81f77-109">A Visual Studio project with C# and VB source code is available to accompany this topic: [Download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114).</span></span> <span data-ttu-id="81f77-110">Bu öğreticide hakkında sorularınız varsa lütfen bunları sonrası [MVC Forumu](https://forums.asp.net/1146.aspx).</span><span class="sxs-lookup"><span data-stu-id="81f77-110">If you have questions about this tutorial, please post them to the [MVC forum](https://forums.asp.net/1146.aspx).</span></span>


## <a name="overview"></a><span data-ttu-id="81f77-111">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="81f77-111">Overview</span></span>

<span data-ttu-id="81f77-112">Uygulama oluşturuyor olacaksınız bir basit kullanıcı listesi Web sitesidir.</span><span class="sxs-lookup"><span data-stu-id="81f77-112">The application you'll be building is a simple user list website.</span></span> <span data-ttu-id="81f77-113">Kullanıcılar girin, görüntüleme ve kullanıcı bilgilerini güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="81f77-113">Users can enter, view, and update user information.</span></span>

![Örnek site](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

<span data-ttu-id="81f77-115">VB ve C# projeyi indirebileceğiniz [burada](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span><span class="sxs-lookup"><span data-stu-id="81f77-115">You can download the VB and C# completed project [here](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).</span></span>

## <a name="creating-the-web-application"></a><span data-ttu-id="81f77-116">Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="81f77-116">Creating the Web Application</span></span>

<span data-ttu-id="81f77-117">Başlangıç öğreticisi için Visual Studio 2010'u açın ve kullanarak yeni bir proje oluşturma *ASP.NET MVC 3, Web uygulaması* şablonu.</span><span class="sxs-lookup"><span data-stu-id="81f77-117">To start the tutorial, open Visual Studio 2010 and create a new project using the *ASP.NET MVC 3 Web Application* template.</span></span> <span data-ttu-id="81f77-118">Uygulama adı &quot;Mvc3Razor&quot;.</span><span class="sxs-lookup"><span data-stu-id="81f77-118">Name the application &quot;Mvc3Razor&quot;.</span></span>

<span data-ttu-id="81f77-119">[![Yeni MVC 3 projesini](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="81f77-119">[![New MVC 3 project](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)</span></span>

<span data-ttu-id="81f77-120">İçinde **yeni ASP.NET MVC 3 projesini** iletişim kutusunda **Internet uygulaması**Razor görünüm Altyapısı'nı seçin ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="81f77-120">In the **New ASP.NET MVC 3 Project** dialog, select **Internet Application**, select the Razor view engine, and then click **OK**.</span></span>

![Yeni ASP.NET MVC 3 projesini iletişim kutusu](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

<span data-ttu-id="81f77-122">Üyelik ve oturum açma ile ilişkili tüm dosyaları silebilirsiniz. Bu öğreticide, ASP.NET üyelik sağlayıcısını kullanmaz.</span><span class="sxs-lookup"><span data-stu-id="81f77-122">In this tutorial you will not be using the ASP.NET membership provider, so you can delete all the files associated with logon and membership.</span></span> <span data-ttu-id="81f77-123">İçinde **Çözüm Gezgini**, aşağıdaki dosyaları ve dizinleri kaldırın:</span><span class="sxs-lookup"><span data-stu-id="81f77-123">In **Solution Explorer**, remove the following files and directories:</span></span>

- <span data-ttu-id="81f77-124">*Controllers\AccountController*</span><span class="sxs-lookup"><span data-stu-id="81f77-124">*Controllers\AccountController*</span></span>
- <span data-ttu-id="81f77-125">*Models\AccountModels*</span><span class="sxs-lookup"><span data-stu-id="81f77-125">*Models\AccountModels*</span></span>
- <span data-ttu-id="81f77-126">*Views\Shared\\_LogOnPartial*</span><span class="sxs-lookup"><span data-stu-id="81f77-126">*Views\Shared\\_LogOnPartial*</span></span>
- <span data-ttu-id="81f77-127">*Views\Account* (ve bu dizindeki tüm dosyaları)</span><span class="sxs-lookup"><span data-stu-id="81f77-127">*Views\Account* (and all the files in this directory)</span></span>

![Soln üs](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

<span data-ttu-id="81f77-129">Düzen  <em>\_Layout.cshtml</em> dosya ve biçimlendirme içinde değiştirin `<div>` adlı bir öğe `logindisplay` iletisiyle <em>&quot;</em>oturum açma devre dışı&quot;.</span><span class="sxs-lookup"><span data-stu-id="81f77-129">Edit the <em>\_Layout.cshtml</em> file and replace the markup inside the `<div>` element named `logindisplay` with the message <em>&quot;</em>Login Disabled&quot;.</span></span> <span data-ttu-id="81f77-130">Aşağıdaki örnek, yeni bir biçimlendirme gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="81f77-130">The following example shows the new markup:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a><span data-ttu-id="81f77-131">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="81f77-131">Adding the Model</span></span>

<span data-ttu-id="81f77-132">İçinde **Çözüm Gezgini**, sağ *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="81f77-132">In **Solution Explorer**, right-click the *Models* folder, select **Add**, and then click **Class**.</span></span>

![Yeni kullanıcı Mdl sınıfı](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

<span data-ttu-id="81f77-134">Sınıf adını `UserModel`.</span><span class="sxs-lookup"><span data-stu-id="81f77-134">Name the class `UserModel`.</span></span> <span data-ttu-id="81f77-135">Öğesinin içeriğini değiştirin *UserModel* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="81f77-135">Replace the contents of the *UserModel* file with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

<span data-ttu-id="81f77-136">`UserModel` Sınıfı, kullanıcılar'ı temsil eder.</span><span class="sxs-lookup"><span data-stu-id="81f77-136">The `UserModel` class represents users.</span></span> <span data-ttu-id="81f77-137">Her sınıf üyesi ile açıklanıyor [gerekli](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) özniteliğini [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı.</span><span class="sxs-lookup"><span data-stu-id="81f77-137">Each member of the class is annotated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute from the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace.</span></span> <span data-ttu-id="81f77-138">Öznitelikleri [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) ad alanı, web uygulamaları için otomatik istemci ve sunucu tarafı doğrulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="81f77-138">The attributes in the [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provide automatic client- and server-side validation for web applications.</span></span>

<span data-ttu-id="81f77-139">Açık `HomeController` sınıfı ve ekleme bir `using` erişebileceğiniz böylece yönerge `UserModel` ve `Users` sınıflar:</span><span class="sxs-lookup"><span data-stu-id="81f77-139">Open the `HomeController` class and add a `using` directive so that you can access the `UserModel` and `Users` classes:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

<span data-ttu-id="81f77-140">Hemen sonrasına `HomeController` bildirimi, aşağıdaki açıklamayı ve referansı eklemek bir `Users` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="81f77-140">Just after the `HomeController` declaration, add the following comment and the reference to a `Users` class:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

<span data-ttu-id="81f77-141">`Users` Sınıfı, bu öğreticide kullanacağınız Basitleştirilmiş, bellek içi veri deposu.</span><span class="sxs-lookup"><span data-stu-id="81f77-141">The `Users` class is a simplified, in-memory data store that you'll use in this tutorial.</span></span> <span data-ttu-id="81f77-142">Gerçek bir uygulamada, kullanıcı bilgilerini depolamak için bir veritabanı kullanırsınız.</span><span class="sxs-lookup"><span data-stu-id="81f77-142">In a real application you would use a database to store user information.</span></span> <span data-ttu-id="81f77-143">İlk birkaç satırı `HomeController` dosya, aşağıdaki örnekte gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="81f77-143">The first few lines of the `HomeController` file are shown in the following example:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

<span data-ttu-id="81f77-144">Uygulama kullanıcı modeli iskele Oluşturma Sihirbazı'nın sonraki adımda kullanılabilir olacak şekilde oluşturun.</span><span class="sxs-lookup"><span data-stu-id="81f77-144">Build the application so that the user model will be available to the scaffolding wizard in the next step.</span></span>

## <a name="creating-the-default-view"></a><span data-ttu-id="81f77-145">Varsayılan görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="81f77-145">Creating the Default View</span></span>

<span data-ttu-id="81f77-146">Sonraki adım, bir eylem yöntemi ve kullanıcılara görüntülenecek görünüm eklemektir.</span><span class="sxs-lookup"><span data-stu-id="81f77-146">The next step is to add an action method and view to display the users.</span></span>

<span data-ttu-id="81f77-147">Var olanı Sil *Views\Home\Index* dosya.</span><span class="sxs-lookup"><span data-stu-id="81f77-147">Delete the existing *Views\Home\Index* file.</span></span> <span data-ttu-id="81f77-148">Yeni oluşturduğunuz *dizin* dosyasını kullanıcıları görüntüler.</span><span class="sxs-lookup"><span data-stu-id="81f77-148">You will create a new *Index* file to display the users.</span></span>

<span data-ttu-id="81f77-149">İçinde `HomeController` sınıfı, içeriklerini `Index` yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="81f77-149">In the `HomeController` class, replace the contents of the `Index` method with the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

<span data-ttu-id="81f77-150">İçinde sağ `Index` yöntemi ve ardından **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="81f77-150">Right-click inside the `Index` method and then click **Add View**.</span></span>

![Görünüm Ekle](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

<span data-ttu-id="81f77-152">Seçin **kesin türü belirtilmiş görünüm oluşturmak** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="81f77-152">Select the **Create a strongly-typed view** option.</span></span> <span data-ttu-id="81f77-153">İçin **görüntülemek veri sınıfı**seçin **Mvc3Razor.Models.UserModel**.</span><span class="sxs-lookup"><span data-stu-id="81f77-153">For **View data class**, select **Mvc3Razor.Models.UserModel**.</span></span> <span data-ttu-id="81f77-154">(Görmüyorsanız **Mvc3Razor.Models.UserModel** içinde **görüntülemek veri sınıfı** kutusunda, projeyi oluşturmak için ihtiyacınız.) Görünüm altyapısı olarak ayarlandığından emin olun **Razor**.</span><span class="sxs-lookup"><span data-stu-id="81f77-154">(If you don't see **Mvc3Razor.Models.UserModel** in the **View data class** box, you need to build the project.) Make sure that the view engine is set to **Razor**.</span></span> <span data-ttu-id="81f77-155">Ayarlama **içeriği görüntüle** için **listesi** ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="81f77-155">Set **View content** to **List** and then click **Add**.</span></span>

![Dizini görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

<span data-ttu-id="81f77-157">Yeni Görünüm, geçirilen kullanıcı verilerini otomatik olarak iskele oluşturulduğunu `Index` görünümü.</span><span class="sxs-lookup"><span data-stu-id="81f77-157">The new view automatically scaffolds the user data that's passed to the `Index` view.</span></span> <span data-ttu-id="81f77-158">Yeni oluşturulan inceleyin *Views\Home\Index* dosya.</span><span class="sxs-lookup"><span data-stu-id="81f77-158">Examine the newly generated *Views\Home\Index* file.</span></span> <span data-ttu-id="81f77-159">**Yeni Oluştur**, **Düzenle**, **ayrıntıları**, ve **Sil** bağlantılar çalışmaz, ancak sayfanın geri kalanını çalışır durumdadır.</span><span class="sxs-lookup"><span data-stu-id="81f77-159">The **Create New**, **Edit**, **Details**, and **Delete** links don't work, but the rest of the page is functional.</span></span> <span data-ttu-id="81f77-160">Sayfayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="81f77-160">Run the page.</span></span> <span data-ttu-id="81f77-161">Kullanıcıların bir listesini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="81f77-161">You see a list of users.</span></span>

![Dizin Sayfası](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

<span data-ttu-id="81f77-163">Açık *Index.cshtml* değiştirin ve dosya `ActionLink` için biçimlendirme **Düzenle**, **ayrıntıları**, ve **Sil** aşağıdaki kodla :</span><span class="sxs-lookup"><span data-stu-id="81f77-163">Open the *Index.cshtml* file and replace the `ActionLink` markup for **Edit**, **Details**, and **Delete** with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

<span data-ttu-id="81f77-164">Seçili kaydın bulmak için kullanılan kullanıcı adı kimliği **Düzenle**, **ayrıntıları**, ve **Sil** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="81f77-164">The user name is used as the ID to find the selected record in the **Edit**, **Details**, and **Delete** links.</span></span>

## <a name="creating-the-details-view"></a><span data-ttu-id="81f77-165">Ayrıntılar görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="81f77-165">Creating the Details View</span></span>

<span data-ttu-id="81f77-166">Sonraki adım eklemektir bir `Details` eylem yöntemi ve kullanıcı ayrıntılarını görüntülemek için görünümü.</span><span class="sxs-lookup"><span data-stu-id="81f77-166">The next step is to add a `Details` action method and view in order to display user details.</span></span>

![Ayrıntılar](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

<span data-ttu-id="81f77-168">Aşağıdaki `Details` giriş denetleyicisine yöntemi:</span><span class="sxs-lookup"><span data-stu-id="81f77-168">Add the following `Details` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

<span data-ttu-id="81f77-169">İçinde sağ `Details` yöntemi ve ardından <strong>Görünüm Ekle</strong>.</span><span class="sxs-lookup"><span data-stu-id="81f77-169">Right-click inside the `Details` method and then select <strong>Add View</strong>.</span></span> <span data-ttu-id="81f77-170">Doğrulayın <strong>görüntülemek veri sınıfı</strong> kutusu içerir <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span><span class="sxs-lookup"><span data-stu-id="81f77-170">Verify that the <strong>View data class</strong> box contains <strong>Mvc3Razor.Models.UserModel</strong><em>.</em></span></span> <span data-ttu-id="81f77-171">Ayarlama <strong>içeriği görüntüle</strong> için <strong>ayrıntıları</strong> ve ardından <strong>Ekle</strong>.</span><span class="sxs-lookup"><span data-stu-id="81f77-171">Set <strong>View content</strong> to <strong>Details</strong> and then click <strong>Add</strong>.</span></span>

![Ayrıntılar Görünümü Ekle](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

<span data-ttu-id="81f77-173">Uygulamayı çalıştırmak ve bir ayrıntı bağlantısını seçin.</span><span class="sxs-lookup"><span data-stu-id="81f77-173">Run the application and select a details link.</span></span> <span data-ttu-id="81f77-174">Otomatik yapı iskelesi modeldeki her bir özellik gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="81f77-174">The automatic scaffolding shows each property in the model.</span></span>

![Ayrıntılar](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a><span data-ttu-id="81f77-176">Düzenleme görünümü oluşturma</span><span class="sxs-lookup"><span data-stu-id="81f77-176">Creating the Edit View</span></span>

<span data-ttu-id="81f77-177">Aşağıdaki `Edit` giriş denetleyicisine yöntemi.</span><span class="sxs-lookup"><span data-stu-id="81f77-177">Add the following `Edit` method to the home controller.</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

<span data-ttu-id="81f77-178">Önceki adımlarda açıklandığı şekilde bir görünüm ekleyin, ama ayarlamak **içeriği görüntüle** için **Düzenle**.</span><span class="sxs-lookup"><span data-stu-id="81f77-178">Add a view as in the previous steps, but set **View content** to **Edit**.</span></span>

![Düzenleme görünümü ekleme](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

<span data-ttu-id="81f77-180">Uygulamayı çalıştırmak ve bir kullanıcı adı ve Soyadı düzenleyin.</span><span class="sxs-lookup"><span data-stu-id="81f77-180">Run the application and edit the first and last name of one of the users.</span></span> <span data-ttu-id="81f77-181">Tüm ihlal `DataAnnotation` uygulanmış kısıtlamaları `UserModel` sınıf, form gönderdiğinizde görürsünüz sunucu kodu tarafından oluşturulan doğrulama hataları.</span><span class="sxs-lookup"><span data-stu-id="81f77-181">If you violate any `DataAnnotation` constraints that have been applied to the `UserModel` class, when you submit the form, you will see validation errors that are produced by server code.</span></span> <span data-ttu-id="81f77-182">Örneğin, ilk adını değiştirirseniz &quot;Ann&quot; için &quot;bir&quot;, form gönderme formunda aşağıdaki hata görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="81f77-182">For example, if you change the first name &quot;Ann&quot; to &quot;A&quot;, when you submit the form, the following error is displayed on the form:</span></span>

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

<span data-ttu-id="81f77-183">Bu öğreticide, kullanıcı adı birincil anahtar olarak davranılması.</span><span class="sxs-lookup"><span data-stu-id="81f77-183">In this tutorial, you're treating the user name as the primary key.</span></span> <span data-ttu-id="81f77-184">Bu nedenle, kullanıcı adı özelliği değiştirilemez.</span><span class="sxs-lookup"><span data-stu-id="81f77-184">Therefore, the user name property cannot be changed.</span></span> <span data-ttu-id="81f77-185">İçinde *Edit.cshtml* hemen sonrasına dosya `Html.BeginForm` deyimi, gizli bir alan için kullanıcı adını ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="81f77-185">In the *Edit.cshtml* file, just after the `Html.BeginForm` statement, set the user name to be a hidden field.</span></span> <span data-ttu-id="81f77-186">Bu modelde geçirilecek özelliği neden olur.</span><span class="sxs-lookup"><span data-stu-id="81f77-186">This causes the property to be passed in the model.</span></span> <span data-ttu-id="81f77-187">Aşağıdaki kod parçası yerleşimini gösterir `Hidden` deyimi:</span><span class="sxs-lookup"><span data-stu-id="81f77-187">The following code fragment shows the placement of the `Hidden` statement:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

<span data-ttu-id="81f77-188">Değiştirin `TextBoxFor` ve `ValidationMessageFor` biçimlendirme için kullanıcı adıyla bir `DisplayFor` çağırın.</span><span class="sxs-lookup"><span data-stu-id="81f77-188">Replace the `TextBoxFor` and `ValidationMessageFor` markup for the user name with a `DisplayFor` call.</span></span> <span data-ttu-id="81f77-189">`DisplayFor` Yöntemi, özelliği salt okunur bir öğe olarak görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="81f77-189">The `DisplayFor` method displays the property as a read-only element.</span></span> <span data-ttu-id="81f77-190">Aşağıdaki örnek, tamamlanan biçimlendirmeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="81f77-190">The following example shows the completed markup.</span></span> <span data-ttu-id="81f77-191">Özgün `TextBoxFor` ve `ValidationMessageFor` çağrıları açıklamalı kullanıma Razor açıklama başlangıcı ve sonu açıklama karakterleri (`@* *@`)</span><span class="sxs-lookup"><span data-stu-id="81f77-191">The original `TextBoxFor` and `ValidationMessageFor` calls are commented out with the Razor begin-comment and end-comment characters (`@* *@`)</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a><span data-ttu-id="81f77-192">İstemci tarafı doğrulamasını etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="81f77-192">Enabling Client-Side Validation</span></span>

<span data-ttu-id="81f77-193">ASP.NET MVC 3'te istemci tarafı doğrulamasını etkinleştirmek için iki bayraklarını ayarlayın ve üç JavaScript dosyaları eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="81f77-193">To enable client-side validation in ASP.NET MVC 3, you must set two flags and you must include three JavaScript files.</span></span>

<span data-ttu-id="81f77-194">Uygulamanın açın *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="81f77-194">Open the application's *Web.config* file.</span></span> <span data-ttu-id="81f77-195">Doğrulama `that ClientValidationEnabled` ve `UnobtrusiveJavaScriptEnabled` ayarlandığından içindeki uygulama ayarlarında true.</span><span class="sxs-lookup"><span data-stu-id="81f77-195">Verify `that ClientValidationEnabled` and `UnobtrusiveJavaScriptEnabled` are set to true in the application settings.</span></span> <span data-ttu-id="81f77-196">Aşağıdaki parça kök *Web.config* dosyasını doğru ayarların gösterir:</span><span class="sxs-lookup"><span data-stu-id="81f77-196">The following fragment from the root *Web.config* file shows the correct settings:</span></span>

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

<span data-ttu-id="81f77-197">Ayar `UnobtrusiveJavaScriptEnabled` örtük Ajax etkinleştirir ve örtük istemci doğrulama true.</span><span class="sxs-lookup"><span data-stu-id="81f77-197">Setting `UnobtrusiveJavaScriptEnabled` to true enables unobtrusive Ajax and unobtrusive client validation.</span></span> <span data-ttu-id="81f77-198">Örtük doğrulama kullandığınızda, doğrulama kurallarına HTML5 öznitelikler açık olabilir.</span><span class="sxs-lookup"><span data-stu-id="81f77-198">When you use unobtrusive validation, the validation rules are turned into HTML5 attributes.</span></span> <span data-ttu-id="81f77-199">HTML5 öznitelik adları yalnızca küçük harf, rakam ve tire içerebilir.</span><span class="sxs-lookup"><span data-stu-id="81f77-199">HTML5 attribute names can consist of only lowercase letters, numbers, and dashes.</span></span>

<span data-ttu-id="81f77-200">Ayar `ClientValidationEnabled` için istemci tarafı doğrulama true etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="81f77-200">Setting `ClientValidationEnabled` to true enables client-side validation.</span></span> <span data-ttu-id="81f77-201">Uygulama bu anahtarları ayarlayarak *Web.config* dosyası, istemci doğrulama ve uygulamanın tamamı için örtük JavaScript'i etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="81f77-201">By setting these keys in the application *Web.config* file, you enable client validation and unobtrusive JavaScript for the entire application.</span></span> <span data-ttu-id="81f77-202">Ayrıca, etkinleştirmek veya tek bir görünüm veya denetleyici yöntemlerinde aşağıdaki kodu kullanarak bu ayarları devre dışı bırakın:</span><span class="sxs-lookup"><span data-stu-id="81f77-202">You can also enable or disable these settings in individual views or in controller methods using the following code:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

<span data-ttu-id="81f77-203">İşlenmiş görünümde çeşitli JavaScript dosyaları eklenip eklenmeyeceğini gerekir.</span><span class="sxs-lookup"><span data-stu-id="81f77-203">You also need to include several JavaScript files in the rendered view.</span></span> <span data-ttu-id="81f77-204">Tüm görünümlerde JavaScript eklemek için kolay bir yol eklemek üzere olduğunu *görünümler/paylaşılan\\_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="81f77-204">An easy way to include the JavaScript in all views is to add them to the *Views\Shared\\_Layout.cshtml* file.</span></span> <span data-ttu-id="81f77-205">Değiştirin `<head>` öğesinin  *\_Layout.cshtml* dosyasındaki kodu aşağıdaki kodla:</span><span class="sxs-lookup"><span data-stu-id="81f77-205">Replace the `<head>` element of the *\_Layout.cshtml* file with the following code:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

<span data-ttu-id="81f77-206">Microsoft Ajax Content Delivery Network (CDN tarafından) ilk iki jQuery betikleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="81f77-206">The first two jQuery scripts are hosted by the Microsoft Ajax Content Delivery Network (CDN).</span></span> <span data-ttu-id="81f77-207">Microsoft Ajax CDN avantajlarından yararlanarak uygulamalarınızı ilk isabet performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="81f77-207">By taking advantage of the Microsoft Ajax CDN, you can significantly improve the first-hit performance of your applications.</span></span>

<span data-ttu-id="81f77-208">Uygulamayı çalıştırmak ve Düzenle bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="81f77-208">Run the application and click an edit link.</span></span> <span data-ttu-id="81f77-209">Sayfanın kaynağını tarayıcıda görüntülemek.</span><span class="sxs-lookup"><span data-stu-id="81f77-209">View the page's source in the browser.</span></span> <span data-ttu-id="81f77-210">Tarayıcı kaynağını formun birçok öznitelikleri gösterir `data-val` (için verisi doğrulama).</span><span class="sxs-lookup"><span data-stu-id="81f77-210">The browser source shows many attributes of the form `data-val` (for data validation).</span></span> <span data-ttu-id="81f77-211">İstemci doğrulama ve unobtrusive JavaScript etkin olduğunda, bir istemci doğrulama kuralı ile giriş alanlarını içeren `data-val="true"` örtük istemci doğrulama tetiklemek için özniteliği.</span><span class="sxs-lookup"><span data-stu-id="81f77-211">When client validation and unobtrusive JavaScript is enabled, input fields with a client-validation rule contain the `data-val="true"` attribute to trigger unobtrusive client validation.</span></span> <span data-ttu-id="81f77-212">Örneğin, `City` alan modeli ile düzenlenmiş [gerekli](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) özniteliği aşağıdaki örnekte gösterilen HTML sonuçlanır:</span><span class="sxs-lookup"><span data-stu-id="81f77-212">For example, the `City` field in the model was decorated with the [Required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) attribute, which results in the HTML shown in the following example:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

<span data-ttu-id="81f77-213">Her istemci doğrulama kuralı için bir özniteliği formun olan eklenir `data-val-rulename="message"`.</span><span class="sxs-lookup"><span data-stu-id="81f77-213">For each client-validation rule, an attribute is added that has the form `data-val-rulename="message"`.</span></span> <span data-ttu-id="81f77-214">Kullanarak `City` gerekli istemci doğrulama kuralı oluşturur. daha önce gösterilen alan örnek `data-val-required` özniteliği ve ileti &quot;Şehir alandır gerekli&quot;.</span><span class="sxs-lookup"><span data-stu-id="81f77-214">Using the `City` field example shown earlier, the required client-validation rule generates the `data-val-required` attribute and the message &quot;The City field is required&quot;.</span></span> <span data-ttu-id="81f77-215">Uygulamayı çalıştırmak, kullanıcılardan birinin düzenleme ve Temizle `City` alan.</span><span class="sxs-lookup"><span data-stu-id="81f77-215">Run the application, edit one of the users, and clear the `City` field.</span></span> <span data-ttu-id="81f77-216">Alanın dışına sekmesinde istemci tarafı doğrulama hata iletisi görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="81f77-216">When you tab out of the field, you see a client-side validation error message.</span></span>

![Şehir gereklidir](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

<span data-ttu-id="81f77-218">Benzer şekilde, istemci doğrulama kuralı her parametre için bir öznitelik formu olan eklenen `data-val-rulename-paramname=paramvalue`.</span><span class="sxs-lookup"><span data-stu-id="81f77-218">Similarly, for each parameter in the client-validation rule, an attribute is added that has the form `data-val-rulename-paramname=paramvalue`.</span></span> <span data-ttu-id="81f77-219">Örneğin, `FirstName` özelliği ile ek açıklamalı [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) özniteliği ve 3 minimum uzunluğu ve uzunluğu en fazla 8 belirtir.</span><span class="sxs-lookup"><span data-stu-id="81f77-219">For example, the `FirstName` property is annotated with the [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) attribute and specifies a minimum length of 3 and a maximum length of 8.</span></span> <span data-ttu-id="81f77-220">Adlı veri doğrulama kuralı `length` parametre adına sahip `max` ve 8 parametre değeri.</span><span class="sxs-lookup"><span data-stu-id="81f77-220">The data validation rule named `length` has the parameter name `max` and the parameter value 8.</span></span> <span data-ttu-id="81f77-221">Aşağıdakiler için oluşturulan HTML gösterir `FirstName` kullanıcılardan birinin düzenlerken alan:</span><span class="sxs-lookup"><span data-stu-id="81f77-221">The following shows the HTML that is generated for the `FirstName` field when you edit one of the users:</span></span>

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

<span data-ttu-id="81f77-222">Örtük istemci doğrulama hakkında daha fazla bilgi için bkz: Giriş [ASP.NET MVC 3'te örtük istemci doğrulama](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) Brad Wilson'ın blogunda.</span><span class="sxs-lookup"><span data-stu-id="81f77-222">For more information about unobtrusive client validation, see the entry [Unobtrusive Client Validation in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) in Brad Wilson's blog.</span></span>

> [!NOTE]
> <span data-ttu-id="81f77-223">ASP.NET MVC 3 Beta aşamasında, bazen istemci tarafı doğrulamayı başlatmak için formu göndermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="81f77-223">In ASP.NET MVC 3 Beta, you sometimes need to submit the form in order to start client-side validation.</span></span> <span data-ttu-id="81f77-224">Bu işlem için son sürüm değiştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="81f77-224">This might be changed for the final release.</span></span>


## <a name="creating-the-create-view"></a><span data-ttu-id="81f77-225">Oluştur görünümünün oluşturma</span><span class="sxs-lookup"><span data-stu-id="81f77-225">Creating the Create View</span></span>

<span data-ttu-id="81f77-226">Sonraki adım eklemektir bir `Create` eylem yöntemi ve yeni bir kullanıcı oluşturmak kullanıcı etkinleştirmek için görünümü.</span><span class="sxs-lookup"><span data-stu-id="81f77-226">The next step is to add a `Create` action method and view in order to enable the user to create a new user.</span></span> <span data-ttu-id="81f77-227">Aşağıdaki `Create` giriş denetleyicisine yöntemi:</span><span class="sxs-lookup"><span data-stu-id="81f77-227">Add the following `Create` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

<span data-ttu-id="81f77-228">Önceki adımlarda açıklandığı şekilde bir görünüm ekleyin, ama ayarlamak **içeriği görüntüle** için **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="81f77-228">Add a view as in the previous steps, but set **View content** to **Create**.</span></span>

![Görünüm oluşturma](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

<span data-ttu-id="81f77-230">Uygulamayı çalıştırmak, seçin **Oluştur** bağlamak ve yeni bir kullanıcı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="81f77-230">Run the application, select the **Create** link, and add a new user.</span></span> <span data-ttu-id="81f77-231">`Create` Yöntemi, otomatik olarak istemci tarafı ve sunucu tarafı doğrulama avantajlarından yararlanır.</span><span class="sxs-lookup"><span data-stu-id="81f77-231">The `Create` method automatically takes advantage of client-side and server-side validation.</span></span> <span data-ttu-id="81f77-232">Boşluk, gibi içeren bir kullanıcı adı girmesini deneyin &quot;Ben X&quot;.</span><span class="sxs-lookup"><span data-stu-id="81f77-232">Try to enter a user name that contains white space, such as &quot;Ben X&quot;.</span></span> <span data-ttu-id="81f77-233">Sekme, kullanıcı adı alanı dışında bir istemci tarafı doğrulama hatası (`White space is not allowed`) görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="81f77-233">When you tab out of the user name field, a client-side validation error (`White space is not allowed`) is displayed.</span></span>

## <a name="add-the-delete-method"></a><span data-ttu-id="81f77-234">Delete yöntemi ekleme</span><span class="sxs-lookup"><span data-stu-id="81f77-234">Add the Delete method</span></span>

<span data-ttu-id="81f77-235">Bu öğreticiyi tamamlamak için aşağıdakileri ekleyin `Delete` giriş denetleyicisine yöntemi:</span><span class="sxs-lookup"><span data-stu-id="81f77-235">To complete the tutorial, add the following `Delete` method to the home controller:</span></span>

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

<span data-ttu-id="81f77-236">Ekleme bir `Delete` görünüm ayarını önceki adımlarda açıklandığı şekilde **içeriği görüntüle** için **Sil**.</span><span class="sxs-lookup"><span data-stu-id="81f77-236">Add a `Delete` view as in the previous steps, setting **View content** to **Delete**.</span></span>

![Görünümü Sil](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

<span data-ttu-id="81f77-238">Artık doğrulama ile basit ancak tam olarak işlevsel bir ASP.NET MVC 3 uygulama vardır.</span><span class="sxs-lookup"><span data-stu-id="81f77-238">You now have a simple but fully functional ASP.NET MVC 3 application with validation.</span></span>
