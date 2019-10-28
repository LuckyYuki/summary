# hasOwnProperty方法 数组去重

```js
function uniqueArray(array) {
			var hashmap = {};
			var unique = [];
			for(var i = 0; i < array.length; i++) {
				
				if(!hashmap.hasOwnProperty([array[i]])) {
					hashmap[array[i]] = 1;
					unique.push(array[i]);
				}
			}
			return unique;
		}
 
		var array = [1, 2, 3, 5, 1, 5, 9, 1, 2, 8];
		
		uniqueArray(array); // [1, 2, 3, 5, 9, 8]
```
