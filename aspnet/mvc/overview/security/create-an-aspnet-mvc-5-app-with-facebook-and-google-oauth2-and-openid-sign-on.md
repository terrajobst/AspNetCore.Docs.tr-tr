---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: MVC 5 oluşturma Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile uygulama | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici, OAuth 2.0 dış authenti kimlik bilgilerini kullanarak oturum açmalarını sağlar bir ASP.NET MVC 5 web uygulamalarının nasıl oluşturulacağını gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: c289c209b50f0c2c1f2d8b15a3aedeaebf671d0b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="7dc34-103">Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma (C#) ile bir ASP.NET MVC 5 uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="7dc34-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="7dc34-104">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="7dc34-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="7dc34-105">Bu öğretici kullanıcıların oturum açmasına imkan sağlayan bir ASP.NET MVC 5 web uygulamasını kullanarak nasıl oluşturulacağını gösterir [OAuth 2.0](http://oauth.net/2/) Facebook, Twitter, LinkedIn, Microsoft veya Google gibi bir dış kimlik doğrulama sağlayıcısından kimlik bilgilerine sahip.</span><span class="sxs-lookup"><span data-stu-id="7dc34-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="7dc34-106">Kolaylık olması için Bu öğretici, Facebook ve Google kimlik bilgilerini ile çalışma hakkında odaklanır.</span><span class="sxs-lookup"><span data-stu-id="7dc34-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="7dc34-107">Milyonlarca kullanıcıya bu dış sağlayıcıları hesaplarıyla olduğundan bu kimlik bilgileri, web sitelerindeki etkinleştirilmesi önemli bir avantajı sağlar.</span><span class="sxs-lookup"><span data-stu-id="7dc34-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="7dc34-108">Bu kullanıcılar oluşturun ve yeni bir kimlik bilgileri kümesi unutmayın gerek yoktur, siteniz için kaydolmanız daha eilimli olabilir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="7dc34-109">Ayrıca bkz. [SMS ve e-posta iki öğeli kimlik doğrulama ile ASP.NET MVC 5 uygulaması](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="7dc34-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="7dc34-110">Öğreticinin Ayrıca kullanıcı için profil verileri ekleme ve üyelik API'si rolleri eklemek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="7dc34-111">Bu öğretici tarafından yazıldı [Rick Anderson](https://blogs.msdn.com/rickAndy) (Lütfen bu bana Twitter'da takip edin: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="7dc34-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="7dc34-112">Başlarken</span><span class="sxs-lookup"><span data-stu-id="7dc34-112">Getting Started</span></span>

<span data-ttu-id="7dc34-113">Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="7dc34-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="7dc34-114">Visual Studio yükleme [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="7dc34-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="7dc34-115">Dropbox, GitHub, LinkedIn, Instagram, arabellek, Salesforce, akış, yığın Exchange, Tripit, Twitch, Twitter, Yahoo! ve daha fazla yardım için bkz [örnek proje](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="7dc34-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="7dc34-116">Visual Studio yüklemelisiniz [2013 güncelleştirme 3](https://go.microsoft.com/fwlink/?LinkId=390521) veya Google OAuth 2'yi kullanın ve yerel olarak SSL uyarılar olmadan hata ayıklamak için daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="7dc34-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="7dc34-117">Tıklatın **yeni proje** gelen **Başlat** sayfa veya menüsünü kullanın ve seçin **dosya**ve ardından **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="7dc34-118">İlk uygulamanızı oluşturma</span><span class="sxs-lookup"><span data-stu-id="7dc34-118">Creating Your First Application</span></span>

<span data-ttu-id="7dc34-119">Tıklatın **yeni proje**seçeneğini belirleyip **Visual C#** solda, ardından **Web** ve ardından **ASP.NET Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="7dc34-120">Projeniz "MvcAuth" olarak adlandırın ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="7dc34-121">İçinde **yeni ASP.NET projesi** iletişim kutusunda, tıklatın **MVC**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="7dc34-122">Kimlik doğrulaması değilse **tek tek kullanıcı hesaplarını**, tıklatın **kimlik doğrulamayı Değiştir** düğmesine tıklayın ve ardından **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="7dc34-123">Denetleyerek **bulutta Barındır**, uygulama Azure'da barındırmak çok kolay olacaktır.</span><span class="sxs-lookup"><span data-stu-id="7dc34-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="7dc34-124">Seçtiyseniz **bulutta Barındır**, Yapılandır iletişim tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="7dc34-125">En son OWIN ara yazılımı için güncelleştirmek için NuGet kullanma</span><span class="sxs-lookup"><span data-stu-id="7dc34-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="7dc34-126">Güncelleştirmek için NuGet paket yöneticisini kullanın [OWIN ara yazılımı](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="7dc34-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="7dc34-127">Seçin **güncelleştirmeleri** soldaki menüde.</span><span class="sxs-lookup"><span data-stu-id="7dc34-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="7dc34-128">Tıklatabilirsiniz **Tümünü Güncelleştir** düğmesini veya yalnızca (sonraki görüntüde gösterilen) OWIN paketler arayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="7dc34-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="7dc34-129">Aşağıdaki resimde, yalnızca OWIN paketler gösterilir:</span><span class="sxs-lookup"><span data-stu-id="7dc34-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="7dc34-130">Paket Yöneticisi Konsolu (PMC) gelen girdiğiniz `Update-Package` tüm paketleri güncelleştirir komutu.</span><span class="sxs-lookup"><span data-stu-id="7dc34-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="7dc34-131">Tuşuna **F5** veya **Ctrl + F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="7dc34-132">Aşağıdaki resimde 1234 bağlantı noktası numarasıdır.</span><span class="sxs-lookup"><span data-stu-id="7dc34-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="7dc34-133">Uygulamayı çalıştırdığınızda, farklı bir bağlantı noktası görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="7dc34-134">Tarayıcı pencerenizin boyutuna bağlı olarak görmek için Gezinti simgesini gerekebilir **giriş**, **hakkında**, **kişi**, **kaydetmek**ve **oturum** bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="7dc34-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="7dc34-135">Projedeki SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="7dc34-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="7dc34-136">Google ve Facebook gibi kimlik doğrulama sağlayıcıları bağlanmak için SSL kullanmak üzere IIS Express'i ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="7dc34-137">Oturum açtıktan sonra SSL kullanmaya devam et ve HTTP geri bırakma değil de önemlidir, oturum açma tanımlama bilgisinin yalnızca gizlilik olarak kullanıcı adı ve parola olarak ve ağ üzerinden düz metin olarak gönderiyoruz SSL kullanmadan.</span><span class="sxs-lookup"><span data-stu-id="7dc34-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="7dc34-138">Anlaşma gerçekleştirmek için güvenli (HTTPS HTTP yavaş kılan toplu olan) kanal zaman yanında, zaten ayırdıktan MVC ardışık düzeni çalıştırmadan önce böylece oturum açtınız sonra geri HTTP yeniden yönlendirme geçerli istek veya gelecekteki yapmaz çok daha hızlı istek sayısı.</span><span class="sxs-lookup"><span data-stu-id="7dc34-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="7dc34-139">İçinde **Çözüm Gezgini**, tıklatın **MvcAuth** projesi.</span><span class="sxs-lookup"><span data-stu-id="7dc34-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="7dc34-140">Proje özellikleri göstermek için F4 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="7dc34-141">Alternatif olarak, gelen **Görünüm** seçebileceğiniz menü **Özellikler penceresini**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="7dc34-142">Değişiklik **SSL özellikli** true.</span><span class="sxs-lookup"><span data-stu-id="7dc34-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="7dc34-143">SSL URL'sini kopyala (olacak `https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece).</span><span class="sxs-lookup"><span data-stu-id="7dc34-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="7dc34-144">İçinde **Çözüm Gezgini**, sağ tıklatın **MvcAuth** proje ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="7dc34-145">Seçin **Web** sekmesini ve ardından içine SSL URL'sini yapıştırın **proje URL'sini** kutusu.</span><span class="sxs-lookup"><span data-stu-id="7dc34-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="7dc34-146">(Ctl + S) dosyasını kaydedin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="7dc34-147">Facebook ve Google kimlik doğrulama uygulamaları yapılandırmak için bu URL gerekir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="7dc34-148">Ekleme [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) özniteliğini `Home` tüm istekleri gerektirecek şekilde denetleyicisi HTTPS kullanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="7dc34-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="7dc34-149">Daha güvenli bir yöntem eklemektir [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtre uygulama.</span><span class="sxs-lookup"><span data-stu-id="7dc34-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="7dc34-150">Bölümüne bakın &quot;SSL ve yetkilendirmek özniteliği uygulamasıyla koruma&quot; my tutoral içinde [auth ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="7dc34-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="7dc34-151">Giriş denetleyicisi bir bölümü aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="7dc34-152">Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="7dc34-153">Geçmişte sertifika yüklediyseniz, bu bölümde geri kalanını atlayın ve atlamak [için OAuth 2 bir Google uygulaması oluşturma ve uygulama projesine bağlanma](#goog), aksi takdirde otomatik olarak imzalanan güven için yönergeleri izleyin IIS Express üretti sertifikası.</span><span class="sxs-lookup"><span data-stu-id="7dc34-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="7dc34-154">Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden sertifika yüklemek istiyorsanız.</span><span class="sxs-lookup"><span data-stu-id="7dc34-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="7dc34-155">IE gösterir *giriş* sayfasında ve SSL uyarı yok.</span><span class="sxs-lookup"><span data-stu-id="7dc34-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="7dc34-156">Google Chrome Ayrıca sertifikayı kabul eder ve HTTPS içerik olmadan bir uyarı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="7dc34-157">Firefox, kendi sertifika deposuna kullanır, bu nedenle bir uyarı görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="7dc34-158">Uygulamamız için güvenli bir şekilde tıklayabilirsiniz **riskleri anlamak**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="7dc34-159">OAuth 2 için bir Google uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="7dc34-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="7dc34-160">Geçerli Google OAuth yönergeler için bkz: [ASP.NET Core yapılandırma Google kimlik doğrulaması](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="7dc34-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="7dc34-161">Gidin [Google geliştiriciler konsol](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="7dc34-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="7dc34-162">Önce bir projeyi oluşturmadıysanız, seçin **kimlik bilgileri** sol sekmesini ve ardından **oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="7dc34-163">Sol sekmede tıklatın **kimlik bilgileri**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="7dc34-164">Tıklatın **kimlik bilgileri oluşturma** sonra **OAuth istemci kimliği**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="7dc34-165">İçinde **istemci kimliği oluşturma** iletişim kutusunda, varsayılan tutmak **Web uygulaması** uygulama türü için.</span><span class="sxs-lookup"><span data-stu-id="7dc34-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="7dc34-166">Ayarlama **yetkili JavaScript** yukarıda kullanılan SSL URL'ye kaynakları (`https://localhost:44300/` diğer SSL projeleri oluşturmuş olduğunuz sürece)</span><span class="sxs-lookup"><span data-stu-id="7dc34-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="7dc34-167">Ayarlama **yetkili yeniden yönlendirme URI'si** için:</span><span class="sxs-lookup"><span data-stu-id="7dc34-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="7dc34-168">OAuth izni ekran menü öğesini tıklatın, ardından e-posta adresi ve ürün adı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="7dc34-169">Ne zaman tamamladığınızdan form tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="7dc34-170">Kitaplık menü öğesini tıklatın, arama **Google + API**, üzerinde tıklatın ardından Etkinleştir'e basın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="7dc34-171">Aşağıdaki görüntü etkin API'leri gösterir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="7dc34-172">Google API'leri API Yöneticisi'nden ziyaret **kimlik bilgileri** elde etmek için sekme **istemci kimliği**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="7dc34-173">Uygulama parolaları ile JSON dosyasının kaydedileceği indirin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="7dc34-174">Kopyalama ve yapıştırma **ClientID** ve **ClientSecret** içine `UseGoogleAuthentication` yöntemi bulunan *Startup.Auth.cs* dosyasını *App_Start* klasör.</span><span class="sxs-lookup"><span data-stu-id="7dc34-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="7dc34-175">**ClientID** ve **ClientSecret** aşağıda gösterilen değerleri örnekleri ve çalışmıyor.</span><span class="sxs-lookup"><span data-stu-id="7dc34-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="7dc34-176">Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="7dc34-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="7dc34-177">Hesabı ve kimlik bilgileri örneği basit tutmak için yukarıdaki kod eklenir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="7dc34-178">Bkz: [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure uygulama hizmeti dağıtmak için](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="7dc34-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="7dc34-179">Tuşuna **CTRL + F5** oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="7dc34-180">Tıklatın **oturum** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="7dc34-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="7dc34-181">Altında **oturum açmak için başka bir hizmet kullanın**, tıklatın **Google**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="7dc34-182">Yukarıdaki adımların kaçırmanıza bir HTTP 401 hatası alırsınız.</span><span class="sxs-lookup"><span data-stu-id="7dc34-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="7dc34-183">Yukarıdaki adımları uygulamanıza yeniden denetleyin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-183">Recheck your steps above.</span></span> <span data-ttu-id="7dc34-184">Gerekli bir ayar kaçırılması durumunda (örneğin **ürün adı**), eksik öğe ekleyin ve kaydedin; kimlik doğrulamasının çalışması için birkaç dakika sürebilir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="7dc34-185">Kimlik bilgilerinizi gireceğiniz Google siteye yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="7dc34-186">Kimlik bilgilerinizi girdikten sonra yeni oluşturduğunuz web uygulaması izinleri vermeniz istenir:</span><span class="sxs-lookup"><span data-stu-id="7dc34-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="7dc34-187">Tıklatın **kabul**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-187">Click **Accept**.</span></span> <span data-ttu-id="7dc34-188">Artık yeniden yönlendirileceği konum **kaydetmek** Burada, kaydolabilir Google hesabınız MvcAuth uygulama sayfası.</span><span class="sxs-lookup"><span data-stu-id="7dc34-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="7dc34-189">Gmail hesabınız için kullanılan yerel e-posta kayıt adını değiştirme seçeneğiniz vardır, ancak genellikle varsayılan e-posta diğer adı (diğer bir deyişle, kimlik doğrulaması için kullanılan bir) tutmak istiyor.</span><span class="sxs-lookup"><span data-stu-id="7dc34-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="7dc34-190">Tıklatın **kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="7dc34-191">İçinde Facebook uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="7dc34-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="7dc34-192">Geçerli Facebook OAuth2 kimlik doğrulama yönergeler için bkz: [yapılandırma Facebook kimlik doğrulaması](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="7dc34-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="7dc34-193">Facebook OAuth2 kimlik doğrulaması için Facebook içinde oluşturduğunuz bir uygulamadan bazı ayarları projenize kopyalamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="7dc34-194">Tarayıcınızda gidin [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) ve oturum açma Facebook kimlik bilgilerinizi girin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="7dc34-195">Facebook geliştirici olarak zaten kayıtlı değil, tıklatın **geliştiricisi olarak kaydolma** ve kaydetmek için yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="7dc34-196">Üzerinde **uygulamaları** sekmesini tıklatın, **yeni uygulama oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Yeni uygulama oluşturma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="7dc34-198">Girin bir **App Name** ve **kategori**, ardından **oluşturma uygulama**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="7dc34-199">Bu Facebook arasında benzersiz olması gerekir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-199">This must be unique across Facebook.</span></span> <span data-ttu-id="7dc34-200"><strong>Uygulama Namespace</strong> uygulamanıza kimlik doğrulaması için Facebook uygulama erişmek için kullanacağınız URL parçası olan (örneğin, https://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="7dc34-200">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="7dc34-201">Belirtmediyseniz bir <strong>uygulama Namespace</strong>, <strong>uygulama kimliği</strong> URL için kullanılacaktır.</span><span class="sxs-lookup"><span data-stu-id="7dc34-201">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="7dc34-202"><strong>Uygulama kimliği</strong> sonraki adımda görürsünüz uzun sistem tarafından oluşturulan bir sayı.</span><span class="sxs-lookup"><span data-stu-id="7dc34-202">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Yeni uygulama iletişim oluşturma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="7dc34-204">Standart güvenlik denetimi gönderin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-204">Submit the standard security check.</span></span>

    ![Güvenlik denetimi](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="7dc34-206">Seçin **ayarları** sol menü çubuğu'nu için![ Facebook geliştiricinin menü çubuğu](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="7dc34-206">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="7dc34-207">Üzerinde **temel** ayarları bölümünde seçin **eklemek Platform** bir Web uygulaması ekleme belirtmek için.</span><span class="sxs-lookup"><span data-stu-id="7dc34-207">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="7dc34-208">![Temel ayarlar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="7dc34-208">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="7dc34-209">Seçin **Web sitesi** platformu seçeneklerden.</span><span class="sxs-lookup"><span data-stu-id="7dc34-209">Select **Website** from the platform choices.</span></span>  
  
    ![Platform seçenekleri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="7dc34-211">Not, **uygulama kimliği** ve **uygulama gizli anahtarı** böylece daha sonra Bu öğreticide MVC uygulamanıza her ikisi de ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-211">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="7dc34-212">Ayrıca, Site URL'si ekleyin (`https://localhost:44300/`) MVC uygulamanızı test etmek için.</span><span class="sxs-lookup"><span data-stu-id="7dc34-212">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="7dc34-213">Ayrıca, bir **ilgili kişi e-posta**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-213">Also, add a **Contact Email**.</span></span> <span data-ttu-id="7dc34-214">Ardından, seçin **Değişiklikleri Kaydet**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-214">Then, select **Save Changes**.</span></span>   

    ![Temel Uygulama Ayrıntıları sayfası](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="7dc34-216">Yalnızca kaydettiğiniz e-posta diğer adını kullanarak kimlik doğrulaması için olacağını unutmayın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-216">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="7dc34-217">Diğer kullanıcılar ve test hesapları kaydetmek mümkün olmaz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-217">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="7dc34-218">Uygulama FaceBook'ta diğer Facebook hesaplarına erişim izni verebilir **Geliştirici rolleri** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="7dc34-218">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="7dc34-219">Visual Studio'da açın *uygulama\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="7dc34-219">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="7dc34-220">Kopyalama ve yapıştırma **AppID** ve **uygulama gizli anahtarı** içine `UseFacebookAuthentication` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="7dc34-220">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="7dc34-221">**AppID** ve **uygulama gizli anahtarı** aşağıda gösterilen değerleri örnekleri ve çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-221">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="7dc34-222">Tıklatın **değişiklikleri kaydetmek**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-222">Click **Save Changes**.</span></span>
13. <span data-ttu-id="7dc34-223">Tuşuna **CTRL + F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-223">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="7dc34-224">Seçin **oturum** oturum açma sayfasını görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-224">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="7dc34-225">Tıklatın **Facebook** altında **oturum açmak için başka bir hizmet kullanın.**</span><span class="sxs-lookup"><span data-stu-id="7dc34-225">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="7dc34-226">Facebook kimlik bilgilerinizi girin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-226">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="7dc34-227">Genel profil ve arkadaş listesi erişmek uygulama izni istenir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-227">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook uygulama ayrıntıları](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="7dc34-229">Artık oturum açtınız.</span><span class="sxs-lookup"><span data-stu-id="7dc34-229">You are now logged in.</span></span> <span data-ttu-id="7dc34-230">Bu gibi durumlarda, bu hesap artık uygulama ile kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-230">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="7dc34-231">Kaydolduğunuzda, bir giriş eklenen *kullanıcılar* üyelik veritabanının tablo.</span><span class="sxs-lookup"><span data-stu-id="7dc34-231">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="7dc34-232">Üyelik verilerinin inceleyin</span><span class="sxs-lookup"><span data-stu-id="7dc34-232">Examine the Membership Data</span></span>

<span data-ttu-id="7dc34-233">İçinde **Görünüm** menüsünde tıklatın **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-233">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="7dc34-234">Genişletme **DefaultConnection (MvcAuth)**, genişletin **tabloları**, sağ tıklatın **AspNetUsers** tıklatıp **tablo verileri Göster**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-234">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers tablo verileri](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="7dc34-236">Profil verileri kullanıcı sınıfına ekleme</span><span class="sxs-lookup"><span data-stu-id="7dc34-236">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="7dc34-237">Bu bölümde, doğum tarihi ve ev Şehir kullanıcı verilerini kayıt sırasında aşağıdaki görüntüde gösterildiği gibi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-237">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg ev Şehir ve Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="7dc34-239">Açık *Models\IdentityModels.cs* dosya ve doğum tarihi ve ev Şehir özellikleri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="7dc34-239">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="7dc34-240">Açık *Models\AccountViewModels.cs* dosya ve kümesi doğum tarihi ve ev Şehir özelliklerinde `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="7dc34-240">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="7dc34-241">Açık *Controllers\AccountController.cs* dosya ve doğum tarihi ve giriş piyasada için kodu ekleyin `ExternalLoginConfirmation` gösterildiği gibi eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="7dc34-241">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="7dc34-242">Doğum tarihi ve ev Şehir eklemek *Views\Account\ExternalLoginConfirmation.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="7dc34-242">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="7dc34-243">Yeniden Facebook hesabınız ile uygulamanızı kaydetme ve ev Şehir profil bilgileri ve yeni doğum tarihi ekleyebilirsiniz doğrulamak için üyelik veritabanını silin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-243">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="7dc34-244">Gelen **Çözüm Gezgini**, tıklatın **tüm dosyaları göster** simgesine ve ardından sağ tıklatma *Ekle\_Data\aspnet-MvcAuth -&lt;tarih damgası&gt;.mdf* tıklatıp **silmek**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-244">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="7dc34-245">Gelen **Araçları** menüsünde tıklatın **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu** (PMC).</span><span class="sxs-lookup"><span data-stu-id="7dc34-245">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="7dc34-246">Aşağıdaki komutları PMC girin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-246">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="7dc34-247">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="7dc34-247">Enable-Migrations</span></span>
2. <span data-ttu-id="7dc34-248">Add-Migration başlatma</span><span class="sxs-lookup"><span data-stu-id="7dc34-248">Add-Migration Init</span></span>
3. <span data-ttu-id="7dc34-249">Update-Database</span><span class="sxs-lookup"><span data-stu-id="7dc34-249">Update-Database</span></span>

<span data-ttu-id="7dc34-250">Uygulamayı çalıştırın ve FaceBook ve Google oturum açın ve bazı kullanıcılar kaydetmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-250">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="7dc34-251">Üyelik verilerinin inceleyin</span><span class="sxs-lookup"><span data-stu-id="7dc34-251">Examine the Membership Data</span></span>

<span data-ttu-id="7dc34-252">İçinde **Görünüm** menüsünde tıklatın **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-252">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="7dc34-253">Sağ tıklayın **AspNetUsers** tıklatıp **Show Table Data**.</span><span class="sxs-lookup"><span data-stu-id="7dc34-253">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="7dc34-254">`HomeTown` Ve `BirthDate` alanları aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-254">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="7dc34-255">Uygulamanızı günlüğe kaydetme ve içinde başka bir hesap ile oturum</span><span class="sxs-lookup"><span data-stu-id="7dc34-255">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="7dc34-256">Uygulamanızı,, Facebook ile oturum açın ve sonra oturumu kapatın ve oturum açmayı deneyin yeniden (aynı tarayıcıyı kullanarak), farklı bir Facebook hesabıyla, hemen kullandığınız önceki Facebook hesabıyla oturum açmanız.</span><span class="sxs-lookup"><span data-stu-id="7dc34-256">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="7dc34-257">Başka bir hesap kullanmak için Facebook hesabına gidin ve Facebook sırasında oturumunuzu gerekir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-257">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="7dc34-258">Aynı kural herhangi diğer 3 taraf kimlik doğrulama sağlayıcısı için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-258">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="7dc34-259">Alternatif olarak, farklı bir tarayıcı kullanarak başka bir hesapla oturum oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="7dc34-259">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7dc34-260">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="7dc34-260">Next Steps</span></span>

<span data-ttu-id="7dc34-261">Bkz: [OWIN için Yahoo ve LinkedIn OAuth güvenlik sağlayıcıları Tanıtımı](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) Jerrie Pelser Yahoo ve LinkedIn yönergeler tarafından.</span><span class="sxs-lookup"><span data-stu-id="7dc34-261">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="7dc34-262">Jerrie'nın bkz sosyal oturum açmayı düğmeleri etkinleştir sosyal oturum açmayı düğmeleri almak ASP.NET MVC 5 için asıl.</span><span class="sxs-lookup"><span data-stu-id="7dc34-262">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="7dc34-263">My öğreticisini izleyin [auth ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), Bu öğretici devam eder ve aşağıdakileri gösterir:</span><span class="sxs-lookup"><span data-stu-id="7dc34-263">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="7dc34-264">Uygulamanızı Azure'a dağıtmak nasıl.</span><span class="sxs-lookup"><span data-stu-id="7dc34-264">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="7dc34-265">Uygulama rolleri ile güvenli hale getirmek nasıl.</span><span class="sxs-lookup"><span data-stu-id="7dc34-265">How to secure you app with roles.</span></span>
3. <span data-ttu-id="7dc34-266">Uygulamanız ile güvenli hale getirmek nasıl [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) ve [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtreler.</span><span class="sxs-lookup"><span data-stu-id="7dc34-266">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="7dc34-267">Kullanıcıları ve rolleri eklemek için üyelik API'si kullanma</span><span class="sxs-lookup"><span data-stu-id="7dc34-267">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="7dc34-268">Lütfen geri bildirim, Bu öğretici beğendiğinizi nasıl ve ne biz artabileceğini bırakın.</span><span class="sxs-lookup"><span data-stu-id="7dc34-268">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="7dc34-269">En yeni konular da isteğinde bulunabilirsiniz [Göster bana nasıl kodu ile](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="7dc34-269">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="7dc34-270">Hatta, sorun ve ASP.NET ile eklenecek yeni özellikler oy verin.</span><span class="sxs-lookup"><span data-stu-id="7dc34-270">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="7dc34-271">Örneğin, bir aracı için oy kullanabilir [kullanıcıları ve rolleri oluşturun ve yönetin.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="7dc34-271">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="7dc34-272">Robert McMurray'nın nasıl ASP.NET Dış kimlik doğrulama hizmetleri işe iyi açıklama için bkz: [Dış kimlik doğrulama hizmetleri](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="7dc34-272">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="7dc34-273">Robert'ın makale aynı zamanda Microsoft ve Twitter kimlik doğrulamasını etkinleştirme içindeki ayrıntısı girmeyeceğini.</span><span class="sxs-lookup"><span data-stu-id="7dc34-273">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="7dc34-274">Zel Dykstra mükemmel [EF/MVC Öğreticisi](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) Entity Framework ile çalışmaya nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="7dc34-274">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
