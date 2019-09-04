```javascript
 let sleep = s => {
  let time1 = Date.now();
  while(Date.now() - time1 < s){}
  console.log('A', `end sleep ${s}ms`)
}
let async1 = async() => {
  await async2();
  console.log('B', 'async1 end')
}
let async2 = async() => {
  console.log('C', 'async2 start')
  await Promise.resolve();
  console.log('D', 'async2 end')
}
setTimeout(() => {
  console.log('1', 'settimeou0')
}, 0)
new Promise((res, rej) => {
  rej();
  console.log('2')
}).catch(() => { console.log('3') })

async1()
sleep(2000)

// 2 C A 3 D B 1
```
