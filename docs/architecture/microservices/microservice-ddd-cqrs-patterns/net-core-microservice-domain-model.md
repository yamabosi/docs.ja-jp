---
title: .NET Core でマイクロサービス ドメイン モデルを実装する
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | DDD 指向ドメイン モデルの実装の詳細について
ms.date: 10/08/2018
ms.openlocfilehash: b2ad62c2a16dd3993b9624ec14f0070e934ac2de
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68676589"
---
# <a name="implement-a-microservice-domain-model-with-net-core"></a><span data-ttu-id="2338a-103">.NET Core でマイクロサービス ドメイン モデルを実装する</span><span class="sxs-lookup"><span data-stu-id="2338a-103">Implement a microservice domain model with .NET Core</span></span>

<span data-ttu-id="2338a-104">前のセクションでは、ドメイン モデルの基本的な設計原則と設計パターンを説明しました。</span><span class="sxs-lookup"><span data-stu-id="2338a-104">In the previous section, the fundamental design principles and patterns for designing a domain model were explained.</span></span> <span data-ttu-id="2338a-105">ここでは、.NET Core (プレーンな C\# コード) と EF Core を使用してドメイン モデルを実装するために可能な手段を調べます。</span><span class="sxs-lookup"><span data-stu-id="2338a-105">Now it is time to explore possible ways to implement the domain model by using .NET Core (plain C\# code) and EF Core.</span></span> <span data-ttu-id="2338a-106">ドメイン モデルを構成するのは自分が書くコードだけであることにご注目ください。</span><span class="sxs-lookup"><span data-stu-id="2338a-106">Note that your domain model will be composed simply of your code.</span></span> <span data-ttu-id="2338a-107">EF Core モデルの要件があるだけで、EF に対する実際の依存関係は存在しません。</span><span class="sxs-lookup"><span data-stu-id="2338a-107">It will have just the EF Core model requirements, but not real dependencies on EF.</span></span> <span data-ttu-id="2338a-108">ドメイン モデルには EF Core または他の ORM への緊密な依存関係や参照を含めないでください。</span><span class="sxs-lookup"><span data-stu-id="2338a-108">You should not have hard dependencies or references to EF Core or any other ORM in your domain model.</span></span>

## <a name="domain-model-structure-in-a-custom-net-standard-library"></a><span data-ttu-id="2338a-109">カスタム .NET Standard ライブラリのドメイン モデル構造</span><span class="sxs-lookup"><span data-stu-id="2338a-109">Domain model structure in a custom .NET Standard Library</span></span>

<span data-ttu-id="2338a-110">eShopOnContainers 参照アプリケーションに使用されているフォルダー編成は、このアプリケーションの DDD モデルを示しています。</span><span class="sxs-lookup"><span data-stu-id="2338a-110">The folder organization used for the eShopOnContainers reference application demonstrates the DDD model for the application.</span></span> <span data-ttu-id="2338a-111">アプリケーションによっては、別のフォルダー編成の方が、選択する設計をより明確に表現できる場合もあります。</span><span class="sxs-lookup"><span data-stu-id="2338a-111">You might find that a different folder organization more clearly communicates the design choices made for your application.</span></span> <span data-ttu-id="2338a-112">図 7-10 に示されているように、注文ドメイン モデルには、注文集約と購入者集約という 2 つの集約があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-112">As you can see in Figure 7-10, in the ordering domain model there are two aggregates, the order aggregate and the buyer aggregate.</span></span> <span data-ttu-id="2338a-113">それぞれの集約はドメイン エンティティと値オブジェクトから成るグループですが、単一のドメイン エンティティ (集約ルートまたはルート エンティティ) で集約を構成することもできます。</span><span class="sxs-lookup"><span data-stu-id="2338a-113">Each aggregate is a group of domain entities and value objects, although you could have an aggregate composed of a single domain entity (the aggregate root or root entity) as well.</span></span>

![<span data-ttu-id="2338a-114">BuyerAggregate および OrderAggregate フォルダーが含まれる AggregatesModel フォルダーが示された Ordering.Domain プロジェクトのソリューション エクスプローラーの表示。それぞれのフォルダーにそのエンティティ クラス、値オブジェクト ファイルなどが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2338a-114">The Solution Explorer view for the Ordering.Domain project, showing the AggregatesModel folder containing the BuyerAggregate and OrderAggregate folders, each one containing it's entity classes, value object files and so on.</span></span> ](./media/image11.png)

<span data-ttu-id="2338a-115">**図 7-10**。</span><span class="sxs-lookup"><span data-stu-id="2338a-115">**Figure 7-10**.</span></span> <span data-ttu-id="2338a-116">eShopOnContainers の注文マイクロサービスのドメイン モデル構造</span><span class="sxs-lookup"><span data-stu-id="2338a-116">Domain model structure for the ordering microservice in eShopOnContainers</span></span>

<span data-ttu-id="2338a-117">また、このドメイン モデル レイヤーには、ドメイン モデルのインフラストラクチャ要件ともなっているリポジトリ コントラクト (インターフェイス) も含まれています。</span><span class="sxs-lookup"><span data-stu-id="2338a-117">Additionally, the domain model layer includes the repository contracts (interfaces) that are the infrastructure requirements of your domain model.</span></span> <span data-ttu-id="2338a-118">つまり、これらのインターフェイスによって、インフラストラクチャ レイヤーで実装しなければならないリポジトリとメソッドが示されます。</span><span class="sxs-lookup"><span data-stu-id="2338a-118">In other words, these interfaces express what repositories and the methods the infrastructure layer must implement.</span></span> <span data-ttu-id="2338a-119">リポジトリの実装はドメイン モデル レイヤーの外側、インフラストラクチャ レイヤー ライブラリ内に配置することが重要です。そのようにすることによって、ドメイン モデル レイヤーは、Entity Framework などのインフラストラクチャ テクノロジの API やクラスに "汚染" されずに済みます。</span><span class="sxs-lookup"><span data-stu-id="2338a-119">It is critical that the implementation of the repositories be placed outside of the domain model layer, in the infrastructure layer library, so the domain model layer is not “contaminated” by API or classes from infrastructure technologies, like Entity Framework.</span></span>

<span data-ttu-id="2338a-120">また、[SeedWork](https://martinfowler.com/bliki/Seedwork.html) フォルダーも参照できます。このフォルダーには、ドメイン エンティティと値オブジェクトの基礎として使用できるカスタム基底クラスが含まれるため、各ドメインのオブジェクト クラスで冗長コードをなくすことができます。</span><span class="sxs-lookup"><span data-stu-id="2338a-120">You can also see a [SeedWork](https://martinfowler.com/bliki/Seedwork.html) folder that contains custom base classes that you can use as a base for your domain entities and value objects, so you do not have redundant code in each domain’s object class.</span></span>

## <a name="structure-aggregates-in-a-custom-net-standard-library"></a><span data-ttu-id="2338a-121">カスタム .NET Standard ライブラリでの構造の集約</span><span class="sxs-lookup"><span data-stu-id="2338a-121">Structure aggregates in a custom .NET Standard library</span></span>

<span data-ttu-id="2338a-122">集約とは、トランザクションの整合性を保つためにグループ化される、ドメイン オブジェクトのクラスターのことです。</span><span class="sxs-lookup"><span data-stu-id="2338a-122">An aggregate refers to a cluster of domain objects grouped together to match transactional consistency.</span></span> <span data-ttu-id="2338a-123">こうしたオブジェクトは、(集約ルートやルート エンティティなどの) エンティティ インスタンスに、追加の値オブジェクトを付加したものとなる場合があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-123">Those objects could be instances of entities (one of which is the aggregate root or root entity) plus any additional value objects.</span></span>

<span data-ttu-id="2338a-124">トランザクションの整合性とは、ビジネス アクションの最後の時点で集約が一貫した状態になり、なおかつ最新状態になることを保証することです。</span><span class="sxs-lookup"><span data-stu-id="2338a-124">Transactional consistency means that an aggregate is guaranteed to be consistent and up to date at the end of a business action.</span></span> <span data-ttu-id="2338a-125">たとえば、eShopOnContainers 注文マイクロサービス ドメイン モデルの注文集約は、図 7-11 のように構成されています。</span><span class="sxs-lookup"><span data-stu-id="2338a-125">For example, the order aggregate from the eShopOnContainers ordering microservice domain model is composed as shown in Figure 7-11.</span></span>

![OrderAggregate フォルダーの詳細な表示:Address.cs は値オブジェクト、IOrderRepository はリポジトリ インターフェイス、Order.cs は集約ルート、OrderItem.cs は子エンティティ、OrderStatus.cs は列挙クラス。](./media/image12.png)

<span data-ttu-id="2338a-127">**図 7-11**。</span><span class="sxs-lookup"><span data-stu-id="2338a-127">**Figure 7-11**.</span></span> <span data-ttu-id="2338a-128">Visual Studio ソリューションにおける注文集約</span><span class="sxs-lookup"><span data-stu-id="2338a-128">The order aggregate in Visual Studio solution</span></span>

<span data-ttu-id="2338a-129">集約フォルダーのいずれかのファイルを開くと、[SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) フォルダーで実装されるエンティティまたは値オブジェクトの場合と同じように、それがカスタム基底クラスとインターフェイスのいずれかとしてマークされている様子を確認できます。</span><span class="sxs-lookup"><span data-stu-id="2338a-129">If you open any of the files in an aggregate folder, you can see how it is marked as either a custom base class or interface, like entity or value object, as implemented in the [SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) folder.</span></span>

## <a name="implement-domain-entities-as-poco-classes"></a><span data-ttu-id="2338a-130">POCO クラスとしてドメイン エンティティを実装する</span><span class="sxs-lookup"><span data-stu-id="2338a-130">Implement domain entities as POCO classes</span></span>

<span data-ttu-id="2338a-131">ドメイン エンティティを実装する POCO クラスを作成して、.NET でドメイン モデルを実装します。</span><span class="sxs-lookup"><span data-stu-id="2338a-131">You implement a domain model in .NET by creating POCO classes that implement your domain entities.</span></span> <span data-ttu-id="2338a-132">次の例では、Order クラスがエンティティとして、さらには集約ルートとしても定義されています。</span><span class="sxs-lookup"><span data-stu-id="2338a-132">In the following example, the Order class is defined as an entity and also as an aggregate root.</span></span> <span data-ttu-id="2338a-133">Order クラスは Entity 基底クラスから派生しているため、エンティティに関連する共通コードを再利用できます。</span><span class="sxs-lookup"><span data-stu-id="2338a-133">Because the Order class derives from the Entity base class, it can reuse common code related to entities.</span></span> <span data-ttu-id="2338a-134">これらの基底クラスとインターフェイスはドメイン モデル プロジェクト内で自分で定義するため、EF などの ORM のインフラストラクチャ コードではなくユーザー コードです。</span><span class="sxs-lookup"><span data-stu-id="2338a-134">Bear in mind that these base classes and interfaces are defined by you in the domain model project, so it is your code, not infrastructure code from an ORM like EF.</span></span>

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE 2.0
// Entity is a custom base class with the ID
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId;

    public OrderStatus OrderStatus { get; private set; }
    private int _orderStatusId;

    private string _description;
    private int? _paymentMethodId;

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    public Order(string userId, Address address, int cardTypeId, string cardNumber, string cardSecurityNumber,
            string cardHolderName, DateTime cardExpiration, int? buyerId = null, int? paymentMethodId = null)
    {
        _orderItems = new List<OrderItem>();
        _buyerId = buyerId;
        _paymentMethodId = paymentMethodId;
        _orderStatusId = OrderStatus.Submitted.Id;
        _orderDate = DateTime.UtcNow;
        Address = address;

        // ...Additional code ...
    }

    public void AddOrderItem(int productId, string productName,
                            decimal unitPrice, decimal discount,
                            string pictureUrl, int units = 1)
    {
        //...
        // Domain rules/logic for adding the OrderItem to the order
        // ...

        var orderItem = new OrderItem(productId, productName, unitPrice, discount, pictureUrl, units);

        _orderItems.Add(orderItem);

    }
    // ...
    // Additional methods with domain rules/logic related to the Order aggregate
    // ...
}
```

<span data-ttu-id="2338a-135">重要なこととして、これが POCO クラスとして実装されるドメイン エンティティである点にご注意ください。</span><span class="sxs-lookup"><span data-stu-id="2338a-135">It is important to note that this is a domain entity implemented as a POCO class.</span></span> <span data-ttu-id="2338a-136">Entity Framework Core または他のインフラストラクチャ フレームワークへの直接の依存関係はありません。</span><span class="sxs-lookup"><span data-stu-id="2338a-136">It does not have any direct dependency on Entity Framework Core or any other infrastructure framework.</span></span> <span data-ttu-id="2338a-137">この実装は、DDD の場合と同様に、ドメイン モデルを実装する C\# コードに過ぎません。</span><span class="sxs-lookup"><span data-stu-id="2338a-137">This implementation is as it should be in DDD, just C\# code implementing a domain model.</span></span>

<span data-ttu-id="2338a-138">また、このクラスは IAggregateRoot というインターフェイスで修飾されます。</span><span class="sxs-lookup"><span data-stu-id="2338a-138">In addition, the class is decorated with an interface named IAggregateRoot.</span></span> <span data-ttu-id="2338a-139">このインターフェイスは、このエンティティ クラスが集約ルートでもあることを示すためだけに使用される空のインターフェイスであり、*マーカー インターフェイス*と呼ばれることもあります。</span><span class="sxs-lookup"><span data-stu-id="2338a-139">That interface is an empty interface, sometimes called a *marker interface*, that is used just to indicate that this entity class is also an aggregate root.</span></span>

<span data-ttu-id="2338a-140">マーカー インターフェイスはアンチ パターンと見なされることもありますが、このインターフェイスが進化していく可能性がある場合には特に、クラスをマークするためのわかりやすい方法ともなります。</span><span class="sxs-lookup"><span data-stu-id="2338a-140">A marker interface is sometimes considered as an anti-pattern; however, it is also a clean way to mark a class, especially when that interface might be evolving.</span></span> <span data-ttu-id="2338a-141">属性はマーカーのもう 1 つの選択肢ですが、クラスの上に集約属性マーカーを配置するよりも、IAggregate インターフェイスの隣に基底クラス (Entity) を配置する方がより迅速に確認できます。</span><span class="sxs-lookup"><span data-stu-id="2338a-141">An attribute could be the other choice for the marker, but it is quicker to see the base class (Entity) next to the IAggregate interface instead of putting an Aggregate attribute marker above the class.</span></span> <span data-ttu-id="2338a-142">いずれにせよ、これは好みの問題です。</span><span class="sxs-lookup"><span data-stu-id="2338a-142">It is a matter of preferences, in any case.</span></span>

<span data-ttu-id="2338a-143">集約ルートを設けるということは、その集約のエンティティの整合性やビジネス ルールに関連するコードのほとんどを注文集約ルート クラスでメソッドとして実装する必要があることを意味します (たとえば、OrderItem オブジェクトを集約に追加する場合の AddOrderItem など)。</span><span class="sxs-lookup"><span data-stu-id="2338a-143">Having an aggregate root means that most of the code related to consistency and business rules of the aggregate’s entities should be implemented as methods in the Order aggregate root class (for example, AddOrderItem when adding an OrderItem object to the aggregate).</span></span> <span data-ttu-id="2338a-144">独立的にであれ、直接的にであれ、OrderItems オブジェクトを作成も更新もしないでください。AggregateRoot クラスが、その子エンティティに対するすべての更新操作を制御し、整合性を保持する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-144">You should not create or update OrderItems objects independently or directly; the AggregateRoot class must keep control and consistency of any update operation against its child entities.</span></span>

## <a name="encapsulate-data-in-the-domain-entities"></a><span data-ttu-id="2338a-145">ドメイン エンティティのデータをカプセル化する</span><span class="sxs-lookup"><span data-stu-id="2338a-145">Encapsulate data in the Domain Entities</span></span>

<span data-ttu-id="2338a-146">エンティティ モデルでよくある問題として、コレクションのナビゲーション プロパティが一般にアクセス可能なリスト型として公開されることがあります。</span><span class="sxs-lookup"><span data-stu-id="2338a-146">A common problem in entity models is that they expose collection navigation properties as publicly accessible list types.</span></span> <span data-ttu-id="2338a-147">そうすると、コラボレーターの開発者が誰でもこれらのコレクション型のコンテンツを操作できるようになり、結果として、コレクションに関連する重要なビジネス ルールがバイパスされ、オブジェクトが無効な状態のままになる可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-147">This allows any collaborator developer to manipulate the contents of these collection types, which may bypass important business rules related to the collection, possibly leaving the object in an invalid state.</span></span> <span data-ttu-id="2338a-148">これに対する解決策は、関連するコレクションへの読み取り専用アクセスを公開することと、クライアントがこれらのコレクションを操作する方法を定義したメソッドを明示的に提供することです。</span><span class="sxs-lookup"><span data-stu-id="2338a-148">The solution to this is to expose read-only access to related collections and explicitly provide methods that define ways in which clients can manipulate them.</span></span>

<span data-ttu-id="2338a-149">前のコードでは、多くの属性が読み取り専用かプライベートであり、クラス メソッドによってでなければ更新できないことにご注意ください。そのため、更新ではどれもビジネス ドメインの不変条件と、クラス メソッドに指定されたロジックが考慮されます。</span><span class="sxs-lookup"><span data-stu-id="2338a-149">In the previous code, note that many attributes are read-only or private and are only updatable by the class methods, so any update considers business domain invariants and logic specified within the class methods.</span></span>

<span data-ttu-id="2338a-150">たとえば、DDD パターンに従い、コマンド ハンドラー メソッドやアプリケーション レイヤー クラスから**以下を実行すべきでは *ありません*** (実際には、そのようにできないようになっています)。</span><span class="sxs-lookup"><span data-stu-id="2338a-150">For example, following DDD patterns, **you should *not* do the following** from any command handler method or application layer class (actually, it should be impossible for you to do so):</span></span>

```csharp
// WRONG ACCORDING TO DDD PATTERNS – CODE AT THE APPLICATION LAYER OR
// COMMAND HANDLERS
// Code in command handler methods or Web API controllers
//... (WRONG) Some code with business logic out of the domain classes ...
OrderItem myNewOrderItem = new OrderItem(orderId, productId, productName,
    pictureUrl, unitPrice, discount, units);

//... (WRONG) Accessing the OrderItems collection directly from the application layer // or command handlers
myOrder.OrderItems.Add(myNewOrderItem);
//...
```

<span data-ttu-id="2338a-151">この場合、Add メソッドは、単に OrderItems コレクションに直接アクセスしてデータを追加する操作に過ぎません。</span><span class="sxs-lookup"><span data-stu-id="2338a-151">In this case, the Add method is purely an operation to add data, with direct access to the OrderItems collection.</span></span> <span data-ttu-id="2338a-152">そのため、子エンティティに対するその操作に関連するドメイン ロジック、ルール、検証の大部分は、アプリケーション レイヤー (コマンド ハンドラーと Web API コントローラー) が担当します。</span><span class="sxs-lookup"><span data-stu-id="2338a-152">Therefore, most of the domain logic, rules, or validations related to that operation with the child entities will be spread across the application layer (command handlers and Web API controllers).</span></span>

<span data-ttu-id="2338a-153">集約ルートで処理する場合、集約ルートでは不変条件、有効性、整合性を保証できません。</span><span class="sxs-lookup"><span data-stu-id="2338a-153">If you go around the aggregate root, the aggregate root cannot guarantee its invariants, its validity, or its consistency.</span></span> <span data-ttu-id="2338a-154">結果として、解読困難なコードやトランザクション スクリプト コードとなります。</span><span class="sxs-lookup"><span data-stu-id="2338a-154">Eventually you will have spaghetti code or transactional script code.</span></span>

<span data-ttu-id="2338a-155">DDD パターンに従うには、エンティティのどのエンティティ プロパティにもパブリック セッターがあってはなりません。</span><span class="sxs-lookup"><span data-stu-id="2338a-155">To follow DDD patterns, entities must not have public setters in any entity property.</span></span> <span data-ttu-id="2338a-156">エンティティ内の変更は、エンティティ内で実行する変更に関する明示的なユビキタス言語を使った、明示的なメソッドにより駆動する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-156">Changes in an entity should be driven by explicit methods with explicit ubiquitous language about the change they are performing in the entity.</span></span>

<span data-ttu-id="2338a-157">また、(注文項目などの) エンティティ内のコレクションは読み取り専用プロパティでなければなりません (AsReadOnly メソッドについて後ほど説明します)。</span><span class="sxs-lookup"><span data-stu-id="2338a-157">Furthermore, collections within the entity (like the order items) should be read-only properties (the AsReadOnly method explained later).</span></span> <span data-ttu-id="2338a-158">集約ルート クラス メソッドか子エンティティ メソッドの中からでなければ、それを更新できないようになっている必要があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-158">You should be able to update it only from within the aggregate root class methods or the child entity methods.</span></span>

<span data-ttu-id="2338a-159">Order 集約ルートのコードに示されているように、すべてのセッターはプライベートにするか、少なくとも外部的に読み取り専用とする必要があります。こうして、エンティティのデータまたはその子エンティティに対するあらゆる操作がエンティティ クラスのメソッドをとおして実行されなければならなくなります。</span><span class="sxs-lookup"><span data-stu-id="2338a-159">As you can see in the code for the Order aggregate root, all setters should be private or at least read-only externally, so that any operation against the entity’s data or its child entities has to be performed through methods in the entity class.</span></span> <span data-ttu-id="2338a-160">その結果、整合性は制御された、オブジェクト指向の方法で確保され、トランザクション スクリプト コードを実装する必要はなくなります。</span><span class="sxs-lookup"><span data-stu-id="2338a-160">This maintains consistency in a controlled and object-oriented way instead of implementing transactional script code.</span></span>

<span data-ttu-id="2338a-161">次のコード スニペットは、OrderItem オブジェクトを Order 集約に追加するタスクをコーディングするための適切な方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="2338a-161">The following code snippet shows the proper way to code the task of adding an OrderItem object to the Order aggregate.</span></span>

```csharp
// RIGHT ACCORDING TO DDD--CODE AT THE APPLICATION LAYER OR COMMAND HANDLERS
// The code in command handlers or WebAPI controllers, related only to application stuff
// There is NO code here related to OrderItem object’s business logic
myOrder.AddOrderItem(productId, productName, pictureUrl, unitPrice, discount, units);

// The code related to OrderItem params validations or domain rules should
// be WITHIN the AddOrderItem method.

//...
```

<span data-ttu-id="2338a-162">このスニペットでは、OrderItem オブジェクトの作成に関連するほとんどの検証またはロジックが Order 集約ルートの AddOrderItem メソッドの制御下に置かれます。集約の他の要素に関連する検証とロジックは特にそう言えます。</span><span class="sxs-lookup"><span data-stu-id="2338a-162">In this snippet, most of the validations or logic related to the creation of an OrderItem object will be under the control of the Order aggregate root—in the AddOrderItem method—especially validations and logic related to other elements in the aggregate.</span></span> <span data-ttu-id="2338a-163">たとえば、AddOrderItem に対して複数の呼び出しを実行した結果として、同じ製品項目を取得できます。</span><span class="sxs-lookup"><span data-stu-id="2338a-163">For instance, you might get the same product item as the result of multiple calls to AddOrderItem.</span></span> <span data-ttu-id="2338a-164">そのメソッドでは、製品項目を検証し、同じ製品項目を、いくつかの単位で構成される単一の OrderItem オブジェクトに統合できます。</span><span class="sxs-lookup"><span data-stu-id="2338a-164">In that method, you could examine the product items and consolidate the same product items into a single OrderItem object with several units.</span></span> <span data-ttu-id="2338a-165">さらに、複数の割引額があるものの製品 ID が同じである場合、大きい方の割り引きを適用したいと思うかもしれません。</span><span class="sxs-lookup"><span data-stu-id="2338a-165">Additionally, if there are different discount amounts but the product ID is the same, you would likely apply the higher discount.</span></span> <span data-ttu-id="2338a-166">この原則を、OrderItem オブジェクトの他のドメイン ロジックに適用します。</span><span class="sxs-lookup"><span data-stu-id="2338a-166">This principle applies to any other domain logic for the OrderItem object.</span></span>

<span data-ttu-id="2338a-167">さらに、新しい OrderItem(params) 操作も、Order 集約ルートの AddOrderItem メソッドにより制御され、実行されます。</span><span class="sxs-lookup"><span data-stu-id="2338a-167">In addition, the new OrderItem(params) operation will also be controlled and performed by the AddOrderItem method from the Order aggregate root.</span></span> <span data-ttu-id="2338a-168">そのため、この操作に関連するほとんどのロジックまたは検証 (特に、他の子エンティティ間の整合性に影響を与えるあらゆるもの) は、集約ルート内の 1 か所に配置されます。</span><span class="sxs-lookup"><span data-stu-id="2338a-168">Therefore, most of the logic or validations related to that operation (especially anything that impacts the consistency between other child entities) will be in a single place within the aggregate root.</span></span> <span data-ttu-id="2338a-169">これが、集約ルート パターンの最終目的です。</span><span class="sxs-lookup"><span data-stu-id="2338a-169">That is the ultimate purpose of the aggregate root pattern.</span></span>

<span data-ttu-id="2338a-170">Entity Framework Core 1.1 以降を使用する場合、DDD エンティティをより優れた方法で表すことができます。プロパティに加えて、[フィールドへのマッピング](https://docs.microsoft.com/ef/core/modeling/backing-field)が行えるためです。</span><span class="sxs-lookup"><span data-stu-id="2338a-170">When you use Entity Framework Core 1.1 or later, a DDD entity can be better expressed because it allows [mapping to fields](https://docs.microsoft.com/ef/core/modeling/backing-field) in addition to properties.</span></span> <span data-ttu-id="2338a-171">これは、子エンティティや値オブジェクトのコレクションを保護する場合に役立ちます。</span><span class="sxs-lookup"><span data-stu-id="2338a-171">This is useful when protecting collections of child entities or value objects.</span></span> <span data-ttu-id="2338a-172">この機能拡張によって、プロパティではなく簡単なプライベート フィールドを使用できます。また、パブリック メソッドでフィールド コレクションへの更新を実装し、AsReadOnly メソッドを使用して読み取り専用アクセスを提供できます。</span><span class="sxs-lookup"><span data-stu-id="2338a-172">With this enhancement, you can use simple private fields instead of properties and you can implement any update to the field collection in public methods and provide read-only access through the AsReadOnly method.</span></span>

<span data-ttu-id="2338a-173">DDD では、エンティティのメソッド (またはコンストラクター) だけをとおしてエンティティを更新して、不変条件とデータの整合性を制御し、こうしてプロパティが get アクセサーによってのみ定義されることが望まれます。</span><span class="sxs-lookup"><span data-stu-id="2338a-173">In DDD you want to update the entity only through methods in the entity (or the constructor) in order to control any invariant and the consistency of the data, so properties are defined only with a get accessor.</span></span> <span data-ttu-id="2338a-174">プロパティは、プライベート フィールドによってサポートされます。</span><span class="sxs-lookup"><span data-stu-id="2338a-174">The properties are backed by private fields.</span></span> <span data-ttu-id="2338a-175">プライベート メンバーには、クラス内からのみアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="2338a-175">Private members can only be accessed from within the class.</span></span> <span data-ttu-id="2338a-176">ただし、1 つ例外があり、EF Core ではこれらのフィールドも設定する必要があります (適切な値でオブジェクトを返せるようにするため)。</span><span class="sxs-lookup"><span data-stu-id="2338a-176">However, there one exception: EF Core needs to set these fields as well (so it can return the object with the proper values).</span></span>

### <a name="map-properties-with-only-get-accessors-to-the-fields-in-the-database-table"></a><span data-ttu-id="2338a-177">get アクセサーのみでプロパティをデータベース テーブル内のフィールドにマッピングする</span><span class="sxs-lookup"><span data-stu-id="2338a-177">Map properties with only get accessors to the fields in the database table</span></span>

<span data-ttu-id="2338a-178">プロパティをデータベース テーブル列にマッピングすることはドメインの責任ではなく、インフラストラクチャと永続レイヤーの役割です。</span><span class="sxs-lookup"><span data-stu-id="2338a-178">Mapping properties to database table columns is not a domain responsibility but part of the infrastructure and persistence layer.</span></span> <span data-ttu-id="2338a-179">ここでこれに言及するのは、エンティティをモデル化する方法に関係する EF Core 1.1 以降の新機能をお知らせするために過ぎません。</span><span class="sxs-lookup"><span data-stu-id="2338a-179">We mention this here just so you are aware of the new capabilities in EF Core 1.1 or later related to how you can model entities.</span></span> <span data-ttu-id="2338a-180">このトピックに関するその他の詳細情報については、インフラストラクチャと永続化に関するセクションで説明します。</span><span class="sxs-lookup"><span data-stu-id="2338a-180">Additional details on this topic are explained in the infrastructure and persistence section.</span></span>

<span data-ttu-id="2338a-181">EF Core 1.0 以降を使用する場合、DbContext で、ゲッターによってのみ定義されるプロパティを、データベース テーブルの実際のフィールドにマップする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2338a-181">When you use EF Core 1.0 or later, within the DbContext you need to map the properties that are defined only with getters to the actual fields in the database table.</span></span> <span data-ttu-id="2338a-182">これは、PropertyBuilder クラスの HasField メソッドを使用して行います。</span><span class="sxs-lookup"><span data-stu-id="2338a-182">This is done with the HasField method of the PropertyBuilder class.</span></span>

### <a name="map-fields-without-properties"></a><span data-ttu-id="2338a-183">プロパティを使用しないでフィールドをマッピングする</span><span class="sxs-lookup"><span data-stu-id="2338a-183">Map fields without properties</span></span>

<span data-ttu-id="2338a-184">EF Core 1.1 以降の機能で列をフィールドにマップすれば、プロパティを使用しないで済ませることもできます。</span><span class="sxs-lookup"><span data-stu-id="2338a-184">With the feature in EF Core 1.1 or later to map columns to fields, it is also possible to not use properties.</span></span> <span data-ttu-id="2338a-185">代わりに、テーブルからフィールドに列をマップするだけです。</span><span class="sxs-lookup"><span data-stu-id="2338a-185">Instead, you can just map columns from a table to fields.</span></span> <span data-ttu-id="2338a-186">この方法の一般的なユース ケースとしては、エンティティの外部からアクセスする必要のない、内部状態用のプライベート フィールドがあります。</span><span class="sxs-lookup"><span data-stu-id="2338a-186">A common use case for this is private fields for an internal state that does not need to be accessed from outside the entity.</span></span>

<span data-ttu-id="2338a-187">たとえば、前述の OrderAggregate コード例の場合、`_paymentMethodId` フィールドなどのいくつかのプライベート フィールドがあり、これにはセッターやゲッターに関連するプロパティがあません。</span><span class="sxs-lookup"><span data-stu-id="2338a-187">For example, in the preceding OrderAggregate code example, there are several private fields, like the  `_paymentMethodId` field, that have no related property for either a setter or getter.</span></span> <span data-ttu-id="2338a-188">このフィールドは注文のビジネス ロジック内で計算でき、注文のメソッドから使用できますが、データベース内に保存することも必要です。</span><span class="sxs-lookup"><span data-stu-id="2338a-188">That field could also be calculated within the order’s business logic and used from the order’s methods, but it needs to be persisted in the database as well.</span></span> <span data-ttu-id="2338a-189">そのため EF Core (v1.1 以降) では、関連プロパティを使用しないでデータベース内の列にフィールドをマップする方法が備わっています。</span><span class="sxs-lookup"><span data-stu-id="2338a-189">So in EF Core (since v1.1) there is a way to map a field without a related property to a column in the database.</span></span> <span data-ttu-id="2338a-190">これは、このガイドの[インフラストラクチャ レイヤー](ddd-oriented-microservice.md#the-infrastructure-layer)に関するセクションでも説明されています。</span><span class="sxs-lookup"><span data-stu-id="2338a-190">This is also explained in the [Infrastructure layer](ddd-oriented-microservice.md#the-infrastructure-layer) section of this guide.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2338a-191">その他の技術情報</span><span class="sxs-lookup"><span data-stu-id="2338a-191">Additional resources</span></span>

- <span data-ttu-id="2338a-192">**Vaughn Vernon。DDD および Entity Framework による集約のモデル化。**</span><span class="sxs-lookup"><span data-stu-id="2338a-192">**Vaughn Vernon. Modeling Aggregates with DDD and Entity Framework.**</span></span> <span data-ttu-id="2338a-193">これは、Entity Framework Core では*ない*ことにご注意ください。</span><span class="sxs-lookup"><span data-stu-id="2338a-193">Note that this is *not* Entity Framework Core.</span></span> \
  <https://kalele.io/blog-posts/modeling-aggregates-with-ddd-and-entity-framework/>

- <span data-ttu-id="2338a-194">**Julie Lerman。データ ポイント - ドメイン駆動設計のコーディング:データを重視する開発者のためのヒント** </span><span class="sxs-lookup"><span data-stu-id="2338a-194">**Julie Lerman. Data Points - Coding for Domain-Driven Design: Tips for Data-Focused Devs** </span></span>\
  <https://msdn.microsoft.com/magazine/dn342868.aspx>

- <span data-ttu-id="2338a-195">**Udi Dahan。完全にカプセル化されたドメイン モデルを作成する方法** </span><span class="sxs-lookup"><span data-stu-id="2338a-195">**Udi Dahan. How to create fully encapsulated Domain Models** </span></span>\
  <http://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

> [!div class="step-by-step"]
> <span data-ttu-id="2338a-196">[前へ](microservice-domain-model.md)
> [次へ](seedwork-domain-model-base-classes-interfaces.md)</span><span class="sxs-lookup"><span data-stu-id="2338a-196">[Previous](microservice-domain-model.md)
[Next](seedwork-domain-model-base-classes-interfaces.md)</span></span>