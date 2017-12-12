## <a name="register-the-database-context"></a><span data-ttu-id="aa88e-101">Veritabanı bağlamı kaydetme</span><span class="sxs-lookup"><span data-stu-id="aa88e-101">Register the database context</span></span>

<span data-ttu-id="aa88e-102">Bu adımda, veritabanı bağlamı kayıtlı [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcı.</span><span class="sxs-lookup"><span data-stu-id="aa88e-102">In this step, the database context is registered with the [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="aa88e-103">Bağımlılık ekleme (dı) kapsayıcı ile kaydedilen Hizmetleri (örneğin, DB bağlamı) denetleyicileri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="aa88e-103">Services (such as the DB context) that are registered with the dependency injection (DI) container are available to the controllers.</span></span>

<span data-ttu-id="aa88e-104">DB bağlamı için yerleşik destek kullanarak hizmet kapsayıcısı kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="aa88e-104">Register the DB context with the service container using the built-in support for [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="aa88e-105">Değiştir *haline* aşağıdaki kod ile dosya:</span><span class="sxs-lookup"><span data-stu-id="aa88e-105">Replace the contents of the *Startup.cs* file with the following code:</span></span>

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

<span data-ttu-id="aa88e-106">Önceki kod:</span><span class="sxs-lookup"><span data-stu-id="aa88e-106">The preceding code:</span></span>

* <span data-ttu-id="aa88e-107">Kullanılmayan kodunu kaldırır.</span><span class="sxs-lookup"><span data-stu-id="aa88e-107">Removes the code that is not used.</span></span>
* <span data-ttu-id="aa88e-108">Hizmet kapsayıcıya bir bellek içi veritabanına eklenen belirtir.</span><span class="sxs-lookup"><span data-stu-id="aa88e-108">Specifies an in-memory database is injected into the service container.</span></span>