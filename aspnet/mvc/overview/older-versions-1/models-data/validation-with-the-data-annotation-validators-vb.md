---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: (VB) veri ek açıklama doğrulayıcıları ile doğrulama | Microsoft Docs
author: microsoft
description: Bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklama Model bağlayıcı yararlanın. Doğrulayıcı farklı türleri kullanmayı öğrenin...
ms.author: aspnetcontent
ms.date: 05/29/2009
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 9d76596ede0763bde3c84dca4a545be250a3062c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837429"
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="a7097-104">(VB) veri ek açıklama doğrulayıcıları ile doğrulama</span><span class="sxs-lookup"><span data-stu-id="a7097-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="a7097-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="a7097-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="a7097-106">Bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklama Model bağlayıcı yararlanın.</span><span class="sxs-lookup"><span data-stu-id="a7097-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="a7097-107">Doğrulayıcı öznitelikleri farklı türde ve bunlarla çalışan Microsoft varlık Çerçevesi'nde nasıl kullanılacağını öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a7097-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="a7097-108">Bu öğreticide bir ASP.NET MVC uygulamasındaki doğrulamayı gerçekleştirmek için veri ek açıklama doğrulayıcıları kullanmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a7097-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="a7097-109">Veri ek açıklama doğrulayıcıları kullanmanın avantajı, bunlar, yalnızca ekleyerek – gerekli gibi bir veya daha fazla öznitelikleri veya StringLength öznitelik – bir sınıf özelliği için doğrulama gerçekleştirmek etkinleştirmenizi ' dir.</span><span class="sxs-lookup"><span data-stu-id="a7097-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="a7097-110">Veri ek açıklama doğrulayıcıları kullanabilmeniz için veri ek açıklamaları Model bağlayıcısını yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="a7097-111">Veri ek açıklamaları Model bağlayıcı örneği tıklayarak CodePlex Web sitesinden indirebilirsiniz [burada](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span><span class="sxs-lookup"><span data-stu-id="a7097-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="a7097-112">Veri ek açıklamaları Model bağlayıcı Microsoft ASP.NET MVC çerçevesi resmi bir parçası olmadığını anlamak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="a7097-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="a7097-113">Veri ek açıklamaları Model bağlayıcı Microsoft ASP.NET MVC ekibi tarafından oluşturulmuş olsa da, Microsoft resmi ürün desteği için veri ek açıklamaları Model bağlayıcı açıklanmış ve Bu öğreticide kullanılan sunmaz.</span><span class="sxs-lookup"><span data-stu-id="a7097-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="a7097-114">Veri ek açıklama Model Bağlayıcısı kullanma</span><span class="sxs-lookup"><span data-stu-id="a7097-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="a7097-115">Bir ASP.NET MVC uygulamasındaki veri ek açıklamaları Model bağlayıcısını kullanmak için öncelikle Microsoft.Web.Mvc.DataAnnotations.dll derleme ve System.ComponentModel.DataAnnotations.dll derlemeye bir başvuru eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="a7097-116">Menü seçeneğini **proje, Başvuru Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a7097-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="a7097-117">İleri'yi **Gözat** sekmesini ve indirdiğiniz (ve sıkıştırması açılan) veri ek açıklamaları Model bağlayıcı örneği konumuna göz atın (bkz **Şekil 1**).</span><span class="sxs-lookup"><span data-stu-id="a7097-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="a7097-118">**Şekil 1**: veri ek açıklamaları Model bağlayıcı için bir başvuru eklemeyi ([tam boyutlu görüntüyü görmek için tıklatın](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a7097-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="a7097-119">Hem Microsoft.Web.Mvc.DataAnnotations.dll derleme hem de System.ComponentModel.DataAnnotations.dll derlemeyi seçin ve tıklayın **Tamam** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a7097-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="a7097-120">.NET Framework Service Pack 1 veri ek açıklamaları Model Bağlayıcısı ile birlikte System.ComponentModel.DataAnnotations.dll derleme kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="a7097-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="a7097-121">Veri ek açıklamaları Model bağlayıcı örneği indirmeye dahil System.ComponentModel.DataAnnotations.dll derleme sürümünü kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="a7097-122">Son olarak, Global.asax dosyasında DataAnnotations Model bağlayıcı kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="a7097-123">Uygulamaya aşağıdaki kod satırını ekleyin\_Start() olay işleyicisi böylece uygulamanın\_Start() yöntemini aşağıdaki gibi görünür:</span><span class="sxs-lookup"><span data-stu-id="a7097-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="a7097-124">Bu kod satırı DataAnnotationsModelBinder tüm ASP.NET MVC uygulaması için varsayılan model bağlayıcısını olarak kaydeder.</span><span class="sxs-lookup"><span data-stu-id="a7097-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="a7097-125">Veri ek açıklama Doğrulayıcı öznitelikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="a7097-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="a7097-126">Veri ek açıklamaları Model bağlayıcısını kullandığınızda, doğrulama gerçekleştirmek için doğrulama öznitelikleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="a7097-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="a7097-127">System.ComponentModel.DataAnnotations ad alanı, aşağıdaki doğrulama öznitelikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="a7097-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="a7097-128">Aralığı – belirtilen bir aralıktaki değerleri arasında bir özelliğin değerini denk olup olmadığını doğrulamak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7097-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="a7097-129">ReqularExpression – bir özelliğin değerini belirtilen normal ifade deseni ile eşleşip eşleşmediğini doğrulamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7097-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="a7097-130">Gerekli: bir özelliği gerekli olarak işaretlemek sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7097-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="a7097-131">StringLength – bir dize özelliği için en fazla uzunluk belirtmenize imkan tanır.</span><span class="sxs-lookup"><span data-stu-id="a7097-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="a7097-132">Doğrulama – tüm Doğrulayıcı öznitelikleri için temel sınıfı.</span><span class="sxs-lookup"><span data-stu-id="a7097-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7097-133">Ardından, doğrulama gereksinimlerinizi standart doğrulayıcıları biriyle tatmin edici değil her zaman yeni bir doğrulayıcı öznitelik temel doğrulama özniteliği devralarak özel Doğrulayıcı sağlayıcısı öznitelik oluşturma seçeneğiniz vardır.</span><span class="sxs-lookup"><span data-stu-id="a7097-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="a7097-134">Ürün sınıfında **listeleme 1** bu Doğrulayıcı özniteliklerinin nasıl kullanılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a7097-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="a7097-135">Ad, açıklama ve UnitPrice özellikler işaretlendi gerektiğinde.</span><span class="sxs-lookup"><span data-stu-id="a7097-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="a7097-136">Name özelliği, bir dize uzunluğu 10 karakter olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a7097-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="a7097-137">Son olarak, UnitPrice özelliği bir para birimi tutarını gösteren bir normal ifade deseni eşleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="a7097-138">**1 listeleme**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="a7097-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="a7097-139">Ürün sınıfı bir ek özellik nasıl kullanıldığı gösterilmektedir: DisplayName özniteliğini.</span><span class="sxs-lookup"><span data-stu-id="a7097-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="a7097-140">DisplayName özniteliğini özelliği bir hata iletisi görüntülendiğinde, özelliğin adını değiştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="a7097-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="a7097-141">Hata iletisi "UnitPrice alanını gerekiyor" yerine "Fiyat alanının gerekiyor" hata iletisini görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7097-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7097-142">Tamamen Doğrulayıcısı tarafından görüntülenen hata iletisini özelleştirmek istiyorsanız Doğrulayıcısı'nın ErrorMessage özelliğine benzer bir özel hata iletisi atayabilirsiniz: `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="a7097-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="a7097-143">Ürün sınıfında kullanabileceğiniz **listeleme 1** Create() denetleyici eylemi ile **listeleme 2**.</span><span class="sxs-lookup"><span data-stu-id="a7097-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="a7097-144">Model durumu hataları içerdiğinde Bu denetleyici Eylem oluştur görünümünün görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a7097-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="a7097-145">**Kod 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="a7097-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="a7097-146">Son olarak, görünümde oluşturabilirsiniz **listeleme 3** Create() eylem sağ tıklayıp menü seçeneğini belirleyerek **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="a7097-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="a7097-147">Kesin türü belirtilmiş görünüm ürün sınıfıyla model sınıfı oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a7097-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="a7097-148">Seçin **Oluştur** görünümü içerik açılır listede (bkz **Şekil 2**).</span><span class="sxs-lookup"><span data-stu-id="a7097-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="a7097-149">**Şekil 2**: oluşturma görünümü ekleme</span><span class="sxs-lookup"><span data-stu-id="a7097-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="a7097-150">**Kod 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="a7097-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7097-151">Tarafından oluşturulan form oluştur Kimliği alanı kaldırma **Görünüm Ekle** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="a7097-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="a7097-152">Kimliği alanı bir kimlik sütununa karşılık olmadığından, bu alan için bir değer girin izin vermek istemezsiniz.</span><span class="sxs-lookup"><span data-stu-id="a7097-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="a7097-153">Bir ürün oluşturmak için form gönderme ve gerekli alanlar için değerler girin değil durumunda doğrulama hatası iletilerini **Şekil 3** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a7097-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="a7097-154">**Şekil 3**: gerekli alanlar eksik</span><span class="sxs-lookup"><span data-stu-id="a7097-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="a7097-155">Geçersiz bir para birimi tutarı ve hata iletisinde girerseniz **Şekil 4** görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="a7097-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="a7097-156">**Şekil 4**: geçersiz bir para birimi tutarı</span><span class="sxs-lookup"><span data-stu-id="a7097-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="a7097-157">Veri ek açıklama doğrulayıcıları ile Entity Framework kullanma</span><span class="sxs-lookup"><span data-stu-id="a7097-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="a7097-158">Microsoft Entity Framework veri modeli sınıfları oluşturmak için kullanıyorsanız, doğrudan sınıflarınızı Doğrulayıcı öznitelikleri uygulanamıyor.</span><span class="sxs-lookup"><span data-stu-id="a7097-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="a7097-159">Entity Framework Designer model sınıfları oluşturur, çünkü tasarımcıda herhangi bir değişiklik bir sonraki açışınızda model sınıflarına yaptığınız tüm değişiklikler yazılır.</span><span class="sxs-lookup"><span data-stu-id="a7097-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="a7097-160">Entity Framework tarafından oluşturulan sınıflar doğrulayıcıları kullanmak istiyorsanız, meta veri sınıfları oluşturmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="a7097-161">Doğrulayıcıları doğrulayıcıları gerçek sınıf için uygulama yerine meta veri sınıfı için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a7097-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="a7097-162">Örneğin, Entity Framework kullanarak bir film sınıf oluşturduğunuz düşünün (bkz **Şekil 5**).</span><span class="sxs-lookup"><span data-stu-id="a7097-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="a7097-163">Ayrıca, film adı ve Direktörü özellikleri gerekli özellikleri yapmak istediğinizi düşünün.</span><span class="sxs-lookup"><span data-stu-id="a7097-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="a7097-164">Bu durumda, kısmi sınıf ve meta veri sınıfında oluşturabilirsiniz **listeleme 4**.</span><span class="sxs-lookup"><span data-stu-id="a7097-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="a7097-165">**Şekil 5**: Entity Framework tarafından oluşturulan film sınıfı</span><span class="sxs-lookup"><span data-stu-id="a7097-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="a7097-166">**4 listeleme**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="a7097-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="a7097-167">Dosyada **listeleme 4** film ve MovieMetaData adlı iki sınıf içerir.</span><span class="sxs-lookup"><span data-stu-id="a7097-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="a7097-168">Kısmi bir sınıf film sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a7097-168">The Movie class is a partial class.</span></span> <span data-ttu-id="a7097-169">DataModel.Designer.vb dosyasında yer alan Entity Framework tarafından üretilen kısmi sınıf karşılık gelir.</span><span class="sxs-lookup"><span data-stu-id="a7097-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="a7097-170">Şu anda, .NET framework kısmi özellikleri desteklemez.</span><span class="sxs-lookup"><span data-stu-id="a7097-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="a7097-171">Bu nedenle, Doğrulayıcı öznitelikleri dosyasında tanımlanan film sınıf özelliklerini Doğrulayıcı öznitelikleri uygulayarak DataModel.Designer.vb dosyasında tanımlanan film sınıf özelliklerini uygulanacak bir yolu yoktur **4listeleme**.</span><span class="sxs-lookup"><span data-stu-id="a7097-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="a7097-172">Film kısmi sınıf'ın MovieMetaData sınıfa işaret eden MetadataType özniteliğiyle donatılmış dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a7097-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="a7097-173">MovieMetaData sınıfı proxy özellikleri film sınıf özelliklerini içerir.</span><span class="sxs-lookup"><span data-stu-id="a7097-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="a7097-174">Doğrulayıcı öznitelikleri MovieMetaData sınıfının özelliklerine uygulanır.</span><span class="sxs-lookup"><span data-stu-id="a7097-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="a7097-175">Başlık, Müdür ve DateReleased özellikleri tüm gerekli özellikleri olarak işaretlenir.</span><span class="sxs-lookup"><span data-stu-id="a7097-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="a7097-176">5'ten daha az karakter içeren bir dize Yöneticisi özelliği atanması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a7097-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="a7097-177">Son olarak, DisplayName özniteliğini "tarih Serbest alan gereklidir."gibi bir hata iletisi görüntülemek için DateReleased özelliğine uygulanır</span><span class="sxs-lookup"><span data-stu-id="a7097-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="a7097-178">hata yerine "DateReleased alan gereklidir."</span><span class="sxs-lookup"><span data-stu-id="a7097-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="a7097-179">Proxy özellikleri MovieMetaData sınıfında aynı türlerine karşılık gelen özelliklerinde film sınıfı temsil eden gerekmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="a7097-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="a7097-180">Örneğin, Müdür film sınıfındaki bir dize özelliğini ve bir nesne özelliği MovieMetaData sınıfında özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="a7097-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="a7097-181">Sayfanın **Şekil 6** film özelliklerini geçersiz değerler girdiğinizde, döndürülen hata iletilerini gösterir.</span><span class="sxs-lookup"><span data-stu-id="a7097-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="a7097-182">**Şekil 6**: doğrulayıcıları ile Entity Framework kullanarak ([tam boyutlu görüntüyü görmek için tıklatın](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="a7097-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="a7097-183">Özet</span><span class="sxs-lookup"><span data-stu-id="a7097-183">Summary</span></span>

<span data-ttu-id="a7097-184">Bu öğreticide bir ASP.NET MVC uygulaması içindeki doğrulama gerçekleştirmek için veri ek açıklama Model bağlayıcı yararlanmak öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="a7097-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="a7097-185">Farklı türde gerekli gibi Doğrulayıcı öznitelikleri ve StringLength öznitelikleri kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="a7097-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="a7097-186">Ayrıca Microsoft Entity Framework ile çalışırken bu öznitelikler kullanmayı öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="a7097-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="a7097-187">Önceki</span><span class="sxs-lookup"><span data-stu-id="a7097-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
