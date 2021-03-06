Bad Testing Practices
##糟糕的测试
作者: [Luis Solano](https://twitter.com/luisobo)

I've been writing automated tests for a few years now, and I have to confess that it's a technique that still fascinates me when it comes down to making a code base more maintainable. In this article, I'd like to share some of my experiences, as well as the lessons I've learned either from others or via trial and error.

时至今日，虽然我写自动化测试也已经有些年头了。但我不得不承认当它使代码更容易维护的时候，我依然对这项技术着迷。本文中，我希望分享一些我的经验，以及我从他人和错误中吸取的教训。


After all these years, I've heard many good (and bad) reasons to write automated tests. On the positive side, writing automated tests can:

这些年，我听到了许多关于写自动化测试的好的(或者不好的)理由。从积极的方面来说，写自动化测试能够:

* Make refactoring easier
* Avoid regressions
* Provide executable specification and documentation
* Reduce the time of creating software
* Reduce the costs of creating software

* 使重构更简单
* 避免回归
* 提供了可执行的说明和文档
* 减少了创建软件的时间
* 降低了创建软件的代价


Sure, you could say those are true, but I want to give another perspective to all those reasons—a unified perspective if you will:

的确，你可以说这些都是对的，但是我想提出一个关于这些理由的另一个视角——一个统一的视角

> The only reason automated tests are valuable is to allow us to modify our own code later on.

> 自动化测试唯一有价值的理由是将来允许我们修改我们的代码。

In other words:
换句话说:

> The only time a test will give value back is when we want to change our code.

> 一个测试能够体现价值的时候仅仅是当我们想修改我们的代码的时候。

Let's see how the classic arguments in favor of writing tests are connected to this premise:

让我们看一看这个经典论断是如何支持我们前面提到的理由的:

* Makes refactoring easier—You can change implementation details with confidence, leaving the public API untouched.
* Avoids regressions—When do regressions occur? When you change your code.
* Provides executable specification and documentation—When do you want to know more about how software actually works? When you want to modify it.
* Reduces the time of creating software—How? By allowing you to modify your code faster, with the confidence that your tests will tell you when something went wrong.
* Reduces the cost of creating software—Well, time is money.

* 使重构更简单——你可以自信的修改实现细节，而不用去触及公有 API。
* 避免回归——回归在什么时候发生？在你修改代码的时候。
* 提供了可执行的说明和文档——你在什么时候更想知道软件实际上是如何工作的？在你想修改他们的时候
* 减少了创建软件的时间——怎么减少时间的？是通过更快速地修改你的代码，出错时测试会自信地告诉你哪里出错了
* 降低了创建软件的代价——好吧，时间就是金钱

Yes, all the reasons above are true at some point, but the reason that this pertains to us developers is that automated tests let us change stuff.

是的，上面所有的理由在某些方面是对的，但是这些理由适用于我们开发者的是自动化测试能够让我们修改代码。

Note that I'm not including the design feedback of writing tests, as in TDD. That could be a separate conversation. We will be talking about tests, once they are written.

注意，我没有包括 *写* 测试的设计反馈，比如TDD。那可以成为一个单独的话题。我们将要谈论的测试是已经写好的测试。

It seems then that writing tests and how to write them should be motivated by change.

看起来好像写测试和 *如何* 写测试应该以 *修改* 作为动机。

An easy way to take this fact into account when writing a test is to always ask your tests these two questions:

一个简单的考虑这个问题的方法是在写测试的时候，向你的测试提出下面两个问题:

"Are you going to fail (or pass) if I change my production code?"
"Is that a good reason for you to fail (or pass)?"

* “如果我修改了我的生产代码，测试是会失败(还是通过)呢?”
* “那是一个让测试失败(或者通过)的好的理由么?”

If you find a bad reason for your test to fail (or pass), fix it.
如果你发现那是一个让测试失败(或者通过)的不好的理由，那么请修正它。

That way, when you change your code later on down the road, your tests will pass or fail only for good reasons, thus giving a better return than flakey tests that fail for the wrong reason.

那样，将来你修改你的代码的时候，你的测试只会因为好的理由而通过或者失败，这会比因为不好的理由而失败的古怪的测试得到的回报要好。

Still, you may be wondering: "What's the big deal about that?"
现在，你可能仍然会问:“什么才是最重要的？”

Let's answer this question with another question: Why do our tests break when we change our code?
让我们用另一个问题来回答这个问题:当我们修改代码的时候，测试为什么会出错？

We agreed that the main purpose of having tests is so we can change our code with ease. If that's so, then how are all those red tests helping us? Those failing tests are nothing but noise—impediments to getting the job done. So, how do we do testing in a way that will help us?

我们都认同的一个观点是我们进行测试的主要原因是为了能够轻松地修改代码。如果是那样的话，那些失败的测试是如何帮助我们的？那些失败的测试除了是噪音之外什么也不是——阻碍着我们完成工作。那么，怎样测试才能帮助我们呢？

It depends on the reason why we are changing the code.
这取决于我们修改代码的理由。

Changing the Behavior of the Code
###修改代码的行为

The starting point should always be green, i.e. all tests should pass.
首先，起点必须是测试全部是绿色的，也就是说所有的测试都已经通过。

If you want to change your code to change its behavior (i.e. changing what your code does) you would:
如果你想通过修改代码来修改它们的行为(也就是，修改代码 *做的事情* )，你需要：

1. Find the test or tests that define the current behavior that you want to change.
2. Modify those tests to expect the new desired behavior.
3. Run the tests and see if those modified tests fail.
4. Update your code to make all tests pass again.

1. 找到定义当你想要修改的前行为的测试。
2. 修改这些测试来满足新的期望的行为。
3. 运行测试，查看那些被修改的测试是否失败了。
4. 更新你的代码，使得所有的测试重新通过。

At the end of this process, we are back at square one—all tests pass, and we are ready to start over if needed.
在这一过程的结尾，我们又回到了起点——所有的测试都通过了，如果需要，我们准备好了再次开始。

Because you know exactly which tests failed and which modification of the code made them pass, you are confident that you only changed what you wanted to change. This is how automated tests help us modify our code—in this case, to change the behavior.

因为你知道哪些测试失败了以及哪些代码的修改使得他们通过了，你会很有信心，因为你只修改了你想要修改的部分。
这就是自动化测试如何帮助我们通过修改代码来修改代码的行为的。

Note that it is OK to see a test failing, as long as it's only a test related to the behavior that we are updating.
注意，看到一个测试失败是正常的，因为它是我们正在更新的行为相对应的测试。


Refactoring: Changing the Implementation of the Code—Leaving the Behavior Intact
###重构：修改代码的实现——保持行为不变

Again, the starting point should always be green.
同样，起点应该是测试全是绿色的。

If all you want is to change the implementation of a piece of code to make it simpler, more performant, easy to extend, etc. (i.e. changing how your code does something but not what it does), this is the extensive list of steps to follow:

如果希望修改一段代码的实现让它变得更简单，高效，易于扩展等等(也就是说，修改 *怎么做*，而不是 *做什么* )，应该遵循接下来的步骤：

Modify your code without touching your tests at all.

1. 在不触及测试的前提下修改你的代码。

Once your code is simpler, faster, or more flexible, your tests should remain just as they were—green. When refactoring, tests should only fail in the case that you made a mistake, such as changing the external behavior of your code. When that happens, you should reverse that mistake and go back to a green state.

当修改后的代码已经简单、快速、更灵活时，你的测试应该仍然是绿色的。在重构的时候，测试应该只在代码出错的时候失败，例如修改了代码的外部行为。当发生这种情况时，你应该退回到那个错误然后回到绿色的状态
  
Because your tests stayed green the whole time, you know that you didn't break anything. That's how automated tests let us modify our code. 

因为你的测试总是在绿色的状态，你知道你没有破坏任何事情。这就是自动化测试如何让我们修改我们的代码的方式。
   
In this case, it is not OK to see a test failing. It could mean that:
在这种情况下，看到测试失败是不应该的。因为这意味着：
   
* We accidentally changed the external behavior of the code. Good, our tests are helping us.
* We didn't change the external behavior of the code. Bad, we got false negatives. This is where most of the trouble is.

* 我们无意识地修改了代码的外部行为。庆幸的是，我们的测试帮助了我们发现这些错误。
* 我们没有修改代码的外部行为。太不幸了，这才是最大的麻烦。
   
We want our tests to aid in the processes described above. So let's see some specific tips that will help make our tests more cooperative.
我希望测试在上面的情形下能够帮助我们。所以让我们来看一些具体的能让我们的测试更有效的 tips。


Good Practices 101
##优秀实践 101

Before jumping into how not to write your tests, I'd like to go over a few good practices really quick. There are five basic rules that every test should obey to be considered a good—or even a valid—test. There is a mnemonic for these five rules: F.I.R.S.T.

在讨论如何写测试之前(此处原文有个 not，不理解何意，不知道是不是原文出错)，我想迅速地回顾一些优秀实践。有 5 条被认为是每个测试都应该遵守的优良原则。便于记忆这5条规则的缩写是: F.I.R.S.T.

Tests should be:
测试应该：

* **F**ast—tests should be able to be executed often.
* **I**solated—tests on their own cannot depend on external factors or on the result of another test.
* **R**epeatable—tests should have the same result every time we run them.
* **S**elf-verifying—tests should include assertions; no human intervention needed.
* **T**imely—tests should be written along with the production code.

* 快速(**F**ast)——测试应该能够被反复执行。
* 隔离(**I**solated)——测试本身不能依赖于外部因素或者其他测试的结果。
* 可重复(**R**epeatable)——每次运行测试都应该产生相同的结果。
* 自检(**S**elf-verifying)——测试应该包括断言，不需要人为干预。
* 及时(**T**imely)——测试应该和生产代码一同书写。

To find out more about this set of rules, you can read this article by Tim Ottinger and Jeff Langr.
更多关于这些规则的内容，你可以阅读 Tim Ottinger 和 Jeff Langr 的[这篇文章](http://pragprog.com/magazines/2012-01/unit-tests-are-first)。


Bad Practices
##坏的实践

How do we maximize the outcome of our tests? In one sentence:
如何将测试的结果收益最大化？一言以蔽之:

> Don't couple your tests to implementation details.
>
> 不要将测试和实现细节耦合在一起


Don't Test Private Methods
###不要测试私有方法

Enough said.
[我说的已经够多了。](http://shoulditestprivatemethods.com/)

Private means private. Period. If you feel the need to test a private method, there is something conceptually wrong with that method. Usually it is doing too much to be a private method, which in turn violates the Single Responsibility Principle.
[私有方法意味着私有。如果你感到有必要测试一个私有方法，那么那个私有方法一定含有概念性错误，通常是作为私有方法，它做的太多了，](http://shoulditestprivatemethods.com/)  从而违背了[单一职责原则](http://www.objectmentor.com/resources/articles/srp.pdf)

Today: Your class has a private method. It does plenty of stuff, so you decide to test it. You make that method public, just for testing purposes, even though it makes no sense to use that method on its own, as it's only meant to be used internally by other public methods of the same class. You write tests for that private (now technically public) method.

今天: 假设你的类有一个私有方法。它做了太多的事情，所以你决定测试它。你仅仅为了测试，就让那个方法变成公有的。它被同一个类的其他的公有方法在内部使用。然后你为那些私有方法(现在使用技术手段让它们变成了公有)编写测试。

Tomorrow: You decide to modify what this method does, because of some change in the requirements (totally fine). You find out that some coworker is using that method from another class for something totally different because "it does what I need." After all, it was public, right? This private method is not part of the public API. You cannot modify this method without breaking your colleague's code.

明天: 因为需求上的一些变化(这完全是有可能的)，你决定修改这个方法。你发现一些同事在其他的类中使用了这个方法，因为他们说“这是我需要的方法”。毕竟，它是公有的，不是么？这个私有方法不是公有 API 的一部分。你要想修改这个方法就不得不修改你同事的代码。

What To Do: Extract that private method to a separate class, give that class a properly defined contract, and test it separately. When testing code that relies on this new class, you can provide a test double of that class if needed.

应该做什么: 将私有方法抽离到一个单独的类中，给这个类一个定义良好的合约，单独地测试它。当测试代码依赖这个新的类的时候，如果那个类有需要的话，你可以提供一个 [test double](http://en.wikipedia.org/wiki/Test_double)(译者注)。

So, how do I test the private methods of a given class? Via the public API of its class. Always test your code via its public API. The public API of your code defines a contract, which is a well-defined set of expectations about how your code is going to act based on different inputs. The private API (private methods or even entire classes) does not define that contract and it is subject to change without notice, so your tests (or your colleagues) cannot rely on them.

所以，我是如何测试一个类的私有方法的？通过这个类的公有 API。永远通过公有 API 测试你的代码。程序的公有 API 定义了一个合约，它是一组关于你的程序对应于不同输入时定义良好的一组期望。私有 API(私有方法或者整个类)并没有定义合约，并且可以不经通知自行修改，所以你的测试(或者你的同事)不能依赖于它们。

By testing your private methods in this way, you will remain free to change your (truly) private code and the design of your code will improve by having smaller classes that do one thing and are properly tested.

通过这种方法测试你的私有方法，你可以自由地修改(这是真的)你的私有代码，通过划分成小的类提升代码的设计。

Don't Stub Private Methods
###不要 Stub 私有方法

Stubbing private methods has all the same caveats as exposing a private method for testing, but on top of that, doing this could be hard to debug. Usually, stubbing libraries rely on hacks to get the job done, making it hard to find out why a test is failing.

Stubbing 私有方法和测试私有方法具有相同的危害，更重要的是，stub 私有方法将会使程序难以调试。通常，stubbing 库依赖于某项工作的完成，这使得发现一个测试为什么会失败变的困难。

Also, when we stub a method, we should be doing it according to its contract. But a private method has no specified contract—that's mostly why it's private, after all. Since the behavior of a private method is subject to change without notice, your stub may diverge from reality, but your test will still pass. Horrendous. Let's see an example:

同样，当我们 stub 一个方法的时候，我们必须依据它的合约(contract)。但是私有方法没有指定的合约——毕竟，这也是为什么它们是私有的。由于私有方法的行为可以不经通知自行修改，你的 stub 可能与实际情况背道而驰，但是你的测试 *仍然会通过* 。这是多么的可怕，让我们来看一个例子：

Today: The public method of a class relies on a private method of the same class. The private method foo never returns nil. Tests for the public method stub out the private method for convenience. When stubbing out method foo, you never consider making foo return nil, because it currently never happens.

今天: 一个类的公有方法依赖于该类的一个私有方法。这个私有方法 `foo` 永远不会返回空。为公有方法编写的测试为了方便起见，stub out 了私有方法。当 stub out `foo` 方法的时候，你永远不会考虑到 `foo` 返回为空的情况，因为那永远不会发生。

Tomorrow: The private method changes and now it returns nil. It's a private method, so that's fine. Tests of the public method are never updated accordingly ("I'm changing a private method, so why should I update any test at all?"). The public method is now broken for the case in which the private method returns nil, but the tests still pass!

明天: 这个私有方法被修改了，现在它返回空了。它是一个私有方法，所以这没什么问题。为公有方法编写的测试不会相应地被修改(“我正在修改一个私有方法，所以我为什么要更新我的测试？”)。公有方法现在在私有方法返回空的情况下会出错，但是测试仍然会通过！

Horrendous.
这实在太可怕了。

What To Do: Since foo is doing too much, extract that method to a new class and test it separately. Then, when testing bar, provide a test double for that new class.

应该做什么: 由于 `foo` 做的事情太多了，所以应该将它抽离至一个新的类，然后单独地测试它。然后，在测试的时候，为那个新类提供一个 [test double](http://en.wikipedia.org/wiki/Test_double)。

Don't Stub External Libraries
###不要 Stub 外部库

Third-party code should never be mentioned directly in your tests.
第三方代码不应该在你的测试中直接出现。

Today: Your networking code relies on the famous HTTP library LSNetworking. To avoid hitting the actual network (to make your tests fast and reliable), you stub out the method -[LSNetworking makeGETrequest:] of that library, properly replacing its behavior (it calls the success callback with a canned response) but without hitting the network.

今天: 你的网络部分的代码依赖于著名的 HTTP 库 `LSNetworking`.为了避免碰到实际的网络(为了让你的测试更快速更可信)，你 stub out 了那个库中的方法 `-[LSNetworking makeGETrequest:]`，没有通过实际的网络合适地替代了它的行为(它通过一个罐头响应调用了执行成功的回调(callback))。

Tomorrow: You need to swap LSNetworking with an alternative (it could be that LSNetworking is no longer maintained or that you need to switch to a more advanced library because it has that feature that you need, etc.). It's a refactor, so you should not change your tests. You replace the library. Your tests fail because the dependency of the network is no longer being stubbed (-[LSNetworking makeGETrequest:] is no longer being called by the implementation).

明天: 你需要使用一个替代品来取代 `LSNetworking`(可能是 `LSNetworking` 已经不再维护或者是你需要换成一个更先进的库，因为它有很多你需要的新特性等等)。这是一次重构，所以你不应该修改测试。你替换了库。你的测试会失败，因为依赖的网络没有被 stubbed(`-[LSNetworking makeGETrequest:]`不会被调用)。

What To Do: Rely on umbrella stubbing to replace the entire functionality of that library during tests.

应该做什么: 测试中，依靠 stubbing 伞(umbrella stubbing) 来替代那个库的全部功能。

Umbrella stubbing (a term that I just made up) consists of stubbing out all the possible ways that your code may use, now and in the future, to get some task done, via a declarative API, agnostic to any implementation detail.

stubbing 伞(一个我刚刚发明的术语)包括 stubbing out 所有你的代码可能用到的方式。

As in the example above, your code can rely on "HTTP library A" today, but there are other possible ways of making an HTTP request, right? Like "HTTP library B."
正如上面的那个例子，你的代码今天可能依赖于"HTTP 库 A"，但是还是有别的可能的方式发起 HTTP 请求，不是么？比如"HTTP 库 B"。

As an example, one solution that provides umbrella stubbing for networking code is my open-source project, Nocilla. With Nocilla, you can stub HTTP requests in a declarative fashion, without mentioning any HTTP library. Nocilla takes care of stubbing any HTTP library out there, so that you don't couple your tests to any implementation detail. This allows you to switch your networking stack without breaking your tests.

作为一个例子，一个为网络代码提供 stubbing 伞的解决方案是我的开源项目，[Nocilla](https://github.com/luisobo/Nocilla)。通过 [Nocilla](https://github.com/luisobo/Nocilla)你可以不依赖任何 HTTP 库，以声明的方式 stub HTTP 请求。 [Nocilla](https://github.com/luisobo/Nocilla)关心 stubbing 的任何一个 HTTP 库，所以你不会将测试和实现细节耦合在一起。这使得你能够在不修改测试的情况下切换网络栈。

Another example could be stubbing out dates. There are many ways of getting the current time in most programming languages, but libraries like TUDelorean take care of stubbing every single time-related API so that you can simulate a different system time for testing purposes without coupling your tests to any of those multiple time APIs. This lets you refactor your implementation to a different time API without breaking your tests.

另一个例子是 stubbing out 时间，在大多数编程语言中都有很多中方法获取当前时间，但是像 [TUDelorean](https://github.com/tuenti/TUDelorean) 这样的库关心 stubbing 每一个与时间相关的 API，所以你可以模仿一个不同的系统时间用来测试。这使得你不用修改测试就可以重构不同的时间 API 的实现细节。

In realms other than HTTP or dates, where a variety of APIs may be available, you can use similar solutions to do umbrella stubbing, or you can create your own open-source solution and share it with the community, so that the rest of us can properly write tests.

除了 HTTP 和时间，在拥有各种各样 API 的其他领域，你可以用类似的方式来 stubbing 伞，或者你可以创建你自己的开源解决方案并分享到社区，这样其他人就可以正确地编写测试了。

If You Stub Out a Dependency, Do It Properly
###如果你 Stub Out 了一个依赖，那么请正确地使用

This goes hand in hand with the previous point, but this problem is more common. Our production code usually relies on dependencies to get something done. For instance, a dependency could help us query a database. Often, these dependencies offer many ways of achieving the exact same thing or, at least, with the same external behavior; in our database example, you could use the find method to retrieve a record by ID, or use a where clause to get the same record. The problem comes when we only stub one of those possible mechanisms. If we only stub the find method—which is the one that our production code uses—but we don't stub the other possibilities, like the where clause, when we decide to refactor our implementation from using find to using where, our test will fail, even though the external behavior of our code hasn't changed.

这部分和前一点关系密切，但是这部分的情况更普遍。我们的生产代码通常依赖于某些事情的完成。比如，一个依赖能够帮助我们查询数据库。通常这些依赖提供了多种方法来实现相同的事情，或者说至少是相同的外部行为；在我们的数据库的例子中，你可以使用 `find` 方法通过 ID 来获取一条记录，或者使用 `where` 子句获取相同的记录。当我们仅仅 stub 可能的机制中的一个的时候，问题就出现了。如果我们仅仅 stub 了 `find` 方法(我们的生产代码使用的机制)，但是没有 stub 其他的可能性，比如 `where` 子句，当我们决定使用 `where` 子句取代 find 方法来重构我们的实现的时候，我们的测试就会失败，即使代码的外部行为并没有修改。

Today: The UsersControllerclass relies on the UsersRepositoryclass to retrieve users from the database. You are testing UsersController and you stub out the find method of the UsersRepository class to make your tests run faster and in a deterministic fashion, which is an awesome thing to do.

今天: `UsersController` 类依赖于 `UserRepository` 类从数据库中取得用户。你正在测试 `UsersController` 并且你为了以确定的方式更快地运行，你 stub out 了 `UsersRepository` 的 `find` 方法，这是一个可怕的事情。

Tomorrow: You decide to refactor UsersController to use the new query syntax of UsersRepository, which is more readable. Because it's a refactoring, you should not touch your tests. You update UsersController to use the more readable method where, in order to find the records of interest. Now your tests are broken because they stub the method find but not where.

明天: 你决定使用 `UsersRepository` 的新的可读性更高的查询语法来重构 `UsersController`，因为这是一次重构，所以不应该触及测试。为了找到感兴趣的记录，你使用了可读性更高的 `where` 方法更新了 `UsersController`。现在你的测试会失败，因为测试 stub 了 `find` 方法，但是没有 stub `where` 方法。

Umbrella stubbing can help here in some cases, but for our case with the UsersController class... well, there is no alternative library to fetch my users from my database.

stubbing 伞在某些情况下能帮上忙，但是对于 `UsersController` 类的这种情形，没有可选的库能够从 *我的* 数据库中获取 *我的* users。

What To Do: Create an alternative implementation of the same class for testing purposes and use it as a test double.

应该做什么：以测试为目的，为同一个类创建可替代的实现，并将它作为 [test double](http://en.wikipedia.org/wiki/Test_double)(译者加)。

To continue with our example, we should provide an InMemoryUsersRepository. This in-memory alternative should comply to every single aspect of the contract established by the original UsersRepository class, except that it stores data in memory to make our tests fast. This means that when you refactor UsersRepository, you do the same with its in-memory version. To make it very clear: yes, you now have to maintain two different implementations of the same class.

继续我们的例子，我们应该提供一个 `InMemoryUsersRepository`。这个在内存中的替代方案，除了它为提高测试速度而把数据保存在内存中之外，它应该遵守原始的 `UsersRepository` 类的每一个单一方面的合约。这意味着，当你重构 `UsersRepository` 的时候，你使用在内存中这个版本做了同样的事情。为了让它更清楚：是的，现在你不得不为同一个类维护两套不同的实现。

You can now provide this lightweight version of your dependency as a test double. The good thing is that it's a full implementation, so when you decide to move your implementation from one method to the other (from find to where, in our example) the test double being used will already support that new method and your tests won't fail when you refactor.

现在你可以为依赖提供这个轻量级的版本作为 test double。好的事情是这是一个完整的实现，所以当你决定将实现从一个方法移动到另一个方法(在我们的例子中是从 `find` 移动到 `where`)的时候，正在使用的 test double 将会支持新的方法，并且当重构的时候，测试也不会失败。

There is nothing wrong with maintaining another version of your class. In my experience, it ends up requiring very little effort, and it definitely pays off.

维护一个类的另一个版本没有什么问题。根据我的经验，它最终只会需要很少的努力，就能得到很大的回报。

You can also provide the lightweight version of your class as part of the production code, just like Core Data does with its in-memory version of the stack. Doing this could be of some use to someone.

你同样可以将类的轻量级版本作为生产代码的一部分，就像 Core Data 使用栈的内存版本一样。这样做可能对某些人有作用。

Don't Test Constructors
###不要测试构造函数

Constructors are implementation details by definition and, since we agreed that we should decouple our tests from implementation details, you should not test constructors.

构造函数定义的是实现细节，你不应该测试构造函数，这是因为我们认同测试应该与实现细节解藕这一观点。

Furthermore, constructors should have no behavior and, since we agree that we should only test the behavior of our code, there is nothing to test.

而且，构造函数不应该包含行为，所以没有值得测试的东西。这是因为我们认同应该只测试代码的行为这一观点。

Today: Your class, Car, has one constructor. You test that, once a Car is constructed, its Engine is not nil (because you know that the constructor creates a new Engine and assigns it to the variable _engine).

今天：你有一个 `Car` 类，并包含一个构造函数。一旦一个 `Car` 被创建了，你测试它的 `Engine` 不为空(因为你知道构造函数创建了一个新的 `Engine` 并将它赋给了变量 `_engine`)。

Tomorrow: The class Engine turns out to be costly to construct, so you decide to lazily initialize it the first time that the getter of Engine is called. (This is a totally fine thing to do.) Your test for the constructor of the Car class breaks because, upon construction, Car no longer has an Engine, even though the Car will work perfectly. Another option is that your test does not fail because testing that Car has an Engine triggers the lazy load initialization of the Engine. So my question is: What are you testing again?

明天：`Engine` 类创建起来变得代价很高，所以你决定使用延迟初始化(lazily initialize)，在第一次调用 `Engine` 的 `getter` 方法时才初始化 `Engine` (这是很好的)。 现在为 `Car` 类的构造函数编写的测试出问题了，即便 `Car` 类运行良好，但 `Car` 并没有包括 `Engine`。另一个可能是你的测试不会失败，因为测试包含 `Engine` 的 `Car` 类会触发 `Engine` 的延迟加载。所以我的问题是：为什么还要测试？

What To Do: Test how the public API of your class behaves when constructing it in different ways. A silly example: test how the method count of the class list behaves when the list is constructed with and without items. Just note that you are testing the behavior of count and not the behavior of the constructor.

应该做什么：当使用不同的方法创建类的时候测试公有 API 的行为。一个愚蠢的例子：测试当 `list`类被创建并且没有包含条目的时候，list 类的 `count` 方法的行为。注意，你测试的是 `count` 的行为而不是构造函数的行为。

In the case that your class has more than one constructor, consider it a smell. Your class may be doing too much. Try to split it in smaller classes, but if there is a legitimate reason for your class to have multiple constructors, just follow the same piece of advice. Make sure you test the public API of that class, constructing it in different ways. In this case, test it using every constructor (i.e. when this class is in this initial state, it behaves like this, and when it's in this other initial state, it behaves like that).

思考一下，类含有多个构造函数的情形。你的类可能做了太多事情了。试着将它们拆分成更小的类，但是如果有足够充分的理由使你的类含有多个构造函数，那么依然遵循同样的建议。保证你的测试的是那个类的公有 API。在这种情况下，使用每一个构造函数去测试(也就是说，当类处在一种初始化状态下时，它的行为就是那种状态下的；当类处在另一种初始化状态下时，它的行为就是另一种状态下的)

Conclusion
##结论

Writing tests is an investment—we need to put in time in the form of writing and maintaining them. The only way we can justify such an investment is because we expect to get that time back. Coupling tests to implementation details will reduce the amount of value that our tests will provide, making that investment less worthwhile, or even worthless in some cases.

编写测试是一项投资——我们需要花时间编写和维护它们。我们可以证明这种投资有回报的唯一方法就是我们期望节省时间。将实现细节和测试耦合在一起会减少测试带来的回报，使得那些投资变得不合算，甚至在某些情况下变得一文不值。

When writing tests, step back and ask yourself if those tests will maximize the outcome of your investment by checking if your tests could fail or pass for the wrong reason, either when refactoring, or when changing the behavior of the system.

在编写测试、重构以及修改系统行为的时候，检查你的测试在面对错误的原因时是失败还是通过，然后退一步问问自己，那些测试是否能够最大化你投资的成果。



-------
[更多关于话题15的文章](http://www.objc.io/issue-15/)




































