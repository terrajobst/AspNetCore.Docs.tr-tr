---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: SMS ve e-posta iki öğeli kimlik doğrulama ile ASP.NET MVC 5 uygulaması | Microsoft Docs
author: Rick-Anderson
description: Bu öğretici bir ASP.NET MVC 5 web uygulaması iki faktörlü kimlik doğrulamasıyla nasıl oluşturulacağını gösterir. Create güvenli bir ASP.NET MVC 5 web uygulaması ile tamamlanacaksa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 5e1c54b3901f2c8c85134445c1fa91ee9f2e0d59
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873618"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="fb3ad-104">SMS ve e-posta iki öğeli kimlik doğrulama ile ASP.NET MVC 5 uygulaması</span><span class="sxs-lookup"><span data-stu-id="fb3ad-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="fb3ad-105">tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="fb3ad-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="fb3ad-106">Bu öğretici bir ASP.NET MVC 5 web uygulaması iki faktörlü kimlik doğrulamasıyla nasıl oluşturulacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="fb3ad-107">Tamamlanmış olmalıdır [günlüğünde, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="fb3ad-108">Tamamlanan uygulama indirebilirsiniz [burada](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="fb3ad-109">Yükleme, bir e-posta veya SMS Sağlayıcısı ayarlamadan test e-posta onayı ve SMS izin hata ayıklama Yardımcıları içerir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="fb3ad-110">Bu öğretici tarafından yazıldı [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="fb3ad-111">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb3ad-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="fb3ad-112">İki faktörlü kimlik doğrulaması için SMS ayarlayın</span><span class="sxs-lookup"><span data-stu-id="fb3ad-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="fb3ad-113">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="fb3ad-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="fb3ad-114">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb3ad-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="fb3ad-115">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="fb3ad-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="fb3ad-116">Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="fb3ad-117">Yükleme [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="fb3ad-118">Uyarı: Tamamlamanız [günlüğünde, e-posta onayı ve parola sıfırlama ile güvenli bir ASP.NET MVC 5 web uygulaması oluşturma](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="fb3ad-119">Yüklemelisiniz [Visual Studio 2013 güncelleştirme 3'ü](https://go.microsoft.com/fwlink/?LinkId=390465) ya da bu öğreticiyi tamamlamak için daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="fb3ad-120">Yeni bir ASP.NET Web projesi oluşturun ve MVC şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="fb3ad-121">Web Forms da destekler ASP.NET kimliği için bir web forms uygulamasında benzer adımları izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="fb3ad-122">Varsayılan kimlik doğrulaması olarak bırakın **tek tek kullanıcı hesaplarını**.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="fb3ad-123">Uygulamayı azure'da barındırmak istiyorsanız, onay kutusunun işaretli bırakın.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="fb3ad-124">Daha sonra öğreticide biz Azure'a dağıtacaksınız.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="fb3ad-125">Yapabilecekleriniz [ücretsiz bir Azure hesabı açabilirsiniz](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="fb3ad-126">Ayarlama [SSL kullanmak üzere proje](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="fb3ad-127">İki faktörlü kimlik doğrulaması için SMS ayarlayın</span><span class="sxs-lookup"><span data-stu-id="fb3ad-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="fb3ad-128">Bu öğretici Twilio veya ASPSMS kullanma yönergeleri sağlar ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="fb3ad-129">**Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="fb3ad-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="fb3ad-130">Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya bir [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="fb3ad-131">**Ek paketler yükleme veya hizmet başvuruları ekleme**</span><span class="sxs-lookup"><span data-stu-id="fb3ad-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="fb3ad-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-132">Twilio:</span></span>  
   <span data-ttu-id="fb3ad-133">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="fb3ad-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-134">ASPSMS:</span></span>  
   <span data-ttu-id="fb3ad-135">Aşağıdaki hizmet başvurusu eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="fb3ad-136">Adresi:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="fb3ad-137">Ad alanı:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="fb3ad-138">**SMS Sağlayıcısı kullanıcı kimlik bilgilerini açık**</span><span class="sxs-lookup"><span data-stu-id="fb3ad-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="fb3ad-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-139">Twilio:</span></span>  
   <span data-ttu-id="fb3ad-140">Gelen **Pano** Twilio hesabınızı kopyalama sekmesinde **hesabının SID** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="fb3ad-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-141">ASPSMS:</span></span>  
   <span data-ttu-id="fb3ad-142">Hesap ayarlarınızı, gitmek **Userkey** ve, kendi kendine tanımlı birlikte kopyalayın **parola**.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="fb3ad-143">Biz daha sonra bu değerleri depolar *web.config* anahtarları dosyasında `"SMSAccountIdentification"` ve `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="fb3ad-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="fb3ad-144">**Senderıd belirtme / gönderen**</span><span class="sxs-lookup"><span data-stu-id="fb3ad-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="fb3ad-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-145">Twilio:</span></span>  
   <span data-ttu-id="fb3ad-146">Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="fb3ad-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-147">ASPSMS:</span></span>  
   <span data-ttu-id="fb3ad-148">İçinde **kilidini başlatıcılarını** menüsünde, bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="fb3ad-149">Biz bu değeri daha sonra depolayacaktır *web.config* anahtar içindeki dosya `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="fb3ad-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="fb3ad-150">**SMS Sağlayıcısı kimlik bilgileri bir uygulamaya aktarma**</span><span class="sxs-lookup"><span data-stu-id="fb3ad-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="fb3ad-151">Kimlik bilgileri ve gönderen telefon numarasını uygulama için kullanılabilir hale getirir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="fb3ad-152">Örneği basit tutmak için bu değerleri depolarız *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="fb3ad-153">Biz Azure'a dağıtırken, güvenli bir şekilde giriş değerleri depolayabilir miyim **uygulama ayarları** bölüm web sitesinde yapılandırma sekmesi.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="fb3ad-154">Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="fb3ad-155">Hesabı ve kimlik bilgileri örneği basit tutmak için yukarıdaki kod eklenir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="fb3ad-156">Bkz: [en iyi uygulamalar parolalar ve diğer hassas verileri ASP.NET ve Azure dağıtmak için](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="fb3ad-157">**Veri aktarımı için SMS sağlayıcı uygulaması**</span><span class="sxs-lookup"><span data-stu-id="fb3ad-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="fb3ad-158">Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="fb3ad-159">Ya da bağlı olarak kullanılan SMS sağlayıcı etkinleştirme **Twilio** veya **ASPSMS** bölümü:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="fb3ad-160">Güncelleştirme *Views\Manage\Index.cshtml* Razor Görünüm: (Not: değil yalnızca yorumları mevcut kodda kaldırmak için aşağıdaki kodu kullanın.)</span><span class="sxs-lookup"><span data-stu-id="fb3ad-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="fb3ad-161">Doğrulama `EnableTwoFactorAuthentication` ve `DisableTwoFactorAuthentication` eylem yöntemlerinde `ManageController` sahip[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) özniteliği:</span><span class="sxs-lookup"><span data-stu-id="fb3ad-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="fb3ad-162">Uygulamayı çalıştırın ve daha önce kaydettiğiniz hesabıyla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="fb3ad-163">Tıklatın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="fb3ad-164">Ekle'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="fb3ad-165">`AddPhoneNumber` Eylem yöntemi SMS iletileri alabilen bir telefon numarası girmek için bir iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="fb3ad-166">Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="fb3ad-167">Girip tuşuna **gönderme**.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="fb3ad-168">Telefon numaranız eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="fb3ad-169">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="fb3ad-169">Enable two-factor authentication</span></span>

<span data-ttu-id="fb3ad-170">Oluşturulan şablon uygulamada, iki faktörlü kimlik doğrulamasını (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="fb3ad-171">2FA etkinleştirmek için gezinti çubuğunda kullanıcı kimliği (e-posta diğer ad) tıklayın.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="fb3ad-172">Üzerinde 2FA Etkinleştir'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="fb3ad-173">Oturum kapatma, ardından günlük geri.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-173">Logout, then log back in.</span></span> <span data-ttu-id="fb3ad-174">E-posta etkinleştirdiyseniz (bkz my [önceki öğretici](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta 2FA için seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="fb3ad-175">Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="fb3ad-176">Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, tarayıcı ve cihaz Burada onay kutusunun seçili kullanırken oturum açmak için 2FA kullanmanıza gerek.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="fb3ad-177">Kötü niyetli kullanıcılar Cihazınızı erişim sağlayamadığı sürece, etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** hala tüm erişim için güçlü 2FA koruma korurken kullanışlı bir adım parola erişimle sağlayacaktır güvenilir olmayan cihazlardan.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="fb3ad-178">Düzenli olarak kullanmak istediğiniz özel cihazda bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="fb3ad-179">Bu öğretici, yeni bir ASP.NET MVC uygulaması üzerinde 2FA etkinleştirmek için bir giriş sağlar.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="fb3ad-180">My öğretici [SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) örnek arka plan kod üzerinde ayrıntıya gider.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="fb3ad-181">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb3ad-181">Additional Resources</span></span>

- <span data-ttu-id="fb3ad-182">[SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) iki öğeli kimlik doğrulama ayrıntılı girmeyeceğini</span><span class="sxs-lookup"><span data-stu-id="fb3ad-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="fb3ad-183">Önerilen bağlantılar ASP.NET Identity için kaynaklar</span><span class="sxs-lookup"><span data-stu-id="fb3ad-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="fb3ad-184">[Hesap onaylama ve parola kurtarma ile ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) parola kurtarma ve hesap onayı hakkında daha fazla ayrıntı girmeyeceğini.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="fb3ad-185">[MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Bu öğreticide ASP.NET MVC 5 uygulamayı yetkilendirme Facebook ve Google OAuth 2 ile yazmak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="fb3ad-186">Ayrıca, ek veri kimlik veritabanına eklemek nasıl gösterir.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="fb3ad-187">[Üyelik, OAuth ve SQL veritabanı ile Güvenli ASP.NET MVC uygulaması için Azure Web dağıtımı](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="fb3ad-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="fb3ad-188">Bu öğretici Azure dağıtım ekler uygulamanızı rolleri ile güvenli hale getirmek nasıl kullanıcılar, roller ve ek güvenlik özellikleri eklemek için üyelik API'si kullanma.</span><span class="sxs-lookup"><span data-stu-id="fb3ad-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="fb3ad-189">OAuth 2 için bir Google uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="fb3ad-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="fb3ad-190">İçinde Facebook uygulaması oluşturma ve uygulama projesine bağlanma</span><span class="sxs-lookup"><span data-stu-id="fb3ad-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="fb3ad-191">Projedeki SSL ayarlama</span><span class="sxs-lookup"><span data-stu-id="fb3ad-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
