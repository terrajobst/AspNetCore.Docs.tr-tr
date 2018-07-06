---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: E-posta onayı ve parola sıfırlama (C#) oturum açma, güvenli bir ASP.NET MVC 5 web uygulaması oluşturma | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide e-posta onayı ve ASP.NET Identity üyelik sistemini kullanarak parola sıfırlama ile bir ASP.NET MVC 5 web uygulaması oluşturma gösterilmektedir. Ca...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: 1387c3e9c03e011b610a070aa0c273ded23b463e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823553"
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="90d0a-104">E-posta onayı ve parola sıfırlama (C#) oturum açma, güvenli bir ASP.NET MVC 5 web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="90d0a-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="90d0a-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="90d0a-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="90d0a-106">Bu öğreticide e-posta onayı ve ASP.NET Identity üyelik sistemini kullanarak parola sıfırlama ile bir ASP.NET MVC 5 web uygulaması oluşturma gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="90d0a-107">Tamamlanmış uygulamayı karşıdan yükleyebileceğiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="90d0a-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="90d0a-108">İndirilen bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS olanak tanıyan hata ayıklama Yardımcıları bulunur.</span><span class="sxs-lookup"><span data-stu-id="90d0a-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="90d0a-109">Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="90d0a-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="90d0a-110">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="90d0a-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="90d0a-111">Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="90d0a-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="90d0a-112">Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="90d0a-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="90d0a-113">Uyarı: Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="90d0a-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="90d0a-114">Yeni ASP.NET Web projesi oluşturun ve MVC şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="90d0a-115">Web Forms da destekler ASP.NET Identity şekilde bir web forms uygulaması benzer adımları izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90d0a-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="90d0a-116">Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="90d0a-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="90d0a-117">Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunu işaretli bırakın.</span><span class="sxs-lookup"><span data-stu-id="90d0a-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="90d0a-118">Öğreticide daha sonra Azure'da dağıtacağız.</span><span class="sxs-lookup"><span data-stu-id="90d0a-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="90d0a-119">Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="90d0a-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="90d0a-120">Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="90d0a-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="90d0a-121">Uygulamayı çalıştırın, tıklayın **kaydetme** bağlamak ve bir kullanıcı kaydı.</span><span class="sxs-lookup"><span data-stu-id="90d0a-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="90d0a-122">Yalnızca e-posta doğrulamasını bu noktada, olan [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="90d0a-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="90d0a-123">Sunucu Gezgini'nde gidin **veri Connections\DefaultConnection\Tables\AspNetUsers**sağ tıklayın ve seçin **tablo tanımını açın**.</span><span class="sxs-lookup"><span data-stu-id="90d0a-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="90d0a-124">Aşağıdaki görüntüde `AspNetUsers` şema:</span><span class="sxs-lookup"><span data-stu-id="90d0a-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="90d0a-125">Sağ tıklayın **AspNetUsers** tablosunu seçip **tablo verilerini Göster**.</span><span class="sxs-lookup"><span data-stu-id="90d0a-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="90d0a-126">Bu noktada e-posta onaylanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="90d0a-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="90d0a-127">Satırına tıklayın ve Sil'i seçin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-127">Click on the row and select delete.</span></span> <span data-ttu-id="90d0a-128">Bu e-postayı yeniden sonraki adımda ekleyecek ve bir onay e-posta gönderin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="90d0a-129">E-posta onayı</span><span class="sxs-lookup"><span data-stu-id="90d0a-129">Email confirmation</span></span>

<span data-ttu-id="90d0a-130">Bunlar değil kimliğe bürünerek başka birisi doğrulamak için yeni bir kullanıcı kaydı e-postayı onaylamak için iyi bir uygulamadır (diğer bir deyişle, bunlar başka birinin e-posta ile kayıtlı olmayabilirsiniz).</span><span class="sxs-lookup"><span data-stu-id="90d0a-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="90d0a-131">Tartışma Forumu tablonuz olduğunu varsayın engellemek istiyorsunuz `"bob@example.com"` olarak kaydetme gelen `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="90d0a-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="90d0a-132">E-posta onayı olmadan `"joe@contoso.com"` uygulamanızdan istenmeyen e-posta alabilir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="90d0a-133">Bob yanlışlıkla kayıtlı varsayalım `"bib@example.com"` ve bunu fark yüklediniz o uygulamayı doğru e-postasını olmadığı için parola kurtarma kullanın saptayamazdınız.</span><span class="sxs-lookup"><span data-stu-id="90d0a-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="90d0a-134">E-posta onayı robotlar yalnızca sınırlı koruma sağlar ve belirlenen istenmeyen posta gönderenlere koruma sağlamaz, sahip oldukları çok sayıda çalışan e-posta diğer adlar kaydetmek için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90d0a-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="90d0a-135">Genellikle, yeni kullanıcıların e-posta, SMS mesajı ya da başka bir mekanizma onaylanmıştır önce web sitenizi herhangi bir veri gönderme engellemek istiyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="90d0a-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="90d0a-136">Aşağıdaki bölümler size e-posta onayı etkinleştirin ve yeni kaydettiğiniz kullanıcıların e-postasına onaylanana kadar oturum açmayı engellemek için kodu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="90d0a-137">SendGrid bağlama</span><span class="sxs-lookup"><span data-stu-id="90d0a-137">Hook up SendGrid</span></span>

<span data-ttu-id="90d0a-138">Bu öğreticide yalnızca e-posta bildirimi aracılığıyla ekleme gösterir, ancak [SendGrid](http://sendgrid.com/), e-posta SMTP veya başka mekanizmalar kullanılarak gönderebilirsiniz (bkz [ek kaynaklar](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="90d0a-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="90d0a-139">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="90d0a-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="90d0a-140">Git [Azure SendGrid kayıt sayfasını](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) ve ücretsiz bir SendGrid hesabı için kaydolun.</span><span class="sxs-lookup"><span data-stu-id="90d0a-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="90d0a-141">SendGrid, aşağıdakine benzer bir kod ekleyerek yapılandırma *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="90d0a-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="90d0a-142">Aşağıdakileri içeren eklemeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="90d0a-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="90d0a-143">Bu örneği basit tutmak için biz uygulama ayarlarında depolayacağınızı *web.config* dosyası:</span><span class="sxs-lookup"><span data-stu-id="90d0a-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="90d0a-144">Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki.</span><span class="sxs-lookup"><span data-stu-id="90d0a-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="90d0a-145">Kimlik ve hesap appSetting içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="90d0a-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="90d0a-146">Azure üzerinde güvenli bir şekilde bu değerleri üzerinde depolamanın **[yapılandırma](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** Azure portalında sekmesi.</span><span class="sxs-lookup"><span data-stu-id="90d0a-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="90d0a-147">Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="90d0a-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="90d0a-148">E-posta onayı hesabı denetleyicisindeki etkinleştir</span><span class="sxs-lookup"><span data-stu-id="90d0a-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="90d0a-149">Doğrulama *Views\Account\ConfirmEmail.cshtml* dosyasının doğru razor sözdizimi vardır.</span><span class="sxs-lookup"><span data-stu-id="90d0a-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="90d0a-150">(@ Karakteri ilk satırı eksik olabilir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="90d0a-151">)</span><span class="sxs-lookup"><span data-stu-id="90d0a-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="90d0a-152">Uygulamayı çalıştırın ve Kaydet bağlantısına tıklayın.</span><span class="sxs-lookup"><span data-stu-id="90d0a-152">Run the app and click the Register link.</span></span> <span data-ttu-id="90d0a-153">Kayıt formunu gönderildikten sonra oturum açtınız.</span><span class="sxs-lookup"><span data-stu-id="90d0a-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="90d0a-154">E-posta hesabınızı denetleyin ve e-postanızı doğrulamak için bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="90d0a-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="90d0a-155">Oturum açma önce e-posta onayı gerektir</span><span class="sxs-lookup"><span data-stu-id="90d0a-155">Require email confirmation before log in</span></span>

<span data-ttu-id="90d0a-156">Şu anda bir kullanıcı kayıt formunu tamamladıktan sonra kullanıcılar oturum açtınız.</span><span class="sxs-lookup"><span data-stu-id="90d0a-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="90d0a-157">Genellikle bunları oturum açmadan önce e-postasına doğrulamak istersiniz.</span><span class="sxs-lookup"><span data-stu-id="90d0a-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="90d0a-158">Aşağıdaki bölümde, biz yeni kullanıcıların oturum önce onaylanan bir e-posta için (kimlik doğrulaması) için kodu değiştirin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="90d0a-159">Güncelleştirme `HttpPost Register` vurgulanan aşağıdaki değişikliklerle birlikte yöntemi:</span><span class="sxs-lookup"><span data-stu-id="90d0a-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="90d0a-160">Çıkış yorum tarafından `SignInAsync` yöntemi, kullanıcının kaydı tarafından oturumunuz açılacak değil.</span><span class="sxs-lookup"><span data-stu-id="90d0a-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="90d0a-161">`TempData["ViewBagLink"] = callbackUrl;` Satır için kullanılabilir [uygulamasında hata ayıklama](#dbg) ve kayıt e-posta göndermeden test edin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="90d0a-162">`ViewBag.Message` Onayla yönergeleri görüntülemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="90d0a-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="90d0a-163">[Örneği indirin](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) e-posta onayı e-posta ayarı olmadan test etmek için kodu içerir ve uygulamada hata ayıklamak için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="90d0a-164">Oluşturma bir `Views\Shared\Info.cshtml` dosyasını açıp aşağıdaki razor işaretlemesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="90d0a-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="90d0a-165">Ekleme [Authorize özniteliği](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) için `Contact` giriş denetleyici eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="90d0a-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="90d0a-166">Tıklayabilirsiniz **kişi** anonim kullanıcılar, erişimi yok ve kimliği doğrulanmış kullanıcılara erişimi doğrulamak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="90d0a-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="90d0a-167">Aynı zamanda güncelleştirmelisiniz `HttpPost Login` eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="90d0a-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="90d0a-168">Güncelleştirme *Views\Shared\Error.cshtml* hata iletisini görüntülemek için görünümü:</span><span class="sxs-lookup"><span data-stu-id="90d0a-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="90d0a-169">Herhangi bir hesabı silme **AspNetUsers** test etmek istediğiniz e-posta diğer adı içeren tablo.</span><span class="sxs-lookup"><span data-stu-id="90d0a-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="90d0a-170">Uygulamayı çalıştırın ve e-posta adresinizi doğrulayıncaya kadar oturum açamıyorsanız doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="90d0a-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="90d0a-171">E-posta adresinizi doğruladıktan sonra tıklayın **kişi** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="90d0a-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="90d0a-172">Parola kurtarma/sıfırlama</span><span class="sxs-lookup"><span data-stu-id="90d0a-172">Password recovery/reset</span></span>

<span data-ttu-id="90d0a-173">Açıklama karakterleri Kaldır `HttpPost ForgotPassword` hesabı denetleyicideki eylem yöntemi:</span><span class="sxs-lookup"><span data-stu-id="90d0a-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="90d0a-174">Açıklama karakterleri Kaldır `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) içinde *Views\Account\Login.cshtml* razor görünüm dosyası:</span><span class="sxs-lookup"><span data-stu-id="90d0a-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="90d0a-175">Oturum açma sayfasında artık parola sıfırlama bağlantısı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="90d0a-176">E-posta doğrulama bağlantısını yeniden gönder</span><span class="sxs-lookup"><span data-stu-id="90d0a-176">Resend email confirmation link</span></span>

<span data-ttu-id="90d0a-177">Bir kullanıcı yeni bir yerel hesap oluşturulduktan sonra kullanıcılar oturum açmadan önce kullanmak için gerekli onay bağlantısını e-posta gönderilir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="90d0a-178">Onay e-postası kullanıcının yanlışlıkla siler veya hiç e-posta geldiğinde, bunlar yeniden gönderilen onay bağlantısının gerekir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="90d0a-179">Aşağıdaki kod değişiklikleri, bunu etkinleştirmek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="90d0a-180">Aşağıdaki yardımcı yöntemini bölmenizin altına eklemek *Controllers\AccountController.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="90d0a-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="90d0a-181">Güncelleştirme kayıt yöntemi, yeni Yardımcısı'nı kullanmak için:</span><span class="sxs-lookup"><span data-stu-id="90d0a-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="90d0a-182">Kullanıcı hesabının onaylanıp değil ise parolayı yeniden göndermek için oturum açma yöntemi güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="90d0a-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="90d0a-183">Sosyal ve yerel oturum açma hesabını birleştirin</span><span class="sxs-lookup"><span data-stu-id="90d0a-183">Combine social and local login accounts</span></span>

<span data-ttu-id="90d0a-184">Yerel hem de sosyal hesaplar, e-posta bağlantısına tıklayarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90d0a-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="90d0a-185">Aşağıdaki sırayla **RickAndMSFT@gmail.com** yerel oturum açma oluşturulur, ancak bir sosyal günlük ilk olarak hesabı oluşturun, ardından bir yerel oturum açma ekleyin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="90d0a-186">Tıklayarak **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="90d0a-186">Click on the **Manage** link.</span></span> <span data-ttu-id="90d0a-187">Not **harici oturum açmaları: 0** bu hesapla ilişkilendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="90d0a-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="90d0a-188">Hizmet başka bir oturum açmak için bağlantıya tıklayın ve uygulama istekleri kabul edin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="90d0a-189">İki hesap birleştirilmiş, herhangi bir hesabı ile oturum açamayacaktır.</span><span class="sxs-lookup"><span data-stu-id="90d0a-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="90d0a-190">Kullanıcılarınız, sosyal kimlik doğrulama hizmeti günlüğü kullanılamıyor veya bunlar sosyal hesaplarına erişim daha büyük bir olasılıkla kaybetmiş durumunda yerel hesapları eklemek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="90d0a-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="90d0a-191">Aşağıdaki resimde, bir sosyal oturum açma Tom olduğu (görebileceğiniz gibi **harici oturum açmaları: 1** sayfasında gösterilen).</span><span class="sxs-lookup"><span data-stu-id="90d0a-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="90d0a-192">Tıklayarak **bir parola seçin** , yerel bir günlük eklemek aynı hesabı ile ilişkili sağlar.</span><span class="sxs-lookup"><span data-stu-id="90d0a-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="90d0a-193">Daha fazla ayrıntılı e-posta onayı</span><span class="sxs-lookup"><span data-stu-id="90d0a-193">Email confirmation in more depth</span></span>

<span data-ttu-id="90d0a-194">My öğretici [hesap onaylama ve parola kurtarma ASP.NET Identity ile](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) bu konuda daha fazla ayrıntı içeren geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="90d0a-195">Uygulamayı hata ayıklama</span><span class="sxs-lookup"><span data-stu-id="90d0a-195">Debugging the app</span></span>

<span data-ttu-id="90d0a-196">Bağlantıyı içeren bir e-posta alamazsanız:</span><span class="sxs-lookup"><span data-stu-id="90d0a-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="90d0a-197">Önemsiz ya da istenmeyen posta klasörünüzü kontrol edin.</span><span class="sxs-lookup"><span data-stu-id="90d0a-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="90d0a-198">SendGrid hesabınızda oturum açın ve tıklayarak [e-posta etkinliğinin bağlantı](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="90d0a-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="90d0a-199">E-posta olmadan doğrulama bağlantısını test etmek için Yükle [tamamlanan örnek](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="90d0a-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="90d0a-200">Onay bağlantısı ve doğrulama kodları sayfada görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="90d0a-201">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="90d0a-201">Additional Resources</span></span>

- [<span data-ttu-id="90d0a-202">Bağlantılar ASP.NET Identity önerilen kaynaklar</span><span class="sxs-lookup"><span data-stu-id="90d0a-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="90d0a-203">[Onaylama ve parola kurtarma ASP.NET Identity ile hesap](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve Hesap doğrulama hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="90d0a-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="90d0a-204">[Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide bir ASP.NET MVC 5 uygulaması Facebook ve Google OAuth 2 yetkilendirmesiyle yazma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="90d0a-205">Ayrıca kimlik veritabanı için ek veri ekleme işlemini de gösterir.</span><span class="sxs-lookup"><span data-stu-id="90d0a-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="90d0a-206">[Membership, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması Azure'da dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="90d0a-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="90d0a-207">Bu öğreticide Azure dağıtım ekler rolleri ile uygulamanızı güvenli hale getirmeyi kullanıcılar ve roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.</span><span class="sxs-lookup"><span data-stu-id="90d0a-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="90d0a-208">OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="90d0a-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="90d0a-209">Facebook uygulama oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="90d0a-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="90d0a-210">Projedeki SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="90d0a-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
