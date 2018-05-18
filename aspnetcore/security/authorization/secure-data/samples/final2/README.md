# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="fa28b-101">Nasıl yapı/güvenli kullanıcı veri örneği çalıştırma</span><span class="sxs-lookup"><span data-stu-id="fa28b-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="fa28b-102">Parola Yöneticisi Aracı ile parola ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="fa28b-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="fa28b-103">Veritabanını güncelleştirme:</span><span class="sxs-lookup"><span data-stu-id="fa28b-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="fa28b-104">Projede SSL etkinleştir</span><span class="sxs-lookup"><span data-stu-id="fa28b-104">Enable SSL in the project</span></span>