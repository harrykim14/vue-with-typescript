# Vue.js에 TypeScript를 적용해보자!

- 강의명 : [Typescript with Vue 실전 프로젝트](https://www.inflearn.com/course/Typescript_Vue)
- 수강기간 : 21. 04. 07 ~
- 수강목적

  1. 이미 Vue.js에 대한 기초를 배운 적이 있음([Vue.js 스터디용 레포지토리](https://github.com/harrykim14/DoitVue.js))
  2. 취업을 원하는 회사들의 기술 스택에 Vue.js가 있고, 이를 더 전문적으로 배운다면 즉전력으로 활약할 수 있을 것 같아서
  3. Vue.js를 복습함과 동시에 새로운 무엇인가를 배우고 싶었는데 마침 Typescript를 적용하는 법에 대한 강의가 있어 흥미가 동함

- 시작하기에 앞서

1. vue-cli를 설치하고 버전 확인하기

```
npm install -g @vue/cli
vue --version
>> @vue/cli 4.5.12
```

2. vue-cli로 프로젝트 생성하기
   - VS code에서 이미 프로젝트 폴더를 생성했기 때문에 .으로 현재 디렉토리에 설치함

```
vue create .

Vue CLI v4.5.12
? Generate project in current directory? Yes

Vue CLI v4.5.12
? Please pick a preset: Default ([Vue 2] babel, eslint)
? Pick the package manager to use when installing dependencies: NPM

Vue CLI v4.5.12
✨  Creating project in [/path/]
🗃  Initializing git repository...
⚙️  Installing CLI plugins. This might take a while...
```

<details>
<summary>3. vue-cli로 plug-in 설치하기</summary>
<div markdown="3">
   - Vue CLI v3.4.0 (강의에서 사용하는 버전) 과는 달리 `Check the features needed for your project:`라는 문구는 출력되지 않으며 vue add /plug-in name/으로 설치할 수 있다

```
vue add typescript
? Use class-style component syntax? Yes
? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? No
? Convert all .js files to .ts? Yes
? Allow .js files to be compiled? Yes
? Skip type checking of all declaration files (recommended for apps)? Yes (default)

vue add router
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes

vue add eslint

vue add vuex
```

- 강의에서 사용된 TSlint는 2019년 이후로 ESLint로 마이그레이션됨 [참고 자료](https://velog.io/@kyusung/eslint-tslint-config)

```
npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

- 루트 폴더에 .eslintrc 파일을 생성하고 설정을 작성하기

```
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "@typescript-eslint",
  ],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "no-console": 1,
    "no-loops/no-loops": 2
  }
}
```

</div>
</details>

4. 명령어 사용해보기

```
npm run serve
```

<image src="https://user-images.githubusercontent.com/67398691/113802120-1ac56f00-9795-11eb-8346-e090aa96111d.png" width="600" alt="vue.js index page"/>
(이미지1. Vue 프로젝트를 실행하여 localhost:8080으로 접속한 화면)

```
npm run build
/*
\  Building for production...Starting type checking service...
Using 1 worker with 2048MB memory limit
\  Building for production...

 DONE  Compiled successfully in 9962ms                                 오전 11:50:05
  File                                 Size                 Gzipped

  dist\js\chunk-vendors.ce698506.js    138.11 KiB           47.73 KiB
  dist\js\app.330dc3d5.js              6.25 KiB             2.38 KiB
  dist\js\about.d3f7cc1a.js            0.44 KiB             0.31 KiB
  dist\css\app.26e87896.css            0.42 KiB             0.26 KiB

  Images and other types of assets omitted.

 DONE  Build complete. The dist directory is ready to be deployed.
 INFO  Check out deployment instructions at https://cli.vuejs.org/guide/deployment.html
*/

npm run lint
 DONE  No lint errors found!
```

<details>
<summary>5. Class-Based 컴포넌트 만들기</summary>
<div markdown="5">
(1) @Component

```javascript
// 기존 JS 문법
Vue.component("App", {
  // ...
});
```

```typescript
// TS에서 컴포넌트 만드는 법
@Component
export default class App extends Vue {}
```

- 컴포넌트란? - [공식 문서](https://kr.vuejs.org/v2/guide/components.html#%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8-%EC%9E%91%EC%84%B1)
  - 컴포넌트는 부모-자식 관계에서 가장 일반적으로 함께 사용하기 위한 것이며 Vue.js에서 부모-자식 컴포넌트 관계는 props는 아래로, events 위로 라고 요약 할 수 있다.

```javascript
// 기존 JS에서 props 넘겨주기
Vue.component("child", {
  props: ["message"],
});
```

```typescript
// TS에서 props 정의하기
@Component
export default class Children extends Vue {
  @Prop() parentMessage!: string;
}
```

- 자식 객체의 prop을 동적으로 변경하기

```html
<template>
  <children :parentMessage="message"></children>
</template>
<script lang="ts">
  ...
  export default class Home extends Vue {
    message = "hello world";
  }
  ...
</script>
```

- Method Decorator : 해당 메서드가 객체에 정의되기 전에 추가적인 행위가 있은 후에 객체에 정의됨

```typescript
function enumerable(value: boolean) {
  return function (
    target: any,
    propertyKey: string,
    descriptor: PropertyDescriptor
  ) {
    descriptor.enumerable = value;
  };
}
// ES6의 Object.defineProperty(obj, prop, descriptor)와 같다
```

[참고1](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) [참고2](https://haeguri.github.io/2019/08/25/typescript-decorator/)

(2) @Watch

```javascript
const watchExample = new Vue({
  el: "#watch-example",
  data: {
    question: "",
    answer: "질문 후에 대답 할 수 있습니다",
  },
  watch: {
    question: function (newQuestion) {
      this.answer = "입력을 기다리는 중...";
    },
  },
});
```

```typescript
@Component
export default class WatchExample exetends Vue {
  question: string = '';
  answer: string = '질문을 하기 전까지는 대답할 수 없습니다.';

  @Watch('question')
  watcher() {
    this.answer = '입력을 기다리는 중...';
  }
}
```

(3) @Emit

```javascript
export default {
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    addToCount(n) {
      this.count += n;
      this.$emit("add-to-count", n);
    },
    resetCount() {
      this.count = 0;
      this.$emit("reset");
    },
    returnValue() {
      this.$emit("return-value", 10);
    },
    promise() {
      const promise = new Promise((resolve) => {
        setTimeout(() => {
          resolve(20);
        }, 0);
      });
      promise.then((value) => {
        this.$emit("promise", value);
      });
    },
  },
};
```

```typescript
@Component
export default class EmitComponent extends Vue {
  count = 0;

