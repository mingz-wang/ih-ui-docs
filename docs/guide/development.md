# ih-ui

## 项目技术栈

1. Monorepo
2. pnpm
3. vue3
4. vite
5. less

## 目录结构

```
-- packages
  -- components 本地开发组件库(@ih/components)
  -- utils 工具包(@ih-ui/utils)
-- play 测试开发组件库的 Vue3 项目目录
```

TODO: 后面会加入如何进行单测的内容

## 组件开发

1. 编写 vue
2. 编写 less
3. 导出
4. 测试
5. 提交

```
-- components
  -- button 按钮组件
    -- src 源码
      -- button.vue
      -- index.ts
    -- style 样式
      -- button.less

```

### 例如

1. 在 `button.vue` 文件中写一个简单的按钮

```vue
<template>
  <button>测试按钮</button>
</template>
```

2. 在 `button/index.ts` 将其导出

```ts
import Button from "./button.vue";
export { Button };
export default Button;
```

3. 在 `components/src/index.ts` 集中导出所有组件(其他组件需要导出)

```ts
export * from "./button";
```

4. 在 `components/index.ts` 导出所有组件提供给外部使用(后续其他组件不需要了)

```ts
export * from "./src/index";
```

5. 在 `play` 中测试

```vue
<template>
  <div>
    <Button />
  </div>
</template>
<script lang="ts" setup>
import { Button } from "@ih-ui/components";
</script>
```

运行

```shell
cd play
pnpm dev
```

首次运行需要下载依赖

```shell
pnpm install
```

6. 组件开发

 Button 组件就需要接收 type、size、round 等属性,这里以只接收一个属性 type 来开发一个简单的 Button 组件为例：

我们可以根据传入的不同 type 来赋予 Button 组件不同类名

新增 `button/types.ts`

```ts
import { ExtractPropTypes } from 'vue'

export const ButtonType = ['primary', 'success', 'info', 'warning', 'danger', 'text']

export const buttonProps = {
    type: {
        type: String,
        validator(value: string) {
            // 这里表示type只能接收这些值
            return ButtonType.includes(value)
        }
    },
}

export type ButtonProps = ExtractPropTypes<typeof buttonProps>
```

编写样式文件 `button/style/button.less`，作为示例只编写 `primary`

```less
.ih-button {
  display: inline-block;
  line-height: 1;
  white-space: nowrap;
  cursor: pointer;
  background: #fff;
  border: 1px solid #dcdfe6;
  color: #606266;
  -webkit-appearance: none;
  text-align: center;
  box-sizing: border-box;
  outline: none;
  margin: 0;
  transition: 0.1s;
  font-weight: 500;
  padding: 12px 20px;
  font-size: 14px;
  border-radius: 4px;
}

.ih-button.ih-button--primary {
  color: #fff;
  background-color: #409eff;
  border-color: #409eff;

  &:hover {
    background: #66b1ff;
    border-color: #66b1ff;
    color: #fff;
  }
}
```

修改 `button.vue`，引入上述文件

```vue
<template>
  <button class="ih-button" :class="buttonStyle">
    <slot />
  </button>
</template>

<script lang="ts" setup>
import "./style/button.less";
import { computed } from "vue";
import { buttonProps } from "./types";

defineOptions({ name: "ih-button" });

const props = defineProps(buttonProps);

const buttonStyle = computed(() => {
  return {
    [`ih-button--${props.type}`]: props.type,
  };
});
</script>
```

7. 测试

修改 `play/app.vue`

```vue
<template>
  <div>
    <Button type="primary">主要按钮</Button>
  </div>
</template>

<script lang="ts" setup>
import { Button } from "@ih-ui/components";
</script>
```

运行

```shell
pnpm dev
```