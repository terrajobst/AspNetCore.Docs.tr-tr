---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Çoklu oturum açma (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 213b8fe091bcac7f55fd62ab305c77fcbc5a77ad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374386"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a><span data-ttu-id="2b56b-104">Çoklu oturum açma (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)</span><span class="sxs-lookup"><span data-stu-id="2b56b-104">Single Sign-On (Building Real-World Cloud Apps with Azure)</span></span>
====================
<span data-ttu-id="2b56b-105">tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="2b56b-105">by [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="2b56b-106">[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="2b56b-106">[Download Fix It Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) or [Download E-book](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)</span></span>

> <span data-ttu-id="2b56b-107">**Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-107">The **Building Real World Cloud Apps with Azure** e-book is based on a presentation developed by Scott Guthrie.</span></span> <span data-ttu-id="2b56b-108">13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="2b56b-108">It explains 13 patterns and practices that can help you be successful developing web apps for the cloud.</span></span> <span data-ttu-id="2b56b-109">E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2b56b-109">For information about the e-book, see [the first chapter](introduction.md).</span></span>


<span data-ttu-id="2b56b-110">Bulut uygulaması, geliştirmekte olduğunuz hakkında düşünmek için birçok güvenlik sorunları vardır, ancak bu dizi için yalnızca birinde odaklanacağız: çoklu oturum açma.</span><span class="sxs-lookup"><span data-stu-id="2b56b-110">There are many security issues to think about when you're developing a cloud app, but for this series we'll focus on just one: single sign-on.</span></span> <span data-ttu-id="2b56b-111">Bir soru genellikle istemem şudur: "ı öncelikle uygulama Şirketim; çalışanlar için oluşturmaya nasıl bu uygulamaları bulutta barındırmak ve bunları my çalışanlar bilmeniz ve bunlar uygulamaları çalıştırıyorsanız şirket içi ortamında kullanın aynı güvenlik modelini kullanmak yine de etkinleştirmek barındırılır güvenlik duvarı içindeki?"</span><span class="sxs-lookup"><span data-stu-id="2b56b-111">A question people often ask is this: "I'm primarily building apps for the employees of my company; how do I host these apps in the cloud and still enable them to use the same security model that my employees know and use in the on-premises environment when they're running apps that are hosted inside the firewall?"</span></span> <span data-ttu-id="2b56b-112">Biz bu senaryoyu yollarından biri, Azure Active Directory (Azure AD) olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-112">One of the ways we enable this scenario is called Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="2b56b-113">Azure AD, Kurumsal satır iş kolu (LOB) uygulamaları Internet üzerinden kullanımına olanak sağlar ve bu uygulamaları da iş ortakları için kullanılabilir olmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="2b56b-113">Azure AD enables you to make enterprise line-of-business (LOB) apps available over the Internet, and it enables you to make these apps available to business partners as well.</span></span>

## <a name="introduction-to-azure-ad"></a><span data-ttu-id="2b56b-114">Azure AD'ye giriş</span><span class="sxs-lookup"><span data-stu-id="2b56b-114">Introduction to Azure AD</span></span>

<span data-ttu-id="2b56b-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) sağlar [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) bulutta.</span><span class="sxs-lookup"><span data-stu-id="2b56b-115">[Azure AD](https://docs.microsoft.com/azure/active-directory/) provides [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) in the cloud.</span></span> <span data-ttu-id="2b56b-116">Temel özellikleri aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="2b56b-116">Key features include the following:</span></span>

- <span data-ttu-id="2b56b-117">Bu, şirket içi Active Directory ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-117">It integrates with on-premises Active Directory.</span></span>
- <span data-ttu-id="2b56b-118">Ancak, uygulamalarınızda çoklu oturum açmayı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-118">It enables single sign-on with your apps.</span></span>
- <span data-ttu-id="2b56b-119">Gibi açık standartları destekler [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Federasyon](http://en.wikipedia.org/wiki/WS-Federation), ve [OAuth 2.0](http://oauth.net/2/).</span><span class="sxs-lookup"><span data-stu-id="2b56b-119">It supports open standards such as [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), and [OAuth 2.0](http://oauth.net/2/).</span></span>
- <span data-ttu-id="2b56b-120">Kurumsal destekler [Graph REST API'si](https://msdn.microsoft.com/library/hh974476.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b56b-120">It supports Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).</span></span>

<span data-ttu-id="2b56b-121">Çalışanlarınızın Intranet uygulamalarında oturum sağlamak için kullandığınız bir şirket içi Windows Server Active Directory ortamında olduğunu varsayalım:</span><span class="sxs-lookup"><span data-stu-id="2b56b-121">Suppose you have an on-premises Windows Server Active Directory environment that you use to enable employees to sign on to Intranet apps:</span></span>

![](single-sign-on/_static/image1.png)

<span data-ttu-id="2b56b-122">Hangi Azure AD yapmanızı sağlayan, bulutta bir dizin oluşturun.</span><span class="sxs-lookup"><span data-stu-id="2b56b-122">What Azure AD enables you to do is create a directory in the cloud.</span></span> <span data-ttu-id="2b56b-123">Ücretsiz bir özelliktir ve kolay ayarlama.</span><span class="sxs-lookup"><span data-stu-id="2b56b-123">It's a free feature and easy to set up.</span></span>

<span data-ttu-id="2b56b-124">Şirket içi Active Directory'nizden tamamen bağımsız olabilir. Herkes Internet uygulamalarda kimlik doğrulaması yapmak ve bunu istediğiniz koyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-124">It can be entirely independent from your on-premises Active Directory; you can put anyone you want in it and authenticate them in Internet apps.</span></span>

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

<span data-ttu-id="2b56b-126">Ya da şirket içi ile tümleştirebilirsiniz AD.</span><span class="sxs-lookup"><span data-stu-id="2b56b-126">Or you can integrate it with your on-premises AD.</span></span>

![AD ve WAAD tümleştirme](single-sign-on/_static/image3.png)

<span data-ttu-id="2b56b-128">Artık şirket içi kimlik doğrulaması tüm çalışanları ayrıca Internet üzerinden – bir Güvenlik Duvarı'nı açın veya veri merkezinizdeki herhangi bir yeni sunucu dağıtma gerek kalmadan kimlik doğrulaması yapabilir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-128">Now all the employees who can authenticate on-premises can also authenticate over the Internet – without you having to open up a firewall or deploy any new servers in your data center.</span></span> <span data-ttu-id="2b56b-129">Bildiğiniz ve bugün iç uygulamalar çoklu oturum yeteneğini vermek için kullanabileceğiniz tüm mevcut Active Directory ortamında yararlanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-129">You can continue to leverage all the existing Active Directory environment that you know and use today to give your internal apps single-sign on capability.</span></span>

<span data-ttu-id="2b56b-130">AD ve Azure AD arasındaki bu bağlantı yaptığınız, web uygulamalarınız ve mobil cihazlarınızı çalışanlarınız bulutta kimlik doğrulaması için de etkinleştirebilirsiniz ve kabul etmek için Office 365, SalesForce.com ve Google apps gibi üçüncü taraf uygulamaları etkinleştirebilirsiniz sonra çalışanların kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="2b56b-130">Once you've made this connection between AD and Azure AD, you can also enable your web apps and your mobile devices to authenticate your employees in the cloud, and you can enable third-party apps, such as Office 365, SalesForce.com, or Google apps, to accept your employees' credentials.</span></span> <span data-ttu-id="2b56b-131">Office 365 kullanıyorsanız, Office 365 Azure AD kimlik doğrulama ve yetkilendirme için kullandığı için Azure AD ile ayarlamış.</span><span class="sxs-lookup"><span data-stu-id="2b56b-131">If you're using Office 365, you're already set up with Azure AD because Office 365 uses Azure AD for authentication and authorization.</span></span>

![3. taraf uygulamalar](single-sign-on/_static/image4.png)

<span data-ttu-id="2b56b-133">Bu yaklaşım güzelliği, kuruluşunuzun ekler ya da bir kullanıcıyı siler veya bir kullanıcının parola değişiklikleri, şirket içi ortamınızda günümüzde kullandığınız aynı işlemi kullanın.</span><span class="sxs-lookup"><span data-stu-id="2b56b-133">The beauty of this approach is that any time your organization adds or deletes a user, or a user changes a password, you use the same process that you use today in your on-premises environment.</span></span> <span data-ttu-id="2b56b-134">Tüm şirket içi bulut ortamında otomatik olarak AD değişiklikleri yayılır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-134">All of your on-premises AD changes are automatically propagated to the cloud environment.</span></span>

<span data-ttu-id="2b56b-135">Şirketiniz kullanarak veya güzel bir haberimiz var olan Office 365'e taşıma yapıyorsanız Azure AD'ye otomatik olarak Office 365, Azure AD kimlik doğrulaması için kullandığı için ayarlanmış sahip olursunuz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-135">If your company is using or moving to Office 365, the good news is that you'll have Azure AD set up automatically because Office 365 uses Azure AD for authentication.</span></span> <span data-ttu-id="2b56b-136">Bu nedenle kendi uygulamalarında kolayca Office 365 kullandığı aynı kimlik doğrulamasını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-136">So you can easily use in your own apps the same authentication that Office 365 uses.</span></span>

## <a name="set-up-an-azure-ad-tenant"></a><span data-ttu-id="2b56b-137">Bir Azure AD Kiracı ayarlamak</span><span class="sxs-lookup"><span data-stu-id="2b56b-137">Set up an Azure AD tenant</span></span>

<span data-ttu-id="2b56b-138">Azure AD dizini Azure AD çağrılır [Kiracı](https://technet.microsoft.com/library/jj573650.aspx), ve bir kiracı ayarı oldukça kolay.</span><span class="sxs-lookup"><span data-stu-id="2b56b-138">an Azure AD directory is called an Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx), and setting up a tenant is pretty easy.</span></span> <span data-ttu-id="2b56b-139">Kavramları göstermek için Azure Yönetim Portalı'nda nasıl yapıldığını göstereceğiz, ancak Elbette gibi diğer portal işlevler ayrıca bir betik veya yönetim API'si kullanarak bunu yapabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-139">We'll show you how it's done in the Azure Management Portal in order to illustrate the concepts, but of course like the other portal functions you can also do it by using a script or management API.</span></span>

<span data-ttu-id="2b56b-140">Yönetim Portalı'nda Active Directory sekmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="2b56b-140">In the management portal click the Active Directory tab.</span></span>

![Portalında WAAD](single-sign-on/_static/image5.png)

<span data-ttu-id="2b56b-142">Azure hesabınız için otomatik olarak bir Azure AD kiracısına sahip ve tıklayabilirsiniz **Ekle** ek dizinler oluşturmak için sayfanın alt kısmındaki düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2b56b-142">You automatically have one Azure AD tenant for your Azure account, and you can click the **Add** button at the bottom of the page to create additional directories.</span></span> <span data-ttu-id="2b56b-143">Örneğin bir test ortamı için bir tane ve bir üretim isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-143">You might want one for a test environment and one for production, for example.</span></span> <span data-ttu-id="2b56b-144">Yeni bir dizin adı hakkında dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="2b56b-144">Think carefully about what you name a new directory.</span></span> <span data-ttu-id="2b56b-145">Dizini için kendi adınızı kullanın ve ardından tekrar kullanıcılardan birinin için adınızı kullanırsanız, kafa karıştırıcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-145">If you use your name for the directory and then you use your name again for one of the users, that can be confusing.</span></span>

![Dizin Ekle](single-sign-on/_static/image6.png)

<span data-ttu-id="2b56b-147">Portalda oluşturma, silme ve bu ortamda kullanıcıları yönetmek için tam destek bulunur.</span><span class="sxs-lookup"><span data-stu-id="2b56b-147">The portal has full support for creating, deleting, and managing users within this environment.</span></span> <span data-ttu-id="2b56b-148">Örneğin, eklemek için bir kullanıcı Git **kullanıcılar** sekmesine **Kullanıcı Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2b56b-148">For example, to add a user go to the **Users** tab and click the **Add User** button.</span></span>

![Kullanıcı Ekle düğmesi](single-sign-on/_static/image7.png)

![Kullanıcı iletişim kutusu Ekle](single-sign-on/_static/image8.png)

<span data-ttu-id="2b56b-151">Yalnızca bu dizinde var. yeni bir kullanıcı oluşturabilir veya bir Microsoft Account bu dizini veya kaydı bir kullanıcı veya başka bir Azure AD dizininden bir kullanıcı olarak bu dizindeki bir kullanıcı olarak kaydedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-151">You can create a new user who exists only in this directory, or you can register a Microsoft Account as a user in this directory, or register or a user from another Azure AD directory as a user in this directory.</span></span> <span data-ttu-id="2b56b-152">(Gerçek bir dizinde ContosoTest.onmicrosoft.com varsayılan etki alanı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-152">(In a real directory, the default domain would be ContosoTest.onmicrosoft.com.</span></span> <span data-ttu-id="2b56b-153">Ayrıca kendi, contoso.com gibi seçtiğiniz bir etki alanı kullanabilirsiniz.)</span><span class="sxs-lookup"><span data-stu-id="2b56b-153">You can also use a domain of your own choosing, like contoso.com.)</span></span>

![Kullanıcı türleri](single-sign-on/_static/image9.png)

![Kullanıcı iletişim kutusu Ekle](single-sign-on/_static/image10.png)

<span data-ttu-id="2b56b-156">Kullanıcı rol atayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-156">You can assign the user to a role.</span></span>

![Kullanıcı profili](single-sign-on/_static/image11.png)

<span data-ttu-id="2b56b-158">Ve hesabı geçici bir parola ile oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="2b56b-158">And the account is created with a temporary password.</span></span>

![Geçici parola](single-sign-on/_static/image12.png)

<span data-ttu-id="2b56b-160">Bu şekilde oluşturduğunuz kullanıcılar hemen web uygulamalarınızı bu bulut dizinini kullanarak oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-160">The users you create this way can immediately log in to your web apps using this cloud directory.</span></span>

<span data-ttu-id="2b56b-161">Yine de Kurumsal çoklu oturum açma için harika nedir olan **dizin tümleştirme** sekmesinde:</span><span class="sxs-lookup"><span data-stu-id="2b56b-161">What's great for enterprise single sign-on, though, is the **Directory Integration** tab:</span></span>

![Dizin tümleştirme sekmesi](single-sign-on/_static/image13.png)

<span data-ttu-id="2b56b-163">Dizin tümleştirme etkinleştirirseniz ve [bir Aracı'nı indirme](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), mevcut şirket içi Active directory'nizle kuruluşunuz içinde zaten kullanmakta olduğunuz bu bulut dizinini eşitleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-163">If you enable directory integration, and [download a tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), you can sync this cloud directory with your existing on-premises Active Directory that you're already using inside your organization.</span></span> <span data-ttu-id="2b56b-164">Daha sonra dizininizde saklanan tüm kullanıcılar bu bulut dizini ile gösterilir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-164">Then all of the users stored in your directory will show up in this cloud directory.</span></span> <span data-ttu-id="2b56b-165">Bulut uygulamalarınızdaki tüm çalışanlarınız, mevcut Active Directory kimlik bilgilerini kullanarak doğrulayabilir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-165">Your cloud apps can now authenticate all of your employees using their existing Active Directory credentials.</span></span> <span data-ttu-id="2b56b-166">Ve tüm bu ücretsiz – eşitleme aracı ve Azure AD oturumunu.</span><span class="sxs-lookup"><span data-stu-id="2b56b-166">And all this is free – both the sync tool and Azure AD itself.</span></span>

<span data-ttu-id="2b56b-167">Bu ekran görüntüleri gördüğünüz gibi kullanımı kolay, bir sihirbaz aracıdır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-167">The tool is a wizard that is easy to use, as you can see from these screen shots.</span></span> <span data-ttu-id="2b56b-168">Bu yönergelerin yalnızca temel işlem gösteren örnek değildir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-168">These are not complete instructions, just an example showing you the basic process.</span></span> <span data-ttu-id="2b56b-169">Bağlantıları, daha ayrıntılı yardım-How-to--BT bilgi için bkz. [kaynakları](#resources) bölümün sonuna bölümü.</span><span class="sxs-lookup"><span data-stu-id="2b56b-169">For more detailed how-to-do-it information, see the links in the [Resources](#resources) section at the end of the chapter.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image14.png)

<span data-ttu-id="2b56b-171">Tıklayın **sonraki**ve ardından Azure Active Directory kimlik bilgilerinizi girin.</span><span class="sxs-lookup"><span data-stu-id="2b56b-171">Click **Next**, and then enter your Azure Active Directory credentials.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image15.png)

<span data-ttu-id="2b56b-173">Tıklayın **sonraki**ve şirket içi enter AD kimlik bilgileri.</span><span class="sxs-lookup"><span data-stu-id="2b56b-173">Click **Next**, and then enter your on-premises AD credentials.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image16.png)

<span data-ttu-id="2b56b-175">Tıklayın **sonraki**ve ardından AD parolalarınızı karma bulutta depolamak istiyorsanız gösterir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-175">Click **Next**, and then indicate if you want to store a hash of your AD passwords in the cloud.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image17.png)

<span data-ttu-id="2b56b-177">Tek yönlü karma bulutta depolayabilirsiniz. parola karması olur; Gerçek parolaları hiçbir zaman Azure AD içinde depolanır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-177">The password hash that you can store in the cloud is a one-way hash; actual passwords are never stored in Azure AD.</span></span> <span data-ttu-id="2b56b-178">Karma bulutta depolama karşı karar verirseniz, kullanmanız gerekir [Active Directory Federasyon Hizmetleri](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span><span class="sxs-lookup"><span data-stu-id="2b56b-178">If you decide against storing hashes in the cloud, you'll have to use [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS).</span></span> <span data-ttu-id="2b56b-179">Ayrıca [göz önüne alınması gereken diğer faktörleri ADFS kullanılıp kullanılmayacağı seçme](https://technet.microsoft.com/library/jj573653.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b56b-179">There are also [other factors to consider when choosing whether or not to use ADFS](https://technet.microsoft.com/library/jj573653.aspx).</span></span> <span data-ttu-id="2b56b-180">ADFS seçeneği birkaç ek yapılandırma adımları gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-180">The ADFS option requires a few additional configuration steps.</span></span>

<span data-ttu-id="2b56b-181">Karma bulutta depolamak seçtiğiniz, işiniz ve tıkladığınızda dizin eşitleme Aracı'nı başlatır **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="2b56b-181">If you choose to store hashes in the cloud, you're done, and the tool starts synchronizing directories when you click **Next**.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image18.png)

<span data-ttu-id="2b56b-183">Ve birkaç dakika içinde hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="2b56b-183">And in a few minutes you're done.</span></span>

![WAAD eşitleme aracı Yapılandırma Sihirbazı](single-sign-on/_static/image19.png)

<span data-ttu-id="2b56b-185">Bu kuruluşta Windows 2003'te veya daha yüksek bir etki alanı denetleyicisinde çalıştırmak yeterlidir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-185">You only have to run this on one domain controller in the organization, on Windows 2003 or higher.</span></span> <span data-ttu-id="2b56b-186">Ve yeniden başlatma gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2b56b-186">And no need to reboot.</span></span> <span data-ttu-id="2b56b-187">İşiniz bittiğinde, bulutta olan tüm kullanıcılarınız ve yapabileceğiniz herhangi bir web veya mobil uygulama, SAML, OAuth ve WS-Federasyon kullanan çoklu oturum açma.</span><span class="sxs-lookup"><span data-stu-id="2b56b-187">When you're done, all of your users are in the cloud and you can do single sign-on from any web or mobile application, using SAML, OAuth, or WS-Fed.</span></span>

<span data-ttu-id="2b56b-188">Bazen biz budur hakkında ne kadar güvenli sorulan – Microsoft kendi hassas iş verileri için kullanıyor mu?</span><span class="sxs-lookup"><span data-stu-id="2b56b-188">Sometimes we get asked about how secure this is – does Microsoft use it for their own sensitive business data?</span></span> <span data-ttu-id="2b56b-189">Ve yanıt evet'tir desteklemiyoruz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-189">And the answer is yes we do.</span></span> <span data-ttu-id="2b56b-190">Örneğin iç Microsoft SharePoint sitesinde gidin, [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), oturum açmak için girmem isteniyor.</span><span class="sxs-lookup"><span data-stu-id="2b56b-190">For example, if you go to the internal Microsoft SharePoint site at [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), you get prompted to log in.</span></span>

![Office 365 oturum açma](single-sign-on/_static/image20.png)

<span data-ttu-id="2b56b-192">Microsoft, ADFS, etkinleştirilmiş şekilde a Microsoft ID girdiğinizde, ADFS oturum açma sayfasına yeniden yönlendirilen.</span><span class="sxs-lookup"><span data-stu-id="2b56b-192">Microsoft has enabled ADFS, so when you enter a Microsoft ID, you get redirected to an ADFS log-in page.</span></span>

![ADFS oturum açma](single-sign-on/_static/image21.png)

<span data-ttu-id="2b56b-194">Ve bir iç Microsoft AD hesapta depolanan kimlik bilgileri girdikten sonra bu iç uygulama erişebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-194">And once you enter credentials stored in an internal Microsoft AD account, you have access to this internal application.</span></span>

![MS SharePoint sitesi](single-sign-on/_static/image22.png)

<span data-ttu-id="2b56b-196">Azure AD kullanılabilir hale geldi, ancak oturum açma işleminiz, buluttaki bir Azure AD dizini ile gittiği önce ayarlamanız ADFS zaten çoğunlukla vardı için bir AD sunucusu oturum açma kullanıyoruz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-196">We're using an AD sign-in server mainly because we already had ADFS set up before Azure AD became available, but the log-in process is going through an Azure AD directory in the cloud.</span></span> <span data-ttu-id="2b56b-197">Biz, müşterilerimize önemli belgeleri, kaynak denetimi, performans yönetimi dosyalarını, satış raporları ve bulutta daha koyun ve bunların güvenliğini sağlamak için bu tam aynı çözümü kullanıyorsanız.</span><span class="sxs-lookup"><span data-stu-id="2b56b-197">We put our important documents, source control, performance management files, sales reports, and more, in the cloud and are using this exact same solution to secure them.</span></span>

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a><span data-ttu-id="2b56b-198">Azure AD için çoklu oturum açmayı kullanan ASP.NET uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="2b56b-198">Create an ASP.NET app that uses Azure AD for single sign-on</span></span>

<span data-ttu-id="2b56b-199">Visual Studio birkaç ekran görüntüleri gördüğünüz gibi çoklu oturum açma için Azure AD kullanan bir uygulama oluşturmak çok kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-199">Visual Studio makes it really easy to create an app that uses Azure AD for single sign-on, as you can see from a few screen shots.</span></span>

<span data-ttu-id="2b56b-200">Yeni bir ASP.NET uygulaması, MVC veya Web Forms oluşturduğunuzda varsayılan kimlik doğrulama yöntemini ASP.NET kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-200">When you create a new ASP.NET application, either MVC or Web Forms, the default authentication method is ASP.NET Identity.</span></span> <span data-ttu-id="2b56b-201">Azure AD'ye değiştirmek için tıklayın bir **kimlik doğrulamasını Değiştir** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="2b56b-201">To change that to Azure AD, you click a **Change Authentication** button.</span></span>

![Kimlik doğrulamayı Değiştir](single-sign-on/_static/image23.png)

<span data-ttu-id="2b56b-203">Kurumsal hesaplar'ı seçin, etki alanı adınızı girin ve ardından, çoklu oturum açma seçin.</span><span class="sxs-lookup"><span data-stu-id="2b56b-203">Select Organizational Accounts, enter your domain name, and then select Single Sign On.</span></span>

![Kimlik doğrulaması iletişim kutusunu yapılandırma](single-sign-on/_static/image24.png)

<span data-ttu-id="2b56b-205">Ayrıca uygulama okuma verin veya izin dizin verilerini okuma/yazma.</span><span class="sxs-lookup"><span data-stu-id="2b56b-205">You can also give the app read or read/write permission for directory data.</span></span> <span data-ttu-id="2b56b-206">Bunu yaparsanız, kullanabileceğiniz [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) kullanıcıların telefon numarası aramak için son oturum olduğunda açık, vb. Office'te olup olmadıklarını öğrenmek.</span><span class="sxs-lookup"><span data-stu-id="2b56b-206">If you do that, it can use the [Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) to look up users' phone number, find out if they're in the office, when they last logged on, etc.</span></span>

<span data-ttu-id="2b56b-207">Tüm yapmanız gereken - Visual Studio için kimlik bilgileri Azure AD kiracınız için yönetici ister ve ardından hem proje hem de Azure AD kiracınıza yeni bir uygulama için yapılandırır.</span><span class="sxs-lookup"><span data-stu-id="2b56b-207">That's all you have to do - Visual Studio asks for the credentials for an administrator for your Azure AD tenant, and then it configures both your project and your Azure AD tenant for the new application.</span></span>

<span data-ttu-id="2b56b-208">Projeyi çalıştırdığınızda, bir oturum açma sayfası görürsünüz ve bir kullanıcının kimlik bilgileriyle Azure AD dizininizde oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-208">When you run the project, you'll see a sign-in page, and you can sign in with credentials of a user in your Azure AD directory.</span></span>

![Kuruluş hesabı oturum açma](single-sign-on/_static/image25.png)

![Oturum açanlar](single-sign-on/_static/image26.png)

<span data-ttu-id="2b56b-211">Uygulamayı Azure'a dağıtırken, yapmanız gereken tek şey seçin bir **kuruluş kimlik doğrulamasını etkinleştir** onay kutusunu işaretleyin ve yine Visual Studio sizin için tüm yapılandırma üstlenir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-211">When you deploy the app to Azure, all you have to do is select an **Enable Organizational Authentication** check box, and once again Visual Studio takes care of all the configuration for you.</span></span>

![Web'de Yayımla](single-sign-on/_static/image27.png)

<span data-ttu-id="2b56b-213">Bu ekran görüntüleri, Azure AD kimlik doğrulamasını kullanan bir uygulamayı nasıl oluşturacağınız gösteren tam bir adım adım öğretici gelen: [Azure Active Directory ile ASP.NET uygulamaları geliştirme](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span><span class="sxs-lookup"><span data-stu-id="2b56b-213">These screen shots come from a complete step-by-step tutorial that shows how to build an app that uses Azure AD authentication: [Developing ASP.NET Apps with Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).</span></span>

## <a name="summary"></a><span data-ttu-id="2b56b-214">Özet</span><span class="sxs-lookup"><span data-stu-id="2b56b-214">Summary</span></span>

<span data-ttu-id="2b56b-215">Bu bölümde, Azure Active Directory, Visual Studio ve ASP.NET, kuruluşunuzdaki kullanıcılar için Internet uygulamalarda çoklu oturum açmayı ayarlama kolaylaştırır olduğunu gördünüz.</span><span class="sxs-lookup"><span data-stu-id="2b56b-215">In this chapter you saw that Azure Active Directory, Visual Studio, and ASP.NET, make it easy to set up single sign-on in Internet applications for your organization's users.</span></span> <span data-ttu-id="2b56b-216">Kullanıcıların Internet uygulamalarında iç ağınızda Active Directory kullanarak oturum açmak için kullandıkları aynı kimlik bilgilerini kullanarak oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="2b56b-216">Your users can sign on in Internet apps using the same credentials they use to sign on using Active Directory in your internal network.</span></span>

<span data-ttu-id="2b56b-217">[Sonraki bölümde](data-storage-options.md) kullanılabilir bir bulut uygulaması için veri depolama seçenekleri bakar.</span><span class="sxs-lookup"><span data-stu-id="2b56b-217">The [next chapter](data-storage-options.md) looks at the data storage options available for a cloud app.</span></span>

<a id="resources"></a>
## <a name="resources"></a><span data-ttu-id="2b56b-218">Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2b56b-218">Resources</span></span>

<span data-ttu-id="2b56b-219">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="2b56b-219">For more information, see the following resources:</span></span>

- <span data-ttu-id="2b56b-220">[Azure Active Directory belgeleri](https://docs.microsoft.com/azure/active-directory/).</span><span class="sxs-lookup"><span data-stu-id="2b56b-220">[Azure Active Directory Documentation](https://docs.microsoft.com/azure/active-directory/).</span></span> <span data-ttu-id="2b56b-221">Azure AD belgelerinde windowsazure.com sitesinde için portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="2b56b-221">Portal page for Azure AD documentation on the windowsazure.com site.</span></span> <span data-ttu-id="2b56b-222">Adım adım öğreticiler için bkz. **geliştirme** bölümü.</span><span class="sxs-lookup"><span data-stu-id="2b56b-222">For step by step tutorials, see the **Develop** section.</span></span>
- <span data-ttu-id="2b56b-223">[Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/).</span><span class="sxs-lookup"><span data-stu-id="2b56b-223">[Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/).</span></span> <span data-ttu-id="2b56b-224">Azure multi-Factor authentication hakkında belgeler için portal sayfası.</span><span class="sxs-lookup"><span data-stu-id="2b56b-224">Portal page for documentation about multi-factor authentication in Azure.</span></span>
- <span data-ttu-id="2b56b-225">[Kurumsal hesap kimlik doğrulama seçenekleri](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span><span class="sxs-lookup"><span data-stu-id="2b56b-225">[Organizational account authentication options](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions).</span></span> <span data-ttu-id="2b56b-226">Visual Studio 2013 yeni proje iletişim kutusunda Azure AD kimlik doğrulama seçenekleri açıklaması.</span><span class="sxs-lookup"><span data-stu-id="2b56b-226">Explanation of the Azure AD authentication options in the Visual Studio 2013 new-project dialog.</span></span>
- <span data-ttu-id="2b56b-227">[Microsoft desenler ve uygulamalar - federe kimlik düzeni](https://msdn.microsoft.com/library/dn589790.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b56b-227">[Microsoft Patterns and Practices - Federated Identity Pattern](https://msdn.microsoft.com/library/dn589790.aspx).</span></span>
- <span data-ttu-id="2b56b-228">[Nasıl yapılır: Azure Active Directory Sync aracının yükleme](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b56b-228">[HowTo: Install the Azure Active Directory Sync Tool](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).</span></span>
- <span data-ttu-id="2b56b-229">[Active Directory Federasyon Hizmetleri 2.0 içerik haritası](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span><span class="sxs-lookup"><span data-stu-id="2b56b-229">[Active Directory Federation Services 2.0 Content Map](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx).</span></span> <span data-ttu-id="2b56b-230">AD FS 2.0 belgelerine bağlantılar.</span><span class="sxs-lookup"><span data-stu-id="2b56b-230">Links to documentation about ADFS 2.0.</span></span>
- <span data-ttu-id="2b56b-231">[Bir Windows Azure AD uygulaması ACL tabanlı ve rol tabanlı yetkilendirme](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span><span class="sxs-lookup"><span data-stu-id="2b56b-231">[Role-Based and ACL-Based Authorization in a Windows Azure AD Application](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1).</span></span> <span data-ttu-id="2b56b-232">Örnek uygulama.</span><span class="sxs-lookup"><span data-stu-id="2b56b-232">Sample application.</span></span>
- <span data-ttu-id="2b56b-233">[Azure Active Directory Graph API blogu](https://blogs.msdn.com/b/aadgraphteam/).</span><span class="sxs-lookup"><span data-stu-id="2b56b-233">[Azure Active Directory Graph API blog](https://blogs.msdn.com/b/aadgraphteam/).</span></span>
- <span data-ttu-id="2b56b-234">[Erişim denetimi KCG ve karma kimlik altyapısını dizin tümleştirme](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span><span class="sxs-lookup"><span data-stu-id="2b56b-234">[Access Control in BYOD and Directory Integration in a Hybrid Identity Infrastructure](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=).</span></span> <span data-ttu-id="2b56b-235">Teknik Ed 2014 oturumunda video Gayana Bagdasaryan.</span><span class="sxs-lookup"><span data-stu-id="2b56b-235">Tech Ed 2014 session video by Gayana Bagdasaryan.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2b56b-236">[Önceki](web-development-best-practices.md)
> [İleri](data-storage-options.md)</span><span class="sxs-lookup"><span data-stu-id="2b56b-236">[Previous](web-development-best-practices.md)
[Next](data-storage-options.md)</span></span>
