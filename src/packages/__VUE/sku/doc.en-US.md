# Sku

### Intro

Often used for commodity selection

### Install

``` javascript
import { createApp } from 'vue';
import { Sku } from '@nutui/nutui';

const app = createApp();
app.use(Sku);
```


### Basic Usage

:::demo
```html
<template>
  <nut-cell :title="Basic Usage" desc="" @click="base = true"></nut-cell>
  <nut-sku
    v-model:visible="base"
    :sku="sku"
    :goods="goods"
    @selectSku="selectSku"
    @clickBtnOperate="clickBtnOperate"
    @close="close"
  ></nut-sku>
</template>
<script lang="ts">
import { ref,reactive,onMounted,toRefs} from 'vue';
export default {
  setup() {
      const base = ref(false);
      const data = reactive({
        sku: [],
        goods: {}
      });

      onMounted(() => {
        fetch('https://storage.360buyimg.com/nutui/3x/data.js')
          .then((response) => response.json())
          .then((res) => {
            const { Sku, Goods, imagePathMap } = res;
            data.sku = Sku;
            data.goods = Goods;
          }) //执行结果是 resolve就调用then方法
          .catch((err) => console.log('Oh, error', err)); //执行结果是 reject就调用catch方法
      });
      // 切换规格类目
      const selectSku = (ss: string) => {
        const { sku, skuIndex, parentSku, parentIndex } = ss;
        if (sku.disable) return false;
        data.sku[parentIndex].list.forEach((s) => {
          s.active = s.id == sku.id;
        });
        data.goods = {
          skuId: sku.id,
          price: '4599.00',
          imagePath:
            '//img14.360buyimg.com/n4/jfs/t1/215845/12/3788/221990/618a5c4dEc71cb4c7/7bd6eb8d17830991.jpg' 
        };
      };
      // 底部操作按钮触发
      const clickBtnOperate = (op:string)=>{
        console.log('点击了操作按钮',op)
      } 
      // 关闭商品规格弹框
      const close = ()=>{}
      return { base, selectSku, clickBtnOperate,close, ...toRefs(data) };
  }
}
</script>
```
:::

### Not Sell

:::demo
```html
<template>
  <nut-cell title="Not Sell" desc="" @click="notSell = true"></nut-cell>
  <nut-sku
    v-model:visible="notSell"
    :sku="sku"
    :goods="goods"
    :btnExtraText="btnExtraText"
    @changeStepper="changeStepper"
    @selectSku="selectSku"
    :btnOptions="['buy', 'cart']"
  >
    <template #sku-operate>
      <div class="sku-operate-box">
        <nut-button class="sku-operate-box-dis" type="warning">查看相似商品</nut-button>
        <nut-button class="sku-operate-box-dis" type="info">到货通知</nut-button>
      </div>
    </template>
  </nut-sku>
</template>
<script lang="ts">
import { ref,reactive,onMounted,toRefs} from 'vue';
export default {
setup() {
    const notSell = ref(false);
    const data = reactive({
      sku: [],
      goods: {}
    });

    const btnExtraText = ref('抱歉，此商品在所选区域暂无存货');

    onMounted(() => {
        fetch('https://storage.360buyimg.com/nutui/3x/data.js')
          .then((response) => response.json())
          .then((res) => {
            const { Sku, Goods, imagePathMap } = res;
            data.sku = Sku;
            data.goods = Goods;
          }) //执行结果是 resolve就调用then方法
          .catch((err) => console.log('Oh, error', err)); //执行结果是 reject就调用catch方法
    });

    // inputNumber 更改
    const changeStepper = (count: number) => {
      console.log('购买数量', count);
    };

    // 切换规格类目
    const selectSku = (ss: string) => {
      const { sku, skuIndex, parentSku, parentIndex } = ss;
      if (sku.disable) return false;
      data.sku[parentIndex].list.forEach((s) => {
        s.active = s.id == sku.id;
      });
      data.goods = {
        skuId: sku.id,
        price: '4599.00',
        imagePath:
          '//img14.360buyimg.com/n4/jfs/t1/216079/14/3895/201095/618a5c0cEe0b9e2ba/cf5b98fb6128a09e.jpg' 
      };
    };
    // 底部操作按钮触发
    const clickBtnOperate = (op:string)=>{
      console.log('点击了操作按钮',op)
    } 
    return { notSell, changeStepper,selectSku,btnExtraText,...toRefs(data) };
  }
}
</script>
<style>
.sku-operate-box {
  width: 100%;
  display: flex;
  padding: 8px 10px;
  box-sizing: border-box;
}
.sku-operate-box-dis{
    flex:1
}
.sku-operate-box-dis:first-child{
  margin-right: 18px;
}
</style>
```
:::

