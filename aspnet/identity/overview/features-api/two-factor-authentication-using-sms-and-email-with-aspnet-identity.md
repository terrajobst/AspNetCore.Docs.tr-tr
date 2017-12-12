---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: "SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması | Microsoft Docs"
author: HaoK
description: "Bu öğretici SMS ve e-posta kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak nasıl yapacağınızı gösterir. Bu makalede Rick Anderson tarafından yazılan ( @RickAndMSFT ), başına..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: ecb1fc693063995a3a05a7af5db64554c9f595e2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="a50c1-104">SMS ve e-posta ile ASP.NET Identity kullanan iki öğeli kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="a50c1-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="a50c1-105">tarafından [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="a50c1-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="a50c1-106">Bu öğretici SMS ve e-posta kullanarak iki faktörlü kimlik doğrulamasını (2FA) ayarlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="a50c1-107">Bu makalede Rick Anderson tarafından yazılan ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung ve Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="a50c1-108">NuGet örnek öncelikle Hao Kung tarafından yazıldı.</span><span class="sxs-lookup"><span data-stu-id="a50c1-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="a50c1-109">Bu konu aşağıdakileri kapsar:</span><span class="sxs-lookup"><span data-stu-id="a50c1-109">This topic covers the following:</span></span>

- [<span data-ttu-id="a50c1-110">Kimliği örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="a50c1-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="a50c1-111">İki faktörlü kimlik doğrulaması için SMS ayarlayın</span><span class="sxs-lookup"><span data-stu-id="a50c1-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="a50c1-112">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="a50c1-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="a50c1-113">İki öğeli kimlik doğrulama sağlayıcısını kaydetme</span><span class="sxs-lookup"><span data-stu-id="a50c1-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="a50c1-114">Sosyal ve yerel oturum açma hesaplarını birleştirmek</span><span class="sxs-lookup"><span data-stu-id="a50c1-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="a50c1-115">Deneme yanılma saldırılarına karşı hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="a50c1-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="a50c1-116">Kimliği örneği oluşturma</span><span class="sxs-lookup"><span data-stu-id="a50c1-116">Building the Identity sample</span></span>

<span data-ttu-id="a50c1-117">Bu bölümde, biz ile çalışacak bir örnek indirmek için NuGet kullanma.</span><span class="sxs-lookup"><span data-stu-id="a50c1-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="a50c1-118">Başlangıç yüklenmesi ve çalıştırılması [için Visual Studio Express 2013 Web](https://go.microsoft.com/fwlink/?LinkId=299058) veya [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="a50c1-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="a50c1-119">Visual Studio yükleme [2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521) ya da daha yüksek.</span><span class="sxs-lookup"><span data-stu-id="a50c1-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="a50c1-120">Uyarı: Visual Studio yüklemelisiniz [2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521) Bu öğreticiyi tamamlamak için.</span><span class="sxs-lookup"><span data-stu-id="a50c1-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="a50c1-121">Yeni bir ***boş*** ASP.NET Web projesi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="a50c1-122">Paket Yöneticisi konsolunda aşağıdaki girin aşağıdaki komutlar:</span><span class="sxs-lookup"><span data-stu-id="a50c1-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
 <span data-ttu-id="a50c1-123">Bu öğreticide, kullanacağız [SendGrid](http://sendgrid.com/) e-posta göndermek için ve [Twilio](https://www.twilio.com/) veya [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) sms metin için.</span><span class="sxs-lookup"><span data-stu-id="a50c1-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="a50c1-124">`Identity.Samples` Çalışmalarımız ile kod paketi yükler.</span><span class="sxs-lookup"><span data-stu-id="a50c1-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="a50c1-125">Ayarlama [SSL kullanmak üzere proje](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="a50c1-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="a50c1-126">*İsteğe bağlı*:'ndaki yönergeleri izleyin my [e-posta onayı öğretici](account-confirmation-and-password-recovery-with-aspnet-identity.md) SendGrid kanca uygulamayı çalıştırın ve bir e-posta hesabını kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="a50c1-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="a50c1-127">* İsteğe bağlı: * tanıtım e-posta bağlantısı onay kodu örnekten kaldırın ( `ViewBag.Link` hesap denetleyicisi kodu.</span><span class="sxs-lookup"><span data-stu-id="a50c1-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="a50c1-128">Bkz: `DisplayEmail` ve `ForgotPasswordConfirmation` eylem yöntemleri ve razor görünümleri).</span><span class="sxs-lookup"><span data-stu-id="a50c1-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="a50c1-129">* İsteğe bağlı: * kaldırmak `ViewBag.Status` yönetin ve hesap denetleyicilerinden gelen ve gelen kodu *Views\Account\VerifyCode.cshtml* ve *Views\Manage\VerifyPhoneNumber.cshtml* razor görünümleri.</span><span class="sxs-lookup"><span data-stu-id="a50c1-129">*Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml* and *Views\Manage\VerifyPhoneNumber.cshtml* razor views.</span></span> <span data-ttu-id="a50c1-130">Alternatif olarak, tutabilirsiniz `ViewBag.Status` bu uygulamayı yerel olarak takma ve e-posta ve SMS iletileri göndermek zorunda kalmadan nasıl çalıştığını test etmek için görüntüleme.</span><span class="sxs-lookup"><span data-stu-id="a50c1-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="a50c1-131">Uyarı: Bu örnekte güvenlik ayarlarından herhangi birini değiştirirseniz, açıkça yapılan değişiklikleri çağıran bir güvenlik denetimi uygulanabilecek üretim uygulamaları gerekir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="a50c1-132">İki faktörlü kimlik doğrulaması için SMS ayarlayın</span><span class="sxs-lookup"><span data-stu-id="a50c1-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="a50c1-133">Bu öğretici Twilio veya ASPSMS kullanma yönergeleri sağlar ancak herhangi bir SMS sağlayıcıyı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="a50c1-134">**Bir SMS Sağlayıcısı ile bir kullanıcı hesabı oluşturma**</span><span class="sxs-lookup"><span data-stu-id="a50c1-134">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="a50c1-135">Oluşturma bir [Twilio](https://www.twilio.com/try-twilio) veya bir [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) hesabı.</span><span class="sxs-lookup"><span data-stu-id="a50c1-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="a50c1-136">**Ek paketler yükleme veya hizmet başvuruları ekleme**</span><span class="sxs-lookup"><span data-stu-id="a50c1-136">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="a50c1-137">Twilio:</span><span class="sxs-lookup"><span data-stu-id="a50c1-137">Twilio:</span></span>  
 <span data-ttu-id="a50c1-138">Paket Yöneticisi konsolunda aşağıdaki komutu girin:</span><span class="sxs-lookup"><span data-stu-id="a50c1-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="a50c1-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a50c1-139">ASPSMS:</span></span>  
 <span data-ttu-id="a50c1-140">Aşağıdaki hizmet başvurusu eklenmesi gerekir:</span><span class="sxs-lookup"><span data-stu-id="a50c1-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
 <span data-ttu-id="a50c1-141">Adresi:</span><span class="sxs-lookup"><span data-stu-id="a50c1-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="a50c1-142">Ad alanı:</span><span class="sxs-lookup"><span data-stu-id="a50c1-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="a50c1-143">**SMS Sağlayıcısı kullanıcı kimlik bilgilerini açık**</span><span class="sxs-lookup"><span data-stu-id="a50c1-143">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="a50c1-144">Twilio:</span><span class="sxs-lookup"><span data-stu-id="a50c1-144">Twilio:</span></span>  
 <span data-ttu-id="a50c1-145">Gelen **Pano** Twilio hesabınızı kopyalama sekmesinde **hesabının SID** ve **kimlik doğrulama belirteci**.</span><span class="sxs-lookup"><span data-stu-id="a50c1-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="a50c1-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a50c1-146">ASPSMS:</span></span>  
 <span data-ttu-id="a50c1-147">Hesap ayarlarınızı, gitmek **Userkey** ve, kendi kendine tanımlı birlikte kopyalayın **parola**.</span><span class="sxs-lookup"><span data-stu-id="a50c1-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="a50c1-148">Daha sonra bu değerler değişkenlerde depolarız `SMSAccountIdentification` ve `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="a50c1-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="a50c1-149">**Senderıd belirtme / gönderen**</span><span class="sxs-lookup"><span data-stu-id="a50c1-149">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="a50c1-150">Twilio:</span><span class="sxs-lookup"><span data-stu-id="a50c1-150">Twilio:</span></span>  
 <span data-ttu-id="a50c1-151">Gelen **numaraları** sekmesinde, Twilio telefon numaranızı kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="a50c1-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a50c1-152">ASPSMS:</span></span>  
 <span data-ttu-id="a50c1-153">İçinde **kilidini başlatıcılarını** menüsünde, bir veya daha fazla başlatıcılarını kilidini veya bir alfasayısal başlatanın (tüm ağları tarafından desteklenmez) seçin.</span><span class="sxs-lookup"><span data-stu-id="a50c1-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="a50c1-154">Daha sonra bu değer değişken depolarız `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="a50c1-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="a50c1-155">**SMS Sağlayıcısı kimlik bilgileri bir uygulamaya aktarma**</span><span class="sxs-lookup"><span data-stu-id="a50c1-155">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="a50c1-156">Kimlik bilgileri ve gönderen telefon numarasını uygulama kullan:</span><span class="sxs-lookup"><span data-stu-id="a50c1-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="a50c1-157">Güvenlik - hiçbir zaman deposu gizli verileri kaynak kodu.</span><span class="sxs-lookup"><span data-stu-id="a50c1-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="a50c1-158">Hesabı ve kimlik bilgileri örneği basit tutmak için yukarıdaki kod eklenir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="a50c1-159">Tan Atten'ın bkz [ASP.NET MVC: kaynak denetimi özel ayarları dışında tutmak](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="a50c1-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="a50c1-160">**Veri aktarımı için SMS sağlayıcı uygulaması**</span><span class="sxs-lookup"><span data-stu-id="a50c1-160">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="a50c1-161">Yapılandırma `SmsService` sınıfını *uygulama\_Start\IdentityConfig.cs* dosya.</span><span class="sxs-lookup"><span data-stu-id="a50c1-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="a50c1-162">Ya da bağlı olarak kullanılan SMS sağlayıcı etkinleştirme **Twilio** veya **ASPSMS** bölümü:</span><span class="sxs-lookup"><span data-stu-id="a50c1-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="a50c1-163">Uygulamayı çalıştırın ve daha önce kaydettiğiniz hesabıyla oturum açın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="a50c1-164">Tıklatın etkinleştirir, kullanıcı kimliği üzerinde `Index` eylem yönteminde `Manage` denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="a50c1-165">Ekle'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="a50c1-166">Birkaç saniye içinde doğrulama kodunu içeren bir kısa mesaj alırsınız.</span><span class="sxs-lookup"><span data-stu-id="a50c1-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="a50c1-167">Girip tuşuna **gönderme**.</span><span class="sxs-lookup"><span data-stu-id="a50c1-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="a50c1-168">Telefon numaranız eklendi yönet görünümü gösterir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="a50c1-169">Kodu inceleyin</span><span class="sxs-lookup"><span data-stu-id="a50c1-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="a50c1-170">`Index` Eylem yönteminde `Manage` denetleyici, önceki eylemine dayalı durum iletisi ayarlar ve yerel parolanızı değiştirmek veya yerel bir hesap eklemek için bağlantılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="a50c1-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="a50c1-171">`Index` Yöntemi de durumu görüntüler veya sizin 2FA telefon numarası, dış oturum açma bilgileri, etkin 2FA ve 2FA yöntemi (daha sonra açıklanmıştır) bu tarayıcı için unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="a50c1-172">Kullanıcı Kimliğinizi (e-posta) başlık çubuğunda tıklayarak bir ileti geçmiyor.</span><span class="sxs-lookup"><span data-stu-id="a50c1-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="a50c1-173">Tıklayarak **telefon numarası: kaldırmak** bağlantı geçişleri `Message=RemovePhoneSuccess` bir sorgu dizesi olarak.</span><span class="sxs-lookup"><span data-stu-id="a50c1-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="a50c1-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="a50c1-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="a50c1-175">`AddPhoneNumber` Eylem yöntemi SMS iletileri alabilen bir telefon numarası girmek için bir iletişim kutusu görüntüler.</span><span class="sxs-lookup"><span data-stu-id="a50c1-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="a50c1-176">Tıklayarak **Gönder doğrulama kodu** düğmesi yazılarını telefon numarası için HTTP POST `AddPhoneNumber` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="a50c1-177">`GenerateChangePhoneNumberTokenAsync` Yöntem kısa mesajla ayarlanan güvenlik belirteci oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a50c1-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="a50c1-178">SMS hizmet yapılandırdıysanız simge dize olarak gönderilir &quot;güvenlik kodunuz &lt;belirteci&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="a50c1-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="a50c1-179">`SmsService.SendAsync` Yöntemi için zaman uyumsuz olarak çağrılır ve ardından uygulamayı yönlendireceği `VerifyPhoneNumber` (aşağıdaki iletişim kutusunu görüntüler) eylem yöntemi girdiğiniz doğrulama kodu.</span><span class="sxs-lookup"><span data-stu-id="a50c1-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="a50c1-180">Kodu girin ve Gönder'i tıklatın ardından, kod HTTP POST nakledilir `VerifyPhoneNumber` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="a50c1-181">`ChangePhoneNumberAsync` Yöntemi gönderilen güvenlik kodunu denetler.</span><span class="sxs-lookup"><span data-stu-id="a50c1-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="a50c1-182">Kodu doğru ise, telefon numarası eklenen `PhoneNumber` alanını `AspNetUsers` tablo.</span><span class="sxs-lookup"><span data-stu-id="a50c1-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="a50c1-183">Bu çağrı başarılı olursa `SignInAsync` yöntemi çağrılır:</span><span class="sxs-lookup"><span data-stu-id="a50c1-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="a50c1-184">`isPersistent` Parametresi, kimlik doğrulama oturumunun çoklu istekler arasında tutarlı olup olmadığını belirler.</span><span class="sxs-lookup"><span data-stu-id="a50c1-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="a50c1-185">Güvenlik profilinizi değiştirdiğinizde, yeni bir güvenlik damgası oluşturulur ve depolanır `SecurityStamp` alanını *AspNetUsers* tablo.</span><span class="sxs-lookup"><span data-stu-id="a50c1-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="a50c1-186">Not `SecurityStamp` alan güvenlik tanımlama bilgisinden farklıdır.</span><span class="sxs-lookup"><span data-stu-id="a50c1-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="a50c1-187">Güvenlik tanımlama bilgisi depolanmaz `AspNetUsers` tablosu (veya kimlik DB'de başka herhangi bir yerde).</span><span class="sxs-lookup"><span data-stu-id="a50c1-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="a50c1-188">Güvenlik tanımlama bilgisi belirteci kullanarak kendinden imzalı [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) ve ile oluşturulan `UserId, SecurityStamp` ve sona erme zamanı bilgileri.</span><span class="sxs-lookup"><span data-stu-id="a50c1-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/en-us/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="a50c1-189">Tanımlama bilgisi Ara her istekte tanımlama bilgisi denetler.</span><span class="sxs-lookup"><span data-stu-id="a50c1-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="a50c1-190">`SecurityStampValidator` Yönteminde `Startup` sınıfı DB İsabetli ve güvenlik damgasını düzenli olarak denetler ile belirtildiği gibi `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="a50c1-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="a50c1-191">Güvenlik profilinizi değiştirmediğiniz sürece bu yalnızca her 30 dakikada bir (bizim örnek) gerçekleşir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="a50c1-192">30 dakikalık zaman aralığı veritabanı gelişler en aza indirmek için seçildi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="a50c1-193">`SignInAsync` Yöntemi güvenlik profili herhangi bir değişiklik yapıldığında çağrılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="a50c1-194">Güvenlik profili değiştiğinde, güncelleştirmelerinin veritabanıdır `SecurityStamp` alan ve çağırmadan `SignInAsync` içinde oturum kalmak yöntemi *yalnızca* sonraki kadar OWIN ardışık düzenini veritabanı isabetler ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="a50c1-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="a50c1-195">Bu değiştirerek test `SignInAsync` hemen döndürülecek yöntemi ve tanımlama bilgisi ayarı `validateInterval` 30 dakika özelliğinden 5 saniye:</span><span class="sxs-lookup"><span data-stu-id="a50c1-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="a50c1-196">Yukarıdaki kod değişikliklerle güvenlik profilinizi değiştirebilirsiniz (durumunu değiştirerek Örneğin, **iki Faktörlü etkin**) 5 saniye içinde kaydedilir zaman `SecurityStampValidator.OnValidateIdentity` yöntemi başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="a50c1-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="a50c1-197">Dönüş satırda kaldırmak `SignInAsync` yöntemi, başka bir güvenlik profili değişiklik yapabilir ve, günlüğe kaydedilmez. `SignInAsync` Yöntem yeni bir güvenlik tanımlama bilgisi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a50c1-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="a50c1-198">İki faktörlü kimlik doğrulamasını etkinleştir</span><span class="sxs-lookup"><span data-stu-id="a50c1-198">Enable two-factor authentication</span></span>

<span data-ttu-id="a50c1-199">Örnek uygulamasında iki faktörlü kimlik doğrulamasını (2FA) etkinleştirmek için kullanıcı arabirimini kullanmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="a50c1-200">2FA etkinleştirmek için gezinti çubuğunda kullanıcı kimliği (e-posta diğer ad) tıklayın.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="a50c1-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="a50c1-201">Üzerinde 2FA Etkinleştir'i tıklatın.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="a50c1-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="a50c1-202">Oturumu kapatın ve tekrar açın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-202">Log out, then log back in.</span></span> <span data-ttu-id="a50c1-203">E-posta etkinleştirdiyseniz (bkz my [önceki öğretici](account-confirmation-and-password-recovery-with-aspnet-identity.md)), SMS veya e-posta 2FA için seçebilirsiniz.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="a50c1-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="a50c1-204">Kod (SMS veya e-posta) girebileceğiniz doğrulayın kod sayfası görüntülenir.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="a50c1-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="a50c1-205">Tıklayarak **bu tarayıcı unutmayın** onay kutusunu muaf, o bilgisayar ve tarayıcı ile oturum açmak için 2FA kullanmaya gerek.</span><span class="sxs-lookup"><span data-stu-id="a50c1-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="a50c1-206">Etkinleştirme 2FA ve tıklayarak **bu tarayıcı unutmayın** bilgisayarınıza erişimi olmayan sürece, hesabınıza erişmeniz çalışılırken kötü niyetli kullanıcıların güçlü 2FA korumasıyla sağlayacaktır.</span><span class="sxs-lookup"><span data-stu-id="a50c1-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="a50c1-207">Düzenli olarak kullandığınız özel makine üzerinde bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="a50c1-208">Ayarlayarak **bu tarayıcı unutmayın**, düzenli olarak kullanmama bilgisayarlardan 2FA ek güvenlik almak ve 2FA kendi bilgisayarlarda gitmek zorunda değil üzerinde kolaylık alın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="a50c1-209">İki öğeli kimlik doğrulama sağlayıcısını kaydetme</span><span class="sxs-lookup"><span data-stu-id="a50c1-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="a50c1-210">Yeni bir MVC projesi oluşturduğunuzda *IdentityConfig.cs* dosyası iki öğeli kimlik doğrulama sağlayıcısını kaydetmek için aşağıdaki kodu içerir:</span><span class="sxs-lookup"><span data-stu-id="a50c1-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="a50c1-211">2FA için bir telefon numarası ekleme</span><span class="sxs-lookup"><span data-stu-id="a50c1-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="a50c1-212">`AddPhoneNumber` Eylem yönteminde `Manage` denetleyicisi bir güvenlik belirteci oluşturur ve bu telefon numarası gönderir olması koşuluyla.</span><span class="sxs-lookup"><span data-stu-id="a50c1-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="a50c1-213">Belirteç gönderdikten sonra onu yönlendirir `VerifyPhoneNumber` eylem yöntemi, burada girebilirsiniz 2FA için SMS kaydetmek için kod.</span><span class="sxs-lookup"><span data-stu-id="a50c1-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="a50c1-214">Telefon numarası olana kadar SMS 2FA kullanılmaz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="a50c1-215">2FA etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="a50c1-215">Enabling 2FA</span></span>

<span data-ttu-id="a50c1-216">`EnableTFA` Eylem yöntemi 2FA sağlar:</span><span class="sxs-lookup"><span data-stu-id="a50c1-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="a50c1-217">Not `SignInAsync` etkinleştir 2FA güvenlik profili için bir değişiklik olduğundan çağrılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="a50c1-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="a50c1-218">2FA etkinleştirildiğinde, kullanıcı oturum açma 2FA kullanın (SMS ve e-posta örnekteki) kayıtlı 2FA yaklaşımlar kullanılarak gerekir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="a50c1-219">QR kodu oluşturucuları gibi daha fazla 2FA sağlayıcıları ekleyebilir veya sahip olduğunuz yazabilirsiniz (bkz [kullanarak Google Authenticator ile ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="a50c1-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="a50c1-220">2FA kodları kullanılarak oluşturulan [zamana dayalı kerelik parola algoritması](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) ve kodları altı dakika için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="a50c1-221">Altı kodu girmek için dakikadan uzun sürerse geçersiz kod hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="a50c1-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="a50c1-222">Sosyal ve yerel oturum açma hesaplarını birleştirmek</span><span class="sxs-lookup"><span data-stu-id="a50c1-222">Combine social and local login accounts</span></span>

<span data-ttu-id="a50c1-223">Yerel ve sosyal hesapları, e-posta bağlantısını tıklatarak birleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="a50c1-224">Aşağıdaki sırayla &quot; RickAndMSFT@gmail.com &quot; yerel bir oturum olarak oluşturulur, ancak ilk sosyal günlüğüne olarak hesabı oluşturun, sonra yerel oturum açma ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a50c1-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="a50c1-225">Tıklayın **Yönet** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="a50c1-225">Click on the **Manage** link.</span></span> <span data-ttu-id="a50c1-226">Bu hesapla ilişkilendirilen 0 dış (sosyal oturum açma bilgileri) unutmayın.</span><span class="sxs-lookup"><span data-stu-id="a50c1-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="a50c1-227">Hizmet başka bir oturum açmak için bağlantıya tıklayın ve uygulama isteklerini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="a50c1-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="a50c1-228">İki hesap birlikte, her iki hesabı ile oturum açabilecek olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a50c1-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="a50c1-229">Kullanıcının sosyal oturum açtığında kimlik doğrulama hizmeti çalışmıyor ya da daha büyük bir olasılıkla bunlar erişim sosyal hesaplarında kesilmiş durumda yerel hesapları eklemek için kullanıcılarınızın isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="a50c1-230">Aşağıdaki görüntüde, sosyal günlüğüne zel olduğu (gelen görebileceğiniz **dış oturum açma: 1** sayfasında gösterilen).</span><span class="sxs-lookup"><span data-stu-id="a50c1-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="a50c1-231">Tıklayarak **parola çekme** yerel günlüğe eklemek aynı hesabı ile ilişkili sağlar.</span><span class="sxs-lookup"><span data-stu-id="a50c1-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="a50c1-232">Deneme yanılma saldırılarına karşı hesap kilitleme</span><span class="sxs-lookup"><span data-stu-id="a50c1-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="a50c1-233">Hesapları, kullanıcı kilitlemesinin etkinleştirerek uygulamanızdan sözlük saldırılarına karşı koruyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="a50c1-234">Aşağıdaki kod `ApplicationUserManager Create` yöntemi kilitlenme yapılandırır:</span><span class="sxs-lookup"><span data-stu-id="a50c1-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="a50c1-235">Yukarıdaki kod kilitleme için yalnızca iki faktörlü kimlik doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="a50c1-236">Oturum açma kilitlenme değiştirerek etkinleştirebilirsiniz sırada `shouldLockout` true `Login` yöntemi hesap denetleyicisinin öneririz, etkinleştirmemeniz kilitlenme oturum açma hesabı açıktır. yaptığından [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) oturum açma saldırıları.</span><span class="sxs-lookup"><span data-stu-id="a50c1-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="a50c1-237">Oluşturduğunuz yönetici hesabı için örnek kodda kilitleme devre dışı `ApplicationDbInitializer Seed` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a50c1-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="a50c1-238">Doğrulanmış e-posta hesabına sahip bir kullanıcının gerektiren</span><span class="sxs-lookup"><span data-stu-id="a50c1-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="a50c1-239">Aşağıdaki kod, bir kullanıcı oturum önce bir doğrulanmış e-posta hesabınız gerektirir:</span><span class="sxs-lookup"><span data-stu-id="a50c1-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="a50c1-240">Nasıl SignInManager 2FA gereksinimini denetler</span><span class="sxs-lookup"><span data-stu-id="a50c1-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="a50c1-241">Hem yerel oturum açma ve sosyal günlüğüne 2FA etkin olup olmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="a50c1-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="a50c1-242">2FA etkinleştirilirse, `SignInManager` oturum açma yöntemini döndürür `SignInStatus.RequiresVerification`, ve kullanıcı yönlendirilecek `SendCode` sahip oldukları sırada oturum tamamlamak için kodu girmek için eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="a50c1-243">Kullanıcılar yerel tanımlama bilgisinde, kullanıcının RememberMe varsa ayarlanır `SignInManager` döndürülecek `SignInStatus.Success` ve 2FA Git gerekmez.</span><span class="sxs-lookup"><span data-stu-id="a50c1-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="a50c1-244">Aşağıdaki kodda gösterildiği `SendCode` eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="a50c1-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="a50c1-245">A [Selectlistıtem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) kullanıcı için etkinleştirilmiş tüm 2FA yöntemleriyle oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a50c1-245">A [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="a50c1-246">[Selectlistıtem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) geçirilir [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) 2FA yaklaşım (genellikle e-posta ve SMS) seçmesini sağlayan Yardımcısı.</span><span class="sxs-lookup"><span data-stu-id="a50c1-246">The [SelectListItem](https://msdn.microsoft.com/en-us/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="a50c1-247">Kullanıcı 2FA yaklaşım yazılarını sonra `HTTP POST SendCode` eylem yöntemi çağrıldıktan `SignInManager` 2FA kod ve kullanıcı için yönlendirildiği gönderir `VerifyCode` eylem yönteminin nerede bunlar oturum açmanızı tamamlamak için kodunu girin.</span><span class="sxs-lookup"><span data-stu-id="a50c1-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="a50c1-248">2FA kilitleme</span><span class="sxs-lookup"><span data-stu-id="a50c1-248">2FA Lockout</span></span>

<span data-ttu-id="a50c1-249">Oturum açma parola denemesi hatalarında hesap kilitleme belirlenebiliyor olsa da, bu yaklaşım, oturum açma getirir [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) kilitlemeleri.</span><span class="sxs-lookup"><span data-stu-id="a50c1-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="a50c1-250">Hesap kilitleme yalnızca 2FA ile kullanmanızı öneririz.</span><span class="sxs-lookup"><span data-stu-id="a50c1-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="a50c1-251">Zaman `ApplicationUserManager` olan oluşturulan örnek kodu 2FA kilitleme ayarlar ve `MaxFailedAccessAttemptsBeforeLockout` beş.</span><span class="sxs-lookup"><span data-stu-id="a50c1-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="a50c1-252">Bir kullanıcı (yerel hesap veya sosyal hesap aracılığıyla), oturum sonra her başarısız girişimden 2FA konumunda depolanır ve en fazla deneme ulaşıldığında, kullanıcı beş dakika kilitlenmeden (süresiyle kilitleme ayarlayabilirsiniz `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="a50c1-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="a50c1-253">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="a50c1-253">Additional Resources</span></span>

- <span data-ttu-id="a50c1-254">[ASP.NET Identity önerilen kaynakları](../getting-started/aspnet-identity-recommended-resources.md) tam listesi kimlik bloglar, videoları, eğitim ve mükemmel şekilde bağlar.</span><span class="sxs-lookup"><span data-stu-id="a50c1-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="a50c1-255">[MVC 5 uygulamayla Facebook, Twitter, LinkedIn ve Google OAuth2 oturum açma](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) Ayrıca kullanıcıların tabloya profil bilgilerini eklemeyi gösterir.</span><span class="sxs-lookup"><span data-stu-id="a50c1-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="a50c1-256">[ASP.NET MVC ve kimlik 2.0: temel kavramları anlama](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) John Atten tarafından.</span><span class="sxs-lookup"><span data-stu-id="a50c1-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="a50c1-257">Hesap onaylamayı ve parola kurtarma ASP.NET kimliğe sahip</span><span class="sxs-lookup"><span data-stu-id="a50c1-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="a50c1-258">ASP.NET Identity giriş</span><span class="sxs-lookup"><span data-stu-id="a50c1-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="a50c1-259">[ASP.NET Identity 2.0.0 RTM Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) Pranav Rastogi tarafından.</span><span class="sxs-lookup"><span data-stu-id="a50c1-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="a50c1-260">[ASP.NET Identity 2.0: Hesap doğrulama ve iki öğeli yetkilendirme ayarlama](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) John Atten tarafından.</span><span class="sxs-lookup"><span data-stu-id="a50c1-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
