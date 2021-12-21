> [TypeScript入门教程](https://ts.xcatliu.com/)

## 基础类型

- :number
  ```typescript
  let sum: number = 0;
  function getSum(): number {};
  ```

- :string

- :void 只允许赋予undefined/null
  ```typescript
  let u: void = undefined;
  let n: void = null;
  ```

- :any 如果定义的时候没有赋值，不管之后有没有赋值，都会被推断成 any 类型而完全不被类型检查
  ```typescript
  let something: any;
  ```

- 联合类型
  ```typescript
  let myFavoriteNumber: string | number;
  function getString(something: string | number): string {}
  ```

- 接口
  ```typescript
  interface Person {
      name: string;
      age: number;
  }

  let tom: Person = {
      name: 'Tom',
      age: 25
  };
  ```

- 数组
  ```typescript
  let arr1: number[] = [1, 2]
  let arr2: Array<number> = [1, 2]
  ```