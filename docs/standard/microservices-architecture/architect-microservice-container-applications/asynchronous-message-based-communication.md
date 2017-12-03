---
title: "基于消息的异步通信"
description: "为容器化的.NET 应用程序的.NET 微服务体系结构 |基于消息的异步通信"
keywords: "Docker, 微服务, ASP.NET, 容器"
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 05/26/2017
ms.prod: .net-core
ms.technology: dotnet-docker
ms.topic: article
ms.openlocfilehash: df39771295d12e122edbe27e91cd899e3318e301
ms.sourcegitcommit: c2e216692ef7576a213ae16af2377cd98d1a67fa
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2017
---
# <a name="asynchronous-message-based-communication"></a><span data-ttu-id="dbf7d-104">基于消息的异步通信</span><span class="sxs-lookup"><span data-stu-id="dbf7d-104">Asynchronous message-based communication</span></span>

<span data-ttu-id="dbf7d-105">异步消息传送和事件驱动的通信，都是关键，这是在跨多个微服务和其相关的域模型中传播更改时。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-105">Asynchronous messaging and event-driven communication are critical when propagating changes across multiple microservices and their related domain models.</span></span> <span data-ttu-id="dbf7d-106">如前面所述的讨论微服务和绑定的上下文 (BCs)，（用户、 客户、 产品、 帐户等） 的模型不同的含义到不同的微服务或 BCs。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-106">As mentioned earlier in the discussion microservices and Bounded Contexts (BCs), models (User, Customer, Product, Account, etc.) can mean different things to different microservices or BCs.</span></span> <span data-ttu-id="dbf7d-107">这意味着未发生更改，你需要某种方式来协调跨不同的模型更改。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-107">That means that when changes occur, you need some way to reconcile changes across the different models.</span></span> <span data-ttu-id="dbf7d-108">解决方案是最终一致性和基于异步消息传送的事件驱动的通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-108">A solution is eventual consistency and event-driven communication based on asynchronous messaging.</span></span>

<span data-ttu-id="dbf7d-109">在使用消息传递时，进程进行通信通过以异步方式交换消息。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-109">When using messaging, processes communicate by exchanging messages asynchronously.</span></span> <span data-ttu-id="dbf7d-110">客户端向服务发出的命令或请求通过将其发送一条消息。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-110">A client makes a command or a request to a service by sending it a message.</span></span> <span data-ttu-id="dbf7d-111">如果服务需要进行答复，它将不同的消息发送回客户端。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-111">If the service needs to reply, it sends a different message back to the client.</span></span> <span data-ttu-id="dbf7d-112">由于这是基于消息的通信，客户端会假设，将立即，不会收到答复和，可能有无响应根本。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-112">Since it is a message-based communication, the client assumes that the reply will not be received immediately, and that there might be no response at all.</span></span>

<span data-ttu-id="dbf7d-113">一条消息由标头 （如标识或安全信息的元数据） 和正文撰写。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-113">A message is composed by a header (metadata such as identification or security information) and a body.</span></span> <span data-ttu-id="dbf7d-114">通常会通过异步如 AMQP 协议发送消息。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-114">Messages are usually sent through asynchronous protocols like AMQP.</span></span>

<span data-ttu-id="dbf7d-115">这种微服务社区中类型的首选基础结构是通信的轻量的消息代理，这不同于大型中转站和 orchestrators 在 SOA 中使用。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-115">The preferred infrastructure for this type of communication in the microservices community is a lightweight message broker, which is different than the large brokers and orchestrators used in SOA.</span></span> <span data-ttu-id="dbf7d-116">轻量的消息代理，在基础结构是通常"笨拙，"仅充当消息代理，使用简单的实现，如 RabbitMQ 或在类似 Azure Service Bus 云的可缩放的服务总线。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-116">In a lightweight message broker, the infrastructure is typically “dumb,” acting only as a message broker, with simple implementations such as RabbitMQ or a scalable service bus in the cloud like Azure Service Bus.</span></span> <span data-ttu-id="dbf7d-117">在此方案中，大部分"智能"考虑仍驻留在生成和使用的消息的终结点-即，在微服务。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-117">In this scenario, most of the “smart” thinking still lives in the endpoints that are producing and consuming messages—that is, in the microservices.</span></span>

