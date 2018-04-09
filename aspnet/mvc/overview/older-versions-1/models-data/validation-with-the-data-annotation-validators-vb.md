---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: Veri ek açıklaması doğrulayıcıları (VB) ile doğrulama | Microsoft Docs
author: microsoft
description: Bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklaması Model bağlayıcı yararlanın. Doğrulayıcı farklı türde kullanmayı öğrenin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="263e4-104">Veri ek açıklaması doğrulayıcıları (VB) ile doğrulama</span><span class="sxs-lookup"><span data-stu-id="263e4-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="263e4-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="263e4-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="263e4-106">Bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklaması Model bağlayıcı yararlanın.</span><span class="sxs-lookup"><span data-stu-id="263e4-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="263e4-107">Doğrulayıcı öznitelikleri farklı türde kullanın ve bunları Microsoft Entity Framework çalışma hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="263e4-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="263e4-108">Bu öğreticide, veri ek açıklamasını doğrulayıcıları bir ASP.NET MVC uygulamasındaki doğrulama gerçekleştirmek için nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="263e4-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="263e4-109">Veri ek açıklamasını doğrulayıcıları kullanmanın avantajı, doğrulama gerçekleştirmek yalnızca ekleyerek – gerekli gibi bir veya daha fazla öznitelikleri ya da StringLength öznitelik – bir sınıf özelliği için Etkinleştir ' dir.</span><span class="sxs-lookup"><span data-stu-id="263e4-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="263e4-110">Veri ek açıklamasını doğrulayıcıları kullanmadan önce veri ek açıklamaları Model bağlayıcısını yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="263e4-111">Tıklayarak veri ek açıklamaları Model bağlayıcı örneği CodePlex Web sitesinden indirebileceğiniz [burada](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="263e4-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="263e4-112">Veri ek açıklamaları Model Bağlayıcısı Microsoft ASP.NET MVC çerçevesi resmi bir parçası olduğunu anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="263e4-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="263e4-113">Veri ek açıklamaları Model bağlayıcı Microsoft ASP.NET MVC ekibi tarafından oluşturulmuş olsa da, Microsoft açıklandığı ve Bu öğreticide kullanılan veri ek açıklamaları Model bağlayıcı için resmi ürün destek sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="263e4-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="263e4-114">Veri ek açıklaması Model Bağlayıcısı kullanma</span><span class="sxs-lookup"><span data-stu-id="263e4-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="263e4-115">Bir ASP.NET MVC uygulamasındaki veri ek açıklamaları Model bağlayıcısını kullanmak için öncelikle Microsoft.Web.Mvc.DataAnnotations.dll derleme ve System.ComponentModel.DataAnnotations.dll derleme için bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="263e4-116">Menü seçeneğini **proje, Başvuru Ekle**.</span><span class="sxs-lookup"><span data-stu-id="263e4-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="263e4-117">İleri'yi **Gözat** sekmesinde ve indirdiğiniz (ve sıkıştırması açılmış) veri ek açıklamaları Model bağlayıcı örnek konumuna göz atın (bkz **Şekil 1**).</span><span class="sxs-lookup"><span data-stu-id="263e4-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="263e4-118">**Şekil 1**: veri ek açıklamaları Model bağlayıcı için bir başvuru ekleyerek ([tam boyutlu görüntüyü görüntülemek için tıklatın](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="263e4-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="263e4-119">Microsoft.Web.Mvc.DataAnnotations.dll derleme ve System.ComponentModel.DataAnnotations.dll derleme seçip tıklatın **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="263e4-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="263e4-120">.NET Framework Service Pack 1 veri ek açıklamaları Model Bağlayıcısı ile birlikte gelen System.ComponentModel.DataAnnotations.dll derleme kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="263e4-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="263e4-121">Veri ek açıklamaları Model bağlayıcı örneği indirmeye dahil System.ComponentModel.DataAnnotations.dll derleme sürümünü kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="263e4-122">Son olarak, Global.asax dosyasında DataAnnotations Model bağlayıcı kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="263e4-123">Uygulamaya aşağıdaki kod satırını ekleyin\_Start() olay işleyicisi böylece uygulama\_Start() yöntemini aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="263e4-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="263e4-124">Bu kod satırı DataAnnotationsModelBinder tüm ASP.NET MVC uygulaması için varsayılan model bağlayıcısını olarak kaydeder.</span><span class="sxs-lookup"><span data-stu-id="263e4-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="263e4-125">Veri ek açıklaması Doğrulayıcı öznitelikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="263e4-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="263e4-126">Veri ek açıklamaları Model bağlayıcısını kullandığınızda, doğrulama gerçekleştirmek için Doğrulayıcı öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="263e4-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="263e4-127">System.ComponentModel.DataAnnotations ad alanı, aşağıdaki Doğrulayıcı öznitelikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="263e4-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="263e4-128">Aralık – bir özelliğin değerini belirtilen bir değer aralığının arasında olup olmadığını doğrulamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="263e4-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="263e4-129">ReqularExpression – bir özelliğin değerini belirtilen normal ifade deseni ile eşleşip eşleşmediğini doğrulamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="263e4-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="263e4-130">Gerekli – bir özellik gerekli olarak işaretlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="263e4-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="263e4-131">StringLength – bir dize özelliği için maksimum uzunluk belirtmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="263e4-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="263e4-132">Doğrulama – tüm Doğrulayıcı öznitelikleri için temel sınıf.</span><span class="sxs-lookup"><span data-stu-id="263e4-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="263e4-133">Doğrulama gereksinimlerinizi herhangi bir standart doğrulayıcıları tarafından değil sağlanırsa sonra her zaman yeni bir doğrulayıcı öznitelik temel doğrulama özniteliğinden devralarak özel Doğrulayıcı özniteliği oluşturma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="263e4-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="263e4-134">Product sınıfında **listeleme 1** bu Doğrulayıcı öznitelikleri nasıl kullanıldığını gösterir.</span><span class="sxs-lookup"><span data-stu-id="263e4-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="263e4-135">Ad, açıklama ve UnitPrice özellikleri işaretlenmiş gerektiği gibi.</span><span class="sxs-lookup"><span data-stu-id="263e4-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="263e4-136">Name özelliği 10'dan az karakter olan bir dize uzunluğu olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="263e4-137">Son olarak, UnitPrice özelliği, bir para birimi miktarını temsil eden bir normal ifade deseni eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="263e4-138">**Kod 1**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="263e4-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="263e4-139">Ürün sınıfı bir ek özniteliğin nasıl kullanılacağını göstermektedir: DisplayName özniteliği.</span><span class="sxs-lookup"><span data-stu-id="263e4-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="263e4-140">DisplayName özniteliği özelliği bir hata iletisi görüntülendiğinde özelliğinin adını değiştirmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="263e4-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="263e4-141">Hata iletisi "UnitPrice gerekli bir alandır" yerine "Fiyat gerekli bir alandır" hata iletisi görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="263e4-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="263e4-142">Tamamen Doğrulayıcısı tarafından görüntülenen hata iletisini özelleştirmek istiyorsanız bu gibi Doğrulayıcısı'nın ErrorMessage özelliği için bir özel hata iletisi atayabilirsiniz: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="263e4-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="263e4-143">Product sınıfında kullanabilirsiniz **listeleme 1** Create() denetleyici eylemi ile **listeleme 2**.</span><span class="sxs-lookup"><span data-stu-id="263e4-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="263e4-144">Model durumu hataları içerdiğinde Bu denetleyici eylemi Oluştur görünümünün görüntüler.</span><span class="sxs-lookup"><span data-stu-id="263e4-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="263e4-145">**Kod 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="263e4-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="263e4-146">Son olarak, görünümünde oluşturabilirsiniz **listeleme 3** Create() eylem sağ tıklayarak ve menü seçeneğini belirleyerek **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="263e4-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="263e4-147">Kesin türü belirtilmiş görünüm model sınıfı ürün sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="263e4-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="263e4-148">Seçin **oluşturma** görünümü içerik aşağı açılan listeden (bkz **Şekil 2**).</span><span class="sxs-lookup"><span data-stu-id="263e4-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="263e4-149">**Şekil 2**: oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="263e4-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="263e4-150">**Listing 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="263e4-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="263e4-151">Tarafından oluşturulan form oluştur ID alanı kaldırma **Görünüm Ekle** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="263e4-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="263e4-152">ID alanı karşılık gelen bir kimlik sütunu olduğundan, bu alan için bir değer girin yapmalarına izin vermek istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="263e4-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="263e4-153">Ürün oluşturma için form gönderme ve gerekli alanlar için değerleri girmeyin durumunda doğrulama hatası iletilerini **Şekil 3** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="263e4-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="263e4-154">**Şekil 3**: gerekli alanlar eksik</span><span class="sxs-lookup"><span data-stu-id="263e4-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="263e4-155">Bir geçersiz para birimi miktarını sonra hata iletisinde girerseniz **Şekil 4** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="263e4-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="263e4-156">**Şekil 4**: geçersiz bir para birimi</span><span class="sxs-lookup"><span data-stu-id="263e4-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="263e4-157">Veri ek açıklaması doğrulayıcıları Entity Framework ile kullanma</span><span class="sxs-lookup"><span data-stu-id="263e4-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="263e4-158">Microsoft Entity Framework veri modeli sınıfları oluşturmak için kullanıyorsanız, sınıflarınızı doğrudan Doğrulayıcı öznitelikleri uygulanamıyor.</span><span class="sxs-lookup"><span data-stu-id="263e4-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="263e4-159">Entity Framework Designer modeli sınıfları oluşturduğundan, model sınıflarına yaptığınız tüm değişiklikler Tasarımcısı'nda herhangi bir değişiklik bir sonraki sefer üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="263e4-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="263e4-160">Entity Framework tarafından oluşturulan sınıflar ile doğrulayıcıları kullanmak istiyorsanız, meta veri sınıfları oluşturmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="263e4-161">Doğrulayıcıları doğrulayıcıları gerçek sınıfı için uygulama yerine meta veri sınıfı için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="263e4-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="263e4-162">Örneğin, Entity Framework kullanarak film sınıf oluşturduğunuzu düşünün (bkz **Şekil 5**).</span><span class="sxs-lookup"><span data-stu-id="263e4-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="263e4-163">Ayrıca, filmi ve Director özellikleri gerekli özellikleri yapmak istediğiniz düşünün.</span><span class="sxs-lookup"><span data-stu-id="263e4-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="263e4-164">Bu durumda, kısmi ve meta veri sınıf oluşturabilirsiniz **listeleme 4**.</span><span class="sxs-lookup"><span data-stu-id="263e4-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="263e4-165">**Şekil 5**: Entity Framework tarafından oluşturulan film sınıfı</span><span class="sxs-lookup"><span data-stu-id="263e4-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="263e4-166">**4 listeleme**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="263e4-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="263e4-167">Dosyada **listeleme 4** film ve MovieMetaData adlı iki sınıflar içerir.</span><span class="sxs-lookup"><span data-stu-id="263e4-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="263e4-168">Film sınıfı kısmi bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="263e4-168">The Movie class is a partial class.</span></span> <span data-ttu-id="263e4-169">DataModel.Designer.vb dosyasında yer alan Entity Framework tarafından oluşturulan sınıfa karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="263e4-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="263e4-170">Şu anda, .NET framework kısmi özellikleri desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="263e4-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="263e4-171">Bu nedenle, Doğrulayıcı öznitelikleri dosyasında tanımlanmış film sınıf özelliklerini uygulayarak DataModel.Designer.vb dosyasında tanımlanan film sınıfının özelliklerine Doğrulayıcı öznitelikleri uygulamak için bir yolu yoktur **4listeleme**.</span><span class="sxs-lookup"><span data-stu-id="263e4-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="263e4-172">Film parçalı sınıf MovieMetaData sınıfa işaret eden bir MetadataType özniteliği ile tasarlandığına dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="263e4-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="263e4-173">MovieMetaData sınıfı film sınıfının özelliklerine proxy özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="263e4-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="263e4-174">Doğrulayıcı öznitelikleri MovieMetaData sınıfının özelliklerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="263e4-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="263e4-175">Başlık, Director ve DateReleased özellikleri tüm gerekli özellikleri olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="263e4-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="263e4-176">5'den az karakter içeren bir dizeyi Director özelliği atanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="263e4-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="263e4-177">Son olarak, DisplayName özniteliği "serbest bırakılmış tarihi gerekli bir alandır."gibi bir hata iletisi görüntülenecek DateReleased özelliğine uygulanır</span><span class="sxs-lookup"><span data-stu-id="263e4-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="263e4-178">hata yerine "DateReleased alan gereklidir."</span><span class="sxs-lookup"><span data-stu-id="263e4-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="263e4-179">MovieMetaData sınıfı proxy özelliklerinde film sınıfı karşılık gelen özelliklerinde aynı türlerine temsil gerekmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="263e4-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="263e4-180">Örneğin, Director film sınıfında bir dize özelliği ve MovieMetaData sınıfında bir nesne özelliği özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="263e4-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="263e4-181">Sayfanın **Şekil 6** film özelliklerini geçersiz değerler girdiğinizde döndürülen hata iletilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="263e4-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="263e4-182">**Şekil 6**: doğrulayıcıları ile Entity Framework kullanarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="263e4-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="263e4-183">Özet</span><span class="sxs-lookup"><span data-stu-id="263e4-183">Summary</span></span>

<span data-ttu-id="263e4-184">Bu öğreticide, bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklaması Model bağlayıcı yararlanmak nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="263e4-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="263e4-185">Gerekli gibi Doğrulayıcı öznitelikleri ve StringLength öznitelikleri farklı türleri kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="263e4-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="263e4-186">Ayrıca Microsoft Entity Framework ile çalışırken, bu öznitelikler kullanma hakkında bilgi edindiniz.</span><span class="sxs-lookup"><span data-stu-id="263e4-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="263e4-187">Önceki</span><span class="sxs-lookup"><span data-stu-id="263e4-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
