# Everything Beyond State Management with Pinia

Every paragraph is dedicated to one slide.

[Online version of the slides](https://everything-beyond-state-management-with-pinia.netlify.app)

## Transcript

### 1.
みなさんこんにちは。私の名前はEduardoです。Vue.jsコアチームの一員で、Vue Routerの作者でもあります。また、PiniaというVuexの代わりに使えるcomposition API向けのちょっとした状態管理ライブラリも作っています。

### 2.
しかし今日は、特に状態管理については取り上げるつもりはありません。Piniaのこれからについてお話しするつもりです。
ただし、最初に少し状態管理についての話をしなければその先に進められませんので少しお話ししようと思います。

### 3.
いくつかの関数の状態管理で変更するのは単なるオブジェクトでしょうか？それが正しいという人もいますし、そして彼らは完全に間違っているわけではありません…

### 5.
そのオブジェクトを`reactive`で囲むと、Vueとシームレスに連動する有効な状態管理が可能になります。そんなに複雑じゃありませんよね？実のところ、状態管理は複雑である必要はなく、うまくやれば、さらに大きなアプリケーションでもシンプルに保つことが可能です。世の中には、アプリケーションが中規模以上になると状態管理が必要になってくるという神話がありますが、これは事実とはまったく違います。

### 6.
状態管理はアプリケーションやコードベースの大きさによるものではありません。あなたが状態をアプリケーション全体でどのように使用するかによります。ページよりも長く状態が保持されていますか？たくさんの場所でユーザー情報が必要ですか？複雑にネストされたコンポーネントツリーの複数の場所からショッピングカートの状態が必要になりますか？
しかし今日、私が皆さんにぜひ考えていただきたい問いかけは、何があなたに状態管理ソリューションを前進させ、また何がそれを複雑にさせているのか、ということです。

### 7.
オブジェクトを使ったソリューションはほとんどのケースでうまくいくので、ストアを使ってもう少し複雑なことをするのは気が進まないかもしれません。
つまり、ここから状態管理を進化させるには当然ながらストアを使用することになりますが、実際のところストアが絶対に必要というわけではありません。
ここで重要なのは、素の状態管理ソリューションとPiniaのようなストアの違いはそれほど大きくないということです。

### 8.
言い換えれば、本当にストアが必要なのか、それともリアクティブなオブジェクトを使うだけの状態管理ソリューションを使い続けることができるのかということです。

### 9.
実際のところ、オブジェクトにプロパティを追加し続けると一元化されたオブジェクトは拡張性に欠けるために、ストアを使用する理由はいくつかあります。一つ目の理由は、サーバーサイドレンダリングです。アプリケーションでSSRを行う場合、状態管理を自前で行うことはまったく無駄な努力です。ストアを使えばスケーリングの問題なく通知されたすべての異なる状態を一括して保存する場所についても提供してくれます。またストアは、devtoolsやホットモジュールの交換を可能にし、他の開発者によって作られた既存のプラグインを活用することで、より優れた開発者体験を与えてくれます。さらにストアに依存するコンポーネントのユニットテストを行うためのモッキングも簡単に行うことができ、全体として堅牢かつ実用的なソリューションとなっています。

### 10.
では、単純な状態管理ソリューションからPiniaのようなストアに移行したい場合はどうでしょうか。まず、Piniaとは何でしょうか？
全ての人が出くわす簡単な例を見てみましょう。これは新しいプロジェクトのmain.jsまたはmain.TSファイルです。

### 13.
Piniaは、インスタンスを作成してアプリケーションに渡すだけで使い始めることができます。
そのPiniaオブジェクトは、状態管理がどのように機能するかにおいて実際に中心的な役割を果たしています。これは、これから行うさまざまな状態をすべて一元化し、DevtoolsとSSRを有効にするためです。

### 14.
そのPiniaは、実際には、空のオブジェクト、または最初は空のオブジェクトの単なる参照である状態プロパティを保持しています。

### 16.
以前に定義したストアを初めて使用し始めるとすぐに、定義した状態が入力されます。たとえば、初回に `useAuthStore` を呼び出すと、初期状態のオブジェクトが `pinia.state.value.auth` に追加されます。その関数を再度呼び出すと、その状態が再利用され、アプリケーションがブラウザで開いている限り、その状態が維持されます。また、呼び出す他の `useStore` 関数についても同じです。

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