### Custom Stepper

You can configure the maximum value and minimum value of the digital input box as required

:::demo
```html
<template>
  <nut-cell title="Custom Stepper" desc="" @click="customStepper = true"></nut-cell>
  <nut-sku
    v-model:visible="customStepper"
    :sku="sku"
    :goods="goods"
    :stepperMax="7"
    :stepperMin="2"
    :stepperExtraText="stepperExtraText"
    @changeStepper="changeStepper"
    @overLimit="overLimit"
    :btnOptions="['buy', 'cart']"
    @selectSku="selectSku"
    @clickBtnOperate="clickBtnOperate"
  ></nut-sku>
</template>
<script lang="ts">
import { ref,reactive,onMounted,toRefs} from 'vue';
import { showToast } from '@nutui/nutui';
import '@nutui/nutui/dist/packages/toast/style'; 
export default {
setup() {
    const customStepper = ref(false);
    const data = reactive({
      sku: [],
      goods: {}
    });

    onMounted(() => {
        fetch('https://storage.360buyimg.com/nutui/3x/data.js')
          .then((response) => response.json())
          .then((res) => {
            const { Sku, Goods, imagePathMap } = res;
            data.sku = Sku;
            data.goods = Goods;
          }) //执行结果是 resolve就调用then方法
          .catch((err) => console.log('Oh, error', err)); //执行结果是 reject就调用catch方法
    });

    const stepperExtraText = () => {
      return `<div style="width:100%;text-align:right;color:#F00">2 件起售</div>`
    };
    // inputNumber 更改
    const changeStepper = (count: number) => {
      console.log('购买数量', count);
    };

    // inputNumber 极限值
    const overLimit = (val: any) => {
      if (val.action == 'reduce') {
        showToast.text(`至少买${val.value}件哦`);
      } else {
        showToast.text(`最多买${val.value}件哦`);
      }
    };
    // 切换规格类目
    const selectSku = (ss: string) => {
      const { sku, skuIndex, parentSku, parentIndex } = ss;
      if (sku.disable) return false;
      data.sku[parentIndex].list.forEach((s) => {
        s.active = s.id == sku.id;
      });
      data.goods = {
        skuId: sku.id,
        price: '4599.00',
        imagePath:
          '//img14.360buyimg.com/n4/jfs/t1/215845/12/3788/221990/618a5c4dEc71cb4c7/7bd6eb8d17830991.jpg' 
      };
    };
    // 底部操作按钮触发
    const clickBtnOperate = (op:string)=>{
      console.log('点击了操作按钮',op)
    } 
    return { customStepper, overLimit, changeStepper,selectSku, clickBtnOperate,stepperExtraText,...toRefs(data) };
}
}
</script>
```
::: 
### Custom Slots

The default partition is divided into several areas, which are defined as slots that can be replaced as required

