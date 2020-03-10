[vue.config.js配置解析](https://cli.vuejs.org/zh/config/#publicpath)



# TypeScript在vue项目中的使用

## 修改vuex

演示目录结构 

![计算机生成了可选文字: 。0test 丨到gettersts 下§index.ts 五§mutations.ts 巨§过猷et$ types-ts 园Index.ts 国typ“](assets/clip_image001.png) 

### stroe根文件夹： 

- #### types.ts：为根节点指定类型 

```javascript
export interface RootState {
  version: string;
}
```

- #### index.ts:store的根文件

```javascript
import Vue from 'vue';    
import Vuex, { StoreOptions } from 'vuex'; //导入StoreOptions这个模块，该模块为store
import { RootState } from './types';
import { profile } from '@/store/test/index'; //模块名称
Vue.use(Vuex);
const store: StoreOptions<RootState> = {
  state: {
    version: '1.0.2' // a simple property
  },
  modules: {
    profile, //模块名称
  }
};
export default new Vuex.Store<RootState>(store);
```

### 子模块文件夹 

- #### test/types.ts子模块的数据类型

```javascript
export interface User {
  firstName: string;
  lastName: string;
  email: string;
  phone?: string;
}
export interface ProfileState {
  user?: User;
  error: boolean;
}
```

- #### test/index.ts 

```javascript
import { Module } from 'vuex'; //vuex对模块的定义<子节点的类型，根节点的类型>
import { getters } from './getters';
import { actions } from './actions';
import { mutations } from './mutations';
import { ProfileState } from './types';
import { RootState } from '../types';
import {state} from "@/store/test/states";
const namespaced = true;
export const profile: Module<ProfileState, RootState> = {  //在这里指定模块名称
  namespaced,
  state,
  getters,
  actions,
  mutations
};
```

- #### test/state.ts  

```javascript
import {ProfileState} from "@/store/test/types";
export const state: ProfileState = {
  user: undefined,
  error: false
};
```

- #### test/getters.ts 

```javascript
// profile/getters.ts
import { GetterTree } from 'vuex'; //vuex中getter的类型
import { ProfileState } from './types';
import { RootState } from '@/store/types';
export const getters: GetterTree<ProfileState, RootState> = {
  fullName (state): string {
    const { user } = state;
    const firstName = (user && user.firstName) || '';
    const lastName = (user && user.lastName) || '';
    return `${firstName} ${lastName}`;
  }
};
```



- #### test/mutations.ts: 

```javascript
// profile/mutations.ts
import { MutationTree } from 'vuex'; //vuex中mutation的类型
import { ProfileState, User } from './types';
export const mutations: MutationTree<ProfileState> = {
  profileLoaded (state, payload: User) {
    state.error = false;
    state.user = payload;
  },
  profileError (state) {
    state.error = true;
    state.user = undefined;
  }
};
```

- test/action.ts 

```javascript
// profile/actions.ts
import { ActionTree } from 'vuex';   //vuex中action的类型
import { ProfileState, User } from './types';
import { RootState } from '@/store/types';
export const actions: ActionTree<ProfileState, RootState> = {
  fetchData({commit}): any {
    commit('profileLoaded', {
      firstName: 'wang',
      lastName: 'yi',
      email: '2928464591@qq.com',
      phone: '13427479705'
    });
  }
};
```

在vue文件中的调用

```javascript
import {
  State,
  Getter,
  Action,
  Mutation,
  namespace
} from 'vuex-class'
// 这几个是提供用于注入的注解
然后在相应的属性或者方法上注解上就可以使用了
const namespace = 'test';
@State(namespace)
userdata!: ProfileState;
@Action('fetchData', { namespace }) //方法名
fun1: any;
@Getter('fullName', { namespace })
full: string | undefined;

```

