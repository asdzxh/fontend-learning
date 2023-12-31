

# 基于 Vue 3 + Vite 的后台管理系统

> 技术栈
>
> - [Vue 3.2 + setup](https://cn.vuejs.org/)
>
> - [Vite](https://vitejs.dev/guide/)
> - [Element Plus](https://element-plus.gitee.io/zh-CN/)
> - [Windi CSS](https://windicss.org/)
> - [VueUse](https://vueuse.org/)
> - Apifox

## 一、项目创建和配置

### 一、创建项目

```shell
# npm 6.x
npm create vite@latest shop-admin --template vue

# npm 7+, extra double-dash is needed:
npm create vite@latest shop-admin -- --template vue

# yarn
yarn create vite shop-admin --template vue

# pnpm
pnpm create vite shop-admin --template vue
```

### 二、引入 Element Plus

1. 添加依赖

```json
"dependencies": {
    "vue": "^3.2.47",
    "element-plus": "^2.3.3"
}
```

2. 添加智能感知插件



3. main.js 引入

```js
import { createApp } from 'vue'
import App from './App.vue'

import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'

const app = createApp(App)

app.use(ElementPlus)

app.mount('#app')
```

4. 测试是否生效

```vue
<script setup>
</script>

<template>
  <el-row class="mb-4">
    <el-button>Default</el-button>
    <el-button type="primary">Primary</el-button>
    <el-button type="success">Success</el-button>
    <el-button type="info">Info</el-button>
    <el-button type="warning">Warning</el-button>
    <el-button type="danger">Danger</el-button>
  </el-row>
</template>
```

### 三、引入 Windi CSS

1. 添加依赖

```json
"devDependencies": {
    "@vitejs/plugin-vue": "^4.1.0",
    "vite": "^4.2.0",
    "windicss": "^3.5.6",
    "vite-plugin-windicss": "^1.8.10",
}
```

2. 添加智能感知插件



3. 添加配置 vite.config.js

```js
import WindiCSS from 'vite-plugin-windicss'

export default {
  plugins: [
    WindiCSS(),
  ],
}
```

4. 在 main.js 导入

```js
import 'virtual:windi.css'
```

5. 验证生效

```vue
  <div class="bg-red-600/50">
    <el-row class="mb-4">
      <el-button>Default</el-button>
      <el-button type="primary">Primary</el-button>
      <el-button type="success">Success</el-button>
      <el-button type="info">Info</el-button>
      <el-button type="warning">Warning</el-button>
      <el-button type="danger">Danger</el-button>
    </el-row>
  </div>
```

6. 使用 @apply 简化样式

```vue
<script setup>
</script>

<template>
  <div>
    <el-row class="mb-4">
      <button class="btn">Default</button>
    </el-row>
  </div>
</template>

<style scoped>
.btn {
  @apply bg-red-500 text-white m-15 px-5 py-2 rounded-full hover:(bg-red-700) focus:(ring-red-300) transition-all duration-500
}
</style>
```

### 四、引入 vue-router4

1. 添加依赖

``` json
"dependencies": {
    "element-plus": "^2.3.3",
    "vue": "^3.2.47",
    "vue-router": "^4.1.6"
},
```

2. src 目录创建 router 文件夹，并且创建 index.js

```js
// index.js


// 引入
import { createRouter, createWebHashHistory } from "vue-router";

// 路由数组
const routes = []

// 创建路由对象
const router = createRouter({
    history: createWebHashHistory(),
    routes
})

// 导出
export default router
```

3. 配置 src 别名

```js
// vite.config.js

export default defineConfig({
  resolve: {
    alias: {
      // 用 ~ 表示 src 更目录别名
      "~": path.resolve(__dirname, "src")
    }
  }
})
```

4. main.js 引入路由配置文件

```js
import router from './router'

app.use(router)
```

5. 配置基础路由以及404路由

​		新建 pages 目录，准备页面，修改路由配置

```js
// router/index.js


// 引入
import { createRouter, createWebHashHistory } from "vue-router";

import Index from '~/pages/index.vue'
import About from '~/pages/about.vue'
import NotFound from '~/pages/404.vue'

// 路由数组
const routes = [
    {
        path: '/',
        component: Index
    },
    {
        path: '/about',
        component: About
    },
    {
        path: '/:pathMatch(.*)*',
        name: 'NotFound',
        component: NotFound
    }
]

// 创建路由对象
const router = createRouter({
    history: createWebHashHistory(),
    routes
})

// 导出
export default router
```

## 二、登录页开发和功能实现

#### 一、页面实现

1. 新建 login.vue 页面，并且在路由文件中注册页面
2. 简单页面实现

```vue
<template>
    <div>
        <el-row class="box-border w-screen h-screen">
            <el-col :span="16" class="bg-blue-400 text-white col-center">
                <span class="text-5xl font-bold mb-10">管 理 系 统</span>
                <span class="text-3xl">shop-admin</span>
            </el-col>
            <el-col :span="8" class="col-center">
                <h2 class="text-3xl font-bold">登录</h2>
                <div class="text-sm text-gray-500 flex w-full justify-center items-center p-5">
                    <span class="h-[1px] w-1/5 bg-gray-200"></span>
                    <span class="mx-2 ">账号密码登录</span>
                    <span class="h-[1px] w-1/5 bg-gray-200"></span>
                </div>
                <el-form label-width="120px" label-position="left">

                    <el-form-item label="Username">
                        <el-input type="text" placeholder="请输入账号" />
                    </el-form-item>

                    <el-form-item label="Password">
                        <el-input type="password" placeholder="请输入密码" />
                    </el-form-item>

                    <el-button type="primary" class="w-80">登 录</el-button>
                </el-form>
            </el-col>
        </el-row>
    </div>
</template>

<script setup>

</script>

<style scoped>
.col-center {
    @apply flex flex-col justify-center items-center
}
</style>
```

3. 全局引入 Element Plus [图标库](https://element-plus.gitee.io/zh-CN/component/icon.html)

```bash
# 添加依赖
pnpm install @element-plus/icons-vue

# main.js 全局引入
import * as ElementPlusIconsVue from '@element-plus/icons-vue'

for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
    app.component(key, component)
}
```

4. 在登录表单中使用图标

```vue
<el-form-item label="Username">
    <el-input type="text" placeholder="请输入账号">
        <template #prefix>
            <el-icon>
                <user />
            </el-icon>
        </template>
    </el-input>
</el-form-item>

<el-form-item label="Password">
    <el-input type="password" placeholder="请输入密码">
        <template #prefix>
            <el-icon>
                <lock />
            </el-icon>
        </template>
    </el-input>
</el-form-item>
```

#### 二、添加表单校验

1. 添加表单规则

```js
import { reactive, ref } from 'vue';

// 定义表单提交对象
const form = reactive({
    username: '',
    password: ''
})

// 绑定表单规则
const rules = {
    username: [
        { required: true, message: '用户名不能为空', trigger: 'blur' }
    ],
    password: [
        { required: true, message: '密码不能为空', trigger: 'blur' }
    ]
}

// 定义表单元素dom
const formRef = ref(null)

// 点击事件
const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        console.log('验证通过');
    })
}
```

2. 页面绑定规则

```html
<!-- model 绑定表单内容对象 、 rules 绑定规则 、 ref 绑定表单元素 -->
<el-form :model="form" label-width="120px" label-position="left" ref="formRef" :rules="rules">
    <!-- prop 绑定表单规则对象 -->
    <el-form-item label="Username" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username">
            <template #prefix>
                <el-icon>
                    <user />
                </el-icon>
            </template>
        </el-input>
    </el-form-item>
    <!-- prop 绑定表单规则对象 -->
    <el-form-item label="Password" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password">
            <template #prefix>
                <el-icon>
                    <lock />
                </el-icon>
            </template>
        </el-input>
    </el-form-item>
    <!-- 绑定点击事件 -->
    <el-button type="primary" class="w-80" @click="onSubmit">登 录</el-button>
</el-form>
```

#### 三、接口请求

##### 一、Apifox 准备 Mock 数据

1. 编写接口文档

![](https://i2.100024.xyz/2023/04/11/p22dd4.webp)

![](https://i2.100024.xyz/2023/04/11/p2r14l.webp)

2. 创建 mock

![](https://i2.100024.xyz/2023/04/11/p8497f.webp)

![](https://i2.100024.xyz/2023/04/11/p8qxrs.webp)

##### 二、页面实现请求

1. 安装 axios
2. 实现 axios 工具包，src 下创建 axios.js

```js
import axios from '@/axios'

const service = axios.create({
    baseURL: 'http://127.0.0.1:4523/m1/2571699-0-default/api',

})

export default service
```

3. src 添加 api 文件夹，添加请求接口

```js
//  src/api/admin.js

import axios from '~/axios'

// 管理员登录
export function adminLogin(username, password) {
    return axios.post(
        '/admin/login',
        {
            username,
            password
        }
    )
}

```

4. 页面引入 admin.js 并请求接口

```js
//  login.vue


import { adminLogin } from '~/api/admin'
import { ElNotification } from 'element-plus'
import router from '~/router/index'

const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        adminLogin(form.username, form.password).then((res) => {
            if (res.status === 200 && res.data.code === 0) {
                ElNotification({
                    message: res.data.msg,
                    type: 'success',
                    duration: 2000
                })
                router.push('/')
            } else {
                ElNotification({
                    message: res.data.msg || '请求失败',
                    type: 'error',
                    duration: 2000
                })
            }
        }).catch((err) => {
            ElNotification({
                message: err.response.data.msg || '请求失败',
                type: 'error',
                duration: 2000
            })
        })
    })
}
```

##### 三、使用 vueuse 保存登录状态

1. 添加 依赖

```json
  "dependencies": {
    "@element-plus/icons-vue": "^2.1.0",
    "@vueuse/core": "^9.13.0",
    "@vueuse/integrations": "^9.13.0",
    "axios": "^1.3.5",
    "element-plus": "^2.3.3",
    "universal-cookie": "^4.0.4",
    "vue": "^3.2.47",
    "vue-router": "^4.1.6"
  },
```

2. login.vue 页面

```vue
<script setup>
import { reactive, ref } from 'vue';
import { adminLogin } from '~/api/admin'
import { ElNotification } from 'element-plus'
import { useCookies } from '@vueuse/integrations/useCookies'
import router from '~/router/index'

const form = reactive({
    username: 'admin',
    password: '123456'
})

const rules = {
    username: [
        { required: true, message: '用户名不能为空', trigger: 'blur' }
    ],
    password: [
        { required: true, message: '密码不能为空', trigger: 'blur' }
    ]
}

const formRef = ref(null)

const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        adminLogin(form.username, form.password).then((res) => {
            // 判断状态码，是否登录成功
            if (res.status === 200 && res.data.code === 0) {
                // 将登录成功返回 token 存入 cookie
                const cookie = useCookies()
                cookie.set('admin-token', res.data.data.token)
                ElNotification({
                    message: res.data.msg,
                    type: 'success',
                    duration: 2000
                })
                router.push('/')
            } else {
                ElNotification({
                    message: res.data.msg || '登录失败',
                    type: 'error',
                    duration: 2000
                })
            }
        }).catch((err) => {
            ElNotification({
                message: err.response.data.message || '请求失败',
                type: 'error',
                duration: 2000
            })
        })
    })
}
</script>
```

3. index.vue 页面

```vue
<template>
    <div>
        {{ token }}
    </div>
</template>

<script setup>
import { useCookies } from '@vueuse/integrations/useCookies'
import { ref } from 'vue';

const cookie =  useCookies()
const token = ref(cookie.get('admin-token'))
</script>

<style scoped>

</style>
```

##### 四、axios 设置请求拦截

1. axios.js 添加请求拦截以及响应拦截

```js
import { useCookies } from '@vueuse/integrations/useCookies'
import { ElNotification } from 'element-plus'

// 请求拦截
service.interceptors.request.use(function (config) {
    // 从 cookie 中取出 token
    const cookie = useCookies()
    const token = cookie.get('admin-token')

    // 如果不为空，向 header 中添加 token
    if (token) {
        config.headers['token'] = token
    }
    return config
}, function(err) {
    // 对于请求出错
    return Promise.reject(err)
})

// 响应拦截
service.interceptors.response.use(function(res) {
    // 对于响应数据处理
    return res.data
}, function(err) {
    ElNotification({
        message: err.response.data.message || '请求失败',
        type: 'error',
        duration: 2000
    })
    return Promise.reject(err)
})
```

2. 修改 login.vue 

```vue
<script setup>
import { reactive, ref } from 'vue';
import { adminLogin } from '~/api/admin'
import { ElNotification } from 'element-plus'
import { useCookies } from '@vueuse/integrations/useCookies'
import router from '~/router/index'

const form = reactive({
    username: 'admin',
    password: '123456'
})

const rules = {
    username: [
        { required: true, message: '用户名不能为空', trigger: 'blur' }
    ],
    password: [
        { required: true, message: '密码不能为空', trigger: 'blur' }
    ]
}

const formRef = ref(null)
const loading = ref(false)

const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        loading.value = true
        adminLogin(form.username, form.password).then((res) => {
            // 判断状态码，是否登录成功
            if (res.code === 0) {
                // 将登录成功返回 token 存入 cookie
                const cookie = useCookies()
                cookie.set('admin-token', res.data.token)
                ElNotification({
                    message: res.msg,
                    type: 'success',
                    duration: 2000
                })
                router.push('/')
            } else {
                ElNotification({
                    message: res.msg || '登录失败',
                    type: 'error',
                    duration: 2000
                })
            }
        }).finally(() => {
            loading.value = false
        })
    })
}
</script>
```

##### 五、封装 token & ElNotification 公共方法

新建 src/utils 包

1. 改造token存取移除方法，新建 token.js

```js
// utils.js

import { useCookies } from '@vueuse/integrations/useCookies'
const tokenKey = 'admin-token'
const cookie = useCookies()

// 获取token
export function getToken() {
    return cookie.get(tokenKey)
}

// 设置token
export function setToken(token) {
    cookie.set(tokenKey, token)
}

export function removeToken() {
    cookie.remove(tokenKey)
}

// ----------------------------------------

// login.vue
import { setToken } from '~/utils/token.js'

const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        loading.value = true
        adminLogin(form.username, form.password).then((res) => {
            // 判断状态码，是否登录成功
            if (res.code === 0) {
                // 将登录成功返回 token 存入 cookie
                setToken(res.data.token)
                ElNotification({
                    message: res.msg,
                    type: 'success',
                    duration: 2000
                })
                router.push('/')
            } else {
                ElNotification({
                    message: res.msg || '登录失败',
                    type: 'error',
                    duration: 2000
                })
            }
        }).finally(() => {
            loading.value = false
        })
    })
}


// --------------------------------------
// index.vue
import { getToken } from '~/utils/token.js'

const token = ref(getToken())


// --------------------------------------
// axios.js
import { getToken } from '~/utils/token.js'

// 请求拦截
service.interceptors.request.use(function (config) {
    // 从 cookie 中取出 token
    const token = getToken()

    // 如果不为空，向 header 中添加 token
    if (token) {
        config.headers['token'] = token
    }
    return config
}, function(err) {
    // 对于请求出错
    return Promise.reject(err)
})
```

2. 封装 弹窗 组件，utils包下新建 toast.js

```js
// toast.js
// 封装消息提示组件
import { ElNotification } from 'element-plus'

// 封装 弹窗消息
export function toast(message, type = 'success', duration = 2000) {
    ElNotification({
        message,
        type,
        duration
    })
}


// --------------------------------------
// axios.js
import { toast } from '~/utils/toast'

// 响应拦截
service.interceptors.response.use(function(res) {
    // 对于响应数据处理
    return res.data
}, function(err) {
    toast(err.response.data.message || '请求失败', 'error')
    return Promise.reject(err)
})


// --------------------------------------
// login.vue
import { toast } from '~/utils/toast'
const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        loading.value = true
        adminLogin(form.username, form.password).then((res) => {
            // 判断状态码，是否登录成功
            if (res.code === 0) {
                // 将登录成功返回 token 存入 cookie
                setToken(res.data.token)
                toast(res.msg)
                router.push('/')
            } else {
                toast(res.msg || '登录失败', 'error')
            }
        }).finally(() => {
            loading.value = false
        })
    })
}
```

##### 六、全局路由拦截

1. 前置路由守卫，新建 src/permission.js

```js
import router from '~/router'

import { getToken } from '~/utils/token'
import { toast } from '~/utils/toast'

// 全局路由前置守卫
router.beforeEach((to, from, next) => {
    // to and from are both route objects. must call `next`.
    const token = getToken()

    // 目标页面不是登录页，且没有token，跳回登录页面
    if (!token && to.path != '/login') {
        toast('请先登录', 'error')
        return next({ path: '/login' })
    }

    // 防止重复登录
    if (to.path == '/login' && token) {
        toast('请勿重复登录', 'error')
        return next({ path: from.path || '/' })
    }

    next()
})
```

2. 全局页面动态标题

```js
// router/index.js
// 路由数组
const routes = [
    {
        path: '/',
        name:"index",
        component: Index,
        meta: {
            title: '后台首页'
        }
    },
    {
        path: '/about',
        name:"about",
        component: About,
        meta: {
            title: '关于'
        }
    },
    {
        path: '/login',
        name:"login",
        component: Login,
        meta: {
            title: '登录'
        }
    },
    {
        path: '/:pathMatch(.*)*',
        name: 'NotFound',
        component: NotFound,
        meta: {
            title: '页面丢失'
        }
    }
]


// ----------------------------------------
// permission.js
// 全局路由前置守卫
router.beforeEach((to, from, next) => {
    const token = getToken()

    // 目标页面不是登录页，且没有token，跳回登录页面
    if (!token && to.path != '/login') {
        toast('请先登录', 'error')
        return next({ path: '/login' })
    }

    // 防止重复登录
    if (to.path == '/login' && token) {
        toast('请勿重复登录', 'error')
        return next({ path: from.path || '/' })
    }

    // 设置页面标题
    let title = `admin - ${to.meta.title || ''}`
    document.title = title

    next()
})
```

##### 七、集成 Pinia 状态存储

1. 添加依赖

```json
  "dependencies": {
    "@element-plus/icons-vue": "^2.1.0",
    "@vueuse/core": "^9.13.0",
    "@vueuse/integrations": "^9.13.0",
    "axios": "^1.3.5",
    "element-plus": "^2.3.3",
    "pinia": "^2.0.34",
    "universal-cookie": "^4.0.4",
    "vue": "^3.2.47",
    "vue-router": "^4.1.6"
  },
```

2. 创建 src/store/index.js

```js
import { defineStore } from 'pinia'

export const useAdmin = defineStore('admin', {
    state: () => ({
        token: '',
        admin: {
            id: 0,
            role: '',
            nickname: '',
            avatar: ''
        },
    }),
    actions: {
        setStoreToken(token) {
            this.token = token
        },
        changeAvatar (avatar) {
            this.admin.avatar = avatar
        }
    }
})
```

3. main.js 引入 pinia

```js
import { createPinia } from 'pinia'

app.use(createPinia())
```

4. login.vue 登录时保存token

```js
import { useAdmin } from '~/store'

const store = useAdmin()
const { setStoreToken } = store

const onSubmit = () => {
    formRef.value.validate((valid) => {
        if (!valid) {
            // 校验失败
            return
        }
        loading.value = true
        adminLogin(form.username, form.password).then((res) => {
            // 判断状态码，是否登录成功
            if (res.code === 0) {
                // 将登录成功返回 token 存入 cookie
                setToken(res.data.token)
                setStoreToken(res.data.token)
                toast(res.msg)
                router.push('/')
            } else {
                toast(res.msg || '登录失败', 'error')
            }
        }).finally(() => {
            loading.value = false
        })
    })
}
```

5. index.vue 取出 token

```vue
<template>
    <div>
        {{ tokenStr }}
    </div>
    <div>
        {{ token }}
    </div>
</template>

<script setup>

import { getToken } from '~/utils/token.js'
import { ref } from 'vue';

import { useAdmin } from '~/store'
import { storeToRefs } from 'pinia'

const store = useAdmin()
const tokenStr = ref(getToken())

const { token, admin } = storeToRefs(store)
</script>
```



## 三、后台管理 Layout 布局开发





## 四、Dashboard 开发和交互





## 五、模块开发





## 六、部署上线





