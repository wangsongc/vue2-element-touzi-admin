## 利用Vue-codemod工具升级Vue2项目

### 概述

Vue3正式版本发布已经半年有余，据说Vue2会逐步停止维护，如何将现有的Vue2工程迁移到Vue3成为了大家关心的问题，尽管目前Vue官方有了详细的迁移指导，但实际项目中源码升级迁移工作量比较大，为减少重复性工作，这里推荐一款迁移工具vue-codemod，该工具可以将大部分的Vue2（推荐vue2.6以上版本）的语法直接升级成Vue3语法，之后配合一些手动更改完成Vue项目的迁移。

[vue-codemod链接](https://github.com/originjs/vue-codemod)

[演示项目链接](https://github.com/wangsongc/vue2-element-touzi-admin/tree/next)

### 全局安装vue-codemod

话不多说，上工具，工具安装

`npm install originjs/vue-codemod -g` or `yarn add originjs/vue-codemod -g`

### 迁移步骤
#### Step1：[使用Vue codemod来改造我们的源代码](https://github.com/wangsongc/vue2-element-touzi-admin/commit/1f6242fe64710f322dd27bc1efc4fd373a43187a)

注意：vue-codemod是在源路径下修改文件，若您的代码没有使用诸如Git、SVN等版本管理工具时，请提前备份。

运行命令：`npx vue-codemod src -a`，在手动修改前直接使用vue-codemod来一波自动升级（实际是语法替换），下面是转换日志，我们可以看到转换了哪些规则，更改了哪些文件；

```cmd
npx vue-codemod src -a
Warning!!
This tool may overwrite files.
press enter or enter yes or enter Y to continue:
Processing use new-component-api transformation
Processing use vue-class-component-v8 transformation
Processing use new-global-api transformation
Processing use vue-router-v4 transformation
Processing use vuex-v4 transformation
Processing use scoped-slots-to-slots transformation
Processing use new-directive-api transformation
Processing use remove-vue-set-and-delete transformation
Processing use rename-lifecycle transformation
Processing use add-emit-declaration transformation
Processing use global-filter transformation
Processing use tree-shaking transformation
Processing use v-model transformation
Processing use render-to-resolveComponent transformation
Processing use vue-i18n-v9 transformation
Processing use vuex-create-logger transformation
Processing use router4-onready-to-isready transformation
Processing use router-update-addRoute transformation
Processing use slot-attribute transformation
Processing use slot-default transformation
Processing use slot-scope-attribute transformation
Processing use v-for-template-key transformation
Processing use v-else-if-key transformation
Processing use transition-group-root transformation
Processing use v-for-v-if-precedence-changed transformation
Processing use remove-listeners transformation
Processing use v-bind-sync transformation
Processing use remove-v-on-native transformation
Processing use router-link-event-tag transformation
Processing use router-link-exact transformation
Processing use router-view-keep-alive-transition transformation
--------------------------------------------------
Processed file:
src/utils/mUtils.js
src/components/iconSvg/index.js
src/main.js
src/lang/index.js
src/router/index.js
src/store/index.js
src/components/echarts/barChart.vue
src/components/echarts/lineChart.vue
src/components/echarts/pieChart.vue
src/components/echarts/radarChart.vue
src/page/fundData/incomePayPosition.vue
src/components/pagination/index.vue
src/page/fundList/components/addFundDialog.vue
src/page/fundList/components/searchItem.vue
src/page/permission/components/SwitchRoles.vue
src/page/share/components/hengShare.vue
src/page/share/components/infoShare.vue
src/page/share/components/inviteShare.vue
src/page/share/components/jianshuLeftShare.vue
src/page/share/components/jianshuShare.vue
src/page/share/components/juejinShare.vue
src/page/share/components/sinaShare.vue
src/page/share/components/wxCodeModal.vue
src/page/share/components/yanShare.vue
src/permission.js
src/layout/bread.vue
src/layout/content.vue
src/layout/footerNav.vue
src/layout/headNav.vue
src/layout/home.vue
src/layout/leftMenu.vue
src/layout/topMenu.vue
src/page/fundData/fundPosition.vue
src/page/fundData/typePosition.vue
src/page/fundList/chinaTabsList.vue
src/page/fundList/fundList.vue
src/page/infoManage/infoModify.vue
src/page/infoManage/infoShow.vue
src/page/permission/directive.vue
src/page/userList/userList.vue
src/page/fundList/moneyData/index.vue
src/page/index/components/cardList.vue
src/page/index/components/commentList.vue
src/page/index/components/logList.vue
src/page/index/components/salesTable.vue
src/page/fundList/components/chinaTabsTable.vue
src/page/login.vue
package.json
Processed 48 files
63 places need to be transformed
63 places was transformed
The transformation rate is 100%
The transformation stats:

{
  'new-component-api': 1,
  'new-global-api': 1,
  'vue-router-v4': 1,
  'vuex-v4': 1,
  'rename-lifecycle': 5,
  'add-emit-declarations': 13,
  'vue-i18n-v9': 1,
  'slot-attribute': 15,
  'slot-scope-attribute': 16,
  'v-for-template-key': 3,
  'v-bind-sync': 2,
  'remove-v-on-native': 3,
  'router-view-keep-alive-transition': 1
}
```

####  Step2：[将element-ui升级为element-plus](https://github.com/wangsongc/vue2-element-touzi-admin/commit/70219a8d882817037f8a27387f23d78089f50b66)

Vue3迁移中最大的限制就是依赖，如果诸如UI组件之类的依赖不支持Vue3，那就建议你暂时不要升级，等支持了再升级；

#### Step3：[修复一些全局 API 错误](https://github.com/wangsongc/vue2-element-touzi-admin/commit/a1a207e68c7b563cdfeeb7c7d22ff86ddc4bfb95)

实践中发现有少部分的全局API工具是无法精准替换的，这里需要根据控制台的报错或告警信息来手动修改；

#### Step4：[修复一些与路由器升级相关的错误](https://github.com/wangsongc/vue2-element-touzi-admin/commit/92608e58ae2d89a161eefcdd1495ef6389c1e2f0)

router4中有一些迁移的坑，有一部分vue-codemod已经帮我们修改了，但仍然有一些非兼容信变更需要手动修改；

#### Step5：[修复一些与 vuex 和 vue-i18n 升级相关的错误](https://github.com/wangsongc/vue2-element-touzi-admin/commit/53dc34d03f64ef75b1f1e1897c135b6ee19a3b9f)

vuex和vue-i18n的升级迁移，vue-codemod也做了一部分，但是此项目中有一些生产环境判断逻辑似乎修改不了，暂时我们注释掉，不影响项目运行；

#### Step6：[修复警告](https://github.com/wangsongc/vue2-element-touzi-admin/commit/adfdafdeb34a7399734dad8f7f75dd4494f7467b)

至此项目已经可以很好的运行起来了，再根据控制台的一些告警进行修复，让它更优雅的运行。。。。

### 最后我们来看看效果图

![image-20210716101939971](https://user-images.githubusercontent.com/40830929/125884473-b89f344c-db98-4496-9d44-33b5e773c93d.png)