:::demo
```html
<template>
  <nut-cell title="Custom Slots" desc="" @click="customBySlot = true"></nut-cell>
  <nut-sku
      v-model:visible="customBySlot"
      :sku="sku"
      :goods="goods"
      :btnOptions="['buy', 'cart']"
      @selectSku="selectSku"
      @clickBtnOperate="clickBtnOperate"
  >
      <!-- 商品展示区，价格区域 -->
      <template #sku-header-price>
          <div>
              <nut-price :price="goods.price" :needSymbol="true" :thousands="false"> </nut-price>
              <span class="tag"></span>
          </div>
      </template> 
      <!-- 商品展示区，编号区域 -->
      <template #sku-header-extra>
          <span class="nut-sku-header-right-extra">重量：0.1kg  编号：{{skuId}}  </span>
      </template> 
      <!-- sku 展示区上方与商品信息展示区下方区域，无默认展示内容 -->
      <template #sku-select-top>
          <div class="address">
              <nut-cell style="box-shadow:none;padding:13px 0" title="送至" :desc="addressDesc" @click="showAddressPopup=true"></nut-cell>
          </div>
      </template>
      <!-- 底部按钮操作区 -->
      <template #sku-operate>
          <div class="sku-operate-box">
          <nut-button class="sku-operate-item" shape="square" type="warning">加入购物车</nut-button>
          <nut-button class="sku-operate-item" shape="square" type="primary">立即购买</nut-button>
          </div>
      </template>
  </nut-sku>

  <nut-address
    v-model:visible="showAddressPopup"
    type="exist"
    :exist-address="existAddress"
    :is-show-custom-address="false"
    @selected="selectedAddress"
    exist-address-title="配送至"
  ></nut-address>

</template>
<script lang="ts">
import { ref,reactive,onMounted,toRefs} from 'vue';
export default {
setup() {
    const customBySlot = ref(false);
    const showAddressPopup = ref(false);
    const data = reactive({
      sku: [],
      goods: {}
    });

    const addressDesc = ref('(配送地会影响库存，请先确认)');
    const existAddress = ref([
      {
        id: 1,
        addressDetail: 'th ',
        cityName: '石景山区',
        countyName: '城区',
        provinceName: '北京',
        selectedAddress: true,
        townName: ''
      },
      {
        id: 2,
        addressDetail: '12 ',
        cityName: '电饭锅',
        countyName: '扶绥县',
        provinceName: '北京',
        selectedAddress: false,
        townName: ''
      },
      {
        id: 3,
        addressDetail: '发大水比 ',
        cityName: '放到',
        countyName: '广宁街道',
        provinceName: '钓鱼岛全区',
        selectedAddress: false,
        townName: ''
      },
      {
        id: 4,
        addressDetail: '还是想吧百度吧 ',
        cityName: '研发',
        countyName: '八里庄街道',
        provinceName: '北京',
        selectedAddress: false,
        townName: ''
      }
    ]);

    onMounted(() => {
        fetch('https://storage.360buyimg.com/nutui/3x/data.js')
          .then((response) => response.json())
          .then((res) => {
            const { Sku, Goods, imagePathMap } = res;
            data.sku = Sku;
            data.goods = Goods;
          }) //执行结果是 resolve就调用then方法
          .catch((err) => console.log('Oh, error', err)); //执行结果是 reject就调用catch方法
    });

    // 切换规格类目
    const selectSku = (ss: string) => {
      const { sku, skuIndex, parentSku, parentIndex } = ss;
      if (sku.disable) return false;
      data.sku[parentIndex].list.forEach((s) => {
        s.active = s.id == sku.id;
      });
      data.goods = {
        skuId: sku.id,
        price: '6002.10',
        imagePath:
          '//img14.360buyimg.com/n4/jfs/t1/215845/12/3788/221990/618a5c4dEc71cb4c7/7bd6eb8d17830991.jpg' 
      };
    };
    const selectedAddress = (prevExistAdd: any, nowExistAdd: any) => {
      const { provinceName, countyName, cityName } = nowExistAdd;
      addressDesc.value = `${provinceName}${countyName}${cityName}`;
    };
    // 底部操作按钮触发
    const clickBtnOperate = (op:string)=>{
      console.log('点击了操作按钮',op)
    } 
    return { customBySlot, selectSku, clickBtnOperate,existAddress,addressDesc,selectedAddress,...toRefs(data) };
}
}
</script>

<style>
.sku-operate-box {
  width: 100%;
  display: flex;
  padding: 8px 10px;
  box-sizing: border-box;
}
.sku-operate-item {
    flex:1
}
.sku-operate-item:first-child {
      border-top-left-radius: 20px;
      border-bottom-left-radius: 20px;
    }
.sku-operate-item:last-child {
      border-top-right-radius: 20px;
      border-bottom-right-radius: 20px;
    }
</style>
```
:::

## API

### Props

| Attribute            | Description               | Type   | Default  |
|--------------|----------------------------------|--------|------------------|
| v-model:visible         | Whether to open popup               | boolean |  `false`             |
| sku         | Sku data | Array | `[]`               |
| goods |  Product Info    | object | - |
| stepper-max         | Stepper max  | string \| number | `99999`               |
| stepper-min         | Stepper min  | string \| number | `1`               |
| btn-options        |   Bottom button      | Array | `[confirm]`           |
| btn-extra-text | Add text above button | string | -            |
| stepper-title         | Stepper left text | string | `Buy Num`                |
| stepper-extra-text        |   The text between the stepper and the headline       | Function \| boolean  | `false`              |
| buy-text |  Buy button text    | string | `Buy It Now` |
| add-cart-text          |        	Add cart button text                 | string | `Add  To cart`             |
| confirm-text          |           Confirm button text              | string | `Confirm`   |

### Events

| Attribute            | Description               | Arguments   |
|--------|----------------|--------------|
| select-sku  | Emitted when select sku | {sku,skuIndex,parentSku,parentIndex} |
| add  | Emitted when click stepper add button | value |
| reduce  | Emitted when click stepper reduce button | value |
| overLimit  | Emitted when click stepper disabled button| value |
| change-stepper  | Emitted when click stepper change | value |
| click-btn-operate  | Emitted when click bottom button | {type:'confirm',value:'inputNumber value'} |
| click-close-icon  | Emitted when click close button | - |
| click-overlay  | Emitted when click mask | - |
| close  | Emitted when popup close | - |


