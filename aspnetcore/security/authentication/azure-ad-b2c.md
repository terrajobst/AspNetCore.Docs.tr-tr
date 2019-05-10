---
title: Azure Active Directory B2C'de ASP.NET Core ile bulut kimlik doğrulaması
author: camsoper
description: ASP.NET Core ile Azure Active Directory B2C kimlik doğrulaması kurma keşfedin.
ms.date: 02/27/2019
ms.custom: mvc
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 86be999e02cfe34193bd594dcf89e8872590cca5
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64903302"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="f44b4-103">Azure Active Directory B2C'de ASP.NET Core ile bulut kimlik doğrulaması</span><span class="sxs-lookup"><span data-stu-id="f44b4-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="f44b4-104">Tarafından [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="f44b4-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="f44b4-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) olan bir bulut kimlik yönetimi çözümü, web ve mobil uygulamaları için.</span><span class="sxs-lookup"><span data-stu-id="f44b4-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="f44b4-106">Hizmet, bulutta ve şirket içinde barındırılan uygulamalar için kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="f44b4-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="f44b4-107">Kimlik doğrulama türleri bireysel hesaplar, sosyal ağ hesabı, içerir ve kurumsal hesaplarda Federasyon.</span><span class="sxs-lookup"><span data-stu-id="f44b4-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="f44b4-108">Ayrıca, Azure AD B2C minimal yapılandırma ile çok faktörlü kimlik doğrulaması sağlar.</span><span class="sxs-lookup"><span data-stu-id="f44b4-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="f44b4-109">Azure Active Directory (Azure AD) ve Azure AD B2C olan ayrı bir ürün teklifleri.</span><span class="sxs-lookup"><span data-stu-id="f44b4-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="f44b4-110">Azure AD kiracısı, Azure AD B2C kiracısı ile bağlı olan taraf uygulamaları kullanılacak kimlikleri koleksiyonunu temsil ederken, bir kuruluşun temsil eder.</span><span class="sxs-lookup"><span data-stu-id="f44b4-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="f44b4-111">Daha fazla bilgi için bkz: [Azure AD B2C: Sık sorulan sorular (SSS)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span><span class="sxs-lookup"><span data-stu-id="f44b4-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="f44b4-112">Bu öğreticide, bilgi nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="f44b4-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f44b4-113">Azure Active Directory B2C kiracısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f44b4-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="f44b4-114">Azure AD B2C'de bir uygulamayı kaydetme</span><span class="sxs-lookup"><span data-stu-id="f44b4-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="f44b4-115">Kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılmış bir ASP.NET Core web uygulaması oluşturmak için Visual Studio'yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="f44b4-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="f44b4-116">Azure AD B2C kiracısı davranışını denetleme ilkelerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f44b4-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f44b4-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="f44b4-117">Prerequisites</span></span>

<span data-ttu-id="f44b4-118">Bu kılavuz için aşağıdakiler gereklidir:</span><span class="sxs-lookup"><span data-stu-id="f44b4-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="f44b4-119">Microsoft Azure aboneliği</span><span class="sxs-lookup"><span data-stu-id="f44b4-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* <span data-ttu-id="f44b4-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (herhangi bir sürümü)</span><span class="sxs-lookup"><span data-stu-id="f44b4-120">[Visual Studio 2017](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) (any edition)</span></span>

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="f44b4-121">Azure Active Directory B2C kiracısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f44b4-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="f44b4-122">Bir Azure Active Directory B2C kiracısı oluşturmayı [belgelerinde açıklanan şekilde](/azure/active-directory-b2c/active-directory-b2c-get-started).</span><span class="sxs-lookup"><span data-stu-id="f44b4-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="f44b4-123">İstendiğinde, Kiracı bir Azure aboneliğiyle ilişkilendirme Bu öğretici için isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f44b4-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="f44b4-124">Azure AD B2C'de uygulamayı kaydetme</span><span class="sxs-lookup"><span data-stu-id="f44b4-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="f44b4-125">Kullanıp uygulamanızın yeni oluşturulan Azure AD B2C kiracısında kaydetme [belgelerindeki adımları](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) altında **bir web uygulaması kaydetme** bölümü.</span><span class="sxs-lookup"><span data-stu-id="f44b4-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/active-directory-b2c-app-registration#register-a-web-app) under the **Register a web app** section.</span></span> <span data-ttu-id="f44b4-126">Adresindeki Durdur **web uygulama gizli anahtarı oluşturma** bölümü.</span><span class="sxs-lookup"><span data-stu-id="f44b4-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="f44b4-127">Bir istemci parolası, Bu öğretici için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="f44b4-128">Aşağıdaki değerleri kullanın:</span><span class="sxs-lookup"><span data-stu-id="f44b4-128">Use the following values:</span></span>

| <span data-ttu-id="f44b4-129">Ayar</span><span class="sxs-lookup"><span data-stu-id="f44b4-129">Setting</span></span>                       | <span data-ttu-id="f44b4-130">Değer</span><span class="sxs-lookup"><span data-stu-id="f44b4-130">Value</span></span>                     | <span data-ttu-id="f44b4-131">Notlar</span><span class="sxs-lookup"><span data-stu-id="f44b4-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f44b4-132">**Ad**</span><span class="sxs-lookup"><span data-stu-id="f44b4-132">**Name**</span></span>                      | <span data-ttu-id="f44b4-133">*&lt;Uygulama adı&gt;*</span><span class="sxs-lookup"><span data-stu-id="f44b4-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="f44b4-134">Girin bir **adı** uygulamanızı müşterilere açıklayan bir uygulama için.</span><span class="sxs-lookup"><span data-stu-id="f44b4-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="f44b4-135">**/ Web API'si Web uygulaması Ekle**</span><span class="sxs-lookup"><span data-stu-id="f44b4-135">**Include web app / web API**</span></span> | <span data-ttu-id="f44b4-136">Evet</span><span class="sxs-lookup"><span data-stu-id="f44b4-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="f44b4-137">**Örtük akışa izin ver**</span><span class="sxs-lookup"><span data-stu-id="f44b4-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="f44b4-138">Evet</span><span class="sxs-lookup"><span data-stu-id="f44b4-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="f44b4-139">**Yanıt URL'si**</span><span class="sxs-lookup"><span data-stu-id="f44b4-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="f44b4-140">Yanıt URL'leri, Azure AD B2C, uygulamanız tarafından istenen belirteçleri döndürdüğü uç noktalardır.</span><span class="sxs-lookup"><span data-stu-id="f44b4-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="f44b4-141">Visual Studio kullanmak için bu yanıt URL'si sağlar.</span><span class="sxs-lookup"><span data-stu-id="f44b4-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="f44b4-142">Şimdilik girin `https://localhost:44300/signin-oidc` formu doldurun.</span><span class="sxs-lookup"><span data-stu-id="f44b4-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="f44b4-143">**Uygulama Kimliği URI'si**</span><span class="sxs-lookup"><span data-stu-id="f44b4-143">**App ID URI**</span></span>                | <span data-ttu-id="f44b4-144">Boş bırakın</span><span class="sxs-lookup"><span data-stu-id="f44b4-144">Leave blank</span></span>               | <span data-ttu-id="f44b4-145">Bu öğretici için gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="f44b4-146">**Yerel istemci Ekle**</span><span class="sxs-lookup"><span data-stu-id="f44b4-146">**Include native client**</span></span>     | <span data-ttu-id="f44b4-147">Hayır</span><span class="sxs-lookup"><span data-stu-id="f44b4-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="f44b4-148">Localhost olmayan yanıt URL'si ayarı farkında olmanız durumunda [yanıt URL'si listede izin verilen üzerindeki kısıtlamaları](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span><span class="sxs-lookup"><span data-stu-id="f44b4-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/active-directory-b2c-app-registration#choosing-a-web-app-or-api-reply-url).</span></span> 

<span data-ttu-id="f44b4-149">Uygulama kaydedildikten sonra kiracıdaki uygulamalar listesinde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="f44b4-150">Yalnızca kayıtlı uygulamayı seçin.</span><span class="sxs-lookup"><span data-stu-id="f44b4-150">Select the app that was just registered.</span></span> <span data-ttu-id="f44b4-151">Seçin **kopyalama** simgesinin sağındaki **uygulama kimliği** panoya kopyalamak için alana.</span><span class="sxs-lookup"><span data-stu-id="f44b4-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="f44b4-152">Hiçbir şey daha şu anda Azure AD B2C kiracısında yapılandırılabilir, ancak tarayıcı penceresini açık bırakın.</span><span class="sxs-lookup"><span data-stu-id="f44b4-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="f44b4-153">ASP.NET Core uygulaması oluşturduktan sonra daha fazla yapılandırma yoktur.</span><span class="sxs-lookup"><span data-stu-id="f44b4-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio-2017"></a><span data-ttu-id="f44b4-154">Visual Studio 2017'de bir ASP.NET Core uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="f44b4-154">Create an ASP.NET Core app in Visual Studio 2017</span></span>

<span data-ttu-id="f44b4-155">Visual Studio Web uygulama şablonu, kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılabilir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="f44b4-156">Visual Studio'da:</span><span class="sxs-lookup"><span data-stu-id="f44b4-156">In Visual Studio:</span></span>

1. <span data-ttu-id="f44b4-157">Yeni bir ASP.NET Core Web uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f44b4-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="f44b4-158">Seçin **Web uygulaması** şablonları listesinden.</span><span class="sxs-lookup"><span data-stu-id="f44b4-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="f44b4-159">Seçin **kimlik doğrulamayı Değiştir** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="f44b4-159">Select the **Change Authentication** button.</span></span>
    
    ![Değişiklik Authentication düğmesi](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="f44b4-161">İçinde **kimlik doğrulamayı Değiştir** iletişim kutusunda **bireysel kullanıcı hesapları**ve ardından **bulutta varolan bir kullanıcı deposuna bağlanın** açılır.</span><span class="sxs-lookup"><span data-stu-id="f44b4-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![Kimlik doğrulaması iletişim kutusu değişimi](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="f44b4-163">Formu aşağıdaki değerlerle izleyin:</span><span class="sxs-lookup"><span data-stu-id="f44b4-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="f44b4-164">Ayar</span><span class="sxs-lookup"><span data-stu-id="f44b4-164">Setting</span></span>                       | <span data-ttu-id="f44b4-165">Değer</span><span class="sxs-lookup"><span data-stu-id="f44b4-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="f44b4-166">**Etki alanı adı**</span><span class="sxs-lookup"><span data-stu-id="f44b4-166">**Domain Name**</span></span>               | <span data-ttu-id="f44b4-167">*&lt;B2C kiracınızın etki alanı adı&gt;*</span><span class="sxs-lookup"><span data-stu-id="f44b4-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="f44b4-168">**Uygulama Kimliği**</span><span class="sxs-lookup"><span data-stu-id="f44b4-168">**Application ID**</span></span>            | <span data-ttu-id="f44b4-169">*&lt;Panodan uygulama Kimliğini yapıştırın&gt;*</span><span class="sxs-lookup"><span data-stu-id="f44b4-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="f44b4-170">**Geri arama yolu**</span><span class="sxs-lookup"><span data-stu-id="f44b4-170">**Callback Path**</span></span>             | <span data-ttu-id="f44b4-171">*&lt;Varsayılan değeri kullanın&gt;*</span><span class="sxs-lookup"><span data-stu-id="f44b4-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="f44b4-172">**Kaydolma veya oturum açma ilkesi**</span><span class="sxs-lookup"><span data-stu-id="f44b4-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="f44b4-173">**Parola sıfırlama İlkesi**</span><span class="sxs-lookup"><span data-stu-id="f44b4-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="f44b4-174">**Profil ilkesini Düzenle**</span><span class="sxs-lookup"><span data-stu-id="f44b4-174">**Edit profile policy**</span></span>       | <span data-ttu-id="f44b4-175">*&lt;Boş bırakın&gt;*</span><span class="sxs-lookup"><span data-stu-id="f44b4-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="f44b4-176">Seçin **kopyalama** yanındaki bağlantı **yanıt URI'si** yanıt URI'si panoya kopyalamak için.</span><span class="sxs-lookup"><span data-stu-id="f44b4-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="f44b4-177">Seçin **Tamam** kapatmak için **kimlik doğrulamayı Değiştir** iletişim.</span><span class="sxs-lookup"><span data-stu-id="f44b4-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="f44b4-178">Seçin **Tamam** web uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="f44b4-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="f44b4-179">B2C uygulaması kaydı tamamlamak</span><span class="sxs-lookup"><span data-stu-id="f44b4-179">Finish the B2C app registration</span></span>

<span data-ttu-id="f44b4-180">B2C uygulaması özelliklerde hala açık tarayıcı penceresine dönün.</span><span class="sxs-lookup"><span data-stu-id="f44b4-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="f44b4-181">Geçici değiştirme **yanıt URL'si** belirtilen değere önceki Visual Studio'dan kopyalanır.</span><span class="sxs-lookup"><span data-stu-id="f44b4-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="f44b4-182">Seçin **Kaydet** pencerenin üst kısmındaki.</span><span class="sxs-lookup"><span data-stu-id="f44b4-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="f44b4-183">Yanıt URL'si kopyalarsanız yaramadı web proje özelliklerinde hata ayıklama sekmesinden HTTPS adresi kullanın ve ekleme **CallbackPath** değerini *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f44b4-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="f44b4-184">ilkeleri yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f44b4-184">Configure policies</span></span>

<span data-ttu-id="f44b4-185">Adımlar için Azure AD B2C belgeleri kullanmak [kaydolma veya oturum açma ilkesi oluşturma](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy)ve ardından [bir parola sıfırlama ilkesi oluşturma](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="f44b4-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-sign-up-or-sign-in-policy), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy).</span></span> <span data-ttu-id="f44b4-186">Belgeler için sağlanan örnek değerleri kullanın **kimlik sağlayıcıları**, **kaydolma özniteliklerini**, ve **uygulama taleplerini**.</span><span class="sxs-lookup"><span data-stu-id="f44b4-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="f44b4-187">Kullanarak **Şimdi Çalıştır** ilkeleri belgelerinde açıklanan şekilde test etmek için düğmeyi, isteğe bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="f44b4-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="f44b4-188">İlke adları belgelerinde açıklandığı gibi tam olarak bu ilkeleri de kullanılan gibi emin **kimlik doğrulamayı Değiştir** Visual Studio'da iletişim kutusu.</span><span class="sxs-lookup"><span data-stu-id="f44b4-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="f44b4-189">İlke adları içinde doğrulanabilir *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="f44b4-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearercookie-options"></a><span data-ttu-id="f44b4-190">Temel alınan OpenIdConnectOptions/JwtBearer/tanımlama bilgisi seçeneklerini yapılandırın</span><span class="sxs-lookup"><span data-stu-id="f44b4-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="f44b4-191">Doğrudan temel alınan seçeneklerini yapılandırmak için uygun düzeni sabiti kullanın `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f44b4-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="f44b4-192">Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="f44b4-192">Run the app</span></span>

<span data-ttu-id="f44b4-193">Visual Studio'da **F5** oluşturun ve uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f44b4-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="f44b4-194">Web uygulamasını başlattıktan sonra seçin **kabul** (istenirse) tanımlama bilgilerinin kullanımını kabul etmek ve ardından **oturum**.</span><span class="sxs-lookup"><span data-stu-id="f44b4-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![Uygulamada oturum açması](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="f44b4-196">Azure AD B2C kiracısı için tarayıcı yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="f44b4-197">(Bir ilkelerini sınama oluşturulduysa) var olan bir hesapla oturum oturum ya da seçin **şimdi kaydolun** yeni bir hesap oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="f44b4-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="f44b4-198">**Parolanızı mı unuttunuz?** bağlantı unutulmuş parola sıfırlama için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="f44b4-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C oturum açma](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="f44b4-200">Başarıyla oturum açtıktan sonra tarayıcının, web uygulamasına yeniden yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Başarılı](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="f44b4-202">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="f44b4-202">Next steps</span></span>

<span data-ttu-id="f44b4-203">Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="f44b4-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f44b4-204">Azure Active Directory B2C kiracısı oluşturma</span><span class="sxs-lookup"><span data-stu-id="f44b4-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="f44b4-205">Azure AD B2C'de bir uygulamayı kaydetme</span><span class="sxs-lookup"><span data-stu-id="f44b4-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="f44b4-206">Kimlik doğrulaması için Azure AD B2C kiracınızı kullanacak şekilde yapılandırılmış bir ASP.NET Core Web uygulaması oluşturmak için Visual Studio'yu kullanın.</span><span class="sxs-lookup"><span data-stu-id="f44b4-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="f44b4-207">Azure AD B2C kiracısı davranışını denetleme ilkelerini yapılandırma</span><span class="sxs-lookup"><span data-stu-id="f44b4-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="f44b4-208">ASP.NET Core uygulaması Azure AD B2C kimlik doğrulaması için kullanmak üzere yapılandırılmış göre [Authorize özniteliği](xref:security/authorization/simple) uygulamanızı güvenli hale getirmek için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="f44b4-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="f44b4-209">Öğrenme için uygulamanızı geliştirmeye devam edin:</span><span class="sxs-lookup"><span data-stu-id="f44b4-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="f44b4-210">[Azure AD B2C'yi kullanıcı arabirimini özelleştirme](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span><span class="sxs-lookup"><span data-stu-id="f44b4-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="f44b4-211">[Parola karmaşıklık gereksinimlerini yapılandırabilirsiniz](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span><span class="sxs-lookup"><span data-stu-id="f44b4-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="f44b4-212">[Çok faktörlü kimlik doğrulamasını etkinleştirme](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span><span class="sxs-lookup"><span data-stu-id="f44b4-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="f44b4-213">Gibi ek kimlik sağlayıcılarını yapılandırma [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter ](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)ve diğerleri.</span><span class="sxs-lookup"><span data-stu-id="f44b4-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="f44b4-214">[Azure AD Graph API'sini](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) Azure AD B2C kiracısı grup üyeliği gibi ek kullanıcı bilgileri alınamıyor.</span><span class="sxs-lookup"><span data-stu-id="f44b4-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="f44b4-215">[Bir ASP.NET Core web API'si Azure AD B2C kullanarak güvenli](xref:security/authentication/azure-ad-b2c-webapi).</span><span class="sxs-lookup"><span data-stu-id="f44b4-215">[Secure an ASP.NET Core web API using Azure AD B2C](xref:security/authentication/azure-ad-b2c-webapi).</span></span>
* <span data-ttu-id="f44b4-216">[Azure AD B2C kullanarak .NET web uygulamasından bir .NET web API'si çağırma](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span><span class="sxs-lookup"><span data-stu-id="f44b4-216">[Call a .NET web API from a .NET web app using Azure AD B2C](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-web-api-dotnet).</span></span>
