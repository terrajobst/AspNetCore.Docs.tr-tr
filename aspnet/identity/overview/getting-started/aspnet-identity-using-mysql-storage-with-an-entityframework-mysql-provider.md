---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Bir EntityFramework MySQL sağlayıcısı ile (C#) MySQL depolama kullanma | Microsoft Docs'
author: maumar
description: Bu öğretici, ASP.NET kimliği için varsayılan veri depolama mekanizmasını EntityFramework (SQL istemcisi sağlayıcısı) ile bir MySQL sağlama ile nasıl değiştirileceğini gösterir...
ms.author: aspnetcontent
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 13cb1d9a862429bfb4e20a8d9700fc17eb721c6b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812681"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="63c1b-103">ASP.NET Identity: (C#) bir EntityFramework MySQL sağlayıcısı ile MySQL depolama kullanma</span><span class="sxs-lookup"><span data-stu-id="63c1b-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="63c1b-104">tarafından [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray '](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="63c1b-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="63c1b-105">Bu öğretici için varsayılan veri depolama mekanizmasını nasıl değiştirileceğini gösterir [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) (SQL istemcisi sağlayıcısı) bir EntityFramework MySQL sağlayıcısı ile ile.</span><span class="sxs-lookup"><span data-stu-id="63c1b-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="63c1b-106">Bu öğreticide aşağıdaki konularda ele alınacak:</span><span class="sxs-lookup"><span data-stu-id="63c1b-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="63c1b-107">Azure üzerinde MySQL veritabanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="63c1b-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="63c1b-108">Visual Studio 2013 MVC şablonu kullanarak bir MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="63c1b-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="63c1b-109">EntityFramework MySQL veritabanı sağlayıcısı ile çalışacak şekilde yapılandırma</span><span class="sxs-lookup"><span data-stu-id="63c1b-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="63c1b-110">Sonuçları doğrulamak için uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="63c1b-110">Running the application to verify the results</span></span>

<span data-ttu-id="63c1b-111">Bu öğreticinin sonunda, Azure'da barındırılan bir MySQL veritabanını kullanan depolama, ASP.NET kimliğe sahip bir MVC uygulaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="63c1b-112">Azure üzerinde MySQL veritabanı örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="63c1b-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="63c1b-113">Oturum [Azure portalında](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="63c1b-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="63c1b-114">Tıklayın **yeni** sayfasına tıklayın ve ardından sayfanın alt kısmında **deposu**:</span><span class="sxs-lookup"><span data-stu-id="63c1b-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="63c1b-115">İçinde **seçin ve eklentinin** seçin **ClearDB MySQL veritabanı**ve ardından **sonraki** çerçevenin altta bulunan oka:</span><span class="sxs-lookup"><span data-stu-id="63c1b-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="63c1b-116">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-116">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-117">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="63c1b-118">Varsayılan tutun **ücretsiz** planlayın, değiştirme **adı** için **IdentityMySQLDatabase**en yakın olan bölgeyi seçin ve ardından **sonraki** çerçevenin altta bulunan oka:</span><span class="sxs-lookup"><span data-stu-id="63c1b-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="63c1b-119">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-119">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-120">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="63c1b-121">Tıklayın **satın alma** veritabanını oluşturmayı tamamlamak için onay işareti.</span><span class="sxs-lookup"><span data-stu-id="63c1b-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="63c1b-122">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-122">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-123">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="63c1b-124">Veritabanınız oluşturulduktan sonra buradan yönetebilirsiniz **eklentileri** Yönetim Portalı'nda sekmesi.</span><span class="sxs-lookup"><span data-stu-id="63c1b-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="63c1b-125">Veritabanınız için bağlantı bilgilerini almak için tıklayın **bağlantı bilgisi** sayfanın alt kısmındaki:</span><span class="sxs-lookup"><span data-stu-id="63c1b-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="63c1b-126">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-126">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-127">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="63c1b-128">Tarafından Kopyala düğmesine tıklayarak bağlantı dizesini kopyalayın **CONNECTIONSTRING** alan ve kaydedin; MVC uygulama için bu öğreticinin sonraki adımlarında bu bilgileri kullanın:</span><span class="sxs-lookup"><span data-stu-id="63c1b-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="63c1b-129">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-129">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-130">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="63c1b-131">Bir MVC uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="63c1b-131">Creating an MVC application project</span></span>

<span data-ttu-id="63c1b-132">Öğreticinin bu bölümünde adımları tamamlamak için önce yüklemeniz gerekir [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="63c1b-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="63c1b-133">Visual Studio yüklendikten sonra yeni bir MVC uygulaması projesi oluşturmak için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="63c1b-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="63c1b-134">Visual Studio 2103 açın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="63c1b-135">Tıklayın **yeni proje** gelen **Başlat** sayfası veya tıklayabilirsiniz **dosya** menüsünü ve ardından **yeni proje**:</span><span class="sxs-lookup"><span data-stu-id="63c1b-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="63c1b-136">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-136">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-137">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="63c1b-138">Zaman **yeni proje** iletişim kutusu görüntülenir, genişletme **Visual C#** şablonları listesinde, ardından **Web**seçip **ASP.NET Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="63c1b-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="63c1b-139">Projenizi adlandırın **IdentityMySQLDemo** ve ardından **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="63c1b-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="63c1b-140">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-140">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-141">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="63c1b-142">İçinde **yeni ASP.NET projesi** iletişim kutusunda **MVC** templatewith varsayılan seçenekleri; bu işlem yapılandırma **bireysel kullanıcı hesapları** kimlik doğrulama yöntemi olarak.</span><span class="sxs-lookup"><span data-stu-id="63c1b-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="63c1b-143">Tıklayın **Tamam**:</span><span class="sxs-lookup"><span data-stu-id="63c1b-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="63c1b-144">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-144">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-145">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="63c1b-146">EntityFramework MySQL veritabanı ile çalışmak üzere yapılandırın</span><span class="sxs-lookup"><span data-stu-id="63c1b-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="63c1b-147">Projeniz için Entity Framework derlemeyi güncelleştirmek</span><span class="sxs-lookup"><span data-stu-id="63c1b-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="63c1b-148">Visual Studio 2013 şablondan oluşturulmuş bir MVC uygulaması için başvuru içerir [EntityFramework 6.0.0'dan](http://www.nuget.org/packages/EntityFramework) paketini, ancak sahip içeren derlemeye kendi sürümden sonraki güncelleştirmeleri önemli bırakıldı performans iyileştirmeleri.</span><span class="sxs-lookup"><span data-stu-id="63c1b-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="63c1b-149">Bu en son güncelleştirmeler, uygulamanızda kullanmak için aşağıdaki adımları kullanın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="63c1b-150">Visual Studio 2013'te MVC projenizi açın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="63c1b-151">Tıklayın **Araçları**, ardından **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="63c1b-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="63c1b-152">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-152">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-153">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="63c1b-154">**Paket Yöneticisi Konsolu** Visual Studio'nun alt kısmında görünür.</span><span class="sxs-lookup"><span data-stu-id="63c1b-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="63c1b-155">Tür &quot; **Update-Package EntityFramework** &quot; ve Enter tuşuna basın:</span><span class="sxs-lookup"><span data-stu-id="63c1b-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="63c1b-156">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-156">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-157">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="63c1b-158">EntityFramework MySQL sağlayıcısı yükleyin</span><span class="sxs-lookup"><span data-stu-id="63c1b-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="63c1b-159">EntityFramework MySQL veritabanına bağlanmak bir MySQL sağlayıcısı yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="63c1b-160">Bunu yapmak için açık **Paket Yöneticisi Konsolu** ve türü &quot; **Install-Package MySql.Data.Entity - öncesi**&quot;, ve ardından Enter tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="63c1b-161">Bu derleme yayın öncesi sürümü olduğu ve bu nedenle hataları içerebilir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="63c1b-162">Provider'ın yayım öncesi sürümü üretim ortamında kullanmamalısınız.</span><span class="sxs-lookup"><span data-stu-id="63c1b-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="63c1b-163">[Genişletmek için aşağıdaki görüntüye tıklayın.]</span><span class="sxs-lookup"><span data-stu-id="63c1b-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="63c1b-164">Uygulamanız için Web.config dosyasının proje yapılandırma değişiklikleri yapma</span><span class="sxs-lookup"><span data-stu-id="63c1b-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="63c1b-165">Bu bölümde, yeni yüklediğiniz MySQL sağlayıcısı kullanmak için Entity Framework yapılandıracağınız MySQL sağlayıcısı üreteci kaydettirir ve Azure'dan bağlantı dizenizi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63c1b-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="63c1b-166">Aşağıdaki örnekler için MySql.Data.dll belirli derleme sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="63c1b-167">Derleme sürümü değişirse, doğru sürümünü uygun yapılandırma ayarlarını değiştirmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="63c1b-168">Visual Studio 2013'te projeniz için Web.config dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="63c1b-169">Fabrika ve varsayılan veritabanı sağlayıcısı için Entity Framework tanımlamak aşağıdaki yapılandırma ayarları bulun:</span><span class="sxs-lookup"><span data-stu-id="63c1b-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="63c1b-170">Bu yapılandırma ayarlarını ve MySQL sağlayıcıyı kullanmak için Entity Framework yapılandıracağınız aşağıdaki ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="63c1b-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="63c1b-171">Bulun &lt;connectionStrings&gt; bölümünde ve Azure'da barındırılan, MySQL veritabanı için bağlantı dizesini tanımlar aşağıdaki kod ile değiştirin (providerName değeri de değiştirilmiştir olduğunu unutmayın. özgün):</span><span class="sxs-lookup"><span data-stu-id="63c1b-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="63c1b-172">Özel MigrationHistory bağlam ekleme</span><span class="sxs-lookup"><span data-stu-id="63c1b-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="63c1b-173">Entity Framework Code First kullanan bir **MigrationHistory** tablo modeli değişiklikleri izlemek ve kavramsal şema ve veritabanı şeması arasındaki tutarlılığı sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="63c1b-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="63c1b-174">Ancak, bu tabloda birincil anahtar çok büyük olduğu için varsayılan olarak MySQL için çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="63c1b-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="63c1b-175">Bu durumu ortadan kaldırmak için bu tablo için anahtar boyutunu küçültmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="63c1b-176">Bunu yapmak için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="63c1b-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="63c1b-177">Bu tablo için şema bilgileri yakalanan bir **HistoryContext**, değiştiren herhangi diğer olabilen **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="63c1b-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="63c1b-178">Bunu yapmak için adlı yeni bir sınıf dosyası ekleyin. **MySqlHistoryContext.cs** projesine ve içeriğini aşağıdaki kodla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="63c1b-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="63c1b-179">Sonraki değiştirilen kullanmak için Entity Framework yapılandırmanız gerekecektir **HistoryContext**, varsayılanın yerine.</span><span class="sxs-lookup"><span data-stu-id="63c1b-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="63c1b-180">Bu, kod tabanlı yapılandırma özelliklerden yararlanarak yapılabilir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="63c1b-181">Bunu yapmak için adlı yeni bir sınıf dosyası ekleyin. **MySqlConfiguration.cs** projenize ve içeriğini ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="63c1b-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="63c1b-182">Özel bir EntityFramework Başlatıcı ApplicationDbContext için oluşturma</span><span class="sxs-lookup"><span data-stu-id="63c1b-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="63c1b-183">Model başlatıcılar veritabanına bağlanmak için kullanılacak ihtiyacınız olacak şekilde Bu öğreticide öne çıkan MySQL sağlayıcısı Entity Framework geçişleri, şu anda desteklemiyor.</span><span class="sxs-lookup"><span data-stu-id="63c1b-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="63c1b-184">Bu öğretici, Azure'da bir MySQL örneği kullandığından, özel bir varlık çerçevesi Başlatıcı oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="63c1b-185">Azure'da veya içinde barındırılan bir veritabanı kullanıyorsanız, bir SQL Server örneğine bağlanıyorsanız Bu adım gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="63c1b-186">MySQL için özel bir varlık çerçevesi Başlatıcı oluşturmak için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="63c1b-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="63c1b-187">Adlı yeni bir sınıf dosyası ekleyin **MySqlInitializer.cs** içeriğini aşağıdaki kodla proje ve Değiştir olduğu:</span><span class="sxs-lookup"><span data-stu-id="63c1b-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="63c1b-188">Açık **IdentityModels.cs** bulunan projeniz için dosya **modelleri** dizin ve onun içeriğini aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="63c1b-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="63c1b-189">Uygulamayı çalıştıran ve veritabanı doğrulanıyor</span><span class="sxs-lookup"><span data-stu-id="63c1b-189">Running the application and verifying the database</span></span>

<span data-ttu-id="63c1b-190">Önceki bölümlerde yer alan adımları tamamladıktan sonra veritabanınızı test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="63c1b-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="63c1b-191">Bunu yapmak için aşağıdaki adımları kullanın:</span><span class="sxs-lookup"><span data-stu-id="63c1b-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="63c1b-192">Tuşuna **Ctrl + F5 tuşlarına basarak** web uygulaması derleme ve çalıştırma için.</span><span class="sxs-lookup"><span data-stu-id="63c1b-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="63c1b-193">Tıklayın **kaydetme** sayfanın üst kısmındaki sekme:</span><span class="sxs-lookup"><span data-stu-id="63c1b-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="63c1b-194">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-194">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-195">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="63c1b-196">Yeni kullanıcı adı ve parola girin ve ardından **kaydetme**:</span><span class="sxs-lookup"><span data-stu-id="63c1b-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="63c1b-197">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-197">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-198">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="63c1b-199">Bu noktada ASP.NET Identity tabloları üzerinde MySQL veritabanı oluşturulur ve kullanıcı kayıtlı ve uygulamaya oturum:</span><span class="sxs-lookup"><span data-stu-id="63c1b-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="63c1b-200">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-200">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-201">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="63c1b-202">Verileri doğrulamak için MySQL Workbench aracını yükleme</span><span class="sxs-lookup"><span data-stu-id="63c1b-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="63c1b-203">Yükleme **MySQL Workbench** gelen aracı [MySQL yükleme](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="63c1b-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="63c1b-204">Yükleme Sihirbazı: **özellik seçimi** sekmesinde **MySQL Workbench** altında **uygulamaları** bölümü.</span><span class="sxs-lookup"><span data-stu-id="63c1b-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="63c1b-205">Uygulamayı başlatın ve begging Bu öğretici oluşturduğunuz Azure MySQL veritabanı bağlantı dizesi verilerini kullanarak yeni bir bağlantı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="63c1b-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="63c1b-206">Bağlantı kurulduktan sonra İnceleme **ASP.NET Identity** üzerinde oluşturulan tablolar **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="63c1b-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="63c1b-207">Tüm ASP.NET Identity tablolarının oluşturulması aşağıdaki resimde gösterildiği gibi gerekli olduğunu görürsünüz:</span><span class="sxs-lookup"><span data-stu-id="63c1b-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="63c1b-208">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-208">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-209">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="63c1b-210">İnceleme **aspnetusers** tablo örneği için yeni kullanıcılar kaydetme gibi girişler için denetlenecek.</span><span class="sxs-lookup"><span data-stu-id="63c1b-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="63c1b-211">[Genişletmek için aşağıdaki görüntüye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="63c1b-211">[Click the following image to expand it.</span></span> <span data-ttu-id="63c1b-212">]</span><span class="sxs-lookup"><span data-stu-id="63c1b-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
