---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 oluşturma Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile uygulama | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, OAuth 2.0 kullanarak bir dış kimlik Doğr kimlik bilgileriyle oturum açmasına olanak tanıyan bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 611a4b59b2ea2eee771f4060fb5d5af041b2ccc6
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577775"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="8b71e-103">Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile bir ASP.NET MVC 5 uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b71e-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="8b71e-104">Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="8b71e-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="8b71e-105">Bu öğreticide, kullanarak kullanıcıların oturum açma olanak sağlayan bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir [OAuth 2.0](http://oauth.net/2/) Facebook, Twitter, LinkedIn, Microsoft veya Google gibi bir dış kimlik doğrulama sağlayıcısı'ndan kimlik bilgilerine sahip.</span><span class="sxs-lookup"><span data-stu-id="8b71e-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="8b71e-106">Kolaylık olması için Bu öğreticide, Facebook ve Google kimlik bilgileriyle ile çalışma hakkındaki odaklanır.</span><span class="sxs-lookup"><span data-stu-id="8b71e-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="8b71e-107">Hesapları bu dış sağlayıcıları ile milyonlarca kullanıcıya zaten olduğundan, web sitelerinizi, bu kimlik bilgileri sağlayarak bu önemli bir avantaj sunar.</span><span class="sxs-lookup"><span data-stu-id="8b71e-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="8b71e-108">Bu kullanıcılar, yeni bir dizi kimlik bilgisi oluşturma ve olmadığı siteniz için kaydolmanız daha eilimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="8b71e-109">Ayrıca bkz: [SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="8b71e-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="8b71e-110">Öğreticinin ayrıca kullanıcıya profil verileri ekleme ve üyelik API'si rolleri eklemek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="8b71e-111">Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Lütfen bu bana Twitter'da takip edin: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="8b71e-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="8b71e-112">Başlarken</span><span class="sxs-lookup"><span data-stu-id="8b71e-112">Getting Started</span></span>

<span data-ttu-id="8b71e-113">Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="8b71e-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="8b71e-114">Visual Studio yükleme [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="8b71e-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="8b71e-115">Dropbox, GitHub, LinkedIn, Instagram, arabellek, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! ve daha fazla yardım için bkz. Bu [kodunuzla](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="8b71e-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="8b71e-116">Visual Studio yüklemelisiniz [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya Google OAuth 2'yi kullanın ve yerel olarak SSL uyarılar olmadan hata ayıklamak için daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="8b71e-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="8b71e-117">Tıklayın **yeni proje** gelen **Başlat** sayfasında veya menüyü kullanın ve seçin **dosya**, ardından **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="8b71e-118">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="8b71e-118">Creating Your First Application</span></span>

<span data-ttu-id="8b71e-119">Tıklayın **yeni proje**, ardından **Visual C#** solda, ardından **Web** seçip **ASP.NET Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="8b71e-120">"MvcAuth" projenizi adlandırın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="8b71e-121">İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklayın **MVC**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="8b71e-122">Kimlik doğrulaması değilse **bireysel kullanıcı hesapları**, tıklayın **kimlik doğrulamayı Değiştir** düğmesini tıklatın ve seçin **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="8b71e-123">Denetleyerek **bulutta Barındır**, uygulamayı Azure'da barındırmak çok kolay.</span><span class="sxs-lookup"><span data-stu-id="8b71e-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="8b71e-124">Seçtiyseniz **bulutta Barındır**, yapılandırma iletişim tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="8b71e-125">En son OWIN ara yazılımını güncelleştirmek için NuGet kullanma</span><span class="sxs-lookup"><span data-stu-id="8b71e-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="8b71e-126">Güncelleştirmek için NuGet paket yöneticisini kullanın [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="8b71e-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="8b71e-127">Seçin **güncelleştirmeleri** soldaki menüde.</span><span class="sxs-lookup"><span data-stu-id="8b71e-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="8b71e-128">Tıklayabilirsiniz **Tümünü Güncelleştir** düğmesini veya (sonraki görüntüde gösterilmiştir) OWIN paketleri arayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="8b71e-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="8b71e-129">Aşağıdaki görüntüde, yalnızca OWIN paketler gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="8b71e-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="8b71e-130">Paket Yöneticisi Konsolu (PMC'yi) öğesinden girdiğiniz `Update-Package` komutu tüm paketleri güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="8b71e-131">Tuşuna **F5** veya **Ctrl + F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="8b71e-132">Aşağıdaki görüntüde, bağlantı noktası numarası 1234 ise.</span><span class="sxs-lookup"><span data-stu-id="8b71e-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="8b71e-133">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="8b71e-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="8b71e-134">Tarayıcı pencerenizin boyutuna bağlı olarak, görmek için Gezinti simgesine tıklamanız gerekebilir **giriş**, **hakkında**, **kişi**, **kaydetme**ve **oturum** bağlantıları.</span><span class="sxs-lookup"><span data-stu-id="8b71e-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="8b71e-135">Projedeki SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="8b71e-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="8b71e-136">Google ve Facebook gibi kimlik doğrulama sağlayıcıları bağlanmak için IIS Express SSL kullanacak şekilde gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="8b71e-137">SSL oturum açtıktan sonra kullanmaya devam edebilirsiniz ve HTTP geri bırak değil önemlidir, oturum açma tanımlama bilgisinin parolası olarak ağ üzerinden düz metin olarak gönderiyorsanız SSL kullanarak olmadan ve kullanıcı adı ve parola olarak.</span><span class="sxs-lookup"><span data-stu-id="8b71e-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="8b71e-138">Yanında, zaten anlaşmasını gerçekleştirmek ve güvenli (HTTPS HTTP yavaş kılan toplu olan) kanal zaman zamandaki MVC ardışık düzeni çalıştırmadan önce bu nedenle oturum açtınız sonra geri HTTP yeniden yönlendirme geçerli istek veya gelecekteki yaratmayacaktır istekler daha hızlı.</span><span class="sxs-lookup"><span data-stu-id="8b71e-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="8b71e-139">İçinde **Çözüm Gezgini**, tıklayın **MvcAuth** proje.</span><span class="sxs-lookup"><span data-stu-id="8b71e-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="8b71e-140">Proje özelliklerini göstermek için F4 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="8b71e-141">Alternatif olarak, gelen **görünümü** seçebileceğiniz menü **Özellikler penceresi**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="8b71e-142">Değişiklik **SSL etkin** true.</span><span class="sxs-lookup"><span data-stu-id="8b71e-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="8b71e-143">SSL URL'sini kopyala (olacak `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).</span><span class="sxs-lookup"><span data-stu-id="8b71e-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="8b71e-144">İçinde **Çözüm Gezgini**, sağ tıklayın **MvcAuth** seçin ve proje **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="8b71e-145">Seçin **Web** sekmesine ve ardından SSL URL'sini yapıştırın **proje URL'si** kutusu.</span><span class="sxs-lookup"><span data-stu-id="8b71e-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="8b71e-146">' % S'dosyası (Ctl + S) kaydedin.</span><span class="sxs-lookup"><span data-stu-id="8b71e-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="8b71e-147">Facebook ve Google kimlik doğrulama uygulamaları yapılandırmak için bu URL gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="8b71e-148">Ekleme [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) özniteliğini `Home` denetleyicisi gerektiren tüm istekler için HTTPS kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="8b71e-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="8b71e-149">Daha güvenli bir yöntem eklemektir [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) uygulama için filtre.</span><span class="sxs-lookup"><span data-stu-id="8b71e-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="8b71e-150">Bölümüne bakın &quot;SSL ve yetkilendirmek özniteliği ile uygulama koruma&quot; my tutoral içinde [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="8b71e-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="8b71e-151">Giriş denetleyicisine değerinin bir bölümü aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="8b71e-152">Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="8b71e-153">Geçmişte sertifika yüklü değilse, bu bölümün geri kalanında atlayın ve atlama [OAuth 2 için Google uygulaması oluşturma ve uygulama projesiyle bağlantı kurulurken](#goog), aksi takdirde otomatik olarak imzalanan güven için yönergeleri izleyin IIS Express'in oluşturduğu sertifika.</span><span class="sxs-lookup"><span data-stu-id="8b71e-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="8b71e-154">Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden bir sertifika yüklemek istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="8b71e-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="8b71e-155">IE gösterir *giriş* sayfasında ve SSL uyarı yok.</span><span class="sxs-lookup"><span data-stu-id="8b71e-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="8b71e-156">Google Chrome, ayrıca sertifikayı kabul eder ve HTTPS içeriğine olmadan bir uyarı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="8b71e-157">Firefox, kendi sertifika deposu kullanır, bu nedenle, bir uyarı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="8b71e-158">Uygulamamız için güvenli bir şekilde tıklayabilirsiniz **riskleri anlıyorum**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="8b71e-159">OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="8b71e-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="8b71e-160">Geçerli Google OAuth yönergeler için bkz: [ASP.NET Core kimlik doğrulaması yapılandırma Google](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="8b71e-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="8b71e-161">Gidin [Google geliştiriciler konsol](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="8b71e-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="8b71e-162">Bir proje önce oluşturmadıysanız, seçin **kimlik bilgilerini** sol sekmeye tıklayın ve ardından içinde **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="8b71e-163">Sol sekmede tıklayın **kimlik bilgilerini**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="8b71e-164">Tıklayın **kimlik bilgilerini oluştur** ardından **OAuth istemcisi kimliği**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="8b71e-165">İçinde **istemci kodu oluşturma** iletişim kutusunda, varsayılan tutun **Web uygulaması** uygulama türü için.</span><span class="sxs-lookup"><span data-stu-id="8b71e-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="8b71e-166">Ayarlama **yetkili JavaScript** kaynakları yukarıda kullanılan SSL URL'sine (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece)</span><span class="sxs-lookup"><span data-stu-id="8b71e-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="8b71e-167">Ayarlama **yetkili yeniden yönlendirme URI'si** için:</span><span class="sxs-lookup"><span data-stu-id="8b71e-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="8b71e-168">OAuth onay ekranı menü öğesini tıklatın ve ardından e-posta adresi ve ürün adı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="8b71e-169">Form tıklayarak bitirdiğinizde **Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="8b71e-170">Kitaplık menü öğesini tıklayın, arama **Google + API**, buna tıklayın ardından etkinleştir tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="8b71e-171">Aşağıdaki resimde, etkin API'leri gösterir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="8b71e-172">Google API'leri API Yöneticisi'nden ziyaret **kimlik bilgilerini** almak için sekmesinde **istemci kimliği**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="8b71e-173">Uygulama gizli dizilerini bir JSON dosyası kaydetmek için indirin.</span><span class="sxs-lookup"><span data-stu-id="8b71e-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="8b71e-174">Kopyalama ve yapıştırma **ClientID** ve **ClientSecret** içine `UseGoogleAuthentication` yöntemi bulunan *Startup.Auth.cs* dosyası *App_Start* klasör.</span><span class="sxs-lookup"><span data-stu-id="8b71e-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="8b71e-175">**ClientID** ve **ClientSecret** aşağıda gösterilen değerleri örnek ve çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="8b71e-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="8b71e-176">Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki.</span><span class="sxs-lookup"><span data-stu-id="8b71e-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="8b71e-177">Örneği basit tutmak için yukarıdaki kod, kimlik bilgilerini ve hesabı eklenir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="8b71e-178">Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure App Service'e dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="8b71e-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="8b71e-179">Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="8b71e-180">Tıklayın **oturum** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8b71e-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="8b71e-181">Altında **başka bir hizmete oturum açmak için kullandığınız**, tıklayın **Google**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="8b71e-182">Yukarıdaki adımların kaçırmanıza bir HTTP 401 hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="8b71e-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="8b71e-183">Yukarıdaki adımları uygulamanıza yeniden denetleyin.</span><span class="sxs-lookup"><span data-stu-id="8b71e-183">Recheck your steps above.</span></span> <span data-ttu-id="8b71e-184">Gerekli bir ayar kaçırmanıza (örneğin **ürün adı**), eksik öğe ekleyip kaydedin; kimlik doğrulamasının çalışması için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="8b71e-185">Google kimlik bilgilerinizi gireceğiniz siteye yönlendirileceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8b71e-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="8b71e-186">Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulamasına izin vermek için istenir:</span><span class="sxs-lookup"><span data-stu-id="8b71e-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="8b71e-187">Tıklayın **kabul**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-187">Click **Accept**.</span></span> <span data-ttu-id="8b71e-188">Artık geri yönlendirilirsiniz **kaydetme** Burada, kaydedebilir Google hesabınızı MvcAuth uygulama sayfası.</span><span class="sxs-lookup"><span data-stu-id="8b71e-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="8b71e-189">Gmail hesabınızı için kullanılan yerel e-posta kayıt adı değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, bir kimlik doğrulaması için kullanılan) tutmak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="8b71e-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="8b71e-190">Tıklayın **kaydetme**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="8b71e-191">Facebook uygulama oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="8b71e-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="8b71e-192">Geçerli Facebook OAuth2 kimlik doğrulaması hakkında yönergeler için bkz. [yapılandırma Facebook kimlik doğrulaması](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="8b71e-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="8b71e-193">Üyelik verilerini İnceleme</span><span class="sxs-lookup"><span data-stu-id="8b71e-193">Examine the Membership Data</span></span>

<span data-ttu-id="8b71e-194">İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-194">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="8b71e-195">Genişletin **DefaultConnection (MvcAuth)**, genişletme **tabloları**, sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-195">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tablo verileri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="8b71e-197">Kullanıcı profil verileri ekleme</span><span class="sxs-lookup"><span data-stu-id="8b71e-197">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="8b71e-198">Bu bölümde, doğum tarihi ve başlangıç belediye kullanıcı verilerini kayıt sırasında aşağıdaki görüntüde gösterildiği gibi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="8b71e-198">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![Giriş Şehir ve Bday ile kayıt defteri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="8b71e-200">Açık *Models\IdentityModels.cs* dosya ve doğum tarihi ve giriş Şehir özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="8b71e-200">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="8b71e-201">Açık *Models\AccountViewModels.cs* dosyasının ve küme doğum tarihi ve giriş Şehir özelliklerinde `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="8b71e-201">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="8b71e-202">Açık *Controllers\AccountController.cs* dosya ve doğum tarihi ve giriş şehir için kod ekleyin `ExternalLoginConfirmation` gösterildiği gibi bir eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="8b71e-202">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="8b71e-203">Doğum tarihi ve giriş şehir için ekleme *Views\Account\ExternalLoginConfirmation.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8b71e-203">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="8b71e-204">Yeniden uygulamanızla Facebook hesabınızı kaydedin ve yeni bir doğum tarihi ve başlangıç belediye profil bilgilerini ekleyebilirsiniz doğrulayın üyelik veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="8b71e-204">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="8b71e-205">Gelen **Çözüm Gezgini**, tıklayın **tüm dosyaları göster** simgesine ve ardından sağ tıklama *Ekle\_Data\aspnet-MvcAuth -&lt;tarih damgası&gt;.mdf* tıklatıp **Sil**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-205">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="8b71e-206">Gelen **Araçları** menüsünde tıklayın **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu** (PMC).</span><span class="sxs-lookup"><span data-stu-id="8b71e-206">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="8b71e-207">PMC'de aşağıdaki komutları girin.</span><span class="sxs-lookup"><span data-stu-id="8b71e-207">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="8b71e-208">Geçişleri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="8b71e-208">Enable-Migrations</span></span>
2. <span data-ttu-id="8b71e-209">Geçiş başlatma</span><span class="sxs-lookup"><span data-stu-id="8b71e-209">Add-Migration Init</span></span>
3. <span data-ttu-id="8b71e-210">Veritabanını Güncelleştir</span><span class="sxs-lookup"><span data-stu-id="8b71e-210">Update-Database</span></span>

<span data-ttu-id="8b71e-211">Uygulamayı çalıştırmak ve FaceBook ve Google oturum açmak ve bazı kullanıcılar kaydetmek için kullanın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-211">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="8b71e-212">Üyelik verilerini İnceleme</span><span class="sxs-lookup"><span data-stu-id="8b71e-212">Examine the Membership Data</span></span>

<span data-ttu-id="8b71e-213">İçinde **görünümü** menüsünde tıklatın **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-213">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="8b71e-214">Sağ tıklayın **AspNetUsers** tıklatıp **tablo verilerini Göster**.</span><span class="sxs-lookup"><span data-stu-id="8b71e-214">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="8b71e-215">`HomeTown` Ve `BirthDate` alanları aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-215">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="8b71e-216">Uygulamanızı günlüğe kaydetme ve başka bir hesap ile günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="8b71e-216">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="8b71e-217">Uygulamanızı, Facebook ile oturum açın ve ardından Oturumu Kapat ve oturum açmayı deneyin yeniden (aynı tarayıcıyı kullanarak), farklı bir Facebook hesabıyla, hemen kullandığınız önceki Facebook hesabıyla oturum açmanız.</span><span class="sxs-lookup"><span data-stu-id="8b71e-217">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="8b71e-218">Başka bir hesap kullanmak için Facebook'a gidin ve Facebook oturumunuzu gerekir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-218">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="8b71e-219">Aynı kural, tüm 3 taraf kimlik sağlayıcıları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-219">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="8b71e-220">Alternatif olarak, farklı bir tarayıcı kullanarak başka bir hesapla oturum oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="8b71e-220">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8b71e-221">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="8b71e-221">Next Steps</span></span>

<span data-ttu-id="8b71e-222">Bkz: [Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları için OWIN giriş](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeleri tarafından.</span><span class="sxs-lookup"><span data-stu-id="8b71e-222">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="8b71e-223">Jerrie'nın bkz Yapıyorsak etkinleştir sosyal oturum açma düğmeleri almak ASP.NET MVC 5 sosyal oturum açma düğmeleri.</span><span class="sxs-lookup"><span data-stu-id="8b71e-223">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="8b71e-224">My öğreticiyi izleyin [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), bu öğreticiye devam eder ve şunları gösterir:</span><span class="sxs-lookup"><span data-stu-id="8b71e-224">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="8b71e-225">Uygulamanızı Azure'a dağıtmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="8b71e-225">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="8b71e-226">Uygulama rolleri ile güvenliğini sağlama.</span><span class="sxs-lookup"><span data-stu-id="8b71e-226">How to secure you app with roles.</span></span>
3. <span data-ttu-id="8b71e-227">Uygulamanız ile güvenli hale getirmeyi [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) ve [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtreler.</span><span class="sxs-lookup"><span data-stu-id="8b71e-227">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="8b71e-228">Kullanıcıları ve rolleri eklemek için üyelik API'si kullanma</span><span class="sxs-lookup"><span data-stu-id="8b71e-228">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="8b71e-229">Lütfen bu öğreticide sevmediğinizi nasıl ve ne geliştirebileceğimiz hakkında geri bildirim bırakın.</span><span class="sxs-lookup"><span data-stu-id="8b71e-229">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="8b71e-230">Yeni konuları da isteyebilirsiniz [Show Me nasıl ile kod](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="8b71e-230">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="8b71e-231">Hatta isteyin ve ASP.NET için eklenecek yeni özellikleri oy verin.</span><span class="sxs-lookup"><span data-stu-id="8b71e-231">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="8b71e-232">Örneğin, bir aracı için oy verebilirsiniz [kullanıcıları ve rolleri oluşturun ve yönetin.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="8b71e-232">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="8b71e-233">ASP.NET Dış kimlik doğrulama hizmetleri nasıl çalışır bir iyi açıklama için bkz: Robert McMurray'nın [Dış kimlik doğrulama hizmetleri](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="8b71e-233">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="8b71e-234">Robert'ın makale aynı zamanda Microsoft ve Twitter kimlik doğrulamasının etkinleştirilmesinde ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="8b71e-234">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="8b71e-235">Tom Dykstra mükemmel [EF/MVC Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Entity Framework ile çalışma hakkında bilgi verilmektedir.</span><span class="sxs-lookup"><span data-stu-id="8b71e-235">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
