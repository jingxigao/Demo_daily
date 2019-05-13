html:
```html
<div class="formitem">
  <input type="password" autocomplete="new-password" placeholder="输入密码（包含字母和数字，8-16位）" class="pwd" v-model="pwd" v-show="ispwd" @blur="testpwd()">
  <input type="text" autocomplete="off" placeholder="输入密码（包含字母和数字，8-16位）" class="pwd" v-model="pwd" v-show="!ispwd" @blur="testpwd()">
  <div class="eye" @click="open()">
    <img src="../../assets/images/eye.png" alt="" v-if="ispwd">
    <img src="../../assets/images/close.png" alt="" v-if="!ispwd">
  </div>
</div>
<div class="formitem">
  <input type="password" placeholder="再次输入密码（请注意大小写）" v-model="apwd" @blur="consistent()">
</div>
```
script:
```js
methods:{
    testpwd() {
            console.log("onblur")
            if(this.pwd==''){
             
            }else if(!(/^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{8,16}$/.test(this.pwd))){
              this.$vux.toast.show({
                text: '请输入正确的密码',
                type: 'text'
              })
            }else{
              if(this.apwd!=''){
                if(this.pwd != this.apwd){
                  this.$vux.toast.show({
                    text: '两次输入密码不一致',
                    type: 'text'
                  })
                  this.same = false;
                }else{
                  this.same = true;
                }
              }
            }
          }
},
      consistent() {
        if(this.apwd == ''){
          this.same = false;
        }else if(this.pwd != this.apwd){
          this.$vux.toast.show({
            text: '两次输入密码不一致',
            type: 'text'
          })
          this.same = false;
        }else{
          this.same = true;
        }
      }
```
