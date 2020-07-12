# AssemblyScript Loader Advanced Examples

Here are some advanced examples covering array of numbers and array of strings which are not available in the [official documentation](https://www.assemblyscript.org/loader.html#installation).

#### 1.) Array of numbers as the argument, array of numbers as the return value

```ts
// AssemblyScript
export function sqrt(arr: Int32Array): Int32Array {
  return arr.map((n) => i32(Math.sqrt(n)))
}

export const Int32Array_ID = idof<Int32Array>()
```

```js
// JavaScript
function doSqrt(values) {
  const inputArrPtr = __retain(__allocArray(Int32Array_ID, values))
  const outputArrPtr = sqrt(inputArrPtr)
__release(inputArrPtr)
  const output = __getArray(outputArrPtr)
__release(outputArrPtr)
  return output
}

console.log(doSqrt([1, 4, 9, 16, 25, 36]))
// [1, 2, 3, 4, 5, 6]
```

#### 2.) Array of strings as the argument, array of strings as the return value

```ts
// AssemblyScript
export function reverseArray(arr: string[]): string[] {
  return arr.reverse()
}

export const StringArray_ID = idof<string[]>()
```

```js
// JavaScript
function doReverseArray(values) {
  const strPtr = values.map((string) => __retain(__allocString(string)))
  const arrPtr = __retain(__allocArray(StringArray_ID, strPtr))

  const outArrPtr = reverseArray(arrPtr)

  __release(arrPtr)
  // No need to do strPtr.forEach(__release) here.

  const outStrPtr = __getArray(outArrPtr)
  const output = outStrPtr.map(__getString)

  __release(outArrPtr)

  return output
}

console.log(doReverseArray(['April', 'March', 'February', 'January']))
// [ 'January', 'February', 'March', 'April' ]
```