  @Emit()
  addToCount(n: number) {
    this.count += n;
  }

  @Emit("reset")
  resetCount() {
    this.count = 0;
  }

  @Emit()
  returnValue() {
    return 10;
  }

  @Emit()
  promise() {
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve(20);
      }, 0);
    });
  }
}
```

(4) @Provide / @Inject

```javascript
const symbol = Symbol("baz");

export const ProvideComponent = Vue.extend({
  inject: {
    foo: "foo",
    bar: "bar",
    optional: { from: "optional", default: "default" },
    [symbol]: symbol,
  },

  data() {
    return {
      foo: "foo",
      baz: "bar",
    };
  },

  provide() {
    return {
      foo: this.foo,
      bar: this.baz,
    };
  },
});
```

```typescript
const symbol = Symbol("baz");

@Component
export class ProvideComponent extends Vue {
  @Inject() readonly foo!: string;
  @Inject("bar") readonly bar!: string;
  @Inject({ from: "optional", default: "default" }) readonly optional!: string;
  @Inject(symbol) readonly baz!: string;

  @Provide() foo = "foo";
  @Provide("bar") baz = "bar";
}
```

여기서 잠깐,

```typescript
@Provide('message') msg: string = 'provide/inject example';
// Type string trivially inferred from a string literal, remove type annotation.
```

`@Provide('message') msg: string = 'provide/inject example'`라고 쓰니 eslint에서 string 타입 할당을 하지 않아도 된다고 한다.
왜일까?
이는 자바스크립트의 타입 추론을 당연하지만 타입스크립트에서도 사용하고 있기 때문인데, eslint에서 이 설정을 바꾸려면 `"no-inferrable-types": [true]`로 설정해 주면 된다
[참고](https://server0.tistory.com/46)

**그래서, Inject/provide가 props와 다른점은?** [공식 문서](https://vuejs.org/v2/api/#provide-inject)

- provide/inject는 위와 같이 사용하기 간편한데에 비해 props는 `:property="value"`와 같이 데이터를 직접 주입해주어야 한다
- 하지만 provide/inject를 매번 사용한다면 데이터의 흐름을 파악하기 어려울 것

(5) @Model (Property decorator)

```javascript
export default {
  model: {
    prop: "checked",
    event: "change",
  },
  props: {
    checked: {
      type: Boolean,
    },
  },
};
```

```typescript
@Component
export default class ModelComponent extends Vue {
  @Model("change", { type: Boolean }) readonly checked!: boolean;
}
```

(6) Mixins
[공식 문서](https://kr.vuejs.org/v2/guide/mixins.html)
[예제용 브랜치](https://github.com/harrykim14/vue-with-typescript/tree/MixinExample)

- 객체들이 공통적으로 사용하는 기능이 있다면 모듈화 하고싶지 않은가?
- 그 기능을 담당하는 것이 Mixins

> 해당 강의를 듣던 도중에 ` public toggle() { this.show = !this.show; }`라고 작성하였을 때 리턴값을 지정해주어야 한다고 경고가 떠서 뒤에 :void를 붙여 해결하였음

```typescript
export default class Dropdown extends Mixins(toggle)
```

와 같이 다중 상속을 받을 수 있는데 이 때 Mixins의 인자값으로 사용할 수 있는 개수는 5개 까지이다

</div>
</details>

<details>
<summary>6. Interface로 Vuex 설계하기</summary>
  <div markdown="6">
   (1) Event bus 활용하기

```typescript
// event-bus.ts라는 파일을 새로 생성하여 Vue의 인스턴스만 생성하게 하고 이를 이벤트 버스로 사용
import Vue from "vue";
export default new Vue();
```

```typescript
// a.vue
<template>
  <div>
    <input type="text" v-model="text" />
    <button @click="click">B로 전송</button>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import Bus from "@/common/event-bus";

