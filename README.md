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

4. 실행해보기
```
npm run serve
```
<image src="https://user-images.githubusercontent.com/67398691/113802120-1ac56f00-9795-11eb-8346-e090aa96111d.png" width="600" alt="vue.js index page"/>
