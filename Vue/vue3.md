# composition api

提高组件的复用性

![image-20230208222215831](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230208222215831.png)

## ref&reactive

响应式值,可被v-model绑定

### 普通值

```js
import { ref } from "vue";

setup() {
	const uName = ref("111");
    
    setTimeout(function() {
        uName.value = 'Max'; // 更改新值需要访问value
    }, 2000);
    
    return { userName: uName };
}
```

### 对象

```js
<h2>{{ user.userName }}</h2>

import { ref } from "vue";

setup() {
	const user = ref({
        userName: 'aaa'
    });
    
    setTimeout(function() {
        user.value.userName = 'Max'; // 通过proxy对象代理
    }, 2000);
    
    return { user }; // 暴露整个对象
}
```

推荐reactive

```js
import { reactive } from "vue";

export default {
    setup() {
        const user = reactive({
            name: "aaa"
        })
        
        setTimeout(function() {
        	user.name = 'Max'; // 通过proxy对象代理
    	}, 2000);
        
        return { user: user }
    }
}
```

## methods

替代method

```js
import { reactive } from "vue";

export default {
    setup() {
        const user = reactive({
            name: "aaa",
            age: 31
        })
        
        function setNewAge() {
            user.age ++;
        }
        
        return { user: user, setAge: setNewAge }; // 设置一个指针
    }
}
```

## computed

```js
import { ref,computed } from "vue";

setup() {
	const firstName = ref("");
    const lastName = ref("");
    
    function setFirstName(event) {
        firstName.value = event.target.value;
    }
    
    function setLastName(event) {
        lastName.value = event.target.value;
    }
    
    const uName = computed(function() {
        return firstName.value + " " + lastName.value;
    });
    
    return { userName: uName };
}
```

## watcher

```js
import { ref,computed,watch } from "vue";

setup() {
	const firstName = ref("");
    const lastName = ref("");
    const uAge = ref(31);
    
    function setFirstName(event) {
        firstName.value = event.target.value;
    }
    
    function setLastName(event) {
        lastName.value = event.target.value;
    }
    
    watch(uAge, function(newValue, oldValue) {
          
    });
    
    // 两个中任意一个变化触发
    watch([uAge, uName], function(newValues, oldValues) {
          console.log(newValues[0]);
    });
    
    const uName = computed(function() {
        return firstName.value + " " + lastName.value;
    });
    
    return { userName: uName };
}
```

