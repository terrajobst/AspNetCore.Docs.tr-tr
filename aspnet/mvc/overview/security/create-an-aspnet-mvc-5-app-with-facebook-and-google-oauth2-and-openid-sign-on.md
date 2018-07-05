---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 oluşturma Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile uygulama | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, OAuth 2.0 kullanarak bir dış kimlik Doğr kimlik bilgileriyle oturum açmasına olanak tanıyan bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388772"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="26e6a-103">Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile bir ASP.NET MVC 5 uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="26e6a-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="26e6a-104">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="26e6a-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="26e6a-105">Bu öğreticide, kullanarak kullanıcıların oturum açma olanak sağlayan bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir [OAuth 2.0](http://oauth.net/2/) Facebook, Twitter, LinkedIn, Microsoft veya Google gibi bir dış kimlik doğrulama sağlayıcısı'ndan kimlik bilgilerine sahip.</span><span class="sxs-lookup"><span data-stu-id="26e6a-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="26e6a-106">Kolaylık olması için Bu öğreticide, Facebook ve Google kimlik bilgileriyle ile çalışma hakkındaki odaklanır.</span><span class="sxs-lookup"><span data-stu-id="26e6a-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="26e6a-107">Hesapları bu dış sağlayıcıları ile milyonlarca kullanıcıya zaten olduğundan, web sitelerinizi, bu kimlik bilgileri sağlayarak bu önemli bir avantaj sunar.</span><span class="sxs-lookup"><span data-stu-id="26e6a-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="26e6a-108">Bu kullanıcılar, yeni bir dizi kimlik bilgisi oluşturma ve olmadığı siteniz için kaydolmanız daha eilimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="26e6a-109">Ayrıca bkz: [SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="26e6a-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="26e6a-110">Öğreticinin ayrıca kullanıcıya profil verileri ekleme ve üyelik API'si rolleri eklemek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="26e6a-111">Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Lütfen bu bana Twitter'da takip edin: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="26e6a-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="26e6a-112">Başlarken</span><span class="sxs-lookup"><span data-stu-id="26e6a-112">Getting Started</span></span>

<span data-ttu-id="26e6a-113">Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="26e6a-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="26e6a-114">Visual Studio yükleme [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="26e6a-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="26e6a-115">Dropbox, GitHub, LinkedIn, Instagram, arabellek, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! ve daha fazla yardım için bkz. Bu [kodunuzla](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="26e6a-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="26e6a-116">Visual Studio yüklemelisiniz [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya Google OAuth 2'yi kullanın ve yerel olarak SSL uyarılar olmadan hata ayıklamak için daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="26e6a-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="26e6a-117">Tıklayın **yeni proje** gelen **Başlat** sayfasında veya menüyü kullanın ve seçin **dosya**, ardından **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="26e6a-118">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="26e6a-118">Creating Your First Application</span></span>

<span data-ttu-id="26e6a-119">Tıklayın **yeni proje**, ardından **Visual C#** solda, ardından **Web** seçip **ASP.NET Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="26e6a-120">"MvcAuth" projenizi adlandırın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="26e6a-121">İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklayın **MVC**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="26e6a-122">Kimlik doğrulaması değilse **bireysel kullanıcı hesapları**, tıklayın **kimlik doğrulamayı Değiştir** düğmesini tıklatın ve seçin **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="26e6a-123">Denetleyerek **bulutta Barındır**, uygulamayı Azure'da barındırmak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="26e6a-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="26e6a-124">Seçtiyseniz **bulutta Barındır**, yapılandırma iletişim tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="26e6a-125">En son OWIN ara yazılımını güncelleştirmek için NuGet kullanma</span><span class="sxs-lookup"><span data-stu-id="26e6a-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="26e6a-126">Güncelleştirmek için NuGet paket yöneticisini kullanın [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="26e6a-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="26e6a-127">Seçin **güncelleştirmeleri** soldaki menüde.</span><span class="sxs-lookup"><span data-stu-id="26e6a-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="26e6a-128">Tıklayabilirsiniz **Tümünü Güncelleştir** düğmesini veya (sonraki görüntüde gösterilmiştir) OWIN paketleri arayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="26e6a-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="26e6a-129">Aşağıdaki görüntüde, yalnızca OWIN paketler gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="26e6a-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="26e6a-130">Paket Yöneticisi Konsolu (PMC'yi) öğesinden girdiğiniz `Update-Package` komutu tüm paketleri güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="26e6a-131">Tuşuna **F5** veya **Ctrl + F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="26e6a-132">Aşağıdaki görüntüde, bağlantı noktası numarası 1234 ise.</span><span class="sxs-lookup"><span data-stu-id="26e6a-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="26e6a-133">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="26e6a-134">Tarayıcı pencerenizin boyutuna bağlı olarak, görmek için Gezinti simgesine tıklamanız gerekebilir **giriş**, **hakkında**, **kişi**, **kaydetme**ve **oturum** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="26e6a-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="26e6a-135">Projedeki SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="26e6a-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="26e6a-136">Google ve Facebook gibi kimlik doğrulama sağlayıcıları bağlanmak için IIS Express SSL kullanacak şekilde gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="26e6a-137">SSL oturum açtıktan sonra kullanmaya devam edebilirsiniz ve HTTP geri bırak değil önemlidir, oturum açma tanımlama bilgisinin parolası olarak ağ üzerinden düz metin olarak gönderiyorsanız SSL kullanarak olmadan ve kullanıcı adı ve parola olarak.</span><span class="sxs-lookup"><span data-stu-id="26e6a-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="26e6a-138">Yanında, zaten anlaşmasını gerçekleştirmek ve güvenli (HTTPS HTTP yavaş kılan toplu olan) kanal zaman zamandaki MVC ardışık düzeni çalıştırmadan önce bu nedenle oturum açtınız sonra geri HTTP yeniden yönlendirme geçerli istek veya gelecekteki yaratmayacaktır istekler daha hızlı.</span><span class="sxs-lookup"><span data-stu-id="26e6a-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="26e6a-139">İçinde **Çözüm Gezgini**, tıklayın **MvcAuth** proje.</span><span class="sxs-lookup"><span data-stu-id="26e6a-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="26e6a-140">Proje özelliklerini göstermek için F4 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="26e6a-141">Alternatif olarak, gelen **görünümü** seçebileceğiniz menü **Özellikler penceresi**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="26e6a-142">Değişiklik **SSL etkin** true.</span><span class="sxs-lookup"><span data-stu-id="26e6a-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="26e6a-143">SSL URL'sini kopyala (olacak `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).</span><span class="sxs-lookup"><span data-stu-id="26e6a-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="26e6a-144">İçinde **Çözüm Gezgini**, sağ tıklayın **MvcAuth** seçin ve proje **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="26e6a-145">Seçin **Web** sekmesine ve ardından SSL URL'sini yapıştırın **proje URL'si** kutusu.</span><span class="sxs-lookup"><span data-stu-id="26e6a-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="26e6a-146">' % S'dosyası (Ctl + S) kaydedin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="26e6a-147">Facebook ve Google kimlik doğrulama uygulamaları yapılandırmak için bu URL gerekir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="26e6a-148">Ekleme [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) özniteliğini `Home` denetleyicisi gerektiren tüm istekler için HTTPS kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="26e6a-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="26e6a-149">Daha güvenli bir yöntem eklemektir [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) uygulama için filtre.</span><span class="sxs-lookup"><span data-stu-id="26e6a-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="26e6a-150">Bölümüne bakın &quot;SSL ve yetkilendirmek özniteliği ile uygulama koruma&quot; my tutoral içinde [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="26e6a-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="26e6a-151">Giriş denetleyicisine değerinin bir bölümü aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="26e6a-152">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="26e6a-153">Geçmişte sertifika yüklü değilse, bu bölümün geri kalanında atlayın ve atlama [OAuth 2 için Google uygulaması oluşturma ve uygulama projesiyle bağlantı kurulurken](#goog), aksi takdirde otomatik olarak imzalanan güven için yönergeleri izleyin IIS Express'in oluşturduğu sertifika.</span><span class="sxs-lookup"><span data-stu-id="26e6a-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="26e6a-154">Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden bir sertifika yüklemek istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="26e6a-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="26e6a-155">IE gösterir *giriş* sayfasında ve SSL uyarı yok.</span><span class="sxs-lookup"><span data-stu-id="26e6a-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="26e6a-156">Google Chrome, ayrıca sertifikayı kabul eder ve HTTPS içeriğine olmadan bir uyarı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="26e6a-157">Firefox, kendi sertifika deposu kullanır, bu nedenle, bir uyarı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="26e6a-158">Uygulamamız için güvenli bir şekilde tıklayabilirsiniz **riskleri anlıyorum**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="26e6a-159">OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="26e6a-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="26e6a-160">Geçerli Google OAuth yönergeler için bkz: [ASP.NET Core kimlik doğrulaması yapılandırma Google](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="26e6a-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="26e6a-161">Gidin [Google geliştiriciler konsol](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="26e6a-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="26e6a-162">Bir proje önce oluşturmadıysanız, seçin **kimlik bilgilerini** sol sekmeye tıklayın ve ardından içinde **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="26e6a-163">Sol sekmede tıklayın **kimlik bilgilerini**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="26e6a-164">Tıklayın **kimlik bilgilerini oluştur** ardından **OAuth istemcisi kimliği**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="26e6a-165">İçinde **istemci kodu oluşturma** iletişim kutusunda, varsayılan tutun **Web uygulaması** uygulama türü için.</span><span class="sxs-lookup"><span data-stu-id="26e6a-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="26e6a-166">Ayarlama **yetkili JavaScript** kaynakları yukarıda kullanılan SSL URL'sine (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece)</span><span class="sxs-lookup"><span data-stu-id="26e6a-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="26e6a-167">Ayarlama **yetkili yeniden yönlendirme URI'si** için:</span><span class="sxs-lookup"><span data-stu-id="26e6a-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="26e6a-168">OAuth onay ekranı menü öğesini tıklatın ve ardından e-posta adresi ve ürün adı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="26e6a-169">Form tıklayarak bitirdiğinizde **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="26e6a-170">Kitaplık menü öğesini tıklayın, arama **Google + API**, buna tıklayın ardından etkinleştir tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="26e6a-171">Aşağıdaki resimde, etkin API'leri gösterir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="26e6a-172">Google API'leri API Yöneticisi'nden ziyaret **kimlik bilgilerini** almak için sekmesinde **istemci kimliği**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="26e6a-173">Uygulama gizli dizilerini bir JSON dosyası kaydetmek için indirin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="26e6a-174">Kopyalama ve yapıştırma **ClientID** ve **ClientSecret** içine `UseGoogleAuthentication` yöntemi bulunan *Startup.Auth.cs* dosyası *App_Start* klasör.</span><span class="sxs-lookup"><span data-stu-id="26e6a-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="26e6a-175">**ClientID** ve **ClientSecret** aşağıda gösterilen değerleri örnek ve çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="26e6a-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="26e6a-176">Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki.</span><span class="sxs-lookup"><span data-stu-id="26e6a-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="26e6a-177">Örneği basit tutmak için yukarıdaki kod, kimlik bilgilerini ve hesabı eklenir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="26e6a-178">Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="26e6a-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="26e6a-179">Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="26e6a-180">Tıklayın **oturum** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="26e6a-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="26e6a-181">Altında **başka bir hizmete oturum açmak için kullandığınız**, tıklayın **Google**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="26e6a-182">Yukarıdaki adımların kaçırmanıza bir HTTP 401 hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="26e6a-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="26e6a-183">Yukarıdaki adımları uygulamanıza yeniden denetleyin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-183">Recheck your steps above.</span></span> <span data-ttu-id="26e6a-184">Gerekli bir ayar kaçırmanıza (örneğin **ürün adı**), eksik öğe ekleyip kaydedin; kimlik doğrulamasının çalışması için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="26e6a-185">Google kimlik bilgilerinizi gireceğiniz siteye yönlendirileceksiniz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="26e6a-186">Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulamasına izin vermek için istenir:</span><span class="sxs-lookup"><span data-stu-id="26e6a-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="26e6a-187">Tıklayın **kabul**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-187">Click **Accept**.</span></span> <span data-ttu-id="26e6a-188">Artık geri yönlendirilirsiniz **kaydetme** Burada, kaydedebilir Google hesabınızı MvcAuth uygulama sayfası.</span><span class="sxs-lookup"><span data-stu-id="26e6a-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="26e6a-189">Gmail hesabınızı için kullanılan yerel e-posta kayıt adı değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, bir kimlik doğrulaması için kullanılan) tutmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="26e6a-190">Tıklayın **kaydetme**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="26e6a-191">Facebook uygulama oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="26e6a-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="26e6a-192">Geçerli Facebook OAuth2 kimlik doğrulaması hakkında yönergeler için bkz. [yapılandırma Facebook kimlik doğrulaması](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="26e6a-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="26e6a-193">Facebook OAuth2 kimlik doğrulaması için Facebook içinde oluşturduğunuz bir uygulamadan bazı ayarları projenize kopyalamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="26e6a-194">Tarayıcınızda gidin [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) ve oturum açma Facebook kimlik bilgilerinizi girerek.</span><span class="sxs-lookup"><span data-stu-id="26e6a-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="26e6a-195">Bir Facebook geliştirici olarak zaten kayıtlı değil, tıklayın **geliştiricisi olarak kaydolma** ve kaydetmek için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="26e6a-196">Üzerinde **uygulamaları** sekmesinde **yeni uygulama oluştur**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Yeni uygulama oluştur](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="26e6a-198">Girin bir **uygulama adı** ve **kategori**, ardından **uygulama oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="26e6a-199"><strong>Uygulama Namespace</strong> uygulamanıza kimlik doğrulaması için Facebook uygulamaya erişmek için kullanacağı bir URL parçası olan (örneğin, https\://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="26e6a-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="26e6a-200">Belirtmezseniz bir <strong>uygulama Namespace</strong>, <strong>uygulama kimliği</strong> URL için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="26e6a-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="26e6a-201"><strong>Uygulama kimliği</strong> sonraki adımda göreceğiniz uzun sistem tarafından oluşturulan bir sayı.</span><span class="sxs-lookup"><span data-stu-id="26e6a-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Yeni uygulama iletişim kutusu oluşturma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="26e6a-203">Standart güvenlik denetimi gönderin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-203">Submit the standard security check.</span></span>

    ![Güvenlik denetimi](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="26e6a-205">Seçin **ayarları** sol menü çubuğu için![ Facebook geliştiricinin menü çubuğu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="26e6a-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="26e6a-206">Üzerinde **temel** sayfanın ayarları bölümü seçin **Platform Ekle** bir Web sitesi uygulama eklemekte olduğunuz belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="26e6a-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="26e6a-207">![Temel ayarları](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="26e6a-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="26e6a-208">Seçin **Web sitesi** platformu seçeneklerden.</span><span class="sxs-lookup"><span data-stu-id="26e6a-208">Select **Website** from the platform choices.</span></span>  
  
    ![Platform seçenekleri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="26e6a-210">Not, **uygulama kimliği** ve **uygulama gizli anahtarı** böylece daha sonra Bu öğreticide hem MVC uygulamanıza ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="26e6a-211">Ayrıca, sitenizin URL'sini ekleyin (`https://localhost:44300/`) MVC uygulamanızı test etmek için.</span><span class="sxs-lookup"><span data-stu-id="26e6a-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="26e6a-212">Ayrıca, bir **ilgili kişi e-posta**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="26e6a-213">Ardından, **Değişiklikleri Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-213">Then, select **Save Changes**.</span></span>   

    ![Temel Uygulama Ayrıntıları sayfası](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="26e6a-215">Yalnızca kayıtlı e-posta diğer adı kullanarak kimlik doğrulaması için olacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="26e6a-216">Diğer kullanıcılar ve test hesapları kaydetmek mümkün olmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="26e6a-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="26e6a-217">Facebook uygulama diğer Facebook hesapları erişimi verebilir **Geliştirici rolleri** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="26e6a-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="26e6a-218">Visual Studio'da açın *uygulama\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="26e6a-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="26e6a-219">Kopyalama ve yapıştırma **AppID** ve **uygulama gizli anahtarı** içine `UseFacebookAuthentication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="26e6a-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="26e6a-220">**AppID** ve **uygulama gizli anahtarı** aşağıda gösterilen değerler örnekleri ve çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="26e6a-221">Tıklayın **değişiklikleri kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="26e6a-222">Tuşuna **CTRL + F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="26e6a-223">Seçin **oturum** oturum açma sayfasını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="26e6a-224">Tıklayın **Facebook** altında **oturum açmak için başka bir hizmet kullanın.**</span><span class="sxs-lookup"><span data-stu-id="26e6a-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="26e6a-225">Facebook kimlik bilgilerinizi girin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="26e6a-226">Genel profiliniz ve arkadaş listesi erişmek uygulamayı izni istenir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook uygulama ayrıntıları](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="26e6a-228">Artık oturum açtınız.</span><span class="sxs-lookup"><span data-stu-id="26e6a-228">You are now logged in.</span></span> <span data-ttu-id="26e6a-229">Bu hesap artık uygulama ile kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="26e6a-230">Kaydolduğunuzda, bir giriş eklenen *kullanıcılar* üyelik veritabanının tablo.</span><span class="sxs-lookup"><span data-stu-id="26e6a-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="26e6a-231">Üyelik verilerini İnceleme</span><span class="sxs-lookup"><span data-stu-id="26e6a-231">Examine the Membership Data</span></span>

<span data-ttu-id="26e6a-232">İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="26e6a-233">Genişletin **DefaultConnection (MvcAuth)**, genişletme **tabloları**, sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tablo verileri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="26e6a-235">Kullanıcı profil verileri ekleme</span><span class="sxs-lookup"><span data-stu-id="26e6a-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="26e6a-236">Bu bölümde, doğum tarihi ve başlangıç belediye kullanıcı verilerini kayıt sırasında aşağıdaki görüntüde gösterildiği gibi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![Giriş Şehir ve Bday ile kayıt defteri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="26e6a-238">Açık *Models\IdentityModels.cs* dosya ve doğum tarihi ve giriş Şehir özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="26e6a-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="26e6a-239">Açık *Models\AccountViewModels.cs* dosyasının ve küme doğum tarihi ve giriş Şehir özelliklerinde `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="26e6a-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="26e6a-240">Açık *Controllers\AccountController.cs* dosya ve doğum tarihi ve giriş şehir için kod ekleyin `ExternalLoginConfirmation` gösterildiği gibi bir eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="26e6a-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="26e6a-241">Doğum tarihi ve giriş şehir için ekleme *Views\Account\ExternalLoginConfirmation.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="26e6a-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="26e6a-242">Yeniden uygulamanızla Facebook hesabınızı kaydedin ve yeni bir doğum tarihi ve başlangıç belediye profil bilgilerini ekleyebilirsiniz doğrulayın üyelik veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="26e6a-243">Gelen **Çözüm Gezgini**, tıklayın **tüm dosyaları göster** simgesine ve ardından sağ tıklama *Ekle\_Data\aspnet-MvcAuth -&lt;tarih damgası&gt;.mdf* tıklatıp **Sil**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="26e6a-244">Gelen **Araçları** menüsünde tıklayın **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu** (PMC).</span><span class="sxs-lookup"><span data-stu-id="26e6a-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="26e6a-245">PMC'de aşağıdaki komutları girin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="26e6a-246">Geçişleri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="26e6a-246">Enable-Migrations</span></span>
2. <span data-ttu-id="26e6a-247">Geçiş başlatma</span><span class="sxs-lookup"><span data-stu-id="26e6a-247">Add-Migration Init</span></span>
3. <span data-ttu-id="26e6a-248">Veritabanını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="26e6a-248">Update-Database</span></span>

<span data-ttu-id="26e6a-249">Uygulamayı çalıştırmak ve FaceBook ve Google oturum açmak ve bazı kullanıcılar kaydetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="26e6a-250">Üyelik verilerini İnceleme</span><span class="sxs-lookup"><span data-stu-id="26e6a-250">Examine the Membership Data</span></span>

<span data-ttu-id="26e6a-251">İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="26e6a-252">Sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.</span><span class="sxs-lookup"><span data-stu-id="26e6a-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="26e6a-253">`HomeTown` Ve `BirthDate` alanları aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="26e6a-254">Uygulamanızı günlüğe kaydetme ve başka bir hesap ile günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="26e6a-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="26e6a-255">Uygulamanızı, Facebook ile oturum açın ve ardından Oturumu Kapat ve oturum açmayı deneyin yeniden (aynı tarayıcıyı kullanarak), farklı bir Facebook hesabıyla, hemen kullandığınız önceki Facebook hesabıyla oturum açmanız.</span><span class="sxs-lookup"><span data-stu-id="26e6a-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="26e6a-256">Başka bir hesap kullanmak için Facebook'a gidin ve Facebook oturumunuzu gerekir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="26e6a-257">Aynı kural, tüm 3 taraf kimlik sağlayıcıları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="26e6a-258">Alternatif olarak, farklı bir tarayıcı kullanarak başka bir hesapla oturum oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="26e6a-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="26e6a-259">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="26e6a-259">Next Steps</span></span>

<span data-ttu-id="26e6a-260">Bkz: [Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları için OWIN giriş](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeleri tarafından.</span><span class="sxs-lookup"><span data-stu-id="26e6a-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="26e6a-261">Jerrie'nın bkz Yapıyorsak etkinleştir sosyal oturum açma düğmeleri almak ASP.NET MVC 5 sosyal oturum açma düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="26e6a-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="26e6a-262">My öğreticiyi izleyin [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), bu öğreticiye devam eder ve şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="26e6a-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="26e6a-263">Uygulamanızı Azure'a dağıtmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="26e6a-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="26e6a-264">Uygulama rolleri ile güvenliğini sağlama.</span><span class="sxs-lookup"><span data-stu-id="26e6a-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="26e6a-265">Uygulamanız ile güvenli hale getirmeyi [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) ve [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtreler.</span><span class="sxs-lookup"><span data-stu-id="26e6a-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="26e6a-266">Kullanıcıları ve rolleri eklemek için üyelik API'si kullanma</span><span class="sxs-lookup"><span data-stu-id="26e6a-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="26e6a-267">Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="26e6a-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="26e6a-268">Yeni konuları da isteyebilirsiniz [Show Me nasıl ile kod](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="26e6a-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="26e6a-269">Hatta isteyin ve ASP.NET için eklenecek yeni özellikleri oy verin.</span><span class="sxs-lookup"><span data-stu-id="26e6a-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="26e6a-270">Örneğin, bir aracı için oy verebilirsiniz [kullanıcıları ve rolleri oluşturun ve yönetin.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="26e6a-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="26e6a-271">ASP.NET Dış kimlik doğrulama hizmetleri nasıl çalışır bir iyi açıklama için bkz: Robert McMurray'nın [Dış kimlik doğrulama hizmetleri](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="26e6a-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="26e6a-272">Robert'ın makale aynı zamanda Microsoft ve Twitter kimlik doğrulamasının etkinleştirilmesinde ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="26e6a-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="26e6a-273">Tom Dykstra mükemmel [EF/MVC Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Entity Framework ile çalışma hakkında bilgi verilmektedir.</span><span class="sxs-lookup"><span data-stu-id="26e6a-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