<span data-ttu-id="dbf7d-118">应尝试照搬，尽最大可能的另一个规则是使用仅异步消息传送之间的内部服务，并为使用同步通信 （如 HTTP) 只能从客户端应用程序的前端服务 （API 网关以及第一个级别微服务）。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-118">Another rule you should try to follow, as much as possible, is to use only asynchronous messaging between the internal services, and to use synchronous communication (such as HTTP) only from the client apps to the front-end services (API Gateways plus the first level of microservices).</span></span>

<span data-ttu-id="dbf7d-119">有两种类型的异步消息传送的通信： 单一接收方基于消息的通信和多个接收方基于消息的通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-119">There are two kinds of asynchronous messaging communication: single receiver message-based communication, and multiple receivers message-based communication.</span></span> <span data-ttu-id="dbf7d-120">下列部分中，我们提供它们的详细信息。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-120">In the following sections we provide details about them.</span></span>

## <a name="single-receiver-message-based-communication"></a><span data-ttu-id="dbf7d-121">单-接收方基于消息的通信</span><span class="sxs-lookup"><span data-stu-id="dbf7d-121">Single-receiver message-based communication</span></span> 

<span data-ttu-id="dbf7d-122">基于消息的异步通信单一接收方意味着将一条消息传递给恰好有一个使用者，从通道中进行读取和消息处理只需一次的点到点通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-122">Message-based asynchronous communication with a single receiver means there is point-to-point communication that delivers a message to exactly one of the consumers that is reading from the channel, and that the message is processed just once.</span></span> <span data-ttu-id="dbf7d-123">但是，有一些特殊的情形。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-123">However, there are special situations.</span></span> <span data-ttu-id="dbf7d-124">例如，在尝试自动从故障中恢复的云系统，同一消息可能多次发送。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-124">For instance, in a cloud system that tries to automatically recover from failures, the same message could be sent multiple times.</span></span> <span data-ttu-id="dbf7d-125">由于网络或其他故障，客户端必须能够重试发送消息和服务器都必须实现一个操作是幂等，以便只需一次处理特定的消息。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-125">Due to network or other failures, the client has to be able to retry sending messages, and the server has to implement an operation to be idempotent in order to process a particular message just once.</span></span>

<span data-ttu-id="dbf7d-126">单-接收方基于消息的通信尤其适合从一个微服务的异步命令发送到另一个在图 4-18，演示了此方法所示。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-126">Single-receiver message-based communication is especially well suited for sending asynchronous commands from one microservice to another as shown in Figure 4-18 that illustrates this approach.</span></span>

<span data-ttu-id="dbf7d-127">一旦开始发送基于消息的通信 （不管是使用命令或事件），应避免混合与同步 HTTP 通信的基于消息的通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-127">Once you start sending message-based communication (either with commands or events), you should avoid mixing message-based communication with synchronous HTTP communication.</span></span>

![](./media/image18.PNG)

<span data-ttu-id="dbf7d-128">**图 4-18**。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-128">**Figure 4-18**.</span></span> <span data-ttu-id="dbf7d-129">接收异步消息单个微服务</span><span class="sxs-lookup"><span data-stu-id="dbf7d-129">A single microservice receiving an asynchronous message</span></span>

<span data-ttu-id="dbf7d-130">请注意，当命令来自客户端应用程序，它们可以作为实现 HTTP 同步命令。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-130">Note that when the commands come from client applications, they can be implemented as HTTP synchronous commands.</span></span> <span data-ttu-id="dbf7d-131">当您需要更高版本的可伸缩性，或者已处于基于消息的业务流程中时，应使用基于消息的命令。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-131">You should use message-based commands when you need higher scalability or when you are already in a message-based business process.</span></span>

## <a name="multiple-receivers-message-based-communication"></a><span data-ttu-id="dbf7d-132">多个接收方基于消息的通信</span><span class="sxs-lookup"><span data-stu-id="dbf7d-132">Multiple-receivers message-based communication</span></span> 