@Component
export default class A extends Vue {
  text = "";
  click(): void {
    Bus.$emit("sendText", this.text);
  }
}
</script>
```

```typescript
// b.vue
<template>
  <div>A에서 작성한 메세지는? => {{ text }}</div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import Bus from "@/common/event-bus";

@Component
export default class A extends Vue {
  text = "";
  created(): void {
    Bus.$on("sendText", (text: string) => {
      this.text = text;
    });
  }
}
</script>

<style scoped></style>

```

```typescript
// App.vue
<template>
  <div>
    <A />
    <B />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import A from "@/components/a.vue";
import B from "@/components/b.vue";
@Component({
  components: {
    A,
    B,
  },
})
export default class App extends Vue {}
</script>

<style></style>

```

- App.vue에서 보는 것 처럼 부모자식이 아닌 A와 B 컴포넌트에 props를 전달하지 않아도 Event-bus를 사용함으로써 객체간 정보 전달이 가능함
- 하지만 프로젝트의 크기가 커지고 컴포넌트 개수가 늘어날수록 이벤트버스를 사용한 방식은 데이터의 흐름 파악이 어렵게 된다

  (2) Vuex를 사용하여 상태 관리하기
  <image src="https://user-images.githubusercontent.com/67398691/113967468-02745380-986c-11eb-9bc2-28f82825ee61.png" width="800" alt="organizing status with vuex"/>
  (이미지2. vuex에서 이벤트를 실행하고 상태를 관리하는 모습을 나타낸 도식도)

  (3) Typescript의 Interface와 Generic을 사용한 Vuex store 구현

  ```typescript
  import Vue from "vue";
  import Vuex, { StoreOptions } from "vuex";

  Vue.use(Vuex);

  interface State {
    //... 상태값 인터페이스
  }

  const Store: StoreOptions<State> = {
    state: {
      // 상태값
    },
    mutations: {
      // 변이 함수
    },
    actions: {
      // 변이를 위한 로직 함수
    },
    getters: {
      // 해당 객체의 데이터를 반환하는 함수
    },
  };

  export default new Vuex.Store(Store);
  ```

  - store에 등록하고 나면 `$store`로 접근이 가능하며 사용하고자 하는 함수를 `$store.dispatch("함수명")`이나 `$store.getters.함수명`과 같이 접근 할 수 있다

  (4) Vuex Module Interface를 구현하여 Store 분리하기

  ```typescript
  import Vue from "vue";
  import Vuex, { StoreOptions, Modules } from "vuex";

  Vue.use(Vuex);

  interface RootState {
    /* 루트 인터페이스 */
  }
  interface ModuleAState {
    /* module A State */
  }
  interface ModuleBState {
    /* module B State */
  }

  const moduleA: Module<ModuleAState, RootState> = {
    namesaced: true,
    state: {},
    mutations: {},
    actions: {},
    getters: {},
  };

  const moduleB: Module<ModuleBState, RootState> = {
    state: {},
    mutations: {},
    actions: {},
    getters: {},
  };

  const store: StoreOptions<RootState> = {
    state: {},
    modules: { moduleA, moduleB },
    mutations: {},
    actions: {},
    getters: {},
  };

  export default new Vuex.Store(Store);
  ```

  - 해당 강의에서는 `created() { console.log(this.$store)}`로 스토어 값을 확인하였지만 Vue 3.0에서는 created()와 beforeCreate()가 lifecycle에서 삭제되었고 setup()으로 변경되어서([참고](https://m.blog.naver.com/dndlab/221952030079)) setup()을 사용해보았으나 console.log()가 찍히지 않았음 → 크롬 확장도구인 Vue devtools를 사용하여 state값을 관찰함

  - 하지만 setup() 함수에서는 this를 지원하지 않기 때문에([공식문서](https://v3.vuejs.org/guide/composition-api-introduction.html#setup-component-option)) 이를 해결하고자 최상위 RootStore를 변경하였음

  - Vuex 4.0의 공식문서와는 달리 createStore를 불러올 수 없었는데, 이는 npm에서 제공하는 최근 릴리즈가 v3.6.2이기 때문이었음

  - vue와 vuex 및 vue-router를 최신버전(vue@3.0.11, vuex@4.0.0, vue-router@4.0.6)으로 업그레이드하고 예제를 적용해 봄

    [참고 영상](https://www.youtube.com/watch?v=fh0VboqAc8k) 및 [샘플 코드 브랜치](https://github.com/harrykim14/vue-with-typescript/tree/newVersionOfVuexExample)

  </div>
  </details>

<details>
<summary>7. 알아두면 좋은 Vuex 오픈소스</summary>
<div markdown="7">
  (1) vuex-module-decorator

- Class style로 vuex를 작성할 수 있다

```typescript
// eg. /app/store/mymodule.ts
import { Module, VuexModule } from "vuex-module-decorators";