### Slots

The default partition is divided into several areas, which are defined as slots that can be replaced as required。

| Event | Description           | 
|--------|----------------|
| sku-header  | Custom header | 
| sku-header-price  | Custom header price area| 
| sku-header-extra  | Extra header area | 
| sku-select-top |  Custom select top | 
| sku-select | Custom sku | 
| sku-stepper  | Custom stepper | 
| sku-stepper-bottom  | Custom stepper bottom | 
| sku-operate | Custom stepper bottom operation |

### goods Data Structure

```javascript
goods:{
    skuId:'', // Product ID
    price: "0", // Product Price
    imagePath: "", // Product Image
}

```

### sku Data Structure

Each array index represents a specification category。Where,`list` represents the category value under the specification category。Each category value object includes：`name`、`id`、`active`、`disable`

```javascript
sku : [{
    id: 1,
    name: '颜色',
    list: [{
        name: '亮黑色',
        id: 100016015112,
        active: true,
        disable: false
      },
      {
        name: '釉白色',
        id: 100016015142,
        active: false,
        disable: false
      },
      {
        name: '秘银色',
        id: 100016015078,
        active: false,
        disable: false
      },
      {
        name: '夏日胡杨',
        id: 100009064831,
        active: false,
        disable: false
      },
      {
        name: '秋日胡杨',
        id: 100009064830,
        active: false,
        disable: false
      }
    ]
  },
  {
    id: 2,
    name: '版本',
    list: [{
        name: '8GB+128GB',
        id: 100016015102,
        active: true,
        disable: false
      },
      {
        name: '8GB+256GB',
        id: 100016015122,
        active: false,
        disable: false
      }
    ]
  },
  {
    id: 3,
    name: '版本',
    list: [{
        name: '4G（有充版）',
        id: 100016015103,
        active: true,
        disable: false
      },
      {
        name: '5G（有充版）',
        id: 100016015123,
        active: false,
        disable: false
      },
      {
        name: '5G（无充版）',
        id: 100016015104,
        active: true,
        disable: true
      },
      {
        name: '5G（无充）质保换新版',
        id: 100016015125,
        active: false,
        disable: false
      }
    ]
  }
];
```

## Theming

### CSS Variables

The component provides the following CSS variables, which can be used to customize styles. Please refer to [ConfigProvider component](#/en-US/component/configprovider).

| Name | Default Value | 
| --------------------------------------- | -------------------------- | 
| --nut-sku-item-border|  _1px solid var(--nut-primary-color)_  |
| --nut-sku-item-disable-line|  _line-through_  |
| --nut-sku-opetate-bg-default|  _linear-gradient(90deg, var(--nut-primary-color), var(--nut-primary-color-end) 100%)_  |
| --nut-sku-item-active-bg|  _var(--nut-primary-color)_  |
| --nut-sku-opetate-bg-buy|  _linear-gradient(135deg,rgba(255, 186, 13, 1) 0%,rgba(255, 195, 13, 1) 69%,rgba(255, 207, 13, 1) 100%)_  |
| --nut-sku-spec-height|  _30px_  |
| --nut-sku-spec-line-height|  _var(--nut-sku-spec-height)_  |
| --nut-sku-spec-font-size|  _11px_  |
| --nut-sku-spec-background|  _rgba(242, 242, 242, 1)_  |
| --nut-sku-spec-color|  _var(--nut-black)_  |
| --nut-sku-spec-margin-right|  _12px_  |
| --nut-sku-spec-padding|  _0 18px_  |
| --nut-sku-spec-title-font-weight|  _bold_  |
| --nut-sku-spec-title-font-size|  _13px_  |
| --nut-sku-spec-title-color|  _var(--nut-black)_  |
| --nut-sku-spec-title-margin-bottom|  _18px_  |
| --nut-sku-operate-btn-height|  _54px_  |
| --nut-sku-operate-btn-border-top|  _0_  |
| --nut-sku-operate-btn-item-height|  _40px_  |
| --nut-sku-operate-btn-item-line-height|  _var(--nut-sku-operate-btn-item-height)_  |
| --nut-sku-operate-btn-item-font-size|  _15px_  |
| --nut-sku-operate-btn-item-font-weight|  _normal_  |
| --nut-sku-product-img-width|  _100px_  |
| --nut-sku-product-img-height|  _var(--nut-sku-product-img-width)_  |
| --nut-sku-product-img-border-radius|  _0_  |

