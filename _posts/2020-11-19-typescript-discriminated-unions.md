---
layout: post
title:  "TypeScript 可辨识联合类型"
author: Kothing
categories: [ Javascript, TypeScript ]
image: assets/images/5.jpg
---

TypeScript 可辨识联合（Discriminated Unions）类型，也称为代数数据类型或标签联合类型。它包含 3 个要点：可辨识、联合类型和类型守卫。

这种类型的本质是结合联合类型和字面量类型的一种类型保护方法。如果一个类型是多个类型的联合类型，且多个类型含有一个公共属性，那么就可以利用这个公共属性，来创建不同的类型保护区块。

### 一、可辨识
可辨识要求联合类型中的每个元素都含有一个单例类型属性，比如：
```typescript
enum CarTransmission {
  Automatic = 200,
  Manual = 300
}

interface Motorcycle {
  vType: "motorcycle"; // discriminant
  make: number; // year
}

interface Car {
  vType: "car"; // discriminant
  transmission: CarTransmission
}

interface Truck {
  vType: "truck"; // discriminant
  capacity: number; // in tons
}
```
在上述代码中，我们分别定义了 `Motorcycle`、 `Car` 和 `Truck` 三个接口，在这些接口中都包含一个 `vType` 属性，该属性被称为可辨识的属性，而其它的属性只跟特性的接口相关。
<br/>

### 二、联合类型
基于前面定义了三个接口，我们可以创建一个 `Vehicle` 联合类型：
```typescript
type Vehicle = Motorcycle | Car | Truck;
```
现在我们就可以开始使用 `Vehicle` 联合类型，对于 `Vehicle` 类型的变量，它可以表示不同类型的车辆。
<br/>

### 三、类型守卫
下面我们来定义一个 `evaluatePrice` 方法，该方法用于根据车辆的类型、容量和评估因子来计算价格，具体实现如下：
```typescript
const EVALUATION_FACTOR = Math.PI; 
function evaluatePrice(vehicle: Vehicle) {
  return vehicle.capacity * EVALUATION_FACTOR;
}

const myTruck: Truck = { vType: "truck", capacity: 9.5 };
evaluatePrice(myTruck);
```

对于以上代码，TypeScript 编译器将会提示以下错误信息：
```typescript
Property 'capacity' does not exist on type 'Vehicle'.
Property 'capacity' does not exist on type 'Motorcycle'.
```

原因是在 `Motorcycle` 接口中，并不存在 `capacity` 属性，而对于 `Car` 接口来说，它也不存在 `capacity` 属性。那么，现在我们应该如何解决以上问题呢？这时，我们可以使用类型守卫。下面我们来重构一下前面定义的 `evaluatePrice` 方法，重构后的代码如下：
```typescript
function evaluatePrice(vehicle: Vehicle) {
  switch(vehicle.vType) {
    case "car":
      return vehicle.transmission * EVALUATION_FACTOR;
    case "truck":
      return vehicle.capacity * EVALUATION_FACTOR;
    case "motorcycle":
      return vehicle.make * EVALUATION_FACTOR;
  }
}
```
在以上代码中，我们使用 `switch` 和 `case` 运算符来实现类型守卫，从而确保在 `evaluatePrice` 方法中，我们可以安全地访问 `vehicle` 对象中的所包含的属性，来正确的计算该车辆类型所对应的价格。
<br/>

### 四、穷举检查
假设我们想要往前面已经定义的 `Vehicle` 联合类型，添加新的类型，那么会出现什么问题呢？下面我们来实际验证一下。首先我们先新增一个新的 `Bicycle` 接口，接着继续更新 `Vehicle` 联合类型的定义，具体代码如下：
```typescript
interface Bicycle {
  vType: "bicycle";
  make: number;
}

type Vehicle = Motorcycle | Car | Truck | Bicycle;
```

更新完以上代码之后，TypeScript 编译器会提示以下的错误信息：
```typescript
Not all code paths return a value.
```

为什么会提示这个错误信息呢？原因是因为我们之前创建的 `evaluatePrice` 方法还没处理 `Bicycle` 类型。那好，我们再来更新一下 `evaluatePrice` 方法，即增加对新增 `Bicycle` 类型的处理逻辑：
```typescript
function evaluatePrice(vehicle: Vehicle) {
  switch(vehicle.vType) {
    case "car":
      return vehicle.transmission * EVALUATION_FACTOR;
    case "truck":
      return vehicle.capacity * EVALUATION_FACTOR;
    case "motorcycle":
      return vehicle.make * EVALUATION_FACTOR;
    case "bicycle":
      return vehicle.make * EVALUATION_FACTOR;
  }
}
```

虽然现在问题已经解决了，但如果以后还要新增新的车辆类型呢？针对这个问题，有没有更好的解决方案呢？答案是有的，可以利用 TypeScript 中的 `never` 类型，具体代码如下：
```typescript
function evaluatePrice(vehicle: Vehicle) {
  switch(vehicle.vType) {
    case "car":
      return vehicle.transmission * EVALUATION_FACTOR;
    case "truck":
      return vehicle.capacity * EVALUATION_FACTOR;
    case "motorcycle":
      return vehicle.make * EVALUATION_FACTOR;
    case "bicycle":
      return vehicle.make * EVALUATION_FACTOR;
    default: 
      const invalidVehicle: never = vehicle;
      throw new Error(`Unknown vehicle: ${invalidVehicle}`);
  }
}
```

在上面代码中，我们新增了默认的处理分支，在该分支中，我们把收窄为 `never` 类型的 `vehicle` 变量赋值给同为 `never` 类型的 `invalidVehicle` 变量，这样有什么好处呢？现在我们来把前面新增的 `Bicycle` 类型的处理逻辑注释掉，这时 `TypeScript` 编译器也会提示错误信息，但此时的错误信息是这样的：
```typescript
Type 'Bicycle' is not assignable to type 'never'.
```

相比之前的错误信息，是不是更加直观了。在 `evaluatePrice` 方法中，我们新增了默认的处理分支，穷举了所有可能的车辆类型。此外我们还引入了 `never` 类型避免出现新增了联合类型没有对应的实现，目的就是写出类型绝对安全的代码。

