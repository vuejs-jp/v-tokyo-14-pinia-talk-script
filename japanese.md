# Everything Beyond State Management with Pinia

Every paragraph is dedicated to one slide.

[Online version of the slides](https://everything-beyond-state-management-with-pinia.netlify.app)

## Transcript

### 1.
Hi everyone my name is Eduardo, I’m a member of the Vue.js core team the author of the Vue Router and also a small library called Pinia, which is an alternative to Vuex designed around the composition API to deal with state management.

### 2.
But today, I don’t want to talk specifically about state management, I want to talk about what’s beyond that with Pinia. However, I can’t go beyond state management without talking about state management itself a little bit first.

### 3.
is it just an object that you mutate with a few functions state management? Some people will tell you it is, and to be fair, they wouldn’t be totally wrong…

### 5.
If you wrap that object with reactive we do have a valid state management that works seamlessly with Vue. And it isn’t that complex, isn’t it? The truth is, state management doesn’t have to be complex and can stay this simple for even bigger apps sometimes. There is this myth about state management that it should only be necessary with big or medium to big sized applications. This couldn't be further from the truth.

### 6.
State management is not about the size of your application or codebase. State management is about how you use the state across your application. Is your state outliving pages? Do you need the user information in many places? Do you need to the state of the shopping cart in multiple places in your complex nested tree of components? But today the question that I want to bring to your mind is what will make you go further and complexify your state management solution?

### 7.
Because this solution works out pretty well for most cases so you might be reluctant to go a bit more complex by using a store so going beyond state management is obviously starting by using a store and you don’t always need or want one so it’s important to note that the difference between a bare-bones state management solution and a store like Pinia isn’t that big.

### 8.
In other words, do you actually need a store or can you keep using your state management solution that just uses a reactive object.

### 9.
And the truth is there are a few reasons to use a store because these centralize object that we are that we can use doesn’t really scale that well if we keep adding properties to the object. The first reason to use a store is Server Side Rendering. You will have to it yourself if you are doing SSR in your application and it’s just not worth it. Store also gives you a centralized place to store all the different states that you may have notification without having scaling issues. He brings you a much better developer experience by enabling devtools, hot module replacement, and leveraging existing plug-ins made by other Developers. It also easily enables mocking to unit test components that rely on stores, so overall a robust and battle tested solution.

### 10.
So what if we want to move from a simple state management solution to a store like Pinia? What is even Pinia? Let’s take the simple example but you might have all come across, it’s usually your main.js or main.TS file in a fresh project.

### 13.
To start using Pinia you just need to create it and pass it to your application. That Pinia object is actually playing a central role in how state management works because it’s going to centralize all the different states that we’re going to have and it’s going to enable Devtools and SSR.

### 14.
That Pinia actually holds a state property that is just a ref of an empty object, or rather initially empty object.

### 16.
Because as soon as you start using your previously defined stores for the first time, it gets populated with whatever state you defined. For example, the first time you call useAuthStore, it will add an object with the initial state to pinia.state.value.auth. Any call to that function again will reuse that state and keep it alive for as long as the application stays open on your browser. And the same for any other useStore function you call.

### 17.
ストアの状態をすべてひとつのオブジェクトに集めるというシンプルなアイデアが、Piniaの拡張を容易にしていると思います。devtools、SSR、プラグインなどの統合を容易にしているのです。そしてそれは、Piniaが誕生した当初から存在しています。

### 18.
そしてそれはずいぶん前のことです。最初のPiniaのバージョンはVue 3の前に登場したので、Vue 2 + composition APIにしか対応していませんでした。当時は、VuexとVue Devtools 5の通信チャネルをハックして、既存のdevtoolsの恩恵を受け、実際のストアでの体験を提供していました。

### 19.
初期のバージョン1では、ほんの数ヶ月前に登場したeffectScope APIがなかったため、Piniaは内部のdevtools関数を呼び出し、useStoreを呼び出すたびに重複したゲッターとアクションを使用していました。また、ストアの機能を拡張するためのプラグインインターフェースもなく、ストアを定義する方法は、オプションAPIを使用することのみでした。要するに、明らかに実験的なものであり、本番環境には対応していませんでした。重要なのは、APIの形とそれを使った開発者の体験でした。TypeScriptをサポートし、すべてをSSRで動作させることに大きな重点が置かれました。

### 20.
この間に物事は大きく進展しました。Vue 2とVue 3の両方をサポートしているだけでなく、ストアを定義するための追加の構文があり、コンポーネントに近い形になっています。それは、すでにcompoistion APIを使っているなら、とてもとても馴染み深いものです。また、ストアに依存するコンポーネントをテストする際に、作業を容易にするテストモジュールも用意されています。Nuxt 2、Bridge、Nuxt 3をサポートするNuxtモジュール。そして最後に、完全に型付けできる非常に完成度の高いプラグインインターフェイスがあります。

### 21.
さらに、PiniaのAPIの核心部分は、Vuex 5のAPIに影響を与えており、RFCで確認することができます。つまり、PiniaとVuexの両方で、Storeの定義とその相互作用はほぼ同じです。これにより、必要に応じて、一方のバージョンから他方のバージョンへの切り替えが容易になります。

### 22.
実は、Vuex 4とPiniaの最大の変更点は、Vuex 4と5と同じです。Vuex 4では、createStoreでストアを作成し、そこにmutationを追加して状態を変異させることができるようになっています。

### 24.
Vuex 5とPiniaでは、ストアの状態を直接変異させることができます。冗長で不必要なミューテーションはもう必要ありません。しかし、Piniaで状態を変異させる方法はそれだけではありません。

### 25.
Piniaでは、`$patch`メソッドを使って、変更したい状態の部分的なバージョンを渡すこともできます。あるいは、状態を直接変異させる関数を渡すこともできます。

### 26.
これは、セット、マップ、配列などのコレクションを扱うときに便利です。また、状態の変更をグループ化することで、ストアへのサブスクリプションが一度だけトリガーされるようになります。配列で複数のプロパティを監視するのと似ています。

### 27.
これは、ストアの変更やストアのアクションを購読したり、作成されたすべてのストアに必要なプロパティを追加したりできるプラグインでは特に便利です。
pinia のプラグインは、現在実行中のアプリケーション、Pinia オブジェクト、プラグインが適用されるストア、ストアの定義に使用されたオプションを含むコンテキストオブジェクトを受け取る関数です。

### 28.
Piniaプラグインでは、主に3つのことを行います。状態の変化を購読する。これは、Vuexにもあることです。変更の種類、ストアのID、状態の変更方法に応じたペイロード、問題のストアの現在の状態にアクセスできます。
アクションが実行される前や後、失敗したとき、あるいはキャンセルしたときに、特別な処理を引き起こすことができます。
そして最後に、ストアに任意のプロパティを追加することができます。例えば、ストア内のプロパティを返すだけで、すべてのストアにルーターへの参照を追加することができます。そして、アクションやゲッター、またはstore.routerから直接アクセスすることができます。

### 31.
Here is a simple example of a plugin that sends errors happening in actions to an external service you might use like Sentry. These 4 lines of code is all you need to report back errors from your stores to detect bugs in production.

### 33.
And all of these things together is what makes Pinia easily extensible. In fact, the devtools interface for Pinia is handled with a plugin.

### 34.
Let's see all of this together with a Demo.

We have a Vite server running with a simple page. It's using a very simple counter store with a single property and HMR setup at the bottom. Which means I can add or remove properties and my app will update without reloading, keeping the application state intact.
In the devtools I can inspect my components using a store and modify the state as I want. I also have a timeline where I can see the different mutations happening to my store.
I'm going to add an action that is going to decrement the counter until it gets to zero. It's going to wait between each decrement a few milliseconds to make the action last a few seconds. So that's it, that's all the code we need. Now I'm going to add a button on my page to invoke that method. We even have autocompletion thanks to Volar, so that's pretty neat. What are we expecting to see here? We want to see how the decreasing of the counter happens periodically and can be inspected individually in the timeline. We are going going to clear the timeline and click the button. And as you can see, we have a dot every single time the action changes the state. On top of that, they all appear grouped in one long yellow frame because all these changes are happening inside the same action. In fact, if I call the increment mutation that I had from the beginning, new dots will appear but they won't get mixed with the existing running action because the mutation is happening outside of it. I can even have multiple actions running in parallel and their mutations don't get mixed up. And then, what is really nice is that we can inspect every action individually by selecting to see the events by group. For instance, the action that took the longest lasted for x ms and did x mutations. But if we inspect the shorter actions, we can see a different event count and duration. And every single dot allows us to inspect the exact state that changed with its previous and current value.
We can also go to the store inspector here and change anything from there. We can even do it while the store is running a decrease function and it will work seamlessly. Modifying the store directly will also not leave any noise in the timeline so you can correctly inspect the changes coming from your application and ignore the changes coming from debugging. I can also mutate multiple pieces of the state directly in the action. For instance, I can count the amount of times the counter gets decreased and if I try it out and check the timeline, I will see more mutations happening. But I can also use a `$patch()` function to group the mutations. If I do that, I will see the mutations grouped in the timeline as well.
One cool thing people often don't realize is they can actually pass any composable to the state. Any `ref` or `computed` work. So I can just pass a `useLocalStorage()` method from `@vueuse/core` to my `n` property and that will just work. I still need to reload the page in this scenario as I modified the nature of an existing property in the state. And then, if I modify my counter and reload the page, I will see its value gets restored from local storage. And everything else just works too, the timeline, the store inspector, etc.
So this is the option syntax of the store, it's pretty straightforward and should be understandable by any developer, even the first time they see a store. It has its limitations though, pretty much like the options API in Vue. So there is an alternative syntax that is pretty much the same as the `setup()` syntax that can be used.
I have this longer example using the NASA API to fetch the picture of the day and I don't have the time to go through the whole store code but as you can see we have one function instead of an object with properties. You create state by using `ref`, or `reactive`, getters by calling `computed` and actions by creating functions. We can also use composables. For instance, I have a cached API call, so if I check a few pictures, I can go back easily without waiting any time at all between pages. And something interesting is that if we go to the store inspector, you can see the new store appeared alongside the counter one. This is because in Pinia (and Vuex 5), all stores are dynamically registered, improving on performance and bundle size without you having to do anything at all.

### 36.
I want to finish by thanking all my sponsors, who bring me closer to working full-time on Open source. If you are a company using Vue.js, I have some interesting options for you. Give them a look on my Sponsor page.
I hope you will give Pinia a try if you haven't already and thank you a lot for your attention!

