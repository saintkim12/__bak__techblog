---
title: "[Vue Router] vue router 캐시 관리"
description:
date: 2021-09-27
tags:
  - Vue.js
  - Vue.js@3
  - Vue Router
  - Vue Router@next
layout: layouts/post.njk
---
(작성중)
---
# vue router 캐시 관리
## 상황
- Vue.js 3, Vue-router next 사용중
- 탭으로 여러 화면을 동시에 관리할 수 있도록 화면 구성 중
- 탭으로 띄운 화면을 종료하는 경우, 다시 켰을 때 탭의 데이터가 남아 있음
  - 같은 화면으로 메뉴 ID에 따라 여러 화면을 처리하는데, 구분값이 query에 있어, 기본 router-view의 키로는 구분이 불가능

### 기본적인 구조
- App.vue
  - Login.vue: 로그인 때 작동
  - Main.vue: 로그인 후 등 인증정보가 있을 때 작동
    - 메뉴 등 공통 화면
    - Vue Router의 router-view로 querystring의 파라미터에 따라 서버로부터 해당하는 값을 가져와 화면을 렌더링

## 처리
### 1. keep-alive를 주어 작업중인 화면을 (종료하지 않은 경우에는) 유지할수 있도록 캐시 처리한다.
- [참고-Vue Router](https://next.router.vuejs.org/guide/migration/#router-view-keep-alive-and-transition)
- 작업 시, 활성화중인 URL(탭)로 접근하면 기존 탭이 활성화된다.(캐싱중인 화면 조회)
```html
<router-view v-slot="{ Component }">
  <keep-alive>
    <component :is="Component" />
  </keep-alive>
</router-view>
```

### (2. query의 값도 router-view Component로 사용할 수 있도록 커스터마이징된 키값을 준다)
- v-slot으로 넘어오는 속성인 route 값을 이용하여, path(params)/query/hash 등으로 화면별 고유값인 fullPath를 키로 사용
  - query값이 필요없는 경우 기본값인 key: route.path를 사용해도 됨
  - ```html
  <router-view v-slot="{ Component, route }">
    <keep-alive>
      <component :is="Component" :key="route.fullPath" />
    </keep-alive>
  </router-view>
  ```
- 여기까지 작업하면, 탭처리로 컴포넌트는 구분되나, 탭 종료시 화면 정보가 남아있음(다시 화면으로 접근하면 이전 정보가 남아있음)
- 이는 vue의 keep-alive의 정보를 vue가 직접 관리하기 때문임(개발자가 컨트롤 불가)

### 3. keep-alive ref화하고, 해당 ref로 keep alive cache를 수동 관리한다
- [참고](https://github.com/vuejs/vue-next/issues/2077#issuecomment-887425577)
```html
<router-view v-slot="{ Component, route }">
  <keep-alive ref="keepAlive">
    <component :is="Component" :key="route.fullPath" />
  </keep-alive>
</router-view>
<script>
/**
 * keepAlive 의 캐시를 수동으로 지우는 함수
 * 추후 keepAlive의 캐시를 관리하는 기능이 추가될 것으로 보임
 * https://github.com/vuejs/vue-next/issues/2077#issuecomment-887425577
 */
const __removeKeepAliveCache = ({ key, keepAliveRef }) => {
  const keepAliveInstance = keepAliveRef.$
  const cache = keepAliveInstance.__v_cache
  const vnode = cache.get(key)
  // console.log('__removeKeepAliveCache', key, vnode)
  if (vnode) {
    let shapeFlag = vnode.shapeFlag
    if (shapeFlag & 256 /* COMPONENT_SHOULD_KEEP_ALIVE */) {
      shapeFlag -= 256 /* COMPONENT_SHOULD_KEEP_ALIVE */
    }
    if (shapeFlag & 512 /* COMPONENT_KEPT_ALIVE */) {
      shapeFlag -= 512 /* COMPONENT_KEPT_ALIVE */
    }
    vnode.shapeFlag = shapeFlag

    const renderer = keepAliveInstance.ctx.renderer
    renderer.um(vnode, keepAliveInstance, keepAliveInstance.suspense)

    cache.delete(key)
  }
  // console.log('cache', cache)
}
({
  setup() {
    // keep-alive 객체를 관리
    const keepAlive = ref()
    // 탭이 닫힐 때 이벤트
    const onTabClose = (fullPath) => {
      // after tab closed
      // key에 해당하는 캐시 삭제
      __removeKeepAliveCache({ key: fullPath, keepAliveRef: keepAlive })
    }
    return {
      keepAlive
    }
  }
})
</script>
```