<span data-ttu-id="dbf7d-133">为更灵活的方法，你可能还想要使用发布/订阅机制，从而使您从发件人的通信将其他订阅服务器微服务或外部应用程序可用。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-133">As a more flexible approach, you might also want to use a publish/subscribe mechanism so that your communication from the sender will be available to additional subscriber microservices or to external applications.</span></span> <span data-ttu-id="dbf7d-134">因此，它可帮助你按照[打开/关闭原则](https://en.wikipedia.org/wiki/Open/closed_principle)发送服务中。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-134">Thus, it helps you to follow the [open/closed principle](https://en.wikipedia.org/wiki/Open/closed_principle) in the sending service.</span></span> <span data-ttu-id="dbf7d-135">这样一来，而无需修改发件人服务将来，可以添加其他订阅服务器。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-135">That way, additional subscribers can be added in the future without the need to modify the sender service.</span></span>

<span data-ttu-id="dbf7d-136">当你使用的发布/订阅的通信时，你可能使用到任何订阅服务器的发布事件的事件总线接口。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-136">When you use a publish/subscribe communication, you might be using an event bus interface to publish events to any subscriber.</span></span>

## <a name="asynchronous-event-driven-communication"></a><span data-ttu-id="dbf7d-137">事件驱动的异步通信</span><span class="sxs-lookup"><span data-stu-id="dbf7d-137">Asynchronous event-driven communication</span></span>

<span data-ttu-id="dbf7d-138">在使用事件驱动的异步通信时，microservice 发布集成事件时出现在其域内，另一个微服务需要注意的如产品目录微服务的价格更改。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-138">When using asynchronous event-driven communication, a microservice publishes an integration event when something happens within its domain and another microservice needs to be aware of it, like a price change in a product catalog microservice.</span></span> <span data-ttu-id="dbf7d-139">其他微服务订阅的事件，以便它们可以以异步方式接收它们。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-139">Additional microservices subscribe to the events so they can receive them asynchronously.</span></span> <span data-ttu-id="dbf7d-140">在这种情况下，接收方可能会更新其自己的域实体，这可能会导致多个集成事件要发布。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-140">When that happens, the receivers might update their own domain entities, which can cause more integration events to be published.</span></span> <span data-ttu-id="dbf7d-141">通过使用的事件总线实现通常执行此发布/订阅系统。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-141">This publish/subscribe system is usually performed by using an implementation of an event bus.</span></span> <span data-ttu-id="dbf7d-142">事件总线可以设计为抽象或接口，使用 API 所需订阅或取消订阅事件以及发布事件。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-142">The event bus can be designed as an abstraction or interface, with the API that is needed to subscribe or unsubscribe to events and to publish events.</span></span> <span data-ttu-id="dbf7d-143">事件总线还可以将一个或多个实现基于任何进程间和消息传送的 broker，类似于消息队列或服务总线支持异步通信和发布/订阅模型。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-143">The event bus can also have one or more implementations based on any inter-process and messaging broker, like a messaging queue or service bus that supports asynchronous communication and a publish/subscribe model.</span></span>

<span data-ttu-id="dbf7d-144">如果系统使用由集成事件驱动的最终一致性，建议，此方法将完全清除给最终用户。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-144">If a system uses eventual consistency driven by integration events, it is recommended that this approach be made completely clear to the end user.</span></span> <span data-ttu-id="dbf7d-145">系统不应使用一种方法，以模拟集成事件，例如 SignalR 或从客户端的轮询系统。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-145">The system should not use an approach that mimics integration events, like SignalR or polling systems from the client.</span></span> <span data-ttu-id="dbf7d-146">最终用户和业务所有者必须显式接受系统中的最终一致性并请注意，在许多情况下业务没有使用此方法时，任何问题，只要它是显式。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-146">The end user and the business owner have to explicitly embrace eventual consistency in the system and realize that in many cases the business does not have any problem with this approach, as long as it is explicit.</span></span>

<span data-ttu-id="dbf7d-147">如上文中所述[的挑战和解决方案分布式数据管理](#challenges-and-solutions-for-distributed-data-management)部分中，你可以使用集成事件来实现跨多个微服务的业务任务。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-147">As noted earlier in the [Challenges and solutions for distributed data management](#challenges-and-solutions-for-distributed-data-management) section, you can use integration events to implement business tasks that span multiple microservices.</span></span> <span data-ttu-id="dbf7d-148">这样你便可以将这些服务之间的最终一致性。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-148">Thus you will have eventual consistency between those services.</span></span> <span data-ttu-id="dbf7d-149">最终一致的事务是分布式的操作的集合组成。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-149">An eventually consistent transaction is made up of a collection of distributed actions.</span></span> <span data-ttu-id="dbf7d-150">在每个操作相关的 microservice 更新域实体，并将发布引发同样的端到端业务任务中的下一步操作的另一个集成事件。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-150">At each action, the related microservice updates a domain entity and publishes another integration event that raises the next action within the same end-to-end business task.</span></span>

<span data-ttu-id="dbf7d-151">重要的一点是，你可能想要与同一事件订阅的多个微服务通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-151">An important point is that you might want to communicate to multiple microservices that are subscribed to the same event.</span></span> <span data-ttu-id="dbf7d-152">若要执行此操作，你可以使用发布/订阅消息传送基于事件驱动的通信，在图 4-19 中所示。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-152">To do so, you can use publish/subscribe messaging based on event-driven communication, as shown in Figure 4-19.</span></span> <span data-ttu-id="dbf7d-153">此发布/订阅机制不专用于微服务体系结构。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-153">This publish/subscribe mechanism is not exclusive to the microservice architecture.</span></span> <span data-ttu-id="dbf7d-154">它是类似的方式[绑定上下文](http://martinfowler.com/bliki/BoundedContext.html)中应传达 DDD，或将从写入数据库中的读取数据库的更新传播的方式[命令和查询责任分离 (CQRS)](http://martinfowler.com/bliki/CQRS.html)体系结构模式。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-154">It is similar to the way [Bounded Contexts](http://martinfowler.com/bliki/BoundedContext.html) in DDD should communicate, or to the way you propagate updates from the write database to the read database in the [Command and Query Responsibility Segregation (CQRS)](http://martinfowler.com/bliki/CQRS.html) architecture pattern.</span></span> <span data-ttu-id="dbf7d-155">目标是能够跨分布式系统的多个数据源之间的最终一致性。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-155">The goal is to have eventual consistency between multiple data sources across your distributed system.</span></span>

![](./media/image19.png)

<span data-ttu-id="dbf7d-156">**图 4-19**。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-156">**Figure 4-19**.</span></span> <span data-ttu-id="dbf7d-157">事件驱动的异步消息通信</span><span class="sxs-lookup"><span data-stu-id="dbf7d-157">Asynchronous event-driven message communication</span></span>

<span data-ttu-id="dbf7d-158">您的实现将确定什么协议用于事件驱动的、 基于消息的通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-158">Your implementation will determine what protocol to use for event-driven, message-based communications.</span></span> <span data-ttu-id="dbf7d-159">[AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol)可帮助实现可靠的排队的通信。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-159">[AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) can help achieve reliable queued communication.</span></span>

<span data-ttu-id="dbf7d-160">当你使用的事件总线时，你可能想要使用基于与使用从消息代理等 API 的代码的相关类中实现的抽象层 （如事件总线接口） [RabbitMQ](https://www.rabbitmq.com/)或如servicebus[主题使用的 azure 服务总线](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-160">When you use an event bus, you might want to use an abstraction level (like an event bus interface) based on a related implementation in classes with code using the API from a message broker like [RabbitMQ](https://www.rabbitmq.com/) or a service bus like [Azure Service Bus with Topics](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions).</span></span> <span data-ttu-id="dbf7d-161">或者，你可能想要使用更高级别的服务总线如 NServiceBus、 MassTransit 或 Brighter 清楚地表述事件总线以及发布/订阅系统。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-161">Alternatively, you might want to use a higher-level service bus like NServiceBus, MassTransit, or Brighter to articulate your event bus and publish/subscribe system.</span></span>

## <a name="a-note-about-messaging-technologies-for-production-systems"></a><span data-ttu-id="dbf7d-162">有关消息传送技术用于生产系统的注意事项</span><span class="sxs-lookup"><span data-stu-id="dbf7d-162">A note about messaging technologies for production systems</span></span>

<span data-ttu-id="dbf7d-163">在不同级别均可用于实现抽象事件总线消息传送技术。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-163">The messaging technologies available for implementing your abstract event bus are at different levels.</span></span> <span data-ttu-id="dbf7d-164">例如，产品，如 RabbitMQ （消息的 broker 传输） 和 Azure Service Bus 坐在较低级别比其他产品，如、 NServiceBus、 MassTransit 或 Brighter，之上 RabbitMQ 和 Azure 服务总线可以工作。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-164">For instance, products like RabbitMQ (a messaging broker transport) and Azure Service Bus sit at a lower level than other products like, NServiceBus, MassTransit, or Brighter, which can work on top of RabbitMQ and Azure Service Bus.</span></span> <span data-ttu-id="dbf7d-165">你的选择取决于在应用程序级别和你的应用程序需要的全新的可伸缩性的多少丰富功能。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-165">Your choice depends on how many rich features at the application level and out-of-the-box scalability you need for your application.</span></span> <span data-ttu-id="dbf7d-166">对于实现只是为开发环境的概念证明事件总线，因为我们已完成在 eShopOnContainers 示例中，在运行 Docker 容器上的 RabbitMQ 之上的简单实现可能不够。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-166">For implementing just a proof-of-concept event bus for your development environment, as we have done in the eShopOnContainers sample, a simple implementation on top of RabbitMQ running on a Docker container might be enough.</span></span>

<span data-ttu-id="dbf7d-167">但是，对于执行关键任务的并且需要超可伸缩性的生产系统，你可能想要评估 Azure Service Bus。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-167">However, for mission-critical and production systems that need hyper-scalability, you might want to evaluate Azure Service Bus.</span></span> <span data-ttu-id="dbf7d-168">有关高级抽象和简化的分布式应用程序开发的功能，我们建议你评估其他商业和开放源代码服务总线，如 NServiceBus、 MassTransit 和 Brighter。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-168">For high-level abstractions and features that make the development of distributed applications easier, we recommend that you evaluate other commercial and open-source service buses, such as NServiceBus, MassTransit, and Brighter.</span></span> <span data-ttu-id="dbf7d-169">当然，你可以构建自己在 RabbitMQ 和 Docker 等较低级别技术之上的服务总线功能。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-169">Of course, you can build your own service-bus features on top of lower-level technologies like RabbitMQ and Docker.</span></span> <span data-ttu-id="dbf7d-170">但该联结工作可能成本太多的自定义的企业应用程序。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-170">But that plumbing work might cost too much for a custom enterprise application.</span></span>

## <a name="resiliently-publishing-to-the-event-bus"></a><span data-ttu-id="dbf7d-171">弹性发布到事件总线</span><span class="sxs-lookup"><span data-stu-id="dbf7d-171">Resiliently publishing to the event bus</span></span>

<span data-ttu-id="dbf7d-172">在跨多个微服务实现事件驱动的体系结构挑战是如何在恢复到某种程度上基于的事件总线发布其相关的集成事件时，以原子方式更新中原始微服务状态事务。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-172">A challenge when implementing an event-driven architecture across multiple microservices is how to atomically update state in the original microservice while resiliently publishing its related integration event into the event bus, somehow based on transactions.</span></span> <span data-ttu-id="dbf7d-173">如下几种方法可以实现此目的，尽管可能有其他方法以及。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-173">The following are a few ways to accomplish this, although there could be additional approaches as well.</span></span>

-   <span data-ttu-id="dbf7d-174">使用像 MSMQ 这样的事务 （基于 DTC） 队列。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-174">Using a transactional (DTC-based) queue like MSMQ.</span></span> <span data-ttu-id="dbf7d-175">（但是，这是旧方法）。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-175">(However, this is a legacy approach.)</span></span>

-   <span data-ttu-id="dbf7d-176">使用[事务日志挖掘](http://www.scoop.it/t/sql-server-transaction-log-mining)。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-176">Using [transaction log mining](http://www.scoop.it/t/sql-server-transaction-log-mining).</span></span>

-   <span data-ttu-id="dbf7d-177">使用完整[事件来源](https://msdn.microsoft.com/en-us/library/dn589792.aspx)模式。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-177">Using full [Event Sourcing](https://msdn.microsoft.com/en-us/library/dn589792.aspx) pattern.</span></span>

-   <span data-ttu-id="dbf7d-178">使用[发件箱模式](http://gistlabs.com/2014/05/the-outbox/)： 事务的数据库表作为消息队列将会创建事件，并将其发布的活动创建者组件的基。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-178">Using the [Outbox pattern](http://gistlabs.com/2014/05/the-outbox/): a transactional database table as a message queue that will be the base for an event-creator component that would create the event and publish it.</span></span>

<span data-ttu-id="dbf7d-179">使用异步通信时要考虑的其他主题是消息幂等性和消息重复数据删除。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-179">Additional topics to consider when using asynchronous communication are message idempotence and message deduplication.</span></span> <span data-ttu-id="dbf7d-180">这些主题涵盖在部分[实现基于事件的微服务 （集成事件） 之间的通信](#implementing_event_based_comms_microserv)本指南中更高版本。</span><span class="sxs-lookup"><span data-stu-id="dbf7d-180">These topics are covered in the section [Implementing event-based communication between microservices (integration events)](#implementing_event_based_comms_microserv) later in this guide.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="dbf7d-181">其他资源</span><span class="sxs-lookup"><span data-stu-id="dbf7d-181">Additional resources</span></span>

-   <span data-ttu-id="dbf7d-182">**事件驱动的消息传送**
    [*http://soapatterns.org/design\_模式/事件\_驱动\_消息传送*](http://soapatterns.org/design_patterns/event_driven_messaging)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-182">**Event Driven Messaging**
[*http://soapatterns.org/design\_patterns/event\_driven\_messaging*](http://soapatterns.org/design_patterns/event_driven_messaging)</span></span>

-   <span data-ttu-id="dbf7d-183">**发布/订阅通道**
    [*http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html*](http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-183">**Publish/Subscribe Channel**
[*http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html*](http://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html)</span></span>

-   <span data-ttu-id="dbf7d-184">**Udi Dahan。阐明了 CQRS**
    [*http://udidahan.com/2009/12/09/clarified-cqrs/*](http://udidahan.com/2009/12/09/clarified-cqrs/)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-184">**Udi Dahan. Clarified CQRS**
[*http://udidahan.com/2009/12/09/clarified-cqrs/*](http://udidahan.com/2009/12/09/clarified-cqrs/)</span></span>

-   <span data-ttu-id="dbf7d-185">**命令和查询责任分离 (CQRS)**
    [*https://docs.microsoft.com/azure/architecture/patterns/cqrs*](https://docs.microsoft.com/azure/architecture/patterns/cqrs)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-185">**Command and Query Responsibility Segregation (CQRS)**
[*https://docs.microsoft.com/azure/architecture/patterns/cqrs*](https://docs.microsoft.com/azure/architecture/patterns/cqrs)</span></span>

-   <span data-ttu-id="dbf7d-186">**之间的通信绑定上下文**
    [*https://msdn.microsoft.com/library/jj591572.aspx*](https://msdn.microsoft.com/library/jj591572.aspx)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-186">**Communicating Between Bounded Contexts**
[*https://msdn.microsoft.com/library/jj591572.aspx*](https://msdn.microsoft.com/library/jj591572.aspx)</span></span>

-   <span data-ttu-id="dbf7d-187">**最终一致性**
    [*https://en.wikipedia.org/wiki/Eventual\_一致性*](https://en.wikipedia.org/wiki/Eventual_consistency)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-187">**Eventual consistency**
[*https://en.wikipedia.org/wiki/Eventual\_consistency*](https://en.wikipedia.org/wiki/Eventual_consistency)</span></span>

-   <span data-ttu-id="dbf7d-188">**Jimmy Bogard。重构入恢复能力： 评估耦合**
    [*https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/*](https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-188">**Jimmy Bogard. Refactoring Towards Resilience: Evaluating Coupling**
[*https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/*](https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/)</span></span>


>[!div class="step-by-step"]
<span data-ttu-id="dbf7d-189">[以前](通信-中的微服务-architecture.md) [下一步] (维护-microservice-apis.md)</span><span class="sxs-lookup"><span data-stu-id="dbf7d-189">[Previous] (communication-in-microservice-architecture.md) [Next] (maintain-microservice-apis.md)</span></span>