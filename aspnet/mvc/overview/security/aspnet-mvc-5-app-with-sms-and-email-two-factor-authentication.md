---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması | Microsoft Docs
author: Rick-Anderson
description: Bu öğreticide, iki öğeli kimlik doğrulaması ile bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir. Oluşturma güvenli bir ASP.NET MVC 5 web uygulaması ile tamamlamanız gerekir...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: f31015f5ebd660d0f95a2e978b305c1d272293d5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753384"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="186f5-104">SMS ve e-posta iki öğeli kimlik doğrulaması ile ASP.NET MVC 5 uygulaması</span><span class="sxs-lookup"><span data-stu-id="186f5-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="186f5-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="186f5-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="186f5-106">Bu öğreticide, iki öğeli kimlik doğrulaması ile bir ASP.NET MVC 5 web uygulaması oluşturma işlemini göstermektedir.</span><span class="sxs-lookup"><span data-stu-id="186f5-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="186f5-107">Tamamlamanız gereken [oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="186f5-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="186f5-108">Tamamlanmış uygulamayı karşıdan yükleyebileceğiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="186f5-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="186f5-109">İndirilen bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS olanak tanıyan hata ayıklama Yardımcıları bulunur.</span><span class="sxs-lookup"><span data-stu-id="186f5-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="186f5-110">Bu öğretici tarafından yazılmıştır [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="186f5-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="186f5-111">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="186f5-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="186f5-112">SMS ' için iki öğeli kimlik doğrulama ayarlayın</span><span class="sxs-lookup"><span data-stu-id="186f5-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="186f5-113">İki öğeli kimlik doğrulamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="186f5-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="186f5-114">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="186f5-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="186f5-115">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="186f5-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="186f5-116">Yükleme ve çalıştırmaya başlayın [Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="186f5-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="186f5-117">Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="186f5-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="186f5-118">Uyarı: Tamamlamanız gereken [oturum açma, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="186f5-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="186f5-119">Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="186f5-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="186f5-120">Yeni ASP.NET Web projesi oluşturun ve MVC şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="186f5-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="186f5-121">Web Forms da destekler ASP.NET Identity şekilde bir web forms uygulaması benzer adımları izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="186f5-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="186f5-122">Varsayılan kimlik doğrulaması olarak bırakın **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="186f5-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="186f5-123">Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunu işaretli bırakın.</span><span class="sxs-lookup"><span data-stu-id="186f5-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="186f5-124">Öğreticide daha sonra Azure'da dağıtacağız.</span><span class="sxs-lookup"><span data-stu-id="186f5-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="186f5-125">Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="186f5-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="186f5-126">Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="186f5-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="186f5-127">SMS ' için iki öğeli kimlik doğrulama ayarlayın</span><span class="sxs-lookup"><span data-stu-id="186f5-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="186f5-128">Bu öğretici, Twilio ya da ASPSMS kullanımıyla ilgili yönergeleri sağlar, ancak herhangi bir SMS Sağlayıcısı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="186f5-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="186f5-129">**Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="186f5-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="186f5-130">Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.</span><span class="sxs-lookup"><span data-stu-id="186f5-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="186f5-131">**Ek paketler yüklenirken veya hizmet başvuruları ekleme**</span><span class="sxs-lookup"><span data-stu-id="186f5-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="186f5-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="186f5-132">Twilio:</span></span>  
   <span data-ttu-id="186f5-133">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="186f5-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="186f5-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="186f5-134">ASPSMS:</span></span>  
   <span data-ttu-id="186f5-135">Aşağıdaki servis başvurusu eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="186f5-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="186f5-136">Adresi:</span><span class="sxs-lookup"><span data-stu-id="186f5-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="186f5-137">Ad alanı:</span><span class="sxs-lookup"><span data-stu-id="186f5-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="186f5-138">**SMS Sağlayıcısı kullanıcı kimlik bilgilerini başarınızda**</span><span class="sxs-lookup"><span data-stu-id="186f5-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="186f5-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="186f5-139">Twilio:</span></span>  
   <span data-ttu-id="186f5-140">Gelen **Pano** sekmesini Twilio hesabınızın kopyalama **hesap SID'si** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="186f5-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="186f5-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="186f5-141">ASPSMS:</span></span>  
   <span data-ttu-id="186f5-142">Hesap ayarlarınıza gidin **Userkey** kopyalayıp, şirket içinde tanımlanan birlikte **parola**.</span><span class="sxs-lookup"><span data-stu-id="186f5-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="186f5-143">Daha sonra bu değerleri depolarız *web.config* anahtarları dosyasında `"SMSAccountIdentification"` ve `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="186f5-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="186f5-144">**Senderıd belirtme / Düzenleyicisi**</span><span class="sxs-lookup"><span data-stu-id="186f5-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="186f5-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="186f5-145">Twilio:</span></span>  
   <span data-ttu-id="186f5-146">Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="186f5-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="186f5-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="186f5-147">ASPSMS:</span></span>  
   <span data-ttu-id="186f5-148">İçinde **kilidini düzenleyicileri** menüsünde, bir veya daha fazla düzenleyicileri kilidini veya bir alfasayısal gönderen (tüm ağlar tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="186f5-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="186f5-149">Bu değer daha sonra depolarız *web.config* anahtar dosyasında `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="186f5-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="186f5-150">**SMS Sağlayıcısı kimlik bilgilerinin uygulamaya aktarma**</span><span class="sxs-lookup"><span data-stu-id="186f5-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="186f5-151">Gönderen telefon numarası ve kimlik bilgileri uygulama için kullanılabilir hale.</span><span class="sxs-lookup"><span data-stu-id="186f5-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="186f5-152">Basit bir anlatım gözetildiği için bu değerleri depolarız *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="186f5-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="186f5-153">Biz Azure'a dağıtırken, güvenli bir şekilde giriş değerleri depolayabilir miyim **uygulama ayarları** bölümü web sitesinde yapılandırma sekmesi.</span><span class="sxs-lookup"><span data-stu-id="186f5-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="186f5-154">Güvenlik - hiçbir zaman deposu hassas verileri, kaynak kodunuzdaki.</span><span class="sxs-lookup"><span data-stu-id="186f5-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="186f5-155">Örneği basit tutmak için yukarıdaki kod, kimlik bilgilerini ve hesabı eklenir.</span><span class="sxs-lookup"><span data-stu-id="186f5-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="186f5-156">Bkz: [parolalar ve diğer hassas verileri ASP.NET ve Azure'a dağıtmak için en iyi yöntemler](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="186f5-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="186f5-157">**Veri aktarımı için SMS Sağlayıcısı uygulama**</span><span class="sxs-lookup"><span data-stu-id="186f5-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="186f5-158">Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="186f5-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="186f5-159">Ya da bağlı olarak kullanılan SMS Sağlayıcısı etkinleştirme **Twilio** veya **ASPSMS** bölümü:</span><span class="sxs-lookup"><span data-stu-id="186f5-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="186f5-160">Güncelleştirme *Views\Manage\Index.cshtml* Razor Görünüm: (Not: Yalnızca mevcut kod açıklamaları kaldırma yoksa, aşağıdaki kodu kullanın.)</span><span class="sxs-lookup"><span data-stu-id="186f5-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="186f5-161">Doğrulama `EnableTwoFactorAuthentication` ve `DisableTwoFactorAuthentication` eylem yöntemlerinde `ManageController` sahip[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="186f5-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="186f5-162">Uygulamayı çalıştırın ve daha önce kaydettiğiniz bir hesapla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="186f5-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="186f5-163">Tıklayın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="186f5-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="186f5-164">Ekle'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="186f5-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="186f5-165">`AddPhoneNumber` Eylem yöntemi, SMS mesajları alıp bir telefon numarası girmek için bir iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="186f5-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="186f5-166">Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="186f5-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="186f5-167">Girip tuşuna **Gönder**.</span><span class="sxs-lookup"><span data-stu-id="186f5-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="186f5-168">Telefon numaranız eklendi Yönet görüntüler.</span><span class="sxs-lookup"><span data-stu-id="186f5-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="186f5-169">İki öğeli kimlik doğrulamayı etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="186f5-169">Enable two-factor authentication</span></span>

<span data-ttu-id="186f5-170">Şablonu oluşturulan uygulamada iki öğeli kimlik doğrulamayı (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="186f5-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="186f5-171">2fa'yı etkinleştirmek için gezinti çubuğunda kullanıcı Kimliğinizi (e-posta diğer adı) tıklayın.</span><span class="sxs-lookup"><span data-stu-id="186f5-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="186f5-172">Üzerinde etkinleştirme 2fa'yı tıklayın.</span><span class="sxs-lookup"><span data-stu-id="186f5-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="186f5-173">Oturum kapatma, daha sonra yeniden oturum açın.</span><span class="sxs-lookup"><span data-stu-id="186f5-173">Logout, then log back in.</span></span> <span data-ttu-id="186f5-174">E-posta etkinleştirdiyseniz (bkz my [önceki öğreticide](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta için 2fa'yı seçin.</span><span class="sxs-lookup"><span data-stu-id="186f5-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="186f5-175">Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="186f5-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="186f5-176">Tıklayarak **bu tarayıcı Hatırlansın** onay kutusunu muaf, cihaz ve tarayıcı onay kutusunun seçili olduğu kullanırken oturum açma 2fa'yı kullanmaya gerek.</span><span class="sxs-lookup"><span data-stu-id="186f5-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="186f5-177">Kötü amaçlı kullanıcılar cihazınıza erişemezler sürece, 2fa'yı etkinleştirme ve tıklayarak **bu tarayıcı Hatırlansın** hala tüm erişim için güçlü 2FA koruma korurken kullanışlı bir adım parola erişimle sağlayacaktır. güvenilir olmayan cihazlardan.</span><span class="sxs-lookup"><span data-stu-id="186f5-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="186f5-178">Düzenli olarak kullandığınız özel cihaz üzerinde bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="186f5-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="186f5-179">Bu öğretici, yeni bir ASP.NET MVC uygulamasında 2fa'yı etkinleştirmek için hızlı bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="186f5-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="186f5-180">My öğretici [SMS ve e-posta ile ASP.NET Identity kullanılarak iki öğeli kimlik doğrulama](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) örnek arkasındaki kodu ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="186f5-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="186f5-181">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="186f5-181">Additional Resources</span></span>

- <span data-ttu-id="186f5-182">[SMS ve e-posta ile ASP.NET Identity kullanılarak iki öğeli kimlik doğrulama](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) iki öğeli kimlik doğrulaması ayrıntıya gider</span><span class="sxs-lookup"><span data-stu-id="186f5-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="186f5-183">Bağlantılar ASP.NET Identity önerilen kaynaklar</span><span class="sxs-lookup"><span data-stu-id="186f5-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="186f5-184">[Onaylama ve parola kurtarma ASP.NET Identity ile hesap](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve Hesap doğrulama hakkında daha fazla ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="186f5-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="186f5-185">[Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma ile MVC 5 uygulaması](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide bir ASP.NET MVC 5 uygulaması Facebook ve Google OAuth 2 yetkilendirmesiyle yazma işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="186f5-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="186f5-186">Ayrıca kimlik veritabanı için ek veri ekleme işlemini de gösterir.</span><span class="sxs-lookup"><span data-stu-id="186f5-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="186f5-187">[Membership, OAuth ve SQL veritabanı ile güvenli bir ASP.NET MVC uygulaması için Azure Web dağıtımı](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="186f5-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="186f5-188">Bu öğreticide Azure dağıtım ekler rolleri ile uygulamanızı güvenli hale getirmeyi kullanıcılar ve roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.</span><span class="sxs-lookup"><span data-stu-id="186f5-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="186f5-189">OAuth 2 için Google uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="186f5-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="186f5-190">Facebook uygulama oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="186f5-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="186f5-191">Projedeki SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="186f5-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
