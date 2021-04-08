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

3. vue-cli로 plug-in 설치하기
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

5. Class-Based 컴포넌트 만들기

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

1. @Watch

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

2. @Emit

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
