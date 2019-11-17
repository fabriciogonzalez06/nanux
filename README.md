# 💎 @Nanux/store
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors)

An experimental library to do Redux in Angular with the new Ivy Engine and higher-order components.

**🔥 Why you should use @nanux/store?**
- 🧙 Selectors are automagically generated!
- 🏎️ Straight forward implementation and in only a few steps you can have a Redux store up and running
- 🐛 Redux Dev Tools support
- 👷 Encourage functional and reactive programming

## 💥 Motivation 
There are great solutions including vanilla Redux implementations but most of them end in a very verbose code. 

## ⚙ How to implement @nanux/store in my project?

There are two ways to use it:

### 🎖️ Decorator Reducer
**1. Import `NanuxStore` in the `app.module.ts`**

```
import { NanuxStore } from '@nanux/store';

@NgModule({
  declarations: [...],
  imports: [
    ...
    NanuxStore.forRoot({ decorators: true })
  ]
  bootstrap: [...]
})
export class AppModule {}

```
Note: `decorators` enable the use of the brand new `reducer` decorator.

**2. Create State**
```
import { Reducer,  GetStore, Store } from '@nanux/store';

export enum TODOs {
  INCREMENT = '[UI] INCREMENT'
}

@Injectable({
  providedIn: 'root',
})
@GetStore('counter', { counter: 0 })
export class AppState {

  counter$: Observable<number>;

  @Reducer(TODOs.INCREMENT)
  increment = (state) => ({ ...state, counter: state.counter + 1 });

  constructor(public store: Store) { }

  increment(: void  {
    this.store.dispatch(new Action(TODOs.INCREMENT));    
  }
}
```
Done! Easy-peasy!..... Wait, wait... How do I access the store data?

**4. Well, behind the scenes the selectors are automagically generated!**

**What name do my selectors have?**
@nanux/store uses the schema of the initial state you defined to generate the selectors with the same name plus `$` symbol.
In the example above `counter$: Observable<number>;` has access to `counter` value in the store.

**Why do I need to declare my selectors properties?**
There is a limitation of Typescript that enforces us to declare the properties that will store the selectors otherwise, the compiler will yell because it currently does not have a way to know that new properties were dynamically generated by @nanux/store.

To know more about this issue go [here](https://github.com/microsoft/TypeScript/issues/4881)

**5. Consume the selector in your component by `<h1>Nanux! {{ appState.counter$ | async }}</h1>`**

**Not feeling that adventurous to go with the reducer decorator? no problem**

### 👴🏻 Tradicional reducer
This library also supports the traditional switch style to declare reducers.

**1. Create a new file for the reducer and remove the initial state from the `GetStore` decorator**

```
export const initialState: State = {
  counter: 0
};

export const reducer = (state = initialState, action: Action): State => {
  switch (action.type) {
    case TODOs.GET_DATA: {
      return { ...state, counter: state.counter + action.payload };
    }
    default:
      return state;
  }
};
```

**2. Go to the `app.module` and define the`reducerMap` config by importing the reducer we created above**
```
@NgModule({  
  imports: [
    ...
    NanuxStore.forRoot({ reducerMap: { counterState: reducer })
  ]...
})
```

That's it! ❤️ 

## 💼 How to Contribute?
Visit the [code of conduct](./CODE_OF_CONDUCT.md) and contribution [guidelines](./CONTRIBUTING.md)

## 🌎 Roadmap
> This document pretends to show what work is in progress and future plans to improve the library. Feel free to create an issue and explain what features would like to have

### @nanux/store
**Alpha**
- [x] Basic service to handle store  
- [x] Ivy support / NG9
- [x] Higher-Order Components to create a store
- [x] Auto-generation of selectors
- [x] Basic Redux DevTools support
- [X] Basic documentation
- [X] Evergreen Browser support
- [ ] First release to NPM registry
- [ ] 80% Unit testing coverage
- [ ] Improve action typing

**Beta**
- [ ] Memoization of data 
- [ ] Implementation of pattern to handle sideffects
- [ ] Create or separate classes in charge of the busineess logic into a different lib to 
- [ ] Schematics to implement the library faster

**V1**
- [ ] CI/CD support to run unit tests and E2E
- [ ] Persistent data - Rehydrid data from storage or HTTP service  

**V1-Next**
- [ ] Better type support 
- [ ] Lazy loading of states 
- [ ] Normalization state proposal
- [ ] Custom pipes to resolve state in the HTML

## FAQ
1. **Can I generate my own selectors?**
    Yes, use the `select` function to get pieces of the state e.g
   `public customSelector = this.store.state(select((state) => state.counter))` where `store` is the Redux store service
2. **Why my selectors are not being generated?**
    Make sure that you have defined a initial state to all your properties. Selectors are created based on the schema of the initial state
3. **Can I mix the use of the reducer decorator and switch syntax?**
    Nope. Each implementation is treated differently.
4. **Why would I use the reducer decorator?**
    Less code and works faster. Each action will map to the reducer specify. A difference to the switch syntax the decorator flow does not need to evaluate all the cases.


## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/mahcr"><img src="https://avatars2.githubusercontent.com/u/16544451?v=4" width="100px;" alt="Mariano Alvarez"/><br /><sub><b>Mariano Alvarez</b></sub></a><br /><a href="#ideas-mahcr" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/mahcr/nanux/commits?author=mahcr" title="Code">💻</a> <a href="#maintenance-mahcr" title="Maintenance">🚧</a> <a href="https://github.com/mahcr/nanux/commits?author=mahcr" title="Tests">⚠️</a></td>
  </tr>
</table>

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