@Module({ namespaced: true, name: "moduleA" })
export default class MyModule extends VuexModule {
  data: string = "moduleA";

  @Mutation
  setData(data: string) {
    this.data = data;
  }

  @Action
  editData(data: string) {
    // 원래는 emit이나 commit 등이 담긴 context를 첫번째 인자로 받음
    // 같은 code block안에 있기 때문에 같은 이름의 함수로 작성해서는 안 된다
    this.context.commit("setData", data);
  }

  get data() {
    return this.data;
  }
}
```

(2) vuex-class

- Decorator를 사용하여 State, Getter, Action 등에 접근 가능

```vue
<!-- count.vue -->
<template>
  <div>
    <!-- 기존에 $store.getters.counter 로 접근하던 것을 counter 만으로도 접근 할 수 있다-->
    {{ counter }}
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import { Getter, State } from "vuex-class";

@Component
export default class Count extends Vue {
  @Getter count: number;
  // @State count: number;
}
</script>
```

```vue
<!-- App.vue -->
<template>
  <div id="app">
    <count></count>
    <button @click="increase">증가</button>
    <button @click="decrease">감소</button>
  </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import { Action, Mutation } from "vuex-class";
import count from "@/components/count.vue";

@Component
export default class App extends Vue {
  @Action readonly increase;
  @Action readonly decrease;

  @Mutation readonly setCount;
}
</script>
```

</div>
</details>
