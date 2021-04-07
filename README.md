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
