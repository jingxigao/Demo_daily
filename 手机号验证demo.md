html:
```html
    <div class="formitem">
      <input type="text" placeholder="输入手机号" class="phone" v-model="phone" oninput="value=value.replace(/[^\d]/g,'')">
      <span class="code" @click="getCode()" v-if="iscode">{{second}}</span>
      <span class="code nocode" v-if="!iscode">{{second}}</span>
    </div>
```
script:
```js
getCode(){
        if(!(/^1[34578]\d{9}$/.test(this.phone))){
          this.$vux.toast.show({
            text: '请输入正确的手机号',
            type: 'text'
          })
        }else{
          // this.isphone = true;
          this.iscode = false;
          this.secondNumber = 60;
          this.second = '('+this.secondNumber+')'+'重新获取';
          this.interval = setInterval(()=>{
              this.secondNumber--;
              this.second = '('+this.secondNumber+')'+'重新获取';
              if(this.secondNumber == 0){
                  clearInterval(this.interval);
                  this.interval = null;
                  this.iscode = true;
                  this.second = "获取验证码";
              }
          },1000)
        }
      }
```
